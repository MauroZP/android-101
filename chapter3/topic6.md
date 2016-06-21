### Diálogos

Un diálogo es una pequeña ventana que pide al usuario que tome una decisión o introducir información adicional. Un diálogo no ocupa toda la pantalla y normalmente se utiliza para eventos modales que requieren los usuarios para realizar una acción antes de que puedan proceder.


#### Pickers

Android proporciona controles para que el usuario escoja una hora o elegir una fecha. Cada selector proporciona controles para la selección de cada parte del tiempo (horas, minutos, AM / PM) o fecha (mes, día, año). El uso de estos recolectores se garantiza que los usuarios pueden elegir una hora o fecha que sea válida, el formato correcto, y se ajusta a la configuración regional del usuario.

<img src="https://developer.android.com/images/ui/pickers.png" width="400">

##### Creando un Time Picker

Para mostrar un TimePickerDialog usando DialogFragment, es necesario definir una clase fragmento que se extiende DialogFragment y devolver una DatePickerDialog del método onCreateDialog() fragmento.

###### Extender un DialogFragment para un time picker

Para definir un DialogFragment para un TimePickerDialog, usted debe:

1. Definir el metodo onCreateDialog() que regrese una instancia de TimePickerDialog

2. Implementar la interface TimePickerDialog.OnTimeSetListener que regrese cuando el usuario seleccione una hora.

Ejemplo:

```java
public static class TimePickerFragment extends DialogFragment
                            implements TimePickerDialog.OnTimeSetListener {

    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
        // Utilizar el time actual como los valores por defecto para el selector
        final Calendar c = Calendar.getInstance();
        int hour = c.get(Calendar.HOUR_OF_DAY);
        int minute = c.get(Calendar.MINUTE);

        // Crear una nueva instancia de diálogo Selector de fechas y devolverlo
        return new TimePickerDialog(getActivity(), this, hour, minute,
                DateFormat.is24HourFormat(getActivity()));
    }

    public void onTimeSet(TimePicker view, int hourOfDay, int minute) {
        // Hacer algo con el time elegido por el usuario
    }
}
```

###### Showing the time picker

Una vez que haya definido una DialogFragment como el que se muestra arriba, puede mostrar el selector de tiempo mediante la creación de una instancia de la DialogFragment y llamar a show().

Por ejemplo, aquí hay un botón que, al ser pulsado, llama a un método para mostrar el cuadro de diálogo:

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/pick_time"
    android:onClick="showTimePickerDialog" />
```

Cuando el usuario hace clic en el botón, el sistema llama el método siguiente:

```java
public void showTimePickerDialog(View v) {
    DialogFragment newFragment = new TimePickerFragment();
    newFragment.show(getSupportFragmentManager(), "timePicker");
}
```

Este método llama show() en una nueva instancia de la DialogFragment definido anteriormente. El método show() requiere una instancia de FragmentManager y un nombre de etiqueta único para el fragmento.

##### Creando un Date Picker

La creación de un DatePickerDialog es igual que la creación de un TimePickerDialog. La única diferencia es el diálogo se crea para el fragmento.

Para mostrar un DatePickerDialog usando DialogFragment, es necesario definir una clase fragmento que se extiende DialogFragment y devolver una DatePickerDialog desde el método del fragmento onCreateDialog().

###### Extending DialogFragment for a date picker

Para definir un DialogFragment para un DatePickerDialog, usted debe:

1. Definir el método onCreateDialog() para devolver una instancia de DatePickerDialog

2. Implementar la interfaz DatePickerDialog.OnDateSetListener para recibir una llamada cuando el usuario establece la fecha.

Ejemplo:

```java
public static class DatePickerFragment extends DialogFragment
                            implements DatePickerDialog.OnDateSetListener {

    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
        // Utilice la fecha actual como la fecha predeterminada en el selector
        final Calendar c = Calendar.getInstance();
        int year = c.get(Calendar.YEAR);
        int month = c.get(Calendar.MONTH);
        int day = c.get(Calendar.DAY_OF_MONTH);

        // Crear una nueva instancia de DatePickerDialog y devolverlo
        return new DatePickerDialog(getActivity(), this, year, month, day);
    }

    public void onDateSet(DatePicker view, int year, int month, int day) {
        // Hacer algo con la fecha elegida por el usuario
    }
}
```

Ahora todo lo que necesita es un evento que agrega una instancia de este fragmento de su actividad

###### Mostrando el date picker

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














