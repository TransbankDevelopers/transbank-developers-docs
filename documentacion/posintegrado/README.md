# POS Integrado

<div class="pos-title-nav">
  <div tbk-link='/referencia/posintegrado?csharp#' tbk-link-name='Referencia'></div>
</div>

## Cómo empezar

Primero, debes instalar en tu máquina la librería/SDK en C, puedes encontrar el código fuente en [GitHub](https://github.com/TransbankDevelopers/transbank-pos-sdk-c) y seguir las instrucciones para compilarlo. También puede usar las DLLs ya compiladas que se adjuntan en el [último release](https://github.com/TransbankDevelopers/transbank-pos-sdk-c/releases/latest)

Esta librería y sus dependencias (libserialport) son requisitos para utilizar el SDK.

Por el momento, hay un SDK para [.NET](https://github.com/TransbankDevelopers/transbank-pos-sdk-dotnet), [Java](https://github.com/TransbankDevelopers/transbank-pos-sdk-java) y [Javascript (tecnologías web)](https://github.com/TransbankDevelopers/transbank-pos-sdk-web-js). 


### SDK .NET
Para .NET lo puedes encontrar en [NuGet.org](https://www.nuget.org/packages/TransbankPosSDK/) para instalarlo puedes utilizar por ejemplo el package manager de VisualStudio.
```bash
PM> Install-Package TransbankPosSDK
```

### SDK Java
Para Java se puede incluir el paquete por [Maven.](https://search.maven.org/artifact/com.github.transbankdevelopers/transbank-pos-sdk-java/)

```xml
		<dependency>
			<groupId>com.github.transbankdevelopers</groupId>
			<artifactId>transbank-sdk-pos-java</artifactId>
			<version>1.0-SNAPSHOT</version>
		</dependency>
```

### SDK Web
El SDK Web consta de dos partes: [SDK Javacript](https://github.com/TransbankDevelopers/transbank-pos-sdk-web-js) y [Cliente Desktop](https://github.com/TransbankDevelopers/transbank-pos-sdk-web-client). 

[Agente Desktop](https://github.com/TransbankDevelopers/transbank-pos-sdk-web-agent): Este agente es un programa que se debe instalar e inicializar en el computador que tendrá el equipo POS conectado físicamente. Al instalar e inicializar este servicio, se creará un servidor de websockets local en el puerto `8090` que permitirá, a través del [SDK de Javascript](https://github.com/TransbankDevelopers/transbank-pos-sdk-web-js), poder enviar y recibir mensajes del equipo POS, de manera simple y transparente.

[SDK Javascript](https://github.com/TransbankDevelopers/transbank-pos-sdk-web-js): Este SDK se debe instalar en el software de caja (o cualquier software web que presente HTML, CSS y JS en un navegador web). Este SDK entrega una interfaz simple para conectarse con el cliente desktop, de manera que se puedan mandar instrucciones al POS con un API fácil de usar directamente desde el browser.

Dentro de cada repositorio se encuentra la documentación más detallada. 

Instalar el SDK en el software web donde se realizará la integración
```bash
npm install transbank-pos-sdk-web
```

También se puede incluir directamente el tag script
```html 
<script src="https://unpkg.com/transbank-pos-sdk-web@2/dist/pos.js"></script>
<script>
// En este caso, el objeto en vez de ser POS, es Transbank.POS
// Ej: Transbank.POS.connect(...); en vez de POS.connect(...) como se especifica en los ejemplos de mas abajo. 
</script>
```

**Instalar el agente desktop:** 
Revisa la lista de versiones públicadas y [descarga la última versión](https://github.com/TransbankDevelopers/transbank-pos-sdk-web-agent/releases). Verás que hay un archivo .exe que debes bajar si usas windows, o un archivo .zip si usas MacOS. Luego, solo debes de instalar y ejecutar el programa. 

Este agente debe estar ejecutándose siempre para que el SDK Javascript funcione correctamente. Puedes ejecutarlo automáticamente cuando se inicie el computador. 
La primera vez que se ejecuta el agente, este se configura automáticamente para iniciar en el startup del computador. 

### Drivers

Actualmente contamos con 2 equipos POS Integrado en circulación.

#### Verifone VX520 y VX520C

Este equipo esta certificado para operar mediante conexión serial, utilizando el puerto RS232.
En este caso solo debes tener instalados los drivers de tu adaptador USB-Serial o tu tarjeta RS232.

<aside class="warning">
Existe la posibilidad que puedas utilizar el puerto USB incluido en este equipo, sin embargo Transbank no puede garantizar el correcto funcionamiento de este.
</aside>

<aside class="notice">
La comunicación con el POS Integrado se realiza mediante puerto serial RS232 y tú eres el/la responsable de instalar el driver correcto para tu tarjeta o adaptador serial.
</aside>

<aside class="success">
Estos drivers son conocidos por funcionar con Adaptadores genéricos que utilicen el [chip CH340](http://www.wch.cn/download/CH341SER_EXE.html). También puedes encontrar drivers para adaptadores con [chip Prolific](http://www.prolific.com.tw/US/ShowProduct.aspx?pcid=41&showlevel=0041-0041)
</aside>

#### Ingenico Desk 3500

Estos equipos funcionan tanto con puerto serial RS232 y USB (Generalmente plug and play), para el cual puedes necesitar instalar un [driver de Ingenico](http://transbankdevelopers.cl/files/Ingenico-USB-driver-3.10.zip).

Este driver es compatible con los siguiente sistemas operativos.

- Windows XP
- Windows Vista
- Windows 7
- Windows 2008 Server
- Windows 2008 Server R2
- Windows 8 and 8.1
- Windows XP Embedded
- Windows 2012 Server
- Windows 2016 Server
- Windows 10
  
<aside class="warning">
Ingenico no proporciona drivers para otros sistemas operativos, sin embargo es posible que de todas maneras puedas utilizar el POS en sistemas operativos distintos a los mencionados. En este caso la responsabilidad del correcto funcionamiento es del comercio.
</aside>

Recuerda que necesitas tener instalados los drivers correspondientes a tu tarjeta de
puerto serial o adaptador USB Serial.

<aside class="notice">
La comunicación con el POS Integrado se realiza mediante puerto serial RS232 y tú eres el/la responsable de instalar el driver correcto para tu tarjeta o adaptador serial.
</aside>

<aside class="success">
Estos drivers son conocidos por funcionar con Adaptadores genéricos que utilicen el [chip CH340](http://www.wch.cn/download/CH341SER_EXE.html). También puedes encontrar drivers para adaptadores con [chip Prolific](http://www.prolific.com.tw/US/ShowProduct.aspx?pcid=41&showlevel=0041-0041)
</aside>

### LibSerialPort

Los SDK dependen de [libSerialPort](https://sigrok.org/wiki/Libserialport) para la comunicación serial.

Incluimos una DLL compilada en el [release de la librería en C](https://github.com/TransbankDevelopers/transbank-pos-sdk-c/releases/latest), pero puedes obtener el código desde el repositorio oficial usando git:

```bash
git clone git://sigrok.org/libserialport
```

Para compilar en windows necesitarás lo siguiente:

- [msys2 - mingw-w64](http://www.msys2.org/) Puedes descargarlo siguiendo el link y las instrucciones provistas en su sitio web.
  - Adicionalmente, necesitarás el toolchain para tu arquitectura:
    - x86: `pacman -S mingw-w64-i686-toolchain`
    - x64: `pacman -S mingw-w64-x86_64-toolchain`

<aside class="notice">
Procura seguir todos los pasos descritos en el sitio de msys2
</aside>

## Proyectos de ejemplo
Para cada SDK se creó un proyecto de ejemplo: 

- [Proyecto de ejemplo SDK Web](https://github.com/TransbankDevelopers/transbank-pos-sdk-web-example)
- [Proyecto de ejemplo SDK Java](https://github.com/TransbankDevelopers/transbank-pos-sdk-java-example)
- [Proyecto de ejemplo SDK .NET](https://github.com/TransbankDevelopers/transbank-pos-sdk-dotnet-example)



## Primeros pasos

<div class="pos-title-nav">
  <div tbk-link='/referencia/posintegrado?csharp#primeros-pasos' tbk-link-name='Referencia'></div>
</div>

Para usar el SDK es necesario incluir las siguientes referencias.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Utils;
using Transbank.POS.Responses:
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
```

```java
import cl.transbank.pos.POS;
import cl.transbank.pos.exceptions.*;
import cl.transbank.pos.responses.*;
```

```js
import POS from "transbank-pos-sdk-web"; // Si se instala por NPM
// <script src="https://unpkg.com/transbank-pos-sdk-web@1/dist/pos.js"></script> si se inserta directamente el script en el HTML
// En el caso de incrustar el script, recordar que el objeto se llama Transbank.POS en vez de POS como se menciona en los siguientes ejemplos. 
```


### Listar puertos disponibles

Si los respectivos drivers están instalados, entonces puedes usar la función `ListPorts()` del paquete
`Transbank.POS.Utils`para identificar los puertos que se encuentren disponibles y seleccionar el que
corresponda con el puerto donde conectaste el POS Integrado.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS.Utils;
//...
List<string> ports = Serial.ListPorts();
```

```c
#include "transbank_serial_utils.h"
//...
char *ports = list_ports();
```

```java
import cl.transbank.pos.POS;

POS pos = POS.getInstance();
List<String> ports = pos.listPorts();
```

```js
import POS from "transbank-pos-sdk-web";

POS.getPorts().then((ports) => {
    console.log('ports');
}).catch(() => {
    alert("No se pudo obtener puertos. ¿Está corriendo el servicio Transbank POS?");
})
```

### Abrir un puerto Serial

Para abrir un puerto serial y comunicarte con el POS Integrado, necesitarás el nombre del puerto (el cual puedes identificar usando [la función mencionada en el apartado anterior](/documentacion/posintegrado#listar-puertos-disponibles)). También necesitarás el _baudrate_ al cual esta configurado el puerto serial del POS Integrado (por defecto es 115200), y puedes obtener los distintos valores desde la clase `Transbank.POS.Utils.TbkBaudrates` en .NET y la clase `cl.transbank.pos.utils.TbkBaudRate` en Java.

Si el puerto no puede ser abierto, se lanzará una excepción `TransbankException` en .NET y Java.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Utils;
//...
string portName = "COM3";
POS.Instance.OpenPort(portName);
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
char *portName = "COM4";
int retval = open_port(portName, 115200);
if ( retval == TBK_OK ){
    //...
}
```

```java
import cl.transbank.pos.POS;

POS pos = POS.getInstance();
String port = "COM4";
pos.openPort(port);
```

```js
import POS from "transbank-pos-sdk-web";
POS.openPort("COM4").then((result) => {
    if (result === true) {
	alert("Conectado satisfactoriamente")
    } else {
	alert("No se pudo conectar conectado")
    }
}).catch(error => console.log(error))
```

### Cerrar un puerto Serial

Al finalizar el uso del POS, o si se desea desconectar de la Caja se debe liberar el puerto serial abierto anteriormente.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
//...
bool closed = POS.Instance.ClosePort();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
retval = close_port();
if(retval == SP_OK){
    //...
}
```

```java
import cl.transbank.pos.POS;
//...
pos.closePort();
```

```js
import POS from "transbank-pos-sdk-web";
POS.closePort();
```

## Transacciones

### Transacción de Venta

Este comando es enviado por la caja para solicitar la ejecución de una venta. Los siguientes parámetros deben ser enviados desde la caja:

- `Monto`: Monto en pesos informados al POS. Este parámetro es remitido a Transbank para realizar la autorización.
- `Número Ticket/Boleta`: Este número es impreso por el POS en el voucher que se genera luego de la venta.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
SaleResponse response = POS.Instance.Sale(ammount, ticket);
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
char* response = sale(ammount, ticket, false);
```

```java
import cl.transbank.pos.POS;
//...
SaleResponse saleResponse = POS.getInstance().sale(amount, ticket);
```

```js
import POS from "transbank-pos-sdk-web";

POS.doSale(this.total, "ticket1").then((saleDetails) => {
    console.log(saleDetails);
    //Acá llega la respuesta de la venta. Si saleDetails.responseCode es 0, entonces la comproa fue aprobada
    if (saleDetails.responseCode===0) {
	alert("Transacción aprobada", "", "success");
    } else {
	alert("Transacción rechazada o fallida")
    }
});
```

El resultado de la venta se entrega en la forma de un objeto `SaleResponse` o un `char*` en el caso de la librería C. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankSaleException` en .NET. En Java puede lanzar `TransbankPortNotConfiguredException`.

El objeto SaleResponse retornará un objeto con los siguientes datos.

```json
"Function": 210
"Response": "Aprobado"
"Commerce Code": 550062700310
"Terminal Id": "ABC1234C"
"Ticket": "ABC123"
"Autorization Code": "XZ123456"
"Ammount": 15000
"Shares Number": 3
"Shares Amount": 5000
"Last 4 Digits": 6677
"Operation Number": 60
"Card Type": CR
"Accounting Date":
"Account Number":
"Card Brand": AX
"Real Date": 28/10/2019 22:35:12
"Employee Id":
"Tip": 1500
```

<aside class="warning">
El SDK no soporta el envío de mensajes intermedios. Por esta razón el 3º parámetro de la función en C es siempre falso.
</aside>

### Transacción de Última Venta

Este comando es enviado por la caja, solicitando al POS la re-impresión de la última venta realizada.

Si el POS recibe el comando de Última Venta y no existen transacciones en memoria del POS, se envía la respuesta a la caja indicando el código de respuesta 11.
([Ver tabla de respuestas](/referencia/posintegrado#tabla-de-respuestas))

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
LastSaleResponse response = POS.Instance.LastSale();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
char *lastSaleResponse = last_sale();
}
```

```java
import cl.transbank.pos.POS;
//...
SaleResponse saleResponse = POS.getInstance().getLastSale();
```

```js
import POS from "transbank-pos-sdk-web";

POS.getLastSale().then((response) => {
    console.log(response)
}).catch(() => {
    alert('Error al obtener última venta');
})
```

El resultado de la transacción última venta devuelve los mismos datos que una venta normal y se entrega en forma de un objeto `LastSaleResponse` o un `char*` en el caso de la librería C, o un objeto `SaleResponse` en el caso de Java. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankLastSaleException` en .NET o `TransbankException` en Java.

```json
"Function": 260
"Response": "Aprobado"
"Commerce Code": 550062700310
"Terminal Id": "ABC1234C"
"Ticket": "ABC086"
"Autorization Code": "XZ123456"
"Ammount": 15000
"Shares Number": 3
"Shares Amount": 5000
"Last 4 Digits": 6677
"Operation Number": 60
"Card Type": CR
"Accounting Date":
"Account Number":
"Card Brand": AX
"Real Date": 28/10/2019 22:35:12
"Employee Id":
"Tip": 1500
```

### Transacción de Anulación

Esta transacción siempre será responsabilidad de la caja y es quien decide cuando realizar una anulación.

<aside class="warning">
Las anulaciones <strong>sólo</strong> pueden realizarse para transacciones con tarjeta de crédito y que aún se encuentren en la memoria del POS.
</aside>

El comando de anulación soporta los siguientes parámetros que pueden ser enviados desde la caja.

<aside class="notice">
<strong>Número de operación:</strong> es el correlativo impreso en el voucher de venta. Este número le indicará al POS la transacción en memoria que se desea anular.
</aside>

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
RefundResp response = POS.Instance.Refund(21);
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
RefundResponse response = refund(21);
```

```java
import cl.transbank.pos.POS;
//...
RefundResponse response = POS.getInstance().refund(21);
```

```js
import POS from "transbank-pos-sdk-web";

POS.refund(21).then(response => console.log(response));
```

Como respuesta el **POS** enviará un código de aprobación, acompañado de un código de autorización. En caso de rechazo el código de error está definido en la tabla de respuestas. [Ver tabla de respuestas](/referencia/posintegrado#tabla-de-respuestas)

```json
"FunctionCode": 1210
"ResponseCode": 0
"CommerceCode": 597029414300
"TerminalId": "ABCD1234"
"AuthorizationCode": "ABCD1234"
"OperationID": 123456
"ResponseMessage": "Aprobado"
"Success": true
```

### Transacción de Cierre

Este comando es gatillado por la caja y no recibe parámetros. El POS ejecuta la transacción de cierre contra el Autorizador (no se contempla Batch Upload). Como respuesta el POS Integrado enviará un aprobado o rechazado. (Puedes ver la tabla de respuestas en este [link](/referencia/posintegrado#tabla-de-respuestas))

<aside class="success">
Esta transacción también realiza el cambio de llaves.
</aside>

<aside class="warning">
La transacción de cierre borra todas las transacciones almacenadas en la memoria del POS.
</aside>

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
CloseResponse response = POS.Instance.Close();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
BaseResponse response = register_close();
}
```

```java
import cl.transbank.pos.POS;
//...
CloseResponse cr = POS.getInstance().close();
```

```js
import POS from "transbank-pos-sdk-web";

POS.close()
```

El resultado del cierre de caja se entrega en la forma de un objeto `CloseResponse` o una estructura `BaseResponse` en el caso de la librería C. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankCloseException`.

```json
"FunctionCode": 510
"ResponseMessage": "Aprobado"
"Success": true
"CommerceCode": 550062700310
"TerminalId": "ABC1234C"
```

<aside class="notice">
Para el cierre no se solicitará tarjeta supervisora.
</aside>

### Transacción Totales

Esta operación le permitirá a la caja obtener desde el _POS_ un resumen con el monto total y la cantidad de transacciones que se han realizado hasta el minuto y que aún permanecen en la memoria del _POS_.

Además la caja podrá determinar si existen transacciones que no fueron informadas desde el _POS_,
haciendo una comparación de los totales entre la caja y el _POS_. La impresión del _Voucher_ con el resumen será realizada por el _POS_.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
TotalsResponse response = POS.Instance.Totals();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
TotalsCResponse response = get_totals();
}
```

```java
import cl.transbank.pos.POS;
//...
TotalsResponse response = POS.getInstance().getTotals();
```

```js
import POS from "transbank-pos-sdk-web";

POS.getTotals().then(response => console.log(response));
```

El resultado de la transacción entrega en la forma de un objeto `TotalsResponse` o una estructura `TotalsCResponse` en el caso de la librería C. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankTotalsException`.

```json
"Function": 710
"Response": "Aprobado"
"TX Count": 3     // Cantidad de transacciones
"TX Total": 15000 // Suma total de los montos de cada transaccion
```

### Transacción de Detalle de Ventas

Esta transacción solicita al POS **todas** las transacciones que se han realizado y permanecen en la memoria del POS. El parámetro que recibe esta función es de tipo booleano e indica si se realiza la impresión del detalle en el POS. En el caso de que no se solicite la impresión, el POS envía **todas** las transacciones a la caja, una por una. Si se realiza la impresión, la caja recibira una lista vacia de transacciónes.

<aside class="warning">
Una transacción de cierre vacía la memoria del POS
</aside>

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
bool printOnPOS = false;
List<DetailResponse> Details(printOnPOS)
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
bool print_on_pos = false;
char *response = sales_detail(print_on_pos);
}
```

```java
import cl.transbank.pos.POS;
//...
List<DetailResponse> ldr = POS.getInstance().details(false);
```

```js
import POS from "transbank-pos-sdk-web";

let printOnPOS = false;
POS.details(printOnPOS).then(response => console.log(response));
```


El resultado de la transacción entrega una lista de objetos  `DetailResponse` o un `char *` en el caso de la librería C. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankSalesDetailException`.

```json
[
  {
    "Function": 261,
    "Response": "Aprobado",
    "Commerce Code": 550062700310,
    "Terminal Id": "ABC1234C",
    "Ticket": "AB123",
    "Autorization Code": "XZ123456",
    "Ammount": 15000,
    "Shares Number": 3,
    "Shares Amount": 5000,
    "Last 4 Digits": 6677,
    "Operation Number": 60,
    "Card Type": CR,
    "Accounting Date": ,
    "Account Number": ,
    "Card Brand": AX,
    "Real Date": "28/10/2019 22:35:12",
    "Employee Id": ,
    "Tip": 1500,
  },
  {
    "Function": 261,
    "Response": "Aprobado",
    "Commerce Code": 550062700310,
    "Terminal Id": "ABC1234C",
    "Ticket": "AB123",
    "Autorization Code": "XZ123456",
    "Ammount": 15000,
    "Shares Number": 3,
    "Shares Amount": 5000,
    "Last 4 Digits": 6677,
    "Operation Number": 60,
    "Card Type": CR,
    "Accounting Date": ,
    "Account Number": ,
    "Card Brand": AX,
    "Real Date": "28/10/2019 22:35:12",
    "Employee Id": ,
    "Tip": 1500
  }
]
```

### Transacción de Carga de Llaves

Esta transacción permite al POS Integrado del comercio requerir cargar nuevas _Working Keys_ desde Transbank. Como respuesta el POS Integrado enviará un aprobado o rechazado. (Puedes ver la tabla de respuestas en este [link](/referencia/posintegrado#tabla-de-respuestas))

<aside class="success">
Las llaves se deben cambiar automáticamente todos los días. Puedes usar este método como parte de un procedimiento de inicialización que se ejecute en forma automática todos los días. Ten presente que la transacción de Cierre también realiza el cambió de llaves.
</aside>

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
LoadKeysResponse response = POS.Instance.LoadKeys();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
BaseResponse response = load_keys();
}
```

```java
import cl.transbank.pos.POS;
//...
KeysResponse kr = POS.getInstance().loadKeys();
```

```js
import POS from "transbank-pos-sdk-web";

let printOnPOS = false;
POS.loadKeys();
```




El resultado de la carga de llaves entrega en la forma de un objeto `LoadKeysResponse` o una estructura `BaseResponse` en el caso de la librería C, un objeto `KeysResponse` para Java. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankLoadKeysException` en .NET o `TransbankException` en Java.

```json
"FunctionCode": 810
"ResponseMessage": "Aprobado"
"Success": true
"CommerceCode": 550062700310
"TerminalId": "ABC1234C"
```

<aside class="warning">
El uso de esta transacción debe ser limitado a pruebas de comunicación o cuando el POS Integrado pierda las llaves.
</aside>

### Transacción de Poll

Esta mensaje es enviado por la caja para saber si el POS está conectado. En el SDK el resultado de esta operación es un `Booleano` o un `0` representado en la constante `TBK_OK` en el caso de la librería en C. Si ocurre algún error al momento de ejecutar la acción en el POS, se lanzará una excepción del tipo `TransbankException`.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
//...
bool connected = POS.Instance.Poll();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
int retval = poll();
if (retval == TBK_OK){
    //...
}
```

```java
import cl.transbank.pos.POS;
//...
boolean pollResult = POS.getInstance().poll();
```


```js
import POS from "transbank-pos-sdk-web";

let printOnPOS = false;
POS.poll().then(result => console.log(result));
```

### Transacción de Cambio a POS Normal

Este comando le permitirá a la caja realizar el cambio de modalidad a través de un comando. El POS debe estar en modo integrado y al recibir el comando quedara en modo normal. El resultado de esta operación es un `Booleano` en el caso del SDK o un `0` representado en la constante `TBK_OK` en el caso de la librería en C. Si ocurre algún error al momento de ejecutar la acción en el POS, se lanzará una excepción del tipo `TransbankException`.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
//...
bool connected = POS.Instance.SetNormalMode();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
int retval = set_normal_mode();
if (retval == TBK_OK){
    //...
}
```

```java
import cl.transbank.pos.POS;
//...
boolean normal = POS.getInstance().setNormalMode();
```

```js
import POS from "transbank-pos-sdk-web";

let printOnPOS = false;
POS.setNormalMode().then(result => console.log(result));
```

<aside class="notice">
Si el POS Integrado se cambia a modo normal, debe ser configurado nuevamente en el POS para regresar al modo Integrado, siguiendo las instrucciones disponibles descritas en [Cambio a POS Integrado](referencia/posintegrado#cambio-modalidad-pos-integrado), pues no es posible realizar esta configuración a través del SDK.
</aside>

## Integración de Onepay para pagos con QR

Ahora es posible realizar pagos con Onepay dentro de tu sistema de caja. Para esto, ponemos a tu disposición, dentro del SDK, las herramientas necesarias para generar un QR de Onepay y procesar el pago en tu sistema de caja.

Lo primero que debes hacer es importar `Transbank.POS` y `Transbank.POS.Model` en tu proyecto, crear un nuevo objeto `OnepayPayment` y suscribirte a los eventos que te notificaran del progreso del pago en el flujo de Onepay.

### Suscribirse a eventos y crear una transacción

```csharp
using Transbank.POS;
using Transbank.POS.Model;
//...

int ticket = new Random().Next(1, 999999);
Pay = new OnepayPayment(ticket, total);

//Subscribe to events
Pay.OnConnect += (sender, e) => UpdateWSStatus("Web Socket Conectado");
Pay.OnDisconnect += WsDisconnected;
Pay.OnNewMessage += UpdateStatus;
Pay.OnSuccessfulPayment += ProcessPaymentResult;
```

### Obtener el QR para iniciar el pago

Luego de crear el objeto OnepayPayment, debes iniciar el pago, lo que te entregará la imagen del QR en base64 y el número de `ott`. Estos le permiten al usuario pagar escaneando la imagen o ingresando el `ott` en la aplicación de Onepay.

```csharp
using Transbank.POS;
using Transbank.POS.Model;
//...

OnepayCreateResponse response = Pay.StarPayment();

//draw the base64 image
byte[] imageBytes = Convert.FromBase64String(response.QrCodeAsBase64);
using (MemoryStream ms = new MemoryStream(imageBytes, 0, imageBytes.Length))
{
  img_qr.Image = Image.FromStream(ms);
}
txt_ott.Text = response.Ott;
```

### Esperar que el usuario termine el pago en la App de Onepay

El último paso es esperar a que el usuario termine el flujo en la app de Onepay. Cuando la transacción termine exitosamente en Onepay, se lanzará el evento `SuccessfulPaymentEventArgs`

```csharp
using Transbank.POS;
using Transbank.POS.Model;
//...
Pay.WatchPayment();
```

## Ejemplos de integración

Ponemos a tu disposición un ejemplo en nuestro Github para ayudarte a entender mejor la integración.

- [Ejemplo Windows Forms](https://github.com/TransbankDevelopers/transbank-pos-sdk-dotnet-example)
- [Ejemplo Java]([TransbankDevelopers/transbank-pos-sdk-java-example](https://github.com/TransbankDevelopers/transbank-pos-sdk-java-example))

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
