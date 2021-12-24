# Oneclick

___

<aside class="warning">
Estás viendo la <strong>nueva referencia REST</strong>. La referencia anterior (SOAP) se encuentra en proceso de 
obsolescencia programada para Julio 2022. Si necesitas revisarla haz [click aquí](/referencia/webpay-soap)
</aside>

## Ambientes y Credenciales

La API REST de Webpay está protegida para garantizar que solamente comercios autorizados por Transbank hagan uso de las operaciones disponibles. La seguridad esta implementada mediante los siguientes mecanismos:

* Canal seguro a través de TLSv1.2 para la comunicación del cliente con Webpay.
* Autenticación y autorización mediante el intercambio de headers `Tbk-Api-Key-Id` (código de comercio) y `Tbk-Api-Key-Secret` (llave secreta).

### Ambiente de Producción

Las URLs de endpoints de producción están alojados dentro de
<https://webpay3g.transbank.cl/>.

```java
// Host: https://webpay3g.transbank.cl
```

```php
// Host: https://webpay3g.transbank.cl
```

```csharp
// Host: https://webpay3g.transbank.cl
```

```ruby
# Host: https://webpay3g.transbank.cl
```

```python
# Host: https://webpay3g.transbank.cl
```

```http
Host: https://webpay3g.transbank.cl
```

### Ambiente de Integración

Las URLs de endpoints de integración están alojados dentro de
<https://webpay3gint.transbank.cl/>.

```java
// Host: https://webpay3gint.transbank.cl
```

```php
// Host: https://webpay3gint.transbank.cl
```

```csharp
// Host: https://webpay3gint.transbank.cl
```

```ruby
# Host: https://webpay3gint.transbank.cl
```

```python
# Host: https://webpay3g.transbank.cl
```

```http
Host: https://webpay3gint.transbank.cl
```

### Tarjetas y usuarios de prueba

Consulta [la documentación para conocer las tarjetas de prueba que funcionan en
el ambiente de integración](/documentacion/como_empezar#ambientes).

### Credenciales del comercio

```java
// Tbk-Api-Key-Id: Código de comercio
// Tbk-Api-Key-Secret: Llave secreta
// Content-Type: application/json
```

```php

```

```csharp
// Tbk-Api-Key-Id: Código de comercio
// Tbk-Api-Key-Secret: Llave secreta
// Content-Type: application/json
```

```ruby
# Tbk-Api-Key-Id: Código de comercio
# Tbk-Api-Key-Secret: Llave secreta
# Content-Type: application/json
```

```python
# Tbk-Api-Key-Id: Código de comercio
# Tbk-Api-Key-Secret: Llave secreta
# Content-Type: application/json
```

```http
Tbk-Api-Key-Id: Código de comercio
Tbk-Api-Key-Secret: Llave secreta
Content-Type: application/json
```

Todas las peticiones que hagas deben incluir el código de comercio y la llave
secreta entregada por Transbank, actuando ambas como las credenciales que autorizan
distintas operaciones.

<aside class="notice">
Ten en cuenta que tu(s) código(s) de comercio en ambiente de producción no son
iguales a los entregados para el ambiente de integración.
</aside>

### Códigos de comercio

En la documentación puedes revisar [todos los códigos de comercio](/documentacion/como_empezar#codigos-de-comercio) del ambiente de integración

> Los SDKs ya incluyen esos códigos de comercio y llaves secretas
> que funcionan en el ambiente de integración, por lo que puedes obtener
> rápidamente una configuración lista para hacer tus primeras pruebas en dicho
> ambiente:

## Oneclick Mall

Revisa la [documentación de Oneclick Mall](/documentacion/oneclick) para tener más información sobre como funciona
el producto y tener más detalles sobre como realizar tu integración.

### Crear una inscripción
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/oneclick#crear-una-inscripcion)

Permite comenzar con el proceso de inscripción.

```java
// Versión 3.x del SDK
Oneclick.MallInscription inscription = new Oneclick.MallInscription(new WebpayOptions(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final OneclickMallInscriptionStartResponse response = inscription.start(username, email, response_url);
// Versión 2.x del SDK
OneclickMallInscriptionStartResponse response = Oneclick.MallInscription.start(userName, email, responseUrl);
```

```php
// Versión 2.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallInscription;

// Identificador del usuario en el comercio
$username = "nombre_de_usuario";
// Correo electrónico del usuario
$email = "nombre_de_usuario@gmail.com";
// URL donde llegará el usuario con su token luego de finalizar la inscripción
$response_url = "https://callback/resultado/de/inscripcion";

$response = (new MallInscription)->start($username, $email, $response_url);

$url_webpay = $response->getUrlWebpay();
$token = $response->getToken();



// Versión 1.x del SDK
// -----------------------

use Transbank\Webpay\Oneclick\MallInscription;
// Identificador del usuario en el comercio
$username = "nombre_de_usuario";
// Correo electrónico del usuario
$email = "nombre_de_usuario@gmail.com";
// URL donde llegará el usuario con su token luego de finalizar la inscripción
$response_url = "https://callback/resultado/de/inscripcion";

$response = MallInscription::start($username, $email, $response_url);

$url_webpay = $response->getUrlWebpay();
$tbk_token = $response->getToken();
```

```csharp
using Transbank.Webpay.Oneclick;

// Versión 4.x del SDK
var ins = new MallInscription(new Options(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = ins.Start(userName, email, returnUrl);

// Versión 3.x del SDK
var response = MallInscription.Start(userName, email, returnUrl);
```

```ruby
response = Transbank::Webpay::Oneclick::MallInscription::start(
  user_name: user_name,
  email: email,
  response_url: response_url
)
```

```python
MallInscription.start(
        user_name=user_name,
        email=email,
        response_url=response_url)
```

```javascript
const Oneclick = require("transbank-sdk").Oneclick; // CommonJS
import { Oneclick } from 'transbank-sdk'; // ES6 Modules

const response = await Oneclick.MallInscription.start(
  userName, email, responseUrl
);
```

```http
POST /rswebpaytransaction/api/oneclick/v1.2/inscriptions

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
 "username": "juanperez",
 "email": "juan.perez@gmail.com",
 "response_url": "http://www.comercio.cl/return_inscription"
}
```

<strong>Parámetros Crear una inscripción</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
username  <br> <i> String </i> | Identificador del usuario registrado en el comercio. Largo máximo: 40.
email  <br> <i> String </i> | Email del usuario registrado en el comercio. Largo máximo: 100.
response_url  <br> <i> String </i> | URL del comercio a la cual Webpay redireccionará posterior al proceso de inscripción. Largo máximo: 255.

<strong>Respuesta Crear una inscripción</strong>

```java
response.getToken();
response.getUrlWebpay();
```

```php
$response->getToken();
$response->getUrlWebpay();
```

```csharp
response.Token;
response.UrlWebpay
```

```ruby
response.token
response.url_webpay
```

```python
response.token
response.url_webpay
```

```javascript
response.token
response.url_webpay
```

```http
200 OK
Content-Type: application/json

{
  "token": "e128a9c24c0a3cbc09223973327b97c8c474f6b74be509196cce4caf162a016a",
  "url_webpay": "https://webpay3g.transbank.cl/webpayserver/bp_inscription.cgi"
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Identificador, único, del proceso de inscripción. Largo: 64.
url_webpay  <br> <i> String </i> | URL de Webpay para iniciar la inscripción. Largo: 255.

<aside class="notice">
Una vez que se llama a este endpoint, el usuario debe ser redireccionado vía
POST a `urlInscriptionForm` con parámetro `TBK_TOKEN` igual al token.
</aside>

### Confirmar una inscripción

Permite finalizar el proceso de inscripción obteniendo el usuario tbk.
Más información en [la documentación](/documentacion/oneclick).

```java
// Versión 3.x del SDK
Oneclick.MallInscription inscription = new Oneclick.MallInscription(new WebpayOptions(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final OneclickMallInscriptionFinishResponse response = inscription.finish(tbk_token);

// Versión 2.x del SDK
final OneclickMallInscriptionFinishResponse response = Oneclick.MallInscription.finish(token);
```

```php

// Versión 2.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallInscription;

// Identificador del usuario en el comercio
$tbk_token = $_GET['TBK_TOKEN']; // token que llega por GET en el parámetro "TBK_TOKEN"
$response = (new MallInscription)->finish($tbk_token);
$tbkUser = $response->getTbkUser();


// Versión 1.x del SDK
// ----------------------- 
$tbk_token = "tbkToken"; // token que llega por POST en el parámetro "TBK_TOKEN"
$response = MallInscription::finish($tbk_token);
$tbkUser = $response->getTbkUser();
```

```csharp
using Transbank.Webpay.Oneclick;

// Versión 4.x del SDK
var ins = new MallInscription(new Options(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = ins.Finish(token);

// Versión 3.x del SDK
var response = MallInscription.Finish(token);

```

```ruby
response = Transbank::Webpay::Oneclick::MallInscription::finish(token: token)
```

```python
response = MallInscription.finish(token=token)
```

```javascript
const Oneclick = require('transbank-sdk').Oneclick; // CommonJS
import { Oneclick } from 'transbank-sdk'; // ES6 Modules

const response = await Oneclick.MallInscription.finish(token);
```

```http
PUT /rswebpaytransaction/api/oneclick/v1.2/inscriptions/{token}

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

<strong>Parámetros Confirmar una inscripción</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Identificador del proceso de inscripción. Es entregado por Webpay en la respuesta del método `Inscription.start()`. (See envía en la URL, no en el body)

<strong>Respuesta Confirmar una inscripción</strong>

```java
response.getAuthorizationCode();
response.getCardType();
response.getCardNumber();
response.getResponseCode();
response.getTbkUser();
```

```php
$response->getAuthorizationCode();
$response->getCardType();
$response->getCardNumber();
$response->getResponseCode();
$response->getTbkUser();
```

```csharp
response.ResponseCode;
response.TransbankUser;
response.AuthorizationCode;
response.CardType;
response.CardNumber;
```

```ruby
response.response_code
response.transbank_user
response.authorization_code
response.card_type
response.card_number
```

```python
response.response_code
response.transbank_user
response.authorization_code
response.card_type
response.card_number
```

```javascript
response.response_code
response.transbank_user
response.authorization_code
response.card_type
response.card_number
```

```http
200 OK
Content-Type: application/json

{
  "response_code": 0,
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "authorization_code": "123456",
  "card_type": "Visa",
  "card_number": "XXXXXXXXXXXX6623"
}

```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
response_code <br> <i> Number </i> | Código de respuesta de la autorización. <br> Largo: 2. <br> Valores posibles: <br> 0 = Transacción aprobada <br> Puedes revisar los códigos de respuesta de rechazo en el siguiente [link](/producto/webpay#codigos-de-respuesta-de-autorizacion)<br>
tbk_user <br> <i> String </i> | Identificador único de la inscripción del cliente en Oneclick, que debe ser usado para realizar pagos o borrar la inscripción. <br> Largo: 40.
authorization_code  <br> <i> String </i> | Código que identifica la autorización de la inscripción. <br> Largo: 6.
card_type <br> <i> cardType </i> | Indica el tipo de tarjeta inscrita por el cliente (Visa, AmericanExpress, MasterCard, Diners, Magna, Redcompra, Prepago). <br> Largo: 15.
card_number <br> <i> String </i> | Últimos 4 dígitos de la tarjeta inscrito.

### Eliminar una inscripción
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/oneclick#eliminar-una-inscripcion)

Una vez finalizado el proceso de inscripción es posible eliminarla de ser necesario. Para esto debes usar el método llamado `Inscription.remove()`.

<strong>Inscription.remove()</strong>

Permite eliminar un usuario enrolado a Oneclick Mall.

```java
// Versión 3.x del SDK
Oneclick.MallInscription inscription = new Oneclick.MallInscription(new WebpayOptions(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
inscription.delete(tbkUser, username);

// Versión 2.x del SDK
Oneclick.MallInscription.delete(tbkUser, userName)
```

```php
// Versión 2.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallInscription;

$username = 'nombre_de_usuario';
$tbkUser = 'tbkUserRetornadoPorInscriptionFinish';

$response = (new MallInscription)->delete($tbkUser, $username);


// Versión 1.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallInscription;

//...
// Identificador del usuario en el comercio
$username = 'nombre_de_usuario';
$tbkUser = 'tbkUserRetornadoPorInscriptionFinish';

//Parámetro opcional
$options = new Options($apiKey, $parentCommerceCode);
$response = MallInscription::delete($tbkUser, $username, $options);
```

```csharp
using Transbank.Webpay.Oneclick;

// Versión 4.x del SDK
var ins = new MallInscription(new Options(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = ins.Delete(tbkUser, username);

// Versión 3.x del SDK
MallInscription.Delete(userName, tbkUser);
```

```ruby
MallInscription::delete(user_name: user_name, tbk_user: tbk_user)
```

```python
MallInscription.delete(tbk_user, user_name)
```

```javascript
const Oneclick = require('transbank-sdk').Oneclick; // CommonJS
import { Oneclick } from 'transbank-sdk'; // ES6 Modules

const response = await Oneclick.MallInscription.delete(tbkUser, userName);
```

```http
DELETE /rswebpaytransaction/api/oneclick/v1.2/inscriptions

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json


{
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "username": "juanperez",
}
```

<strong>Parámetros Eliminar una inscripción</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente (devuelto por `Inscription.finish()`). Largo: 40.
username  <br> <i> String </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `Inscription.start()`). Largo máximo: 40.

<strong>Respuesta Eliminar una inscripción</strong>

Esta petición no posee cuerpo de respuesta, solo entrega un 204 cuando se realiza correctamente

```java
// 204 OK
```

```php
// 204 OK
```

```csharp
// 204 OK
```

```ruby
# 204 OK
```

```python
# 204 OK
```

```javascript
// 204 OK
```

```http
204 OK
Content-Type: application/json
```

### Autorizar una transacción
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/oneclick#autorizar-una-transaccion)

Una vez realizada la inscripción, el comercio puede usar el `tbkUser` recibido
para realizar transacciones. Para eso debes usar el método `Transaction.authorize()`.

<strong>Transaction.authorize()</strong>

Permite autorizar un pago.

```java
MallTransactionCreateDetails details = MallTransactionCreateDetails.build()
  .add(amountMallOne, commerceCodeMallOne, buyOrderMallOne, installmentsNumberMallOne)
  .add(amountMallTwo, commerceCodeMallTwo, buyOrderMallTwo, installmentsNumberMallTwo);

// Versión 3.x del SDK
Oneclick.MallTransaction tx = new Oneclick.MallTransaction(new WebpayOptions(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final OneclickMallTransactionAuthorizeResponse response = tx.authorize(username, tbkUser, buyOrder, details);

// Versión 2.x del SDK
final OneclickMallTransactionAuthorizeResponse response = Oneclick.Transaction.authorize(username, tbkUser, buyOrder, details);
```

```php
// Versión 2.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallTransaction;
use Transbank\Webpay\Oneclick;

// Identificador del usuario en el comercio
$parentBuyOrder = rand(100000, 999999999);

$details = [
    [
        "commerce_code" => Oneclick::DEFAULT_CHILD_COMMERCE_CODE_1,
        "buy_order" => rand(100000, 999999999), // Tu propio buyOrder
        "amount" => 50000,
        "installments_number" => 1
    ],
    [
        "commerce_code" => Oneclick::DEFAULT_CHILD_COMMERCE_CODE_2,
        "buy_order" => rand(100000, 999999999), // Tu propio buyOrder
        "amount" => 20000,
        "installments_number" => 1  
    ]
];

$response = (new MallTransaction)->authorize($username, $tbkUser, $parentBuyOrder, $details);


// Versión 1.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallInscription;

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

$response = MallTransaction::authorize($username, $tbkUser, $parentBuyOrder, $details);
```

```csharp
using Transbank.Webpay.Oneclick;

List<PaymentRequest> details = new List<PaymentRequest>();
details.Add(new PaymentRequest(
  childCommerceCodeOne, buyOrderMallOne, amountMallOne, installmentsNumber
));
details.Add(new PaymentRequest(
  childCommerceCodeTwo, buyOrderMallTwo, amountMallTwo, installmentsNumber
));

// Versión 4.x del SDK
var tx = new MallTransaction(new Options(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var result = tx.Authorize(userName, tbkUser, buyOrder, details);

// Versión 3.x del SDK
var result = MallTransaction.Authorize(userName, tbkUser, buyOrder, details);
```

```ruby
details = [
  {
    commerce_code: amountMallOne,
    buy_order: buyOrderMallOne,
    amount: amountMallOne,
    installments_number: installmentsNumberMallOne
  },
  {
    commerce_code: amountMallTwo,
    buy_order: buyOrderMallTwo,
    amount: amountMallTwo,
    installments_number: installmentsNumberMallTwo
  }
]

Transbank::Webpay::Oneclick::MallTransaction::authorize(username: username,
                                                       tbk_user: tbk_user,
                                                       parent_buy_order: buy_order,
                                                       details: details)
```

```python
details = MallTransactionAuthorizeDetails(
  commerce_code, buy_order_child, installments_number, amount
).add(
  commerce_code2, buy_order_child2, installments_number2, amount2
)

MallTransaction.authorize(
  user_name=user_name,
  tbk_user=tbk_user,
  buy_order=buy_order,
  details=details
)
```

```javascript
const Oneclick = require('transbank-sdk').Oneclick; // CommonJS
const TransactionDetail = require('transbank-sdk').TransactionDetail; // CommonJS
import { Oneclick, TransactionDetail } from 'transbank-sdk'; // ES6 Modules

const details = [
  new TransactionDetail(amount, commerceCode, childBuyOrder),
  new TransactionDetail(amount2, commerceCode2, childBuyOrder2)
];

const response = await Oneclick.MallTransaction.authorize(
  userName, tbkUser, buyOrder, details
);
```

```http
POST /rswebpaytransaction/api/oneclick/v1.2/transactions

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "username": "juanperez",
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "buy_order": "ordenCompra123456789",
  "details": [
    {
      "commerce_code": "597055555542",
      "buy_order": "ordenCompra123445",
      "amount": 1000,
      "installments_number": 5
  }]
}
```

<strong>Parámetros Autorizar un pago</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
username  <br> <i> String </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `Inscription.start()`). Largo máximo: 40.
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente (devuelto por `Inscription.finish()`). Largo: 40.
buy_order  <br> <i> Number </i> | Identificador único de la compra generado por el comercio. Largo máximo: 26.
details  <br> <i> Array </i> | Lista de objetos, uno por cada tienda diferente del mall que participa en la transacción.
details [].commerce_code  <br> <i>String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo: 12.
details [].buy_order  <br> <i> String </i> | Identificador único de la compra generado por el comercio hijo (tienda). Largo máximo: 26.
details [].amount  <br> <i> Decimal </i> | Monto de la transacción de pago. Largo máximo: 17.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la transacción de pago. Largo 2. No obligatorio.

<strong>Respuesta Autorizar un pago</strong>

```java
response.getAccountingDate();
response.getBuyOrder();
response.getTransactionDate();
final CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
final List<Detail> detailsResp = response.getDetails();
for (Detail detail : detailsResp) {
    detail.getAmount();
    detail.getAuthorizationCode();
    detail.getBuyOrder();
    detail.getCommerceCode();
    detail.getInstallmentsNumber();
    detail.getPaymentTypeCode();
    detail.getStatus();
}
```

```php
print_r($response);
$response->getAccountingDate();
$response->getBuyOrder();
$response->getTransactionDate();
$details = $response->getDetails();
foreach($details as $detail){
    $detail->getAmount();
    $detail->getAuthorizationCode();
    $detail->getBuyOrder();
    $detail->getCommerceCode();
    $detail->getInstallmentsNumber();
    $detail->getPaymentTypeCode();
    $detail->getResponseCode();
    $detail->getStatus();
}

// Transbank\Webpay\Oneclick\Responses\MallTransactionAuthorizeResponse Object
// (
//     [buyOrder] => 433025339
//     [sessionId] => 
//     [cardNumber] => 6623
//     [expirationDate] => 
//     [accountingDate] => 0413
//     [transactionDate] => 2021-04-13T22:59:53.767Z
//     [details] => Array
//         (
//             [0] => Transbank\Webpay\Oneclick\Responses\TransactionDetail Object
//                 (
//                     [amount] => 50000
//                     [status] => AUTHORIZED
//                     [authorizationCode] => 1213
//                     [paymentTypeCode] => VN
//                     [responseCode] => 0
//                     [installmentsNumber] => 0
//                     [installmentsAmount] => 
//                     [commerceCode] => 597055555542
//                     [buyOrder] => 523485045
//                 )

//             [1] => Transbank\Webpay\Oneclick\Responses\TransactionDetail Object
//                 (
//                     [amount] => 20000
//                     [status] => AUTHORIZED
//                     [authorizationCode] => 1213
//                     [paymentTypeCode] => VN
//                     [responseCode] => 0
//                     [installmentsNumber] => 0
//                     [installmentsAmount] => 
//                     [commerceCode] => 597055555543
//                     [buyOrder] => 224502696
//                 )

//         )

// )
```

```csharp
response.AccountingDate;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.TransactionDate;
var details = response.Details;
foreach (var detail in details) {
    detail.Amount;
    detail.AuthorizationCode;
    detail.BuyOrder;
    detail.CommerceCode;
    detail.InstallmentsNumber;
    detail.PaymentTypeCode;
    detail.ResponseCode;
    detail.Status;
}
```

```ruby
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.transaction_date
details = response.details
details.each do |detail|
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
end
```

```python
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.transaction_date
details = response.details
for detail in details:
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
```

```javascript
response.accounting_date
response.buy_order
cardDetail = response.card_detail
cardDetail.card_number
response.transaction_date
details = response.details
for(let detail of details) {
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
}
```

```http
200 OK
Content-Type: application/json

{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15:43:48.523Z",
  "details": [
    {
      "amount": 500,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555542",
      "buy_order": "505479072"
  }]
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra generada por el comercio padre.
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_detail.card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción.
accounting_date  <br> <i> String </i> | Fecha contable de la autorización del pago.
transaction_date  <br> <i> DateTime </i> | Fecha completa (timestamp) de la autorización del pago. ISO 8601
details  <br> <i> Array </i> | Lista con el resultado de cada transacción de las tiendas hijas.
details [].amount  <br> <i> Decimal </i> | Monto de la transacción de pago.
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED).
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción de pago.
details [].payment_type_code  <br> <i> String </i> | [Tipo](/producto/webpay#tipos-de-pago) de pago de la transaccion. <br> VD = Venta Débito.<br>VP = Venta prepago<br>VN = Venta Normal.<br>VC = Venta en cuotas.<br>SI = 3 cuotas sin interés.<br>S2 = 2 cuotas sin interés.<br>NC = N Cuotas sin interés<br>
details [].response_code  <br> <i> Number </i> | Código del resultado del pago, donde: 0 (cero) es aprobado. Valores posibles: <br> 0 = Transacción aprobada <br> Puedes revisar los códigos de respuesta de rechazo en el siguiente [link](/producto/webpay#codigos-de-respuesta-de-autorizacion)<br> Algunos códigos específicos para Oneclick son: <br>-96: `tbk_user` no existente <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la transacción de pago.
details [].commerce_code  <br> <i> Number </i> | Código de comercio del comercio hijo (tienda).
details [].buy_order  <br> <i> String </i> | Orden de compra generada por el comercio hijo para la transacción de pago.

<aside class="warning">
Cualquier valor distinto de número en `installmentsNumber` (incluyendo letras,
inexistencia del campo o nulo) será asumido como cero, es decir "Sin cuotas".
</aside>

### Obtener estado de una transacción

Permite consultar el estado de pago realizado a través de Oneclick.
Retorna el resultado de la autorización.

Puedes revisar más detalles de esta operación en [su documentación](/documentacion/oneclick#obtener-estado-de-una-transaccion)

```java
// Versión 3.x del SDK
Oneclick.MallTransaction tx = new Oneclick.MallTransaction(new WebpayOptions(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final OneclickMallTransactionStatusResponse response = tx.status(buyOrder);

// Versión 2.x del SDK
final OneclickMallTransactionStatusResponse response =
  Oneclick.Transaction.status(buyOrder);
```

```php
// Versión 2.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallTransaction;
$response = (new MallTransaction)->status($buyOrder);


// Versión 1.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallTransaction;

$response = MallTransaction::getStatus($buyOrder);
```

```csharp
using Transbank.Webpay.Oneclick;
// Versión 4.x del SDK
var tx = new MallTransaction(new Options(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var result = tx.Status(buyOrder);

// Versión 3.x del SDK
var result = MallTransaction.Status(buyOrder);
```

```ruby
response = Transbank::Webpay::Oneclick::MallTransaction::status(buy_order: buy_order)
```

```python
var response = MallTransaction.status(buy_order)
```

```javascript
const Oneclick = require('transbank-sdk').Oneclick; // CommonJS
import { Oneclick } from 'transbank-sdk'; // ES6 Modules

const response = await Oneclick.MallTransaction.status(token);
```

```http
GET /rswebpaytransaction/api/oneclick/v1.2/transactions/{buyOrder}

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

<strong>Parámetros Consultar un pago realizado</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la transacción a consultar (se envía en la URL, no en el body).

<strong>Respuesta Consultar un pago realizado</strong>

```java
response.getAccountingDate();
response.getBuyOrder();
response.getTransactionDate();
final CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
final List<Detail> detailsResp = response.getDetails();
for (Detail detail : detailsResp) {
    detail.getAmount();
    detail.getAuthorizationCode();
    detail.getBuyOrder();
    detail.getCommerceCode();
    detail.getInstallmentsNumber();
    detail.getPaymentTypeCode();
    detail.getStatus();
}
```

```php
print_r($response);
$response->getAccountingDate();
$response->getBuyOrder();
$response->getTransactionDate();
$details = $response->getDetails();
foreach($details as $detail){
    $detail->getAmount();
    $detail->getAuthorizationCode();
    $detail->getBuyOrder();
    $detail->getCommerceCode();
    $detail->getInstallmentsNumber();
    $detail->getPaymentTypeCode();
    $detail->getResponseCode();
    $detail->getStatus();
}

// Transbank\Webpay\Oneclick\Responses\MallTransactionAuthorizeResponse Object
// (
//     [buyOrder] => 433025339
//     [sessionId] => 
//     [cardNumber] => 6623
//     [expirationDate] => 
//     [accountingDate] => 0413
//     [transactionDate] => 2021-04-13T22:59:53.767Z
//     [details] => Array
//         (
//             [0] => Transbank\Webpay\Oneclick\Responses\TransactionDetail Object
//                 (
//                     [amount] => 50000
//                     [status] => AUTHORIZED
//                     [authorizationCode] => 1213
//                     [paymentTypeCode] => VN
//                     [responseCode] => 0
//                     [installmentsNumber] => 0
//                     [installmentsAmount] => 
//                     [commerceCode] => 597055555542
//                     [buyOrder] => 523485045
//                 )

//             [1] => Transbank\Webpay\Oneclick\Responses\TransactionDetail Object
//                 (
//                     [amount] => 20000
//                     [status] => AUTHORIZED
//                     [authorizationCode] => 1213
//                     [paymentTypeCode] => VN
//                     [responseCode] => 0
//                     [installmentsNumber] => 0
//                     [installmentsAmount] => 
//                     [commerceCode] => 597055555543
//                     [buyOrder] => 224502696
//                 )

//         )

// )
```

```csharp
response.AccountingDate;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.SessionId;
response.TransactionDate;
response.Vci;
var details = response.Details;
foreach (var detail in details) {
    detail.Amount;
    detail.AuthorizationCode;
    detail.BuyOrder;
    detail.CommerceCode;
    detail.InstallmentsNumber;
    detail.PaymentTypeCode;
    detail.ResponseCode;
    detail.Status;
}
```

```ruby
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.each do |detail|
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
end
```

```python
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for detail in details:
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
```

```javascript
response.accounting_date
response.buy_order
cardDetail = response.card_detail
cardDetail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for(detail on details) {
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
}
```

```http
200 OK
Content-Type: application/json

{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15:43:48.523Z",
  "details": [
    {
      "amount": 500,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555542",
      "buy_order": "505479072"
  }]
}

```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra generada por el comercio padre.
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_detail.card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción.
accounting_date  <br> <i> String </i> | Fecha contable de la autorización del pago.
transaction_date  <br> <i> DateTime </i> | Fecha completa (timestamp) de la autorización del pago. Largo: 24, formato: ISO 8601 (Ej: yyyy-mm-ddTHH:mm:ss.xxxZ)
details  <br> <i> Array </i> | Lista con el resultado de cada transacción de las tiendas hijas.
details [].amount  <br> <i> Decimal </i> | Monto de la sub-transacción de pago.
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED).
details [].authorization_code  <br> <i> String </i> | Código de autorización de la sub-transacción de pago.
details [].payment_type_code  <br> <i> String </i> | [Tipo](/producto/webpay#tipos-de-pago) de pago de la transaccion. <br> VD = Venta Débito.<br>VP = Venta prepago<br>VN = Venta Normal.<br>VC = Venta en cuotas.<br>SI = 3 cuotas sin interés.<br>S2 = 2 cuotas sin interés.<br>NC = N Cuotas sin interés<br>
details [].response_code  <br> <i> Number </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. Valores posibles: <br> 0 = Transacción aprobada <br> Puedes revisar los códigos de respuesta de rechazo en el siguiente [link](/producto/webpay#codigos-de-respuesta-de-autorizacion)<br> Algunos códigos específicos para Oneclick son: <br>-96: `tbk_user` no existente <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la sub-transacción de pago.
details [].commerce_code  <br> <i> Number </i> | Código de comercio del comercio hijo (tienda).
details [].buy_order  <br> <i> String </i> | Orden de compra generada por el comercio hijo para la sub-transacción de pago.
status  <br> <i> Text </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
balance  <br> <i> Decimal </i> | Monto restante de la sub-transacción de pago original: monto inicial – monto anulado. Largo máximo: 17

### Reversar o anular una transacción

Puedes revisar más detalles de esta operación en [su documentación](/documentacion/oneclick#reversar-o-anular-una-transaccion)

```java
// Versión 3.x del SDK
Oneclick.MallTransaction tx = new Oneclick.MallTransaction(new WebpayOptions(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final OneclickMallTransactionRefundResponse response = tx.refund(buyOrder, childCommerceCode, childBuyOrder, amount);

// Versión 2.x del SDK
final OneclickMallTransactionRefundResponse response =
  Oneclick.Transaction.refund(buyOrder, childCommerceCode, childBuyOrder, amount);
```

```php
// Versión 2.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallTransaction;

$response = (new MallTransaction)->refund($buyOrder, $childCommerceCode, $childBuyOrder, $amount);

// Transbank\Webpay\Oneclick\Responses\MallTransactionRefundResponse Object
// (
//     [type] => NULLIFIED
//     [authorizationCode] => 183633
//     [authorizationDate] => 2021-04-13T23:07:37.683Z
//     [nullifiedAmount] => 10000
//     [balance] => 40000
//     [responseCode] => 0
// )







// Versión 1.x del SDK
// ----------------------- 

//Parámetro opcional
$options = new Options($apiKey, $parentCommerceCode);

$response = MallTransaction::refund($buyOrder, $childCommerceCode, $childBuyOrder, $amount);
```

```csharp
using Transbank.Webpay.Oneclick;

// Versión 4.x del SDK
var tx = new MallTransaction(new Options(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var result = tx.Refund(buyOrder, childCommerceCode, childBuyOrder, amount);

// Versión 3.x del SDK
var response = MallTransaction.Refund(buyOrder, childCommerceCode,childBuyOrder,amount);
```

```ruby
response = Transbank::Webpay::Oneclick::MallTransaction::refund(
  buy_order: buy_order,
  child_commerce_code: child_commerce_code,
  child_buy_order: child_buy_order,
  amount: amount
)
```

```python
var response = MallTransaction.refund(buy_order, child_commerce_code, child_buy_order, amount)
```

```javascript
const Oneclick = require('transbank-sdk').Oneclick; // CommonJS
import { Oneclick } from 'transbank-sdk'; // ES6 Modules

const response = await Oneclick.MallTransaction.refund(buyOrder, childCommerceCode, childBuyOrder, amount);
```

```http
POST /rswebpaytransaction/api/oneclick/v1.2/transactions/{buyOrder}/refunds

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "commerce_code": "597055555542",
  "detail_buy_order": "ordenCompra12345",
  "amount": 1000
}
```

<strong>Parámetros Reversar o Anular</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la transacción a  reversar o anular. Se envía en la URL, no en el body. Largo máximo: 26.
commerce_code  <br> <i> String </i> | Código de comercio hijo. Largo máximo: 12.
detail_buy_order  <br> <i> String </i> | Orden de compra hija de la transacción a  reversar o anular. Largo máximo: 26.
amount  <br> <i> Formato número entero para transacciones en peso. Sólo en caso de dólar acepta dos decimales. </i> | Monto que se desea anular o reversar de la transacción. Largo máximo: 17

<strong>Respuesta Reversar o Anular</strong>

En el caso de que la transacción corresponda a una Reversa solo se retorna el parámetro <i>type<i> (REVERSED).

```java
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getBalance();
response.getNullifiedAmount();
response.getResponseCode();
response.getType();
```

```php
$response->getAuthorizationCode();
$response->getAuthorizationDate();
$response->getBalance();
$response->getNullifiedAmount();
$response->getResponseCode();
$response->getType();
```

```csharp
response.AuthorizationCode;
response.AuthorizationDate;
response.Balance;
response.NullifiedAmount;
response.ResponseCode;
response.Type;
```

```ruby
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
```

```python
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
```

```javascript
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
```

```http
200 OK
Content-Type: application/json

{
  "type": "NULLIFIED",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20:18:20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}

En caso de una reversa no devuelve más información
{
  "type": "REVERSED",
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso, REVERSED o NULLIFIED, si es REVERSED no se devolverán datos de la transacción (authorization code, etc). Largo máximo: 10
authorization_code  <br> <i> Boolean </i> | (Solo si es NULLIFIED)  Código de autorización. Largo máximo: 6
authorization_date  <br> <i> ISO8601 </i> | (Solo si es NULLIFIED)  Fecha de la autorización de la transacción.
nullified_amount  <br> <i> Decimal </i> | (Solo si es NULLIFIED)  Monto anulado. Largo máximo: 17
balance  <br> <i> Decimal </i> | (Solo si es NULLIFIED)  Monto restante de la transacción de pago original: monto inicial – monto anulado. Largo máximo: 17
response_code <br> <i> Number </i> | (Solo si es NULLIFIED)  Código del resultado del pago, donde: 0 (cero) es aprobado. Largo máximo: 2
buy_order  <br> <i> String </i> | (Solo si es NULLIFIED)  Orden de compra generada por el comercio hijo para la transacción de pago. Largo máximo: 26.

### Captura diferida de una transacción

Revisa más detalles sobre esta modalidad en [la documentación](/documentacion/oneclick#capturar-una-transaccion)

```java
// Versión 3.x del SDK
Oneclick.MallTransaction tx = new Oneclick.MallTransaction(new WebpayOptions(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final OneclickMallTransactionCaptureResponse response = tx.capture(childCommerceCode, childBuyOrder, authorizationCode, amount);

// Versión 2.x del SDK
final OneclickMallTransactionCaptureResponse response = Oneclick.MallDeferredTransaction.capture(
  childCommerceCode, childBuyOrder, amount, authorizationCode
);
```

```php
// Versión 2.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallTransaction;
$response = (new MallTransaction)->capture($commerce_code, $buy_order, $authorization_code, $amount);

// Versión 1.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallTransaction;
$response = MallTransaction::capture($commerce_code, $buy_order, $authorization_code, $amount);```

```csharp
// Versión 4.x del SDK
var tx = new MallTransaction(new Options(IntegrationCommerceCodes.ONECLICK_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var result = tx.Capture(ChildcommerceCode, ChildbuyOrder, authorizationCode, amount);
```

```ruby
response = Transbank::Webpay::Oneclick::MallDeferredTransaction::capture(
  child_commerce_code: @commerce_code, child_buy_order: @buy_order,
  amount: @capture_amount, authorization_code: @authorization_code
)
```

```python
# Esta funcion aun no se encuentra disponible en el SDK
```

```javascript
const Oneclick = require('transbank-sdk').Oneclick; // CommonJS
import { Oneclick } from 'transbank-sdk'; // ES6 Modules

const response = Oneclick.MallTransaction.capture(
  childCommerceCode, childBuyOrder, amount, authorizationCode
);
```

```http
PUT /rswebpaytransaction/api/oneclick/mall/v1.2/transactions/capture
Tbk-Api-Key-Id: 597055555547
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
{
    "commerce_code": 597055555548,
    "buy_order": "OCDT12345678",
    "capture_amount": 50,
    "authorization_code": "1213"
}
```

<strong>Parámetros Captura Diferida</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
commerce_code  <br> <i> Number </i> | Tienda hija que realizó la transacción. Largo: 6.
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
capture_amount  <br> <i> Decimal </i> | Monto que se desea capturar. Largo máximo: 17.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere capturar Largo máximo: 6.

<aside class="notice">
El método `capture()` debe ser invocado siempre indicando el código del
comercio de la tienda virtual específica.
</aside>

<strong>Respuesta Captura Diferida</strong>

```java
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getCapturedAmount();
response.getResponseCode();
```

```php
$response->getAuthorizationCode();
$response->getAuthorizationDate();
$response->getCapturedAmount();
$response->getResponseCode();
```

```csharp
response.AuthorizationCode;
response.AuthorizationDate;
response.CapturedAmount;
response.ResponseCode;
```

```ruby
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
```

```python
# Esta función aun no se encuentra disponible en el SDK
```

```javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
```

```http
200 OK
Content-Type: application/json
{
    "authorization_code": "152759",
    "authorization_date": "2020-04-03T01:49:50.181Z",
    "captured_amount": 50,
    "response_code": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorization_code  <br> <i> String </i> | Código de autorización de la captura diferida. Largo máximo: 6
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización.
captured_amount  <br> <i> Decimal </i> | Monto capturado. Largo máximo: 6
response_code  <br> <i> Number </i> | Código de resultado de la captura. Si es exitoso es 0,de lo contrario la captura no fue realizada. Largo máximo: 2

<aside class="notice">
En caso de error apareceran los mismos códigos exclusivos del método `capture()`
para captura simpultanea.
</aside>


## Códigos y mensajes de error

Al realizar cualquier solicitud al API REST, además de los datos de respuesta, se incluirá uno de los siguientes códigos de estado de respuesta HTTP dependiendo del resultado obtenido:  

### Solicitud exitosa

Cuando la operación solcitada es ejecutada correctamente, se pueden recibir estos status HTTP:

Código de estado HTTP | Descripción
------ | -----------
200 | La operación se ha ejecutado exitosamente
204 | La operación DELETE se ha ejecutado exitosamente

<strong>Códigos de error</strong>

Todos los errores reportados por la API REST de Webpay despliegan un mensaje JSON con una descripción del error.

```json
{
  "error_message": "token is required"
}
```

Código de estado HTTP | Descripción
------ | -----------
400 | El mensaje JSON es inválido. Puedes ser que no corresponda a un mensaje bien estructurado o que contenga un campo no esperado.
401 | No autorizado. API Key y/o API Secret inválidos
404 | La transacción no ha sido encontrada.
405 | Método no permitido.
406 | No fue posible procesar la respuesta en el formato que el cliente indica.
415 | Tipo de mensaje no permitido.
422 | El requerimiento no ha podido ser procesado ya sea por validaciones de datos o por lógica de negocios.
500 | Ha ocurrido un error inesperado.

## Puesta en Producción

1. Una vez que el comercio determine que ha finalizado su integración, se debe realizar un [proceso de validación](/referencia/webpay#proceso-de-validacion).

2. Una vez que Transbank confirme que la planilla de integración se encuentra correcta (no aplica para plugins), se enviará al comercio la confirmación y se generará su **secreto compartido**, que en conjunto con el código de comercio, permiten operar en producción.

3. Cuando recibas el correo, será necesario [cambiar la configuración del e-commerce para funcionar en producción](#configuracion-para-produccion-utilizando-los-sdk)

4. Con la configuración del ambiente de producción ya lista, será necesario realizar una compra de $10 para validar el correcto funcionamiento.

### Proceso de validación

Durante la validación de la integración se pretende verificar que el comercio transacciona de manera segura y sin problemas, por lo que se solicitarán una serie de pruebas y su posterior envío de evidencias para validar la integración. Esta validación es un requisito para que el comercio pueda operar en el ambiente de producción (bancos y dinero real) y no se permitirá que un comercio utilice productivamente el servicio sin poseer una validación.

Transbank solo validará las integraciones de aquellos comercios que tengan un código de comercio productivo. Para obtenerlo, sigue las instrucciones para hacerte cliente en el portal [http://www.transbank.cl](http://www.transbank.cl) o contacta a tu ejecutivo comercial.

En esta etapa, el comercio envía las evidencias a [soporte@transbank.cl](mailto:soporte@transbank.cl) en **formato PDF** empleando el formulario correspondiente al producto integrado indicando claramente las órdenes de compra, fecha y hora de las transacciones. Para integraciones Webpay que utilicen algún [plugin oficial](https://transbankdevelopers.cl/plugin) existe un formulario especial.

[Descargar el formulario de envidencias](https://transbankdevelopers.cl/files/evidencia-integracion-webpay-rest.docx)

Soporte validará que los casos de prueba sean consistentes con los registrados en los sistemas de Webpay y, de estar todo correcto, se le notificará al comercio la conformidad para pasar a producción, recibiendo las instrucciones para ello. De no estar consistentes las pruebas, se le hará alcances al comercio respecto de su integración, para que realices las correcciones correspondientes y vuelvas a enviar las evidencias una vez terminadas dichas correcciones.

En el proceso de contratación recibiste tu código de comercio, y junto con el **secreto compartido** que se te entregó luego de la certificación puedes completar tus credenciales, las cuales **debes custodiar y evitar que estén en manos de terceros** ya que permiten hacer (o anular) transacciones en nombre de tu comercio.

* Código de comercio (*API Key*)
* Secreto compartido (*Shared Secret*)

Luego que el proceso de validación de tu integración está terminado, debes realizar la configuración para que tu sitio se encuentre en producción.

### Configuración para producción utilizando los SDK

Si estas utilizando algún SDK oficial de Transbank, entonces debes seguir los siguientes pasos.

<aside class="warning">
Nunca dejes tu código de comercio y secreto compartido directamente en tu código, te recomendamos utilizar variables de entorno u otro método que te permita mantener tus credenciales seguras.
</aside>

1. Asignar el código de comercio productivo, entregado por Transbank al momento de contratar el producto.

```java
// Para Oneclick
Oneclick.setCommerceCode('TU_CODIGO_DE_COMERCIO');
```

```php
// Sin configurar nada, el SDK viene preconfigurado con las credenciales de prueba de Oneclick mall captura simultanea para el ambiente de integración
// Para configurar en producción: 
use \Transbank\Webpay\Oneclick;
Oneclick:configureForProduction('597012345678', 'ApiKey');

// Para configurar en integración: 
Oneclick::configureForIntgration('597012345678', 'ApiKey');
Oneclick::configureForTesting();
Oneclick::configureForTestingDeferred();

// También puedes crear un objeto Options y pasarlo directo a la instancia
use \Transbank\Webpay\Options;
use \Transbank\Webpay\Oneclick\MallTransaction;
use \Transbank\Webpay\Oneclick\MallInscription;

$options = Options::forProduction('codigo-comercio', 'apikey'); // o Options::forIntegration('comercio', 'key')
$transaction = new MallTransaction($options);
$transaction->authorize(...);
$inscription = new MallInscription($options);
$inscription->start(...);

// También es posible así: 
$transaction = (new MallTransaction())->configureForProduction('codigo-comercio', 'api-key');
$transaction->authorize();
```

```csharp
// Esta función aun no se encuentra disponible en el SDK
```

```ruby
# Esta función aun no se encuentra disponible en el SDK

```

```python
# Esta función aun no se encuentra disponible en el SDK
```

```javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
```

```http
200 OK
Content-Type: application/json
{
    "authorization_code": "152759",
    "authorization_date": "2020-04-03T01:49:50.181Z",
    "captured_amount": 50,
    "response_code": 0
}
```

```php
```

```csharp
// Esta función aun no se encuentra disponible en el SDK
```

```ruby
# Esta función aun no se encuentra disponible en el SDK

```

```python
# Esta función aun no se encuentra disponible en el SDK
```

```javascript
const Oneclick = require('transbank-sdk').Oneclick; // CommonJS
import { Oneclick } from 'transbank-sdk'; // ES6 Modules

Oneclick.commerceCode = 'TU_CODIGO_DE_COMERCIO';
```

2. Configuración del secreto compartido.

```java
// Para Oneclick
Oneclick.setApiKey('TU_API_KEY');
```

```php
```

```csharp
// Esta función aun no se encuentra disponible en el SDK
```

```ruby
# Esta función aun no se encuentra disponible en el SDK

```

```python
# Esta función aun no se encuentra disponible en el SDK
```

```javascript
const Oneclick = require('transbank-sdk').Oneclick; // CommonJS
import { Oneclick } from 'transbank-sdk'; // ES6 Modules

Oneclick.apiKey = 'TU_API_KEY';
```

3. Selección del ambiente productivo.

```java
// Para Oneclick
Oneclick.setIntegrationType(IntegrationType.LIVE);
```

```php
```

```csharp
// Esta función aun no se encuentra disponible en el SDK
```

```ruby
# Esta función aun no se encuentra disponible en el SDK

```

```python
# Esta función aun no se encuentra disponible en el SDK
```

```javascript
const Oneclick = require('transbank-sdk').Oneclick; // CommonJS
const Environment = require('transbank-sdk').Environment // CommonJS
import { Oneclick, Environment } from 'transbank-sdk'; // ES6 Modules

Oneclick.environment = Environment.Production;
```

Alternativamente algunos SDK ofrecen un método para configurar directamente a producción
```java
// Esta función aun no se encuentra disponible en el SDK
```

```php
```

```csharp
// Esta función aun no se encuentra disponible en el SDK
```

```ruby
# Esta función aun no se encuentra disponible en el SDK

```

```python
# Esta función aun no se encuentra disponible en el SDK
```

```javascript
const Oneclick = require('transbank-sdk').Oneclick; // CommonJS
import { Oneclick } from 'transbank-sdk'; // ES6 Modules

Oneclick.configureForProduction('TU_CODIGO_DE_COMERCIO', 'TU_API_KEY');
```
