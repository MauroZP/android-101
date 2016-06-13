### Estructura de proyecto Android

<img src="https://developer.android.com/studio/images/intro/project-android-view_2-1_2x.png" width="300">

Cada proyecto en Android Studio contiene uno o más módulos con archivos de código fuente y archivos de recursos. Tipos de módulos incluyen:

- Módulos de aplicaciones para Android.
- Módulos de biblioteca.
- Módulos de Google App Engine.

De forma predeterminada, Android Studio muestra los archivos del proyecto en la vista del proyecto Android, como se muestra en la imagen anterior. Este punto de vista está organizado por módulos para proporcionar un acceso rápido a los archivos de fuentes esenciales de su proyecto.

Todos los archivos construidos son visibles en el nivel superior bajo Scripts Gradle y cada módulo de aplicación contiene las siguientes carpetas:

**manifests:** Contiene el archivo AndroidManifest.xml.

**java:** Contiene los archivos de código fuente de Java, incluyendo el código de prueba JUnit.

**res:** Contiene todos los recursos de no código, tales como diseños de XML, string de la IU, y las imágenes.

La estructura del proyecto Android en el disco se diferencia de esta representación aplanada. Para ver la estructura de archivos real del proyecto, seleccione Proyecto en el menú desplegable del proyecto.

También puede personalizar la vista de los archivos de proyecto para centrarse en aspectos específicos de su desarrollo de aplicaciones. Por ejemplo, la selección de la vista Problemas de su proyecto muestra enlaces a los archivos de origen que contengan cualquier error de sintaxis de codificación y reconocidos, como un elemento XML que falta la etiqueta de cierre en un archivo de diseño.

#### Los archivos generados para el proyecto

**app/src/main/res/layout/activity_main.xml**

	Este archivo XML es el diseño de la actividad que ha añadido al crear el proyecto con Android Studio. Siguiendo el flujo de trabajo Nuevo proyecto, Android Studio presenta este archivo tanto con una vista de texto y una vista previa de la interfaz de usuario de pantalla. El archivo contiene algunos ajustes y un elemento de Vista de Texto que muestra el mensaje "Hola mundo!".

**app/src/main/java/com.....myappname/MainActivity.java**

	Una pestaña de este archivo aparece en Android Studio cuando el flujo de trabajo Nuevo proyecto termina. Cuando se selecciona el archivo que aparece la definición de clase para la actividad que ha creado. Cuando se construye y ejecuta la aplicación, el tipo de actividad se inicia la actividad y carga el archivo de diseño que dice "Hello World!"

**app/src/main/AndroidManifest.xml**

	El archivo de manifiesto describe las características fundamentales de la aplicación y define cada uno de sus componentes. Vas a volver a este archivo como sigue estas lecciones y añadir más componentes de la aplicación.

**app/build.gradle**

	Android Studio utiliza Gradle para compilar y construir su aplicación. Hay un archivo build.gradle para cada módulo de su proyecto, así como un archivo build.gradle para todo el proyecto. Por lo general, sólo está interesado en el archivo build.gradle para el módulo, en este caso la aplicación o módulo de aplicación. Aquí es donde se encuentran las dependencias de construcción de tu aplicación, incluyendo la configuración defaultConfig:

- **CompiledSdkVersion** es la versión de la plataforma contra la que va a compilar su aplicación. De forma predeterminada, esto se establece en la última versión de Android disponible en el SDK. (Debe ser Android 4.1 o superior; si no tiene una versión de dicha disposición, debe instalar uno utilizando el SDK Manager.) Todavía se puede construir su aplicación para apoyar a las versiones anteriores, pero este valor está a la última versión que permite para habilitar nuevas características y optimizar su aplicación para una gran experiencia de usuario en los dispositivos más recientes.

- **applicationId** es el nombre de paquete completo para su aplicación que ha especificado durante el flujo de trabajo en Nuevo proyecto.

- **minSdkVersion** es la versión del SDK mínimo especificado durante el flujo de trabajo en Nuevo proyecto. Esta es la primera versión del SDK de Android que soporta la aplicación.

- **targetSdkVersion** indica la versión más alta de Android con la que ha probado su aplicación. A medida que las nuevas versiones de Android estén disponibles, se debería probar la aplicación en la nueva versión y actualizar este valor para que coincida con el último nivel de la API y de esta manera aprovechar las nuevas características de la plataforma. Para obtener más información, lea soportar a las diferentes versiones de la plataforma.

**drawable-< density >/**

	Directorios de recursos imagenes y otros, distintos de los iconos de la app, diseñados para varias densidades.

**layout/**

	Directorio para los archivos que definen la interfaz de usuario de la aplicación como activity_main.xml, discutido anteriormente, que describe un diseño básico para la clase de actividad.

**menu/**

	Directorio para los archivos que definen elementos de menú de la aplicación.

**mipmap/**

	Los iconos Lanzadores residen en la carpeta mipmap/ en lugar de la carpeta drawable/. Esta carpeta contiene la imagen ic_launcher.png que aparece cuando se ejecuta la aplicación por defecto.

**values/**

	Directorio para otros archivos XML que contienen una colección de recursos, como los textos y colores.

#### Interfaz de usuario

<img src="https://developer.android.com/studio/images/intro/main-window_2-1_2x.png" width="700">

1. La barra de herramientas le permite llevar a cabo una amplia gama de acciones, incluyendo la ejecución de la aplicación y el lanzamiento de herramientas de Android.

2. La barra de navegación le ayuda a navegar a través de su proyecto y los archivos abiertos para su edición. Proporciona una visión más compacta de la estructura visible en la ventana de herramientas del proyecto.

3. La ventana del editor es donde puede crear y modificar el código. Dependiendo del tipo de archivo actual, esta ventana puede cambiar. Por ejemplo, al ver un archivo de diseño, la ventana de edición muestra el editor de diseño y ofrece la opción de ver el archivo XML correspondiente.

4. Ventanas de herramientas le dan acceso a tareas específicas como gestión de proyectos, búsqueda, control de versiones, y mucho más. Puede expandirlos y contraerlos.

5. La barra de estado muestra el estado de su proyecto y el propio IDE, así como mensajes o avisos.

#### El archivo Gradle Settings

El archivo gradle.settings, ubicado en el directorio raíz del proyecto, dice Gradle los módulos que se debe incluir en la construcción de su aplicación. Para la mayoría de los proyectos, el archivo es simple y sólo incluye lo siguiente:

	include ‘:app’

Sin embargo, los proyectos de varios módulos tienen que especificar cada módulo que debe ir a la versión final.

#### The Top-level Build File

El archivo build.gradle de nivel superior, ubicado en el directorio raíz del proyecto, define crear configuraciones que se aplican a todos los módulos en su proyecto. Por defecto, el archivo de generación de nivel superior utiliza el bloque buildscript {} para definir los repositorios y las dependencias que son comunes a todos los módulos del proyecto Gradle. El ejemplo de código siguiente describe la configuración predeterminada y elementos de DSL se pueden encontrar en el build.gradle de nivel superior después de crear un nuevo proyecto.

	/**
	 * El bloque buildscript {} es donde se configuran los repositorios y dependencias para Gradle,
	 * no debes incluir las dependencias de los módulos de aquí. Por ejemplo, este bloque incluye
	 * el plugin para Android Gradle como una dependencia, ya que proporciona las instrucciones
	 * adicionales Gradle necesita para construir módulos de aplicaciones para Android.
	 */

	buildscript {

	    /**
	     * El bloque de repositorios {} configura los repositorios Gradle utiliza para buscar o descargar
	     * las dependencias. Gradle pre-configura apoyo a repositorios remotos, como JCenter, Maven Central,
	     * u Ivy. También puede utilizar los repositorios locales o definir sus propios repositorios remotos.
	     * El siguiente código define JCentro como repositorio Gradle debe utilizar para buscar sus dependencias.
	     */

	    repositories {
	        jcenter()
	    }

	    /**
	     * El bloque de dependencias {} configura las dependencias Gradle tiene que utilizar para construir su
	     * proyecto. La siguiente línea añade Android Plugin para Gradle versión 2.1.2 como una dependencia de la * ruta de clases.
	     */

	    dependencies {
	        classpath 'com.android.tools.build:gradle:2.1.2'
	    }
	}

	/**
	 * El bloque allprojects {} es donde se configuran los repositorios y las dependencias utilizadas por todos
	 * los módulos del proyecto, como complementos de terceros o bibliotecas. Dependencias que no son requeridos
	 * por todos los módulos en el proyecto deben ser configurados en archivos build.gradle de nivel de módulo.
	 * Para los nuevos proyectos, Android Studio configura JCentro como repositorio por defecto, pero no configura
	 * ninguna dependencia.
	 */

	allprojects {
	   repositories {
	       jcenter()
	   }
	}

#### El archivo Module-level Build

El archivo build.gradle de nivel de módulo, que se encuentra en cada < proyecto > / < módulo > / directorio, le permite configurar los valores de creación específicos de los módulos que se encuentra en cada uno. La configuración de estos ajustes se te permiten proporcionar opciones de embalaje a medida, tales como adicionales tipos de construcción y de productos y configuración sobreescribir en el archivo main/ app manifest o de nivel superior build.gradle.

Este archivo build.gradle módulo de aplicación para Android es un ejemplo donde se describen algunos de los elementos básicos de DSL y ajustes que usted debe saber.

	/**
	 * La primera línea en la configuración de generación se aplica el plugin para Android Gradle
	 * a esta construcción y hace que el android {} bloque este disponible para especificar las opciones
	 * de compilación de Android.
	 */

	apply plugin: 'com.android.application'

	/**
	 * El bloque android {} es donde se configuran todos las especificaciones de construccion de Android.
	 */

	android {

	  /**
	   * compileSdkVersion especifica el nivel de la API de Android Gradle debe utilizar para compilar su
	   * aplicación. Esto significa que su aplicación puede utilizar las funciones de la API se incluyen en este
	   * nivel de la API e inferior.
	   *
	   * buildToolsVersion especifica la versión de las herramientas SDK de construcción, servicios de línea de
	   * comandos, y compilador que Gradle debe utilizar para construir su aplicación. Es necesario descargar las
	   * herramientas de construcción utilizando el SDK Manager.
	   */

	  compileSdkVersion 23
	  buildToolsVersion "23.0.3"

	  /**
	   * El bloque defaultConfig {} encapsula configuración predeterminada y entradas para todas las variantes de
	   * construcción, y puede anular algunos atributos en AndroidManifest.xml dinámica del sistema de construcción. Puede configurar productos para anular estos valores para diferentes versiones de su aplicación.
	   */

	  defaultConfig {

	    /**
	     * applicationId identifica de forma exclusiva el paquete para su publicación. Sin embargo, su código
	     * fuente todavía deberá hacer referencia al nombre del paquete definido por el atributo de paquete en el
	     * AndroidManifest.xml.
	     */

	    applicationId 'com.example.myapp'

	    // Define el nivel mínimo de la API necesaria para ejecutar la aplicación.
	    minSdkVersion 14

	    // Especifica el nivel de API utilizado para probar la aplicación.
	    targetSdkVersion 23

	    // Define el número de versión de la aplicación.
	    versionCode 1

	    // Define un nombre de versión mas amigable de usar para la aplicación.
	    versionName "1.0"
	  }

	  /**
	   * El bloque buildTypes {} es donde se pueden configurar varios tipos de construcción. Por defecto, el
	   * sistema de construcción se definen dos tipos de construcción: debug y release. El tipo de generación de
	   * debug no se muestra en la configuración por defecto, sino que incluye herramientas de debug y se
	   * firma con la clave de debug. El tipo de construcción de la release se aplica la configuración de Proguard
	   * y no está firmado por defecto.
	   */

	  buildTypes {

	    /**
	     * De forma predeterminada, Android Studio configura el tipo de versión de release para permitir la
	     * contracción de código, usando minifyEnabled, especificado el archivo de configuración Proguard.
	     */

	    release {
	        minifyEnabled true // Permite la reducción de código para el tipo de construcción de la release.
	        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
	    }
	  }

	  /**
	   * El bloque productFlavors {} es donde puede configurar múltiples sabores de productos. Esto le permite
	   * crear diferentes versiones de su aplicación que se pueden sobreescribir defaultConfig {} con su propia
	   * configuración. Los sabores de productos son opcionales, y el sistema de construcción no los crea de forma
	   * predeterminada. En este ejemplo se crea un sabor de productos gratuitos y de pago. Cada sabor del
	   * producto a continuación, especifica su propio ID de aplicación, de manera que puedan existir en la tienda
	   * de Google Play, o un dispositivo Android, de forma simultánea.
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
	 * El bloque de dependencias {} en el archivo de configuración de nivel de módulo sólo
	 * especifica las dependencias requeridas para construir el propio módulo.
	 */

	dependencies {
	    compile project(":lib")
	    compile 'com.android.support:appcompat-v7:22.0.1'
	    compile fileTree(dir: 'libs', include: ['*.jar'])
	}

#### Otros archivos

gradle.properties

	Aquí es donde puede configurar los ajustes Gradle de todo el proyecto, tales como el tamaño máximo de almacenamiento dinámico Gradle demonio.

local.properties

	Configura las propiedades de entorno local para el sistema de construcción, tales como la ruta de acceso a la instalación del SDK. Debido a que el contenido de este archivo es generado automáticamente por Android Studio y es específico para el entorno de desarrollo local, no debería modificar este archivo manualmente o incluirlo que en su sistema de control de versiones.

src/main/

	Este conjunto incluye el código fuente y los recursos comunes a todas las variantes de construcción.

src/< buildType >/

	Cree este directorio para incluir el código y los recursos sólo para un tipo específico de construcción.


###[Anterior (Estructura de proyecto Android)](/chapter2/topic3.md)
###[Siguiente (Ciclo de vida de una actividad)](/chapter2/topic4.md)