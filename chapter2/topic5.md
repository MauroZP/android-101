### El emulador y ejecutar desde el celular

#### Ejecutar en un dispositivo real

##### Configura tu celular

1. Conectar el dispositivo a su equipo de desarrollo con un cable USB. Si está desarrollando en Windows, puede que tenga que instalar el controlador USB adecuado para su dispositivo. Para ayudar a los conductores de la instalación, consulte el documento de los controladores USB OEM.

2. Habilitar la depuración USB en el dispositivo. En Android 4.0 y más reciente, vaya a Configuración > Opciones de desarrollador.

```
Nota: En Android 4.2 y versiones posteriores, Opciones de desarrollador se oculta de forma predeterminada. Para
que esté disponible, vaya a Ajustes> Acerca del teléfono y toque el número de compilación siete veces. Volver a la
pantalla anterior para encontrar opciones de desarrollador.
```

##### Ejecutar la app desde Android Studio

1. Seleccione uno de los archivos de su proyecto y haga clic en Ejecutar en la barra de herramientas.

2. En la ventana Seleccionar dispositivo que aparece, seleccione el botón de radio Elija un dispositivo que ejecuta, seleccione el dispositivo y haga clic en OK.

Android Studio instala la aplicación en el dispositivo conectado y la inicia.


#### Ejecutar en el Emulador on the Emulator

Ya sea que esté usando Android Studio o la línea de comandos, para ejecutar la aplicación en el emulador es necesario crear primero un dispositivo virtual de Android (AVD). AVD es una configuración de dispositivo para el emulador de Android que permite modelar un dispositivo específico.

##### Crear un AVD

1. Iniciar el Administrador de dispositivos virtuales de Android (AVD Manager):
	- En Android Studio, seleccione Tools > Android > AVD Manager, o haga clic en el icono de AVD Manager en la barra de herramientas. Aparece la pantalla del Administrador de AVD.
	- O bien, desde la línea de comandos, cambie al directorio SDK / y ejecutar:

```
tools/android avd
```

	Nota: El Administrador de AVD que aparece cuando se inicia desde la línea de comandos
	es diferente de la versión de Android de estudio, por lo que las siguientes instrucciones
	pueden no aplicar.

2. En la pantalla principal AVD Manager, haga clic en **Create Virtual Device**.

3. En la ventana Selección de dispositivos, seleccione una configuración de dispositivo, como Nexus 6, a continuación, haga clic en **Next**.

4. Seleccione la versión del sistema deseado para la AVD y haga clic en **Next**.

5. Verificar los ajustes de configuración, a continuación, haga clic en **Finish**.

Para obtener más información sobre el uso AVDs, consulte [Managing AVDs with AVD Manager](https://developer.android.com/studio/run/managing-avds.html)

##### Ejecutar la app desde Android Studio

1. En Android Studio, seleccione el proyecto y haga clic en Ejecutar en la barra de herramientas.

2. En la ventana Seleccionar dispositivo, haga clic en el botón de lanzamiento del emulador.

3. En el menú desplegable de dispositivo virtual Android, seleccionar el emulador que ha creado y haga clic en OK.

Puede tomar unos minutos para que el emulador para cargar en sí. Puede que tenga que desbloquear la pantalla. Cuando lo haga, mi primera aplicación aparece en la pantalla del emulador.

###[Anterior (El emulador y ejecutar desde el celular)](/chapter2/topic5.md)
###[Siguiente (Ejercicio 1)](/chapter2/topic6.md)
