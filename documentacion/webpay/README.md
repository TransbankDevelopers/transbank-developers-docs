# Webpay

## Webpay Plus %<span class='tbk-tagTitleDesc'>SOAP</span>%

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

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay





```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay





```

```javascript
const Transbank = require('transbank-sdk');

const transaction = new Transbank.Webpay(
  Transbank.Configuration.forTestingWebpayPlusNormal()
).getNormalTransaction();

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

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay















```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay















```

```javascript
const Transbank = require('transbank-sdk');

const amount = 1000;
// Identificador que será retornado en el callback de resultado:
const sessionId = 'mi-id-de-sesion';
// Identificador único de orden de compra:
const buyOrder = Math.round(Math.random()*999999999);
const returnUrl = 'https://callback/resultado/de/transaccion';
var finalUrl = 'https://callback/final/post/comprobante/webpay';

transaction.initTransaction(amount, buyOrder, sessionId, returnUrl, finalUrl)
  .then((response) => {
    // response tendrá el token de respuesta de esta transacción
    const token = response.token;
  })
  .catch((tbkError) => {
    // Cualquier error será recibido a través del método catch de la promesa
  });
```

La URL y el token retornados te indican donde debes redirigir al usuario para
que comience el flujo de pago. Esta redirección debe ser vía `POST` por lo que
deberás crear un formulario web con un campo `token_ws` hidden y enviarlo
programáticamente para entregar el control a Webpay.

<aside class="warning">
Debido a la posibilidad que el formulario de WebPay no se despliegue, no se
recomienda el uso de Iframe para WebPay Plus.
</aside>

<aside class="notice">
Tip: En el ambiente de integración puedes usar la tarjeta VISA 4051885600446623
para hacer pruebas simples de éxito. El CVV es 123 y la fecha de vencimiento
cualquiera superior a la fecha actual. Luego para la autenticación bancaria usa
el RUT 11.111.111-1 y la clave 123. Para pruebas exhaustivas [consulta todas las
tarjetas de prueba en la sección de
Ambientes](/documentacion/como_empezar#ambientes).
</aside>

<div class='url-modal-embed' data-lenguaje-visible='php' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/YBy7DlQ-Bws" >
  <div class="container-embed">
    <div class="data-info-url">
      <b>Video tutorial de integración SDK PHP 1</b><br>
      Crear una transacción
    </div>
    <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
  </div>
</div>

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

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay








```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay








```

```javascript
const Transbank = require('transbank-sdk');

transaction.getTransactionResult(token)
  .then((response) => {
    const output = response.detailOutput[0];
    if (output.responseCode === 0) {
      // La transacción se ha realizado correctamente
    }
  })
  .catch((tbkError) => {
    // Cualquier error durante la transacción será recibido acá
  });
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

<aside class="warning">
En la confirmación es posible obtener dos excepciones de Timeout diferentes. Se obtendrá la excepción `Timeout error(272)` en caso de un fallo de _getTransactionResult_, si el fallo corresponda al _acknowledge_ la excepción que se obtendrá será `Timeout error (Transaction is REVERSED)(272)`
</aside>


En [la referencia detallada de Webpay Plus puedes ver cada paso del flujo, incluyendo
los casos de borde que también debes manejar](https://www.transbankdevelopers.cl/referencia/webpay#webpay-plus-normal).

<div class='url-modal-embed' data-lenguaje-visible='php' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/J0vTgY6jnnk" >
  <div class="container-embed">
    <div class="data-info-url">
      <b>Video tutorial de integración SDK PHP 2</b><br>
      Confirmar una transacción
    </div>
    <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
  </div>
</div>


## Webpay Plus Mall %<span class='tbk-tagTitleDesc'>SOAP</span>%

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-plus-mall' tbk-link-name='Referencia Api'></div>
</div>

### Crear una transacción

Para una transacción asociada a una tienda mall, lo primero que necesitas es preparar una instancia de `WebpayMallNormal` con la `Configuration` que incluye los códigos de comercio de cada tienda incluida y los certificados a usar.

Una forma fácil de comenzar es usar la configuración para pruebas que viene incluida en el SDK:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.webpay.configuration.Configuration;
import cl.transbank.webpay.Webpay;
import cl.transbank.webpay.WebpayMallNormal;
// ...

WebpayMallNormal transaction =
    new Webpay(Configuration.forTestingWebpayPlusMall())
                .getMallNormalTransaction();
```

```php
use Transbank\Webpay\Configuration;

use Transbank\Webpay\Webpay;
// ...

$transaction = (new Webpay(Configuration::forTestingWebpayPlusMall()))
                ->getMallNormalTransaction();
```

```csharp
using Transbank.Webpay;
//...

var transaction = new Webpay(Configuration.ForTestingWebpayPlusMall())
            .MallNormalTransaction;
```

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay





```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay





```

```javascript
const Transbank = require('transbank-sdk');

const transaction = new Transbank.Webpay(
  Transbank.Configuration.forTestingWebpayPlusMall()
).getMallNormalTransaction();
```

<aside class="notice">
Como necesitarás ese objeto `transaction` en múltiples ocasiones, es buena idea encapsular la lógica que lo genera en algún método que puedas reutilizar.
</aside>

Una vez que ya cuentas con esa preparación, puedes iniciar transacciones:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpay.wswebpay.service.WsInitTransactionOutput;
// ...
// Identificador único de orden de compra generada por el comercio mall
String buyOrder = String.valueOf(Math.abs(new Random().nextLong()));
// Identificador que será retornado en el callback de resultado:
String sessionId = "mi-id-de-sesion";
String returnUrl = "https://callback/resultado/de/transaccion";
String finalUrl = "https://callback/final/post/comprobante/webpay";

// Lista con detalles de cada una de las transacciones:
WsTransactionDetail storeTransaction = new WsTransactionDetail();
List<WsTransactionDetail> transactions = new ArrayList<>();
// Detalles de transacción 1
storeTransaction.setAmount("1000");
storeTransaction.setCommerceCode("597044444402");
// Identificador único de orden de compra generada por tienda 1
storeTransaction.setBuyOrder(String.valueOf(Math.abs(new Random().nextLong())));
transactions.add(storeTransaction);
// Detalles de transacción 2
storeTransaction.setAmount("2000");
storeTransaction.setCommerceCode("597044444403");
// Identificador único de orden de compra generada por tienda 2
storeTransaction.setBuyOrder(String.valueOf(Math.abs(new Random().nextLong())));
transactions.add(storeTransaction);
//...

WsInitTransactionOutput initResult = transaction.initTransaction(buyOrder, sessionId, returnUrl, finalUrl, transactions);

String tokenWs = initResult.getToken();
String formAction = initResult.getUrl();
```

```php
// Identificador único de orden de compra generada por el comercio mall:
$buyOrder = strval(rand(100000, 999999999));
// Identificador que será retornado en el callback de resultado:
$sessionId = "mi-id-de-sesion";
$returnUrl = "https://callback/resultado/de/transaccion";
$finalUrl = "https://callback/final/post/comprobante/webpay";

// Lista con detalles de cada una de las transacciones:
$transactions = array();
// Detalles de transacción 1
$transactions[] = array(
    "storeCode" => "597044444402",
    "amount" => 1000,
    // Identificador único de orden de compra generada por tienda 1
    "buyOrder" => strval(rand(100000, 999999999))
)
// Detalles de transacción 2
$transactions[] = array(
    "storeCode" => "597044444403",
    "amount" => 2000,
    // Identificador único de orden de compra generada por tienda 2
    "buyOrder" => strval(rand(100000, 999999999))
)
//...

$initResult = $transaction->initTransaction($buyOrder, $sessionId, $returnUrl, $finalUrl, $transactions);

$tokenWs = $initResult->token;
$formAction = $initResult->url;
```

```csharp
using Transbank.Webpay;
//...

// Identificador único de orden de compra:
var buyOrder = new Random().Next(100000, 999999999).ToString();
// Identificador que será retornado en el callback de resultado:
var sessionId = "mi-id-de-sesion";
var returnUrl = "https://callback/resultado/de/transaccion";
var finalUrl = "https://callback/final/post/comprobante/webpay";

// Lista con detalles de cada una de las transacciones:
var transactions = new Dictionary<string, string[]>();

// Detalles de transacción 1
transactions.Add("597044444402", new string[] {"597044444402", "1000", new Random().Next(100000, 999999999).ToString()});
// Detalles de transacción 2
transactions.Add("597044444403", new string[] {"597044444403", "2000", new Random().Next(100000, 999999999).ToString()});
//...

var initResult = transaction.initTransaction(buyOrder, sessionId, returnUrl, finalUrl, transactions);

var tokenWs = initResult.token;
var formAction = initResult.url;
```

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay















```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay















```

```javascript
const Transbank = require('transbank-sdk');
// ...

// Identificador único de orden de compra:
const buyOrder = Math.round(Math.random()*999999999);
// Identificador que será retornado en el callback de resultado:
const sessionId = 'mi-id-de-sesion';
const returnUrl = 'https://callback/resultado/de/transaccion';
var finalUrl = 'https://callback/final/post/comprobante/webpay';

// Lista con detalles de cada una de las transacciones:
const transactions = [];

// Detalles de transacción 1
transactions.push({
  storeCode: "597044444402",
  amount: 1000,
  buyOrder: Math.round(Math.random()*999999999),
});
// Detalles de transacción 2
transactions.push({
  storeCode: "597044444403",
  amount: 2000,
  buyOrder: Math.round(Math.random()*999999999),
});
//...

transaction.initTransaction(buyOrder, sessionId, returnUrl, finalUrl, transactions)
  .then((response) => {
    // response tendrá el token de respuesta de esta transacción
    const token = response.token;
  });
  .catch((tbkError) => {
    // Cualquier error será recibido a través del método catch de la promesa
  });
```
<aside class="notice">
Observar que existe un `buyOrder` generado para el comercio mall y un `buyOrder` para cada una de las tiendas.
</aside>

La URL y el token retornados te indican donde debes redirigir al usuario para que comience el flujo de pago. Esta redirección debe ser vía `POST` por lo que deberás crear un formulario web con un campo `token_ws` hidden y enviarlo programáticamente para entregar el control a Webpay.

<aside class="warning">
Debido a la posibilidad que el formulario de WebPay no se despliegue, no se recomienda el uso de Iframe para WebPay Plus.
</aside>

<aside class="notice">
Tip: En el ambiente de integración puedes usar la tarjeta VISA 4051885600446623 para hacer pruebas simples de éxito. El CVV es 123 y la fecha de vencimiento es cualquiera superior a la fecha actual. Luego para la autenticación bancaria usa el RUT 11.111.111-1 y la clave 123. Para pruebas exhaustivas [consulta todas las tarjetas de prueba en la sección de Ambientes](/documentacion/como_empezar#ambientes).
</aside>

### Confirmar una transacción

Una vez que el tarjetahabiente ha pagado (o declinado, o ha ocurrido un error), Webpay retornará el control vía `POST` a la URL que indicaste en el `returnUrl`. Recibirás también el parámetro `token_ws` que te permitirá conocer el resultado de la transacción:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpay.wswebpay.service.TransactionResultOutput;
import com.transbank.webpay.wswebpay.service.WsTransactionDetailOutput;
// ...
TransactionResultOutput result =
    transaction.getTransactionResult(request.getParameter("token_ws"));
for (WsTransactionDetailOutput output: result.getDetailOutput()) {
  // Se debe chequear cada transacción de cada tienda del
  // mall por separado:
  if (output.getResponseCode() == 0) {
    // Transaccion exitosa, puedes procesar el resultado
    // con el contenido de las variables result y output.
  }
}
```

```php
$result = $transaction->getTransactionResult($request->input("token_ws"));
foreach ($result->detailOutput as $output) {
  // Se debe chequear cada transacción de cada tienda del
  // mall por separado:
  if ($output->responseCode == 0) {
    // Transaccion exitosa, puedes procesar el resultado
    // con el contenido de las variables result y output.
  }
}
```

```csharp
using Transbank.Webpay;
//...

var result = transaction.getTransactionResult(tokenWs);
foreach (var output in result.detailOutput){
  // Se debe chequear cada transacción de cada tienda del
  // mall por separado:
  if (output.responseCode == 0) {
    // Transaccion exitosa, puedes procesar el resultado
    // con el contenido de las variables result y output.
  }
}
```

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay








```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay








```

```javascript
const Transbank = require('transbank-sdk');

transaction.getTransactionResult(token)
  .then((response) => {
    response.detailOutput.forEach((output) => {
      // Se debe chequear cada transacción de cada tienda del
      // mall por separado:
      if (output.responseCode == 0) {
        // Transaccion exitosa, puedes procesar el resultado
        // con el contenido de las variables result y output.
      }
    });
  })
  .catch((tbkError) => {
    // Cualquier error durante la transacción será recibido acá
  });
```

> **Importante**: El SDK se encarga de que al mismo tiempo que se obtiene el resultado de la transacción se haga el _acknowledge_ a Transbank de manera que no haya posibilidad de que la transacción se revierta. Si luego necesitas que la transacción no se lleve a cabo (por ejemplo porque ya no tienes stock o porque se generó un error en tu lógica de negocio que entrega el producto o servicio), deberás [anular la transacción](/referencia/webpay#anulacion-webpay-plus).

En el caso exitoso deberás llevar el control vía `POST` nuevamente a Webpay para que el tarjetahabiente vea el comprobante que le deja claro que se ha realizado el cargo en su tarjeta. Nuevamente deberás generar un formulario con el `token_ws` como un campo hidden. La URL para redirigir la debes obtener desde `result.getUrlRedirection()`.

Finalmente después del comprobante Webpay redirigirá otra vez (vía `POST`) a tu sitio, esta vez a la URL que indicaste en el `finalUrl` cuando iniciaste la transacción. Tal como antes, recibirás el `token_ws` que te permitirá identificar la transacción y mostrar un comprobante o página de éxito a tu usuario. Con eso habrás completado el flujo "feliz" en que todo funciona.

En [la referencia detallada de Webpay Plus Mall puedes ver cada paso del flujo, incluyendo los casos de borde que también debes manejar](https://www.transbankdevelopers.cl/referencia/webpay#webpay-plus-mall).


## Webpay OneClick %<span class='tbk-tagTitleDesc'>SOAP</span>%

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-oneclick-normal' tbk-link-name='Referencia Api'></div>
</div>

### Crear una inscripción

Para usar Webpay OneClick en transacciones asociadas a un único comercio, lo
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

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay






```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay






```

```javascript
const Transbank = require('transbank-sdk');

const transaction = new Transbank.Webpay(
  Transbank.Configuration.forTestingWebpayOneClickNormal()
).getOneClickTransaction();
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

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay











```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay











```
```javascript
const Transbank = require('transbank-sdk');
//...

// Identificador del usuario en el comercio
const username = "pepito"
// Correo electrónico del usuario
const email = "pepito@gmail.com";
const urlReturn = "https://callback/resultado/de/transaccion";
transaction.initInscription(username, email, urlReturn)
  .then((response) => {
    // response tendrá el token de respuesta de esta transacción
    const token = response.token;
    // También la URL de webpay
    const formAction = response.urlWebpay;
  });
  .catch((tbkError) => {
    // Cualquier error durante la transacción será recibido acá
  });
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

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay








```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay








```

```javascript
const Transbank = require('transbank-sdk');
//...
transaction.finishInscription(token)
  .then((response) => {
    const output = response.detailOutput[0];
    if (output.responseCode === 0) {
      // La transacción se ha realizado correctamente
    }
  })
  .catch((tbkError) => {
    // Cualquier error durante la transacción será recibido acá
  });
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

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay










```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay










```

```javascript
const Transbank = require('transbank-sdk');
//...

const buyOrder = Math.round(Math.random()*999999999);
const tbkUser = tbkUserRetornadoPorFinishInscription;
const username = "pepito"; // El mismo usado en initInscription.
const amount = 50000;
transaction.authorize(buyOrder, tbkUser, username, amount)
  .then((response) => {
    const output = response.detailOutput[0];
    if (output.responseCode === 0) {
      // La transacción se ha realizado correctamente
    }
  })
  .catch((tbkError) => {
    // Cualquier error durante la transacción será recibido acá
  })
```

## Webpay OneClick Mall %<span class='tbk-tagTitleDesc'>REST</span>%

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-oneclick-mall' tbk-link-name='Referencia Api'></div>
</div>

Para usar Webpay OneClick Mall en transacciones asociadas a varios comercios, lo primero que se debe hacer es definir las dependencias necesarias para poder realizar cualquier tipo de transacción.

<div class="language-simple" data-multiple-language></div>

```java

import cl.transbank.webpay.oneclick.OneClickMall;

// ...

```

```php

use Transbank\Webpay\Oneclick\MallInscription;
use Transbank\Webpay\Oneclick\MallTransaction;

// ...

```

```csharp

using Transbank.Webpay.Oneclick;

// ...

```

```ruby

// Incluir en el Gemfile
gem 'transbank-sdk', git: "https://github.com/TransbankDevelopers/transbank-sdk-ruby.git"


```

```python

from transbank.oneclick.mall_inscription import MallInscription
from transbank.oneclick.mall_transaction import MallTransaction

from transbank.oneclick.request import MallTransactionAuthorizeDetails

// ...

```

Una vez que ya cuentas con esa preparación, puedes iniciar transacciones:


### Crear una inscripción

Te permite realizar la inscripción del tarjetahabiente:

<div class="language-simple" data-multiple-language></div>

```java
//...


// Identificador del usuario en el comercio
String username = "nombre_de_usuario";
// Correo electrónico del usuario
String email = "nombre_de_usuario@gmail.com";
String response_url = "https://callback/resultado/de/transaccion";

OneclickMallInscriptionStartResponse response = OneclickMall.Inscription.start(username, email, response_url);

String url_webpay = response.getUrlWebpay();
String tbk_token = response.getToken();
```

```php
//...


// Identificador del usuario en el comercio
$username = "nombre_de_usuario";
// Correo electrónico del usuario
$email = "nombre_de_usuario@gmail.com";
$response_url = "https://callback/resultado/de/transaccion";

$resp = MallInscription::start($username, $email, $response_url);

$url_webpay = $resp->getUrlWebpay();
$tbk_token = $resp->getToken();

```

```csharp
//...


// Identificador del usuario en el comercio
var username = "nombre_de_usuario";
// Correo electrónico del usuario
var email = "nombre_de_usuario@gmail.com";
var response_url = "https://callback/resultado/de/transaccion";

var response = Inscription.start(username, email, response_url);

var url_webpay = response.Url;
var tbk_token = response.Token;

```

```ruby


@username = "nombre_de_usuario"
@email = "nombre_de_usuario@gmail.com"
@response_url = "https://callback/resultado/de/transaccion"


@resp = Transbank::Webpay::Oneclick::MallInscription::start(user_name: @username,email: @email,response_url: @response_url)

@url_webpay = @resp.url_webpay
@tbk_token = @resp.token

```

```python

username = "nombre_de_usuario"
email = "nombre_de_usuario@gmail.com"
response_url = "https://callback/resultado/de/transaccion"

resp = MallInscription.start(user_name=username,email=email,response_url=response_url)

url_webpay = resp.url_webpay
tbk_token = resp.token


```

Tal como en el caso de Webpay Oneclick Normal, debes redireccionar vía `POST` el navegador del usuario a la url retornada en `url_webpay`. **Recordando que el nombre del parámetro que contiene el token se debe llamar `TBK_TOKEN`**.

### Confirmar una inscripción

Una vez que se autorice la inscripción del usuario, se retornará el control al comercio vía `POST` en la url indicada en `response_url`, con el parámetro `TBK_TOKEN` identificando la transacción. Con esa información se puede finalizar la inscripción:

<div class="language-simple" data-multiple-language></div>

```java
//...

String tbk_token = "tbkTokenRetornadoPorInscriptionStart";

OneclickMallInscriptionFinishResponse response = OneclickMall.Inscription.finish(tbk_token);

String tbkUser = response.getTbkUser();

```

```php
//...

$tbk_token = "tbkTokenRetornadoPorInscriptionStart";

$resp = MallInscription::finish($tbk_token);

$tbkUser = $resp->getTbkUser();


```

```csharp
//...

var token = "tbkTokenRetornadoPorInscriptionStart";

var result = Inscription.Finish(tbk_token);

var tbkUser = result.TbkUser;

```

```ruby
//...

@tbk_token = "tbkTokenRetornadoPorInscriptionStart";

@resp = Transbank::Webpay::Oneclick::MallInscription::finish(token: @tbk_token)

@tbkUser = @resp.tbk_user

```

```python
//...

tbk_token = "tbkTokenRetornadoPorInscriptionStart"

resp = MallInscription.finish(token=tbk_token)

tbkUser = resp.tbk_user

```

Con eso habrás completado el flujo "feliz" en que todo funciona OK. En [la referencia detallada de Webpay OneClick Mall puedes ver cada paso del flujo, incluyendo los casos de borde que también debes manejar](https://www.transbankdevelopers.cl/referencia/webpay#webpay-oneclick-mall).

### Eliminar una inscripción

Si en algún momento se quiere eliminar la inscripción de un usuario, se debe invocar a `Inscription.delete()`, con el identificador de inscripción `tbkUser` obtenido en `Inscription.finish()`.

<div class="language-simple" data-multiple-language></div>

```java
//...

// Identificador del usuario en el comercio
String username = "nombre_de_usuario";
String tbkUser = "tbkUserRetornadoPorInscriptionFinish";

OneclickMall.Inscription.delete(username, tbkUser);


```

```php
//...

// Identificador del usuario en el comercio
$username = "nombre_de_usuario";
$tbkUser = $tbkUserRetornadoPorInscriptionFinish;

//Parámetro opcional
$options = new Options($apiKey, $parentCommerceCode);

$resp = MallInscription::delete($tbkUser, $username, $options);


```

```csharp
//...

// Identificador del usuario en el comercio
var username = "nombre_de_usuario";
var tbkUser = "tbkUserRetornadoPorInscriptionFinish";

var result = Inscription.Delete(username, tbkUser);

```

```ruby
//...

@username = "nombre_de_usuario"
@tbkUser = "tbkUserRetornadoPorInscriptionFinish"

@resp = Transbank::Webpay::Oneclick::MallInscription::delete(user_name: @username,tbk_user: @tbkUser)

```

```python
//...

tbkUser = "tbkUserRetornadoPorInscriptionFinish"
username = "nombre_de_usuario"

resp = MallInscription.delete(tbk_user=tbkUser, user_name=username)

```

Si se quiere comprobar si se eliminó correctamente, la función retorna un boolean, el cual será `true` en caso de éxito y `false` en otro caso.

### Realizar transacciones

Con el `tbkUser` retornado de `Inscription.finish()` puedes autorizar transacciones:

<div class="language-simple" data-multiple-language></div>

```java

// Identificador único de orden de compra generado por el comercio:
String username = "nombre_de_usuario";
String tbkUser = "tbkUserRetornadoPorInscriptionFinish";
String buyOrder = String.valueOf(new Random().nextInt(Integer.MAX_VALUE));


double amountOne = 10000;
String MallOneCommerceCode = "597055555542";
String buyOrderMallOne = String.valueOf(new Random().nextInt(Integer.MAX_VALUE));
int installmentNumberOne = 3;

double amountTwo = 50000;
String MallTwoCommerceCode = "597055555543";
String buyOrderMallTwo = String.valueOf(new Random().nextInt(Integer.MAX_VALUE));
int installmentNumberTwo = 3;

MallTransactionCreateDetails details = MallTransactionCreateDetails.build()
                .add(amountOne, MallOneCommerceCode, buyOrderMallOne, installmentNumberOne)
                .add(amuntTwo, MallTwoCommerceCode, buyOrderMallTwo, installmentNumberTwo);


OneclickMallTransactionAuthorizeResponse response = OneclickMall.Transaction.authorize(username, tbkUser, buyOrder, details);


```

```php

// Identificador del usuario en el comercio
$username = "nombre_de_usuario";
$tbkUser = $tbkUserRetornadoPorInscriptionFinish;
$parentBuyOrder = rand(100000, 999999999);

$childCommerceCode1 = "597055555543";
$childBuyOrder1 = strval(rand(100000, 999999999));
$amount1 = 50000;
$installmentsNumber1 = 1;

$childCommerceCode2 = "597055555543";
$childBuyOrder2 = strval(rand(100000, 999999999));
$amount2 = 50000;
$installmentsNumber2 = 1;

$details = [
    [
        "commerce_code" => $childCommerceCode1,
        "buy_order" => $childBuyOrder1,
        "amount" => $amount1,
        "installments_number" => $installmentsNumber1
    ],
    [
        "commerce_code" => $childCommerceCode2,
        "buy_order" => $childBuyOrder2,
        "amount" => $amount2,
        "installments_number" => $installmentsNumber2
    ]
];

$resp = MallTransaction::authorize($username, $tbkUser, $parentBuyOrder, $details);


```

```csharp

var username = "nombre_de_usuario";
var tbkUser = "tbkUserRetornadoPorInscriptionFinish";
var buyOrder = RandomString(10);

var childCommerceCode = "597055555542";
var childBuyOrder = RandomString(10);
var amount = Decimal.Parse(Request.Form["amount"]);
var installmentsNumber = 1;

List<PaymentRequest> details = new List<PaymentRequest>();
details.Add(new PaymentRequest(childCommerceCode, childBuyOrder, amount, installmentsNumber));

var result = MallTransaction.Authorize(username, tbkUser, buyOrder, details);


```

```ruby

@username = "nombre_de_usuario"
@tbkUser = "tbkUserRetornadoPorInscriptionFinish"
@buy_order = "12345" + Time.now.to_i.to_s

@details =
  [{
    commerce_code: "597055555542",
    buy_order: "abcdef" + Time.now.to_i.to_s,
    amount: 10000,
    installments_number: 3
  },
  {
    commerce_code: "597055555543",
    buy_order: "abcdef" + Time.now.to_i.to_s,
    amount: 50000,
    installments_number: 3
  }]
end


@resp = Transbank::Webpay::Oneclick::MallTransaction::authorize(username: @username, tbk_user: @tbkUser, parent_buy_order: @buy_order, details: @details)


```

```python

username = "nombre_de_usuario"
tbkUser = "tbkUserRetornadoPorInscriptionFinish"
buy_order = str(random.randrange(1000000, 99999999))

commerce_code1 = "597055555542"
buy_order_child1 = str(random.randrange(1000000, 99999999))
installments_number1 = 3
amount1 = 10000

commerce_code2 = "597055555543"
buy_order_child2 = str(random.randrange(1000000, 99999999))
installments_number2 = 4
amount2 = 50000

details = MallTransactionAuthorizeDetails(commerce_code1, buy_order_child1, installments_number1, amount1) \
    .add(commerce_code2, buy_order_child2, installments_number2, amount2)

resp = MallTransaction.authorize(user_name=username, tbk_user=tbkUser, buy_order=buy_order, details=details)

```

### Anular una transacción

En el caso de que se quiera anular alguna transacción, se invoca a `Transaction.refund()`.

<div class="language-simple" data-multiple-language></div>

```java
//...


String buyOrder = "buyOrderIndicadoEnTransactionAuthorize";
String childCommerceCode = "childCommerceCodeIndicadoEnTransactionAuthorize";
String childBuyOrder = "childBuyOrderIndicadoEnTransactionAuthorize";
double amount = (byte) 1;

OneclickMallTransactionRefundResponse response = OneclickMall.Transaction.refund(buyOrder, childCommerceCode, childBuyOrder, amount);

```

```php
//...

$buyOrder = $buyOrderIndicadoEnTransactionAuthorize;
$childCommerceCode = $childCommerceCodeIndicadoEnTransactionAuthorize;
$childBuyOrder = $childBuyOrderIndicadoEnTransactionAuthorize;
$amount = $amountIndicadoEnTransactionAuthorize;

//Parámetro opcional
$options = new Options($apiKey, $parentCommerceCode);

$resp = MallTransaction::refund($buyOrder, $childCommerceCode, $childBuyOrder, $amount, $options);


```

```csharp
//...

var buyOrder = Request.Form["buy_order"];
var childCommerceCode = Request.Form["child_commerce_code"];
var childBuyOrder = Request.Form["child_buy_order"];
var amount = decimal.Parse(Request.Form["amount"]);

var result = MallTransaction.Refund(buyOrder, childCommerceCode,childBuyOrder,amount);


```

```ruby
//...

@buy_order = "12345" + Time.now.to_i.to_s
@child_commerce_code = "597055555542"
@child_buy_order = "abcdef" + Time.now.to_i.to_s
@amount = 1000

@resp = Transbank::Webpay::Oneclick::MallTransaction::refund(buy_order: @buy_order, child_commerce_code: @child_commerce_code, child_buy_order: @child_buy_order, amount: @amount)

```

```python
//...


buy_order = str(random.randrange(1000000, 99999999))
child_commerce_code = '597055555542'
child_buy_order = str(random.randrange(1000000, 99999999))
amount = 10000

resp = MallTransaction.refund(buy_order, child_commerce_code, child_buy_order, amount)

```

## Webpay Transacción Completa {data-submenuhidden=true} %<span class='tbk-tagTitleDesc'>REST</span>%


### Crear una transacción

Antes de realizar cualquier transacción es necesario instanciar el producto.

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;

Transaction.setCommerceCode = "Codigo de Comercio";
Transaction.setApiKey = "Apikey entregado por Transabank";
Transaction.setIntegrationType = "TEST/LIVE"; //ambiente de integracion;
```

```php
use Transbank\TransaccionCompleta;

TransaccionCompleta::setCommerceCode = "Codigo de Comercio";
TransaccionCompleta::setApiKey = "Apikey entregado por Transabank";
TransaccionCompleta::setIntegrationType = "TEST/LIVE"; //ambiente de integracion;


```

```csharp
...
using Transbank.Webpay.Common;
using Transbank.Webpay.TransaccionCompleta;
...

TransaccionCompleta.CommerceCode = "Codigo de Comercio";
TransaccionCompleta.ApiKey = "Apikey entregado por Transabank";
TransaccionCompleta.IntegrationType = "TEST/LIVE"; //ambiente de integracion;

```

```ruby

```

```python


```
Es recomendado encapsular la asignacion para utilizarla sin problemas en los demas metodos.

Ahora se pueden realizar transacciones sin problemas

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;

String buyOrder = "Orden de compra de la transaccion";
String sessionId = "Identificador del servicio unico de transacción";
double amount = 10000; // mongo en pesos
String cardNumber= "Numero de Tarjeta";
String cardExpirationDate= "Fecha de expiracion en formato AA/MM";
short cvv = 123; // CVV de la tarjeta.

FullTransactionCreateResponse response = FullTransaction.Transaction.create(buyOrder, sessionId, amount, cardNumber, cardExpirationDate, cvv);

```

```php
use Transbank\TransaccionCompleta\Transaction;

$buyOrder = "Orden de compra de la transaccion";
$sessionId = "Identificador del servicio unico de transacción";
$amount = 10000; // mongo en pesos
$cardNumber= "Numero de Tarjeta";
$cardExpirationDate= "Fecha de expiracion en formato AA/MM";
$cvv = 123; // CVV de la tarjeta.

$response = Transaction::create($buyOrder, $sessionId, $amount, $cardNumber, $cardExpirationDate, $cvv);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

var buy_order = "Orden de compra de la transaccion";
var session_id = "Identificador del servicio unico de transacción";
var amount = 10000; // monto en pesos
var card_number = "Numero de Tarjeta";
var card_expiration_date = "Fecha de expiracion en formato AA/MM";
var cvv = 123; // CVV de la tarjeta.

var response = FullTransaction.Create(
                buyOrder: buy_order,
                sessionId: session_id,
                amount: amount,
                cvv: cvv,
                cardNumber: card_number,
                cardExpirationDate: card_expiration_date);
```

```ruby
buy_order = "Orden de compra de la transaccion"
session_id = "Identificador del servicio unico de transacción"
amount = 1000 /* monto en pesos */
card_number = "Numero de Tarjeta"
card_expiration_date = "Fecha de expiracion en formato AA/MM"
cvv = 123 /* CVV de la tarjeta. */


response = Transbank::TransaccionCompleta::Transaction::create(
                                                            buy_order: buy_order,
                                                            session_id: session_id,
                                                            amount: amount,
                                                            card_number: card_number,
                                                            cvv: cvv,
                                                            card_expiration_date: card_expiration_date
                                                            )

```

```python

from transbank.transaccion_completa.transaction import Transaction

    # leyendo variables desde un formulario
    buy_order = 'Orden de compra de la transaccion '
    session_id = 'Identificador del servicio unico de transacción'
    amount = 10000; #monto en pesos
    card_number = 'Numero de Tarjeta'
    cvv = 123 #CVV de la tarjeta.
    card_expiration_date = 'Fecha de expiracion en formato AA/MM'

    resp = Transaction.create(
        buy_order=buy_order, session_id=session_id, amount=amount,
        card_number=card_number, cvv=cvv, card_expiration_date=card_expiration_date
    )

```

### Consulta de Cuotas

Antes de confirmar una transaccion es necesario confirmar la cantidad de cuotas y entregar el valor de estas.

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;


String token = "token obtenido como respuesta de la creacion de transaccion";
int installmentsNumber = 10; // numero de cuotas;

FullTransactionInstallmentResponse response = FullTransaction.Transaction.installment(
  token,
  installmentsNumber
);
```

```php
use Transbank\TransaccionCompleta\Transaction;

$token = "token obtenido como respuesta de la creacion de transaccion";
$installmentsNumber = 10; // numero de cuotas;

$response = Transaction::installments(
    token,
    installmentsNumber
);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

var token = "token obtenido como respuesta de la creacion de transaccion";
var installments_number = 10; // numero de cuotas;

var response = FullTransaction.Installments(
  token: token,
  installmentsNumber: installments_number
  );
```

```ruby
token = "token obtenido como respuesta de la creacion de transaccion"
installments_number = 10 /* numero de cuotas */

response = Transbank::TransaccionCompleta::Transaction::installments( token: token,
                                                                      installments_number: installments_number )
```

```python
from transbank.transaccion_completa.transaction import Transaction

#obtener form desde el request
    req = request.form
    token = request.form.get('token') #token obtenido al iniciar la transaccion
    installments_number = 10 #numero de cuotas
    resp = Transaction.installments(token=token, installments_number=installments_number)

```

### Confirmar Transaccion

Una vez obtenido la respuesta de la consulta de cuotas, con los datos de esta se puede realizar commit de la transaccion:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;


String token = "token obtenido como respuesta de la creacion de transaccion";
int idQueryInstallments = 12345679; // numero identificador de las cuotas.
byte deferredPeriodIndex= 1;
Boolean gracePeriod = false;

FullTransactionCommitResponse response = FullTransaction.Transaction.commit(
  token,
  idQueryInstallments,
  deferredPeriodIndex,
  gracePeriod
);

```

```php
use Transbank\TransaccionCompleta\Transaction;

$token = "token obtenido como respuesta de la creacion de transaccion";
$idQueryInstallments = 12345679; // numero identificador de las cuotas.
$deferredPeriodIndex= 1;
$gracePeriod = false;


$response = Transaction::commit(
    $token,
    $idQueryInstallments,
    $deferredPeriodIndex,
    $gracePeriod,
);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

var token = "token obtenido como respuesta de la creacion de transaccion";
var idQueryInstallments = 12345679; // numero identificador de las cuotas.
var deferredPeriodsIndex = 1;
var gracePeriods = false;

var result = FullTransaction.Commit(
  token:token,
  idQueryInstallments: idQueryInstallments,
  deferredPeriodsIndex: deferredPeriodsIndex,
  gracePeriods: gracePeriods
);
```

```ruby
token = var token = "token obtenido como respuesta de la creacion de transaccion"
id_query_installments = 12345679 /* numero identificador de las cuotas. */
deferred_period_index = 1
grace_period = false

response = Transbank::TransaccionCompleta::Transaction::commit( token: token,
                                                            id_query_installments: id_query_installments,
                                                            deferred_period_index:deferred_period_index,
                                                            grace_period: grace_period )
```

```python
    from transbank.transaccion_completa.transaction import Transaction
    
    #token obtenido como respuesta de la creacion de transaccion
    token = request.form.get('token')
    id_query_installments = 12345679 # numero identificador de las cuotas.
    deferred_period_index = 1
    grace_period = 'false'

    response = Transaction.commit(token=token,
                              id_query_installments=id_query_installments,
                              deferred_period_index=deferred_period_index,
                              grace_period=grace_period)

```


### Estado Transaccion

Para obtener el resultado de la transaccion exite el siguiente metodo:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;


String token = "token obtenido como respuesta de la creacion de transaccion";

FullTransactionCommitResponse response = FullTransaction.Transaction.status(
  token,
);

```

```php
use Transbank\TransaccionCompleta\Transaction;

$token = "token obtenido como respuesta de la creacion de transaccion";

$response = Transaction::status(
    $token
);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

var token = "token obtenido como respuesta de la creacion de transaccion";

var response = FullTransaction.Status(
  token:token
);
```

```ruby
token = "token obtenido como respuesta de la creacion de transaccion"

response = Transbank::TransaccionCompleta::Transaction::status(token: token)
```

```python
from transbank.transaccion_completa.transaction import Transaction

    response = Transaction.status(token=token) #token obtenido como respuesta de la creacion de transaccion

```

### Reembolso Transaccion

Para procesar un reembolso de la transaccion exite el siguiente metodo:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;
  String tokenWs = //token obtenido al crear transaccion
  double amount = 1000;

  final FullTransactionRefundResponse response = FullTransaction.Transaction.refund(tokenWs,amount);

```

```php
use Transbank\TransaccionCompleta\Transaction;

$token = "token obtenido como respuesta de la creacion de transaccion";
$amount = 1000; // monto a reembolsar

$response = Transaction::refund(
    $token,
    $amount
);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

var token = "token obtenido como respuesta de la creacion de transaccion";
var amount = 1000; // monto a reembolsar

var response = FullTransaction.Refund(
  token: token,
  amount: amount
);
```

```ruby
token = "token obtenido como respuesta de la creacion de transaccion"
amount = 1000 /* monto a reembolsar */

response =  Transbank::TransaccionCompleta::Transaction::refund(token: token, amount: amount)
```

```python
from transbank.transaccion_completa.transaction import Transaction

   #obtener form desde el request
    req = request.form
    token = req.get('token') #token obtenido al crear la transaccion
    amount = '1000' #monto a reembolsar
    response = Transaction.refund(token=token, amount=amount)

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

// a continuación va tu código de comercio, Si el código que posees es de 8 dígitos debes anteponer 5970.
configuration.setCommerceCode("597012345678");
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

// a continuación va tu código de comercio, Si el código que posees es de 8 dígitos debes anteponer 5970.
$configuration->setCommerceCode("12345");
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
    // a continuación va tu código de comercio, Si el código que posees es de 8 dígitos debes anteponer 5970.
    CommerceCode = "12345",
    PrivateCertPfxPath = @"C:\Certs\certificado.pfx", // pega acá la ruta a tu archivo pfx o p12
    Password = "secret123" // pega acá el secreto con el cual se genero el archivo pfx o p12
};

var webpay = new Webpay(configuration);
// Ahora puedes obtener las instancias de las transacciones
// que usarás, por ejemplo:
var oneClickTransaction = webpay.OneClickTransaction;
```

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay














```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay














```

```javascript
const Transbank = require('transbank-sdk');

const transaction = new Transbank.Webpay(
  new Transbank.Configuration()
  .withPrivateCert(/* pon tu certificado privado en forma de string */)
  .withPublicCert(/* pon tu certificado público en forma de string */)
  .withCommerceCode(/* pon tu código de comercio */)
);
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

```ruby
# Para integrar Webpay en Ruby puedes utilizar la Referencia API, alguna librería externa o libwebpay








```

```python
# Para integrar Webpay en Python puedes utilizar la Referencia API, alguna librería externa o libwebpay








```

```javascript
const Transbank = require('transbank-sdk');

const transaction = new Transbank.Webpay(
  new Transbank.Configuration()
  .withPrivateCert(/* pon tu certificado privado en forma de string */)
  .withPublicCert(/* pon tu certificado público en forma de string */)
  .withCommerceCode(/* pon tu código de comercio */)
  .setEnvironment(Transbank.environments.production) // Se fija el ambiente como producción
);
```

<aside class="warning">
El SDK se encarga de configurar el certificado público de
Transbank correspondiente al ambiente seleccionado. Pero es tu responsabilidad
actualizar periódicamente la versión del SDK para recibir actualizaciones a
dicho certificado. De lo contrario, expirará el certificado y dejarás de poder
operar con Transbank.
</aside>

## Conciliación de Transacciones

Una vez hayas realizado transacciones en producción quedará un historial de
transacciones que puedes revisar entrando a
[www.transbank.cl](https://www.transbank.cl/). Si lo deseas  puedes realizar una
conciliación entre tu sistema y el reporte que entrega el portal.

### Webpay Plus

Para realizar la conciliación debes seguir los siguientes pasos:

1. Iniciar sesión con tu usuario y contraseña en [www.transbank.cl](https://www.transbank.cl)

2. Una vez entras al portal, en el menú principal presiona "Webpay", y luego "Reporte Transaccional"
![Paso 2](/images/documentacion/conciliacion1.png)

3. En la parte superior de la ventana puedes encontrar un buscador que te ayudará
a filtrar, según los parámetros que gustes, las transacciones que quieras cuadrar.
Para filtrar por las transacciones de Webpay Plus, en el campo "Producto" debes
seleccionar **Webpay3G**. Debes tener en cuenta que lamentablemente
**el reporte no distingue entre Webpay y Onepay**, debido a esto, bajo el producto "Webpay3G" encontrarás transacciones de ambos productos.
![Paso 3](/images/documentacion/conciliacion2.png)

4. Dentro de la tabla en la imagen anterior puedes presionar el número de orden de
compra para abrir los detalles de la transacción. Es en esta sección donde podrás encontrar y conciliar los parámetros devueltos por el SDK al confirmar una transacción.
![Paso 4](/images/documentacion/conciliacion3.png)

5. Sólo queda realizar la conciliación. A continuación puedes ver una lista de
parámetros que recibirás al momento de confirmar una transacción y a que fila
de la tabla "Detalles de la transacción" corresponden (la lista completa de
parámetros de Webpay Plus la puedes encontrar
[acá](/referencia/webpay#confirmar-una-transaccion-webpay-plus-normal))

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

### OneClick

Para realizar la conciliación debes seguir los siguientes pasos:

1. Iniciar sesión con tu usuario y contraseña en [www.transbank.cl](https://www.transbank.cl/)

2. Una vez entras al portal, en el menú principal presiona "Webpay", y luego "Reporte Transaccional"
![Paso 2](/images/documentacion/conciliacion1.png)

3. En la parte superior de la ventana puedes encontrar un buscador que te ayudará
a filtrar, según los parámetros que gustes, las transacciones que quieras cuadrar.
Para filtrar por las transacciones de OneClick, en el campo "Producto" debes
seleccionar **OneClick**.
![Paso 3](/images/documentacion/conciliacion2.png)

4. Dentro de la tabla en la imagen anterior puedes presionar el número de orden de
compra para abrir los detalles de la transacción. Es en esta sección donde podrás
encontrar y conciliar los parámetros devueltos por el SDK al confirmar una transacción.
![Paso 4](/images/documentacion/conciliacion3.png)

5. Sólo queda realizar la conciliación. A continuación puedes ver una lista de
parámetros que recibirás al momento de confirmar una transacción y a que fila
de la tabla "Detalles de la transacción" corresponden (la lista completa de
parámetros de OneClick la puedes encontrar
[acá](/referencia/webpay#autorizar-un-pago-con-webpay-oneclick))

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

En el caso de integrar webpay en una aplicación móvil Android, usando webview, debes tener presente la siguiente configuración:

1. Al momento de abrir el webview.

```javascript
// habilitar el Cookie Manager. Depende del nivel de la API de Android que se utilice se habilita de diferente forma
if (android.os.Build.VERSION.SDK_INT >= 21)
    CookieManager.getInstance().setAcceptThirdPartyCookies(myWebPayView, true); // myWebPayView es el WebView
else
    CookieManager.getInstance().setAcceptCookie(true);

// Asignar el caché en el webview
webPayView.getSettings().setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);
```

2. Al momento de cerrar el webview

```javascript
// Remover Cookies
if (android.os.Build.VERSION.SDK_INT >= 21)
    CookieManager.getInstance().removeAllCookies(null);
else
    CookieManager.getInstance().removeAllCookie();

// Borrar caché
myWebPayView.clearCache(true);
```

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