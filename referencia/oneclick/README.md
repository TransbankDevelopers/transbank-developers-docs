# Oneclick REST

___

<aside class="notice">
Estás viendo la <strong>nueva referencia REST</strong>. Si quieres ver la referencia anterior (deprecada)
(SOAP) haz [click aquí](/referencia/webpay-soap)
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
// Tbk-Api-Key-Id: Código de comercio
// Tbk-Api-Key-Secret: Llave secreta
// Content-Type: application/json
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

Permite gatillar el inicio del proceso de inscripción.
Más información en [la documentación](/documentacion/oneclick).

```java
OneclickMallInscriptionStartResponse response = OneclickMall.Inscription.start(userName, email, responseUrl);
```

```php
use Transbank\Webpay\Oneclick;

$response = MallInscription::start($userName, $email, $responseUrl);
```

```csharp
using Transbank.Webpay.Oneclick;

var response = Inscription.Start(userName, email, returnUrl);
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

```http
POST /rswebpaytransaction/api/oneclick/v1.0/inscriptions

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
Una vez que se llama a este webservice el usuario debe ser redireccionado vía
POST a `urlInscriptionForm` con parámetro `TBK_TOKEN` igual al token.
</aside>

### Confirmar una inscripción

Permite finalizar el proceso de inscripción obteniendo el usuario tbk.
Más información en [la documentación](/documentacion/oneclick).

```java
final OneclickMallInscriptionFinishResponse response = OneclickMall.Inscription.finish(token);
```

```php
use Transbank\Webpay\Oneclick;

$response = MallInscription::finish($token);
```

```csharp
using Transbank.Webpay.Oneclick;

var response = Inscription.Finish(token);

```

```ruby
response = Transbank::Webpay::Oneclick::MallInscription::finish(token: token)
```

```python
response = MallInscription.finish(token=token)
```

```http
PUT /rswebpaytransaction/api/oneclick/v1.0/inscriptions/{token}

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
response->getAuthorizationCode();
response->getCardType();
response->getCardNumber();
response->getResponseCode();
response->getTbkUser();
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
response_code <br> <i> Number </i> | Código de respuesta de la autorización. <br> Largo: 2. <br> Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente (Posible error en el ingreso de datos de la transacción) <br> -2 = Rechazo de transacción (Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada) <br> -3 = Error en transacción (Interno Transbank) <br> -4 = Rechazo emisor (Rechazada por parte del emisor) <br> -5 = Rechazo - Posible Fraude (Transacción con riesgo de posible fraude)
tbk_user <br> <i> String </i> | Identificador único de la inscripción del cliente en Oneclick, que debe ser usado para realizar pagos o borrar la inscripción. <br> Largo: 40.
authorization_code  <br> <i> String </i> | Código que identifica la autorización de la inscripción. <br> Largo: 6.
card_type <br> <i> cardType </i> | Indica el tipo de tarjeta inscrita por el cliente (Visa, AmericanExpress, MasterCard, Diners, Magna, Redcompra). <br> Largo: 10.
card_number <br> <i> String </i> | Últimos 4 dígitos de la tarjeta inscrito: <br> Largo: 4.

### Eliminar una inscripción

Una vez finalizado el proceso de inscripción es posible eliminarla de ser necesario. Para esto debes usar el método llamado `Inscription.remove()`.

#### Inscription.remove()

Permite eliminar un usuario enrolado a Oneclick Mall.

```java
Oneclick.MallInscription.delete(tbkUser, userName)
```

```php
use Transbank\Webpay\Oneclick;

MallInscription::delete($tbkUser, $userName);
```

```csharp
using Transbank.Webpay.Oneclick;

Inscription.Delete(userName, tbkUser);
```

```ruby
MallInscription::delete(user_name: user_name, tbk_user: tbk_user)
```

```python
MallInscription.delete(tbk_user, user_name)
```

```http
DELETE /rswebpaytransaction/api/oneclick/v1.0/inscriptions

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

```http
204 OK
Content-Type: application/json
```

### Autorizar un pago

Una vez realizada la inscripción, el comercio puede usar el `tbkUser` recibido
para realizar transacciones. Para eso debes usar el método `Transaction.authorize()`.

#### Transaction.authorize()

Permite autorizar un pago.

```java
MallTransactionCreateDetails transactionDetails = MallTransactionCreateDetails.build()
  .add(amountMallOne, commerceCodeMallOne, buyOrderMallOne, installmentsNumberMallOne)
  .add(amountMallTwo, commerceCodeMallTwo, buyOrderMallTwo, installmentsNumberMallTwo);

final OneclickMallTransactionAuthorizeResponse response = OneclickMall.Transaction.authorize(username, tbkUser, buyOrder, transactionDetails);
```

```php
use Transbank\Webpay\Oneclick;

$details = [
    [
        "commerce_code" => "597055555542",
        "buy_order" => "ordenCompra123445",
        "amount" => 1000,
        "installments_number" => 5
    ]
];


$response = MallTransaction::authorize($userName, $tbkUser, $parentBuyOrder, $details);
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

```http
POST /rswebpaytransaction/api/oneclick/v1.0/transactions

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
$response->getAccountingDate();
$response->getBuyOrder();
$card_detail = response->getCardDetail();
$card_detail->getCardNumber();
$response->getTransactionDate();
$response->getVci();
$details = response->getDetails();
foreach($details as $detail){
    detail->getAmount();
    detail->getAuthorizationCode();
    detail->getBuyOrder();
    detail->getCommerceCode();
    detail->getInstallmentsNumber();
    detail->getPaymentTypeCode();
    detail->getResponseCode();
    detail->getStatus();
}
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
details [].response_code  <br> <i> Number </i> | Código del resultado del pago, donde: 0 (cero) es aprobado. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br> -96: `tbk_user` no existente <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la transacción de pago.
details [].commerce_code  <br> <i> Number </i> | Código de comercio del comercio hijo (tienda).
details [].buy_order  <br> <i> String </i> | Orden de compra generada por el comercio hijo para la transacción de pago.

<aside class="warning">
Cualquier valor distinto de número en `installmentsNumber` (incluyendo letras,
inexistencia del campo o nulo) será asumido como cero, es decir "Sin cuotas".
</aside>

### Consultar un pago realizado

Permite consultar el estado de pago realizado a través de Oneclick.
Retorna el resultado de la autorización.

Revisa la [documentación](/documentacion/oneclick#consultar-un-pago-realizado) de este método para mayor detalle.

```java
final OneclickMallTransactionStatusResponse response =
  OneclickMall.Transaction.status(buyOrder);
```

```php
use Transbank\Webpay\Oneclick;

$response = MallTransaction::getStatus($buyOrder);
```

```csharp
using Transbank.Webpay.Oneclick;

var result = MallTransaction.Status(buyOrder);
```

```ruby
response = Transbank::Webpay::Oneclick::MallTransaction::status(buy_order: buy_order)
```

```python
var response = MallTransaction.status(buy_order)
```

```http
GET /rswebpaytransaction/api/oneclick/v1.0/transactions/{buyOrder}

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
$response->getAccountingDate();
$response->getBuyOrder();
$card_detail = response->getCardDetail();
$card_detail->getCardNumber();
$response->getTransactionDate();
$response->getVci();
$details = response->getDetails();
foreach($details as $detail){
    detail->getAmount();
    detail->getAuthorizationCode();
    detail->getBuyOrder();
    detail->getCommerceCode();
    detail->getInstallmentsNumber();
    detail->getPaymentTypeCode();
    detail->getResponseCode();
    detail->getStatus();
}
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
transaction_date  <br> <i> DateTime </i> | Fecha completa (timestamp) de la autorización del pago.
details  <br> <i> Array </i> | Lista con el resultado de cada transacción de las tiendas hijas.
details [].amount  <br> <i> Decimal </i> | Monto de la sub-transacción de pago.
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED).
details [].authorization_code  <br> <i> String </i> | Código de autorización de la sub-transacción de pago.
details [].payment_type_code  <br> <i> String </i> | [Tipo](/producto/webpay#tipos-de-pago) de pago de la transaccion. <br> VD = Venta Débito.<br>VP = Venta prepago<br>VN = Venta Normal.<br>VC = Venta en cuotas.<br>SI = 3 cuotas sin interés.<br>S2 = 2 cuotas sin interés.<br>NC = N Cuotas sin interés<br>
details [].response_code  <br> <i> Number </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br> -96: `tbk_user` no existente <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la sub-transacción de pago.
details [].commerce_code  <br> <i> Number </i> | Código de comercio del comercio hijo (tienda).
details [].buy_order  <br> <i> String </i> | Orden de compra generada por el comercio hijo para la sub-transacción de pago.
status  <br> <i> Text </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
balance  <br> <i> Decimal </i> | Monto restante de la sub-transacción de pago original: monto inicial – monto anulado. Largo máximo: 17

### Reversar o Anular un pago Oneclick Mall

Revisa la [documentación](/documentacion/oneclick#anular-una-transaccion) de este método para mayor detalle.

```java
final OneclickMallTransactionRefundResponse response =
  OneclickMall.Transaction.refund(buyOrder, childCommerceCode, childBuyOrder, amount);
```

```php
use Transbank\Webpay\Oneclick;

$response = MallTransaction::refund($buyOrder, $childCommerceCode, $childBuyOrder, $amount);
```

```csharp
using Transbank.Webpay.Oneclick;

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

```http
POST /rswebpaytransaction/api/oneclick/v1.0/transactions/{buyOrder}/refunds

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

```java
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getBalance();
response.getNullifiedAmount();
response.getResponseCode();
response.getType();
```

```php
response->getAuthorizationCode;
response->getAuthorizationDate;
response->getBalance;
response->getNullifiedAmount;
response->getResponseCode;
response->getType;
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

```http
200 OK
Content-Type: application/json

{
  "type": "NULLIFY",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20:18:20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}

En caso de une reversa no devuelve más información
{
  "type": "REVERSED",
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSE o NULLIFY). Largo máximo: 10
authorization_code  <br> <i> Boolean </i> | Código de autorización. Largo máximo: 6
authorization_date  <br> <i> ISO8601 </i> | Fecha de la autorización de la transacción.
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17
balance  <br> <i> Decimal </i> | Monto restante de la transacción de pago original: monto inicial – monto anulado. Largo máximo: 17
response_code <br> <i> Number </i> | Código del resultado del pago, donde: 0 (cero) es aprobado. Largo máximo: 2
buy_order  <br> <i> String </i> | Orden de compra generada por el comercio hijo para la transacción de pago. Largo máximo: 26.

### Captura Diferida Oneclick Mall

Una transacción Oneclick Mall permite que el tarjetahabiente registre su
tarjeta de la misma forma en que ocurre con una transacción Oneclick, asociando
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

#### capture()

Este método permite a los comercios Oneclick Mall habilitados, poder
realizar capturas diferidas de una transacción previamente autorizada. El método
contempla una única captura por cada autorización. Para ello se deberá indicar los
datos asociados a la transacción de venta y el monto requerido para capturar, el cual
debe ser menor o igual al monto originalmente autorizado.
Para capturar una transacción, ésta debe haber sido creada por un código de
comercio configurado para captura diferida. De esta forma la transacción estará
autorizada pero requerirá una captura explícita posterior para confirmar la
transacción.

```java
// Esta funcion aun no se encuentra disponible en el SDK
```

```php
// Esta funcion aun no se encuentra disponible en el SDK
```

```csharp
// Esta funcion aun no se encuentra disponible en el SDK
```

```ruby
# Esta funcion aun no se encuentra disponible en el SDK
```

```python
# Esta funcion aun no se encuentra disponible en el SDK
```

```http
PUT /rswebpaytransaction/api/oneclick/mall/v1_0/transactions/capture
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
// Esta función aun no se encuentra disponible en el SDK
```

```php
// Esta función aun no se encuentra disponible en el SDK
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

## Captura Diferida

Los comercios que están configurados para operar con captura diferida deben ejecutar el método de captura para realizar el cargo el cargo al tarjetahabiente.

**Válido para :**

* Webpay Plus Captura Diferida
* Transacción Completa Captura Diferida

### Ejecutar captura diferida

Este método permite a todo comercio habilitado realizar capturas de una
transacción autorizada sin captura generada en Webpay Plus o Transacción Completa.
El método contempla una única captura por cada autorización. Para ello se
deberá indicar los datos asociados a la transacción de venta con autorización
sin captura y el monto requerido para capturar el cual debe ser menor o igual al
monto originalmente autorizado.

Para capturar una transacción, ésta debe haber sido creada (según lo visto
anteriormente para Webpay Plus o Webpay Plus Mall) por un código de
comercio configurado para captura diferida. De esa forma la transacción estará
autorizada pero requerirá una captura explícita posterior para confirmar la
transacción.

Puedes [leer más sobre la captura en la información del
producto Webpay](/producto/webpay#autorizacion-y-captura)
para conocer más detalles y restricciones.

Para realizar esa captura explícita debe usarse el método `Transaction.capture()`

#### Transaction.capture()

Permite solicitar a Webpay la captura diferida de una transacción con
autorización y sin captura simultánea.

> Los SDKs permiten indicar opcionalmente el código de comercio de la
> transacción a capturar, para soportar la captura en comercios Webpay Plus
> Mall o Transacción Completa Mall. En comercios sin modalidad Mall no es necesario especificar el código
> de comercio, ya que se usa el indicado en la configuración.

<aside class="notice">
El método `Transaction.capture()` debe ser invocado siempre indicando el código del
comercio que realizó la transacción. En el caso de comercios modalidad Mall,
el código debe ser el código de la tienda virtual específica.
</aside>

```java
final CaptureWebpayPlusTransactionResponse response = WebpayPlus.DeferredTransaction.capture(token, buyOrder, authorizationCode, amount);
```

```php
use Transbank\Webpay\WebpayPlus;

$response = Transaction::capture($token, $buyOrder, $authCode, $amount);
```

```csharp
var response = DeferredTransaction.Capture(token, buyOrder, authorizationCode, captureAmount);
```

```ruby
response = Transbank::Webpay::WebpayPlus::DeferredTransaction::capture(
  token: @token,
  buy_order: @buy_order,
  authorization_code: @auth_code,
  capture_amount: @amount
)
```

```python
response = MallDeferredTransaction.capture(
  token=token, capture_amount=amount, commerce_code=commerce_code,
  buy_order=buy_order, authorization_code=authorization_code
)
```

```http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/capture
Tbk-Api-Key-Id: 597055555531
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "commerce_code": "597055555531",
  "buy_order": "415034240",
  "authorization_code": "12345",
  "capture_amount": 1000
}
```

<strong>Parámetros Ejecutar captura diferida</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
commerce_code  <br> <i> Number </i> | (Opcional, solo usar en caso Mall) Tienda hija que realizó la transacción. Largo: 12.
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere capturar Largo máximo: 6.
capture_amount  <br> <i> Decimal </i> | Monto que se desea capturar. Largo máximo: 17.

<strong>Respuesta Ejecutar captura diferida</strong>

```java
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getCapturedAmount();
response.getResponseCode();
```

```php
response->getAuthorizationCode();
response->getAuthorizationDate();
response->getCapturedAmount();
response->getResponseCode();
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
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
```

```http
200 OK
Content-Type: application/json
{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20:18:20Z",
  "captured_amount": 1000,
  "response_code": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo máximo: 64
authorization_code  <br> <i> String </i> | Código de autorización de la captura diferida. Largo máximo: 6
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización.
captured_amount  <br> <i> Decimal </i> | Monto capturado. Largo máximo: 6
response_code  <br> <i> Number </i> | Código de resultado de la captura. Si es exitoso es 0,de lo contrario la captura no fue realizada. Largo máximo: 2

En caso de error pueden aparecer los siguientes códigos exclusivos del método
`Transaction.capture()`:

Código | Descripción
------ | -----------
304 | Validación de campos de entrada nulos
245 | Código de comercio no existe
22 | El comercio no se encuentra activo
316 | El comercio indicado no corresponde al certificado o no es hijo del comercio Mall en caso de transacciones MALL
308 | Operación no permitida
274 | Transacción no encontrada
16 | La transacción no es de captura diferida
292 | La transacción no está autorizada
284 | Periodo de captura excedido
310 | Transacción reversada previamente
309 | Transacción capturada previamente
311 | Monto a capturar excede el monto autorizado
315 | Error del autorizador

## Códigos y mensajes de error

Al realizar cualquier solicitud al API REST, además de los datos de respuesta, se incluirá uno de los siguientes códigos de estado de respuesta HTTP dependiendo del resultado obtenido:  

### Solicitud exitosa

Cuando la operación solcitada es ejecutada correctamente, se pueden recibir estos status HTTP:

Código de estado HTTP | Descripción
------ | -----------
200 | La operación se ha ejecutado exitosamente
204 | La operación DELETE se ha ejecutado exitosamente

#### Códigos de error

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
    OneclickMall.setCommerceCode('TU_CODIGO_DE_COMERCIO');
    ```

2. Configuración del secreto compartido.

    ```java
    // Para Oneclick
    OneclickMall.setApiKey('TU_API_KEY');
    ```

3. Selección del ambiente productivo.

```java
// Para Oneclick
OneclickMall.setIntegrationType(IntegrationType.LIVE);
```
