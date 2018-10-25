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
$transaction = (new Webpay(Configuration::forTestingWebpayPlusNormal()))
               ->getNormalTransaction();
```

```csharp
var transaction =
    new Webpay(Configuration.ForTestingWebpayPlusNormal).NormalTransaction;
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
var amount = 1000;
// Identificador que será retornado en el callback de resultado:
var sessionId = "mi-id-de-sesion";
// Identificador único de orden de compra:
var buyOrder = new Random().next(100000, 999999999).ToString();
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
> servicio), deberás [anular la transacción](/referencia/webpay#anulaci-n-webpay-plus).

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
$transaction =
    (new Webpay(Configuration::forTestingWebpayOneClickNormal()))
    ->getOneClickTransaction();
```

```csharp
var transaction =
    new Webpay(Configuration.ForTestingWebpayOneClickNormal)
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
validación](/documentacion/como_empezar#el-proceso-de-validaci-n-y-puesta-en-producci-n)
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
Configuration configuration = new Configuration();
configuration.CommerceCode = "12345"; // acá va tu código de comercio
configuration.PrivateKey =  // pega acá la llave privada de tu certificado
    "-----BEGIN RSA PRIVATE KEY-----\n" +
    "MIIEpQIBAAKCAQEA0ClVcH8RC1u+KpCPUnzYSIcmyXI87REsBkQzaA1QJe4w/B7g\n" +
    //....
    "MtdweTnQt73lN2cnYedRUhw9UTfPzYu7jdXCUAyAD4IEjFQrswk2x04=\n" +
    "-----END RSA PRIVATE KEY-----");
configuration.PublicCert = // pega acá tu certificado público
    "-----BEGIN CERTIFICATE-----\n" +
    "MIIDujCCAqICCQCZ42cY33KRTzANBgkqhkiG9w0BAQsFADCBnjELMAkGA1UEBhMC\n" +
    //....
    "-----END CERTIFICATE-----");

var webpay = new Webpay(configuration);
// Ahora puedes obtener las instancias de las transacciones
// que usarás, por ejemplo:
var oneClickTransaction = webpay.OneClickTransaction;
```

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
$configuration = new Configuration();
$configuration->setEnvironment("PRODUCCION");
// agregar también configuración del código de comercio y certificados
```

```csharp
Configuration configuration = new Configuration();
configuration.Environment = "PRODUCCION";
// agregar también configuración del código de comercio y certificados
```

<aside class="warning">
El SDK se encarga de configurar el certificado público de
Transbank correspondiente al ambiente seleccionado. Pero es tu responsabilidad
actualizar periódicamente la versión del SDK para recibir actualizaciones a
dicho certificado. De lo contrario, expirará el certificado y dejarás de poder
operar con Transbank.
</aside>

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

- [Anular Transacciones Webpay Plus](/referencia/webpay#anulaci-n-webpay-plus) para devolver dinero parcial o
totalmente.

- [Reversar Transacciones Webpay OneClick](/referencia/webpay#reversar-un-pago-webpay-oneclick) para dejar sin efecto una
transacción realizada durante el día.

- [Eliminar Inscripciones Webpay OneClick](/referencia/webpay#eliminar-una-inscripci-n-webpay-oneclick) para eliminar el `tbkUser`
cuando tus usuarios no quieren continuar con el servicio.

- [Transacciones Webpay OneClick Mall](/referencia/webpay#webpay-oneclick-mall).

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
