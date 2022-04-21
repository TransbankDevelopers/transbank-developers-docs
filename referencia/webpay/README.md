# Webpay Plus

___

<!-- <aside class="warning">
Estás viendo la <strong>nueva referencia REST</strong>. La referencia anterior (SOAP) tiene fin de soporte programado para enero 2022. Si necesitas revisarla haz [click aquí](/referencia/webpay-soap)
</aside> -->

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

Consulta [la documentación para conocer las tarjetas de prueba que funcionan en
el ambiente de integración](/documentacion/como_empezar#ambientes).

### Credenciales del Comercio

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

```java
// El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente

// Versión 3.x del SDK
WebpayPlus.Transaction tx = new WebpayPlus.Transaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));

// Versión 2.x del SDK
WebpayPlus.Transaction.setCommerceCode(597055555532);
WebpayPlus.Transaction.setApiKey('579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C');
WebpayPlus.Transaction.setIntegrationType(IntegrationType.TEST);
```

```php
// SDK Versión 2.x
// El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
\Transbank\Webpay\WebpayPlus::configureForIntegration('597055555532', '579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C');

// Si deseas apuntar a producción: 
\Transbank\Webpay\WebpayPlus::configureForProduction('tu-codigo-comercio', 'tu-api-key');
```

```csharp
// Versión 4.x del SDK
var tx = new Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));

// Versión 3.x del SDK
// El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
WebpayPlus.Transaction.CommerceCode = 597055555532;
WebpayPlus.Transaction.ApiKey = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C";
WebpayPlus.Transaction.IntegrationType = WebpayIntegrationType.Test;
```

```ruby
# El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente

## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::Transaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS, ::Transbank::Common::IntegrationApiKeys::WEBPAY, :integration)

## Versión 1.x del SDK
Transbank::Webpay::WebpayPlus::Base.commerce_code = 597055555532;
Transbank::Webpay::WebpayPlus::Base.api_key = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C";
Transbank::Webpay::WebpayPlus::Base.integration_type = "TEST";

```

```python
## Versión 3.x del SDK
tx = Transaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))

## Versión 2.x del SDK
# El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
transbank.webpay.webpay_plus.webpay_plus_default_commerce_code = 597055555532
transbank.webpay.webpay_plus.default_api_key = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C"
transbank.webpay.webpay_plus.default_integration_type = IntegrationType.TEST
```

```javascript
const Environment = require('transbank-sdk').Environment;

// Versión 3.x del SDK
const tx = new WebpayPlus.Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, Environment.Integration));

// Versión 2.x del SDK
WebpayPlus.commerceCode = 597055555532;
WebpayPlus.apiKey = '579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C';
WebpayPlus.environment = Environment.Integration;
```

```http
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
```

## Webpay Plus

Una transacción de autorización normal (o transacción normal), corresponde a
una solicitud de autorización financiera de un pago con tarjetas de crédito o
débito, en donde quién realiza el pago ingresa al sitio del comercio,
selecciona productos o servicio, y el ingreso asociado a los datos de la tarjeta
de crédito, débito o prepago lo realiza en forma segura en Webpay.

### Flujo en caso de éxito y abortar un pago

Revisa [la documentación](/documentacion/webpay-plus#flujo-en-caso-de-exito) de Webpay plus para revisar los diferentes flujos de pago posibles. 


### Crear una transacción
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#crear-una-transaccion)

Permite inicializar una transacción en Webpay. Como respuesta a la invocación
se genera un token que representa en forma única una transacción.

Para crear una transacción basta llamar al método `Transaction.create()`

```java
import cl.transbank.webpay.webpayplus.WebpayPlus;
import cl.transbank.webpay.webpayplus.model.WebpayPlusTransactionCreateResponse;

// Versión 3.x del SDK
WebpayPlus.Transaction tx = new WebpayPlus.Transaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusTransactionCreateResponse response = tx.create(buyOrder, sessionId, amount, returnUrl);

// Versión 2.x del SDK
final WebpayPlusTransactionCreateResponse response = WebpayPlus.Transaction.create(
  buyOrder, sessionId, amount, returnUrl
);
```

```php
// SDK Versión 2.x
use Transbank\Webpay\WebpayPlus\Transaction;

$response = (new Transaction)->create($buy_order, $session_id, $amount, $return_url);
```

```csharp
using Transbank.Webpay.WebpayPlus;

// Versión 4.x del SDK
var tx = new Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = tx.Create(buyOrder, sessionId, amount, returnUrl);

// Versión 3.x del SDK
var response = Transaction.Create(buyOrder, sessionId, amount, returnUrl);
```

```ruby
## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::Transaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS)
@resp = @tx.create(
  buy_order: buy_order,
  session_id: session_id,
  amount: amount,
  return_url: return_url
)

## Versión 1.x del SDK
@resp = Transbank::Webpay::WebpayPlus::Transaction::create(
  buy_order: buy_order,
  session_id: session_id,
  amount: amount,
  return_url: return_url
)
```

```python
## Versión 3.x del SDK
tx = Transaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))
resp = tx.create(buy_order, session_id, amount, return_url)

## Versión 2.x del SDK
resp = transbank.webpay.webpay_plus.create(buy_order, session_id, amount, return_url)
```

```javascript
const WebpayPlus = require('transbank-sdk').WebpayPlus;
// Versión 3.x del SDK
const tx = new WebpayPlus.Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, Environment.Integration));
const response = await tx.create(buyOrder, sessionId, amount, returnUrl);

// Versión 2.x del SDK
const response = await WebpayPlus.Transaction.create(buyOrder, sessionId, amount, returnUrl);
```

```http
POST /rswebpaytransaction/api/webpay/v1.2/transactions

Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
 "buy_order": "ordenCompra12345678",
 "session_id": "sesion1234557545",
 "amount": 10000,
 "return_url": "http://www.comercio.cl/webpay/retorno"
}
```

<strong>Parámetros Transaction.create</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la tienda. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>
session_id  <br> <i> String </i> | Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
amount  <br> <i> Decimal </i> | Monto de la transacción. Máximo 2 decimales para USD. Largo máximo: 17
return_url  <br> <i> String </i> | URL del comercio, a la cual Webpay redireccionará posterior al proceso de autorización. Largo máximo: 256

<strong>Respuesta Transaction.create</strong>

```java
response.getUrl();
response.getToken();
```

```php
// Puedes obtener la URL y el token devuelo utilizando estos métodos del objeto de respuesta:

$response->getUrl();
$response->getToken();
```

```csharp
response.Url;
response.Token;
```

```ruby
response.url
response.token
```

```python
## Versión 3.x del SDK
response['url']
response['token']

## Versión 2.x del SDK
response.url
response.token
```

```javascript
response.url
response.token
```

```http
200 OK
Content-Type: application/json

{
 "token": "e9d555262db0f989e49d724b4db0b0af367cc415cde41f500a776550fc5fddd3",
 "url": "https://webpay3gint.transbank.cl/webpayserver/initTransaction"
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.
url  <br> <i> String </i> | URL de formulario de pago Webpay. Largo máximo: 255.

### Confirmar una transacción
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#confirmar-una-transaccion)

Permite confirmar y obtener el resultado de la transacción una vez que Webpay ha resuelto su autorización financiera.

Cuando el comercio retoma el control mediante `return_url` debes confirmar y obtener
el resultado de una transacción usando el método  `Transaction.commit()`.

```java
// Versión 3.x del SDK
WebpayPlus.Transaction tx = new WebpayPlus.Transaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_DEFERRED, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusTransactionCommitResponse response = tx.commit(token);

// Versión 2.x del SDK
final WebpayPlusTransactionCommitResponse response = WebpayPlus.Transaction.commit(token);
```

```php
// SDK Versión 2.x
use Transbank\Webpay\WebpayPlus\Transaction;

$response = (new Transaction)->commit($token);
```

```csharp
using Transbank.Webpay.WebpayPlus;

// Versión 4.x del SDK
var tx = new Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = tx.Commit(token);

// Versión 3.x del SDK
var response = Transaction.Commit(token);
```

```ruby
## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::Transaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS)
@resp = @tx.commit(token: @token)

## Versión 1.x del SDK
@resp = Transbank::Webpay::WebpayPlus::Transaction::commit(token: @token)
```

```python
## Versión 3.x del SDK
tx = Transaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))
resp = tx.commit(token)

## Versión 2.x del SDK
resp = transbank.webpay.webpay_plus.transaction.commit(token)
```

```javascript
const WebpayPlus = require('transbank-sdk').WebpayPlus;
// Versión 3.x del SDK
const tx = new WebpayPlus.Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, Environment.Integration));
const response = await tx.commit(token);

// Versión 2.x del SDK
const response = await WebpayPlus.Transaction.commit(token);
```

```http
PUT /rswebpaytransaction/api/webpay/v1.2/transactions/{token}
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

<strong>Parámetros Transaction.commit</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

<strong>Respuesta Transaction.commit</strong>

```java
response.getVci();
response.getAmount();
response.getStatus();
response.getBuyOrder();
response.getSessionId();
response.getCardDetail().getCardNumber();
response.getAccountingDate();
response.getTransactionDate();
response.getAuthorizationCode();
response.getPaymentTypeCode();
response.getResponseCode();
response.getInstallmentsAmount();
response.getInstallmentsNumber();
response.getBalance();
```

```php
$response->getVci();
$response->getAmount();
$response->getStatus();
$response->getBuyOrder();
$response->getSessionId();
$response->getCardDetail();
$response->getAccountingDate();
$response->getTransactionDate();
$response->getAuthorizationCode();
$response->getPaymentTypeCode();
$response->getResponseCode();
$response->getInstallmentsAmount();
$response->getInstallmentsNumber();
$response->getBalance();
```

```csharp
response.Vci;
response.Amount;
response.Status;
response.BuyOrder;
response.SessionId;
response.CardDetail;
response.AccountingDate;
response.TransactionDate;
response.AuthorizationCode;
response.PaymentTypeCode;
response.ResponseCode;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.Balance;
```

```ruby
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
```

```python
## Versión 3.x del SDK
response['vci']
response['amount']
response['status']
response['buy_order']
response['session_id']
response['card_detail']
response['accounting_date']
response['transaction_date']
response['authorization_code']
response['payment_type_code']
response['response_code']
response['installments_amount']
response['installments_number']
response['balance']

## Versión 2.x del SDK
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
```

```javascript
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
```

```http
200 OK
Content-Type: application/json

{
  "vci": "TSY",
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
      "card_number": "6623"
  },
  "accounting_date": "0522",
  "transaction_date": "2019-05-22T16:41:21.063Z",
  "authorization_code": "1213",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Algunos de ellos son: <br>TSY - Autenticación Exitosa<br>TSN - Autenticación Rechazada<br>NP - No Participa, sin autenticación<br>U3 - Falla conexión, Autenticación Rechazada<br>INV - Datos Inválidos<br>A - Intentó<br>CNP1 - Comercio no participa<br>EOP - Error operacional<br>BNA - BIN no adherido<br>ENA - Emisor no adherido<br><br>Para venta extranjera, estos son algunos de los códigos:<br>TSYS (Autenticación exitosa Sin fricción. Resultado autenticación: Autenticación Existosa)<br>TSAS (Intento, tarjeta no enrolada / emisor no disponible. Resultado autenticación: Autenticación Exitosa)<br>TSNS (Fallido, no autenticado, denegado / no permite intentos. Resultado autenticación: Autenticación denegada)<br>TSRS (Autenticación rechazada - sin fricción. Resultado autenticación: Autenticación rechazada)<br>TSUS (Autenticación no se pudo realizar por problema técnico u otro motivo. Resultado autenticación: Autenticación fallida)<br>TSCF (Autenticación con fricción (No aceptada por el comercio). Resultado autenticación: Autenticación incompleta)<br>TSYF (Autenticación exitosa con fricción. Resultado autenticación: Autenticación exitosa)<br>TSNF (No autenticado. Transacción denegada con fricción. Resultado autenticación: Autenticación denegada)<br>TSUF (Autenticación con fricción no se pudo realizar por problema técnico u otro. Resultado autenticación: Autenticación fallida)<br>NPC (Comercio no Participa. Resultado autenticación: Comercio/BIN no participa)<br>NPB (BIN no participa. Resultado autenticación: Comercio/BIN no participa)<br>NPCB (Comercio y BIN no participan. Resultado autenticación: Comercio/BIN no participa)<br>SPCB (Comercio y BIN sí participan. Resultado autenticación: Autorización incompleta)<br>Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
amount  <br> <i> Decimal </i> | Formato número entero para transacciones en peso y decimal para transacciones en dólares. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
buy_order  <br> <i> String </i> | Orden de compra de la tienda indicado en `Transaction.create()`. Largo máximo: 26
session_id  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `Transaction.create()`. Largo máximo: 61.
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accounting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 24, formato: ISO 8601 (Ej: yyyy-mm-ddTHH:mm:ss.xxxZ)
authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
response_code  <br> <i> Number </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> Puedes revisar los códigos de respuesta de rechazo en el siguiente [link](/producto/webpay#codigos-de-respuesta-de-autorizacion)<br>
installments_amount <br> <i> Number </i> | Monto de las cuotas. Largo máximo: 17
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
balance  <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17


### Obtener estado de una transacción
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#obtener-estado-de-una-transaccion)

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable 
que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las 
acciones que correspondan.

<strong>Transaction.status()</strong>


```java
// Versión 3.x del SDK
WebpayPlus.Transaction tx = new WebpayPlus.Transaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_DEFERRED, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusTransactionStatusResponse response = tx.status(token);

// Versión 2.x del SDK
final WebpayPlusTransactionStatusResponse response = WebpayPlus.Transaction.status(token);
```

```php
// SDK Versión 2.x
use Transbank\Webpay\WebpayPlus\Transaction;

$response = (new Transaction)->status('token-de-la-transaccion');
```

```csharp
using Transbank.Webpay.WebpayPlus;

// Versión 4.x del SDK
var tx = new Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = tx.Status(token);

// Versión 3.x del SDK
var response = Transaction.Status(token);
```

```ruby
## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::Transaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS)
@resp = @tx.status(token: @token)

## Versión 1.x del SDK
@resp = Transbank::Webpay::WebpayPlus::Transaction::status(token: @token)
```

```python
## Versión 3.x del SDK
tx = Transaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))
resp = tx.status(token)

## Versión 2.x del SDK
resp = transbank.webpay.webpay_plus.transaction.status(token)
```

```javascript
const WebpayPlus = require('transbank-sdk').WebpayPlus;
// Versión 3.x del SDK
const tx = new WebpayPlus.Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, Environment.Integration));
const response = await tx.status(token);

// Versión 2.x del SDK
const response = await WebpayPlus.Transaction.status(token);
```

```http
GET /rswebpaytransaction/api/webpay/v1.2/transactions/{token}
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

<strong>Parámetros Transaction.status</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

<strong>Respuesta Transaction.status</strong>

```java
response.getVci();
response.getAmount();
response.getStatus();
response.getBuyOrder();
response.getSessionId();
response.getCardDetail().getCardNumber();
response.getAccountingDate();
response.getTransactionDate();
response.getAuthorizationCode();
response.getPaymentTypeCode();
response.getResponseCode();
response.getInstallmentsAmount();
response.getInstallmentsNumber();
response.getBalance();
```

```php
$response->getVci();
$response->getAmount();
$response->getStatus();
$response->getBuyOrder();
$response->getSessionId();
$response->getCardDetail();
$response->getAccountingDate();
$response->getTransactionDate();
$response->getAuthorizationCode();
$response->getPaymentTypeCode();
$response->getResponseCode();
$response->getInstallmentsAmount();
$response->getInstallmentsNumber();
$response->getBalance();
```

```csharp
response.Vci;
response.Amount;
response.Status;
response.BuyOrder;
response.SessionId;
response.CardDetail;
response.AccountingDate;
response.TransactionDate;
response.AuthorizationCode;
response.PaymentTypeCode;
response.ResponseCode;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.Balance;
```

```ruby
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
```

```python
## Versión 3.x del SDK
response['vci']
response['amount']
response['status']
response['buy_order']
response['session_id']
response['card_detail']
response['accounting_date']
response['transaction_date']
response['authorization_code']
response['payment_type_code']
response['response_code']
response['installments_amount']
response['installments_number']
response['balance']

## Versión 2.x del SDK
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
```

```javascript
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
```

```http
200 OK
Content-Type: application/json

{
  "vci": "TSY",
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
      "card_number": "6623"
  },
  "accounting_date": "0522",
  "transaction_date": "2019-05-22T16:41:21.063Z",
  "authorization_code": "1213",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Algunos de ellos son: <br>TSY - Autenticación Exitosa<br>TSN - Autenticación Rechazada<br>NP - No Participa, sin autenticación<br>U3 - Falla conexión, Autenticación Rechazada<br>INV - Datos Inválidos<br>A - Intentó<br>CNP1 - Comercio no participa<br>EOP - Error operacional<br>BNA - BIN no adherido<br>ENA - Emisor no adherido<br><br>Para venta extranjera, estos son algunos de los códigos:<br>TSYS (Autenticación exitosa Sin fricción. Resultado autenticación: Autenticación Existosa)<br>TSAS (Intento, tarjeta no enrolada / emisor no disponible. Resultado autenticación: Autenticación Exitosa)<br>TSNS (Fallido, no autenticado, denegado / no permite intentos. Resultado autenticación: Autenticación denegada)<br>TSRS (Autenticación rechazada - sin fricción. Resultado autenticación: Autenticación rechazada)<br>TSUS (Autenticación no se pudo realizar por problema técnico u otro motivo. Resultado autenticación: Autenticación fallida)<br>TSCF (Autenticación con fricción (No aceptada por el comercio). Resultado autenticación: Autenticación incompleta)<br>TSYF (Autenticación exitosa con fricción. Resultado autenticación: Autenticación exitosa)<br>TSNF (No autenticado. Transacción denegada con fricción. Resultado autenticación: Autenticación denegada)<br>TSUF (Autenticación con fricción no se pudo realizar por problema técnico u otro. Resultado autenticación: Autenticación fallida)<br>NPC (Comercio no Participa. Resultado autenticación: Comercio/BIN no participa)<br>NPB (BIN no participa. Resultado autenticación: Comercio/BIN no participa)<br>NPCB (Comercio y BIN no participan. Resultado autenticación: Comercio/BIN no participa)<br>SPCB (Comercio y BIN sí participan. Resultado autenticación: Autorización incompleta)<br>Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
buy_order  <br> <i> String </i> | Orden de compra de la tienda indicado en `Transaction.create()`. Largo máximo: 26
session_id  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `Transaction.create()`. Largo máximo: 61.
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accounting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 24, formato: ISO 8601 (Ej: yyyy-mm-ddTHH:mm:ss.xxxZ)
authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> Puedes revisar los códigos de respuesta de rechazo en el siguiente [link](/producto/webpay#codigos-de-respuesta-de-autorizacion)<br>
installments_amount <br> <i> Number </i> | Monto de las cuotas. Largo máximo: 17
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
balance  <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Reversar o Anular un pago
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#reversar-o-anular-una-transaccion)

Para anular una transacción se debe invocar al método `Transaction.refund()`.

<strong>Transaction.refund()</strong>


> Los SDKs permiten indicar opcionalmente el código de comercio de la
> transacción a anular, para soportar la anulación en comercios Webpay Plus
> Mall. En comercios Webpay Plus, no es necesario especificar el código
> de comercio pues se usa el indicado en la configuración.

<aside class="notice">
El método `Transaction.refund()` debe ser invocado siempre indicando el código del comercio que realizó la transacción. En el caso de comercios Webpay Plus Mall, el código debe ser el código de la tienda virtual específica.
</aside>

```java
// Versión 3.x del SDK
WebpayPlus.Transaction tx = new WebpayPlus.Transaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusTransactionRefundResponse response = tx.refund(token, amount);

// Versión 2.x del SDK
final WebpayPlusTransactionRefundResponse response = WebpayPlus.Transaction.refund(token, amount);
```

```php
// SDK Versión 2.x
use Transbank\Webpay\WebpayPlus\Transaction;

$amount = 1000; // Monto que se desea devolver
$response = (new Transaction)->refund('token-de-la-transaccion', $amount);
```

```csharp
using Transbank.Webpay.WebpayPlus;

// Versión 4.x del SDK
var tx = new Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = tx.Refund(token, refundAmount);

// Versión 3.x del SDK
var response = Transaction.Refund(token, refundAmount);
```

```ruby
## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::Transaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS)
@resp = @tx.refund(token: @token, amount: @amount)

## Versión 1.x del SDK
@resp = Transbank::Webpay::WebpayPlus::Transaction::refund(token: @token, amount: @amount)
```

```python
## Versión 3.x del SDK
tx = Transaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))
resp = tx.refund(token, amount)

## Versión 2.x del SDK
response = Transbank.webpay.webpay_plus.refund(token, amount)
```

```javascript
const WebpayPlus = require('transbank-sdk').WebpayPlus;
// Versión 3.x del SDK
const tx = new WebpayPlus.Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS, IntegrationApiKeys.WEBPAY, Environment.Integration));
const response = await tx.refund(token, amount);

// Versión 2.x del SDK
const response = await WebpayPlus.Transaction.refund(token, amount);
```

```http
POST /rswebpaytransaction/api/webpay/v1.2/transactions/{token}/refunds
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "amount": 1000
}
```

<strong>Parámetros Transaction.refund</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
amount  <br> <i> Formato número entero para transacciones en peso. Sólo en caso de dólar acepta dos decimales. </i> | Monto que se desea anular o reversar de la transacción. Largo máximo: 17.

<strong>Respuesta Transaction.refund</strong>

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
// Puedes obtener los datos de la respuesta usando cualquiera de estos métedos del objeto de respuesta: 

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
response.authorization_code;
response.authorization_date;
response.balance;
response.nullified_amount;
response.response_code;
response.type;
```

```python
## Versión 3.x del SDK
response['authorization_code']
response['authorization_date']
response['balance']
response['nullified_amount']
response['response_code']
response['type']

## Versión 2.x del SDK
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
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSED o NULLIFIED).  Si es REVERSED no se devolverán datos de la transacción (authorization code, etc). Largo máximo: 10
authorization_code  <br> <i> String </i> | (Solo si es NULLIFIED) Código de autorización de la anulación. Largo máximo: 6
authorization_date  <br> <i> String </i> | (Solo si es NULLIFIED) Fecha y hora de la autorización.
balance  <br> <i> Decimal </i> | (Solo si es NULLIFIED) Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17
nullified_amount  <br> <i> Decimal </i> | (Solo si es NULLIFIED) Monto anulado. Largo máximo: 17
response_code <br> <i> Number </i> | (Solo si es NULLIFIED) Código de resultado de la reversa/anulacion. Si es exitoso es 0, de lo contrario la reversa/anulación no fue realizada Largo Máximo: 2

En caso de error pueden aparecer los siguientes códigos de error comunes para el método `Transaction.refund()`:

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

### Capturar una transacción
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#capturar-una-transaccion)

Permite solicitar a Webpay la captura diferida de una transacción con
autorización y sin captura simultánea.

<strong>Transaction.capture()</strong>

```java
// Versión 3.x del SDK
WebpayPlus.Transaction tx = new WebpayPlus.Transaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_DEFERRED, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusTransactionCaptureResponse response = tx.capture(token, buyOrder, authorizationCode, amount);

// Versión 2.x del SDK
final WebpayPlusTransactionCaptureResponse response = WebpayPlus.DeferredTransaction.capture(token, buyOrder, authorizationCode, amount);
```

```php
// SDK Versión 2.x
use Transbank\Webpay\WebpayPlus\Transaction;

$response = (new Transaction)->capture($token, $buyOrder, $authCode, $amount);
```

```csharp
// Versión 4.x del SDK
var tx = new Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS_DEFERRED, IntegrationApiKeys.WEBPAY_DEFERRED, WebpayIntegrationType.Test));
var response = tx.Capture(token, buyOrder, authorizationCode, captureAmount);

// Versión 3.x del SDK
var response = DeferredTransaction.Capture(token, buyOrder, authorizationCode, captureAmount);
```

```ruby
## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::Transaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS_DEFERRED)
@resp = @tx.capture(
  token: @token,
  buy_order: @buy_order,
  authorization_code: @auth_code,
  amount: @amount
)

## Versión 1.x del SDK
@resp = Transbank::Webpay::WebpayPlus::DeferredTransaction::capture(
  token: @token,
  buy_order: @buy_order,
  authorization_code: @auth_code,
  capture_amount: @amount
)
```

```python
## Versión 3.x del SDK
tx = Transaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_DEFERRED, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))
resp = tx.capture(
  token=token, buy_order=buy_order, authorization_code=authorization_code, capture_amount=amount
)

## Versión 2.x del SDK
resp = DeferredTransaction.capture(
  token=token, buy_order=buy_order, authorization_code=authorization_code, capture_amount=amount
)
```

```javascript

// Versión 3.x del SDK
const tx = new WebpayPlus.Transaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS_DEFERRED, IntegrationApiKeys.WEBPAY, Environment.Integration));
const response = await tx.capture(token, buyOrder, authorizationCode, captureAmount);

// Versión 2.x del SDK
const response  = await WebpayPlus.DeferredTransaction.capture(token, buyOrder, authorizationCode, captureAmount);
```

```http
PUT /rswebpaytransaction/api/webpay/v1.2/transactions/{token}/capture
Tbk-Api-Key-Id: 597055555540
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "buy_order": "415034240",
  "authorization_code": "12345",
  "capture_amount": 1000
}
```

<strong>Parámetros Transaction.capture</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere capturar Largo máximo: 6.
capture_amount  <br> <i> Decimal </i> | Monto que se desea capturar. Largo máximo: 17.

<strong>Respuesta Transaction.capture</strong>

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
## Versión 3.x del SDK
response['authorization_code']
response['authorization_date']
response['captured_amount']
response['response_code']

## Versión 2.x del SDK
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
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

## Webpay Plus Mall

Una transacción Mall Normal corresponde a una solicitud de autorización
financiera de un conjunto de pagos con tarjetas de crédito o débito, en donde
quién realiza el pago ingresa al sitio del comercio, selecciona productos o
servicios, y el ingreso asociado a los datos de la tarjeta de crédito o débito
lo realiza una única vez en forma segura en Webpay para el conjunto de pagos.
Cada pago tendrá su propio resultado, autorizado o rechazado.

Revisa más detalles sobre esta modalidad en [la documentación](/documentacion/webpay-plus#webpay-plus-mall)

### Crear una transacción mall
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#crear-una-transaccion-mall)

Para crear una transacción basta llamar al método `Transaction.create()`

<strong>Transaction.create() Mall</strong>

```java
MallTransactionCreateDetails mallDetails = MallTransactionCreateDetails.build()
    .add(amountMallOne, commerceCodeMallOne, buyOrderMallOne)
    .add(amountMallTwo, commerceCodeMallTwo, buyOrderMallTwo);

// Versión 3.x del SDK
WebpayPlus.MallTransaction tx = new WebpayPlus.MallTransaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusMallTransactionCreateResponse response = tx.create(buyOrder, sessionId, returnUrl, mallDetails);

// Versión 2.x del SDK
final WebpayPlusMallTransactionCreateResponse response = WebpayPlus.MallTransaction.create(buyOrder, sessionId, returnUrl, mallDetails);
```

```php
use Transbank\Webpay\WebpayPlus\MallTransaction;
// SDK 2.x
// Por defecto, la clase MallTransaction viene configurada para integración con un código Webpay Plus Mall Captura simultanea
// También se puede configurar \Transbank\Webpay\WebpayPlus::configureForTestingMall() y  \Transbank\Webpay\WebpayPlus::configureForTestingMallDeferred()
$transaction_details = [
  [
      "amount" => 10000,
      "commerce_code" => 597055555536,
      "buy_order" => "ordenCompraDetalle1234"
  ],
  [
     "amount" =>12000,
     "commerce_code" => 597055555537,
     "buy_order" => "ordenCompraDetalle4321"
  ],
];
  
$response = (new MallTransaction)->create($buy_order, $session_id, $return_url, $transaction_details);

```

```csharp
using Transbank.Webpay.WebpayPlus;

var transactionDetails = new List<TransactionDetail>();
transactions.Add(new TransactionDetail(
    amountMallOne,
    commerceCodeMallOne,
    buyOrderMallOne
));
transactions.Add(new TransactionDetail(
    amountMallTwo,
    comerceCodeMallTwo,
    buyOrderMallTwo
));

// Versión 4.x del SDK
var tx = new MallTransaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var result = tx.Create(buyOrder, sessionId, returnUrl, transactions);

// Versión 3.x del SDK
var result = MallTransaction.Create(buyOrder, sessionId, returnUrl, transactionDetails);
```

```ruby
transaction_details = [
  {
      amount: 10000,
      commerce_code: 597055555536,
      buy_order: "ordenCompraDetalle1234"
  },
  {
     amount: 12000,
     commerce_code: 597055555537,
     buy_order: "ordenCompraDetalle4321"
  },
]

## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::MallTransaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS_MALL)
@resp = @tx.create(
  buy_order: buy_order,
  session_id: session_id,
  return_url: return_url,
  details: transaction_details
)

## Versión 1.x del SDK
response = Transbank::Webpay::WebpayPlus::MallTransaction::create(
  buy_order: buy_order,
  session_id: session_id,
  return_url: return_url,
  details: transaction_details
)
```

```python
transaction_details = MallTransactionCreateDetails(
  amount_child_1, commerce_code_child_1, buy_order_child_1
).add(
  amount_child_2, commerce_code_child_2, buy_order_child_2
)

## Versión 3.x del SDK
tx = MallTransaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))
resp = tx.create(
    buy_order=buy_order,
    session_id=session_id,
    return_url=return_url,
    details = transaction_details,
)

## Versión 2.x del SDK
response = MallTransaction.create(
    buy_order=buy_order,
    session_id=session_id,
    return_url=return_url,
    details = transaction_details,
)
```

```javascript
const WebpayPlus = require('transbank-sdk').WebpayPlus;
const TransactionDetail = require('transbank-sdk').TransactionDetail;

let details = [
  new TransactionDetail(
    amount, commerceCode, buyOrder
  ),
  new TransactionDetail(
    amount2, commerceCode2, buyOrder2
  ),
];

// Versión 3.x del SDK
const tx = new WebpayPlus.MallTransaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, Environment.Integration));
const response = await tx.create(
  buyOrder,
  sessionId,
  returnUrl,
  details
);

// Versión 2.x del SDK
const createResponse = await WebpayPlus.MallTransaction.create(
  buyOrder,
  sessionId,
  returnUrl,
  details
);
```

```http
POST /rswebpaytransaction/api/webpay/v1.2/transactions

Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
 "buy_order": "ordenCompra12345678",
 "session_id": "sesion1234557545",
 "return_url": "http://www.comercio.cl/webpay/retorno",
 "details": [
     {
         "amount": 10000,
         "commerce_code": 597055555536,
         "buy_order": "ordenCompraDetalle1234"
     },
     {
        "amount": 12000,
        "commerce_code":  597055555537,
        "buy_order": "ordenCompraDetalle4321"
     },
 ]
}
```

<strong>Parámetros Transaction.create Mall</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Es el código único de la orden de compra generada por el comercio mall.
session_id  <br> <i> String </i> |  Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
return_url  <br> <i> String </i> | URL del comercio, a la cual Webpay redireccionará posterior al proceso de autorización Largo máximo: 256.
details  <br> <i> Array </i> | Lista de objetos, uno por cada tienda diferente del mall que participa en la transacción.
details [].amount  <br> <i> Decimal </i> | Monto de la transacción de una tienda del mall. Máximo 2 decimales para USD. Largo máximo: 10.
details [].commerce_code  <br> <i>String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo: 12.
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda del mall. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>.

<strong>Respuesta Transaction.create() Mall</strong>

```java
response.getToken();
response.getUrl();
```

```php
$response->getToken();
$response->getUrl();
```

```csharp
response.Token;
response.Url;
```

```ruby
response.token
response.url
```

```python
## Versión 3.x del SDK
response['token']
response['url']

## Versión 2.x del SDK
response.token
response.url
```

```javascript
response.token
response.url
```

```http
200 OK
Content-Type: application/json

{
 "token": "e9d555262db0f989e49d724b4db0b0af367cc415cde41f500a776550fc5fddd3",
 "url": "https://webpay3gint.transbank.cl/webpayserver/initTransaction"
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.
url  <br> <i> String </i> | URL de formulario de pago Webpay. Largo máximo: 256.

### Confirmar una transacción mall
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#confirmar-una-transaccion-mall)


Permite confirmar una tranascción y obtener el resultado de la transacción
una vez que Webpay ha resueltosu autorización financiera.


<strong>Transaction.commit() Mall</strong>

```java
// Versión 3.x del SDK
WebpayPlus.MallTransaction tx = new WebpayPlus.MallTransaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusMallTransactionCommitResponse response = tx.commit(token);

// Versión 2.x del SDK
final WebpayPlusMallTransactionCommitResponse response = WebpayPlus.MallTransaction.commit(token);
```

```php
// SDK 2.x
use Transbank\Webpay\WebpayPlus\MallTransaction;
$response = (new MallTransaction)->commit($token);
```

```csharp
using Transbank.Webpay.WebpayPlus;
// Versión 4.x del SDK
var tx = new MallTransaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = tx.Commit(token);

// Versión 3.x del SDK
var response = MallTransaction.commit(token);
```

```ruby
## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::MallTransaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS_MALL)
@resp = @tx.commit(token: @token)

## Versión 1.x del SDK
@resp = Transbank::Webpay::WebpayPlus::MallTransaction::commit(token: @token)
```

```python
## Versión 3.x del SDK
tx = MallTransaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))
resp = tx.commit(token)

## Versión 2.x del SDK
response = MallTransaction.commit(token)
```

```javascript
const WebpayPlus = require('transbank-sdk').WebpayPlus;

// Versión 3.x del SDK
const tx = new WebpayPlus.MallTransaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, Environment.Integration));
const response = await tx.commit(token);

// Versión 2.x del SDK
const response = await WebpayPlus.MallTransaction.commit(token);
```

```http
PUT /rswebpaytransaction/api/webpay/v1.2/transactions/{token}
Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

<strong>Parámetros Transaction.commit Mall</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

<strong>Respuesta Transaction.commit Mall</strong>

```java
response.getAccountingDate();
response.getBuyOrder();
final CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
response.getSessionId();
response.getTransactionDate();
response.getVci();
final List<Detail> details = response.getDetails();
for (Detail detail : details) {
    detail.getAmount();
    detail.getAuthorizationCode();
    detail.getBuyOrder();
    detail.getCommerceCode();
    detail.getInstallmentsNumber();
    detail.getPaymentTypeCode();
    detail.getResponseCode();
    detail.getStatus();
}
```

```php
$response->getAccountingDate();
$response->getBuyOrder();
$response->getCardDetail();
$response->getCardNumber(); // Solo en SDK 2.x
$response->getSessionId();
$response->getTransactionDate();
$response->getVci();
$details = $response->getDetails();
foreach($details as $detail){ // En SDk 2.x cada $detail es un Objeto TransactionDetails
    $detail->getAmount();
    $detail->getAuthorizationCode();
    $detail->getBuyOrder();
    $detail->getCommerceCode();
    $detail->getInstallmentsNumber();
    $detail->getPaymentTypeCode();
    $detail->getResponseCode();
    $detail->getStatus();
    $detail->isApproved(); // Solo en SDK 2.x - Indica si esta sub transacción puede ser considerada como aprobada

}
$response->isApproved(); // Solo en SDK 2.x - Devuelve true si al menos una de las subtransacciones fue autorizada. 
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
## Versión 3.x del SDK
response['accounting_date']
response['buy_order']
response['card_detail']
response['session_id']
response['transaction_date']
response['vci']

## Versión 2.x del SDK
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
details.forEach(detail => {
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
});
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
      "amount": 10000,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555536",
      "buy_order": "505479072",
      "status": "AUTHORIZED"
  }]
}

```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra del mall. Largo máximo: 26
session_id  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `Transaction.create()`. Largo máximo: 61.
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente.Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 16.
accouting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 24, formato: ISO 8601 (Ej: yyyy-mm-ddTHH:mm:ss.xxxZ)
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Algunos de ellos son: <br>TSY - Autenticación Exitosa<br>TSN - Autenticación Rechazada<br>NP - No Participa, sin autenticación<br>U3 - Falla conexión, Autenticación Rechazada<br>INV - Datos Inválidos<br>A - Intentó<br>CNP1 - Comercio no participa<br>EOP - Error operacional<br>BNA - BIN no adherido<br>ENA - Emisor no adherido<br><br>Para venta extranjera, estos son algunos de los códigos:<br>TSYS (Autenticación exitosa Sin fricción. Resultado autenticación: Autenticación Existosa)<br>TSAS (Intento, tarjeta no enrolada / emisor no disponible. Resultado autenticación: Autenticación Exitosa)<br>TSNS (Fallido, no autenticado, denegado / no permite intentos. Resultado autenticación: Autenticación denegada)<br>TSRS (Autenticación rechazada - sin fricción. Resultado autenticación: Autenticación rechazada)<br>TSUS (Autenticación no se pudo realizar por problema técnico u otro motivo. Resultado autenticación: Autenticación fallida)<br>TSCF (Autenticación con fricción (No aceptada por el comercio). Resultado autenticación: Autenticación incompleta)<br>TSYF (Autenticación exitosa con fricción. Resultado autenticación: Autenticación exitosa)<br>TSNF (No autenticado. Transacción denegada con fricción. Resultado autenticación: Autenticación denegada)<br>TSUF (Autenticación con fricción no se pudo realizar por problema técnico u otro. Resultado autenticación: Autenticación fallida)<br>NPC (Comercio no Participa. Resultado autenticación: Comercio/BIN no participa)<br>NPB (BIN no participa. Resultado autenticación: Comercio/BIN no participa)<br>NPCB (Comercio y BIN no participan. Resultado autenticación: Comercio/BIN no participa)<br>SPCB (Comercio y BIN sí participan. Resultado autenticación: Autorización incompleta)<br>Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
details  <br> <i> Array </i> | Lista con resultado de cada una de las transacciones enviados en `Transaction.create()`.
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
details [].payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
details [].response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> Puedes revisar los códigos de respuesta de rechazo en el siguiente [link](/producto/webpay#codigos-de-respuesta-de-autorizacion)<br>
details [].amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 10
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 12
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 26
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 26
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Obtener estado de una transacción mall
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#obtener-estado-de-una-transaccion-mall)

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

<strong>Transaction.status() Mall</strong>

Obtiene resultado de transacción a partir de un token.

```java
// Versión 3.x del SDK
WebpayPlus.MallTransaction tx = new WebpayPlus.MallTransaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusMallTransactionStatusResponse response = tx.status(token);

// Versión 2.x del SDK
final WebpayPlusMallTransactionStatusResponse response = WebpayPlus.MallTransaction.status(token);
```

```php
// SDK 2.x
use Transbank\Webpay\WebpayPlus\MallTransaction;
$response = (new MallTransaction)->status($token);

```

```csharp
using Transbank.Webpay.WebpayPlus;
// Versión 4.x del SDK
var tx = new MallTransaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = tx.Status(token);

// Versión 3.x del SDK
var response = MallTransaction.status(token)
```

```ruby
## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::MallTransaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS_MALL)
@resp = @tx.status(token: @token)

## Versión 1.x del SDK
@resp = Transbank::Webpay::WebpayPlus::MallTransaction.status(token: @token)
```

```python
## Versión 3.x del SDK
tx = MallTransaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))
resp = tx.status(token)

## Versión 2.x del SDK
response = MallTransaction.status(token)
```

```javascript
const WebpayPlus = require('transbank-sdk').WebpayPlus;
// Versión 3.x del SDK
const tx = new WebpayPlus.MallTransaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, Environment.Integration));
const response = await tx.status(token);

// Versión 2.x del SDK
const response = await WebpayPlus.MallTransaction.status(token);
```

```http
GET /rswebpaytransaction/api/webpay/v1.2/transactions/{token}
Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

<strong>Parámetros Transaction.status Mall</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

<strong>Respuesta Transaction.status Mall</strong>

```java
response.getAccountingDate();
response.getBuyOrder();
final CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
response.getSessionId();
response.getTransactionDate();
response.getVci();
final List<Detail> details = response.getDetails();
for (Detail detail : details) {
    detail.getAmount();
    detail.getAuthorizationCode();
    detail.getBuyOrder();
    detail.getCommerceCode();
    detail.getInstallmentsNumber();
    detail.getPaymentTypeCode();
    detail.getResponseCode();
    detail.getStatus();
}
```

```php
$response->getAccountingDate();
$response->getBuyOrder();
$response->getCardDetail();
$response->getCardNumber(); // Solo en SDK 2.x
$response->getSessionId();
$response->getTransactionDate();
$response->getVci();
$details = $response->getDetails();
foreach($details as $detail){ // En SDk 2.x cada $detail es un Objeto TransactionDetails
    $detail->getAmount();
    $detail->getAuthorizationCode();
    $detail->getBuyOrder();
    $detail->getCommerceCode();
    $detail->getInstallmentsNumber();
    $detail->getPaymentTypeCode();
    $detail->getResponseCode();
    $detail->getStatus();
    $detail->isApproved(); // Solo en SDK 2.x - Indica si esta sub transacción puede ser considerada como aprobada

}
$response->isApproved(); // Solo en SDK 2.x - Devuelve true si al menos una de las subtransacciones fue autorizada.
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
## Versión 3.x del SDK
response['accounting_date']
response['buy_order']
response['card_detail']
response['session_id']
response['transaction_date']
response['vci']

## Versión 2.x del SDK
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
details.forEach(detail => {
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
});
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
  "vci": "TSY",
  "details": [
    {
      "amount": 10000,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555536",
      "buy_order": "505479072",
      "status": "AUTHORIZED"
  }]
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra del mall. Largo máximo: 26
session_id  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `Transaction.create()`. Largo máximo: 61.
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente.Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 16.
accouting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 24, formato: ISO 8601 (Ej: yyyy-mm-ddTHH:mm:ss.xxxZ)
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Algunos de ellos son: <br>TSY - Autenticación Exitosa<br>TSN - Autenticación Rechazada<br>NP - No Participa, sin autenticación<br>U3 - Falla conexión, Autenticación Rechazada<br>INV - Datos Inválidos<br>A - Intentó<br>CNP1 - Comercio no participa<br>EOP - Error operacional<br>BNA - BIN no adherido<br>ENA - Emisor no adherido<br><br>Para venta extranjera, estos son algunos de los códigos:<br>TSYS (Autenticación exitosa Sin fricción. Resultado autenticación: Autenticación Existosa)<br>TSAS (Intento, tarjeta no enrolada / emisor no disponible. Resultado autenticación: Autenticación Exitosa)<br>TSNS (Fallido, no autenticado, denegado / no permite intentos. Resultado autenticación: Autenticación denegada)<br>TSRS (Autenticación rechazada - sin fricción. Resultado autenticación: Autenticación rechazada)<br>TSUS (Autenticación no se pudo realizar por problema técnico u otro motivo. Resultado autenticación: Autenticación fallida)<br>TSCF (Autenticación con fricción (No aceptada por el comercio). Resultado autenticación: Autenticación incompleta)<br>TSYF (Autenticación exitosa con fricción. Resultado autenticación: Autenticación exitosa)<br>TSNF (No autenticado. Transacción denegada con fricción. Resultado autenticación: Autenticación denegada)<br>TSUF (Autenticación con fricción no se pudo realizar por problema técnico u otro. Resultado autenticación: Autenticación fallida)<br>NPC (Comercio no Participa. Resultado autenticación: Comercio/BIN no participa)<br>NPB (BIN no participa. Resultado autenticación: Comercio/BIN no participa)<br>NPCB (Comercio y BIN no participan. Resultado autenticación: Comercio/BIN no participa)<br>SPCB (Comercio y BIN sí participan. Resultado autenticación: Autorización incompleta)<br>Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
details  <br> <i> Array </i> | Lista con resultado de cada una de las transacciones enviados en `Transaction.create()`.
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
details [].payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
details [].response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> Puedes revisar los códigos de respuesta de rechazo en el siguiente [link](/producto/webpay#codigos-de-respuesta-de-autorizacion)<br>
details [].amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 10
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 12
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 26
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 26
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Reversar o anular una transaccion mall
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#reversar-o-anular-una-transaccion-mall)

Este método permite a todo comercio habilitado reversar o anular una transacción
que fue generada en Webpay Plus Mall. 

Para anular una transacción se debe invocar al método `Transaction.refund()`.

<strong>Transaction.refund() Mall</strong>

```java
// Versión 3.x del SDK
WebpayPlus.MallTransaction tx = new WebpayPlus.MallTransaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusTransactionRefundResponse response = tx.refund(token, childBuyOrder, childCommerceCode, amount);

// Versión 2.x del SDK
final WebpayPlusTransactionRefundResponse response = WebpayPlus.MallTransaction.refund(token, childBuyOrder, childCommerceCode, amount);
```

```php
//SDK 2.x
$response = (new \Transbank\Webpay\WebpayPlus\MallTransaction)->refund($token, $buy_order, $commerce_code, $amount);

```

```csharp
// Versión 4.x del SDK
var tx = new MallTransaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = tx.Refund(token, buyOrder, commerceCode, amount);

// Versión 3.x del SDK
var response = Transaction.refund(token, buyOrder, commerceCode, amount);
```

```ruby
## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::MallTransaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS_MALL)
@resp = @tx.refund(token: @token, buy_order: @child_buy_order, child_commerce_code: @child_commerce_code,  amount: @amount)

## Versión 1.x del SDK
response = Transbank::Webpay::WebpayPlus::MallTransaction:refund(token: @token, buy_order: @child_buy_order, commerce_code: @child_commerce_code, amount: @amount)
```

```python
## Versión 3.x del SDK
tx = MallTransaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))
resp = tx.refund(token, child_buy_order, child_commerce_code, amount)

## Versión 2.x del SDK
response = Transaction.refund(token, buy_order, commerce_code, amount)
```

```javascript
const WebpayPlus = require('transbank-sdk').WebpayPlus;
// Versión 3.x del SDK
WebpayPlus.MallTransaction tx = new WebpayPlus.MallTransaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusTransactionRefundResponse response = tx.refund(token, childBuyOrder, childCommerceCode, amount);

// Versión 2.x del SDK
const response = await WebpayPlus.MallTransaction.refund(token, buyOrder, commerceCode, amount);
```

```http
POST /rswebpaytransaction/api/webpay/v1.2/transactions/{token}/refunds
Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "commerce_code": "597055555536",
  "buy_order": "ordenCompra12345678",
  "amount": 1000
}
```

<strong>Parámetros Transaction.refund Mall</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere anular. Largo máximo: 26.
amount  <br> <i> Formato número entero para transacciones en peso. Sólo en caso de dólar acepta dos decimales. </i> | Monto que se desea anular o reversar de la transacción. Largo máximo: 17.
commerce_code  <br> <i> Number </i> | Código de comercio de la tienda mall que realizó la transacción. Largo: 12.

<strong>Respuesta Transaction.refund Mall</strong>

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
## Versión 3.x del SDK
response['authorization_code']
response['authorization_date']
response['balance']
response['nullified_amount']
response['response_code']
response['type']

## Versión 2.x del SDK
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
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSED o NULLIFIED).  Si es REVERSED no se devolverán datos de la transacción (authorization code, etc). Largo máximo: 10
authorization_code  <br> <i> String </i> | (Solo si es NULLIFIED) Código de autorización de la anulación. Largo máximo: 6
authorization_date  <br> <i> String </i> | (Solo si es NULLIFIED) Fecha y hora de la autorización.
balance  <br> <i> Decimal </i> | (Solo si es NULLIFIED) Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17
nullified_amount  <br> <i> Decimal </i> | (Solo si es NULLIFIED) Monto anulado. Largo máximo: 17
response_code <br> <i> Number </i> | (Solo si es NULLIFIED) Código de resultado de la reversa/anulación. Si es exitoso es 0, de lo contrario la reversa/anulación no fue realizada. Largo máximo: 2

En caso de error pueden aparecer los siguientes códigos de error comunes para el método `Transaction.refund()`:

Código  | Descripción
------  | -----------
304     | Validación de campos de entrada nulos
245     | Código de comercio no existe
22      | El comercio no se encuentra activo
316     | El comercio indicado no corresponde al certificado o no es hijo del comercio MALL en caso de transacciones MALL
308     | Operación no permitida
274     | Transacción no encontrada
16      | La transacción no permite anulación
292     | La transacción no está autorizada
284     | Periodo de anulación excedido
310     | Transacción anulada previamente
311     | Monto a anular excede el saldo disponible para anular
312     | Error genérico para anulaciones
315     | Error del autorizador
53      | La transacción no permite anulación parcial de transacciones con cuotas

### Capturar una transacción mall
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#capturar-una-transaccion-mall)

Permite solicitar a Webpay la captura diferida de una transacción con
autorización y sin captura simultánea.

<strong>Transaction.capture()</strong>

```java
// Versión 3.x del SDK
WebpayPlus.MallTransaction tx = new WebpayPlus.MallTransaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL_DEFERRED, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final WebpayPlusMallTransactionCaptureResponse response = tx.capture(token, childCommerceCode, childBuyOrder, authorizationCode, amount);

// Versión 2.x del SDK
final WebpayPlusMallTransactionCaptureResponse response = WebpayPlus.MallDeferredTransaction.capture(token, childCommerceCode, childBuyOrder, authorizationCode, amount);
```

```php
//SDK 2.x
use Transbank\Webpay\WebpayPlus\MallTransaction;
$response = (new MallTransaction)->capture($childCommerceCode, $token, $buyOrder, $authorizationCode, $captureAmount);

```

```csharp
// Versión 4.x del SDK
var tx = new MallTransaction(new Options(IntegrationCommerceCodes.WEBPAY_PLUS_MALL_DEFERRED, IntegrationApiKeys.WEBPAY, WebpayIntegrationType.Test));
var response = tx.Capture(token, childCommerceCode, buyOrder, authorizationCode, captureAmount);

// Versión 3.x del SDK
var response = Transbank.Webpay.WebpayPlus.MallDeferredTransaction.Capture(token, childCommerceCode, buyOrder, authorizationCode, captureAmount);
```

```ruby
## Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlus::MallTransaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS_MALL_DEFERRED)
@resp = @tx.capture(child_commerce_code: @child_commerce_code, token: @token, buy_order: @buy_order, authorization_code: @authorization_code, capture_amount: @capture_amount)

## Versión 1.x del SDK
response = Transbank::Webpay::WebpayPlus::MallDeferredTransaction::capture(
  token: @token,
  child_commerce_code: @child_commerce_code,
  buy_order: @buy_order,
  authorization_code: @authorization_code,
  capture_amount: @capture_amount
)
```

```python
## Versión 3.x del SDK
tx = MallTransaction(WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MALL_DEFERRED, IntegrationApiKeys.WEBPAY, IntegrationType.TEST))
resp = tx.capture(
  token=token, capture_amount=amount, commerce_code=child_commerce_code,
  buy_order=buy_order, authorization_code=authorization_code
)

## Versión 2.x del SDK
response = MallDeferredTransaction.capture(
  token=token, capture_amount=amount, commerce_code=commerce_code,
  buy_order=buy_order, authorization_code=authorization_code
)
```

```http
PUT /rswebpaytransaction/api/webpay/v1.2/transactions/{token}/capture
Tbk-Api-Key-Id: 597055555581
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "commerce_code": "597055555582",
  "buy_order": "your_buy_order_here",
  "authorization_code": "your_authorization_code_here",
  "capture_amount": 1000
}
```

<strong>Parámetros Transaction.capture</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
commerce_code  <br> <i> Number </i> | Tienda **hija** (no usar el código de comercio mall) que realizó la transacción. Largo: 12.
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere capturar Largo máximo: 6.
capture_amount  <br> <i> Decimal </i> | Monto que se desea capturar. Largo máximo: 17.

<strong>Respuesta Transaction.capture</strong>
```java
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getCapturedAmount();
response.getResponseCode();
```

```php
$response->getAuthorizationCode();
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
## Versión 3.x del SDK
response['authorization_code']
response['authorization_date']
response['captured_amount']
response['response_code']

## Versión 2.x del SDK
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

Cuando la operación solicitada es ejecutada correctamente, se pueden recibir estos status HTTP:

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

Puedes revisar el proceso necesario para operar en el ambiente de producción en [la documentación](/documentacion/como_empezar#puesta-en-produccion)

### Configuración para producción utilizando los SDK

Si estas utilizando algún SDK oficial de Transbank, entonces debes seguir los siguientes pasos.

<aside class="warning">
Nunca dejes tu código de comercio y secreto compartido directamente en tu código, te recomendamos utilizar variables de entorno u otro método que te permita mantener tus credenciales seguras.
</aside>

Revisa la [esta sección](/documentacion/como_empezar#b-utilizando-los-sdk) de la documentación para ver el código necesario para configurar tu propio código de comercio y Api Key Secret. 

