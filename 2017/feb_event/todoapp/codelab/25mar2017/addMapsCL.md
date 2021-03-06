
Add maps code lab
===============

1- Primeramente comencemos este CodeLab descargando Google play services desde nuestro SDK manager. Para acceder al SDK manager, la barra superior selecciona Tools > Android > SDK Manager

<img src="http://i.imgur.com/z5BFUeI.png">

Se abrirá una ventana, haz click en **Launch Standalone SDK Manager** debajo de la lista que aparece en esta ventana. Esto nos llevara a otra ventana donde podemos ver todo el todas las opciones del SDK, descarga/actualiza la opcion de Google Play Services:

<img src="http://i.imgur.com/DKWUOyR.png">


2- Ahora agrega un nuevo paquete dentro de activities y llámalo **map**,  la estructura de tu proyecto se deberá ver así:

<img src="http://i.imgur.com/cLKXDa0.png">

Para agregar mapas vamos a utilizar la ayuda de Android Studio. crea una nueva Activity con mapas dentro del paquete **map**:

<img src="http://i.imgur.com/1N5yJT4.png">

Selecciona la opcion de **Gallery..**

<img src="http://i.imgur.com/68NTnKH.png">

Y despues **Google Maps Activity**

Esto nos generara la **MapsActivity**, **activity_maps.xml**(no le moveremos nada) **google_maps_api.xml** y modificara nuestro **AndroidManifest.xml** y **build.gradle**(Module: app).

3- En **build.gradle**(Module: app) el código autogenerado nos habrá agregado:
``` 
compile 'com.google.android.gms:play-services:10.2.0'
``` 
La versión puede variar al momento de la redacción este era la versión mas reciente. Reemplaza esta linea por :
``` 
compile 'com.google.android.gms:play-services-maps:10.2.0'
``` 
Ya que no ocupamos los otros servicios de google mas que los mapas.

4- haz log-in en https://console.developers.google.com/ después entra a **google_maps_api.xml** copia y pega la URL mas larga que se genero

<img src="http://i.imgur.com/mUV2qzJ.png">

crea un proyecto nuevo 

<img src="http://i.imgur.com/G2mF8Gc.png">

crea una clave de API 

<img src="http://i.imgur.com/XzxHbNL.png">

Registra tu clave, dale un **Nombre** y guardalo

<img src="http://i.imgur.com/BHj3x75.png">

una vez obtenida tu clave copiala a **google_maps_api.xml** y pegala en donde dice YOUR_KEY_HERE. 

5- Ahora como llegamos a MapsActivity? muy simple modifiquemos **ToDoListAdapter** para que cuando presionemos el ImageButton nos lleve a MapsActivity:
``` java
@Override
public void onClick(View view) {
...
if (view.getId() == R.id.mapImageButton) {
Intent intent = new Intent(view.getContext(), MapsActivity.class);
intent.putExtra(DetailActivity.EXTRA_TODO_KEY, item);
view.getContext().startActivity(intent);
}
...
}
```
6- Corre la aplicación y presiona mapImageButton, debería de llevarte a ver el mapa en sydney.

7- En **MapsActivity** declara la siguiente constante que representa un zoom al mapa a una altura de 200 metros sobre el suelo.
``` java
private final float TWO_HUNDRED_MTS_ZOOM = 15;
```

modifica **onCreate()** para que reciba el ToDo que le vamos estar enviando y también modifica **onMapReady()** para que muestre la información de locación que tiene el ToDo:

``` java
@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_maps);

if (getIntent().hasExtra(DetailActivity.EXTRA_TODO_KEY)) {
toDoContent = (ToDoContent) getIntent().getExtras()
.getSerializable(DetailActivity.EXTRA_TODO_KEY);
}

SupportMapFragment mapFragment = (SupportMapFragment) getSupportFragmentManager()
.findFragmentById(R.id.map);
mapFragment.getMapAsync(this);
}

@Override
public void onMapReady(GoogleMap googleMap) {
map = googleMap;
LatLng toDoLatLng = new LatLng(toDoContent.getLat(), toDoContent.getLng());
map.addMarker(new MarkerOptions().position(toDoLatLng).title(toDoContent.getTitle()));
map.moveCamera(CameraUpdateFactory.newLatLngZoom(toDoLatLng, TWO_HUNDRED_MTS_ZOOM));
}
```

Con esto dejamos listo mapsActivity para la funcionalidad de Locations!.
