___

<aside class="notice">
Acabamos de publicar la nueva y futura referencia REST para los servicios 
de Transbank que próximamente pasará a ser la referencia oficial. 
Si tienes curiosidad, te invitamos a conocerla [aquí](/referencia/webpayrest)
</aside>

# Webpay 

## Ambientes y Credenciales

### Ambiente de Producción

```java
Configuration configuration = new Configuration();
configuration.setEnvironment(Webpay.Environment.LIVE);
```

```php
$configuration = new Configuration();
$configuration->setEnvironment("PRODUCCION");
```

```csharp
var configuration = new Configuration();
configuration.Environment = "PRODUCCION";
```

Las URLs de endpoints de producción están alojados dentro de
<https://webpay3g.transbank.cl/>.

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
$configuration->setWebpayCert(
     "-----BEGIN CERTIFICATE-----\n" .
    "MIIEDzCCAvegAwIBAgIJAMaH4DFTKdnJMA0GCSqGSIb3DQEBCwUAMIGdMQswCQYD\n" .
    "VQQGEwJDTDERMA8GA1UECAwIU2FudGlhZ28xETAPBgNVBAcMCFNhbnRpYWdvMRcw\n" .
    ...
    "MX5lzVXafBH/sPd545fBH2J3xAY3jtP764G4M8JayOFzGB0=\n" .
    "-----END CERTIFICATE-----\n");
);
```

```csharp
configuration.WebpayCertPath = @"C:\Certs\certificado-publico-transbank.crt"
```

Para validar las respuestas generadas por Transbank debes usar un certificado
público de Webpay. En [el repositorio GitHub
`transbank-webpay-credenciales`](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/)
podrás encontrar [el certificado
actualizado](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/tree/master/produccion).

### Ambiente de Integración

```java
configuration.setEnvironment(Webpay.Environment.TEST);
```

```php
$configuration->setEnvironment("INTEGRACION");
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
configuration.setCommerceCode("597020000540");
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
$configuration->setCommerceCode("597020000540");
$configuration->setPrivateKey(
    "-----BEGIN RSA PRIVATE KEY-----\n" .
    ... .
    "-----END RSA PRIVATE KEY-----\n"
);
$configuration->setPublicCert(
    "-----BEGIN CERTIFICATE-----\n" .
    ... .
    "-----END CERTIFICATE-----\n"
);
```

```csharp
configuration.CommerceCode = "597020000540";
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

Puedes mirar el siguiente enlace para obtener una guía rápida de como generar tu
propio archivo: [Crear archivo pfx usando openssl](https://www.ssl.com/how-to/create-a-pfx-p12-certificate-file-using-openssl/)
</aside>

Consulta [la documentación para generar una llave privada y un certificado
usando openssl](/documentacion/como_empezar#credenciales-en-webpay) si
no sabes aún como realizarlo.

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
    Configuration.forTestingWebpayPlusNormal();

Configuration configuration =
    Configuration.forTestingWebpayPlusCapture();

Configuration configuration =
    Configuration.forTestingWebpayPlusMall();

Configuration configuration =
    Configuration.forTestingWebpayOneClickNormal();
```

```php
$configuration = Configuration::forTestingWebpayPlusNormal();

$configuration = Configuration::forTestingWebpayPlusCapture();

$configuration = Configuration::forTestingWebpayPlusMall();

$configuration = Configuration::forTestingWebpayOneClickNormal();
```

```csharp
var configuration = Configuration.ForTestingWebpayPlusNormal();

var configuration = Configuration.ForTestingWebpayPlusCapture();

var configuration = Configuration.ForTestingWebpayPlusMall();

var configuration = Configuration.ForTestingWebpayOneClickNormal();
```

## Webpay Plus Normal

```java
WebpayNormal transaction =
    new Webpay(configuration).getNormalTransaction();
```

```php
$transaction =
    (new Webpay($configuration))->getNormalTransaction();
```

```csharp
var transaction = new Webpay(configuration).NormalTransaction;
```

WSDL: `/WSWebpayTransaction/cxf/WSWebpayService?wsdl`

Una transacción de autorización normal (o transacción normal), corresponde a
una solicitud de autorización financiera de un pago con tarjetas de crédito o
débito, en donde quién realiza el pago ingresa al sitio del comercio,
selecciona productos o servicio, y el ingreso asociado a los datos de la tarjeta
de crédito o débito lo realiza en forma segura en Webpay.

### Flujo en caso de éxito

De cara al tarjetahabiente, el flujo de páginas para la transacción es el
siguiente:

<img class="td_img-night" src="/images/flujo-paginas-webpay.png" alt="Flujo de páginas Webpay Plus Normal">

Desde el punto de vista técnico, la secuencia es la siguiente:

<img class="td_img-night" src="/images/diagrama-secuencia-webpay.png" alt="Diagrama de secuencia Webpay Plus Normal">

1. Una vez seleccionado los bienes o servicios, tarjetahabiente decide pagar a
   través de Webpay.
2. El comercio inicia una transacción en Webpay, invocando el método
   `initTransaction()`.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el
   token de la transacción y URL de redireccionamiento a la cual se deberá
   redirigir al tarjetahabiente.
4. Comercio redirecciona al tarjetahabiente hacia Webpay, con el token de la
   transacción a la URL indicada en punto 3. La redirección se realiza
   enviando por método POST el token en variable `token_ws`.
5. El navegador Web del tarjetahabiente realiza una petición HTTPS a Webpay, en
   base al redireccionamiento generado por el comercio en el punto 4.
6. Webpay responde al requerimiento desplegando el formulario de pago de Webpay.
   Desde este punto la comunicación es entre Webpay y el tarjetahabiente, sin
   interferir el comercio. El formulario de pago de Webpay despliega, entre
   otras cosas, el monto de la transacción, información del comercio como
   nombre y logotipo, las opciones de pago a través de crédito o débito.
7. Tarjetahabiente ingresa los datos de la tarjeta, hace clic en pagar en
   formulario Webpay.
8. Webpay procesa la solicitud de autorización (primero autenticación bancaria
   y luego la autorización de la transacción).
9. Una vez resuelta la autorización, Webpay retorna el control al comercio,
   realizando un redireccionamiento HTTPS hacia la página de transición
   del comercio, en donde se envía por método POST el token de la transacción
   en la variable `token_ws`. El comercio debe implementar la recepción de esta
   variable.
10. El navegador Web del tarjetahabiente realiza una petición HTTPS al
    sitio del comercio, en base a la redirección generada por Webpay en el
    punto 9.
11. El sitio del comercio recibe la variable `token_ws` e invoca el segundo
    método Web, `getTransactionResult()` (mientras se despliega la página de
    transición), para obtener el resultado de la autorización. Se recomienda
    que el resultado de la autorización sea persistida en los sistemas del
    comercio, ya que este método se puede invocar una única vez por
    transacción.
12. Comercio recibe el resultado de la invocación del método
    `getTransactionResult()`.
13. Para que el comercio informe a Webpay que el resultado de la transacción se
    ha recibido sin problemas, el sistema del comercio debe consumir el tercer
    método `acknowledgeTransaction()`. Si esto fue ejecutado correctamente el
    producto puede ser liberado al cliente.

> Los SDKs consumen `acknowledgeTransaction()` de manera automática, tan pronto
> reciben una respuesta de `getTransactionResult()`.

<aside class="warning">
De no ser consumido `acknowledgeTransaction()` o demorar más de 30 segundos en
su consumo, Webpay realizará la reversa de la transacción, asumiendo que
existieron problemas de comunicación. En este caso el método retorna una
Excepción indicando la situación. Esta excepción debe ser manejada para no
entregar el producto o servicio en caso que ocurra.
</aside>

14. Una vez recibido el resultado de la transacción e informado a Webpay su
    correcta recepción, el sitio del comercio debe redirigir al tarjetahabiente
    nuevamente a Webpay, con la finalidad de desplegar el comprobante de pago.
    Es importante realizar este punto para que el tarjetahabiente entienda que
    el proceso de pago fue exitoso, y que involucrará un cargo a su tarjeta
    bancaria. El redireccionamiento a Webpay se hace utilizando como destino la
    URL informada por el método `getTransactionResult()`enviando por método
    POST el token de la transacción en la variable `token_ws`.
15. Webpay recibe un requerimiento con la variable `token_ws`
16. Webpay identifica la transacción y despliega el comprobante de pago al
    tarjetahabiente.
17. Una vez visualizado el comprobante de pago por un periodo acotado de tiempo,
    el tarjetahabiente es redirigido de vuelta al sitio del comercio, por medio
    de redireccionamiento con el token en la variable token_ws enviada por
    método POST hacia la página final informada por el comercio en el método
    initTransaction.
18. Sitio del comercio despliega página final de pago

#### Flujo si usuario aborta el pago

<img class="td_img-night" src="/images/diagrama-secuencia-webpay-abortar.png" alt="Diagrama de secuencia si usuario aborta el pago">

Si el tarjetahabiente anula la transacción en el formulario de pago de Webpay,
el flujo cambia y los pasos son los siguientes:

1. Una vez seleccionado los bienes o servicios, tarjetahabiente decide pagar a
   través de Webpay.
2. El comercio inicia una transacción en Webpay, invocando el método
   `initTransaction()`.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el
   token de la transacción y URL de redireccionamiento a la cual se deberá
   redirigir al tarjetahabiente.
4. Comercio redirecciona al tarjetahabiente hacia Webpay, con el token de la
   transacción a la URL indicada en punto 3. La redirección se realiza
   enviando por método POST el token en variable `token_ws`.
5. El navegador Web del tarjetahabiente realiza una petición HTTPS a Webpay, en
   base al redireccionamiento generado por el comercio en el punto 4.
6. Webpay responde al requerimiento desplegando el formulario de pago de Webpay.
   Desde este punto la comunicación es entre Webpay y el tarjetahabiente, sin
   interferir el comercio. El formulario de pago de Webpay despliega, entre
   otras cosas, el monto de la transacción, información del comercio como
   nombre y logotipo, las opciones de pago a través de crédito o débito.
7. Tarjetahabiente hace clic en “anular”, en formulario Webpay.
8. Webpay retorna el control al comercio, realizando un redireccionamiento
   HTTPS hacia la página de **final del comercio**, en donde se envía por
   método POST el token de la transacción en la variable `TBK_TOKEN` además de las variables `TBK_ORDEN_COMPRA` y `TBK_ID_SESION`.

<aside class="warning">
Nota que el nombre de las variables recibidas es diferente. En lugar de `token_ws` acá el token viene en la variable `TBK_TOKEN`.
</aside>

9. El comercio con la variable `TBK_TOKEN` debe invocar el método
   `getTransactionResult()`, para obtener el resultado de la autorización. En
   este caso debe obtener una excepción, pues el pago fue abortado.
10. El comercio debe informar al tarjetahabiente que su pago no se completó.

### Crear una transacción Webpay Plus Normal

Para crear una transacción basta llamar al método `initTransaction()`

#### `initTransaction()`

Permite inicializar una transacción en Webpay. Como respuesta a la invocación
se genera un token que representa en forma única una transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

```java
WsInitTransactionOutput initResult =
    transaction.initTransaction(
        amount, sessionId, buyOrder, returnUrl, finalUrl);
```

```php
$initResult = $transaction->initTransaction(
        $amount, $sessionId, $buyOrder, $returnUrl, $finalUrl);
```

```csharp
var initResult = transaction.initTransaction(
        amount, buyOrder, sessionId, returnUrl, finalUrl);
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
WSTransactionType  <br> <i> wsTransactionType </i> | Indica el tipo de transacción, su valor debe ser siempre TR_NORMAL_WS para transacciones normales. Los SDKs se encargan automáticamente de este parámetro
sessionId  <br> <i> xs:string </i> | (Opcional) Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
returnURL  <br> <i> xs:anyURI </i> | URL del comercio, a la cual Webpay redireccionará posterior al proceso de autorización. Largo máximo: 256
finalURL  <br> <i>xs:anyURI </i> | URL del comercio a la cual Webpay redireccionará posterior al voucher de éxito de Webpay. Largo máximo 256
transactionDetails  <br> <i>wsTransactionDetail </i> | Lista de objetos del tipo wsTransactionDetail, el cual contiene datos de la transacción. Para transacciones normales debe contener exactamente un elemento
transactionDetails[0].amount  <br> <i> xs:decimal </i> | Monto de la transacción. Máximo 2 decimales para USD. Largo máximo: 10
transactionDetails[0].buyOrder  <br> <i> xs:string </i> | Orden de compra de la tienda. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>
transactionDetails[0].commerceCode  <br> <i>xs:string </i> | Código comercio de la tienda entregado por Transbank. Largo: 12. Los SDKs se encargan automáticamente de este parámetro a partir de la configuración de comercio y certificados/llaves usada para iniciar la transacción

**Respuesta**

```java
initResult.getToken();
initResult.getUrl();
```

```php
$initResult->token;
$initResult->url;
```

```csharp
initResult.token;
initResult.url;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Token de la transacción. Largo: 64.
url  <br> <i> xs:string </i> | URL de formulario de pago Webpay. Largo máximo: 256.

### Confirmar una transacción Webpay Plus Normal

Cuando el comercio retoma el control mediante `returnURL` puedes confirmar una
transacción usando los métodos  `getTransactionResult()` y
`acknowledgeTransaction()`

#### `getTransactionResult()`

Permite obtener el resultado de la transacción una vez que Webpay ha resuelto su autorización financiera.

```java
TransactionResultOutput result =
    transaction.getTransactionResult(
        request.getParameter("token_ws"));
```

```php
$result = transaction->getTransactionResult(
    $request->input("token_ws"));
```

```csharp
var result = transaction.getTransactionResult(tokenWs));
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tokenInput  <br> <i> xs:string </i> | Token de la transacción. Largo: 64.

**Respuesta**

```java

WsTransactionDetailOutput output =
    result.getDetailOutput().get(0);
if (output.getResponseCode() == 0) {
    // Transaccion exitosa, puedes procesar el resultado
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
}
```

```php
$output = $result->detailOutput;
if ($output->responseCode == 0) {
    // Transaccion exitosa, puedes procesar el resultado
    // con el contenido de las variables $result y $output.
    $result->buyOrder;
    $result->sessionId;
    $result->cardDetail->cardNumber;
    $result->cardDetail->cardExpirationDate;
    $result->accountingDate;
    $result->transactionDate;
    $result->vci;
    $result->urlRedirection;
    $output->authorizationCode;
    $output->paymentType;
    $output->amount;
    $output->sharesNumber;
    $output->commerceCode;
    $output->buyOrder;
}
```

```csharp
var output = result.detailOutput[0]
if (output.responseCode == 0) {
    // Transaccion exitosa, puedes procesar el resultado
    // con el contenido de las variables result y output.
    result.buyOrder;
    result.sessionId;
    result.cardDetail.cardNumber;
    result.cardDetail.cardExpirationDate;
    result.accountingDate;
    result.transactionDate;
    result.vci;
    result.urlRedirection;
    output.authorizationCode;
    output.paymentType;
    output.amount;
    output.sharesNumber;
    output.commerceCode;
    output.buyOrder;
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buyOrder  <br> <i> xs:string </i> | Orden de compra de la tienda indicado en `initTransaction()`. Largo máximo: 26
sessionId  <br> <i> xs:string </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `initTransaction()`. Largo máximo: 61.
cardDetails  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
cardDetails.cardNumber  <br> <i> xs:string </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 16.
cardDetails.cardExpirationDate  <br> <i> xs:string </i> |(Opcional) Fecha de expiración de la tarjeta de crédito del tarjetahabiente. Formato YYMM Solo para comercios autorizados por Transbank. Largo máximo: 4
accoutingDate  <br> <i> xs:string </i> | Fecha de la autorización. Largo: 4, formato MMDD
transactionDate  <br> <i> xs:string </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
VCI  <br> <i> xs:string </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor:<br>TSY = Autenticación exitosa<br>TSN = Autenticación fallida<br>TO = Tiempo máximo excedido para autenticación<br>ABO = Autenticación abortada por tarjetahabiente<br>U3 = Error interno en la autenticación<br>NP = No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure<br>ACS2 = Autenticación fallida extranjera <br>A = Intento de autenticación<br>INV = Autenticación inválida<br>EOP = Error Operativo<br>Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
urlRedirection  <br> <i> xs:string </i> | URL de redirección para visualización de voucher. Largo máximo: 256
detailsOutput  <br> <i> wsTransactionDetailOutput </i> | Lista con resultado de cada una de las `transactionDetails` enviados en `initTransaction()`. Para Webpay Plus Normal tiene máximo un elemento.
detailsOutput[0].authorizationCode  <br> <i> xs:string </i> | Código de autorización de la transacción Largo máximo: 6
detailsOutput[0].paymentTypeCode   <br> <i> xs:string </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
detailsOutput[0].responseCode  <br> <i> xs:string </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada.<br> -1 = Rechazo de transacción.<br> -2 =  Transacción debe reintentarse. <br> -3 = Error en transacción. <br> -4 = Rechazo de transacción.<br> -5 = Rechazo por error de tasa. <br> -6 = Excede cupo máximo mensual. <br> -7 = Excede límite diario por transacción. <br> -8 = Rubro no autorizado.
detailsOutput[0].amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 10
detailsOutput[0].sharesNumber  <br> <i> xs:int </i> | Cantidad de cuotas. Largo máximo: 2
detailsOutput[0].commerceCode  <br> <i> xs:string </i> | Código comercio de la tienda. Largo: 12
detailsOutput[0].buyOrder  <br> <i> xs:string </i> | Orden de compra de la tienda. Largo máximo: 26

#### `acknowledgeTransaction()`

Indica a Webpay que se ha recibido conforme el resultado de la transacción.

<aside class="warning">
El método acknowledgeTransaction debe ser invocado siempre. Si la invocación
no se realiza en un período de 30 segundos, Webpay reversará la transacción,
asumiendo que el comercio no pudo informar de su resultado, evitando así el
cobro al tarjetahabiente.
</aside>

**Parámetros**

> Los SDKs ejecutan automáticamente `acknowledgeTransaction()` cuando reciben la
> respuesta de `getTransactionResult()`.

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tokenInput  <br> <i> xs:string </i> | Token de la transacción. Largo: 64.

**Respuesta**

> Los SDKs arrojarán una excepción dentro de `getTransactionResult()` si falla
> el `acknowledgeTransaction()` que se ejecuta automáticamente.

Ninguna.

<aside class="warning">
En caso de llamar al método acknowledgeTransaction después de 30 segundos de
ocurrida la autorización, se informará la excepción descrita más abajo y el
comercio no debe entregar producto o servicio, ya que la transacción ha sido
reversada por Webpay: Timeout error (Transactions REVERSED) con código 277.
</aside>

## Webpay Plus Mall

```java
WebpayMallNormal transaction =
    new Webpay(configuration).getMallNormalTransaction();
```

```php
$transaction =
    (new Webpay(configuration))->getMallNormalTransaction();
```

```csharp
var transaction =
    new Webpay(configuration).MallNormalTransaction;
```

WSDL: `/WSWebpayTransaction/cxf/WSWebpayService?wsdl`

Una transacción Mall Normal corresponde a una solicitud de autorización
financiera de un conjunto de pagos con tarjetas de crédito o débito, en donde
quién realiza el pago ingresa al sitio del comercio, selecciona productos o
servicios, y el ingreso asociado a los datos de la tarjeta de crédito o débito
lo realiza una única vez en forma segura en Webpay para el conjunto de pagos.
Cada pago tendrá su propio resultado, autorizado o rechazado.

![Desagregación de un pago Webpay Mall](/images/pago-webpay-mall.png)

El Mall Webpay agrupa múltiples tiendas, son estas últimas las que pueden
generar transacciones. Tanto el mall como las tiendas asociadas son
identificadas a través de un número denominado código de comercio.

#### Flujo Webpay Plus Mall

El flujo de Webpay Plus Mall es en general el mismo que el de [Webpay Plus
Normal](#webpay-plus-normal) tanto de cara al tarjeta habiente como de cara al
integrador.

Las diferencias son:

- Se debe usar un código de comercio configurado para modalidad Mall en
  Transbank, el cual debe ser indicado al iniciar la transacción.
- Se pueden indicar múltiples transacciones, cada una asociada a un código de
  comercio de tienda (que debe estar configurada en Transbank como perteneciente
  al mall).
- Se debe verificar por separado el resultado de cada una de esas transacciones
  individualmente, pues es posible que el emisor de la tarjeta autorice algunas
  y otras no.

### Crear una transacción Webpay Plus Mall

Para crear una transacción basta llamar al método `initTransaction()`

#### `initTransaction()`

Permite inicializar una transacción en Webpay. Como respuesta a la invocación
se genera un token que representa en forma única una transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

```java
WsTransactionDetail storeTransaction = new WsTransactionDetail();
List<WsTransactionDetail> transactions = new ArrayList<>();

storeTransaction.setAmount(amount);
storeTransaction.setCommerceCode(storeCode);
storeTransaction.setBuyOrder(storeBuyOrder);
transactions.add(storeTransaction);

storeTransaction.setAmount(anotherAmount);
storeTransaction.setCommerceCode(anotherStoreCode);
storeTransaction.setBuyOrder(anotherStoreBuyOrder);
transactions.add(storeTransaction);

WsInitTransactionOutput initResult = transaction.initTransaction(
    buyOrder, sessionId, returnUrl, finalUrl, transactions);
```

```php
$transactions = array();
$transactions[] = array(
    "storeCode" => $storeCode,
    "amount" => $amount,
    "buyOrder" => $buyOrder,
    "sessionId" => $sessionId
)
$transactions[] = array(
    "storeCode" => $anotherStoreCode,
    "amount" => $anotherAmount,
    "buyOrder" => $anotherBuyOrder,
    "sessionId" => $anotherSessionId
)

$initResult = $transaction->initTransaction(
    $buyOrder, $sessionId, $returnUrl, $finalUrl, $transactions);
```

```csharp
var transactions = new Dictionary<string, string[]>();

transactions.Add(storeCode,
    new string[] {storeCode, amount, buyOrder});
transactions.Add(anotherStoreCode,
    new string[] {anotherStoreCode, anotherAmount, anotherBuyOrder});

var initResult = transaction.initTransaction(
    buyOrder, sessionId, returnUrl, finalUrl, transactions);
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
WSTransactionType  <br> <i> wsTransactionType </i> | Indica el tipo de transacción, su valor debe ser siempre TR_MALL_WS para transacciones mall. Los SDKs se encargan automáticamente de este parámetro.
sessionId  <br> <i> xs:string </i> | (Opcional) Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
returnURL  <br> <i> xs:anyURI </i> | URL del comercio, a la cual Webpay redireccionará posterior al proceso de autorización Largo máximo: 256.
finalURL  <br> <i>xs:anyURI </i> | URL del comercio a la cual Webpay redireccionará posterior al voucher de éxito de Webpay. Largo máximo 256.
buyOrder  <br> <i> xs:string </i> | Es el código único de la orden de compra generada por el comercio mall.
transactionDetails  <br> <i>wsTransactionDetail </i> | Lista de objetos del tipo wsTransactionDetail, uno por cada tienda diferente del mall que participa en la transacción.
transactionDetails[].amount  <br> <i> xs:decimal </i> | Monto de la transacción de una tienda del mall. Máximo 2 decimales para USD. Largo máximo: 10.
transactionDetails[].buyOrder  <br> <i> xs:string </i> | Orden de compra de la tienda del mall. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>.
transactionDetails[].commerceCode  <br> <i>xs:string </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo: 12.

**Respuesta**

```java
initResult.getToken();
initResult.getUrl();
```

```php
$initResult->token;
$initResult->url;

```

```csharp
initResult.token;
initResult.url;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Token de la transacción. Largo: 64.
url  <br> <i> xs:string </i> | URL de formulario de pago Webpay. Largo máximo: 256.

### Confirmar una transacción Webpay Plus Mall

Para confirmar una transacción se deben usar los métodos  `getTransactionResult()` y
`acknowledgeTransaction()`

#### `getTransactionResult()`

Permite obtener el resultado de la transacción una vez que Webpay ha resuelto
su autorización financiera.

```java
TransactionResultOutput result =
    transaction.getTransactionResult(
        request.getParameter("token_ws"));
```

```php
$result = transaction->getTransactionResult(
    $request->input("token_ws"));
```

```csharp
var result = transaction.getTransactionResult(tokenWs));
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tokenInput  <br> <i> xs:string </i> | Token de la transacción. Largo: 64.

**Respuesta**

```java


for (WsTransactionDetailOutput output: result.getDetailOutput()) {
    // Se debe chequear cada transacción de cada tienda del
    // mall por separado:
    if (output.getResponseCode() == 0) {
        // Transaccion exitosa, puedes procesar el resultado
        // con el contenido de las variables result y output.
        output.getAuthorizationCode();
        output.getPaymentType();
        output.getAmount();
        output.getSharesNumber();
        output.getCommerceCode();
        output.getBuyOrder();
    }
}
result.getBuyOrder();
result.getSessionId();
result.getCardDetail().getCardNumber();
result.getCardDetail().getCardExpirationDate();
result.getAccountingDate();
result.getTransactionDate();
result.getVci();
result.getUrlRedirection();

```
```php
foreach ($result->detailOutput as $output) {
    // Se debe chequear cada transacción de cada tienda del
    // mall por separado:
    if ($output->responseCode == 0) {
        // Transaccion exitosa, puedes procesar el resultado
        // con el contenido de las variables result y output.
        output->authorizationCode;
        output->paymentType;
        output->amount;
        output->sharesNumber;
        output->commerceCode;
        output->buyOrder;
    }
}
result->buyOrder;
result->sessionId;
result->cardDetail->cardNumber;
result->cardDetail->cardExpirationDate;
result->accountingDate;
result->transactionDate;
result->vci;
result->urlRedirection;
```

```csharp
foreach (var output in result.detailOutput) {
    // Se debe chequear cada transacción de cada tienda del
    // mall por separado:
    if (output.responseCode == 0) {
        // Transaccion exitosa, puedes procesar el resultado
        // con el contenido de las variables result y output.
        output.authorizationCode;
        output.paymentType;
        output.amount;
        output.sharesNumber;
        output.commerceCode;
        output.buyOrder;
    }
}
result.buyOrder;
result.sessionId;
result.cardDetail.cardNumber;
result.cardDetail.cardExpirationDate;
result.accountingDate;
result.transactionDate;
result.vci;
result.urlRedirection;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buyOrder  <br> <i> xs:string </i> | Orden de compra del mall. Largo máximo: 26
sessionId  <br> <i> xs:string </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `initTransaction()`. Largo máximo: 61.
cardDetails  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
cardDetails.cardNumber  <br> <i> xs:string </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente.Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 16.
cardDetails.cardExpirationDate  <br> <i> xs:string </i> |(Opcional) Fecha de expiración de la tarjeta de crédito del tarjetahabiente. Formato YYMM Solo para comercios autorizados por Transbank. Largo máximo: 4
accoutingDate  <br> <i> xs:string </i> | Fecha de la autorización. Largo: 4, formato MMDD
transactionDate  <br> <i> xs:string </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
VCI  <br> <i> xs:string </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor:<br>TSY = Autenticación exitosa<br>TSN = Autenticación fallida<br>TO = Tiempo máximo excedido para autenticación<br>ABO = Autenticación abortada por tarjetahabiente<br>U3 = Error interno en la autenticación<br>NP = No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure<br>ACS2 = Autenticación fallida extranjera <br>A = Intento de autenticación<br>INV = Autenticación inválida<br>EOP = Error Operativo<br>Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
urlRedirection  <br> <i> xs:string </i> | URL de redirección para visualización de voucher. Largo máximo: 256
detailsOutput  <br> <i> wsTransactionDetailOutput </i> | Lista con resultado de cada una de las `transactionDetails` enviados en `initTransaction()`.
detailsOutput[].authorizationCode  <br> <i> xs:string </i> | Código de autorización de la transacción Largo máximo: 6
detailsOutput[].paymentTypeCode   <br> <i> xs:string </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
detailsOutput[].responseCode  <br> <i> xs:string </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada.<br> -1 = Rechazo de transacción.<br> -2 =  Transacción debe reintentarse. <br> -3 = Error en transacción. <br> -4 = Rechazo de transacción.<br> -5 = Rechazo por error de tasa. <br> -6 = Excede cupo máximo mensual. <br> -7 = Excede límite diario por transacción. <br> -8 = Rubro no autorizado.
detailsOutput[].amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 10
detailsOutput[].sharesNumber  <br> <i> xs:int </i> | Cantidad de cuotas. Largo máximo: 2
detailsOutput[].commerceCode  <br> <i> xs:string </i> | Código comercio de la tienda. Largo: 12
detailsOutput[].buyOrder  <br> <i> xs:string </i> | Orden de compra de la tienda. Largo máximo: 26

#### `acknowledgeTransaction()`

Indica a Webpay que se ha recibido conforme el resultado de la transacción.

<aside class="warning">
El método acknowledgeTransaction debe ser invocado siempre. Si la invocación
no se realiza en un período de 30 segundos, Webpay reversará la transacción,
asumiendo que el comercio no pudo informar de su resultado, evitando así el
cobro al tarjetahabiente.
</aside>

**Parámetros**

> Los SDKs ejecutan automáticamente `acknowledgeTransaction()` cuando reciben la
> respuesta de `getTransactionResult()`.

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tokenInput  <br> <i> xs:string </i> | Token de la transacción. Largo: 64.

**Respuesta**

> Los SDKs arrojarán una excepción dentro de `getTransactionResult()` si falla
> el `acknowledgeTransaction()` que se ejecuta automáticamente.

Ninguna.

<aside class="warning">
En caso de llamar al método acknowledgeTransaction después de 30 segundos de
ocurrida la autorización, se informará la excepción descrita más abajo y el
comercio no debe entregar producto o servicio, ya que la transacción ha sido
reversada por Webpay: Timeout error (Transactions REVERSED) con código 277.
</aside>

## Otros Servicios Webpay Plus

WSDL: `/WSWebpayTransaction/cxf/WSCommerceIntegrationService?wsdl`

### Captura diferida Webpay Plus

Este método permite a todo comercio habilitado realizar capturas de una
transacción autorizada sin captura generada en Webpay Plus o Webpay OneClick.
El método contempla una única captura por cada autorización. Para ello se
deberá indicar los datos asociados a la transacción de venta con autorización
sin captura y el monto requerido para capturar el cual debe ser menor o igual al
monto originalmente autorizado.

Las ejecuciones con errores entregarán un `SoapFault` de acuerdo a la
codificación de errores definida más abajo.

Para capturar una transacción, ésta debe haber sido creada (según lo visto
anteriormente para Webpay Plus Normal o Webpay Plus Mall) por un código de
comercio configurado para captura diferida. De esa forma la transacción estará
autorizada pero requerirá una captura explícita posterior para confirmar la
transacción.

Puedes [leer más sobre la captura en la información del
producto Webpay](/producto/webpay#autorizacion-y-captura)
para conocer más detalles y restricciones.

Para realizar esa captura explícita debe usarse el método `capture()`

#### `capture()`

Permite solicitar a Webpay la captura diferida de una transacción con
autorización y sin captura simultánea.

> Los SDKs permiten indicar opcionalmente el código de comercio de la
> transacción a capturar, para soportar la captura en comercios Webpay Plus
> Mall. En comercios Webpay Plus Normal, no es necesario especificar el código
> de comercio pues se usa el indicado en la configuración.

<aside class="notice">
El método `capture()` debe ser invocado siempre indicando el código del
comercio que realizó la transacción. En el caso de comercios Webpay Plus Mall,
el código debe ser el código de la tienda virtual específica.
</aside>

```java
WebpayCapture transaction =
    new Webpay(configuration).getCaptureTransaction();

// Para comercios Webpay Plus Normal
CaptureOutput captureResult = transaction.capture(
    authorizationCode, capturedAmount, buyOrder);

// Para comercios Webpay Plus Mall
CaptureOutput captureResult = transaction.capture(
    authorizationCode, capturedAmount, buyOrder, storeCommerceCode);
```

```php
$transaction = (new Webpay(configuration))->getCaptureTransaction();

// Para comercios Webpay Plus Normal
$captureResult = transaction.capture(
    $authorizationCode, $capturedAmount, $buyOrder);

// Para comercios Webpay Plus Mall
$captureResult = transaction.capture(
    $authorizationCode, $capturedAmount, $buyOrder, $storeCommerceCode);
```

```csharp
var transaction = new Webpay(configuration).CaptureTransaction;

// Para comercios Webpay Plus Normal
var captureResult = transaction.capture(
    authorizationCode, capturedAmount, buyOrder);

// Para comercios Webpay Plus Mall
var captureResult = transaction.capture(
    authorizationCode, capturedAmount, buyOrder, storeCommerceCode);

```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorizationCode  <br> <i> xs:string </i> | Código de autorización de la transacción que se requiere capturar Largo máximo: 6.
buyOrder  <br> <i> xs:string </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
commerceId  <br> <i> xs:long </i> | Código de comercio o tienda mall que realizó la transacción. Largo: 12.
capturedAmount  <br> <i> xs:decimal </i> | Monto que se desea capturar. Largo máximo: 10.

**Respuesta**

```java
captureResult.getToken();
captureResult.getAuthorizationCode();
captureResult.getAuthorizationDate();
captureResult.getCapturedAmount();
```

```php
$captureResult->token;
$captureResult->authorizationCode;
$captureResult->authorizationDate;
$captureResult->capturedAmount;
```

```csharp
captureResult.token;
captureResult.authorizationCode;
captureResult.authorizationDate;
captureResult.capturedAmount;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Token de la transacción.
authorizationCode  <br> <i> xs:string </i> | Código de autorización de la captura diferida.
authorizationDate  <br> <i> xs:string </i> | Fecha y hora de la autorización.
capturedAmount  <br> <i> xs:decimal </i> | Monto capturado.

En caso de error pueden aparecer los siguientes códigos exclusivos del método
`capture()`:

Código | Descripción
------ | -----------
304 | Validación de campos de entrada nulos
245 | Código de comercio no existe
22 | El comercio no se encuentra activo
316 | El comercio indicado no corresponde al certificado o no es hijo del comercio MALL en caso de transacciones MALL
308 | Operación no permitida
274 | Transacción no encontrada
16 | La transacción no es de captura diferida
292 | La transacción no está autorizada
284 | Periodo de captura excedido
310 | Transacción reversada previamente
309 | Transacción capturada previamente
311 | Monto a capturar excede el monto autorizado
315 | Error del autorizador

### Anulación Webpay Plus

Este método permite a todo comercio habilitado anular una transacción que fue
generada en Webpay Plus (Normal y Mall) o Webpay OneClick Normal. El método
contempla anular total o parcialmente una transacción. Para ello se deberá
indicar los datos asociados a la transacción de venta en línea que se desea
anular y los montos requeridos para anular. Se considera totalmente anulada una
transacción cuando el monto anulado o el monto total de anulaciones cursadas
alcancen el monto autorizado en la venta en línea.

El servicio web de anulación de transacciones Webpay soporta una sola
anulación parcial para la transacción de venta en línea. En caso de enviar
una segunda anulación parcial se retornará una `Exception`.

Las ejecuciones con errores entregarán un SoapFault de acuerdo a la
codificación de errores definida mas abajo.

La anulación puede realizarse máximo 90 días después de la fecha de la
transacción original.

Puedes [leer más sobre la anulación en la información del
producto Webpay](/producto/webpay#anulaciones) para conocer
más detalles y restricciones.

Para anular una transacción se debe invocar al método `nullify()`.

#### `nullify()`

Permite solicitar a Webpay la anulación de una transacción realizada previamente y que se encuentra vigente.

> Los SDKs permiten indicar opcionalmente el código de comercio de la
> transacción a anular, para soportar la anulación en comercios Webpay Plus
> Mall. En comercios Webpay Plus Normal, no es necesario especificar el código
> de comercio pues se usa el indicado en la configuración.

<aside class="notice">
El método `nullify()` debe ser invocado siempre indicando el código del comercio que realizó la transacción. En el caso de comercios Webpay Plus Mall, el código debe ser el código de la tienda virtual específica.
</aside>

```java
WebpayNullify transaction =
    new Webpay(configuration).getNullifyTransaction();

// Para comercios Webpay Plus Normal
NullificationOutput result = transaction.nullify(
    authorizationCode, authorizedAmount, buyOrder, nullifyAmount);

// Para comercios Webpay Plus Mall
NullificationOutput result = transaction.nullify(
    authorizationCode, authorizedAmount, buyOrder, nullifyAmount,
    storeCommerceCode);

```

```php
$transaction = (new Webpay(configuration))->getNullifyTransaction();

// Para comercios Webpay Plus Normal
$result = transaction.nullify(
    $authorizationCode, $authorizedAmount, $buyOrder, $nullifyAmount);

// Para comercios Webpay Plus Mall
$result = transaction.nullify(
    $authorizationCode, $authorizedAmount, $buyOrder, $nullifyAmount,
    $storeCommerceCode);

```

```csharp
var transaction = new Webpay(configuration).NullifyTransaction;

// Para comercios Webpay Plus Normal
var result = transaction.nullify(
    authorizationCode, authorizedAmount, buyOrder, nullifyAmount);

// Para comercios Webpay Plus Mall
var result = transaction.nullify(
    authorizationCode, authorizedAmount, buyOrder, nullifyAmount,
    storeCommerceCode);
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorizationCode  <br> <i> xs:string </i> | Código de autorización de la transacción que se requiere anular. Si la transacción es de captura diferida, se debe usar el código obtenido al llamar a `capture()`. Largo máximo: 6.
authorizedAmount  <br> <i> xs:decimal </i> | Monto autorizado de la transacción que se requiere anular. Si la transacción es de captura diferida, se debe usar el monto capturado cuando se invocó a `capture()`. Largo máximo: 10.
buyOrder  <br> <i> xs:string </i> | Orden de compra de la transacción que se requiere anular. Largo máximo: 26.
nullifyAmount  <br> <i> xs:decimal </i> | Monto que se desea anular de la transacción. Largo máximo: 10.
commerceId  <br> <i> xs:long </i> | Código de comercio o tienda mall que realizó la transacción. Largo: 12.

**Respuesta**

```java
result.getToken();
result.getAuthorizationCode();
result.getAuthorizationDate();
result.getBalance();
result.getNullifiedAmount();
```

```php
$result->token;
$result->authorizationCode;
$result->authorizationDate;
$result->balance;
$result->nullifiedAmount;
```

```csharp
result.token;
result.authorizationCode;
result.authorizationDate;
result.balance;
result.nullifiedAmount;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Token de la transacción.
authorizationCode  <br> <i> xs:string </i> | Código de autorización de la anulación.
authorizationDate  <br> <i> xs:string </i> | Fecha y hora de la autorización.
balance  <br> <i> xs:decimal </i> | Saldo actualizado de la transacción (considera la venta menos el monto anulado).
nullifiedAmount  <br> <i> xs:decimal </i> | Monto anulado.

En caso de error pueden aparecer los siguientes códigos de error comunes para el método `nullify()`:

Código | Descripción
------ | -----------
304 | Validación de campos de entrada nulos
245 | Código de comercio no existe
22 | El comercio no se encuentra activo
316 | El comercio indicado no corresponde al certificado o no es hijo del comercio MALL en caso de transacciones MALL
308 | Operación no permitida
274 | Transacción no encontrada
16 | La transacción no permite anulación
292 | La transacción no está autorizada
284 | Periodo de anulación excedido
310 | Transacción anulada previamente
311 | Monto a anular excede el saldo disponible para anular
312 | Error genérico para anulaciones
315 | Error del autorizador
53 | La transacción no permite anulación parcial de transacciones con cuotas

## Webpay OneClick Normal

```java
WebpayOneClick transaction =
    new Webpay(configuration).getOneClickTransaction();
```

```php
$transaction = (new Webpay(configuration))->getOneClickTransaction();
```

```csharp
var transaction = new Webpay(configuration).OneClickTransaction;

```

WSDL: `/webpayserver/wswebpay/OneClickPaymentService?wsdl`

La modalidad de pago OneClick permite al tarjetahabiente realizar pagos en el
comercio sin la necesidad de ingresar cada vez información de la tarjeta de
crédito al momento de realizar la compra. El modelo de pago contempla un
proceso previo de inscripción o enrolamiento del tarjetahabiente, a través del
comercio, que desee utilizar el servicio. Este tipo de pago facilita la venta,
disminuye el tiempo de la transacción y reduce los riesgos de ingreso erróneo
de los datos del medio de pago.

El proceso de integración con Webpay OneClick consiste en desarrollar por parte
del comercio las llamadas a los servicios web dispuestos por Transbank para la
inscripción de los tarjetahabientes, así como para la realización de los
pagos.

#### Flujo de inscripción y pago

La inscripción es el proceso en el cual el tarjetahabiente registra los datos
de su tarjeta en Webpay OneClick para usarlo en compras futuras. Estos datos son
almacenados de forma segura en Transbank, y nunca son conocidos por el comercio.
Este proceso debe ser iniciado por la tienda del comercio y es requisito que el
cliente esté autenticado en la página del comercio antes de iniciar la
inscripción.

Proceso:

<img class="td_img-night" src="/images/diagrama-secuencia-oneclick-inscripcion.png" alt="Diagrama de secuencia inscripción Oneclick">

- El cliente se conecta y autentica en la página del comercio, mediante su
  nombre de usuario y clave.
- El cliente selecciona la opción de inscripción, la cual debe estar explicada
  en la página del comercio.
- El comercio consume un servicio web publicado por Transbank, donde entrega los
  datos del cliente y la URL de término; obtiene un token y URL de Webpay.
- El comercio envía el browser del cliente a la URL obtenida y pasa por
  parámetro el token (método POST).
- Webpay presenta el formulario de inscripción, este es similar al formulario
  de pago actual de Webpay Plus, para que el cliente ingrese los datos de su
  tarjeta.
- El cliente será autenticado por su banco emisor, de forma similar al flujo
  normal de pago. En este punto se realiza una transacción de $1 peso, la cual
  no se captura (no se verá reflejada en su estado de cuenta).
- Finalizada la inscripción, Webpay envía el browser del cliente a la URL
  entregada por el comercio, pasando por parámetro el token.
- El comercio debe consumir otro servicio web de Transbank, con el token, para
  obtener el resultado de la inscripción y el identificador de usuario, que
  debe utilizar en el futuro para realizar los pagos.
- El comercio presenta al cliente el resultado de la inscripción.

Después de realizado el proceso de inscripción, el comercio puede iniciar el proceso de pago cuando corresponda.

El pago es el proceso donde el comercio solicita el cargo de una compra a la
tarjeta de crédito de un usuario inscrito anteriormente, usando el
identificador entregado por Transbank al momento de la inscripción.

Los pagos en esta modalidad no requieren necesariamente la intervención del
usuario.

El monto del pago debe estar dentro de los límites establecidos para este tipo
de transacciones, el proceso interno es similar a un cargo normal de Webpay.
Proceso:

<img class="td_img-night" src="/images/diagrama-secuencia-oneclick-pago.png" alt="Diagrama de secuencia inscripción Oneclick">

- El cliente se conecta y autentica en la página o aplicación del comercio
  mediante su nombre de usuario y clave.
- El cliente selecciona la opción de pagar con Webpay Oneclick.
- El comercio usa el servicio web de pago, publicado por Transbank, entregando
  el identificador de usuario (que se obtuvo en la inscripción), el monto del
  pago y la orden de compra. Obtiene la respuesta con el código de
  autorización.
- El comercio presenta el resultado del pago al cliente.

### Crear una inscripción Webpay OneClick

Para realizar el primero de los procesos descritos (la inscripción), debe llamarse al método `initInscription()`

#### `initInscription()`

Permite realizar la inscripción del tarjetahabiente e información de su
tarjeta de crédito. Retorna como respuesta un token que representa la
transacción de inscripción y una URL (`urlWebpay`), que corresponde a la URL
de inscripción de OneClick.

Una vez que se llama a este servicio Web, el usuario debe ser redireccionado
vía POST a `urlWebpay` con parámetro `TBK_TOKEN` igual al token obtenido.

<aside class="notice">
Nota que a diferencia de Webpay Plus, donde el parámetro se llama `token_ws`, en
Webpay OneClick el parámetro se llama `TBK_TOKEN`.
</aside>

```java
OneClickInscriptionOutput initResult =
    transaction.initInscription(username, email, urlReturn);
```

```php
$initResult =
    transaction->initInscription(username, email, urlReturn);

```

```csharp
var initResult =
    transaction.initInscription(username, email, urlReturn);
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
username  <br> <i> xs:string </i> | Identificador del usuario registrado en el comercio. Largo máximo: 256.
email  <br> <i> xs:string </i> | Email del usuario registrado en el comercio. Largo máximo: 256.
responseURL  <br> <i> xs:string </i> | URL del comercio a la cual Webpay redireccionará posterior al proceso de inscripción. Largo máximo: 256.

**Respuesta**

```java
initResult.getToken();
initResult.getUrlWebpay();
```

```php
$initResult->token;
$initResult->urlWebpay;
```

```csharp
initResult.token;
initResult.urlWebpay;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Token de la inscripción.
urlWebpay  <br> <i> xs:string </i> | URL de formulario Webpay.

<aside class="notice">
Una vez que se llama a este webservice el usuario debe ser redireccionado vía
POST a `urlWebpay` con parámetro `TBK_TOKEN` igual al token.
</aside>

### Confirmar una inscripción Webpay OneClick

Una vez terminado el flujo de inscripción en Transbank el usuario es enviado a
la URL de fin de inscripción que definió el comercio. En ese instante el
comercio debe llamar a `finishInscription()`.

#### `finishInscription()`

Permite finalizar el proceso de inscripción del tarjetahabiente en OneClick.
Retorna el identificador del usuario en OneClick, el cual será utilizado para
realizar las transacciones de pago.

```java
OneClickFinishInscriptionOutput result =
    transaction.finishInscription(
        request.getParameter("TBK_TOKEN"));
```

```php
$result = $transaction->finishInscription(
    $request->input("TBK_TOKEN"));
```

```csharp
var result = transaction.finishInscription(tbkToken);
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Token de la inscripción.

**Respuesta**

```java
result.getResponseCode();
result.getAuthCode();
result.getCreditCardType().getValue();
result.getLast4CardDigits();
result.getTbkUser();
```

```php
$result->responseCode;
$result->authCode;
$result->creditCardType;
$result->last4CardDigits;
$result->tbkUser;
```

```csharp
result.responseCode;
result.authCode;
result.creditCardType;
result.last4CardDigits;
result.tbkUser;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
responseCode  <br> <i> xs:int </i> | Código de retorno del proceso de inscripción, donde 0 (cero) es aprobado.
authCode  <br> <i> xs:string </i> | Código que identifica la autorización de la inscripción.
creditCardType  <br> <i> creditCardType </i> | Indica el tipo de tarjeta inscrita por el cliente (Visa, AmericanExpress, MasterCard, Diners, Magna).
last4CardDigits  <br> <i> xs:string </i> | Los últimos 4 dígitos de la tarjeta ingresada por el cliente en la inscripción.
tbkUser  <br> <i> xs:string </i> | Identificador único de la inscripción del cliente en Webpay OneClick, que debe ser usado para realizar pagos o borrar la inscripción.

### Autorizar un pago con Webpay OneClick

Una vez realizada la inscripción, el comercio puede usar el `tbkUser` recibido
para realizar transacciones. Para eso debe usar el método `authorize()`.

#### `authorize()`

Permite realizar transacciones de pago. Retorna el resultado de la
autorización. Este método debe ser ejecutado cada vez que el usuario
selecciona pagar con OneClick en el comercio.

```java
OneClickPayOutput output = transaction.authorize(
    buyOrder, tbkUser, username, amount);
```

```php
$output = $transaction.authorize(
    buyOrder, tbkUser, username, amount);
```

```csharp
var output = transaction.authorize(
    buyOrder, tbkUser, username, amount);

```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
amount  <br> <i> xs:decimal </i> | Monto del pago en pesos.
buyOrder  <br> <i> xs:long </i> | Identificador único de la compra generado por el comercio.
tbkUser  <br> <i> xs:string </i> | Identificador único de la inscripción del cliente (devuelto por `finishInscription()`).
username  <br> <i> xs:string </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `initInscription()`).

**Respuesta**

```java
output.getAuthorizationCode();
output.getCreditCardType().value();
output.getLast4CardDigits();
output.getTransactionId();
output.getResponseCode();
```

```php
$output->authorizationCode;
$output->creditCardType;
$output->last4CardDigits;
$output->transactionId;
$output->responseCode;
```

```csharp
output.authorizationCode;
output.creditCardType;
output.last4CardDigits;
output.transactionId;
output.responseCode;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorizationCode  <br> <i> xs:string </i> | Código de autorización de la transacción de pago.
creditCardType  <br> <i> creditCardType </i> | Indica el tipo de tarjeta que fue inscrita por el cliente (Visa, AmericanExpress, MasterCard, Diners, Magna).
last4CardDigits  <br> <i> xs:string </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción.
transactionId  <br> <i> xs:long </i> | Identificador único de la transacción de pago.
responseCode  <br> <i> xs:int </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. <br> -1, -2, -3, -4, -5, -6, -7, -8: Rechazo <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.

### Reversar un pago Webpay OneClick

Este proceso permite reversar una venta cuando esta no pudo concretarse, dentro del mismo día contable, con la finalidad de anular un cargo realizado al cliente. Para esto se debe consumir el método `codeReverseOneClick()` con la orden de compra de la transacción a reversar.

#### `codeReverseOneClick()`

Permite reversar una transacción de venta autorizada con anterioridad. Este
método retorna como respuesta un identificador único de la transacción de
reversa.

```java
boolean success = transaction.reverseTransaction(buyOrder);
```

```php
$result = $transaction->reverseTransaction($buyOrder);
```

```csharp
var result = transaction.reverseTransaction(buyOrderLong);
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buyOrder  <br> <i> xs:long </i> | Orden de compra de la transacción a reversar.

**Respuesta**

```java
// El SDK Java solo entrega el booleano de estado, no el código de reversa :(
success;
```

```php
// El SDK PHP solo entrega el booleano de estado, no el código de reversa :(
$result->success;
```

```csharp
result.reversed;
result.reverseCode;
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
reversed  <br> <i> xs:boolean </i> | Indica si tuvo éxito la reversa.
reverseCode  <br> <i> xs:long </i> | Identificador único de la transacción de reversa.

### Anular un pago Webpay OneClick

En caso que ya no sea el mismo día contable y se requiera dejar sin efecto una
venta, es posible anular un pago realizado con Webpay OneClick Normal usando [el
mismo método de anulación Webpay Plus](#anulacion-webpay-plus).

### Eliminar una inscripción Webpay OneClick

En el caso que el comercio requiera eliminar la inscripción de un usuario en
Webpay OneClick ya sea por la eliminación de un cliente en su sistema o por la
solicitud de este para no operar con esta forma de pago, el comercio deberá
invocar a `removeUser()` con el identificador de usuario entregado en la
inscripción.

#### `removerUser()`

Permite eliminar una inscripción de usuario en Transbank

```java
boolean success = transaction.removeUser(tbkUser, username);
```

```php
$success = $transaction->removeUser($tbkUser, $username);
```

```csharp
var success = transaction.RemoveUser(tbkUser, username);
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tbkUser  <br> <i> xs:string </i> | Identificador único de la inscripción del cliente
username  <br> <i> xs:string </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `initInscription()`).

**Respuesta**

El boolean de respuesta será `true` en caso de éxito y `false` en caso contrario.

## Webpay OneClick Mall

> Los SDKs no soportan aún los servicios Webpay OneClick Mall.

WSDL: `/WSWebpayTransaction/cxf/WSOneClickMulticodeService?wsdl`

#### Flujo de inscripción y pago

El flujo de Webpay OneClick Mall es en general el mismo que el de [Webpay
OneClick Normal](#webpay-oneclick-normal) tanto de cara al tarjeta habiente como
de cara al integrador.

Las diferencias son:

- El usuario debe estar registrado en la página del comercio "mall" agrupador,
  pero las transacciones son a nombre de loas "tiendas" del mall.
- Se pueden indicar múltiples transacciones a autorizar en una misma operación.
- Se debe verificar por separado el resultado de cada una de esas transacciones
  individualmente, pues es posible que el emisor de la tarjeta autorice algunas
  y otras no.

### Crear una inscripción Webpay OneClick Mall

Para iniciar la inscripción debe usarse el método `initInscription()`

#### `initInscription()`

Permite gatillar el inicio del proceso de inscripción.

> Los SDKs no soportan aún los servicios Webpay OneClick Mall.

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
username  <br> <i> xs:string </i> | Identificador del usuario registrado en el comercio.
email  <br> <i> xs:string </i> | Email del usuario registrado en el comercio.
returnUrl  <br> <i> xs:string </i> | URL a la que será enviado el cliente finalizado el proceso de inscripción.

**Respuesta**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Identificador, único, del proceso de inscripción. Debe ser enviado por parámetro (`TBK_TOKEN`) a la URL `urlInscriptionForm`.
urlInscriptionForm  <br> <i> xs:string </i> | URL de Webpay para iniciar la inscripción.

<aside class="notice">
Una vez que se llama a este webservice el usuario debe ser redireccionado vía
POST a `urlInscriptionForm` con parámetro `TBK_TOKEN` igual al token.
</aside>

### Confirmar una inscripción Webpay OneClick Mall

Una vez terminado el flujo de inscripción en Transbank el usuario es enviado a
la URL de fin de inscripción que definió el comercio (`returnUrl`). En ese
instante el comercio debe llamar a `finishInscription()`.

<aside class="warning">
El comercio tendrá un máximo de 60 segundos para llamar a este método luego
de recibir el token en la URL de fin de inscripción (`returnUrl`). Pasados los
60 segundos sin llamada a finishInscription, la inscripción en curso junto con
el usuario serán eliminados.
</aside>

#### `finishInscription()`

Permite finalizar el proceso de inscripción obteniendo el usuario tbk.

> Los SDKs no soportan aún los servicios Webpay OneClick Mall.

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Identificador del proceso de inscripción. Es entregado por Webpay en la respuesta del método `initInscription()`.

**Respuesta**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
responseCode  <br> <i> xs:int </i> | Código de respuesta del proceso de inscripción, donde 0 (cero) es aprobado.
authorizationCode  <br> <i> xs:string </i> | Código que identifica la autorización de la inscripción.
creditCardType  <br> <i> creditCardType </i> | Indica la marca de la tarjeta de crédito que fue inscrita por el cliente (Visa, AmericanExpress, MasterCard, Diners, Magna)
cardNumber  <br> <i> xs:string </i> | Indica los últimos 4 dígitos y/o BIN de la tarjeta de crédito utilizada.
cardExpirationDate  <br> <i> xs:string </i> | Indica la fecha de expiración de la tarjeta de crédito utilizada.
cardOrigin  <br> <i> cardOrigin </i> | Indica si la tarjeta de crédito utilizada es nacional (NATIONAL_CARD) o extranjera (FOREIGN_CARD).
tbkUser  <br> <i> xs:string </i> | Identificador único de la inscripción del cliente, este debe ser usado para realizar pagos o eliminar la inscripción.

### Autorizar un pago con Webpay OneClick Mall

Una vez realizada la inscripción, el comercio puede usar el `tbkUser` recibido
para realizar transacciones. Para eso debes usar el método `authorize()`.

#### `authorize()`

Permite autorizar un pago.

> Los SDKs no soportan aún los servicios Webpay OneClick Mall.

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buyOrder  <br> <i> xs:long </i> | Identificador único de la operación para el comercio mall.
tbkUser  <br> <i> xs:string </i> | Identificador único de la inscripción del cliente (devuelto por `finishInscription()`).
username  <br> <i> xs:string </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `initInscription()`).
storesInput  <br> <i> wsOneClickMulticodeStorePaymentInput </i> | Lista de objetos con la información de cada transacción de las tiendas del mall.
storesInput[].commerceId  <br> <i> xs:long </i> | Código de comercio hijo de la tienda.
storesInput[].buyOrder  <br> <i> xs:string </i> | Identificador único de la compra generado por el comercio hijo (tienda).
storesInput[].amount  <br> <i> xs:decimal </i> | Monto de la sub-transacción de pago. En pesos o dólares según configuración comercio mall padre.
storesInput[].sharesNumber  <br> <i> xs:int </i> | Cantidad de cuotas de la sub-transacción de pago.

<aside class="warning">
Cualquier valor distinto de número en `sharesNumber` (incluyendo letras,
inexistencia del campo o nulo) será asumido como cero, es decir "Sin cuotas".
</aside>

**Respuesta**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
commerceId  <br> <i> xs:long </i> | Código de comercio del comercio padre.
buyOrder  <br> <i> xs:string </i> | Orden de compra generada por el comercio padre.
settlementDate  <br> <i> xs:string </i> | Fecha contable de la autorización del pago.
authorizationDate  <br> <i> xs:dateTime </i> | Fecha completa (timestamp) de la autorización del pago.
storesOutput  <br> <i> wsOneClickMulticodeStorePaymentOutput </i> | Lista con el resultado de cada transacción de las tiendas hijas.
storesOutput[].commerceId  <br> <i> xs:long </i> | Código de comercio del comercio hijo (tienda).
storesOutput[].buyOrder  <br> <i> xs:string </i> | Orden de compra generada por el comercio hijo para la sub-transacción de pago.
storesOutput[].amount  <br> <i> xs:decimal </i> | Monto de la sub-transacción de pago.
storesOutput[].authorizationCode  <br> <i> xs:string </i> | Código de autorización de la sub-transacción de pago.
storesOutput[].paymentType  <br> <i> xs:string </i> | [Tipo](/producto/webpay#tipos-de-pago) (VC, NC, SI, etc.) de la sub-transacción de pago.
storesOutput[].responseCode  <br> <i> xs:int </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. <br> -1, -2, -3, -4, -5, -6, -7, -8: Rechazo <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
storesOutput[].sharesNumber  <br> <i> xs:int </i> | Cantidad de cuotas de la sub-transacción de pago.
storesOutput[].shareAmount  <br> <i> xs:decimal </i> | Monto por cuota de la sub-transacción de pago.

### Reversar o Anular un pago Webpay OneClick Mall

Para Webpay OneClick Mall hay dos operaciones diferentes para dejar sin efecto
transacciones autorizadas: La reversa y la anulación.

**La reversa** se aplica para **problemas operacionales (lado comercio) o de
comunicación entre comercio y Transbank que impidan recibir a tiempo la
respuesta de una autorización**. En tal caso el comercio **debe** intentar
reversar la transacción de autorización para evitar un posible descuadre entre
comercio y Transbank. La reversa funciona sobre la operación completa del mall,
lo que significa que **todas las transacciones realizadas en la operación mall
serán reversadas**. Y sólo puede ser usada hasta 30 segundos después de la
llamada a `authorize()`

**La anulación**, en cambio, actúa individualmente sobre las transacciones de
las _tiendas_ de un mall. Por ende, **la anulación es la operación correcta a
utilizar para fines financieros**, de manera de anular un cargo ya realizado.
También es posible *reversar una anulación* debido a problemas operacionales
(por ejemplo un error de comunicación al momento de anular, que le impida al
comercio saber si Transbank recibió la anulación).

Para llevar a cabo la reversa, el comercio debe usar el método `reverse()`. Para la anulación, se debe usar el método `nullify()`. Y para reversar una anulación existe el método `reverseNullification() `

#### `reverse()`

Permite reversar una operación de autorización.

> Los SDKs no soportan aún los servicios Webpay OneClick Mall.

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buyOrder  <br> <i> xs:string </i> | Orden de compra generada por el comercio padre (mall) para la operación a reversar.

**Respuesta**

La respuesta es una lista con tantos elementos como transacciones haya tenido la
operación identificada por `buyOrder`:

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
[].commerceId   <br> <i> xs:long </i> | Código de comercio del comercio hijo (tienda).
[].buyOrder  <br> <i> xs:string </i> | Orden de compra del comercio hijo para la sub-transacción de pago.
[].reversed  <br> <i> xs:boolean </i> | Indica si la reversa se realizó correctamente o no.
[].reverseCode  <br> <i> xs:string </i> | Identificador único de la transacción de reversa.

#### `nullify()`

Permite anular un pago.

> Los SDKs no soportan aún los servicios Webpay OneClick Mall.

**Parámetros**

Nombre | Descripción
------ | -----------
commerceId | Identificador del comercio hijo (tienda) para la sub-transacción de pago a anular.
buyOrder  | Orden de compra generada por el comercio hijo, para la sub-transacción de pago a anular.
authorizedAmount | Monto de la sub-transacción de pago a anular.
authorizationCode | Código de autorización de la sub-transacción de pago a anular.
nullifyAmount | Monto a anular de la sub-transacción de pago. Puede ser un monto parcial o monto total.

**Respuesta**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Identificador único de la transacción de anulación.
buyOrder  <br> <i> xs:string </i> | Orden de compra de la sub-transacción de pago anulada.
commerceId  <br> <i> xs:long </i> | Código de comercio del comercio hijo (tienda).
authorizationCode  <br> <i> xs:string </i> | Código de autorización de la transacción de anulación.
authorizationDate  <br> <i> xs:dateTime </i> | Fecha de la autorización de la transacción de anulación.
nullifiedAmount  <br> <i> xs:decimal </i> | Monto anulado.
balance   <br> <i> xs:decimal </i> | Monto restante de la sub-transacción de pago original: monto inicial – monto anulado.

#### `reverseNullification()`

Permite reversar una anulación.

> Los SDKs no soportan aún los servicios Webpay OneClick Mall.

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buyOrder  <br> <i> xs:string </i> | Orden de compra generada por el comercio hijo, para la sub-transacción de pago anulada.
commerceId  <br> <i> xs:long </i> | Código de comercio del comercio hijo (tienda).
nullifyAmount  <br> <i> xs:decimal </i> | Monto anulado en la transacción de anulación que se intenta reversar.

**Respuesta**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
reversed  <br> <i> xs:boolean </i> | Indica si la reversa se realizó correctamente o no.
reverseCode  <br> <i> xs:long </i> | Identificador único de la transacción de reversa.

<aside class="warning">
La llamada a este método y por lo tanto la operación de reversa de anulación,
sólo será válida dentro de los 30 segundos posteriores a la llamada del
método `nullify()`.
</aside>

### Eliminar una inscripción Webpay OneClick Mall

En el caso que el comercio requiera eliminar la inscripción de un usuario en Webpay OneClick Mall ya sea por la eliminación de un cliente en su sistema o por la solicitud de este para no operar con esta forma de pago, el comercio deberá invocar a `removeInscription()` con el identificador de usuario entregado en la inscripción.

#### `removeInscription()`

Permite eliminar una inscripción de usuario en Webpay OneClick Mall

> Los SDKs no soportan aún los servicios Webpay OneClick Mall.

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tbkUser  <br> <i> xs:string </i> | Identificador único de la inscripción del cliente.
username  <br> <i> xs:string </i> | Nombre de usuario, del cliente, en el sistema del comercio.

**Respuesta**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
result  <br> <i> xs:boolean </i> | Indica si la eliminación se realizó correctamente o no.

### Captura Diferida Webpay OneClick Mall
> Los SDKs no soportan aún los servicios Webpay OneClick Mall.

Una transacción OneClick Mall permite que el tarjetahabiente registre su
tarjeta de la misma forma en que ocurre con una transacción OneClick, asociando
dicha inscripción a un comercio padre. Ahora, una vez realizada la inscripción, 
el comercio padre tiene permitido autorizar transacciones sin captura para
los comercios “hijo” registrados que tengan habilitado captura diferida.
Además posterior a la autorización tiene permitido capturar dicho monto reservado.
La autorización se encarga de validar si es posible realizar el cargo a la
cuenta asociada a la tarjeta de crédito realizando en el mismo acto la reserva
de monto de la transacción.
La captura hace efectiva la reserva hecha previamente o cargo en la cuenta de
crédito asociada a la tarjeta del titular.
Estas modalidades, por separado, solo son válidas para tarjetas de crédito.

Para realizar esa captura explícita debe usarse el método `capture()`

#### `capture()`

Este método permite a los comercios OneClick Mall habilitados, poder
realizar capturas diferidas de una transacción previamente autorizada. El método
contempla una única captura por cada autorización. Para ello se deberá indicar los
datos asociados a la transacción de venta y el monto requerido para capturar, el cual
debe ser menor o igual al monto originalmente autorizado.
Para capturar una transacción, ésta debe haber sido creada por un código de
comercio configurado para captura diferida. De esta forma la transacción estará
autorizada pero requerirá una captura explícita posterior para confirmar la
transacción.


**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorizationCode  <br> <i> xs:string </i> | Código de autorización de la transacción que se requiere capturar. Largo máximo: 6.
buyOrder  <br> <i> xs:string </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
commerceId  <br> <i> xs:long </i> | Código de tienda mall que realizó la transacción. Largo: 12.
capturedAmount  <br> <i> xs:decimal </i> | Monto que se desea capturar. Largo máximo: 10.

<aside class="notice">
El método `capture()` debe ser invocado siempre indicando el código del
comercio de la tienda virtual específica.
</aside>

**Respuesta**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Token de la transacción.
authorizationCode  <br> <i> xs:string </i> | Código de autorización de la captura diferida.
authorizationDate  <br> <i> xs:string </i> | Fecha y hora de la autorización.
capturedAmount  <br> <i> xs:decimal </i> | Monto capturado.

<aside class="notice">
En caso de error apareceran los mismos códigos exclusivos del método `capture()`
para captura simpultanea.
</aside>
