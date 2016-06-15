### Elementos de un layout

Cada subclase de la clase ViewGroup ofrece una manera única de mostrar los puntos de vista que anidan en su interior. A continuación se presentan algunos de los tipos de diseño más comunes que están integradas en la plataforma Android.

```
Nota: Aunque se pueden anidar uno o más diseños dentro de una disposición diferente a su diseño
de interfaz de usuario, usted debe esforzarse para mantener su jerarquía de disposición lo más
planas posible. Su diseño se basa más rápidamente si tiene un menor número de diseños anidados
(una amplia jerarquía de la vista es mejor que una jerarquía de vistas de profundidad).
```

#### Linear Layout

LinearLayout es un grupo de la vista que se alinea a todos los hijos en una sola dirección, vertical u horizontalmente. Puede especificar la dirección de diseño con el atributo android: orientación.

<img src="https://developer.android.com/images/ui/linearlayout.png" width="400">

Todos los hijos de un LinearLayout están apilados uno tras otro, por lo que una lista vertical sólo tendrán un hijo por cada fila, independientemente del tamaño de que son, y una lista horizontal solamente serán alto de una fila (la altura del hijo más alto, más relleno). Un LinearLayout respeta los márgenes entre los hijos y la gravedad (derecha, centro o alineación a la izquierda) de cada hijo.

##### Layout Weight

LinearLayout también es compatible con la asignación de un peso a cada hijo con el atributo android:layout_weight. Este atributo asigna un valor de "importancia" a una vista en términos de la cantidad de espacio que debe ocupar en la pantalla. Un valor de peso más grande permite que se expanda para llenar cualquier espacio que queda en la vista padre. vistas secundarias pueden especificar un valor de peso, y entonces cualquier espacio restante en el GroupView no se ha asignado a los hijos en la proporción de su peso declarado. peso por defecto es cero.

[Ejercicio 1](/chapter3/exercise1.xml)

#### Relative Layout

RelativeLayout es un GroupView que muestra puntos de vista del hijo en posiciones relativas. La posición de cada punto de vista se puede especificar como en relación con elementos hermanos  (tales como a la izquierda de o por debajo de otra vista) o en posiciones relativas a los padres RelativeLayout (como alineada con la parte inferior, izquierda o central).

<img src="https://developer.android.com/images/ui/relativelayout.png" width="400">

Un RelativeLayout es una muy potente utilidad para el diseño de una interfaz de usuario, ya que puede eliminar los grupos de vistas anidadas y mantener su jerarquía de disposición plana, lo que mejora el rendimiento. Si usted se encuentra el uso de varios grupos LinearLayout anidados, es posible que pueda sustituirlas por un solo RelativeLayout.

##### Positioning Views

RelativeLayout permite especificar su posición con respecto a la vista padre o entre sí (especificado por id). Para que pueda alinear dos elementos de borde derecho, o hacer uno debajo de otro, centrado en la pantalla, centrada izquierda, y así sucesivamente. Por defecto, todos los puntos de vista hijos se dibujan en la parte superior izquierda de la vista, por lo que debe definir la posición de cada punto de vista utilizando las diversas propiedades de diseño disponibles de RelativeLayout.LayoutParams.

Algunas de las muchas propiedades de diseño disponibles para los RelativeLayout incluyen:

**android:layout_alignParentTop**

	Si es "true", hace que el borde superior de este punto de vista coincide con el borde superior de la matriz.

**android:layout_centerVertical**

	Si es "true", centra el hijo verticalmente dentro de su padre.

**android:layout_below**

	Posiciona el borde superior de esta vista por debajo de la vista especificada con un identificador de recurso.

**android:layout_toRightOf**

	Posiciona el borde izquierdo de esta vista a la derecha de la vista especificada con un identificador de recurso.

[Ejercicio 2](/chapter3/exercise2.xml)

#### Webview

Si desea entregar una aplicación web (o simplemente una página web) como parte de una aplicación de cliente, puede hacerlo utilizando WebView. La clase WebView es una extensión de vista de clase de Android que permite visualizar páginas web como parte de su diseño de la actividad. No incluye ninguna característica de un navegador web completamente desarrollado, como los controles de navegación o una barra de direcciones. Todo lo que no WebView, por defecto, es mostrar una página web.

https://developer.android.com/guide/webapps/webview.html