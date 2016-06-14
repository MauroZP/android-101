### Elementos de un layout

Each subclass of the ViewGroup class provides a unique way to display the views you nest within it. Below are some of the more common layout types that are built into the Android platform.

	Note: Although you can nest one or more layouts within another layout to acheive your UI design, you should strive to keep your layout hierarchy as shallow as possible. Your layout draws faster if it has fewer nested layouts (a wide view hierarchy is better than a deep view hierarchy).

#### Linear Layout

LinearLayout is a view group that aligns all children in a single direction, vertically or horizontally. You can specify the layout direction with the android:orientation attribute.

https://developer.android.com/images/ui/linearlayout.png

All children of a LinearLayout are stacked one after the other, so a vertical list will only have one child per row, no matter how wide they are, and a horizontal list will only be one row high (the height of the tallest child, plus padding). A LinearLayout respects margins between children and the gravity (right, center, or left alignment) of each child.

#### Layout Weight

LinearLayout also supports assigning a weight to individual children with the android:layout_weight attribute. This attribute assigns an "importance" value to a view in terms of how much space it should occupy on the screen. A larger weight value allows it to expand to fill any remaining space in the parent view. Child views can specify a weight value, and then any remaining space in the view group is assigned to children in the proportion of their declared weight. Default weight is zero.

For example, if there are three text fields and two of them declare a weight of 1, while the other is given no weight, the third text field without weight will not grow and will only occupy the area required by its content. The other two will expand equally to fill the space remaining after all three fields are measured. If the third field is then given a weight of 2 (instead of 0), then it is now declared more important than both the others, so it gets half the total remaining space, while the first two share the rest equally.

	To create a linear layout in which each child uses the same amount of space on the screen, set the android:layout_height of each view to "0dp" (for a vertical layout) or the android:layout_width of each view to "0dp" (for a horizontal layout). Then set the android:layout_weight of each view to "1".

[Ejercicio 1](/chapter3/exercise1.xml)

#### Relative Layout

RelativeLayout is a view group that displays child views in relative positions. The position of each view can be specified as relative to sibling elements (such as to the left-of or below another view) or in positions relative to the parent RelativeLayout area (such as aligned to the bottom, left or center).

https://developer.android.com/images/ui/relativelayout.png

A RelativeLayout is a very powerful utility for designing a user interface because it can eliminate nested view groups and keep your layout hierarchy flat, which improves performance. If you find yourself using several nested LinearLayout groups, you may be able to replace them with a single RelativeLayout.

##### Positioning Views

RelativeLayout lets child views specify their position relative to the parent view or to each other (specified by ID). So you can align two elements by right border, or make one below another, centered in the screen, centered left, and so on. By default, all child views are drawn at the top-left of the layout, so you must define the position of each view using the various layout properties available from RelativeLayout.LayoutParams.

Some of the many layout properties available to views in a RelativeLayout include:

**android:layout_alignParentTop**

	If "true", makes the top edge of this view match the top edge of the parent.

**android:layout_centerVertical**

	If "true", centers this child vertically within its parent.

**android:layout_below**

	Positions the top edge of this view below the view specified with a resource ID.

**android:layout_toRightOf**

	Positions the left edge of this view to the right of the view specified with a resource ID.

[Ejercicio 2](/chapter3/exercise2.xml)

#### Relative Layout

Building Web Apps in WebView

If you want to deliver a web application (or just a web page) as a part of a client application, you can do it using WebView. The WebView class is an extension of Android's View class that allows you to display web pages as a part of your activity layout. It does not include any features of a fully developed web browser, such as navigation controls or an address bar. All that WebView does, by default, is show a web page.

https://developer.android.com/guide/webapps/webview.html