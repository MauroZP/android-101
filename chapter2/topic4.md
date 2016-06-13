### Ciclo de vida de una actividad

<img src="https://developer.android.com/images/training/basics/basic-lifecycle.png" width="700">

**onCreate:** Se llama en la creación de la actividad. Se utiliza para realizar todo tipo de inicializaciones, como la creación de la interfaz de usuario o la inicialización de estructuras de datos. Puede recibir información de estado dela actividad (en una instancia de la clase Bundle), por si se reanuda desde una actividad que ha sido destruida y vuelta a crear.

**onStart:** Nos indica que la actividad está a punto de ser mostrada al usuario.

**onResume:** Se llama cuando la actividad va a comenzar a interactuar con el usuario. Es un buen lugar para lanzar las animaciones y la música.

**onPause:** Indica que la actividad está a punto de ser lanzada a segundo plano, normalmente porque otra actividad es lanzada. Es el lugar adecuado para detener animaciones, música o almacenar los datos que estaban en edición.

**onStop:** La actividad ya no va a ser visible para el usuario. Ojo si hay muy poca memoria, es posible que la actividad se destruya sin llamar a este método.

**onRestart:** Indica que la actividad va a volver a ser representada después de haber pasado por onStop().

**onDestroy:** Se llama antes de que la actividad sea totalmente destruida. Por ejemplo, cuando el usuario pulsa el botón de volver o cuando se llama al método finish(). Ojo si hay muy poca memoria, es posible que la actividad se destruya sin llamar a este método.

##### Destroy Activity

Mientras que la primera llamada de ciclo de vida de la actividad es onCreate(), su última llamada es OnDestroy(). El sistema llama a este método en su actividad como la señal definitiva de que su instancia de actividad está siendo retirado por completo de la memoria del sistema.

La mayoría de las aplicaciones no necesitan poner en práctica este método porque las referencias de clases locales se destruyen con la actividad y su actividad deben realizar la mayor parte de limpieza durante onPause() y onStop (). Sin embargo, si su actividad incluye hilos de fondo que ha creado durante onCreate() u otros recursos de larga duración que podría potencialmente pérdida de memoria si no se ha cerrado correctamente, usted debe matar a ellos durante OnDestroy().

```java
@Override
public void onDestroy() {
    super.onDestroy();  // Siempre llame a la superclase

    // Pare método de seguimiento que la actividad se inició durante onCreate()
    android.os.Debug.stopMethodTracing();
}
```

##### Pause Activity

<img src="https://developer.android.com/images/training/basics/basic-lifecycle-paused.png" width="700">

Cuando el sistema llama onPause () para la actividad, que técnicamente significa que su actividad es todavía parcialmente visible, pero más a menudo es una indicación de que el usuario abandona la actividad y que pronto entrará en el estado "Stopped". Por lo general, debe utilizar la devolución de llamada onPause ():

+ Detener animaciones u otras acciones en curso que podrían consumir CPU.
+ Confirmar los cambios no guardados, pero sólo si los usuarios esperan que tales cambios se guardarán de forma permanente cuando salen (por ejemplo, un proyecto de e-mail).
+ los recursos del sistema de liberación, tales como broadcast receivers, sensores (como el GPS), o cualquier recurso que pueden afectar la vida de la batería, mientras que su actividad se detuvo y el usuario no los necesita.

Por ejemplo, si su aplicación utiliza la cámara, el método onPause() es un buen lugar para liberarlo.

```java
@Override
public void onPause() {
    super.onPause();  // Siempre llame al método de la superclase primera

    // Liberar la cámara porque no lo necesitamos, cuando se detiene y otras actividades
    // puede ser que necesiten usarla.
    if (mCamera != null) {
        mCamera.release();
        mCamera = null;
    }
}
```
##### Resume Your Activity

When the user resumes your activity from the Paused state, the system calls the onResume() method.

Be aware that the system calls this method every time your activity comes into the foreground, including when it's created for the first time. As such, you should implement onResume() to initialize components that you release during onPause() and perform any other initializations that must occur each time the activity enters the Resumed state (such as begin animations and initialize components only used while the activity has user focus).

The following example of onResume() is the counterpart to the onPause() example above, so it initializes the camera that's released when the activity pauses.

```java
@Override
public void onResume() {
    super.onResume();  // Always call the superclass method first

    // Get the Camera instance as the activity achieves full user focus
    if (mCamera == null) {
        initializeCamera(); // Local method to handle camera init
    }
}
```

#### Stopping y Restarting  Activity

<img src="https://developer.android.com/images/training/basics/basic-lifecycle-stopped.png" width="700">

Adecuadamente Detener y reiniciar su actividad es un proceso importante en el ciclo de vida de la actividad que asegura que sus usuarios perciben que su aplicación es siempre viva y no pierde su progreso. Hay unos pocos de los principales ejemplos en los que se detiene su actividad y se reinicia:

+ El usuario abre la ventana de aplicaciones recientes y cambia de tu aplicación a otra aplicación.
+ La actividad en la aplicación que está actualmente en el primer plano se detiene.
+ Si el usuario vuelve a su aplicación desde el icono del lanzador pantalla de inicio o en la ventana de aplicaciones recientes, se reinicia la actividad.
+ El usuario realiza una acción de la aplicación que se inicia una nueva actividad. La actividad actual se detiene cuando se crea la segunda actividad. Si el usuario pulsa entonces el botón Atrás, se reinicia la primera actividad.
+ El usuario recibe una llamada telefónica durante el uso de la aplicación en su teléfono.

##### Stop Activity

Cuando su actividad recibe una llamada al método onStop(), ya no es visible y debe liberar a casi todos los recursos que no son necesarios mientras que el usuario no lo está utilizando. Una vez que se detuvo su actividad, el sistema podría destruir la instancia si necesita recuperar la memoria del sistema. En casos extremos, el sistema podría simplemente matar a su proceso de aplicación sin llamar OnDestroy(), por lo que es importante que utilice onStop() para liberar recursos que podrían perder memoria.

Aunque el método onPause() es llamado antes de onStop(), debe utilizar onStop() para realizar las operaciones de la CPU, tales como escribir información en una base de datos.

#### Recreando Activity

Hay algunas situaciones en las que su actividad se destruye como consecuencia de un comportamiento normal de aplicaciones, tales como cuando el usuario presiona el botón Atrás o su actividad señala su propia destrucción llamando finish(). El sistema también puede destruir su actividad si está actualmente detenida y no se ha utilizado en mucho tiempo o la actividad de primer plano requiere más recursos de lo que el sistema debe cerrar procesos en segundo plano para recuperar memoria.

	Atención: Su actividad se destruye y vuelve a crear cada vez que el usuario hace girar la pantalla. Cuando la pantalla cambia la orientación, el sistema destruye y recrea la actividad de primer plano debido a que la configuración de la pantalla ha cambiado y que su actividad puede ser que necesite para cargar recursos alternativos (como la distribución de elementos).

Por defecto, el sistema utiliza el Bundle para guardar información acerca de cada objeto View en su diseño de la actividad (por ejemplo, el valor texto introducido en un objeto EditarTexto). Por lo tanto, si su instancia de actividad es destruido y recreado, el estado de la disposición se restaura a su estado anterior sin código requerido tuyo. Sin embargo, su actividad puede tener más información de estado que desea restaurar, como variables de miembros que hacen un seguimiento del progreso del usuario en la actividad.

	Nota: Para que el sistema Android para restaurar el estado de los puntos de vista de su actividad, cada vista debe tener un identificador único, suministrado por el androide: id attribute.

<img src="https://developer.android.com/images/training/basics/basic-lifecycle-savestate.png" width="700">

##### Guardar el Activity State

A medida que su actividad comienza a detenerse, el sistema llama onSaveInstanceState() para que su actividad puede guardar la información de estado con una colección de pares de clave y valor. La implementación por defecto de este método guarda la información sobre el estado de la jerarquía de la vista de la actividad, como el texto de un widget de EditarTexto o la posición de desplazamiento de un ListView.

Para guardar la información de estado adicional para su actividad, debe implementar onSaveInstanceState() y añadir pares clave-valor al objeto Bundle. Por ejemplo:

```java
static final String STATE_SCORE = "playerScore";
static final String STATE_LEVEL = "playerLevel";
...

@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
    // Guardar el estado del juego actual del usuario
    savedInstanceState.putInt(STATE_SCORE, mCurrentScore);
    savedInstanceState.putInt(STATE_LEVEL, mCurrentLevel);

    // Siempre llame a la superclase para que pueda guardar el estado de vista de jerarquía
    super.onSaveInstanceState(savedInstanceState);
}
```

##### Restore Your Activity State

Cuando se vuelve a crear su actividad después de que fuera destruida anteriormente, puede recuperar su estado guardado en el Bundle el cual el sistema pasa a tu actividad. Tanto el onCreate() y onRestoreInstanceState() los métodos reciben el mismo paquete que contiene la información de estado de la instancia.

Debido a que el método onCreate() se llama si el sistema es la creación de una nueva instancia de la actividad o la recreación de una anterior, debe comprobar si el archivo de estado es nulo antes de intentar leerlo. Si es nulo, entonces el sistema es la creación de una nueva instancia de la actividad, en lugar de restaurar una anterior que fue destruida.

Por ejemplo, aquí es cómo puede restaurar algunos datos de estado en onCreate ():

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState); // Siempre llame a la superclase primera

    // Comprobamos si estamos creando una instancia previamente destruida
    if (savedInstanceState != null) {
        // Restaurar el valor de los miembros de un estado guardado
        mCurrentScore = savedInstanceState.getInt(STATE_SCORE);
        mCurrentLevel = savedInstanceState.getInt(STATE_LEVEL);
    } else {
        // Probablemente inicializar los miembros con valores por defecto para una nueva instancia
    }
    ...
}
```

En lugar de restaurar el estado durante onCreate() puede optar por aplicar onRestoreInstanceState(), que el sistema llama después de que el método onStart(). El sistema llama onRestoreInstanceState() sólo si hay un estado guardado para restaurar, por lo que no es necesario comprobar si el paquete es nulo:

```java
public void onRestoreInstanceState(Bundle savedInstanceState) {
    // Siempre llame a la superclase para que pueda restaurar la jerarquía de vistas
    super.onRestoreInstanceState(savedInstanceState);

    // Restaurar los estados miembros de la instancia salvado
    mCurrentScore = savedInstanceState.getInt(STATE_SCORE);
    mCurrentLevel = savedInstanceState.getInt(STATE_LEVEL);
}
```

###[Anterior (Ciclo de vida de una actividad)](/chapter2/topic4.md)
###[Siguiente (El emulador y ejecutar desde el celular)](/chapter2/topic5.md)
