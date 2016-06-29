### Fragments

Cuando empezaron a aparecer dispositivos de gran tamaño tipo tablet, el equipo de Android tuvo que solucionar el problema de la adaptación de la interfaz gráfica de las aplicaciones a ese nuevo tipo de pantallas. Una interfaz de usuario diseñada para un teléfono móvil no se adaptaba fácilmente a una pantalla varias pulgadas mayor. La solución a esto vino en forma de un nuevo tipo de componente llamado Fragment.

Un fragment no puede considerarse ni un control ni un contenedor, aunque se parecería más a lo segundo. Un fragment podría definirse como una porción de la interfaz de usuario que puede añadirse o eliminarse de la interfaz de forma independiente al resto de elementos de la actividad, y que por supuesto puede reutilizarse en otras actividades. Esto, aunque en principio puede parecer algo trivial, nos va a permitir poder dividir nuestra interfaz en varias porciones de forma que podamos diseñar diversas configuraciones de pantalla, dependiendo de su tamaño y orientación, sin tener que duplicar código en ningún momento, sino tan sólo utilizando o no los distintos fragmentos para cada una de las posibles configuraciones. Intentemos aclarar esto un poco con un ejemplo.

A modo de ejemplo, nosotros vamos a simular la aplicación de correo que hemos comentado antes, adaptándola a tres configuraciones distintas: pantalla “normal” (p.e. un teléfono), pantalla grande horizontal (p.e. tablet) y pantalla grande vertical (p.e. tablet). Para el primer caso colocaremos el listado de correos en una actividad y el detalle en otra, mientras que para el segundo y el tercero ambos elementos estarán en la misma actividad, a derecha/izquierda para el caso horizontal, y arriba/abajo en el caso vertical.

Definiremos por tanto dos fragments: uno para el listado y otro para la vista de detalle. Ambos serán muy sencillos. Al igual que una actividad, cada fragment se compondrá de un fichero de layout XML para la interfaz (colocado en alguna carpeta /res/layout) y una clase java para la lógica asociada.

El primero de los fragment a definir contendrá tan sólo un control ListView, para el que definiremos un adaptador personalizado para mostrar dos campos por fila (“De” y “Asunto”). Ya describimos cómo hacer esto en el artículo dedicado al control ListView. El layout XML (lo llamaremos fragment_listado.xml) quedaría por tanto de la siguiente forma:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    <ListView
        android:id="@+id/LstListado"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" >
    </ListView>
</LinearLayout>
```

```java
import android.app.Activity;
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AdapterView.OnItemClickListener;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;
import android.widget.TextView;

public class FragmentListado extends Fragment {

    private Correo[] datos =
            new Correo[]{
                new Correo("Persona 1", "Asunto del correo 1", "Texto del correo 1"),
                new Correo("Persona 2", "Asunto del correo 2", "Texto del correo 2"),
                new Correo("Persona 3", "Asunto del correo 3", "Texto del correo 3"),
                new Correo("Persona 4", "Asunto del correo 4", "Texto del correo 4"),
                new Correo("Persona 5", "Asunto del correo 5", "Texto del correo 5")};

    private ListView lstListado;

    @Override
    public View onCreateView(LayoutInflater inflater,
                             ViewGroup container,
                             Bundle savedInstanceState) {

        return inflater.inflate(R.layout.fragment_listado, container, false);
    }

    @Override
    public void onActivityCreated(Bundle state) {
        super.onActivityCreated(state);

        lstListado = (ListView)getView().findViewById(R.id.LstListado);

        lstListado.setAdapter(new AdaptadorCorreos(this));
    }

    class AdaptadorCorreos extends ArrayAdapter<Correo> {

        Activity context;

        AdaptadorCorreos(Fragment context) {
            super(context.getActivity(), R.layout.listitem_correo, datos);
            this.context = context.getActivity();
        }

        public View getView(int position, View convertView, ViewGroup parent) {
            LayoutInflater inflater = context.getLayoutInflater();
            View item = inflater.inflate(R.layout.listitem_correo, null);

            TextView lblDe = (TextView)item.findViewById(R.id.LblDe);
            lblDe.setText(datos[position].getDe());

            TextView lblAsunto = (TextView)item.findViewById(R.id.LblAsunto);
            lblAsunto.setText(datos[position].getAsunto());

            return(item);
        }
    }
}
```

```java
public class Correo
{
    private String de;
    private String asunto;
    private String texto;

    public Correo(String de, String asunto, String texto){
        this.de = de;
        this.asunto = asunto;
        this.texto = texto;
    }

    public String getDe(){
        return de;
    }

    public String getAsunto(){
        return asunto;
    }

    public String getTexto(){
        return texto;
    }
}
```

El primero de ellos, onCreateView(), es el “equivalente” al onCreate() de las actividades, y dentro de él es donde normalmente asignaremos un layout determinado al fragment. En este caso tendremos que “inflarlo” (convertir el XML en la estructura de objetos java equivalente) mediante el método inflate() pasándole como parámetro el ID del layout correspondiente, en nuestro caso fragment_listado.

El segundo de los métodos, onActivityCreated(), se ejecutará cuando la actividad contenedora del fragment esté completamente creada. En nuestro caso, estamos aprovechando este evento para obtener la referencia al control ListView y asociarle su adaptador.

Con esto ya tenemos creado nuestro fragment de listado, por lo que podemos pasar al segundo, que como ya dijimos se encargará de mostrar la vista de detalle. La definición de este fragment será aún más sencilla que la anterior. El layout, que llamaremos fragment_detalle.xml, tan sólo se compondrá de un cuadro de texto:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="#FFBBBBBB" >

    <TextView
        android:id="@+id/TxtDetalle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

</LinearLayout>
```

```java
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

public class FragmentDetalle extends Fragment {

    @Override
    public View onCreateView(LayoutInflater inflater,
                             ViewGroup container,
                             Bundle savedInstanceState) {

        return inflater.inflate(R.layout.fragment_detalle, container, false);
    }

    public void mostrarDetalle(String texto) {
        TextView txtDetalle =
            (TextView)getView().findViewById(R.id.TxtDetalle);

        txtDetalle.setText(texto);
    }
}
```
Una vez definidos los dos fragments, ya tan solo nos queda definir las actividades de nuestra aplicación, con sus respectivos layouts que harán uso de los fragments que acabamos de implementar.

Para la actividad principal definiremos 3 layouts diferentes: el primero de ellos para los casos en los que la aplicación se ejecute en una pantalla normal (por ejemplo un teléfono móvil) y dos para pantallas grandes (por ejemplo una tablet, uno pensado para orientación horizontal y otro para vertical). Todos se llamarán activity_main.xml, y lo que marcará la diferencia será la carpeta en la que colocaremos cada uno. Así, el primero de ellos lo colocaremos en la carpeta por defecto /res/layout, y los otros dos en las carpetas /res/layout-large (pantalla grande) y /res/latout-large-port (pantalla grande con orientación vertical) respectivamente. De esta forma, según el tamaño y orientación de la pantalla Android utilizará un layout u otro de forma automática sin que nosotros tengamos que hacer nada.

Para el caso de pantalla normal, la actividad principal mostrará tan sólo el listado de correos, por lo que el layout incluirá tan sólo el fragment FragmentListado.

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment xmlns:android="http://schemas.android.com/apk/res/android"
    class=".FragmentListado"
    android:id="@+id/FrgListado"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

Como podemos ver, para incluir un fragment en un layout utilizaremos una etiqueta < fragment > con un atributo class que indique la ruta completa de la clase java correspondiente al fragment, en este primer caso “.FragmentListado“. Los demás atributos utilizados son los que ya conocemos de id, layout_width y layout_height.

En este caso de pantalla normal, la vista de detalle se mostrará en una segunda actividad, por lo que también tendremos que crear su layout, que llamaremos activity_detalle.xml. Veamos rápidamente su implementación:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment xmlns:android="http://schemas.android.com/apk/res/android"
    class=".FragmentDetalle"
    android:id="@+id/FrgDetalle"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />

```

Como vemos es análoga a la anterior, con la única diferencia de que añadimos el fragment de detalle en vez de el de listado.

Por su parte, el layout para el caso de pantalla grande horizontal, será de la siguiente forma:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment class="net.sgoliver.android.fragments.FragmentListado"
        android:id="@+id/FrgListado"
        android:layout_weight="30"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <fragment class="net.sgoliver.android.fragments.FragmentDetalle"
        android:id="@+id/FrgDetalle"
        android:layout_weight="70"
        android:layout_width="0px"
        android:layout_height="match_parent" />

</LinearLayout>
```

Como ven en este caso incluimos los dos fragment en la misma pantalla, ya que tendremos espacio de sobra, ambos dentro de un LinearLayout horizontal, asignando al primero de ellos un peso (propiedad layout_weight) de 30 y al segundo de 70 para que la columna de listado ocupe un 30% de la pantalla a la izquierda y la de detalle ocupe el resto.

Por último, para el caso de pantalla grande vertical será practicamente igual, sólo que usaremos un LinearLayout vertical.


```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment class="net.sgoliver.android.fragments.FragmentListado"
        android:id="@+id/FrgListado"
        android:layout_weight="40"
        android:layout_width="match_parent"
        android:layout_height="0px" />

    <fragment class="net.sgoliver.android.fragments.FragmentDetalle"
        android:id="@+id/FrgDetalle"
        android:layout_weight="60"
        android:layout_width="match_parent"
        android:layout_height="0px" />

</LinearLayout>
```

Hecho esto, ya podríamos ejecutar la aplicación en el emulador y comprobar si se selecciona automáticamente el layout correcto dependiendo de las características del AVD que estemos utilizando.


Tarea:

Implementar el codigo para mostrar el texto