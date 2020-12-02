# PatPass

## Ambientes y Credenciales

### Ambiente de Producción

```java
Configuration configuration = new Configuration();
configuration.setEnvironment(Webpay.Environment.LIVE);
```

```php
PENDIENTE
```

```csharp
var configuration = new Configuration();
configuration.Environment = "PRODUCCION";
```

Las URLs de endpoint de producción están alojadas dentro de
<https://webpay3g.transbank.cl/.>

> Los SDKs traen pre-configurado los certificados de Transbank y validan
> automáticamente las respuestas. Sólo debes asegurarte de mantener tu SDK
> actualizado para evitar que los certificados preconfigurados expiren.
> O también puedes sobre-escribir manualmente el certificado de Webpay a usar
> para la validación:

```java
configuration.setWebpayCert(
    "-----BEGIN CERTIFICATE-----\n" +
    "MIIEDzCCAvegAwIBAgIJAMaH4DFTKdnJMA0GCSqGSIb3DQEBCwUAMIGdMQswCQYD\n" +
    "VQQGEwJDTDERMA8GA1UECAwIU2FudGlhZ28xETAPBgNVBAcMCFNhbnRpYWdvMRcw\n" +
    ...
    "MX5lzVXafBH/sPd545fBH2J3xAY3jtP764G4M8JayOFzGB0=\n" +
    "-----END CERTIFICATE-----\n");
```

```php
PENDIENTE
```

```csharp
configuration.WebpayCertPath = @"C:\Certs\certificado-publico-transbank.crt"
```

Para validar las respuestas generadas por Transbank debes usar un certificado público de Webpay. En [el repositorio GitHub
`transbank-webpay-credenciales`](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/)
podrás encontrar [el certificado
actualizado](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/tree/master/produccion).

### Ambiente de Integración

```java
configuration.setEnvironment(Webpay.Environment.TEST);
```

```php
PENDIENTE
```

```csharp
configuration.Environment = "INTEGRACION";
```

Las URLs de endpoints de integración están alojados dentro de
<https://webpay3gint.transbank.cl/>.

Consulta [la documentación para conocer las tarjetas de prueba que funcionan en
el ambiente de integración](/documentacion/como_empezar#ambientes).

### Credenciales del Comercio

```java
configuration.setCommerceCode("597044444432");
configuration.setPrivateKey(
    "-----BEGIN RSA PRIVATE KEY-----\n" +
    ... +
    "-----END RSA PRIVATE KEY-----\n"
);
configuration.setPublicCert(
    "-----BEGIN CERTIFICATE-----\n" +
    ... +
    "-----END CERTIFICATE-----\n"
);
```

```php
PENDIENTE
```

```csharp
configuration.CommerceCode = "597044444432";
configuration.PrivateCertPfxPath = @"C:/Path/to/private/Cert.pfx";
configuration.Password = "PfxPassword";
```

Todas las peticiones que hagas deben estar firmadas con tu llave privada y
certificado enviado a Transbank. Dichas credenciales deben coincidir con el
código de comercio usado en cada petición.

<aside class="notice">
Ten en cuenta que tu(s) código(s) de comercio en ambiente de producción no son
iguales a los entregados para el ambiente de integración.
</aside>

<aside class="warning">
A diferencia de otros SDK, en .NET debes especificar la ruta a un archivo pfx o p12
el cual debes generar tu a partir de tu llave privada y certificado público.

Puedes mirar el siguiente enlace para obtener una guía rápida de como generar tu propio archivo: [Crear archivo pfx usando openssl](https://www.ssl.com/how-to/create-a-pfx-p12-certificate-file-using-openssl/)
</aside>

Consulta [la documentación para generar una llave privada y un certificado
usando openssl](/documentacion/como_empezar#credenciales-en-webpay si no sabes aún como realizarlo.

En [el repositorio GitHub
`transbank-webpay-credenciales`](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/)
podrás encontrar [códigos de comercios y certificados actualizados para probar
en integración aunque aún no tengas tu propio código de
comercio](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/tree/master/integracion).

> Los SDKs ya incluyen esos códigos de comercio, certificados y llaves privadas
> que funcionan en el ambiente de integración, por lo que puedes obtener
> rápidamente una configuración lista para hacer tus primeras pruebas en dicho
> ambiente:

```java
Configuration configuration =
    Configuration.forTestingPatPassByWebpayNormal("test@mail.co");
```

```php
PENDIENTE
```

```csharp
var configuration = Configuration.ForTestingPatPassByWebpayNormal("test@mail.co");
```

## PatPass by Webpay Normal

```java
PatPassByWebpayNormal transaction =
    new Webpay(configuration).getPatPassByWebpayTransaction();
```

```php
PENDIENTE
```

```csharp
var transaction = new Webpay(configuration).PatPassByWebpayTransaction;
```

WSDL: `/WSWebpayTransaction/cxf/WSWebpayService?wsdl`

Una transacción de autorización de PatPass by WebPay corresponde a una solicitud de inscripción de pago recurrente con tarjetas de crédito, en donde el primer pago se resuelve al instante, y los subsiguientes quedan programados para ser ejecutados mes a mes. PatPass by WebPay cuenta con fecha de caducidad o termino, la cual debe ser proporcionada junto a otros datos para esta transacción. La transacción puede ser realizada en pesos y es posible enviar el monto en UF. WebPay realizará la conversión a pesos al momento de realizar el cargo al tarjetahabiente.

El flujo de pago en PatPass by WebPay se inicia desde el comercio, en donde el Tarjetahabiente selecciona el producto o servicio. Una vez realizado esto, elige pagar con PatPass by WebPay, en donde se despliega el formulario de pago y se completan los datos requeridos, dando paso al proceso de autenticación del Tarjetahabiente con el objetivo de validar que la tarjeta este siendo utilizada por el titular. Una vez resuelta la autenticación se procede a autorizar el pago y si esta es aprobada, se emite un comprobante electrónico de pago y el sistema procede a generar la inscripción encargada de la recurrencia mensual.

Dentro de los atributos más relevantes de WebPay se pueden mencionar:

* Permite realizar transacciones seguras y en línea a través de Internet.
* En transacciones con WebPay Plus se solicita al tarjetahabiente autenticarse con su emisor, protegiendo de esta forma al comercio por eventuales fraudes o desconocimientos de compra.
* La seguridad es reforzada por medio de la utilización de servidores seguros, protegidos con TLS 1.2
* Firma digital.

### Flujo en caso de éxito

De cara al tarjetahabiente, el flujo de páginas para la transacción es el siguiente:

![Flujo de páginas PatPass by Webpay Normal](/images/flujo-paginas-webpay.png)

Desde el punto de vista técnico, la secuencia es la siguiente:

![Diagrama de secuencia PatPass by Webpay Normal](/images/diagrama-secuencia-webpay.png)

1. Una vez seleccionado los bienes o servicios, tarjetahabiente decide pagar a través de PatPass by WebPay.
2. El comercio inicia una transacción en Webpay, invocando el método `initTransaction()`.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el token de la transacción y URL de redireccionamiento a la cual se deberá redirigir al tarjetahabiente.
4. Comercio redirecciona al tarjetahabiente hacia Webpay, con el token de la transacción a la URL indicada en punto 3. La redirección se realiza enviando por método POST el token en variable `token_ws`.
5. El navegador Web del tarjetahabiente realiza una petición HTTPS a Webpay, en base al redireccionamiento generado por el comercio en el punto 4.
6. Webpay responde al requerimiento desplegando el formulario de pago de Webpay. Desde este punto la comunicación es entre Webpay y el tarjetahabiente, sin interferir el comercio. El formulario de pago de PatPass by Webpay despliega, entre otras cosas, el monto de la transacción, fecha de término de suscripción, información del comercio como nombre y logotipo, y la opción de pago a través de crédito.
7. Tarjetahabiente ingresa los datos de la tarjeta, hace clic en pagar.
8. WebPay procesa la solicitud de autorización (primero autenticación bancaria y luego la autorización de la transacción), y si todo resulta exitoso, realiza el proceso de inscripción de PatPass by WebPay.
9. Una vez resuelta la autorización, WebPay retorna el control al comercio, realizando un redireccionamiento HTTPS hacia la página de transición del comercio, en donde se envía por método POST el token de la transacción en la variable `token_ ws`. El comercio debe implementar la recepción de esta variable.
10. El navegador Web del tarjetahabiente realiza una petición HTTPS al sitio del comercio, en base a la redirección generada por WebPay en el punto 9.
11. El sitio del comercio recibe la variable `token_ws` e invoca el segundo método Web, `getTransactionResult()` (mientras se despliega la página de transición), para obtener el resultado de la autorización. Se recomienda que el resultado de la autorización sea persistida en los sistemas del comercio, ya que este método se puede invocar una única vez por transacción.
12. Comercio recibe el resultado de la invocación del método `getTransactionResult()`.
13. Para que el comercio informe a WebPay que el resultado de la transacción se ha recibido sin problemas, el sistema del comercio debe consumir el tercer método `acknowledgeTrasaction()`. Si esto fue ejecutado correctamente el producto puede ser liberado al cliente.

    >    Los SDKs consumen `acknowledgeTransaction()` de manera automática, tan pronto
    >    reciben una respuesta de `getTransactionResult()`.

    <aside class="warning">
      De no ser consumido `acknowledgeTransaction()` o demorar más de 30 segundos en
      su consumo, Webpay realizará la reversa de la transacción, asumiendo que
      existieron problemas de comunicación. En este caso el método retorna una
      Excepción indicando la situación. Esta excepción debe ser manejada para no
      entregar el producto o servicio en caso que ocurra.
    </aside>

14. Una vez recibido el resultado de la transacción e informado a WebPay su correcta recepción, el sitio del comercio debe redirigir al tarjetahabiente nuevamente a WebPay, con la finalidad de desplegar el comprobante de pago. Es importante realizar este punto para que el tarjetahabiente entienda que el proceso de pago fue exitoso, y que involucrará un cargo a su tarjeta bancaria. El redireccionamiento a WebPay se hace utilizando como destino la URL informada por el método `getTransactionResult()` enviando por método POST el token de la transacción en la variable `token_ws`.
15. WebPay recibe un requerimiento con el token en la variable `token_ws`.
16. WebPay identifica la transacción y despliega el comprobante de pago al tarjetahabiente.
17. Una vez visualizado el comprobante de pago por un periodo acotado de tiempo, el tarjetahabiente es redirigido de vuelta al sitio del comercio, por medio de redireccionamiento con el token en la variable `token_ws` enviada por método POST hacia la página final informada por el comercio en el método `initTransaction()`.
18. Sitio del comercio despliega página final de pago.

### Flujo si usuario aborta el pago

![Diagrama de secuencia si usuario aborta el
pago](/images/diagrama-secuencia-webpay-abortar.png)

Si el tarjetahabiente anula la transacción en el formulario de pago de Webpay, el flujo cambia y los pasos son los siguientes:

1. Una vez seleccionado los bienes o servicios, tarjetahabiente decide pagar a través de PatPass by WebPay.
2. El comercio inicia una transacción en Webpay, invocando el método `initTransaction()`.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el token de la transacción y URL de redireccionamiento a la cual se deberá redirigir al tarjetahabiente.
4. Comercio redirecciona al tarjetahabiente hacia Webpay, con el token de la transacción a la URL indicada en punto 3. La redirección se realiza enviando por método POST el token en variable `token_ws`.
5. El navegador Web del tarjetahabiente realiza una petición HTTPS a Webpay, en base al redireccionamiento generado por el comercio en el punto 4.
6. Webpay responde al requerimiento desplegando el formulario de pago de Webpay. Desde este punto la comunicación es entre Webpay y el tarjetahabiente, sin interferir el comercio. El formulario de pago de PatPass by Webpay despliega, entre otras cosas, el monto de la transacción, fecha de término de suscripción, información del comercio como nombre y logotipo, y la opción de pago a través de crédito.
7. Tarjetahabiente hace clic en anular, en formulario PatPass by WebPay.
8. Webpay retorna el control al comercio, realizando un redireccionamiento HTTPS hacia la página de **final del comercio**, en donde se envía por método POST el token de la transacción en la variable `TBK_TOKEN` además de las variables `TBK_ORDEN_COMPRA` y `TBK_ID_SESION`.

    <aside class="warning">
        Nota que el nombre de las variables recibidas es diferente. En lugar de `token_ws` acá el token viene en la variable `TBK_TOKEN`.
    </aside>

9. El comercio con la variable `TBK_TOKEN` debe invocar el método
   `getTransactionResult()`, para obtener el resultado de la autorización. En
   este caso debe obtener una excepción, pues el pago fue abortado.
10. El comercio debe informar al tarjetahabiente que su pago no se completó.

### Crear una transacción PatPass by Webpay Normal

Para crear una transacción basta llamar al método `initTransaction()`

#### `initTransaction()`

Permite inicializar una transacción en  Webpay. Como respuesta a la invocación se genera un toke que representa en forma única una transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el token es caducado y no podrá ser utilizado en un pago.
</aside>

```java
WsInitTransactionOutput initResult = transaction.initTransaction(
        amount, sessionId, buyOrder, returnUrl, finalUrl, patPassInfo);
```

```php
PENDIENTE
```

```csharp
wsInitTransactionOutput initResult = transaction.initTransaction(
    amount, buyOrder, sessionId, returnUrl, finalUrl, patPassInfo);
```

##### Parámetros

Nombre <br><i> tipo </i> | Descripción
------ | -----------
WSTransactionType <br><i> wsTransactionType </i> | Indica el tipo de transacción, su valor debe ser siempre TR_NORMAL_WS_WPM. Los SDKs se encargan automáticamente de este parámetro
sessionId <br><i> xs:string </i> | (Opcional) Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
returnURL <br><i> xs:anyURI </i> | URL del comercio, a la cual Webpay redireccionará posterior al proceso de autorización. Largo máximo: 256
finalURL <br><i>xs:anyURI </i> | URL del comercio a la cual Webpay redireccionará posterior al voucher de éxito de Webpay. Largo máximo 256
transactionDetails <br><i>wsTransactionDetail </i> | Lista de objetos del tipo wsTransactionDetail, el cual contiene datos de la transacción. Para este tipo de transacciones debe contener exactamente un elemento
transactionDetails[0].amount <br><i> xs:decimal </i> | Monto de la transacción. Máximo 2 decimales para USD y UF. Largo máximo: 10
transactionDetails[0].buyOrder <br><i> xs:string </i> | Orden de compra de la tienda. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>
transactionDetails[0].commerceCode <br><i> xs:string </i> | Código comercio de la tienda entregado por Transbank. Largo: 12. Los SDKs se encargan automáticamente de este parámetro a partir de la configuración de comercio y certificados/llaves usada para iniciar la transacción
wPMDetail <br><i> wpmDetailInput </i> | Objeto del tipo wpmDetailInput, el cual contiene datos asociados a la inscripción de PatPass by WebPay. Los SDKs se encargan de estos parámetros al pasarlos dentro de un objeto PatPassInfo en la función `initTransacction()`
wPMDetail.serviceId <br><i> xs:string </i> | Corresponde al identificador de servicio con que el comercio identifica a su cliente. Largo máximo: 30
wPMDetail.cardHolderId <br><i> xs:string </i> | RUT del tarjetahabiente. Largo máximo: 12. Formato: NNNNNNNNA
wPMDetail.cardHolderName <br><i> xs:string </i> | Nombre tarjetahabiente. Largo máximo: 50
wPMDetail.cardHolderLastName1 <br><i> xs:string </i> | Apellido paterno tarjetahabiente. Largo máximo: 50
wPMDetail.cardHolderLastName2 <br><i> xs:string </i> | Apellido materno tarjetahabiente. Largo máximo: 50
wPMDetail.cardHolderMail <br><i> xs:string </i> | Correo electrónico tarjetahabiente. Largo máximo: 50
wPMDetail.cellPhoneNumber <br><i> xs:string </i> | Número teléfono celular tarjetahabiente. Largo máximo: 12
wPMDetail.expirationDate <br><i> xs:dateTime </i> | Fecha expiración de PatPass by WebPay, corresponde al último pago. Largo: 10. Formato AAAA-MM-DD
wPMDetail.commerceMail <br><i> xs:string </i> | Correo electrónico comercio. Largo máximo: 50. Los SDKs se encargan automáticamente de este parámetro a partir del email de comercio ingresado en la configuración usada para iniciar la transacción
wPMDetail.ufFlag <br><i> xs:boolean </i> | Valor en true indica que el monto enviado está expresado en UF, valor en false indica que valor esta expresado en pesos. Los SDKs se encargan automáticamente de este parámetro a partir de la configuración de moneda y certificados/llaves usada para iniciar la transacción

##### Respuesta

```java
initResult.getUrl();
initResult.getToken();
```

```php
PENDIENTE
```

```csharp
initResult.url;
initResult.token;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Token de la transacción. Largo: 64.
url  <br> <i> xs:string </i> | URL de formulario de pago PatPass by Webpay. Largo máximo: 256.

### Confirmar una transacción PatPass by Webpay Normal

Cuando el comercio retoma el control mediante `returnURL` puedes confirmar una transacción usando los métodos  `getTransactionResult()` y `acknowledgeTransaction()`

#### `getTransactionResult()`

Permite obtener el resultado de la transacción una vez que Webpay ha resuelto su autorización financiera.

```java
TransactionResultOutput result =
    transaction.getTransactionResult(
        request.getParameter("token_ws"));
```

```php
PENDIENTE
```

```csharp
transactionResultOutput result =
    transaction.getTransactionResult(Request.Form["token_ws"]);
```

#<strong>Parámetros getTransactionResult</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tokenInput  <br> <i> xs:string </i> | Token de la transacción. Largo: 64.

#<strong>Respuesta getTransactionResult</strong>

```java
WsTransactionDetailOutput output = result.getDetailOutput().get(0);
if (output.getResponseCode() == 0) {
    // Suscripción exitosa, puedes procesar el resultado
    // con el contenido de las variables result y output.
    result.getBuyOrder();
    result.getSessionId();
    result.getCardDetail().getCardNumber();
    result.getCardDetail().getCardExpirationDate();
    result.getAccountingDate();
    result.getTransactionDate();
    result.getVci();
    result.getUrlRedirection();
    output.getAuthorizationCode();
    output.getPaymentType();
    output.getAmount();
    output.getSharesNumber();
    output.getCommerceCode();
    output.getBuyOrder();
```

```php
PENDIENTE
```

```csharp
transactionResultOutput result =
    transaction.getTransactionResult(Request.Form["token_ws"]);
wsTransactionDetailOutput output = result.detailOutput[0];
if (output.responseCode == 0){
    // Suscripción exitosa, puedes procesar el resultado con el
    // contenido de las variables result y output.
    result.buyOrder;
    result.sessionId;
    result.cardDetail.cardNumber;
    result.cardDetail.cardExpirationDate;
    result.accountingDate;
    result.transactionDate;
    result.VCI;
    result.urlRedirection;
    output.authorizationCode;
    output.paymentTypeCode;
    output.amount;
    output.sharesNumber;
    output.commerceCode;
    output.buyOrder;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buyOrder  <br> <i> xs:string </i> | Orden de compra de la tienda indicado en `initTransaction()`. Largo máximo: 26
sessionId  <br> <i> xs:string </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `initTransaction()`. Largo máximo: 61.
cardDetails  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
cardDetails.cardNumber  <br> <i> xs:string </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente.Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 16.
cardDetails.cardExpirationDate  <br> <i> xs:string </i> |(Opcional) Fecha de expiración de la tarjeta de crédito del tarjetahabiente. Formato YYMM Solo para comercios autorizados por Transbank. Largo máximo: 4
accoutingDate  <br> <i> xs:string </i> | Fecha de la autorización. Largo: 4, formato MMDD
transactionDate  <br> <i> xs:string </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
VCI  <br> <i> xs:string </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor TSY (Autenticación exitosa), TSN (Autenticación fallida), TO (Tiempo máximo excedido para autenticación), ABO (Autenticación abortada por tarjetahabiente), U3 (Error interno en la autenticación), CNP (No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure), A (Intento de autenticación). Puede ser vacío si la transacción no se autenticó. Largo máximo: 3
urlRedirection  <br> <i> xs:string </i> | URL de redirección para visualización de voucher. Largo máximo: 256
detailsOutput  <br> <i> wsTransactionDetailOutput </i> | Lista con resultado de cada una de las `transactionDetails` enviados en `initTransaction()`. Para PatPass by Webpay tiene máximo un elemento.
detailsOutput[0].authorizationCode  <br> <i> xs:string </i> | Código de autorización de la transacción Largo máximo: 6
detailsOutput[0].paymentTypeCode   <br> <i> xs:string </i> | Tipo de pago de la transacción. <br> VN = Venta Normal <br> Largo máximo: 2
detailsOutput[0].responseCode  <br> <i> xs:string </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada.<br> -1 = Rechazo de transacción.<br> -2 =  Transacción debe reintentarse. <br> -3 = Error en transacción. <br> -4 = Rechazo de transacción.<br> -5 = Rechazo por error de tasa. <br> -6 = Excede cupo máximo mensual. <br> -7 = Excede límite diario por transacción. <br> -8 = Rubro no autorizado. <br> -100 Rechazo por inscripción de PatPass by Webpay.
detailsOutput[0].amount  <br> <i> xs:decimal </i> | Monto de la transacción. Formato número entero para transacciones en pesos y UF máximo 2 decimales. Largo máximo: 10
detailsOutput[0].sharesNumber  <br> <i> xs:int </i> | Cantidad de cuotas. Valor por defecto 0. Largo máximo: 2
detailsOutput[0].commerceCode  <br> <i> xs:string </i> | Código comercio de la tienda. Largo: 12
detailsOutput[0].buyOrder  <br> <i> xs:string </i> | Orden de compra de la tienda. Largo máximo: 26

#### `acknowledgeTransaction()`

Indica a Webpay que se ha recibido conforme el resultado de la transacción.

<aside class="warning">
El método acknowledgeTransaction debe ser invocado siempre. Si la invocación no se realiza en un período de 30 segundos, Webpay reversará la transacción, asumiendo que el comercio no pudo informar de su resultado, evitando así el cobro al tarjetahabiente.
</aside>

#<strong>Parámetros acknowledgeTransaction</strong>

> Los SDKs ejecutan automáticamente `acknowledgeTransaction()` cuando reciben la
> respuesta de `getTransactionResult()`.

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tokenInput  <br> <i> xs:string </i> | Token de la transacción. Largo: 64.

#<strong>Respuesta acknowledgeTransaction</strong>

> Los SDKs arrojarán una excepción dentro de `getTransactionResult()` si falla
> el `acknowledgeTransaction()` que se ejecuta automáticamente.

Ninguna.

<aside class="warning">
En caso de llamar al método acknowledgeTransaction después de 30 segundos de
ocurrida la autorización, se informará la excepción descrita más abajo y el
comercio no debe entregar producto o servicio, ya que la transacción ha sido
reversada por Webpay: Timeout error (Transactions REVERSED) con código 277.
</aside>
