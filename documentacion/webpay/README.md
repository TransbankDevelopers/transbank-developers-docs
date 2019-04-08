# Webpay

## Webpay Plus
<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-plus' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugin/webpay' tbk-link-name='Plugins'></div>
</div>

### Crear una transacción

Para una transacción asociada a un único comercio (también conocida como
"normal"), lo primero que necesitas es preparar una instancia de `WebpayNormal`
con la `Configuration` que incluye el código de comercio y los certificados a
usar.

Una forma fácil de comenzar es usar la configuración para pruebas que viene
incluida en el SDK:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.webpay.configuration.Configuration;
import cl.transbank.webpay.Webpay;
import cl.transbank.webpay.WebpayNormal;
// ...

WebpayNormal transaction =
    new Webpay(Configuration.forTestingWebpayPlusNormal())
    .getNormalTransaction();
```

```php
use Transbank\Webpay\Configuration;
use Transbank\Webpay\Webpay;
// ...

$transaction = (new Webpay(Configuration::forTestingWebpayPlusNormal()))
               ->getNormalTransaction();
```

```csharp
using Transbank.Webpay;
//...

var transaction =
    new Webpay(Configuration.ForTestingWebpayPlusNormal()).NormalTransaction;
```

<aside class="notice">
Como necesitarás ese objeto `transaction` en múltiples ocasiones, es buena idea
encapsular la lógica que lo genera en algún método que puedas reutilizar.
</aside>

Una vez que ya cuentas con esa preparación, puedes iniciar transacciones:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpay.wswebpay.service.WsInitTransactionOutput;
// ...
double amount = 1000;
// Identificador que será retornado en el callback de resultado:
String sessionId = "mi-id-de-sesion";
// Identificador único de orden de compra:
String buyOrder = String.valueOf(Math.abs(new Random().nextLong()));
String returnUrl = "https://callback/resultado/de/transaccion";
String finalUrl = "https://callback/final/post/comprobante/webpay";
WsInitTransactionOutput initResult = transaction.initTransaction(
        amount, sessionId, buyOrder, returnUrl, finalUrl);

String formAction = initResult.getUrl();
String tokenWs = initResult.getToken();
```

```php
$amount = 1000;
// Identificador que será retornado en el callback de resultado:
$sessionId = "mi-id-de-sesion";
// Identificador único de orden de compra:
$buyOrder = strval(rand(100000, 999999999));
$returnUrl = "https://callback/resultado/de/transaccion";
$finalUrl = "https://callback/final/post/comprobante/webpay";
$initResult = $transaction->initTransaction(
        $amount, $buyOrder, $sessionId, $returnUrl, $finalUrl);

$formAction = $initResult->url;
$tokenWs = $initResult->token;
```

```csharp
using Transbank.Webpay;
//...

var amount = 1000;
// Identificador que será retornado en el callback de resultado:
var sessionId = "mi-id-de-sesion";
// Identificador único de orden de compra:
var buyOrder = new Random().Next(100000, 999999999).ToString();
var returnUrl = "https://callback/resultado/de/transaccion";
var finalUrl = "https://callback/final/post/comprobante/webpay";
var initResult = transaction.initTransaction(
        amount, buyOrder, sessionId, returnUrl, finalUrl);

var formAction = initResult.url;
var tokenWs = initResult.token;
```

La URL y el token retornados te indican donde debes redirigir al usuario para
que comience el flujo de pago. Esta redirección debe ser vía `POST` por lo que
deberás crear un formulario web con un campo `token_ws` hidden y enviarlo
programáticamente para entregar el control a Webpay.

<aside class="notice">
Tip: En el ambiente de integración puedes usar la tarjeta VISA 4051885600446623
para hacer pruebas simples de éxito. El CVV es 123 y la fecha de vencimiento
cualquiera superior a la fecha actual. Luego para la autenticación bancaria usa
el RUT 11.111.111-1 y la clave 123. Para pruebas exhaustivas [consulta todas las
tarjetas de prueba en la sección de
Ambientes](/documentacion/como_empezar#ambientes).
</aside>

### Confirmar una transacción

Una vez que el tarjetahabiente ha pagado (o declinado, o ha ocurrido un error),
Webpay retornará el control vía `POST` a la URL que indicaste en el `returnUrl`.
Recibirás también el parámetro `token_ws` que te permitirá conocer el resultado
de la transacción:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpay.wswebpay.service.TransactionResultOutput;
import com.transbank.webpay.wswebpay.service.WsTransactionDetailOutput;
// ...
TransactionResultOutput result =
    transaction.getTransactionResult(request.getParameter("token_ws"));
WsTransactionDetailOutput output = result.getDetailOutput().get(0);
if (output.getResponseCode() == 0) {
    // Transaccion exitosa, puedes procesar el resultado con el contenido de
    // las variables result y output.
}
```

```php
$result = $transaction->getTransactionResult($request->input("token_ws"));
$output = $result->detailOutput;
if ($output->responseCode == 0) {
    // Transaccion exitosa, puedes procesar el resultado con el contenido de
    // las variables result y output.
}
```

```csharp
using Transbank.Webpay;
//...

var result = transaction.getTransactionResult(tokenWs);
var output = result.detailOutput[0];
if (output.responseCode == 0) {
    // Transaccion exitosa, puedes procesar el resultado con el contenido de
    // las variables result y output.
}
```

> **Importante**: El SDK se encarga de que al mismo tiempo que se obtiene el
> resultado de la transacción se haga el _acknowledge_ a Transbank de manera que
> no haya posibilidad de que la transacción se revierta. Si luego necesitas que
> la transacción no se lleve a cabo (por ejemplo porque ya no tienes stock o
> porque se generó un error en tu lógica de negocio que entrega el producto o
> servicio), deberás [anular la transacción](/referencia/webpay#anulacion-webpay-plus).

En el caso exitoso deberás llevar el control vía `POST` nuevamente a Webpay para
que el tarjetahabiente vea el comprobante que le deja claro que se ha realizado
el cargo en su tarjeta. Nuevamente deberás generar un formulario con el
`token_ws` como un campo hidden. La URL para redirigir la debes obtener desde
`result.getUrlRedirection()`.

Finalmente después del comprobante Webpay redirigirá otra vez (vía `POST`) a tu
sitio, esta vez a la URL que indicaste en el `finalUrl` cuando iniciaste la
transacción. Tal como antes, recibirás el `token_ws` que te permitirá
identificar la transacción y mostrar un comprobante o página de éxito a tu
usuario. Con eso habrás completado el flujo "feliz" en que todo funciona.

En [la referencia detallada de Webpay Plus puedes ver cada paso del flujo, incluyendo
los casos de borde que también debes manejar](https://www.transbankdevelopers.cl/referencia/webpay#webpay-plus-normal).

## Webpay OneClick
<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-oneclick-normal' tbk-link-name='Referencia Api'></div>
</div>

### Crear una inscripción

Para usar Webpay Onelick en transacciones asociadas a un único comercio, lo
primero que necesitas es preparar una instancia de `WebpayOneClick` con la
`Configuration` que incluye el código de comercio y los certificados a
usar

Una forma fácil de comenzar es usar la configuración para pruebas que viene
incluida en el SDK:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.webpay.configuration.Configuration;
import cl.transbank.webpay.Webpay;
import cl.transbank.webpay.WebpayOneClick;
// ...

WebpayOneClick transaction =
    new Webpay(Configuration.forTestingWebpayOneClickNormal())
    .getOneClickTransaction();
```

```php
use Transbank\Webpay\Configuration;
use Transbank\Webpay\Webpay;
// ...

$transaction =
    (new Webpay(Configuration::forTestingWebpayOneClickNormal()))
    ->getOneClickTransaction();
```

```csharp
using Transbank.Webpay;
//...

var transaction =
    new Webpay(Configuration.ForTestingWebpayOneClickNormal())
    .OneClickTransaction;
```

<aside class="notice">
Como necesitarás ese objeto `transaction` en múltiples ocasiones, es buena idea
encapsular la lógica que lo genera en algún método que puedas reutilizar.
</aside>

Una vez que ya cuentas con esa preparación, puedes iniciar transacciones:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpayserver.webservices.OneClickInscriptionOutput;
//...

// Identificador del usuario en el comercio
String username = "pepito"
// Correo electrónico del usuario
String email = "pepito@gmail.com";
String urlReturn = "https://callback/resultado/de/transaccion";
OneClickInscriptionOutput initResult =
    transaction.initInscription(username, email, urlReturn);
String formAction = initResult.getUrlWebpay();
String tbkToken = initResult.getToken();
```

```php
// Identificador del usuario en el comercio
$username = "pepito"
// Correo electrónico del usuario
$email = "pepito@gmail.com";
$urlReturn = "https://callback/resultado/de/transaccion";
$initResult = $transaction->initInscription($username, $email, $urlReturn);
$formAction = $initResult->urlWebpay;
$tbkToken = $initResult->token;
```

```csharp
using Transbank.Webpay;
//...

// Identificador del usuario en el comercio
var username = "pepito"
// Correo electrónico del usuario
var email = "pepito@gmail.com";
var urlReturn = "https://callback/resultado/de/transaccion";
var initResult = transaction.initInscription(username, email, urlReturn);
var formAction = initResult.urlWebpay;
var tbkToken = initResult.token;
```

Tal como en el caso de Webpay Plus, debes redireccionar vía `POST` el
navegador del usuario a la url retornada en `initInscription`. **A diferencia
de Webpay Plus, acá el nombre del parámetro que contiene el token se debe
llamar `TBK_TOKEN`**.

### Confirmar una inscripción

Una vez que el usuario autorice la inscripción, retornará el control al
comercio vía `POST` en la url indicada en `urlReturn`, con el parámetro
`TBK_TOKEN` identificando la transacción. Con esa información se puede
finalizar la inscripción:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpayserver.webservices.OneClickFinishInscriptionOutput;
//...

OneClickFinishInscriptionOutput result =
    transaction.finishInscription(tbkToken);
if (result.getResponseCode() == 0) {
    // Inscripcion exitosa.
    // Ahora puedes usar result.tbkUser para autorizar transacciones
    // oneclick sin nueva intervención del usuario.
}
```

```php
$result = $transaction->finishInscription($tbkToken);
if ($result->responseCode == 0) {
    // Inscripcion exitosa.
    // Ahora puedes usar $result->tbkUser para autorizar transacciones
    // oneclick sin nueva intervención del usuario.
}
```

```csharp
using Transbank.Webpay;
//...

var result = transaction.finishInscription(tbkToken);
if (result.responseCode == 0) {
    // Inscripcion exitosa.
    // Ahora puedes usar result.tbkUser para autorizar transacciones
    // oneclick sin nueva intervención del usuario.
}
```

Con eso habrás completado el flujo "feliz" en que todo funciona OK. En [la
referencia detallada de Webpay OneClick puedes ver cada paso del flujo,
incluyendo los casos de borde que también debes
manejar](https://www.transbankdevelopers.cl/referencia/webpay#webpay-oneclick-normal).


### Realizar transacciones

Finalmente, puedes autorizar transacciones usando el `tbkUser` retornado:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpayserver.webservices.OneClickPayOutput;
//...

// Identificador único de orden de compra generado por el comercio:
Long buyOrder = Math.abs(new Random().nextLong());
String tbkUser = tbkUserRetornadoPorFinishInscription;
String username = "pepito"; // El mismo usado en initInscription.
BigDecimal amount = BigDecimal.valueof(50000);
OneClickPayOutput output =
    transaction.authorize(buyOrder, tbkUser, username, amount);
if (output.getResponseCode() == 0) {
    // Transacción exitosa, procesar output
}
```

```php
$buyOrder = rand(100000, 999999999);
$tbkUser = $tbkUserRetornadoPorFinishInscription;
$username = "pepito"; // El mismo usado en initInscription.
$amount = 50000;
$output = $transaction->authorize($buyOrder, $tbkUser, $username, $amount);
if ($output->responseCode == 0) {
    // Transacción exitosa, procesar $output
}
```

```csharp
using Transbank.Webpay;
//...

var buyOrder = new Random().next(100000, 999999999);
var tbkUser = tbkUserRetornadoPorFinishInscription;
var username = "pepito"; // El mismo usado en initInscription.
var amount = BigDecimal.valueof(50000);
var output = transaction.authorize(buyOrder, tbkUser, username, amount);
if (output.responseCode == 0) {
    // Transacción exitosa, procesar output
}
```

## Credenciales y Ambiente

Para Webpay, las credenciales del comercio (código de comercio y certificados)
varían según el subproducto usado (Webpay Plus, Webpay Plus Mall, Webpay OneClick,
Webpay OneClick Mall). Asimismo, varían las credenciales si la captura es
diferida. Y también varían si la moneda a manejar es pesos chilenos (CLP) o
dólares (USD).

Por lo tanto, es clave que antes de operar con las clases que permiten
realizar transacciones (`WebpayNormal`, `WebpayMallNormal`, `WebpayCapture`,
`WebpayNullify`, `WebpayOneClick`) se configure correctamente el objeto `Webpay` desde donde se
obtienen dichas instancias.

Ese objeto `Webpay` se configura en base a un objeto `Configuration`. Y si bien
para hacer pruebas iniciales pueden usarse las credenciales pre-configuradas
(como se puede ver en todos los ejemplos anteriores), para poder superar el
[proceso de
validación](/documentacion/como_empezar#el-proceso-de-validacion-y-puesta-en-produccion)
en el ambiente de integración será necesario configurar explícitamente tu código
de comercio y certificados:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.webpay.configuration.Configuration;
import cl.transbank.webpay.Webpay;
import cl.transbank.webpay.WebpayNormal;
import cl.transbank.webpay.WebpayMallNormal;
import cl.transbank.webpay.WebpayCapture;
import cl.transbank.webpay.WebpayNullify;
import cl.transbank.webpay.WebpayOneClick;


//...

Configuration configuration = new Configuration();
configuration.setCommerceCode("12345"); // acá va tu código de comercio
configuration.setPrivateKey( // pega acá la llave privada de tu certificado
    "-----BEGIN RSA PRIVATE KEY-----\n" +
    "MIIEpQIBAAKCAQEA0ClVcH8RC1u+KpCPUnzYSIcmyXI87REsBkQzaA1QJe4w/B7g\n" +
    //....
    "MtdweTnQt73lN2cnYedRUhw9UTfPzYu7jdXCUAyAD4IEjFQrswk2x04=\n" +
    "-----END RSA PRIVATE KEY-----");
configuration.setPublicCert( // pega acá tu certificado público
    "-----BEGIN CERTIFICATE-----\n" +
    "MIIDujCCAqICCQCZ42cY33KRTzANBgkqhkiG9w0BAQsFADCBnjELMAkGA1UEBhMC\n" +
    //....
    "-----END CERTIFICATE-----");

Webpay webpay = new Webpay(configuration);
// Ahora puedes obtener las instancias de las transacciones
// que usarás, por ejemplo:
WebpayOneClick oneClickTransaction = webpay.getOneClickTransaction();
```

```php
use Transbank\Webpay\Configuration;
use Transbank\Webpay\Webpay;
// ...

$configuration = new Configuration();
$configuration->setCommerceCode("12345"); // acá va tu código de comercio
$configuration->setPrivateKey( // pega acá la llave privada de tu certificado
    "-----BEGIN RSA PRIVATE KEY-----\n" +
    "MIIEpQIBAAKCAQEA0ClVcH8RC1u+KpCPUnzYSIcmyXI87REsBkQzaA1QJe4w/B7g\n" +
    //....
    "MtdweTnQt73lN2cnYedRUhw9UTfPzYu7jdXCUAyAD4IEjFQrswk2x04=\n" +
    "-----END RSA PRIVATE KEY-----");
$configuration->setPublicCert( // pega acá tu certificado público
    "-----BEGIN CERTIFICATE-----\n" +
    "MIIDujCCAqICCQCZ42cY33KRTzANBgkqhkiG9w0BAQsFADCBnjELMAkGA1UEBhMC\n" +
    //....
    "-----END CERTIFICATE-----");

$webpay = new Webpay($configuration);
// Ahora puedes obtener las instancias de las transacciones
// que usarás, por ejemplo:
$oneClickTransaction = $webpay->getOneClickTransaction();
```

```csharp
using Transbank.Webpay;
//...

var configuration = new Configuration()
{
    CommerceCode = "12345", // acá va tu código de comercio
    PrivateCertPfxPath = @"C:\Certs\certificado.pfx", // pega acá la ruta a tu archivo pfx o p12
    Password = "secret123" // pega acá el secreto con el cual se genero el archivo pfx o p12
};

var webpay = new Webpay(configuration);
// Ahora puedes obtener las instancias de las transacciones
// que usarás, por ejemplo:
var oneClickTransaction = webpay.OneClickTransaction;
```

<aside class="warning">
A diferencia de otros SDK, en .NET debes especificar la ruta a un archivo pfx o p12
el cual debes generar tu a partir de tu llave privada y certificado público.

Puedes mirar el siguiente enlace para obtener una guía rápida de como generar tu 
propio archivo: [Crear archivo pfx usando openssl](https://www.ssl.com/how-to/create-a-pfx-p12-certificate-file-using-openssl/) 
</aside>

### Apuntar a producción

Para cambiar el ambiente al que apunta el SDK (que por defecto es integración),
también debes usar el objeto `Configuration` (antes de crear una instancia de
`Webpay`):

<div class="language-simple" data-multiple-language></div>

```java
Configuration configuration = new Configuration();
configuration.setEnvironment(Webpay.Environment.PRODUCCION);
// agregar también configuración del código de comercio y certificados
```

```php
use Transbank\Webpay\Configuration;
// ...

$configuration = new Configuration();
$configuration->setEnvironment("PRODUCCION");
// agregar también configuración del código de comercio y certificados
```

```csharp
using Transbank.Webpay;
//...

Configuration configuration = new Configuration()
{
    Environment = "PRODUCCION";
    // agregar también configuración del código de comercio y certificados
}
```

<aside class="warning">
El SDK se encarga de configurar el certificado público de
Transbank correspondiente al ambiente seleccionado. Pero es tu responsabilidad
actualizar periódicamente la versión del SDK para recibir actualizaciones a
dicho certificado. De lo contrario, expirará el certificado y dejarás de poder
operar con Transbank.
</aside>

## Conciliación de Transacciones
Una vez hayas realizado transacciones en producción quedará un historial de transacciones que puedes revisar entrando a [www.transbank.cl](https://www.transbank.cl/). Si lo deseas realizar una conciliación entre tu sistema y el reporte que entrega el portal.

Para realizar la conciliación debes seguir los siguientes pasos:

1. Iniciar sesión con tu usuario y contraseña en [www.transbank.cl](https://www.transbank.cl/)
   
2. Luego, en el menú principal presionar "Webpay" y luego "Reporte transaccional". ![Paso 2](/images/documentacion/conciliacion1.png)
   
3. En la parte superior de la ventana puedes encontrar un buscador que te ayudará a filtrar, según los parámetros que gustes, las transacciones que quieras cuadrar. Para encontrar las transacciones de Webpay Plus, en producto, debes seleccionar Webpay3G, en caso de querer las de Webpay OneClick selecciona "OneClick" ![Paso 3](/images/documentacion/conciliacion2.png)

4. Dentro de la tabla en la imagen anterior puedes presionar el número de orden de compra para abrir los detalles de la transacción. Es en esta sección donde podrás encontrar y conciliar la mayoría de los parámetros devueltos al confirmar una transacción. ![Paso 4](/images/documentacion/conciliacion3.png)

5. Sólo queda realizar la conciliación. A continuación puedes ver una lista de parámetros que recibirás al momento de confirmar una transacción y a que fila de la tabla "Detalles de la transacción" corresponden (la lista completa de parámetros de Webpay Plus la puedes encontrar [acá](/referencia/webpay#confirmar-una-transaccion-webpay-plus-normal) y la de Webpay OneClick [acá](/referencia/webpay#autorizar-un-pago-con-webpay-oneclick)).


**En el caso de Webpay Plus Normal**
Nombre parámetro   <br> <i> tipo </i> | Fila en tabla
------   | -----------
buyOrder  <br> <i> xs:string </i> | Orden de compra
cardDetails.cardNumber  <br> <i> xs:string </i> | Final número de tarjeta (solo para comercios autorizados por Transbank se envía el número completo).
transactionDate  <br> <i> xs:string </i> | Fecha creación
VCI  <br> <i> xs:string </i> | VCI. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
detailsOutput[0].authorizationCode  <br> <i> xs:string </i> | Código de autorización
detailsOutput[0].paymentTypeCode   <br> <i> xs:string </i> | Tipo de producto
detailsOutput[0].responseCode  <br> <i> xs:string </i> | Código de respuesta.
detailsOutput[0].amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto
detailsOutput[0].sharesNumber  <br> <i> xs:int </i> | Número de cuotas
detailsOutput[0].commerceCode  <br> <i> xs:string </i> | Comercio, desde el quinto dígito en adelante del `commerceCode` corresponde al número antes del guión que podemos apreciar en la imagen anterior.
detailsOutput[0].buyOrder  <br> <i> xs:string </i> | Orden de compra

**En el caso de Webpay Oneclick**
Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorizationCode  <br> <i> xs:string </i> | Código de autorización
creditCardType  <br> <i> creditCardType </i> | Medio de Pago
last4CardDigits  <br> <i> xs:string </i> | Final número tarjeta
responseCode  <br> <i> xs:int </i> | Código de respuesta

## Más Funcionalidades

Consulta la referencia del API para más funcionalidades ofrecidas por Webpay
Plus y Webpay OneClick:

- [Transacciones Webpay Plus Mall](/referencia/webpay#webpay-plus-mall) para
  realizar cargos atribuibles a múltiples comercios dentro de una agrupación
  denominada _mall_. Con esto puedes realizar una sola integración y cobrar a
  nombre de tus clientes o partners.

- [Realizar Captura Diferida](/referencia/webpay#captura-diferida-webpay-plus) de manera que la autorización de
Webpay Plus Normal solo reserve el cupo y la captura de la transacción se pueda
realizar posteriormente.

- [Anular Transacciones Webpay Plus](/referencia/webpay#anulacion-webpay-plus) para devolver dinero parcial o
totalmente.

- [Reversar Transacciones Webpay OneClick](/referencia/webpay#reversar-un-pago-webpay-oneclick) para dejar sin efecto una
transacción realizada durante el día contable actual.

- [Anular Transacciones Webpay
  OneClick](/referencia/webpay#anular-un-pago-webpay-oneclick) para dejar sin
  efecto una transacción realizada en otra fecha distinta al día contable
  actual.

- [Eliminar Inscripciones Webpay OneClick](/referencia/webpay#eliminar-una-inscripcion-webpay-oneclick) para eliminar el `tbkUser`
cuando tus usuarios no quieren continuar con el servicio.

- [Transacciones Webpay OneClick Mall](/referencia/webpay#webpay-oneclick-mall).

## Ejemplos de integración

Ponemos a tu disposición una serie de repositorios en nuestro Github para ayudarte a entender la integración mejor.

- [Ejemplo de Webpay en PHP](https://github.com/TransbankDevelopers/transbank-sdk-php-webpay-example)
- [Ejemplo de Webpay en .Net](https://github.com/TransbankDevelopers/transbank-sdk-dotnet-webpay-example)
- [Ejemplo de Webpay en Java](https://github.com/TransbankDevelopers/transbank-sdk-dotnet-webpay-example)

<div class="container slate">
  <div class='slate-after-footer'>
    <div class='row d-flex align-items-stretch'>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22'>¿Tienes alguna duda de integración?</h3>
        <a href='https://join-transbankdevelopers-slack.herokuapp.com/' target='_blank'>
          <div class='td_block_gray'>
            <img src="https://p9.zdassets.com/hc/theme_assets/138842/200037786/logo.png" alt="" style="width: 90px; min-width: 100px;">
            <div class='td_pa-txt'>
              Únete a la comunidad de integradores en Slack y comparte buenos tips ayudando a los nuevos o buscando soluciones alternativas. Nuestro equipo está ahí para ayudarte.
            </div>
          </div>
        </a>
      </div>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22'>Si aún tienes dudas envíanos un mensaje</h3>
        <a class="pointer magenta" data-toggle='modal' data-target='#modalContactForm'>
          <div class='td_block_gray'>
            <div class="fo-size-20"><i class="fas fa-envelope"></i> Envianos un mensaje..</div>
            <div class='td_pa-txt'>
              Si necesitas resolver algún tipo de incidencia en el portal o si existe algún problema con tu integración y  que no has podido resolver, contáctanos a través de nuestro formulario.
            </div>
          </div>
        </a>
      </div>
    </div>
  </div>
</div>
