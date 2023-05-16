# RESUMEN SEGUNDO PARCIAL CM
## MAPAS
New Activity -> Gallery
• Se utiliza el asistente de creación de
actividades
• Una vez la actividad ha sido creada, el
asistente agrega las dependencias en el
archivo gradle del módulo
• Se crean tres archivos
• La actividad y el layout del mapa
• Un nuevo tag en el manifest con
## instrucciones para obtener un API KEY
```bash
'com.google.android.gms:play-services-maps:18.0.2
```
## API Key Secret
La clave no debería incluirse en los elementos que se envían al
repositorio, para esto se puede usar el archivo local.properties en la
base del proyecto
En el archivo manifest, se hace referencia a la llave secreta
Interfaz de Usuario
## Controles del mapa:
• Habilitar los “gestures” como “pinch to zoom”
```bash
mMap.uiSettings.isZoomGesturesEnabled = true
```
• Habilitar los botones de zoom
```bash
mMap.uiSettings.isZoomControlsEnabled = true
```
• Inclinación, rotación, desplazamiento etc.

## Interfaz de Usuario – Fragmentos
Representa una parte de la interfaz o del comportamiento dentro de
una actividad.
• Se pueden usar varios fragmentos en una actividad y reutilizarlos en
varias actividades.
• Siempre debe estar integrado en una actividad y su ciclo de vida es
propio pero depende del ciclo de vida de la actividad.
• Si la actividad esta en onPause() o onDestroy() los fragmentos igual,
sin embargo cuando la actividad esta en onResume() cada fragmento
se maneja de forma independiente.
El fragmento puede estar contenido dentro de la
actividad o puede estar en un archivo diferente.
## Interfaz de Usuario – Fragmentos – FrameLayout
Con un frameLayout, el mapa puede permanecer del
mismo tamaño y el cuadro de texto puede aparecer
encima con un nivel de transparencia.
• Para la transparencia, puede definir los dos primeros
valores del color de tamaños menores para algo más
transparente, y mayores para algo menos
transparente "#bb0099cc"

## Interfaz de Usuario – Marcadores
Son indicadores de una posición en el mapa. Se pueden personalizar de forma simple
(colores) o incluso colocando un ícono personalizado.
```bash
val plazaBolivar = LatLng(4.598302399831038, -74.0759898584449)
mMap.addMarker(MarkerOptions().position(plazaBolivar).title("Marker in Plaza de Bolivar"))
```
Las ventanas de información asociadas proporcionan información adicional.
• Agregando un snippet se agrega información que se despliega cuando se hace click en
el marcador
```bash
mMap.addMarker(MarkerOptions().position(plazaBolivar)
.title("Marker in Plaza de Bolivar")
.snippet("Población 10.331.626") //Texto de Información
.alpha(0.5f)) //Trasparencia
```

## Interfaz de Usuario – Marcadores Personalizados
Se puede cambiar el color del marcador
```bash
val plazaBolivarAzul = mMap.addMarker(MarkerOptions().position(plazaBolivar)
.icon(BitmapDescriptorFactory.defaultMarker(BitmapDescriptorFactory.HUE_BLUE)))
O utilizar una imagen personalizada
val plazaBolivarBike = mMap.addMarker(MarkerOptions().position(plazaBolivar)
.icon(BitmapDescriptorFactory.fromResource(R.drawable.bike)))
```
## Interfaz de Usuario – Quitar Marcadores
Cuando se hace seguimiento o hay mucha información, se hace
necesario eliminar los marcadores.
# PERSONALIZAR MAPAS

El resultado de la personalización será un archivo JSON de
propiedades del mapa, que se debe guardar en la siguiente ruta
del proyecto (new Android Resource Directory -> Raw):
/res/raw/style_json.json
Desde el código, hay que hacer referencia al archivo de estilo
del mapa:
```bash
mMap.setMapStyle(MapStyleOptions.loadRawResourceStyle(this,R.raw.style_json))
```
Un caso de uso clásico es definir un estilo con colores oscuros
en la noche. Para esto se pueden tener dos archivos de
personalización del mapa y cambiarlos de acuerdo a la hora

## SENSORES Y HARDWARE
Android cuenta con un API que permite la consulta de los diferentes
sensores del dispositivo. Estos sensores incluyen:
• Temperatura
• Presión Atmosférica
• Luminosidad
• Acelerómetro
• Giroscopio
• Proximidad

## Sensor de Luminosidad

Se necesitan tres atributos nuevos para la actividad disponibles en el paquete
android.hardware.
```bash
private lateinit var sensorManager: SensorManager
private lateinit var lightSensor: Sensor
private lateinit var lightSensorListener: SensorEventListener
```
## Inicializar el administrador de los sensores
```bash
//onCreate
sensorManager = getSystemService(Context.SENSOR_SERVICE) as SensorManager
lightSensor = sensorManager.getDefaultSensor(Sensor.TYPE_LIGHT)
```
El atributo lightSensor será nulo si el dispositivo no cuenta con este sensor.
Definir las acciones del listener. El sensor de luminosidad recibe
valores entre 0 y 40000 lx
## DIRECCIONES
API de Google – Limitar Búsquedas a una Región
Si se quiere limitar geográficamente la búsqueda a sólo una zona del
planeta (por ejemplo Bogotá), se define una región a través de cuatro
puntos:
```bash
// Limits for the geocoder search (Colombia)
companion object {
const val lowerLeftLatitude = 1.396967
const val lowerLeftLongitude = -78.903968
const val upperRightLatitude = 11.983639
const val upperRightLongitude = -71.869905
}
```
Luego se invoca a geocoder incluyendo los parámetros:
```bash
val addresses: List<Address>? = mGeocoder.getFromLocationName(
addressString, 2, lowerLeftLatitude, lowerLeftLongitude, upperRightLatitude, upperRightLongitude)
```
## MAPS ACTIVITY
Listener sobre un mapa
• Es posible programar dos eventos en el mapa, Click o LongClick.
• El evento generado retorna las coordenadas (objeto LatLng) donde el
usuario hace click.
• El siguiente fragmento de código genera un marcador con el nombre de
la dirección encontrada donde la persona hace un click largo.
```bash
mMap.setOnMapLongClickListener { latLng ->
mMap.clear()
mMap.addMarker(MarkerOptions().position(latLng).title(geoCoderSearchLatLang(latLng)))
}
```

## OPEN STREET MAPS
• Para usar open street maps, se debe incluir la dependencia en
gradle, versión actual: 6.1.14
```bash
'org.osmdroid:osmdroid-android:6.1.14'
```
• Definir el layout de su preferencia e incluir un elemento de tipo
MapView:
```bash
<org.osmdroid.views.MapView
android:layout_width="match_parent"
android:layout_height="match_parent"
android:id="@+id/osmMap">
</org.osmdroid.views.MapView>
```
## Dark Mode
• Desde Android 10, el usuario puede cambiar su interfaz para activar del modo oscuro,
el mapa se puede cambiar para reaccionar a este cambio.
Dark Mode – Crear el mapa de acuerdo al tema
• En onResume se debe acceder a las propiedades de interfaz para saber si el modo
oscuro esta activo. Si lo está, se puede cambiar el estilo del mapa para que se muestre
de acuerdo al modo seleccionado por el usuario:
```bash
override fun onResume() {
super.onResume()
binding.osmMap.onResume()
val mapController: IMapController = binding.osmMap.controller
mapController.setZoom(18.0)
mapController.setCenter(this.startPoint)
val uiManager = getSystemService(Context.UI_MODE_SERVICE) as UiModeManager
if(uiManager.nightMode == UiModeManager.MODE_NIGHT_YES)
binding.osmMap.overlayManager.tilesOverlay.setColorFilter(TilesOverlay.INVERT_COLORS)
}
```

# FIREBASE
## MULTI-USER
## Backend
• Para la creación de aplicaciones móviles existen dos formas de trabajar el
• Utilizar un backend genérico provisto por un tercero, por ejemplo:
Google firebase
Amazon amplify
Parse platform(sin infraestructura)
• Construir un backend a la medida, típicamente exponer servicios REST a través de:
• Java (JAX-RS)
• Spring
• Django
• .Net
• Etc!
## Autenticación – Pre-Requisitos
• Android Studio en su versión más reciente.
• El nivel de API objetivo debe ser 19 (KitKat) o superior. Android 4.4 o una
versión posterior.
• Jetpack (AndroidX), que incluye el cumplimiento de los siguientes requisitos
de versión:
• com.android.tools.build:gradle 3.2.1 o una versión posterior
• compileSdkVersion 28 o una versión posterior
• Servicios de Google Play habilitados tanto en el dispositivo como en el
emulador
• Cuenta de Google
## Autenticación – Correo y Password – Agregar proyecto a Firebase
Crear o seleccionar Proyecto Firebase desde la consola (Entrar con cuenta
Gmail)
## Registrar la App con Firebase
3. Agregar el archivo de configuración de Firebase
4. Ajustar las dependencias del Gradle para permitir autenticación
## Autenticación – Correo y Password
• Definir los atributos de la actividad necesarios para la autenticación
class semana10_E1 : 
```bash
AppCompatActivity() {
// [START declare_auth]
private lateinit var auth: FirebaseAuth
// [END declare_auth]
override fun onCreate(savedInstanceState: Bundle?) {
super.onCreate(savedInstanceState)
setContentView(R.layout.activity_semana10_e1)
// Initialize Firebase Auth
auth = Firebase.auth
}
}
```
• En onStart se verifica el estado actual de la autenticación del usuario, en este caso,
si el usuario esta autenticado, se lanza la siguiente actividad
```bash
override fun onStart() {
super.onStart()
val currentUser = auth.currentUser
updateUI(currentUser)
}

private fun updateUI(currentUser: FirebaseUser?) {
if (currentUser != null) {
val intent = Intent(this, MainApp_ICM::class.java)
intent.putExtra("user", currentUser.email)
startActivity(intent)
} else {
binding.editTextTextEmailAddress.setText("")
binding.editTextTextPassword.setText("")
}
}
```
## Autenticación – Correo y Password – Validación de Campos
• Verificación básica de correo
```bash
private fun isEmailValid(email: String): Boolean {
if (!email.contains("@") ||
!email.contains(".") ||
email.length < 5)
return false
return true
}
Autenticación – Correo y Password – SignIn (Validado)
private fun signInUser(email: String, password: String){
if(validateForm()){
auth.signInWithEmailAndPassword(email,password)
.addOnCompleteListener(this) { task ->
if (task.isSuccessful) {
// Sign in success, update UI
Log.d(TAG, "signInWithEmail:success:")
val user = auth.currentUser
updateUI(auth.currentUser)
} else {
Log.w(TAG, "signInWithEmail:failure", task.exception)
Toast.makeText(this, "Authentication failed.",
Toast.LENGTH_SHORT).show()
updateUI(null)
}
}
}
}
```
## Autenticación – Correo y Password – LogOut
• Es necesario dar al usuario la opción de salir de la sesión autenticada.
• Una forma simple de hacerlo:
```bash
binding.btSignOut.setOnClickListener {
auth.signOut()
}
```
## Opciones de menú
• Se pueden definir opciones en la barra de titulo de la aplicación
• Para esto, hay que definir un archivo de menú en la carpeta de recursos
## FIREBASE DATABASES
• Realtime Database es la base de datos original de Firebase. Es una solución eficiente y de baja latencia para
aplicaciones móviles que requieren estados sincronizados entre clientes en tiempo real.
• Cloud Firestore es la nueva base de datos insignia de Firebase para el desarrollo de aplicaciones móviles. Mejora
los éxitos de Realtime Database con un nuevo modelo de datos más intuitivo. Cloud Firestore también ofrece
consultas más ricas y rápidas y se escala mejor que la Realtime Database.
Firebase Realtime Database
• Base de datos documental (basada en JSON) dinámica
• Por defecto, sólo usuarios autenticados pueden leer y escribir datos. Se pueden modificar
las reglas para permitir hacer cambios a cualquier usuario, pero no se recomienda dejar sin
protección los datos.
• Los datos en Firebase se pueden inicializar a partir de un documento JSON.
También es posible exportar un documento con el contenido de la base de datos.

## Firebase Realtime Database – Conectar a la App
• Seguir el procedimiento del asistente (ya debe estar conectado si se hizo el
proceso con la autenticación). Luego agregar las dependencias. También se
pueden agregar de forma manual a través de Gradle.
## Firebase Realtime Database – Escribir Datos
• Operaciones básicas de escritura
• Para operaciones básicas de escritura, puede usar setValue() para guardar datos en una referencia
específica, reemplazando cualquier dato existente en esa ruta. Puede utilizar este método para:
• Pasar los tipos que correspondan a los tipos JSON disponibles de la siguiente manera:
• String
• Long
• Double
• Boolean
• Map<String, Object>
• List<Object>
• Pasar un objeto Java personalizado, si la clase que lo define tiene un constructor
predeterminado que no toma argumentos y tiene captadores públicos para las propiedades
que se van a asignar.
## Firebase Realtime Database – Escribir Objetos
• Definir un objeto POJO, con getters públicos, y un constructor sin argumentos.
  ```bash
public class MyUser {
var name: String = “” //...}
  ```
• Definir un path para todos los usuarios de la aplicación
  ```bash
//Root path of every user in FB
const val PATH_USERS="users/"
  ```
• Una vez los datos esten completos, persistir el objeto
  ```bash
val myUser = MyUser()
myUser.name = "Andrés"
myUser.lastName = "Acevedo"
myUser.age = 30
myUser.height = 1.80
myUser.weight = 80.0
myRef = database.getReference(PATH_USERS+auth.currentUser!!.uid)
myRef.setValue(myUser)
  ```
## Firebase Realtime Database – Escribir Objetos
## Creación de índices
• Se puede dejar a Firebase la tarea de generar índices para objetos que se quieran
persistir usando el método push:
  ```bash
val key = myRef.push().key
myRef = database.getReference(PATH_USERS + key)
myRef.setValue(myUser)
  ```
## Firebase Realtime Database – Consulta Vs Suscripción
• El comportamiento típico de la base de datos dinámica de Firebase es suscribirse
a todos los cambios que haya para algún dato (Path) en particular.
• A diferencia de SQL, se recibe un evento cuando algún dato al interior del path
especificado cambia de valor usando el evento addValueEventListener()
• El evento se recibe en todos los dispositivos que corran la aplicación, y es
multiplataforma (e.g., IOS, Ionic, etc.)
• Si sólo es necesario leer una vez los datos, es posible hacerlo con una consulta
única a través del evento addListenerForSingleValueEvent()
Firebase Realtime Database – Remover Suscripción
• Los callbacks se eliminan llamando al método removeEventListener() en su
referencia de base de datos Firebase.
• Si un listener ha sido añadido múltiples veces a una localización de datos, es
llamado múltiples veces para cada evento, y debes desvincularlo el mismo
número de veces para eliminarlo completamente.
• La llamada a removeEventListener() en un listener padre no elimina
automáticamente los listener registrados en sus nodos hijos;
removeEventListener() debe ser llamada también en cualquier listener hijo
para eliminar la llamada de retorno.
Recomendaciones de Almacenamiento
• La base de datos es un árbol en JSON, esto trae algunas implicaciones
que requieren un diseño diferente al relacional:
• Evitar anidamientos de datos, usar índices para identificar
información relacionada.
• Definir una estructura de datos tan plana como sea posible.
• Duplicar datos para mejorar rendimiento en relaciones
bidireccionales.

# FLUTTER
• Flutter es un framework de desarrollo móvil,web y de escritorio
(windows) creado por Google.
• Presentado en 2015. La version 1 fue lanzado en diciembre de 2018.
• Basado en Material Design.
• Se utiliza el lenguaje de programación Dart de Google
• Una base de código para las dos plataformas Android y iOS
• Generación de codigo nativo!
- No se depende de un browser
- No está basado en JavaScript ni HTML 5
• Se puede desarrollar con Linux, Windows o Mac usando Android Studio
o Visual Studio Code
• Se pueden usar los mismos emuladores creados con Android Studio
para Android, y los de XCode para iOS
- Si se desea probar la aplicación en iOS es necesario usar un computador con
MacOS y XCode

## Estructura del Proyecto
• lib
- Carpeta con el código fuente
• test
- Carpeta con el código fuente de las pruebas
• pubscpec.yaml
- Archivo de configuración del proyecto
• android e ios
- Carpetas generadas automáticamente para
cada plataforma destino
## DART
• Existe un tipo de dato dinámico que puede cambiar en ejecución. Para
esto es necesario usar la palabra reservada Dynamic
• Constructor
- Al igual que en java el constructor de la clase lleva el mismo nombre de la clase
y los parámetros a inicializar.
• Las funciones en Dart pueden tener parámetros con nombre y opcionales
• Referencias a funciones. Dart es un lenguaje funcional y permite tener
referencias a funciones y pasarlas como parámetro.
• Funciones anónimas
- Son funciones con un bloque de Código que no tienen un nombre. Si en el ejemplo
anterior no se quisiera definir una función, el atributo onPressed se podría definir
así:
  ```bash
floatingActionButton: FloatingActionButton(
onPressed: () {
_counter++;
}, //fin función anónima
tooltip: 'Increment',
child: Icon(Icons.add),
),
  ```
• List, es un conjunto de elementos
• Map, conjunto llave valor
• Todo es un widget!!
- Cada Widget tiene un método build asociado
• Los widgets pueden ser:
- Stateless
‣ No tienen estado, solo se corre el método build una vez y si tienen datos estos no pueden
variar durante la ejecución (final)
- Statefull
‣ Tiene un estado asociado
‣ Se corre el método build cada vez que cambia el estado
‣ Se debe usaR el método setState para actualizar el estado
• Flutter funciona de forma declarativa.
• Se define un árbol o jerarquía de widgets que deben aparecer en la
pantalla.
• El método build se encarga de recorrer el árbol y dibujar los elementos
que encuentre.
- Si es un stateless widget, se hace una vez
- Si es un stateful widget, se hace cada vez que cambie el estado
TextField y Controladores
• Para acceder al valor de un textfield en flutter, es necesario definir un controlador, y
asignarlo al textfield
• Para saber lo que el usuario ingresó en el TextField se accede a la
propiedad text del controlador
## IMÁGENES
• Para almacenar las imágenes en la aplicación, se agregan a una
carpeta dentro del proyecto y se referencian desde el archivo
pubspec.yaml
## TEXTSFIELDS & CONTROLLERS
• Para acceder al valor de un campo de texto en flutter, es necesario
definir un controlador, y asignarlo al campo
• Para saber lo que el usuario ingresó en el campo se accede a la
propiedad text del controlador
## ACCESO A INTERFAZ
• Dado que flutter se encarga del método build para construir el árbol
de elementos, éstos no se pueden modificar de forma programática
desde otras funciones de la clase.
• En este caso para modificar un valor de la interfaz, por ejemplo
deshabilitar un botón, hay que hacerlo a través de la modificación del
estado del widget.
• Primero hay que definir que el botón depende del estado y luego
modificar el estado con setState cuando sea necesario para lograr el
efecto deseado.

## NAVEGACIÓN
• Para hacer una transición entre dos pantallas se usa el objeto Navigator
de Flutter.
Transición entre pantallas
• En el origen se puede usar el método push para iniciar la transición hacia la pantalla
(Widget) de destino
• También se pueden enviar datos entre las pantallas. En este caso los datos llegan como
parámetros al constructor del widget de destino
Named Navigation
• Si en la aplicación se tienen muchas pantallas, es ideal definir todas las
rutas posibles, asignarles un nombre y luego a través del Navigator, ir a una
pantalla en específico.
• Para esto es necesario primero definir las rutas.
• Cada ruta debe aparecer identificada, tipicamente en el primer widget de la
aplicación: MaterialApp
• En el widget de la aplicación se pueden definir las rutas y la ruta inicial a cargar. Cada
ruta puede estar en un archivo diferente pero deben importarse.
• Una vez se tengan las rutas, se puede utilizar el método pushedNamed( )
del Navigator:
  ```bash
void updateUI() {
if (validateLogin()) {
_userEmail = FirebaseAuth.instance.currentUser?.email;
Navigator.of(context).pushNamed('/map');
}
}
  ```
## MANEJO DE ESTADOS
• Si bien se puede usar setState, esta práctica no es recomendable para
aplicaciones más complejas que manejen diferentes estados en varias
pantallas con muchos widgets.
• Para esto se puede usar una libreria como Provider para separar la
responsabilidad del manejo del estado de cada widget y definir un
funcionamiento basado en eventos.
• Provider se puede entender como la implementación de un patron
Observer para el estado de la aplicación.
## Provider
## Pasos para usar Provider
1. Agregar dependencias
2. Definir el modelo para el estado
3. Registrarlo en el contexto
4. Operaciones sobre el estado
1. Watch
2. Read
3. Select
## Asignación de Dependencias
• Dentro del archivo pubspec.yaml, es necesario agregar la dependencia de
Provider:
dev_dependencies:
flutter_test:
sdk: flutter
provider: ^6.0.0
• Para actualizar las dependencias (si no se hace automáticamente), se
pueden ejecutar los comandos:
• flutter clean
• flutter pub get
Definir el modelo para el estado
• El modelo corresponde a una o varias clases que extienden de ChangeNotifier. En
cada modificación del estado, se debe llamar a notifyListeners()
## Registro estado – contexto
• Hay que identificar un ancestro común a todos los widgets que dependen del estado
qué se esta definiendo. Si toda la aplicación depende del estado, este Widget puede ser
MaterialApp
Registro estado
• En este caso se envuelve al widget de MaterialApp con ChangeNotifierProvider. Este
recibe dos parámetros, el constructor del estado y la clase original que se quiere
envolver.
Acceder al estado
Hay tres métodos para acceder al estado en Provider:
• watch: se usa cuando se quiere acceder al modelo y además se queire
reconstruir el widget cuando el modelo cambie
• read: se usa cuando se quiere acceder al modelo, pero no se quiere reconstruir
el widget cuando el modelo cambie
• select: igual que watch, pero sólo para una parte del modelo
• watch y select se pueden usar sólo dentro del método build de un widget
• read no se puede usar en el método build, se usa típicamente en lo métodos
callback referenciados en el onPress de un botón por ejemplo
• Cuando se llama a notifyListeners() en un modelo, todos los métodos build que
hayan accedido al modelo usando watch o select se vuelven a invocar.

