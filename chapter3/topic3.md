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

Radio buttons allow the user to select one option from a set. You should use radio buttons for optional sets that are mutually exclusive if you think that the user needs to see all available options side-by-side. If it's not necessary to show all options side-by-side, use a spinner instead.

https://developer.android.com/images/ui/radiobuttons.png

To create each radio button option, create a RadioButton in your layout. However, because radio buttons are mutually exclusive, you must group them together inside a RadioGroup. By grouping them together, the system ensures that only one radio button can be selected at a time.

##### Responding to Click Events

When the user selects one of the radio buttons, the corresponding RadioButton object receives an on-click event.

To define the click event handler for a button, add the android:onClick attribute to the <RadioButton> element in your XML layout. The value for this attribute must be the name of the method you want to call in response to a click event. The Activity hosting the layout must then implement the corresponding method.

For example, here are a couple RadioButton objects:

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

	Note: The RadioGroup is a subclass of LinearLayout that has a vertical orientation by default.

Within the Activity that hosts this layout, the following method handles the click event for both radio buttons:

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

The method you declare in the android:onClick attribute must have a signature exactly as shown above. Specifically, the method must:

+ Be public
+ Return void
+ Define a View as its only parameter (this will be the View that was clicked)

```



#### Toggle Buttons

A toggle button allows the user to change a setting between two states.

You can add a basic toggle button to your layout with the ToggleButton object. Android 4.0 (API level 14) introduces another kind of toggle button called a switch that provides a slider control, which you can add with a Switch object.

https://developer.android.com/images/ui/switch.png

##### Responding to Button Presses

To detect when the user activates the button or switch, create an CompoundButton.OnCheckedChangeListener object and assign it to the button by calling setOnCheckedChangeListener(). For example:

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

Spinners provide a quick way to select one value from a set. In the default state, a spinner shows its currently selected value. Touching the spinner displays a dropdown menu with all other available values, from which the user can select a new one.

https://developer.android.com/images/ui/spinner.png

You can add a spinner to your layout with the Spinner object. You should usually do so in your XML layout with a <Spinner> element. For example:

```xml
<Spinner
    android:id="@+id/planets_spinner"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />

```

To populate the spinner with a list of choices, you then need to specify a SpinnerAdapter in your Activity or Fragment source code.

##### Populate the Spinner with User Choices

The choices you provide for the spinner can come from any source, but must be provided through an SpinnerAdapter, such as an ArrayAdapter if the choices are available in an array or a CursorAdapter if the choices are available from a database query.

For instance, if the available choices for your spinner are pre-determined, you can provide them with a string array defined in a string resource file:

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
With an array such as this one, you can use the following code in your Activity or Fragment to supply the spinner with the array using an instance of ArrayAdapter:

```java
Spinner spinner = (Spinner) findViewById(R.id.spinner);
// Create an ArrayAdapter using the string array and a default spinner layout
ArrayAdapter<CharSequence> adapter = ArrayAdapter.createFromResource(this,
        R.array.planets_array, android.R.layout.simple_spinner_item);
// Specify the layout to use when the list of choices appears
adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
// Apply the adapter to the spinner
spinner.setAdapter(adapter);
```

The createFromResource() method allows you to create an ArrayAdapter from the string array. The third argument for this method is a layout resource that defines how the selected choice appears in the spinner control. The simple_spinner_item layout is provided by the platform and is the default layout you should use unless you'd like to define your own layout for the spinner's appearance.

You should then call setDropDownViewResource(int) to specify the layout the adapter should use to display the list of spinner choices (simple_spinner_dropdown_item is another standard layout defined by the platform).

Call setAdapter() to apply the adapter to your Spinner.

##### Responding to User Selections

When the user selects an item from the drop-down, the Spinner object receives an on-item-selected event.

To define the selection event handler for a spinner, implement the AdapterView.OnItemSelectedListener interface and the corresponding onItemSelected() callback method. For example, here's an implementation of the interface in an Activity:

```java
public class SpinnerActivity extends Activity implements OnItemSelectedListener {
    ...

    public void onItemSelected(AdapterView<?> parent, View view,
            int pos, long id) {
        // An item was selected. You can retrieve the selected item using
        // parent.getItemAtPosition(pos)
    }

    public void onNothingSelected(AdapterView<?> parent) {
        // Another interface callback
    }
}
```

The AdapterView.OnItemSelectedListener requires the onItemSelected() and onNothingSelected() callback methods.

Then you need to specify the interface implementation by calling setOnItemSelectedListener():

```java
Spinner spinner = (Spinner) findViewById(R.id.spinner);
spinner.setOnItemSelectedListener(this);
```

If you implement the AdapterView.OnItemSelectedListener interface with your Activity or Fragment (such as in the example above), you can pass this as the interface instance.

#### Pickers

Android provides controls for the user to pick a time or pick a date as ready-to-use dialogs. Each picker provides controls for selecting each part of the time (hour, minute, AM/PM) or date (month, day, year). Using these pickers helps ensure that your users can pick a time or date that is valid, formatted correctly, and adjusted to the user's locale.

https://developer.android.com/images/ui/pickers.png

##### Creating a Time Picker

To display a TimePickerDialog using DialogFragment, you need to define a fragment class that extends DialogFragment and return a TimePickerDialog from the fragment's onCreateDialog() method.

###### Extending DialogFragment for a time picker

To define a DialogFragment for a TimePickerDialog, you must:

1. Define the onCreateDialog() method to return an instance of TimePickerDialog

2. Implement the TimePickerDialog.OnTimeSetListener interface to receive a callback when the user sets the time.

Here's an example:

```java
public static class TimePickerFragment extends DialogFragment
                            implements TimePickerDialog.OnTimeSetListener {

    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
        // Use the current time as the default values for the picker
        final Calendar c = Calendar.getInstance();
        int hour = c.get(Calendar.HOUR_OF_DAY);
        int minute = c.get(Calendar.MINUTE);

        // Create a new instance of TimePickerDialog and return it
        return new TimePickerDialog(getActivity(), this, hour, minute,
                DateFormat.is24HourFormat(getActivity()));
    }

    public void onTimeSet(TimePicker view, int hourOfDay, int minute) {
        // Do something with the time chosen by the user
    }
}
```

###### Showing the time picker

Once you've defined a DialogFragment like the one shown above, you can display the time picker by creating an instance of the DialogFragment and calling show().

For example, here's a button that, when clicked, calls a method to show the dialog:

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/pick_time"
    android:onClick="showTimePickerDialog" />
    ```

When the user clicks this button, the system calls the following method:

```java
public void showTimePickerDialog(View v) {
    DialogFragment newFragment = new TimePickerFragment();
    newFragment.show(getSupportFragmentManager(), "timePicker");
}
```

This method calls show() on a new instance of the DialogFragment defined above. The show() method requires an instance of FragmentManager and a unique tag name for the fragment.

##### Creating a Date Picker

Creating a DatePickerDialog is just like creating a TimePickerDialog. The only difference is the dialog you create for the fragment.

To display a DatePickerDialog using DialogFragment, you need to define a fragment class that extends DialogFragment and return a DatePickerDialog from the fragment's onCreateDialog() method.

###### Extending DialogFragment for a date picker

To define a DialogFragment for a DatePickerDialog, you must:

1. Define the onCreateDialog() method to return an instance of DatePickerDialog

2. Implement the DatePickerDialog.OnDateSetListener interface to receive a callback when the user sets the date.

Here's an example:

```java
public static class DatePickerFragment extends DialogFragment
                            implements DatePickerDialog.OnDateSetListener {

    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
        // Use the current date as the default date in the picker
        final Calendar c = Calendar.getInstance();
        int year = c.get(Calendar.YEAR);
        int month = c.get(Calendar.MONTH);
        int day = c.get(Calendar.DAY_OF_MONTH);

        // Create a new instance of DatePickerDialog and return it
        return new DatePickerDialog(getActivity(), this, year, month, day);
    }

    public void onDateSet(DatePicker view, int year, int month, int day) {
        // Do something with the date chosen by the user
    }
}
```

Now all you need is an event that adds an instance of this fragment to your activity

###### Showing the date picker

Once you've defined a DialogFragment like the one shown above, you can display the date picker by creating an instance of the DialogFragment and calling show().

For example, here's a button that, when clicked, calls a method to show the dialog:

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/pick_date"
    android:onClick="showDatePickerDialog" />
```

When the user clicks this button, the system calls the following method:

```java
public void showDatePickerDialog(View v) {
    DialogFragment newFragment = new DatePickerFragment();
    newFragment.show(getSupportFragmentManager(), "datePicker");
}
```

This method calls show() on a new instance of the DialogFragment defined above. The show() method requires an instance of FragmentManager and a unique tag name for the fragment.














