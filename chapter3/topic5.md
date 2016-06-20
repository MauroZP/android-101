### Creating a Navigation Drawer

The navigation drawer is a panel that displays the app’s main navigation options on the left edge of the screen. It is hidden most of the time, but is revealed when the user swipes a finger from the left edge of the screen or, while at the top level of the app, the user touches the app icon in the action bar.

	Navigation Drawer Design
	Before you decide to use a navigation drawer in your app, you should understand the use
	cases and design principles defined in the Navigation Drawer design guide.
	https://material.google.com/patterns/navigation-drawer.html


#### Create a Drawer Layout

To add a navigation drawer, declare your user interface with a DrawerLayout object as the root view of your layout. Inside the DrawerLayout, add one view that contains the main content for the screen (your primary layout when the drawer is hidden) and another view that contains the contents of the navigation drawer.

For example, the following layout uses a DrawerLayout with two child views: a FrameLayout to contain the main content (populated by a Fragment at runtime), and a ListView for the navigation drawer.

```xml
<android.support.v4.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <!-- The main content view -->
    <FrameLayout
        android:id="@+id/content_frame"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
    <!-- The navigation drawer -->
    <ListView android:id="@+id/left_drawer"
        android:layout_width="240dp"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:choiceMode="singleChoice"
        android:divider="@android:color/transparent"
        android:dividerHeight="0dp"
        android:background="#111"/>
</android.support.v4.widget.DrawerLayout>
```

This layout demonstrates some important layout characteristics:

+ The main content view (the FrameLayout above) must be the first child in the DrawerLayout because the XML order implies z-ordering and the drawer must be on top of the content.

+ The main content view is set to match the parent view's width and height, because it represents the entire UI when the navigation drawer is hidden.

+ The drawer view (the ListView) must specify its horizontal gravity with the android:layout_gravity attribute. To support right-to-left (RTL) languages, specify the value with "start" instead of "left" (so the drawer appears on the right when the layout is RTL).

+ The drawer view specifies its width in dp units and the height matches the parent view. The drawer width should be no more than 320dp so the user can always see a portion of the main content.


#### Initialize the Drawer List

In your activity, one of the first things to do is initialize the navigation drawer's list of items. How you do so depends on the content of your app, but a navigation drawer often consists of a ListView, so the list should be populated by an Adapter (such as ArrayAdapter or SimpleCursorAdapter).

For example, here's how you can initialize the navigation list with a string array:

```java
public class MainActivity extends Activity {
    private String[] mPlanetTitles;
    private DrawerLayout mDrawerLayout;
    private ListView mDrawerList;
    ...

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mPlanetTitles = getResources().getStringArray(R.array.planets_array);
        mDrawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);
        mDrawerList = (ListView) findViewById(R.id.left_drawer);

        // Set the adapter for the list view
        mDrawerList.setAdapter(new ArrayAdapter<String>(this,
                R.layout.drawer_list_item, mPlanetTitles));
        // Set the list's click listener
        mDrawerList.setOnItemClickListener(new DrawerItemClickListener());

        ...
    }
}
```

This code also calls setOnItemClickListener() to receive click events in the navigation drawer's list. The next section shows how to implement this interface and change the content view when the user selects an item.

#### Handle Navigation Click Events

When the user selects an item in the drawer's list, the system calls onItemClick() on the OnItemClickListener given to setOnItemClickListener().

What you do in the onItemClick() method depends on how you've implemented your app structure. In the following example, selecting each item in the list inserts a different Fragment into the main content view (the FrameLayout element identified by the R.id.content_frame ID):

```java
private class DrawerItemClickListener implements ListView.OnItemClickListener {
    @Override
    public void onItemClick(AdapterView parent, View view, int position, long id) {
        selectItem(position);
    }
}

/** Swaps fragments in the main content view */
private void selectItem(int position) {
    // Create a new fragment and specify the planet to show based on position
    Fragment fragment = new PlanetFragment();
    Bundle args = new Bundle();
    args.putInt(PlanetFragment.ARG_PLANET_NUMBER, position);
    fragment.setArguments(args);

    // Insert the fragment by replacing any existing fragment
    FragmentManager fragmentManager = getFragmentManager();
    fragmentManager.beginTransaction()
                   .replace(R.id.content_frame, fragment)
                   .commit();

    // Highlight the selected item, update the title, and close the drawer
    mDrawerList.setItemChecked(position, true);
    setTitle(mPlanetTitles[position]);
    mDrawerLayout.closeDrawer(mDrawerList);
}

@Override
public void setTitle(CharSequence title) {
    mTitle = title;
    getActionBar().setTitle(mTitle);
}
```

#### Listen for Open and Close Events

To listen for drawer open and close events, call setDrawerListener() on your DrawerLayout and pass it an implementation of DrawerLayout.DrawerListener. This interface provides callbacks for drawer events such as onDrawerOpened() and onDrawerClosed().

However, rather than implementing the DrawerLayout.DrawerListener, if your activity includes the action bar, you can instead extend the ActionBarDrawerToggle class. The ActionBarDrawerToggle implements DrawerLayout.DrawerListener so you can still override those callbacks, but it also facilitates the proper interaction behavior between the action bar icon and the navigation drawer (discussed further in the next section).

As discussed in the Navigation Drawer design guide, you should modify the contents of the action bar when the drawer is visible, such as to change the title and remove action items that are contextual to the main content. The following code shows how you can do so by overriding DrawerLayout.DrawerListener callback methods with an instance of the ActionBarDrawerToggle class:

```java
public class MainActivity extends Activity {
    private DrawerLayout mDrawerLayout;
    private ActionBarDrawerToggle mDrawerToggle;
    private CharSequence mDrawerTitle;
    private CharSequence mTitle;
    ...

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ...

        mTitle = mDrawerTitle = getTitle();
        mDrawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);
        mDrawerToggle = new ActionBarDrawerToggle(this, mDrawerLayout,
                R.drawable.ic_drawer, R.string.drawer_open, R.string.drawer_close) {

            /** Called when a drawer has settled in a completely closed state. */
            public void onDrawerClosed(View view) {
                super.onDrawerClosed(view);
                getActionBar().setTitle(mTitle);
                invalidateOptionsMenu(); // creates call to onPrepareOptionsMenu()
            }

            /** Called when a drawer has settled in a completely open state. */
            public void onDrawerOpened(View drawerView) {
                super.onDrawerOpened(drawerView);
                getActionBar().setTitle(mDrawerTitle);
                invalidateOptionsMenu(); // creates call to onPrepareOptionsMenu()
            }
        };

        // Set the drawer toggle as the DrawerListener
        mDrawerLayout.setDrawerListener(mDrawerToggle);
    }

    /* Called whenever we call invalidateOptionsMenu() */
    @Override
    public boolean onPrepareOptionsMenu(Menu menu) {
        // If the nav drawer is open, hide action items related to the content view
        boolean drawerOpen = mDrawerLayout.isDrawerOpen(mDrawerList);
        menu.findItem(R.id.action_websearch).setVisible(!drawerOpen);
        return super.onPrepareOptionsMenu(menu);
    }
}
```

#### Open and Close with the App Icon

Users can open and close the navigation drawer with a swipe gesture from or towards the left edge of the screen, but if you're using the action bar, you should also allow users to open and close it by touching the app icon. And the app icon should also indicate the presence of the navigation drawer with a special icon. You can implement all this behavior by using the ActionBarDrawerToggle shown in the previous section.

To make ActionBarDrawerToggle work, create an instance of it with its constructor, which requires the following arguments:

+ The Activity hosting the drawer.

+ The DrawerLayout.

+ A drawable resource to use as the drawer indicator.

+ The standard navigation drawer icon is available in the Download the Action Bar Icon Pack.

+ A String resource to describe the "open drawer" action (for accessibility).

+ A String resource to describe the "close drawer" action (for accessibility).

Then, whether or not you've created a subclass of ActionBarDrawerToggle as your drawer listener, you need to call upon your ActionBarDrawerToggle in a few places throughout your activity lifecycle:

```java
public class MainActivity extends Activity {
    private DrawerLayout mDrawerLayout;
    private ActionBarDrawerToggle mDrawerToggle;
    ...

    public void onCreate(Bundle savedInstanceState) {
        ...

        mDrawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);
        mDrawerToggle = new ActionBarDrawerToggle(
                this,                  /* host Activity */
                mDrawerLayout,         /* DrawerLayout object */
                R.drawable.ic_drawer,  /* nav drawer icon to replace 'Up' caret */
                R.string.drawer_open,  /* "open drawer" description */
                R.string.drawer_close  /* "close drawer" description */
                ) {

            /** Called when a drawer has settled in a completely closed state. */
            public void onDrawerClosed(View view) {
                super.onDrawerClosed(view);
                getActionBar().setTitle(mTitle);
            }

            /** Called when a drawer has settled in a completely open state. */
            public void onDrawerOpened(View drawerView) {
                super.onDrawerOpened(drawerView);
                getActionBar().setTitle(mDrawerTitle);
            }
        };

        // Set the drawer toggle as the DrawerListener
        mDrawerLayout.setDrawerListener(mDrawerToggle);

        getActionBar().setDisplayHomeAsUpEnabled(true);
        getActionBar().setHomeButtonEnabled(true);
    }

    @Override
    protected void onPostCreate(Bundle savedInstanceState) {
        super.onPostCreate(savedInstanceState);
        // Sync the toggle state after onRestoreInstanceState has occurred.
        mDrawerToggle.syncState();
    }

    @Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
        mDrawerToggle.onConfigurationChanged(newConfig);
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Pass the event to ActionBarDrawerToggle, if it returns
        // true, then it has handled the app icon touch event
        if (mDrawerToggle.onOptionsItemSelected(item)) {
          return true;
        }
        // Handle your other action bar items...

        return super.onOptionsItemSelected(item);
    }

    ...
}
```

