# Modulo 2 - Entorno de trabajo con Android Studio y Android SDK

### Java JDK y Android SDK

El JDK es un subconjunto de lo que se define en términos generales como un kit de desarrollo de software (SDK) en el sentido general. En las descripciones que acompañan a sus versiones más recientes de Java SE, EE y ME, Sun reconoce que en virtud de su terminología, el JDK forma el subconjunto del SDK que se encarga de la redacción y ejecución de programas Java. El resto del SDK se compone de software adicional, tales como servidores de aplicaciones, depuradores, y documentación.

El SDK de Android (kit de desarrollo de software) es un conjunto de herramientas de desarrollo utilizadas para desarrollar aplicaciones para la plataforma Android. El SDK de Android incluye lo siguiente:

- Bibliotecas necesarias.
- Depurador.
- Un emulador.
- La documentación pertinente para las interfaces de programación de aplicaciones de Android (API).
- Código fuente de ejemplo.
- Tutoriales para el sistema operativo Android.

### Instalando y usando Android Studio

[Instalador](https://developer.android.com/studio/install.html)

Android Studio es la herramienta de entorno de desarrollo integrado (IDE) para el desarrollo de aplicaciones para Android, basado en IntelliJ IDEA. En la parte superior de potentes herramientas de edición de código y desarrolladores de IntelliJ, Android Studio ofrece aún más características que mejoran su productividad en la construcción de aplicaciones de Android, tales como:

- Un sistema de construcción basado en Gradle flexibles.
- Un emulador rápido y varias funciones extras.
- Un entorno único en el que se pueden desarrollar para todos los dispositivos Android.
- Ejecución instantánea para actualizar cambios a su aplicación en ejecución sin necesidad de construir un nuevo APK.
- Plantillas de código y la integración GitHub para ayudarle a construir características de la aplicación comunes y código de ejemplo de importación.
- Extensas herramientas de prueba.
- Herramientas para medir rendimiento, facilidad de uso, compatibilidad de versiones, y otros problemas.
- El soporte integrado para Google Cloud Platform, por lo que es fácil de integrar GCM y App Engine.

### Hola mundo - Primera app

1. En Android Studio, crear un nuevo proyecto:
	- Si todavía no has abierto un proyecto, en la pantalla de bienvenida, haga clic en Nuevo proyecto.
	- Si has abierto un proyecto, en el menú Archivo, seleccione Nuevo proyecto. Aparece la pantalla Crear nuevo proyecto.

2. Rellene los campos de la pantalla y haga clic en Next.

	Es más fácil de seguir este capitulo si utiliza los mismos valores como se muestra.
	- **Application Name** es el nombre de la aplicación que aparece a los usuarios.
	- **Company domain** proporciona un calificador que se añadirá al nombre del paquete; Android Studio recordará este valor para cada nuevo proyecto se crea.
	- **Package name** es el nombre completo para el proyecto (siguiendo las mismas reglas que las de denominación de paquetes en el lenguaje de programación Java). Su nombre del paquete debe ser único en todos los paquetes instalados en el sistema Android. Puede editar este valor independientemente del nombre de la aplicación o en el dominio de la empresa.
	- **Project location** es el directorio en el sistema que guardan los archivos de proyecto.

3. En **Select the form factors your app will run on**, selecciona la opción **Phone and Tablet**.

4. Para **Minimum SDK**, selecciona API 15: Android 4.0.3 (IceCreamSandwich).

	El SDK Obligatorio mínimo es la primera versión de Android que sus soportes de aplicaciones, indican usando el nivel de la API. Para soportar el mayor numero de dispositivos que sea posible, debe establecer este a la versión más baja disponible que abarque el mayor numero de usuarios o su mercado deseado. Si alguna de las funciones de su aplicación sólo es posible en las versiones más recientes de Android y no es crítica para el conjunto de características de la aplicación, se puede habilitar la función sólo cuando se ejecuta en las versiones que lo soportan.

5. Dejar las otras opciones (TV, Wear, y Glass) sin seleccionar y click **Next**.

6. En **Add an activity to <template>**, seleccionar **Empty Activity** y click **Next**.

7. Deja las opciones como las sugieren y click en **Next**.

8. Click the **Finish** button to create the project.

### Estructura de proyecto Android

<img src="https://developer.android.com/studio/images/intro/project-android-view_2-1_2x.png" width="300">

Each project in Android Studio contains one or more modules with source code files and resource files. Types of modules include:

Android app modules
Library modules
Google App Engine modules
By default, Android Studio displays your project files in the Android project view, as shown in figure 1. This view is organized by modules to provide quick access to your project's key source files.

All the build files are visible at the top level under Gradle Scripts and each app module contains the following folders:

manifests: Contains the AndroidManifest.xml file.
java: Contains the Java source code files, including JUnit test code.
res: Contains all non-code resources, such as XML layouts, UI strings, and bitmap images.
The Android project structure on disk differs from this flattened representation. To see the actual file structure of the project, select Project from the Project dropdown (in figure 1, it's showing as Android).

You can also customize the view of the project files to focus on specific aspects of your app development. For example, selecting the Problems view of your project displays links to the source files containing any recognized coding and syntax errors, such as a missing XML element closing tag in a layout file.

**app/src/main/res/layout/activity_main.xml**

	This XML layout file is for the activity you added when you created the project with Android Studio. Following the New Project workflow, Android Studio presents this file with both a text view and a preview of the screen UI. The file contains some default interface elements from the material design library, including the app bar and a floating action button. It also includes a separate layout file with the main content.
	This XML layout file resides in activity_my.xml, and contains some settings and a TextView element that displays the message, "Hello world!".

**app/src/main/java/com.....myappname/MainActivity.java**

	A tab for this file appears in Android Studio when the New Project workflow finishes. When you select the file you see the class definition for the activity you created. When you build and run the app, the Activity class starts the activity and loads the layout file that says "Hello World!"

**app/src/main/AndroidManifest.xml**

	The manifest file describes the fundamental characteristics of the app and defines each of its components. You'll revisit this file as you follow these lessons and add more components to your app.

**app/build.gradle**

	Android Studio uses Gradle to compile and build your app. There is a build.gradle file for each module of your project, as well as a build.gradle file for the entire project. Usually, you're only interested in the build.gradle file for the module, in this case the app or application module. This is where your app's build dependencies are set, including the defaultConfig settings:

		- compiledSdkVersion is the platform version against which you will compile your app. By default, this is set to the latest version of Android available in your SDK. (It should be Android 4.1 or greater; if you don't have such a version available, you must install one using the SDK Manager.) You can still build your app to support older versions, but setting this to the latest version allows you to enable new features and optimize your app for a great user experience on the latest devices.

		- applicationId is the fully qualified package name for your application that you specified during the New Project workflow.

		- minSdkVersion is the Minimum SDK version you specified during the New Project workflow. This is the earliest version of the Android SDK that your app supports.

		- targetSdkVersion indicates the highest version of Android with which you have tested your application. As new versions of Android become available, you should test your app on the new version and update this value to match the latest API level and thereby take advantage of new platform features. For more information, read Supporting Different Platform Versions.

**drawable-< density >/**

	Directories for drawable resources, other than launcher icons, designed for various densities.

**layout/**

	Directory for files that define your app's user interface like activity_my.xml, discussed above, which describes a basic layout for the MyActivity class.

**menu/**

	Directory for files that define your app's menu items.

**mipmap/**

	Launcher icons reside in the mipmap/ folder rather than the drawable/ folders. This folder contains the ic_launcher.png image that appears when you run the default app.

**values/**

	Directory for other XML files that contain a collection of resources, such as string and color definitions.

#### The User Interface

<img src="https://developer.android.com/studio/images/intro/main-window_2-1_2x.png" width="700">

1. The toolbar lets you carry out a wide range of actions, including running your app and launching Android tools.
2. The navigation bar helps you navigate through your project and open files for editing. It provides a more compact view of the structure visible in the Project tool window.
3. The editor window is where you create and modify code. Depending on the current file type, this window can change. For example, when viewing a layout file, the editor window displays the layout editor and offers the option to view the corresponding XML file.
4. Tool windows give you access to specific tasks like project management, search, version control, and more. You can expand them and collapse them.
5. The status bar displays the status of your project and the IDE itself, as well as any warnings or messages.

	You can organize the main window to give yourself more screen space by hiding or moving toolbars and tool windows. You can also use keyboard shortcuts to access most IDE features.

	At any time, you can search across your source code, databases, actions, elements of the user interface, and so on, by double-pressing the Shift key, or clicking the magnifying glass in the upper right-hand corner of the Android Studio window. This can be very useful if, for example, you are trying to locate a particular IDE action that you have forgotten how to trigger.

#### The Gradle Settings File

The gradle.settings file, located in the root project directory, tells Gradle which modules it should include when building your app. For most projects, the file is simple and only includes the following:

	include ‘:app’

However, multi-module projects need to specify each module that should go into the final build.

#### The Top-level Build File

The top-level build.gradle file, located in the root project directory, defines build configurations that apply to all modules in your project. By default, the top-level build file uses the buildscript {} block to define the Gradle repositories and dependencies that are common to all modules in the project. The following code sample describes the default settings and DSL elements you can find in the top-level build.gradle after creating a new project.

	/**
	 * The buildscript {} block is where you configure the repositories and
	 * dependencies for Gradle itself--meaning, you should not include dependencies
	 * for your modules here. For example, this block includes the Android plugin for
	 * Gradle as a dependency because it provides the additional instructions Gradle
	 * needs to build Android app modules.
	 */

	buildscript {

	    /**
	     * The repositories {} block configures the repositories Gradle uses to
	     * search or download the dependencies. Gradle pre-configures support for remote
	     * repositories such as JCenter, Maven Central, and Ivy. You can also use local
	     * repositories or define your own remote repositories. The code below defines
	     * JCenter as the repository Gradle should use to look for its dependencies.
	     */

	    repositories {
	        jcenter()
	    }

	    /**
	     * The dependencies {} block configures the dependencies Gradle needs to use
	     * to build your project. The following line adds Android Plugin for Gradle
	     * version 2.0.0 as a classpath dependency.
	     */

	    dependencies {
	        classpath 'com.android.tools.build:gradle:2.0.0'
	    }
	}

	/**
	 * The allprojects {} block is where you configure the repositories and
	 * dependencies used by all modules in your project, such as third-party plugins
	 * or libraries. Dependencies that are not required by all the modules in the
	 * project should be configured in module-level build.gradle files. For new
	 * projects, Android Studio configures JCenter as the default repository, but it
	 * does not configure any dependencies.
	 */

	allprojects {
	   repositories {
	       jcenter()
	   }
	}

#### The Module-level Build File

The module-level build.gradle file, located in each <project>/<module>/ directory, allows you to configure build settings for the specific module it is located in. Configuring these build settings allows you to provide custom packaging options, such as additional build types and product flavors, and override settings in the main/ app manifest or top-level build.gradle file.

This sample Android app module build.gradle file outlines some of the basic DSL elements and settings that you should know.

	/**
	 * The first line in the build configuration applies the Android plugin for
	 * Gradle to this build and makes the android {} block available to specify
	 * Android-specific build options.
	 */

	apply plugin: 'com.android.application'

	/**
	 * The android {} block is where you configure all your Android-specific
	 * build options.
	 */

	android {

	  /**
	   * compileSdkVersion specifies the Android API level Gradle should use to
	   * compile your app. This means your app can use the API features included in
	   * this API level and lower.
	   *
	   * buildToolsVersion specifies the version of the SDK build tools, command-line
	   * utilities, and compiler that Gradle should use to build your app. You need to
	   * download the build tools using the SDK Manager.
	   */

	  compileSdkVersion 23
	  buildToolsVersion "23.0.3"

	  /**
	   * The defaultConfig {} block encapsulates default settings and entries for all
	   * build variants, and can override some attributes in main/AndroidManifest.xml
	   * dynamically from the build system. You can configure product flavors to override
	   * these values for different versions of your app.
	   */

	  defaultConfig {

	    /**
	     * applicationId uniquely identifies the package for publishing.
	     * However, your source code should still reference the package name
	     * defined by the package attribute in the main/AndroidManifest.xml file.
	     */

	    applicationId 'com.example.myapp'

	    // Defines the minimum API level required to run the app.
	    minSdkVersion 14

	    // Specifies the API level used to test the app.
	    targetSdkVersion 23

	    // Defines the version number of your app.
	    versionCode 1

	    // Defines a user-friendly version name for your app.
	    versionName "1.0"
	  }

	  /**
	   * The buildTypes {} block is where you can configure multiple build types.
	   * By default, the build system defines two build types: debug and release. The
	   * debug build type is not explicitly shown in the default build configuration,
	   * but it includes debugging tools and is signed with the debug key. The release
	   * build type applies Proguard settings and is not signed by default.
	   */

	  buildTypes {

	    /**
	     * By default, Android Studio configures the release build type to enable code
	     * shrinking, using minifyEnabled, and specifies the Proguard settings file.
	     */

	    release {
	        minifyEnabled true // Enables code shrinking for the release build type.
	        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
	    }
	  }

	  /**
	   * The productFlavors {} block is where you can configure multiple product
	   * flavors. This allows you to create different versions of your app that can
	   * override defaultConfig {} with their own settings. Product flavors are
	   * optional, and the build system does not create them by default. This example
	   * creates a free and paid product flavor. Each product flavor then specifies
	   * its own application ID, so that they can exist on the Google Play Store, or
	   * an Android device, simultaneously.
	   */

	  productFlavors {
	    free {
	      applicationId 'com.example.myapp.free'
	    }

	    paid {
	      applicationId 'com.example.myapp.paid'
	    }
	  }
	}

	/**
	 * The dependencies {} block in the module-level build configuration file
	 * only specifies dependencies required to build the module itself.
	 */

	dependencies {
	    compile project(":lib")
	    compile 'com.android.support:appcompat-v7:22.0.1'
	    compile fileTree(dir: 'libs', include: ['*.jar'])
	}

#### FILES

gradle.properties

	This is where you can configure project-wide Gradle settings, such as the Gradle daemon's maximum heap size. For more information, see Configuring the build environment.

local.properties

	Configures local environment properties for the build system, such as the path to the SDK installation. Because the content of this file is automatically generated by Android Studio and is specific to the local developer environment, you should not modify this file manually or check it into your version control system.

src/main/

	This source set includes code and resources common to all build variants.

src/< buildType >/

	Create this source set to include code and resources only for a specific build type.


### Ciclo de vida de una actividad

<img src="https://developer.android.com/images/training/basics/basic-lifecycle.png" width="700">

**onCreate:** Se llama en la creación de la actividad. Se utiliza para realizar todo tipo de inicializaciones, como la creación de la interfaz de usuario o la inicialización de estructuras de datos. Puede recibir información de estado dela actividad (en una instancia de la clase Bundle), por si se reanuda desde una actividad que ha sido destruida y vuelta a crear.

**onStart:** Nos indica que la actividad está a punto de ser mostrada al usuario.

**onResume:** Se llama cuando la actividad va a comenzar a interactuar con el usuario. Es un buen lugar para lanzar las animaciones y la música.

**onPause:** Indica que la actividad está a punto de ser lanzada a segundo plano, normalmente porque otra actividad es lanzada. Es el lugar adecuado para detener animaciones, música o almacenar los datos que estaban en edición.

**onStop:** La actividad ya no va a ser visible para el usuario. Ojo si hay muy poca memoria, es posible que la actividad se destruya sin llamar a este método.

**onRestart:** Indica que la actividad va a volver a ser representada después de haber pasado por onStop().

**onDestroy:** Se llama antes de que la actividad sea totalmente destruida. Por ejemplo, cuando el usuario pulsa el botón de volver o cuando se llama al método finish(). Ojo si hay muy poca memoria, es posible que la actividad se destruya sin llamar a este método.

##### Destroy the Activity

While the activity's first lifecycle callback is onCreate(), its very last callback is onDestroy(). The system calls this method on your activity as the final signal that your activity instance is being completely removed from the system memory.

Most apps don't need to implement this method because local class references are destroyed with the activity and your activity should perform most cleanup during onPause() and onStop(). However, if your activity includes background threads that you created during onCreate() or other long-running resources that could potentially leak memory if not properly closed, you should kill them during onDestroy().

```java
@Override
public void onDestroy() {
    super.onDestroy();  // Always call the superclass

    // Stop method tracing that the activity started during onCreate()
    android.os.Debug.stopMethodTracing();
}
```

##### Pause Your Activity

<img src="https://developer.android.com/images/training/basics/basic-lifecycle-paused.png" width="700">

When the system calls onPause() for your activity, it technically means your activity is still partially visible, but most often is an indication that the user is leaving the activity and it will soon enter the Stopped state. You should usually use the onPause() callback to:

Stop animations or other ongoing actions that could consume CPU.
Commit unsaved changes, but only if users expect such changes to be permanently saved when they leave (such as a draft email).
Release system resources, such as broadcast receivers, handles to sensors (like GPS), or any resources that may affect battery life while your activity is paused and the user does not need them.

For example, if your application uses the Camera, the onPause() method is a good place to release it.

```java
@Override
public void onPause() {
    super.onPause();  // Always call the superclass method first

    // Release the Camera because we don't need it when paused
    // and other activities might need to use it.
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

#### Stopping and Restarting an Activity

<img src="https://developer.android.com/images/training/basics/basic-lifecycle-stopped.png" width="700">

Properly stopping and restarting your activity is an important process in the activity lifecycle that ensures your users perceive that your app is always alive and doesn't lose their progress. There are a few of key scenarios in which your activity is stopped and restarted:

The user opens the Recent Apps window and switches from your app to another app. The activity in your app that's currently in the foreground is stopped. If the user returns to your app from the Home screen launcher icon or the Recent Apps window, the activity restarts.
The user performs an action in your app that starts a new activity. The current activity is stopped when the second activity is created. If the user then presses the Back button, the first activity is restarted.
The user receives a phone call while using your app on his or her phone.

##### Stop Your Activity

When your activity receives a call to the onStop() method, it's no longer visible and should release almost all resources that aren't needed while the user is not using it. Once your activity is stopped, the system might destroy the instance if it needs to recover system memory. In extreme cases, the system might simply kill your app process without calling the activity's final onDestroy() callback, so it's important you use onStop() to release resources that might leak memory.

Although the onPause() method is called before onStop(), you should use onStop() to perform larger, more CPU intensive shut-down operations, such as writing information to a database.

#### Recreating an Activity

There are a few scenarios in which your activity is destroyed due to normal app behavior, such as when the user presses the Back button or your activity signals its own destruction by calling finish(). The system may also destroy your activity if it's currently stopped and hasn't been used in a long time or the foreground activity requires more resources so the system must shut down background processes to recover memory.

	Caution: Your activity will be destroyed and recreated each time the user rotates the screen. When the screen changes orientation, the system destroys and recreates the foreground activity because the screen configuration has changed and your activity might need to load alternative resources (such as the layout).

By default, the system uses the Bundle instance state to save information about each View object in your activity layout (such as the text value entered into an EditText object). So, if your activity instance is destroyed and recreated, the state of the layout is restored to its previous state with no code required by you. However, your activity might have more state information that you'd like to restore, such as member variables that track the user's progress in the activity.

	Note: In order for the Android system to restore the state of the views in your activity, each view must have a unique ID, supplied by the android:id attribute.

<img src="https://developer.android.com/images/training/basics/basic-lifecycle-savestate.png" width="700">

##### Save Your Activity State

As your activity begins to stop, the system calls onSaveInstanceState() so your activity can save state information with a collection of key-value pairs. The default implementation of this method saves information about the state of the activity's view hierarchy, such as the text in an EditText widget or the scroll position of a ListView.

To save additional state information for your activity, you must implement onSaveInstanceState() and add key-value pairs to the Bundle object. For example:

```java
static final String STATE_SCORE = "playerScore";
static final String STATE_LEVEL = "playerLevel";
...

@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
    // Save the user's current game state
    savedInstanceState.putInt(STATE_SCORE, mCurrentScore);
    savedInstanceState.putInt(STATE_LEVEL, mCurrentLevel);

    // Always call the superclass so it can save the view hierarchy state
    super.onSaveInstanceState(savedInstanceState);
}
```

##### Restore Your Activity State

When your activity is recreated after it was previously destroyed, you can recover your saved state from the Bundle that the system passes your activity. Both the onCreate() and onRestoreInstanceState() callback methods receive the same Bundle that contains the instance state information.

Because the onCreate() method is called whether the system is creating a new instance of your activity or recreating a previous one, you must check whether the state Bundle is null before you attempt to read it. If it is null, then the system is creating a new instance of the activity, instead of restoring a previous one that was destroyed.

For example, here's how you can restore some state data in onCreate():

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState); // Always call the superclass first

    // Check whether we're recreating a previously destroyed instance
    if (savedInstanceState != null) {
        // Restore value of members from saved state
        mCurrentScore = savedInstanceState.getInt(STATE_SCORE);
        mCurrentLevel = savedInstanceState.getInt(STATE_LEVEL);
    } else {
        // Probably initialize members with default values for a new instance
    }
    ...
}
```

Instead of restoring the state during onCreate() you may choose to implement onRestoreInstanceState(), which the system calls after the onStart() method. The system calls onRestoreInstanceState() only if there is a saved state to restore, so you do not need to check whether the Bundle is null:

```java
public void onRestoreInstanceState(Bundle savedInstanceState) {
    // Always call the superclass so it can restore the view hierarchy
    super.onRestoreInstanceState(savedInstanceState);

    // Restore state members from saved instance
    mCurrentScore = savedInstanceState.getInt(STATE_SCORE);
    mCurrentLevel = savedInstanceState.getInt(STATE_LEVEL);
}
```

### Ejercicio 1
¿Cuándo se llama a los eventos del ciclo de vida en una actividad?

En este ejercicio vamos a implementar todos los métodos del ciclo de vida de la actividad MainActivity y añadiremos un toast para mostrar cuando se ejecuta. De esta forma comprenderemos mejor cuando se llama a cada método.

1. Abre la actividad MainActivity del proyecto Asteroides o Mis Lugares.
2. Copia el codigo que se encuentra aqui: [Codigo de ejemplo](/chapter2/MainActivity.java)
3. Ejecuta la aplicación y observa la secuencia de Toast.
4. Selecciona la opción cambiar de aplicacion y luego regresa a la actividad. Observa la secuencia de Toast.
5. Sal de la actividad y observa la secuencia de Toast.

### El emulador y ejecutar desde el celular

#### Run on a Real Device

##### Set up your device

1. Plug in your device to your development machine with a USB cable. If you're developing on Windows, you might need to install the appropriate USB driver for your device. For help installing drivers, see the OEM USB Drivers document.

2. Enable USB debugging on your device. On Android 4.0 and newer, go to Settings > Developer options.
```
Note: On Android 4.2 and newer, Developer options is hidden by default. To make it available, go to Settings > About phone and tap Build number seven times. Return to the previous screen to find Developer options.
```

##### Run the app from Android Studio

1. Select one of your project's files and click Run  from the toolbar.

2. In the Choose Device window that appears, select the Choose a running device radio button, select your device, and click OK .

Android Studio installs the app on your connected device and starts it.



#### Run on the Emulator

Whether you're using Android Studio or the command line, to run your app on the emulator you need to first create an Android Virtual Device (AVD). An AVD is a device configuration for the Android emulator that allows you to model a specific device.

##### Create an AVD

1. Launch the Android Virtual Device Manager:
	- In Android Studio, select Tools > Android > AVD Manager, or click the AVD Manager icon  in the toolbar. The AVD Manager screen appears.
	- Or, from the command line, change directories to sdk/ and execute:
	```
	tools/android avd
	```

	Note: The AVD Manager that appears when launched from the command line is different from the version in Android Studio, so the following instructions may not all apply.

2. On the AVD Manager main screen, click Create Virtual Device.
3. In the Select Hardware window, select a device configuration, such as Nexus 6, then click Next.
4. Select the desired system version for the AVD and click Next.
5. Verify the configuration settings, then click Finish.

For more information about using AVDs, see [Managing AVDs with AVD Manager](https://developer.android.com/studio/run/managing-avds.html)

##### Run the app from Android Studio
1. In Android Studio, select your project and click Run   from the toolbar.
2. In the Choose Device window, click the Launch emulator radio button.
3. From the Android virtual device pull-down menu, select the emulator you created, and click OK.

It can take a few minutes for the emulator to load itself. You may have to unlock the screen. When you do, My First App appears on the emulator screen.

That's how you build and run your Android app on the emulator! To start developing, continue to the next lesson.



