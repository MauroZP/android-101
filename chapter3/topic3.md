### Input Controls

Los inputs son los componentes interactivos en la interfaz de usuario de la aplicación. Android ofrece una amplia variedad de controles que se pueden utilizar en la interfaz de usuario, tales como buttons, text fields, seek bars, checkboxes, zoom buttons, toggle buttons, y muchos más.

<img src="https://developer.android.com/images/ui/ui-controls.png" width="400">

#### Buttons

Un botón consiste en texto o un icono (o texto y un icono) que se comunica lo que se produce la acción cuando el usuario lo toca.

<img src="https://developer.android.com/images/ui/button-types.png" width="400">

En función de si desea un botón con el texto, un icono, o ambos, puede crear el botón en su diseño de tres maneras:

**Con el texto, usando la clase Button:**

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/button_text"
    ... />
```

**Con un icono, utilizando la clase de imagen del botón:**
```xml
<ImageButton
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/button_icon"
    ... />
```
**Con texto y un icono, utilizando la clase Button con el atributo android:drawableLeft:**
```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/button_text"
    android:drawableLeft="@drawable/button_icon"
    ... />
```

##### Respondiendo a Eventos de Click

Cuando el usuario hace clic en un botón, el objeto Button recibe un evento de clic.

Para definir el controlador de eventos Click para un botón, añadir el atributo android:onclick al elemento < Button > en su diseño XML. El valor de este atributo debe ser el nombre del método que desea llamar en respuesta a un evento de clic. La actividad de alojamiento a continuación, el diseño debe implementar el método correspondiente.

Por ejemplo, aquí está un diseño con un botón con android:onClick :

```xml
<?xml version="1.0" encoding="utf-8"?>
<Button xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/button_send"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/button_send"
    android:onClick="sendMessage" />
```

Dentro de la actividad que acoge esta vista, el siguiente método maneja el evento click:

```java
/** Se llama cuando el usuario toca el botón */
public void sendMessage(View view) {
    // Hacer algo en respuesta al clic de botón
}
```

El método se declara en el atributo android:onclick debe tener una firma tal y como se muestra arriba. En concreto, el método debe:

+ Ser publico
+ Devolver void
+ Definir un View como su único parámetro (esto será el view que se ha hecho clic)

##### Usando el OnClickListener

También puede declarar el controlador de eventos mediante programación en lugar de hacer clic en un diseño XML. Esto puede ser necesario si usted instancia del botón en tiempo de ejecución o si tiene que declarar el comportamiento en una subclase Fragment.

Declarar el controlador de eventos mediante programación, crear un objeto View.OnClickListener y asignarla al botón llamando setOnClickListener (View.OnClickListener). Por ejemplo:

```java
Button button = (Button) findViewById(R.id.button_send);
button.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        // Hacer algo en respuesta al clic de botón
    }
});
```

##### Button Sin bordes

Un diseño que puede ser útil es un botón "sin bordes". botones sin bordes parecen botones básicos, excepto que no tienen bordes ni fondo, pero aún cambian de aspecto durante diferentes estados, como cuando se hace clic.

Para crear un botón sin bordes, aplicar el estilo borderlessButtonStyle al botón. Por ejemplo:

```xml
<Button
    android:id="@+id/button_send"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/button_send"
    android:onClick="sendMessage"
    style="?android:attr/borderlessButtonStyle" />
```

##### Custom background

Si desea volver a definir realmente el aspecto de un botón, puede especificar un fondo personalizado. En lugar de proporcionar un mapa de bits simple o color, sin embargo, el fondo debe ser un recurso de la lista de estado que cambia de aspecto dependiendo del estado actual del botón.

Se puede definir la lista del estado en un archivo XML que define tres imágenes o colores diferentes de usar para los diferentes estados de botón.

Para crear una lista de estados para su fondo de botón:

1. Crear tres mapas de bits para el fondo del botón que representa el valor por defecto, presionado, y estados de botón enfocadas. Para asegurarse de que sus imágenes se ajustan los botones de diversos tamaños, crear los mapas de bits como mapas de bits de nueve parches.

2. Coloque los mapas de bits en res/drawable/. Asegúrese de que cada mapa de bits tiene el nombre correcto para reflejar el estado de los botones que representan cada uno, como button_default.9.png, button_pressed.9.png, y button_focused.9.png.

3. Crear un nuevo archivo XML en res/drawable (nombrarlo algo así como button_custom.xml). Inserte el siguiente código XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/button_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/button_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/button_default" />
</selector>
```

	Nota: El orden de los elementos <item> es importante. Cuando se hace referencia a este
    drawable, los elementos <item> son renderizados en el fin de determinar cuál es el adecuado
    para el estado del botón actual. Debido a que el mapa de bits por defecto es la última, que
    sólo se aplica cuando android ha evaluado las condiciones: state_pressed y android: state_focused como false.

```xml
<Button
    android:id="@+id/button_send"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/button_send"
    android:onClick="sendMessage"
    android:background="@drawable/button_custom"  />
```

#### Text Fields

Un campo de texto permite al usuario escribir texto en su aplicación. Puede ser una sola línea o varias líneas. Tocar un campo de texto coloca el cursor y muestra automáticamente el teclado. Además de escribir, campos de texto permiten una variedad de otras actividades, tales como la selección de texto (cortar, copiar, pegar) y datos de consulta a través de auto-completado.

Puede añadir un campo de texto para usted diseño con el objeto EditText. Por lo general, debe hacerlo en su diseño de XML con un elemento < EditarTexto >.

<img src="https://developer.android.com/images/ui/edittext-noextract.png" width="400">

##### Especificación del tipo de teclado

Los campos de texto pueden tener diferentes tipos de entrada, como el número, la fecha, la contraseña o dirección de correo electrónico. El tipo determina lo que está permitido tipo de caracteres dentro del campo, y le puede indicar el teclado virtual para optimizar su diseño para los caracteres utilizados con frecuencia.

Se puede especificar el tipo de teclado que desea para su objeto EditText con el atributo android:inputType. Por ejemplo, si desea que el usuario introduzca una dirección de correo electrónico, se debe utilizar el tipo de entrada textEmailAddress:

```xml
<EditText
    android:id="@+id/email_address"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:hint="@string/email_hint"
    android:inputType="textEmailAddress" />
```

Hay varios tipos de entrada disponibles para diferentes situaciones. Éstos son algunos de los valores más comunes para android:inputType:

**"text"**

	Normal text keyboard.

**"textEmailAddress"**

	Normal text keyboard con el caracter @.

**"textUri"**

	Normal text keyboard con el caracter /.

**"number"**

	Basico number keypad.

**"phone"**

	Phone-style keypad.

##### Control de otros comportamientos

El androide: inputType también le permite especificar ciertos comportamientos del teclado, como si hacer mayusculas todas las palabras nuevas o características del uso como sugerencias de auto-completar y ortográficos.

El atributo android:inputType permite combinaciones de bits para que pueda especificar un diseño de teclado y una o más conductas a la vez.

Éstos son algunos de los valores de tipo de entrada comunes que definen los comportamientos de teclado:

**"textCapSentences"**

	Normal text keyboard que capitaliza la primera letra de cada frase nueva.

**"textCapWords"**

	Normal text keyboard que capitaliza cada palabra. Bueno para los títulos o nombres de personas.

**"textAutoCorrect"**

	Normal text keyboard que corrige las palabras mal escritas habitualmente.

**"textPassword"**

	Normal text keyboard, pero los caracteres introducidos se convierten en puntos.

**"textMultiLine"**

	Normal text keyboard que permiten a los usuarios de entrada de largas cadenas de texto que incluyen saltos de línea.

Por ejemplo, he aquí cómo puede obtener una dirección postal, en mayúsculas cada palabra y desactivar las sugerencias de texto:

```xml
<EditText
    android:id="@+id/postal_address"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:hint="@string/postal_address_hint"
    android:inputType="textPostalAddress|
                       textCapWords|
                       textNoSuggestions" />
```

##### Specifying Keyboard Actions

Además de cambiar el tipo de entrada del teclado, Android le permite especificar una acción que se realiza cuando los usuarios hayan completado su entrada. La acción especifica el botón que aparece en lugar de la tecla de retorno de carro y la acción a realizar, tales como "Búsqueda" o "Enviar".

Puede especificar la acción estableciendo con android:imeOptions. Por ejemplo, aquí es cómo se puede especificar la acción Enviar:

```xml
<EditText
    android:id="@+id/search"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:hint="@string/search_hint"
    android:inputType="text"
    android:imeOptions="actionSend" />
```

Si no se especifica explícitamente una acción de entrada a continuación, el sistema intenta determinar si hay cualquier android antes, campos enfocables. Si cualquiera de los campos enfocable se encuentran a despues de éste, el sistema aplica la acción "actionNext" al EditText actual por lo que el usuario puede seleccionar Siguiente para pasar al siguiente campo. Si no hay ningún campo enfocable siguiente, el sistema aplica la acción "actionDone". Puede anular esta configurando el android:imeOptions cambiando por cualquier otro valor como "actionSend" o "actionSearch" o suprimir el comportamiento predeterminado mediante la acción "actionNone".

##### Respondiendo a la acción de button events

Si ha especificado una acción de teclado para el método de entrada usando android:imeOptions (como "actionSend"), se puede detectar el evento de acción específico utilizando un TextView.OnEditorActionListener. La interfaz TextView.OnEditorActionListener proporciona un método de llamado onEditorAction() que indica el tipo de acción invocada con un ID de acción como IME_ACTION_SEND o IME_ACTION_SEARCH.

Por ejemplo, aquí es cómo se puede detectar cuando el usuario hace clic en el botón Enviar en el teclado:

```java
EditText editText = (EditText) findViewById(R.id.search);
editText.setOnEditorActionListener(new OnEditorActionListener() {
    @Override
    public boolean onEditorAction(TextView v, int actionId, KeyEvent event) {
        boolean handled = false;
        if (actionId == EditorInfo.IME_ACTION_SEND) {
            sendMessage();
            handled = true;
        }
        return handled;
    }
});
```

#### Checkboxes

Los checkboxes permiten al usuario seleccionar una o más opciones de un conjunto. Por lo general, usted debe presentar cada opción de casilla de verificación en una lista vertical.

<img src="https://developer.android.com/images/ui/checkboxes.png" width="400">

Para crear cada opción de checkbox, crear un CheckBox en su diseño. Debido a que un conjunto de opciones de checkbox permite al usuario seleccionar varios elementos, cada casilla es administrado por separado y se debe registrar un listener clic para cada uno.

##### Respondiendo a eventos Click

Cuando el usuario selecciona un checkbox, el objeto CheckBox recibe un evento de clic.

Para definir el controlador de eventos clic para un checkbox, añadir el android:onclick al elemento < CheckBox > en su diseño XML. El valor de este atributo debe ser el nombre del método que desea llamar en respuesta a un evento de clic. La actividad del diseño debe implementar el método correspondiente.

Por ejemplo, aquí hay un par de objetos CheckBox en una lista:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <CheckBox android:id="@+id/checkbox_meat"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/meat"
        android:onClick="onCheckboxClicked"/>
    <CheckBox android:id="@+id/checkbox_cheese"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/cheese"
        android:onClick="onCheckboxClicked"/>
</LinearLayout>
```

Dentro de la actividad esta el siguiente método que maneja el evento click para ambas casillas de verificación:

```java
public void onCheckboxClicked(View view) {
    // Es checked?
    boolean checked = ((CheckBox) view).isChecked();

    // Cual checkbox fue checkeado
    switch(view.getId()) {
        case R.id.checkbox_meat:
            if (checked)
                // Agrega carne al sandwich
            else
                // Quitale la carne
            break;
        case R.id.checkbox_cheese:
            if (checked)
                // Ponle queso
            else
                // No tolero el queso
            break;
    }
}
```
+ Ser publico
+ Devolver void
+ Definir un View como su único parámetro (esto será el view que se ha hecho clic)

#### Radio Buttons

Los radio buttons permiten al usuario seleccionar una opción de un conjunto. Debe utilizar los radio buttons para los conjuntos opcionales que son mutuamente excluyentes si usted piensa que el usuario necesita ver todas las opciones disponibles de lado a lado. Si no es necesario para mostrar todas las opciones de lado a lado, utilizar un spinner en su lugar.

<img src="https://developer.android.com/images/ui/radiobuttons.png" width="400">

Para crear cada opción, crear un RadioButton en su diseño. Sin embargo, ya que los botones de radio son mutuamente excluyentes, es necesario agruparlos juntos dentro de una RadioGroup. Agrupándolas, el sistema garantiza que sólo un botón de opción se puede seleccionar a la vez.

##### Respondiendo a to Click Events

Cuando el usuario selecciona uno de los radio button, el objeto RadioButton correspondiente recibe un evento de clic.

Para definir el Click para un botón, añadir el android:onclick atributo al elemento <RadioButton> en su diseño XML. El valor de este atributo debe ser el nombre del método que desea llamar en respuesta a un evento de clic. La actividad de alojamiento a continuación.

Por ejemplo, aquí hay un par de objetos RadioButton:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RadioGroup xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical">
    <RadioButton android:id="@+id/radio_pirates"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/pirates"
        android:onClick="onRadioButtonClicked"/>
    <RadioButton android:id="@+id/radio_ninjas"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/ninjas"
        android:onClick="onRadioButtonClicked"/>
</RadioGroup>
```

	Nota: El RadioGroup es una subclase de LinearLayout que tiene una orientación vertical de forma predeterminada.

Dentro de la actividad, el siguiente método maneja el evento click para ambos botones de radio:

```java
public void onRadioButtonClicked(View view) {
    // Is the button now checked?
    boolean checked = ((RadioButton) view).isChecked();

    // Check which radio button was clicked
    switch(view.getId()) {
        case R.id.radio_pirates:
            if (checked)
                // Pirates are the best
            break;
        case R.id.radio_ninjas:
            if (checked)
                // Ninjas rule
            break;
    }
}
```

El método que se declara en el android:onclick atributo debe tener una firma tal y como se muestra arriba. En concreto, el método debe :

+ Ser publico
+ Devolver void
+ Definir un View como su único parámetro (esto será el view que se ha hecho clic)


#### Toggle Buttons

Un botón toglle permite al usuario cambiar un ajuste entre dos estados.

Puede añadir un botón toglle use el objeto ToggleButton. Android 4.0 (API de nivel 14) introduce otro tipo de botón de conmutación llama un interruptor que proporciona un control deslizante, el cual puede añadir con un objeto Switch.

<img src="https://developer.android.com/images/ui/switch.png" width="400">

##### Respondiendo al evento click del Button

Para detectar cuando el usuario activa el botón o interruptor, crear un objeto CompoundButton.OnCheckedChangeListener y asignarla al botón llamando setOnCheckedChangeListener (). Por ejemplo:

```java
ToggleButton toggle = (ToggleButton) findViewById(R.id.togglebutton);
toggle.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        if (isChecked) {
            // The toggle is enabled
        } else {
            // The toggle is disabled
        }
    }
});
``

#### Spinners

Un Spinner proporcionan una manera rápida para seleccionar un valor de un conjunto. En el estado por defecto, una ruleta muestra su valor seleccionado en ese momento. Al tocar el spinner muestra un menú desplegable con todos los demás valores disponibles, de los cuales el usuario puede seleccionar una nueva.

<img src="https://developer.android.com/images/ui/spinner.png" width="400">

Puede agregar este control con el objeto Spinner. Por lo general, debe hacerlo en su diseño de XML con un elemento <Spinner>. Por ejemplo:

```xml
<Spinner
    android:id="@+id/planets_spinner"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />

```

Para llenar la ruleta con una lista de opciones, este caso es necesario especificar un SpinnerAdapter en su actividad o un Fragment.

##### Llenar el Spinner con opciones del usuario

Las cadenas que llenan el spinner pueden venir de cualquier fuente, pero tienen que ser proporcionadas a través de un SpinnerAdapter, tal como un ArrayAdapter si las opciones están disponibles en una array o un CursorAdapter si las opciones están disponibles a partir de una consulta de base de datos.

Por ejemplo, si las opciones disponibles para su spinner son pre-determinado, se les puede proporcionar con una array de cadenas se definen en un archivo de recurso de arrays:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="planets_array">
        <item>Mercury</item>
        <item>Venus</item>
        <item>Earth</item>
        <item>Mars</item>
        <item>Jupiter</item>
        <item>Saturn</item>
        <item>Uranus</item>
        <item>Neptune</item>
    </string-array>
</resources>
```

Puede utilizar el siguiente código en su actividad o fragmento para abastecer el spinner con el array mediante una instancia de ArrayAdapter:

```java
Spinner spinner = (Spinner) findViewById(R.id.spinner);
// Crear una ArrayAdapter utilizando el array de strings y un diseño predeterminado spinner
ArrayAdapter<CharSequence> adapter = ArrayAdapter.createFromResource(this,
        R.array.planets_array, android.R.layout.simple_spinner_item);
// Especificar el diseño a utilizar cuando aparezca la lista de opciones
adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
// Aplicar el adapter a la spinner
spinner.setAdapter(adapter);
```

El método createFromResource() le permite crear una ArrayAdapter del array de strings. El tercer argumento de este método es un recurso de diseño que define la forma en que aparecera la opción seleccionada. La disposición simple_spinner_item es proporcionado por la plataforma y es el diseño predeterminado que debe utilizar a menos que desea definir su propio diseño de la apariencia del spinner.

A continuación, debe llamar setDropDownViewResource(int) para especificar el diseño del adaptador debe utilizar para mostrar la lista de opciones (simple_spinner_dropdown_item es otro diseño estándar definido por la plataforma).

Llame setAdapter() para aplicar el adaptador a su Spinner.

##### Respondiendo a la selección de usuario

Cuando el usuario selecciona un elemento de la lista desplegable, el objeto Spinner recibe un evento en el elemento seleccionado.

Para definir el controlador de eventos, implementar la interfaz AdapterView.OnItemSelectedListener y el método de devolución de llamada correspondiente onItemSelected(). Por ejemplo, aquí está una implementación de la interfaz en una actividad:

```java
public class SpinnerActivity extends Activity implements OnItemSelectedListener {
    ...

    public void onItemSelected(AdapterView<?> parent, View view,
            int pos, long id) {
        // Se ha seleccionado un elemento. Puede recuperar el elemento seleccionado usando
        // parent.getItemAtPosition(pos)
    }

    public void onNothingSelected(AdapterView<?> parent) {
        // Otra interfaz de devolución de llamada
    }
}
```

El AdapterView.OnItemSelectedListener requiere que se implementen los onItemSelected() y onNothingSelected().

Luego hay que especificar la implementación de la interfaz llamando setOnItemSelectedListener():

```java
Spinner spinner = (Spinner) findViewById(R.id.spinner);
spinner.setOnItemSelectedListener(this);
```

Si implementa la interfaz AdapterView.OnItemSelectedListener con su actividad o fragmento (como en el ejemplo anterior), puede pasar esto como la instancia de interfaz.
