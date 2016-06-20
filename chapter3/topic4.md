### Guardando conjuntos Key-Value

Si tienes una colección relativamente pequeña de valores que deseas guardar, debes utilizar la API SharedPreferences. SharedPreferences apunta a un archivo que contiene pares de valores clave y proporciona métodos sencillos para leer y escribir de ellos. Cada archivo SharedPreferences es administrado por el marco y puede ser privado o compartido.

#### Obtener un identificador para SharedPreferences

Se puede crear un nuevo archivo de SharedPreferences o usar una ya existente llamando a uno de estos dos métodos:

**getSharedPreferences():** Utilice esta opción si necesita múltiples archivos de preferencias compartidas identificados por su nombre, que se especifica con el primer parámetro. Puede llamar a este desde cualquier contexto en su aplicación.

**getPreferences():** Use esto desde una actividad si es necesario utilizar un único archivo de preferencias compartido para la actividad. Debido a que este recupera un archivo de preferencias por defecto compartido que pertenece a la actividad, no es necesario que proporcione un nombre.

Por ejemplo, el siguiente código se ejecuta dentro de un fragmento. Se accede al archivo de preferencias compartida que se identifica por la cadena de recursos R.string.preference_file_key y la abre utilizando el modo privado por lo que el archivo es accesible sólo por su aplicación.

```java
Context context = getActivity();
SharedPreferences sharedPref = context.getSharedPreferences(
        getString(R.string.preference_file_key), Context.MODE_PRIVATE);
```

Al asignar nombres a los archivos compartidos de preferencia, se debe utilizar un nombre que es único de identificación para su aplicación, tales como "com.example.myapp.PREFERENCE_FILE_KEY"

Alternativamente, si necesitas un solo archivo de preferencias compartida para su actividad, se puede utilizar el método getPreferences ():

```java
SharedPreferences sharedPref = getActivity().getPreferences(Context.MODE_PRIVATE);
```

	Precaución: Si crea un archivo de preferencias compartidas con MODE_WORLD_READABLE o
	MODE_WORLD_WRITEABLE, cualquier otra aplicacion que conoce el identificador de archivo
	puede acceder a sus datos.

#### Escribir en Shared Preferences

Para escribir en un archivo de preferencias compartida, crear un SharedPreferences.Editor llamando edit() en sus SharedPreferences.

Pasar las key-value que desea escribir con métodos tales como putInt() y putString(). A continuación, llame commit() para guardar los cambios. Por ejemplo:

```java
SharedPreferences sharedPref = getActivity().getPreferences(Context.MODE_PRIVATE);
SharedPreferences.Editor editor = sharedPref.edit();
editor.putInt(getString(R.string.saved_high_score), newHighScore);
editor.commit();
```

#### Leer desde Shared Preferences
Para recuperar los valores de un archivo de preferencias compartida, llamar a métodos tales como getInt() y getString(), y proporcione el key para el value que desee y, opcionalmente, un valor por defecto para volver si la clave no está presente. Por ejemplo:

```java
SharedPreferences sharedPref = getActivity().getPreferences(Context.MODE_PRIVATE);
int defaultValue = getResources().getInteger(R.string.saved_high_score_default);
long highScore = sharedPref.getInt(getString(R.string.saved_high_score), defaultValue);
```