### Mi primer layout

1. En Android de estudio, desde el directorio res/layout, abra el archivo activity_main.xml.

2. La plantilla EmptyActivity que escogió cuando se creó este proyecto incluye el archivo activity_main.xml con un View RelativeLayout y un View Text.

3. En el panel de vista previa, haga clic en el icono Ocultar para cerrar el panel de vista previa.

4. En Android de estudio, cuando se abre un archivo de diseño, se le muestra por primera vez el panel de vista previa. Al hacer clic en los elementos en este panel se abre las herramientas WYSIWYG en el panel de diseño. Para esta lección, usted va a trabajar directamente con el XML.

5. Eliminar el elemento < TextView >.

6. Cambie el elemento < RelativeLayout > a < LinearLayout >.

7. Agregue el atributo android: orientación y ponerlo en "horizontal".

8. Retire el atributo android: con padding y la  atributo tools:context.

LinearLayout es un View Group (una subclase de ViewGroup) que establece puntos de vista hijos, ya sea en una orientación vertical u horizontal, según lo especificado por el atributo android:orientation. Todos los hijos de LinearLayout aparecen en la pantalla en el orden en que aparece en el XML.

Otros dos atributos, android:layout_width y android:layout_height, son necesarios para todos los views con el fin de especificar su tamaño.

Debido a que el LinearLayout es la vista raíz en el diseño, se debe llenar toda la pantalla que está disponible para la aplicación mediante el establecimiento de la anchura y la altura de "match_parent". Este valor se declara que la vista debería ampliar su anchura o la altura para que coincida con la anchura o la altura de la vista padre.


#### Agregar un Text Field

Como con cada objeto View, debe definir determinados atributos de XML para especificar las propiedades del objeto EditText.

1. En el archivo activity_main.xml, dentro del elemento < LinearLayout >, definir un elemento < EditText > con el atributo id @+id/edit_message.

2. Definir el layout_width y layout_height como wrap_content.

3. Definir un atributo hint como un string llamado edit_message.

El elemento < EditarTexto > debe quedar así:

```xml
<EditText android:id="@+id/edit_message"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:hint="@string/edit_message" />
```

**android:id**

    Esto proporciona un identificador único para la vista, que se puede utilizar para hacer
    referencia al objeto a partir de código de la aplicación, por ejemplo, para leer y manipular
    el objeto.

    La arroba (@) es necesario para que usted se refiere a cualquier objeto de recurso de XML.
    Es seguido por el tipo de recurso (id en este caso), una barra, entonces el nombre del
    recurso (edit_message).

    El signo más (+) antes de que el tipo de recurso que sólo se necesita cuando se está
    definiendo un identificador de recursos por primera vez. Al compilar la aplicación, las
    herramientas de SDK utilizan el nombre de ID para crear un nuevo identificador de recurso en
    el archivo GEN / R.java de su proyecto que se refiere al elemento EditText. Con el ID del
    recurso que declaró una vez de esta manera, otras referencias al ID no necesitan el signo.
    Utilizando el signo es necesaria sólo cuando se especifica un nuevo identificador de
    recursos y no es necesario para los recursos concretos, tales como cadenas o diseños.

**android:layout_width y android:layout_height**

    En lugar de utilizar tamaños específicos para la anchura y la altura, el valor "wrap_content"
    especifica que la vista debe ser sólo tan grande como sea necesario para adaptarse a los
    contenidos de la vista. Si se va a usar en su lugar "match_parent", entonces el elemento
    EditText llenaría la pantalla, ya que podría coincidir con el tamaño del LinearLayout.

**android:hint**

    Esta es una cadena predeterminada que se mostrará cuando el campo de texto este vacía. En
    lugar de utilizar un string codificado como el valor, el "@string/edit_message" se refiere a
    un recurso de string se definen en un archivo separado. Debido a esto se refiere a un recurso
    concreto (no sólo un identificador), que no necesita el signo más. Sin embargo, debido a que
    no ha definido el recurso string, verá un error de compilación en un primer momento. Vas a
    arreglar esto en la siguiente sección mediante la definición del string.

```
Nota: Este string tiene el mismo nombre que el id del elemento :edit_message. Sin embargo, las
referencias a los recursos siempre están en el ámbito por el tipo de recurso (por ejemplo, id o
string), por lo que usar el mismo nombre no causa colisiones.
```

#### Agregando strings

Por defecto, su proyecto Android incluye un archivo de recurso de string en res/values/strings.xml. A continuación, vamos a añadir una nueva cadena denominada "edit_message" y establece el valor a "Introduzca un mensaje."

1. En Android de estudio, a partir de la res/values, abrir el archivo strings.xml.

2. Añada una línea para una cadena denominada "edit_message" con el valor "Introduzca un mensaje".

3. Añada una línea para una cadena denominada "button_send" con el valor "Enviar".

4. Vamos a crear el botón que utiliza esta cadena en la siguiente sección.

El resultado para strings.xml se ve así:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">My First App</string>
    <string name="edit_message">Enter a message</string>
    <string name="button_send">Send</string>
    <string name="action_settings">Settings</string>
</resources>
```

#### Agregando un botón

1. En Android de estudio, desde el directorio res/layout, edite el archivo activity_main.xml.

2. Dentro del elemento < LinearLayout >, definir un elemento < Button > inmediatamente después del elemento < EditarTexto >.

3. Establecer el ancho del botón y atributos de altura para "wrap_content" para que el botón es sólo tan grande como sea necesario para adaptarse a la etiqueta de texto del botón.

4. Definir etiqueta de texto del botón con el atributo android:text; establecer su valor en el recurso de cadena button_send ha definido en la sección anterior.

Su < LinearLayout > debería tener este aspecto:

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

El Layout está diseñado de modo que tanto el EditText y el botón se tan grande como sea necesario para adaptarse a su contenido, tal como muestra la figura 2.

<img src="https://developer.android.com/images/training/firstapp/edittext_wrap.png" width="600">

Esto funciona muy bien para el botón, pero no así para el campo de texto, ya que el usuario puede escribir algo más largo. Sería bueno para llenar el ancho de la pantalla sin usar con el campo de texto. Usted puede hacer esto dentro de un LinearLayout con la propiedad de peso, que se puede especificar mediante el atributo android: layout_weight.

El valor de peso es un número que especifica la cantidad de espacio que queda cada vista debe consumir, en relación con la cantidad consumida por vistas hermanas. Esto funciona algo así como la cantidad de los ingredientes en una receta de una bebida: "2 partes de soda, 1 parte de jarabe" significa dos tercios de la bebida es soda. Por ejemplo, si se le da un punto de vista un peso de 2 y otro un peso de 1, la suma es 3, por lo que el primer punto de vista se llena 2/3 del espacio restante y el segundo punto de vista se llena el resto. Si se agrega una tercera vista y darle un peso de 1, entonces la primera vista (con un peso de 2) ahora recibe 1/2 del espacio restante, mientras que los dos restantes obtienen, cada 1/4.

El peso por defecto para todos los puntos de vista es 0, por lo que si se especifica ningún valor de peso superior a 0 a un solo punto de vista, entonces ese punto de vista se llena cualquier espacio que queda después de todas las vistas se les da el espacio que requieren.

#### Hacer que el EditText rellene el ancho de pantalla

Para llenar el espacio restante en tu diseño con el elemento EditText, haga lo siguiente:

1. En el archivo activity_main.xml, asigne en < EditarTexto > al layout_weight un valor de 1.

2. Además, asignar igual en < EditarTexto > a layout_width valor de 0dp.

```xml
<EditText
    android:layout_weight="1"
    android:layout_width="0dp"
    ... />
```

Para mejorar la eficiencia de diseño cuando se especifica el peso, usted debe cambiar el ancho de la EditarTexto a ser cero (0DP). Ajuste de la anchura a cero mejora el rendimiento de diseño, porque el uso de "wrap_content" como la anchura requiere que el sistema calcule una anchura que es en última instancia es irrelevante debido a que el valor de peso requiere otro cálculo de ancho para llenar el espacio restante.

Como debe quedar el layout: [Codigo](/chapter3/main.xml)

#### Load the XML Resource

Al compilar la aplicación, cada archivo del formato XML se compila en un recurso View. Debe cargar el recurso diseño desde código de la aplicación, en su aplicación desde el metodo Activity.onCreate(). Hacerlo llamando setContentView(), pasándole la referencia al recurso de diseño en forma de: R.layout.layout_file_name. Por ejemplo, si su diseño XML se guarda como main_layout.xml, debería cargarla para su actividad, así:

```java
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.main_layout);
}
```

El método onCreate() en su actividad es llamado por Android cuando se inicia su actividad.

#### Layout Parameters

Atributos de diseño XML denominado layout_something definen los parámetros de diseño para la vista que son apropiados para el ViewGroup en el que reside.

Cada clase ViewGroup implementa una clase anidada que se extiende de ViewGroup.LayoutParams. Esta subclase contiene los tipos de propiedad que definen el tamaño y la posición de cada punto de vista de hijo, según sea apropiado para el grupo de vistas. Como se puede ver en la figura 1, el grupo de vista padre define los parámetros de diseño para cada vista hija (incluyendo el grupo de la vista del hijo).

<img src="https://developer.android.com/images/layoutparams.png" width="600">

```
Nota: que cada LayoutParams subclase tiene su propia sintaxis para establecer valores. Cada
elemento hijo debe definir LayoutParams que sean apropiados para su padre, aunque también puede
definir diferentes LayoutParams por sus propios hijos.
```

Todos los grupos de vistas incluyen un ancho y altura (layout_width y layout_height), y se requiere que cada view los defina. Muchos LayoutParams también incluyen márgenes y bordes opcionales.

Puede especificar el ancho y la altura con medidas exactas, aunque probablemente no querras hacer esto a menudo. Más a menudo, se utiliza una de estas constantes para establecer la anchura o altura:

+ **wrap_content:** le dice a su punto de vista propio tamaño a las dimensiones requeridas por su contenido.

+ **match_parent:** le dice a su fin de llegar a ser tan grande como su grupo de vista padre se lo permita.

En general, no se recomienda especificar un ancho de la disposición y altura usando unidades absolutas como píxeles. En su lugar, el uso de mediciones relativas tales como unidades independientes de la densidad de píxeles (dp), wrap_content, o match_parent, es un mejor enfoque, ya que ayuda a asegurar que su aplicación mostrará correctamente en una variedad de tamaños de pantalla del dispositivo.

#### Layout Position

La geometría de una vista es la de un rectángulo. Una vista tiene una ubicación, expresado como un par de coordenadas izquierda y superior, y dos dimensiones, expresado como un ancho y una altura. La unidad para la ubicación y las dimensiones es el pixel.

Es posible recuperar la ubicación de un punto de vista invocando los métodos getLeft() y getTop(). Regresa la izquierda, o X, coordenadas del rectángulo que representa la vista. Este último regresa el top, o Y, coordenadas del rectángulo que representa la vista. Estos métodos ambos regresan la ubicación de la vista con respecto a su padre. Por ejemplo, cuando getLeft() devuelve 20, eso significa que la vista se encuentra a 20 píxeles a la derecha del borde izquierdo de su padre directo.

Además, varios métodos de conveniencia se ofrecen para evitar cálculos innecesarios, como Getright() y getBottom(). Estos métodos devuelven las coordenadas de los bordes derecho e inferior de la rectángulo que representan la vista. Por ejemplo, llamar a getRight() es similar al siguiente cálculo: getLeft() + getWidth().

#### Size, Padding y Margins

El tamaño de un punto de vista se expresa con una ancho y una altura. Una visión en realidad posee dos pares de valores de ancho y altura.

El primer par se conoce como ancho medida y altura medida. Estas dimensiones definen qué tan grande quiere estar dentro de su padre. Las dimensiones medidas se pueden obtener llamando getMeasuredWidth() y getMeasuredHeight().

El segundo par se conoce simplemente como el ancho y la altura. Estas dimensiones definen el tamaño real de la vista en pantalla, en tiempo de dibujo y después de la disposición. Estos valores pueden, pero no tienen que, ser diferente del ancho y la altura medida. El ancho y la altura se pueden obtener llamando getWidth() y getHeight().

Para medir sus dimensiones, un punto de vista toma en cuenta su padding. El padding se expresa en píxeles de la izquierda, arriba, derecha y parte inferior de la vista. El padding se puede utilizar para compensar el contenido de la vista por un número específico de píxeles. Por ejemplo, un padding a la izquierda de 2 empujará el contenido de la vista por 2 píxeles a la derecha del borde izquierdo. El padding se puede ajustar mediante el método (int, int, int, int) setPadding y consulta llamando getPaddingLeft(), getPaddingTop(), getPaddingRight() y getPaddingBottom().
