# POS Autoservicio

La solución Autoservicio se integra al kiosco de tu comercio, con la particularidad de que no necesita la intervención
de un dependiente de tu establecimiento, sino que permite que los mismos clientes realicen la transacción de pago de
manera completa y autónoma. Es ideal para negocios como estacionamientos, bencineras y cines, entre otros.

## Cómo funciona

El paso a paso para su implementación:

1. **Auditoría de factibilidad técnica**
Establecerá si tu comercio tiene las facultades para realizar el desarrollo tecnológico que requiere esta
integración. En caso de aprobación, se te entregará un Documento Técnico con las indicaciones a seguir.

2. **Desarrollo**
Tu comercio deberá realizar el desarrollo según las especificaciones que te entregará Transbank.

3. **Control de Calidad**
Este proceso verificará que el desarrollo de integración que hiciste,
cumple con los que te entregamos en las especificaciones, o si requiere alguna corrección o mejora.

4. **Etapa piloto**
En un lugar que acordemos juntos, llevamos a producción tu punto de venta autoservicio, donde con clientes reales haremos monitoreo en conjunto de su funcionamiento, por un periódo acotado de 2 semanas. Evaluamos los resultados, y acordamos masificación, también si se requiere algún ajuste.

5. **Masificación**
Construimos en conjunto un plan de instalación de los puntos de ventas.

<aside class="notice">
Para el desarrollo y pruebas del comercio, Transbank entrega un set de pruebas que se deben ejecutar y un set de tarjetas físicas para estos casos, estas tarjetas solo funcionan en ambiente de desarrollo, certificación o integración.
</aside>

**Flujo de venta en POS autoservicio:**

1. El cliente realiza la selección del producto o servicio a comprar.
2. El cliente selecciona la forma de pago que disponga el kiosko, efectivo, tarjeta de crédito o tarjeta de débito.
3. El kiosco invoca al POS pasándole el monto a cobrar.
4. El cliente opera la tarjeta, selecciona cuotas si corresponde, e ingresa su pin.
5. El POS informa al kiosco el resultado de la venta (aprobada o rechazada).
6. El kiosko libera el producto e imprime los comprobantes.

## Equipos y conexiones disponibles

### Verifone ux100, ux400, ux300

POS autoservicio con conexión LAN y Serial
<img src="/images/documentacion/autoatencion/ux100completo.png" alt="Verifone ux100, ux300 y ux400">

## Funciones de pago disponibles en HOST

* Venta Crédito, con o sin cuotas
* Venta Débito

## Cómo empezar

Por el momento, hay un SDK para [.NET](https://github.com/TransbankDevelopers/transbank-pos-sdk-dotnet).

### SDK .NET

Para .NET lo puedes encontrar en [NuGet.org](https://www.nuget.org/packages/TransbankPosSDK/). Para instalarlo puedes utilizar por ejemplo el package manager de VisualStudio.

```bash
PM> Install-Package TransbankPosSDK
```
### Integración Nativa

Es recomendable utilizar un SDK disponible a la hora de desarrollar la integración, lo que ahorra tiempo y te despreocupa de desarrollar las comunicaciones con el equipo POS Autoservicio, facilitando bastante la integración, pero en el caso que prefieras realizar la integración por tu cuenta y utilizar los comandos nativos, puedes revisarlos en el manual de integración disponible en la sección de [documentación](/documentacion/pos-autoservicio#documentacion-disponible).

## Primeros pasos

<!-- <div class="pos-title-nav">
  <div tbk-link='/referencia/pos-autoservicio?csharp#primeros-pasos' tbk-link-name='Referencia'></div>
</div> -->

Para usar el SDK es necesario incluir las siguientes referencias.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
using Transbank.Responses.CommonResponses;
using Transbank.Responses.AutoservicioResponse;
```

### Listar puertos disponibles

Si los respectivos drivers están instalados, entonces puedes usar la función `ListPorts()` para identificar los puertos que se encuentren disponibles y seleccionar el que
corresponda con el puerto donde conectaste el POS Autoservicio.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
//...
List<string> ports = POSAutoservicio.Instance.ListPorts();
```

### Abrir un puerto Serial

Para abrir un puerto serial y comunicarte con el POS Autoservicio, necesitarás el nombre del puerto (el cual puedes identificar usando [la función mencionada en el apartado anterior](/documentacion/pos-autoservicio#listar-puertos-disponibles)). También necesitarás el _baudrate_ al cual esta configurado el puerto serial del POS Autoservicio (por defecto es **9600**).

Si el puerto no puede ser abierto, se lanzará una excepción `TransbankException` en .NET y Java.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
//...
string portName = "COM3";
POSAutoservicio.Instance.OpenPort(portName);
```

### Cerrar un puerto Serial

Al finalizar el uso del POS, o si se desea desconectar de la Caja se debe liberar el puerto serial abierto anteriormente.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
//...
POSAutoservicio.Instance.ClosePort();
```

## Transacciones

### Transacción de Venta

Este comando es enviado por la caja para solicitar la ejecución de una venta. Los siguientes parámetros deben ser enviados desde la caja:

* `Monto`: Monto en pesos informados al POS. Este parámetro es remitido a Transbank para realizar la autorización.
* `Número Ticket/Boleta`: Este número es entregado por el POS en la respuesta o en el voucher que se genera luego de la venta.
* `Enviar voucher`: (Opcional) Indica si el POS al finalizar la transacción envía el voucher formateado en la respuesta (verdadero) o se omite (falso, por defecto).
* `Enviar Status`: (Opcional) Indica si se envían los mensajes intermedios (verdadero) o se omiten (falso, por defecto).

En el caso de C#, los mensajes intermedios se reciben mediante el evento `IntermediateResponseChange` y el argumento retornado es de tipo `IntermediateResponse`.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
using Transbank.Responses.CommonResponses;
using Transbank.Responses.AutoservicioResponse;
//...

POSAutoservicio.Instance.IntermediateResponseChange += NewIntermadiateMessageRecived; //EventHandler para los mensajes intermedios.
Task<SaleResponse> response = POSAutoservicio.Instance.Sale(ammount, ticket, true, true);

//...
//Manejador de mensajes intermedios...
private static void NewIntermadiateMessageRecived(object sender, IntermediateResponse e){
  //...
}
//...
```

El resultado de la venta se entrega en la forma de un objeto `SaleResponse>`. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankSaleException` en .NET.

El objeto SaleResponse retornará un objeto con los siguientes datos:

```json
{
  "Function": 210,
  "ResponseCode": 0,
  "ResponseMessage": "Aprobado",
  "Commerce Code": 550062700310,
  "Terminal Id": "ABC1234C",
  "Ticket": "ABC123",
  "Autorization Code": "XZ123456",
  "Ammount": 15000,
  "Last 4 Digits": 6677,
  "Operation Number": 60,
  "Card Type": "CR",
  "Accounting Date": "28/10/2019 22:35:12",
  "Account Number": "30000000000",
  "Card Brand": "AX",
  "Real Date": "28/10/2019 22:35:12",
  "Printing Field:": List<string>,
  "Shares Type": 3,
  "Shares Number": 3,
  "Shares Amount": 5000,
  "Shares Type Gloss": " "
}
```

<aside class="notice">
Si se solicita que el POS envíe en la respuesta el voucher formateado, se entregará una lista de strings que contendrá cada línea del voucher.
</aside>

<aside class="warning">
No se permite realizar transacciones que requieran firma ni operar tarjetas no bancarias.
</aside>

### Transacción de Venta Multicódigo

Este comando es enviado por la caja para solicitar la ejecución de una venta multicódigo. Los siguientes parámetros deben ser enviados desde la caja:

* `Monto`: Monto en pesos informados al POS. Este parámetro es remitido a Transbank para realizar la autorización.
* `Número Ticket/Boleta`: Este número es impreso por el POS en el voucher que se genera luego de la venta.
* `CodigoDeComercio`: Código de comercio que realiza la venta. (No es el mismo código del POS, ya que en multicódigo el código padre no puede realizar ventas.)
* `Enviar voucher`: (Opcional) Indica si el POS al finalizar la transacción envía el voucher formateado en la respuesta (verdadero) o se omite (falso, por defecto).
* `Enviar Status`: (Opcional) Indica si se envían los mensajes intermedios (verdadero) o se omiten (falso, por defecto).

En el caso de C#, los mensajes intermedios se reciben mediante el evento `IntermediateResponseChange` y el argumento retornado es de tipo `IntermediateResponse`.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
using Transbank.Responses.CommonResponses;
using Transbank.Responses.AutoservicioResponse;
//...

POSAutoservicio.Instance.IntermediateResponseChange += NewIntermadiateMessageRecived; //EventHandler para los mensajes intermedios.
Task<MultiCodeSaleResponse> response = POSAutoservicio.Instance.MultiCodeSale(ammount, ticket, commerceCode, true, true);

//...
//Manejador de mensajes intermedios...
private static void NewIntermadiateMessageRecived(object sender, IntermediateResponse e){
  //...
}
//...
```

El resultado de la venta se entrega en la forma de un objeto `MultiCodeSaleResponse` Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankSaleException` en .NET.

El objeto MultiCodeSaleResponse retornará un objeto con los siguientes datos:

```json
{
  "Function": 271,
  "ResponseCode": 0,
  "ResponseMessage": "Aprobado",
  "Commerce Code": 550062700310,
  "Terminal Id": "ABC1234C",
  "Ticket": "ABC123",
  "Autorization Code": "XZ123456",
  "Ammount": 15000,
  "Last 4 Digits": 6677,
  "Operation Number": 60,
  "Card Type": "CR",
  "Accounting Date": "28/10/2019 22:35:12",
  "Account Number": "30000000000",
  "Card Brand": "AX",
  "Real Date": "28/10/2019 22:35:12",
  "CommerceProviderCode:": 550062712310,
  "Printing Field:": List<string>,
  "Shares Type": 3,
  "Shares Number": 3,
  "Shares Amount": 5000,
  "Shares Type Gloss": " "
}
```

<aside class="notice">
Si se solicita que el POS envie en la respuesta el voucher formateado, se entregará una lista de strings que contendrá cada línea del voucher.
</aside>

<aside class="warning">
No se permite realizar transacciones que requieran firma ni operar tarjetas no bancarias.
</aside>

### Transacción de última venta

Este comando es enviado por la caja, solicitando al POS los datos de la última venta realizada.

Si el POS recibe el comando de Última Venta y no existen transacciones en memoria del POS, se envía la respuesta a la caja indicando el código de respuesta 11. 
<!-- ([Ver tabla de respuestas](/referencia/pos-autoservicio#tabla-de-respuestas)) -->
El siguiente parámetro debe ser enviado desde la caja:

* `Enviar voucher`: (Opcional) Indica si el POS al finalizar la transacción envía el voucher formateado en la respuesta (verdadero) o se omite (falso, por defecto).

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
using Transbank.Responses.AutoservicioResponse;
//...
Task<LastSaleResponse> response = POSAutoservicio.Instance.LastSale();
```

El resultado de la transacción última venta devuelve los mismos datos que una venta normal y se entrega en forma de un objeto `LastSaleResponse`. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankLastSaleException` en .NET.

El objeto LastSaleResponse retornará un objeto con los siguientes datos:

```json
{
  "Function": 260,
  "ResponseCode": 0,
  "ResponseMessage": "Aprobado",
  "Commerce Code": 550062700310,
  "Terminal Id": "ABC1234C",
  "Ticket": "ABC123",
  "Autorization Code": "XZ123456",
  "Ammount": 15000,
  "Last 4 Digits": 6677,
  "Operation Number": 60,
  "Card Type": "CR",
  "Accounting Date": "28/10/2019 22:35:12",
  "Account Number": "30000000000",
  "Card Brand": "AX",
  "Real Date": "28/10/2019 22:35:12",
  "Printing Field:": List<string>,
  "Shares Type": 3,
  "Shares Number": 3,
  "Shares Amount": 5000,
  "Shares Type Gloss": " "
}
```

<aside class="notice">
Si se solicita que el POS envíe en la respuesta el voucher formateado, se entregará una lista de strings que contendrá cada línea del voucher.
</aside>

### Transacción de Anulación

Esta transacción siempre será responsabilidad de la caja y es quien decide cuándo realizar una anulación.

<aside class="warning">
Las anulaciones <strong>solo</strong> pueden realizarse para transacciones con tarjeta de crédito y considerando que solo puede ser anulada la última transacción.
</aside>

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
using Transbank.Responses.CommonResponses;
//...
Task<RefundResponse> response = POSAutoservicio.Instance.Refund();
```

Como respuesta el **POS** enviará un código de aprobación, acompañado de un código de autorización. En caso de rechazo el código de error está definido en la tabla de respuestas del manual de integración POS Autoservicio. <!-- [Ver tabla de respuestas](/referencia/pos-autoservicio#tabla-de-respuestas) -->

El resultado de la anulación se entrega en la forma de un objeto `RefundResponse` Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankRefundException` en .NET.

El objeto RefundResponse retornará un objeto con los siguientes datos:

```json
{
  "FunctionCode": 1210,
  "ResponseCode": 0,
  "ResponseMessage": "Aprobado",
  "CommerceCode": 597029414300,
  "TerminalId": "ABCD1234",
  "AuthorizationCode": "ABCD1234",
  "OperationID": 123456
}
```

### Transacción de Cierre

Este comando es gatillado por la caja. El POS ejecuta la transacción de cierre contra el Autorizador (no se contempla Batch Upload). Como respuesta, el POS Autoservicio enviará un aprobado o rechazado. <!-- (Puedes ver la tabla de respuestas en este [link](/referencia/pos-autoservicio#tabla-de-respuestas)) -->

El siguiente parámetro debe ser enviado desde la caja:

* `Enviar voucher`: (Opcional) Indica si el POS al finalizar la transacción envía el voucher formateado en la respuesta (verdadero) o se omite (falso, por defecto).

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
using Transbank.Responses.AutoservicioResponse;
//...
Task<CloseResponse> response = POSAutoservicio.Instance.Close();
```

El resultado del cierre de caja se entrega en la forma de un objeto `CloseResponse`. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankCloseException` en .NET.

El objeto CloseResponse retornará un objeto con los siguientes datos:

```json
{
  "FunctionCode": 510,
  "ResponseCode": 0,
  "ResponseMessage": "Aprobado",
  "Success": true,
  "CommerceCode": 550062700310,
  "TerminalId": "ABC1234C",
  "Printing Field:": List<string>
}
```

<aside class="notice">
Si se solicita que el POS envíe en la respuesta el voucher formateado, se entregará una lista de strings que contendrá cada línea del voucher.
</aside>

<aside class="notice">
Para el cierre no se solicitará tarjeta supervisora.
</aside>

### Transacción de Carga de Llaves

Esta transacción permite al POS Autoservicio del comercio requerir cargar nuevas _Working Keys_ desde Transbank. Como respuesta el POS Autoservicio enviará un aprobado o rechazado. <!-- (Puedes ver la tabla de respuestas en este [link](/referencia/pos-autoservicio#tabla-de-respuestas)) -->

<aside class="success">
Su uso debe ser limitado como prueba de comunicación IP para validar conectividad hacia el exterior.
</aside>

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
using Transbank.Responses.CommonResponses;
//...
Task<LoadKeysResponse> response = POSAutoservicio.Instance.LoadKeys();
```

El resultado de la carga de llaves se entrega en la forma de un objeto `LoadKeysResponse`. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankLoadKeysException` en .NET.

El objeto LoadKeysResponse retornará un objeto con los siguientes datos:

```json
{
  "FunctionCode": 810,
  "ResponseCode": 0,
  "ResponseMessage": "Aprobado",
  "Success": true,
  "CommerceCode": 550062700310,
  "TerminalId": "ABC1234C"
}
```

### Transacción de Poll

Esta mensaje es enviado por la caja para saber si el POS está conectado. En el SDK el resultado de esta operación es un `Booleano`. Si ocurre algún error al momento de ejecutar la acción en el POS, se lanzará una excepción del tipo `TransbankException`.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
//...
Task<bool> connected = POSAutoservicio.Instance.Poll();
```

### Transacción de Inicialización

Este mensaje es enviado por la caja para que el POS autoservicio pueda cargar los parámetros y el aplicativo. En el SDK el resultado de esta operación es un `Booleano`. Si ocurre algún error al momento de ejecutar la acción en el POS, se lanzará una excepción del tipo `TransbankException` en .NET.

Debido a que la transacción de Inicialización tiene un tiempo superior a una venta normal y el tiempo en que el POS autoservicio queda fuera de comunicación con la caja es variable, se dividió en 2 comandos:
- Transacción de Inicialización: Realiza la operación de inicialización.
- Respuesta de Inicialización: Se utiliza para conocer el resultado de la operación de inicialización.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
//...
Task<bool> initialized = POSAutoservicio.Instance.Initialization();
```

<aside class="notice">
La operación de inicialización es usada por los técnicos al realizar la instalación de los equipos en el comercio. Previo a la ejecución de esta transacción, es necesario realizar una transacción de cierre.
</aside>

### Transacción de Respuesta Inicialización

Esta mensaje es enviado por la caja para conocer el resultado de la última operación de inicialización realizada por el POS autoservicio.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
using Transbank.Responses.AutoservicioResponse;
//...
Task<InitializationResponse> response = POSAutoservicio.Instance.InitializationResponse();
```

El resultado de la inicialización se entrega en la forma de un objeto `InitializationResponse`. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankInitializationResponseException` en .NET.

El objeto InitializationResponse retornará un objeto con los siguientes datos:

```json
{
  "FunctionCode": 1080,
  "ResponseCode": 0,
  "ResponseMessage": "Aprobado",
  "Success": true,
  "Real Date": "28/10/2019 22:35:12"
}
```

## Voucher

Los voucher serán generados por el POS Autoservicio cuando el párametro `Enviar voucher` sea verdadero, el voucher puede ser retornado en la respuesta de las transacciones de venta, venta multicódigo, última venta, anulación y cierre.

Cada línea contendrá 40 caracteres, los que se concatenarán en un solo buffer que será enviado en el campo de impresión en las respuesta de las transacciones mencionadas anteriormente. Al recibir el buffer se debe considerar que cada 40 caracteres se compone una línea del voucher.

En el SDK de .NET se entregará una lista con cada línea del voucher.

```json
[
  "       REPORTE DE CIERRE DEL TERMINAL   ",
  "           OPERACIONES TRANSBANK        ",
  "           CALLE HUERFANOS 770 10       ",
  "                SANTIAGO                ",
  "            597029983518-V18.1A2        ",
  "FECHA            HORA           TERMINAL",
  "22/04/21        14:14:56        70000933",
  "                                        ",
  "                 NUMERO            TOTAL",
  "DEBITO             003           $ 3.000",
  "MAESTRO            001           $ 2.000",
  "VISA               010         $ 111.100",
  "MASTERCARD         004          $ 80.000",
  "AMEX               000               $ 0",
  "MAGNA              000               $ 0",
  "DINERS             002             $ 500",
  "----------------------------------------",
  "TOTAL CAPTURAS     020         $ 196.600"
]
```

<aside class="notice">
La cantidad de líneas del voucher dependerá del tipo de transacción.
</aside>

## Documentación disponible

A continuación, encontrarás la documentación en formato PDF:

* **Manual de integración POS Autoservicio UX100/300/400** - _20.1_ | [Descargar](/files/manual-integracion-pos-autoservicio-20-1.pdf)
_Este documento tiene por objetivo explicar las funcionalidades que debe implementar el
Cliente o su proveedor de caja para el desarrollo del módulo de autoservicio (en este caso los
ejemplos se efectuarán con el teclado Verifone UX100, el lector de tarjeta Verifone UX300 y el
lector de tarjetas Contactless Verifone UX400 (pago sin contacto)), permitiendo realizar
transacciones bancarias de Crédito o Débito (redcompra)_

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
