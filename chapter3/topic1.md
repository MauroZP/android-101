### Mi primer layout

1. In Android Studio, from the res/layout directory, open the content_my.xml file.

2. The BlankActivity template you chose when you created this project includes the content_my.xml file with a RelativeLayout root view and a TextView child view.

3. In the Preview pane, click the Hide icon  to close the Preview pane.

4. In Android Studio, when you open a layout file, you’re first shown the Preview pane. Clicking elements in this pane opens the WYSIWYG tools in the Design pane. For this lesson, you’re going to work directly with the XML.

5. Delete the <TextView> element.

6. Change the <RelativeLayout> element to <LinearLayout>.

7. Add the android:orientation attribute and set it to "horizontal".

8. Remove the android:padding attributes and the tools:context attribute.

LinearLayout is a view group (a subclass of ViewGroup) that lays out child views in either a vertical or horizontal orientation, as specified by the android:orientation attribute. Each child of a LinearLayout appears on the screen in the order in which it appears in the XML.

Two other attributes, android:layout_width and android:layout_height, are required for all views in order to specify their size.

Because the LinearLayout is the root view in the layout, it should fill the entire screen area that's available to the app by setting the width and height to "match_parent". This value declares that the view should expand its width or height to match the width or height of the parent view.


#### Add a Text Field

As with every View object, you must define certain XML attributes to specify the EditText object's properties.

1. In the content_my.xml file, within the <LinearLayout> element, define an <EditText> element with the id attribute set to @+id/edit_message.

2. Define the layout_width and layout_height attributes as wrap_content.

3. Define a hint attribute as a string object named edit_message.

The <EditText> element should read as follows:

```xml
<EditText android:id="@+id/edit_message"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:hint="@string/edit_message" />
```

**android:id**

	This provides a unique identifier for the view, which you can use to reference the object from your app code, such as to read and manipulate the object (you'll see this in the next lesson).
	The at sign (@) is required when you're referring to any resource object from XML. It is followed by the resource type (id in this case), a slash, then the resource name (edit_message).

	The plus sign (+) before the resource type is needed only when you're defining a resource ID for the first time. When you compile the app, the SDK tools use the ID name to create a new resource ID in your project's gen/R.java file that refers to the EditText element. With the resource ID declared once this way, other references to the ID do not need the plus sign. Using the plus sign is necessary only when specifying a new resource ID and not needed for concrete resources such as strings or layouts. See the sidebox for more information about resource objects.

**android:layout_width and android:layout_height**

	Instead of using specific sizes for the width and height, the "wrap_content" value specifies that the view should be only as big as needed to fit the contents of the view. If you were to instead use "match_parent", then the EditText element would fill the screen, because it would match the size of the parent LinearLayout. For more information, see the Layouts guide.

**android:hint**

	This is a default string to display when the text field is empty. Instead of using a hard-coded string as the value, the "@string/edit_message" value refers to a string resource defined in a separate file. Because this refers to a concrete resource (not just an identifier), it does not need the plus sign. However, because you haven't defined the string resource yet, you’ll see a compiler error at first. You'll fix this in the next section by defining the string.

```
Note: This string resource has the same name as the element ID: edit_message. However, references to resources are always scoped by the resource type (such as id or string), so using the same name does not cause collisions.
```

#### Add String Resources

By default, your Android project includes a string resource file at res/values/strings.xml. Here, you'll add a new string named "edit_message" and set the value to "Enter a message."

1. In Android Studio, from the res/values directory, open strings.xml.

2. Add a line for a string named "edit_message" with the value, "Enter a message".

3. Add a line for a string named "button_send" with the value, "Send".

4. You'll create the button that uses this string in the next section.

The result for strings.xml looks like this:


```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">My First App</string>
    <string name="edit_message">Enter a message</string>
    <string name="button_send">Send</string>
    <string name="action_settings">Settings</string>
</resources>
```

For text in the user interface, always specify each string as a resource. String resources allow you to manage all UI text in a single location, which makes the text easier to find and update. Externalizing the strings also allows you to localize your app to different languages by providing alternative definitions for each string resource.

#### Add a Button

1. In Android Studio, from the res/layout directory, edit the content_my.xml file.

2. Within the <LinearLayout> element, define a <Button> element immediately following the <EditText> element.

3. Set the button's width and height attributes to "wrap_content" so the button is only as big as necessary to fit the button's text label.

4. Define the button's text label with the android:text attribute; set its value to the button_send string resource you defined in the previous section.

Your <LinearLayout> should look like this:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:showIn="@layout/activity_my">
        <EditText android:id="@+id/edit_message"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:hint="@string/edit_message" />
        <Button
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="@string/button_send" />
</LinearLayout>
```

The layout is currently designed so that both the EditText and Button widgets are only as big as necessary to fit their content, as Figure 2 shows.

https://developer.android.com/images/training/firstapp/edittext_wrap.png

This works fine for the button, but not as well for the text field, because the user might type something longer. It would be nice to fill the unused screen width with the text field. You can do this inside a LinearLayout with the weight property, which you can specify using the android:layout_weight attribute.

The weight value is a number that specifies the amount of remaining space each view should consume, relative to the amount consumed by sibling views. This works kind of like the amount of ingredients in a drink recipe: "2 parts soda, 1 part syrup" means two-thirds of the drink is soda. For example, if you give one view a weight of 2 and another one a weight of 1, the sum is 3, so the first view fills 2/3 of the remaining space and the second view fills the rest. If you add a third view and give it a weight of 1, then the first view (with weight of 2) now gets 1/2 the remaining space, while the remaining two each get 1/4.

The default weight for all views is 0, so if you specify any weight value greater than 0 to only one view, then that view fills whatever space remains after all views are given the space they require.

#### Make the Input Box Fill in the Screen Width

To fill the remaining space in your layout with the EditText element, do the following:

1. In the content_my.xml file, assign the <EditText> element's layout_weight attribute a value of 1.

2. Also, assign <EditText> element's layout_width attribute a value of 0dp.

```xml
<EditText
    android:layout_weight="1"
    android:layout_width="0dp"
    ... />
```

To improve the layout efficiency when you specify the weight, you should change the width of the EditText to be zero (0dp). Setting the width to zero improves layout performance because using "wrap_content" as the width requires the system to calculate a width that is ultimately irrelevant because the weight value requires another width calculation to fill the remaining space.

Como debe quedar el layout: [Codigo](/chapter3/main.xml)

#### Load the XML Resource

When you compile your application, each XML layout file is compiled into a View resource. You should load the layout resource from your application code, in your Activity.onCreate() callback implementation. Do so by calling setContentView(), passing it the reference to your layout resource in the form of: R.layout.layout_file_name. For example, if your XML layout is saved as main_layout.xml, you would load it for your Activity like so:

```java
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.main_layout);
}
```

The onCreate() callback method in your Activity is called by the Android framework when your Activity is launched (see the discussion about lifecycles, in the Activities document).

#### Layout Parameters

XML layout attributes named layout_something define layout parameters for the View that are appropriate for the ViewGroup in which it resides.

Every ViewGroup class implements a nested class that extends ViewGroup.LayoutParams. This subclass contains property types that define the size and position for each child view, as appropriate for the view group. As you can see in figure 1, the parent view group defines layout parameters for each child view (including the child view group).

https://developer.android.com/images/layoutparams.png

	Note that every LayoutParams subclass has its own syntax for setting values. Each child element must define LayoutParams that are appropriate for its parent, though it may also define different LayoutParams for its own children.

All view groups include a width and height (layout_width and layout_height), and each view is required to define them. Many LayoutParams also include optional margins and borders.

You can specify width and height with exact measurements, though you probably won't want to do this often. More often, you will use one of these constants to set the width or height:

+ wrap_content tells your view to size itself to the dimensions required by its content.

+ match_parent tells your view to become as big as its parent view group will allow.

In general, specifying a layout width and height using absolute units such as pixels is not recommended. Instead, using relative measurements such as density-independent pixel units (dp), wrap_content, or match_parent, is a better approach, because it helps ensure that your application will display properly across a variety of device screen sizes. The accepted measurement types are defined in the Available Resources document.

#### Layout Position

The geometry of a view is that of a rectangle. A view has a location, expressed as a pair of left and top coordinates, and two dimensions, expressed as a width and a height. The unit for location and dimensions is the pixel.

It is possible to retrieve the location of a view by invoking the methods getLeft() and getTop(). The former returns the left, or X, coordinate of the rectangle representing the view. The latter returns the top, or Y, coordinate of the rectangle representing the view. These methods both return the location of the view relative to its parent. For instance, when getLeft() returns 20, that means the view is located 20 pixels to the right of the left edge of its direct parent.

In addition, several convenience methods are offered to avoid unnecessary computations, namely getRight() and getBottom(). These methods return the coordinates of the right and bottom edges of the rectangle representing the view. For instance, calling getRight() is similar to the following computation: getLeft() + getWidth().

#### Size, Padding and Margins


The size of a view is expressed with a width and a height. A view actually possess two pairs of width and height values.

The first pair is known as measured width and measured height. These dimensions define how big a view wants to be within its parent. The measured dimensions can be obtained by calling getMeasuredWidth() and getMeasuredHeight().

The second pair is simply known as width and height, or sometimes drawing width and drawing height. These dimensions define the actual size of the view on screen, at drawing time and after layout. These values may, but do not have to, be different from the measured width and height. The width and height can be obtained by calling getWidth() and getHeight().

To measure its dimensions, a view takes into account its padding. The padding is expressed in pixels for the left, top, right and bottom parts of the view. Padding can be used to offset the content of the view by a specific number of pixels. For instance, a left padding of 2 will push the view's content by 2 pixels to the right of the left edge. Padding can be set using the setPadding(int, int, int, int) method and queried by calling getPaddingLeft(), getPaddingTop(), getPaddingRight() and getPaddingBottom().

Even though a view can define a padding, it does not provide any support for margins. However, view groups provide such a support. Refer to ViewGroup and ViewGroup.MarginLayoutParams for further information.

For more information about dimensions, see Dimension Values.

