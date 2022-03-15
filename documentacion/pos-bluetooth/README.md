# POS Integrado Bluetooth

## Modelo de Operación

<img src="/images/documentacion/flujo-POS-bt.svg" class="td_img-night" alt="Modelo de operación">

***La caja movilizada del comercio es la encargada de entregar internet al POS Integrado Bluetooth mediante la conexión bluetooth que se establece entre los dos dispositivos.**  

****Es el POS Integrado Bluetooth quien se comunica con Transbank para toda transacción financiera**

## Requerimientos
### Componentes

#### Android
*  Librería POS Integrado Bluetooth: Librería encargada de manejar la transacción de POS  integrado Bluetooth, iniciándola y obteniendo la respuesta.

#### iOS
* Framework iSMP(PCL): librería necesaria para comunicación Bluetooth con terminal.
* Framework Mpos Integrado: Librería encargada de manejar la transacción de Mpos integrado, iniciándola y obteniendo la respuesta.

### Herramientas
* IDE Android Studio
* Xcode, una versión compatible con Swift 4.1 o 3+

### Observaciones
* Todas las instrucciones fueron hechas en base Android Studio 4.0.1, Gradle 5.6.4, Xcode 9.4.1 y Swift 4.1

### Lenguajes soportados
* Java Nativo Android
* Kotlin 1.x
* Swift

### Versión mínima de desarrollo
* Android 4.x (API 14)
* iOS 10.0

##  Android

### Instalación
Es importante aclarar que la librería cuenta con dos componentes, uno por el lado de la comunicación con el terminal 
llamado PCL y la parte de POS Integrado Bluetooth la cual se encarga de la transacción. Esta sección explica que es 
necesario para establecer la comunicación Bluetooth entre el terminal de pago (Link2500) y el smartphone.

* Copiar el archivo “mposintegrado.aar” [**Descargar**](https://transbankdevelopers.cl/files/mposintegrado.aar) en el directorio de librerías (en este caso libs) de la aplicación.
* En el archivo build.gradle a nivel de proyecto agregar en la sección allProjects->repositories , lo siguiente:

```java
flatDir {dirs 'libs'}
```

* En el archivo build.gradle a nivel de la aplicación en la sección dependencies agregar:

```java  

implementation (name:'mposintegrado', ext:'aar')
```

Esto agregara la librería para su uso en el proyecto, luego para importarlo en el código se utiliza:

```java  
import com.ingenico.pclservice.PclService;
import com.ingenico.pclutilities.PclUtilities;
import com.ingenico.pclutilities.PclUtilities.BluetoothCompanion;
import com.ingenico.pclutilities.SslObject;
import posintegrado.ingenico.com.mposintegrado.mposLib;
```

### Requerimientos
Para el correcto funcionamiento de la librería se deben agregar los siguientes permisos al tag “manifest” del Android manifest de la aplicación:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.BLUETOOTH"/>
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
```

Adicional es necesario agregar los siguientes servicios dentro del tag application del Android manifest:

```xml
<service android:name="com.ingenico.pclservice.PclService" />
<service android:name="com.ingenico.pclservice.BluetoothService"/>
```

### Estableciendo comunicación con el terminal de pago

La comunicación con el terminal como ya se dijo con anterioridad es realizada mediante el componente
PCL incluido ya en la librería de POS Integrado Bluetooth.
Para la implementación usaremos dos clases:

* CommonActivity: Clase abstracta la cual cuenta con la implementación (y definición) de distintos
métodos y atributos asociados a la comunicación, los cuales son usados por las distintas
actividades.
* MainActivity: Clase que extiende commonActivity, esta contiene la lógica de la aplicación en sí.


*Observación: El uso de una clase abstracta en común para las distintas actividades del app es recomendado, pero esto 
queda a decisión del desarrollador. Para efectos del ejemplo se irá detallando lo que se encuentra en cada clase y su objetivo.*

### CommonActivity

Lo primero a tomar en cuenta, es que esta clase debe extender de Activity
Lo siguiente es definir las siguientes variables utilizadas en el proceso de conexión:

* protected PclService mPclService = null;
  - Manejo del servicio PCL (comunicación)
* Protected boolean mBound = false;
  - Control del estado servicio PCL (comunicación)
* Protected PclUtilities mPclUtil;
  - Manejo de funcionalidades de PCL (comunicación)
* Protected mposLib mposLibobj;
  - Manejo de transacciones de mposIntegrado.
* Protected boolean mServiceStarted;
  - Control del estado servicio PCL (comunicación)
* Protected CharSequence mCurrentDevice;
  - Dispositivo actual.

### Binding al servicio PCL (Comunicación)

Se define la siguiente clase para la interacción con el terminal la cual se encarga de hacerle Bind al servicio
de PCL para la comunicación entre terminal y smartphone:

```java
// Implement ServiceConnection
class PclServiceConnection implements ServiceConnection
{
  public void onServiceConnected(ComponentName className, IBinder boundService )
  {
    /*We've bound to LocalService, cast the IBinder and get
    LocalService instance*/
    PclService.LocalBinder binder = (PclService.LocalBinder)
    boundService;
    mPclService = (PclService) binder.getService();
    onPclServiceConnected();
  } 
    public void onServiceDisconnected(ComponentName className)
    {
      mPclService = null;
    }
};
```

Esta clase tiene dos métodos definidos para la desconexión y conexión respectivamente.
Luego de ser definida esta clase es importante agregar una instancia de esta, es decir:

```java
private PclServiceConnection mServiceConnection;
```

Adicional se definen los siguientes métodos para iniciar el servicio de PCL y hacer release de este.

```java
// You can call this method in onCreate for instance
protected void initService()
{
    if (!mBound)
  {
    mServiceConnection = new PclServiceConnection();
    Intent intent = new Intent(this, PclService.class);
    mBound = bindService(intent, mServiceConnection,
    Context.BIND_AUTO_CREATE);
   }
}
protected void releaseService()
{
  if (mBound)
  {
    unbindService(mServiceConnection );
    mBound = false;
  }
}
```

### Manejo de estado de la conexión

Para controlar el manejo de la conexión se puede realizar de dos formas, mediante una llamada al api de
PCL serverStatus constantemente o mediante la implementación de un broadcast receiver. Para efecto de
esta implementación se usa la segunda opción:

Primero se implementa la clase **StateReceiver** la cual extiende de **BroadcastReceiver**:

```java
private class StateReceiver extends BroadcastReceiver
{
    private CommonActivity ViewOwner = null;
    @SuppressLint("UseValueOf")
    public void onReceive(Context context, Intent intent)
    {
      String state = intent.getStringExtra("state");
      ViewOwner.onStateChanged(state);
    }
    StateReceiver(CommonActivity receiver)
    {
      super();
      ViewOwner = receiver;
    }
}
```

Luego de definida esta clase es importante agregar una instancia de esta, es decir:

```java
private StateReceiver m_StateReceiver = null;
```

Luego se agrega los métodos que permiten iniciar o detener el broadcast receiver
```java
private void initStateReceiver()
{
  if(m_StateReceiver == null)
  {
    m_StateReceiver = new StateReceiver(this);
    IntentFilter intentfilter = new IntentFilter(
    IntentFilter("com.ingenico.pclservice.intent.action.STATE_CHANGED" );
    registerReceiver(m_StateReceiver, intentfilter);
  }
}
private void releaseStateReceiver()
{
  if(m_StateReceiver != null)
  {
    unregisterReceiver(m_StateReceiver);
    m_StateReceiver = null;
  }
}
```

El objetivo es de escuchar por **"com.ingenico.pclservice.intent.action.STATE_CHANGED"** y obtener el string
extra de “state” el cual puede ser **“CONNECTED”** o **“DISCONNECTED”**

Adicional se agregan las siguientes definiciones abstractas:

```java
abstract void onStateChanged(String state);
abstract void onPclServiceConnected();
```

Las cuales deben ser implementadas por las distintas clases que extiendan de esta. Con el fin de que cada
una de ellas pueda decidir qué hacer al momento de conectarse o desconectarse a nivel de dispositivo y
servicio.

Se agrega a los eventos **onResume** y **onPause** la llamada a los métodos **initStateReceiver** y **releaseStateReceiver** implementados anteriormente para controlar cuando se está escuchando y cuando no.


```java
@Override
protected void onResume()
{
  super.onResume();
  initStateReceiver();
}
@Override
protected void onPause()
{
  super.onPause();
  releaseStateReceiver();
}
```

Otra función importante de implementar es una en la cual se utilice el api de PCL “serverStatus” la cual
permite validar si hay algún dispositivo conectado actualmente. La forma de implementación de esto queda
a definición del desarrollador. En el ejemplo se implementó de la siguiente forma:

```java
public boolean isCompanionConnected()
{
  boolean bRet = false;
  if (mPclService != null)
  {
    byte result[] = new byte[1];
    {
      if (mPclService.serverStatus(result) == true)
      {
        if (result[0] == 0x10)
        {
          bRet = true;
        }
      }
    }
  }
  return bRet;
}
```

### Main Activity

Esta es la actividad principal de implementación y la cual extiende a **CommonActivity**.

Lo primero a realizar, es instanciar en el método **onCreate** el evento **“mPclUtil”** que revisa si hay algún dispositivo ya activado de la siguiente forma:


```java
mPclUtil = new PclUtilities(this, "posintegrado.ingenico.com.sampleandroidapp", "pairing_addr.txt");
```

Luego se crean los siguientes métodos para iniciar o detener el servicio de PCL(comunicación)

```java
private void startPclService()
{
  if (!mServiceStarted)
  {
    SharedPreferences settings = getSharedPreferences("PCLSERVICE", MODE_PRIVATE);
    boolean enableLog = settings.getBoolean("ENABLE_LOG", true);
    Intent i = new Intent(this, PclService.class);
    i.putExtra("PACKAGE_NAME", "posintegrado.ingenico.com.sampleandroidapp");
    i.putExtra("FILE_NAME", "pairing_addr.txt");
    i.putExtra("ENABLE_LOG", enableLog);
    if (getApplicationContext().startService(i) != null) mServiceStarted = true;
  }
}

private void stopPclService()
{
  if (mServiceStarted)
  {
    Intent i = new Intent(this, PclService.class);
    if (getApplicationContext().stopService(i))
        mServiceStarted = false;
  }
}
```

En las siguientes líneas de código se realiza la elección de terminal y se llama a la función de inicio de la comunicación.

```java
/*Variable de control si ya se encontró un dispositivo*/
boolean bFound = false;
/*Se obtiene con esto la lista de los dispositivos ingenico paired con el
terminal*/
Set<BluetoothCompanion> btComps = mPclUtil.GetPairedCompanions();
if (btComps != null && (btComps.size() > 0)) {
    /* Loop through paired devices*/
    for (BluetoothCompanion comp : btComps) {
        /*Aca se revisa si el dispositivo esta activo y lo define como el actual*/
        if (comp.isActivated()) {
            bFound = true;
            mCurrentDevice = comp.getBluetoothDevice().getAddress() + " - " +
            comp.getBluetoothDevice().getName();
        }
        else {
        /*Se activa el dispositivo*/
        mPclUtil.ActivateCompanion(comp.getBluetoothDevice().getAddress());
        return;
        }
    }
}
```

Para efectos de este ejemplo si no encuentra ninguno activado, activa el primer dispositivo pareado con el Smartphone. Esta lógica debe ser implementada según defina el desarrollador ya que es un flujo que depende de la aplicación misma.  

### Efectuando transacciones con POS Integrado Bluetooth

Luego de tener el terminal con la comunicación establecida con la caja
movilizada como se explicó anteriormente, se puede dar inicio a la
implementación de las diversas transacciones disponibles en la Integración
Nativa de POS Integrado:
- [Transacción de Venta](/referencia/posintegrado#mensaje-de-venta)
- [Transacción Última Venta](/referencia/posintegrado#mensaje-de-ultima-venta)
- [Transacción Anulación Venta](/referencia/posintegrado#mensaje-de-anulacion)
- [Transacción de Cierre](/referencia/posintegrado#mensaje-de-cierre)
- [Transacción Detalle de Ventas](/referencia/posintegrado#mensaje-detalle-de-ventas)
- [Transacción Totales](/referencia/posintegrado#mensaje-de-totales)
- [Transacción Carga de Llaves](/referencia/posintegrado#mensaje-de-carga-de-llaves)
- [Transacción Polling](/referencia/posintegrado#mensaje-de-poll)
  
Por ejemplo, para implementar una Transacción de Venta, lo primero es crear un método que gatillara la acción para transaccionar con el POS integrado bluetooth:


```java
mposLibobj = new mposLib(mPclService);
String stx = "02";
String ext = "03";

String mensajeriaTrx = "0200|20000|123456|||0";
/*Convierto mi string de trx a hex*/
String trxToHex = mposLibobj.convertStringToHex(mensajeriaTrx);
/*Luego calculo el largo de mi trama en hex (LRC)*/
String obtenerLrc = calcularLRC(trxToHex);
/*Ahora armo el comando completo de trx*/
String trxCompleta = stx+trxToHex+ext+ obtenerLrc;
/*Envio el comando completo para que el POS integrado Bluetooth lo procese*/
mposLibobj.stratTransaction(trxCompleta);
```

El método que calcula el LRC del comando de transacción en hex es el siguiente:

```java
private String calcularLRC(String input){
    String LRC = "";
    int uintVal = 0;
    if(input != ""){
        byte[] arrayofhex = hexStringToByteArray(input);
        for(int count = 1; count < arrayofhex.length; ++count){
            if(count == 1){
                uintVal = arrayofhex[count - 1] ^ arrayofhex[count];
            }else{
                uintVal ^= arrayofhex[count];
            }
        }
    }
    LRC = Interger.toHexString(uintVal).toUpperCase();
    int f = LRC.length();
    if(f == 2){
        return LRC;
    }else {
        char[] chars = LRC.toCharArray();
        StringBuffer hex = new StringBuffer();
        for(int i = 0; i < chars.length; i++) {
            hex.append(Integer.toHexString(chars[i]));
        }
        return hex.toString();
    }
}
```

Luego se debe implementar el callback de la librería de POS Integrado Bluetooth encargado de capturar la respuesta luego de finalizada la transacción. Esto se hace de la siguiente forma

```java
mposLibobj.setOnTransactionFinishedListener(new mposLib.onTransactionFinishedListener() {
    public void onFinish(String response) {
    /*EJEMPLO DE RESPONSE*/
        String respToString = mposLibobj.convertHexToString(response);
        Log.i("Respuesta response: ", respToString);
    }
});
```
Acá el integrador deberá decidir qué hacer con la respuesta obtenida en la variable response, cuando se gatille este callback.

<div class="container slate">
  <div class='slate-after-footer'>
    <div class='row d-flex align-items-stretch'>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22 text-center'>¿Tienes alguna duda de integración?</h3>
        <a href='https://join-transbankdevelopers-slack.herokuapp.com/' target='_blank'>
          <div class='td_block_gray'>
            <img src="https://p9.zdassets.com/hc/theme_assets/138842/200037786/logo.png" alt="" >
            <div class='td_pa-txt'>
              Únete a la comunidad de integradores en Slack y comparte buenos tips ayudando a los nuevos o buscando soluciones alternativas. Nuestro equipo está ahí para ayudarte.
            </div>
          </div>
        </a>
      </div>
      <div class='mt-3 mt-lg-0 col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22 text-center'>Si aún tienes dudas envíanos un mensaje</h3>
        <a class="pointer magenta" data-toggle='modal' data-target='#modalContactForm'>
          <div class='td_block_gray'>
            <div class="fo-size-20 text-center sub-title_bloq"><i class="fas fa-envelope"></i> Envíanos un mensaje.</div>
            <div class='td_pa-txt'>
              Si necesitas resolver algún tipo de incidencia en el portal o si existe algún problema con tu integración y  que no has podido resolver, contáctanos a través de nuestro formulario.
            </div>
          </div>
        </a>
      </div>
    </div>
  </div>
</div>
