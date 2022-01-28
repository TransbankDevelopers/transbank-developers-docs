# Webpay Modal

___

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

```javascript
// Host: https://webpay3g.transbank.cl
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

```javascript
// Host: https://webpay3g.transbank.cl
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

```javascript
// Tbk-Api-Key-Id: Código de comercio
// Tbk-Api-Key-Secret: Llave secreta
// Content-Type: application/json
```

Todas las peticiones que hagas deben incluir el código de comercio y la llave
secreta entregada por Transbank, actuando ambas como las credenciales que autorizan
distintas operaciones.

<aside class="notice">
Ten en cuenta que tu(s) código(s) de comercio en ambiente de producción no son
iguales a los entregados para el ambiente de integración.
</aside>

### Códigos de comercio

En la documentación puedes revisar [el código de comercio para Webpay Modal](/documentacion/webpay-modal#credenciales-y-ambiente) del ambiente de integración

> Los SDKs ya incluyen esos códigos de comercio y llaves secretas
> que funcionan en el ambiente de integración, por lo que puedes obtener
> rápidamente una configuración lista para hacer tus primeras pruebas en dicho
> ambiente:

```java
// El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
WebpayPlusModal.Transaction tx = new WebpayPlusModal.Transaction();
```

```php
// SDK Versión 2.x
// El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
\Transbank\Webpay\WebpayModal::configureForIntegration('597055555584', '579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C');

// Si deseas apuntar a producción: 
\Transbank\Webpay\WebpayModal::configureForProduction('tu-codigo-comercio', 'tu-api-key');
```

```csharp
// El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
WebpayModal.Transaction.CommerceCode = 597055555584;
WebpayModal.Transaction.ApiKey = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C";
WebpayModal.Transaction.IntegrationType = WebpayIntegrationType.Test;
```

```ruby

// Versión 2.x del SDK
@tx = Transbank::Webpay::WebpayPlusModal::Transaction.new(::Transbank::Common::IntegrationCommerceCodes::WEBPAY_PLUS_MODAL)

```

```python
# El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
transbank.webpay.webpay_modal.webpay_modal_default_commerce_code = 597055555584
transbank.webpay.webpay_modal.default_api_key = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C"
transbank.webpay.webpay_modal.default_integration_type = IntegrationType.TEST
```

```javascript
const Environment = require('transbank-sdk').Environment;

WebpayPlusModal.commerceCode = 597055555584;
WebpayPlusModal.apiKey = '579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C';
WebpayPlusModal.environment = Environment.Integration;
```

```http
Tbk-Api-Key-Id: 597055555584
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
```

### Crear una transacción
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-modal#crear-una-transaccion)

Permite inicializar una transacción en Webpay. Como respuesta a la invocación
se genera un token que representa en forma única una transacción.

Para crear una transacción basta llamar al método `Transaction.create()`

```php
use Transbank\Webpay\Webpay\Modal\Transaction;

$response = (new Transaction)->create($buy_order, $session_id, $amount, $return_url);
```

```http
POST /rswebpaytransaction/api/webpay/v1.2/transactions

Tbk-Api-Key-Id: 597055555584
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
 "buy_order": "ordenCompra12345678",
 "session_id": "sesion1234557545",
 "amount": 10000
}
```

```java
import cl.transbank.webpay.modal.WebpayPlusModal;
import cl.transbank.webpay.modal.WebpayModalTransaction.ModalTransactionCreateResponse;

WebpayPlusModal.Transaction tx = new WebpayPlusModal.Transaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MODAL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final ModalTransactionCreateResponse response = tx.create(buyOrder, sessionId, amount);
```

```javascript
const WebpayPlusModal = require('transbank-sdk').WebpayPlusModal;

const response = await WebpayPlusModal.Transaction.create(buyOrder, sessionId, amount);
```

<strong>Parámetros Transaction.create</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la tienda. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>
session_id  <br> <i> String </i> | Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
amount  <br> <i> Decimal </i> | Monto de la transacción. Máximo 2 decimales para USD. Largo máximo: 17

<strong>Respuesta Transaction.create</strong>

```php
$response->getToken();
```

```http
200 OK
Content-Type: application/json

{
 "token": "e9d555262db0f989e49d724b4db0b0af367cc415cde41f500a776550fc5fddd3",
}
```

```java
response.getToken();
```

```javascript
response.url
response.token
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.

Una vez obtenido el resultado de la operación el comercio debe crear un div con la url indicada
en la respuesta entregando el token como un parámetro llamado “token_ws”. Este parámetro
debe ser proporcionado para levantar el modal en el sitio del comercio y de esta forma modal
poder continuar con el proceso de pago.

### Generar el formulario de pago

Para el ingreso de datos de la tarjeta de débito, crédito o prepago, el comercio debe mostrar un formulario de pago.
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-modal#mostrar-formulario-de-pago)

### Confirmar una transacción
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-modal#confirmar-una-transaccion)

Permite confirmar y obtener el resultado de la transacción una vez que Webpay ha resuelto su autorización financiera.

Una vez completado el tiempo de espera para que el cliente ingrese sus datos debes autorizar y obtener
el resultado de la transacción usando el método `Transaction.commit()`.


```php
// SDK Versión 2.x
use Transbank\Webpay\Webpay\Modal\Transaction;

$response = (new Transaction)->commit($token);
```

```http
PUT /rswebpaytransaction/api/webpay/v1.2/transactions/{token}
Tbk-Api-Key-Id: 597055555584
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

```java
import cl.transbank.webpay.modal.WebpayPlusModal;
import cl.transbank.webpay.modal.WebpayModalTransaction.ModalTransactionCommitResponse;

WebpayPlusModal.Transaction tx = new WebpayPlusModal.Transaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MODAL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final ModalTransactionCommitResponse response = tx.commit(token);
```

```javascript
const WebpayPlusModal = require('transbank-sdk').WebpayPlusModal;

const response = await WebpayPlusModal.Transaction.commit(token);
```

<strong>Parámetros Transaction.commit</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

<strong>Respuesta Transaction.commit</strong>

```php
$response->getAmount();
$response->getStatus();
$response->getBuyOrder();
$response->getSessionId();
$response->getCardDetail();
$response->getCardNumber();
$response->getAccountingDate();
$response->getTransactionDate();
$response->getAuthorizationCode();
$response->getPaymentTypeCode();
$response->getResponseCode();
$response->getInstallmentsAmount();
$response->getInstallmentsNumber();
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

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor TSY (Autenticación exitosa), TSN (Autenticación fallida), TO (Tiempo máximo excedido para autenticación), ABO (Autenticación abortada por tarjetahabiente), U3 (Error interno en la autenticación), NP (No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure), ACS2 (Autenticación fallida extranjera). Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
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
response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> Puedes revisar los códigos de respuesta de rechazo en el siguiente [link](/producto/webpay#codigos-de-respuesta-de-autorizacion)<br>
installments_amount <br> <i> Number </i> | Monto de las cuotas. Largo máximo: 17
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
balance  <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17


### Obtener estado de una transacción
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-modal#obtener-estado-de-una-transaccion)

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable 
que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las 
acciones que correspondan.

<strong>Transaction.status()</strong>

```php
// SDK Versión 2.x
use Transbank\Webpay\Webpay\Modal\Transaction;

$response = (new Transaction)->status('token-de-la-transaccion');
```

```http
GET /rswebpaytransaction/api/webpay/v1.2/transactions/{token}
Tbk-Api-Key-Id: 597055555584
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

```java
import cl.transbank.webpay.modal.WebpayPlusModal;
import cl.transbank.webpay.modal.WebpayModalTransaction.ModalTransactionStatusResponse;

WebpayPlusModal.Transaction tx = new WebpayPlusModal.Transaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MODAL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final ModalTransactionStatusResponse response = tx.status(token);
```

```javascript
const WebpayPlusModal = require('transbank-sdk').WebpayPlusModal;

const response = await WebpayPlusModal.Transaction.status(token);
```


<strong>Parámetros Transaction.status</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

<strong>Respuesta Transaction.status</strong>

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
        "card_number": "1234" 
        },
    "accounting_date": "0320", 
    "transaction_date": "2019-03-20T20:18:20Z", 
    "authorization_code": "877550", 
    "payment_type_code": "VN", 
    "response_code": 0,
    "installments_number": 3,
    "installments_amount": 1000 }
```

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

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor TSY (Autenticación exitosa), TSN (Autenticación fallida), TO (Tiempo máximo excedido para autenticación), ABO (Autenticación abortada por tarjetahabiente), U3 (Error interno en la autenticación), NP (No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure), ACS2 (Autenticación fallida extranjera). Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
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

### Reversar o Anular un pago
Puedes revisar más detalles de esta operación en [su documentación](/documentacion/webpay-plus#reversar-o-anular-una-transaccion)

Para anular una transacción se debe invocar al método `Transaction.refund()`.

<strong>Transaction.refund()</strong>

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$amount = 1000; // Monto que se desea devolver
$response = (new Transaction)->refund('token-de-la-transaccion', $amount);
```

```http
POST /rswebpaytransaction/api/webpay/v1.2/transactions/{token}/refunds
Tbk-Api-Key-Id: 597055555584
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "amount": 1000
}
```

```java
import cl.transbank.webpay.modal.WebpayPlusModal;
import cl.transbank.webpay.modal.WebpayModalTransaction.ModalTransactionRefundResponse;

WebpayPlusModal.Transaction tx = new WebpayPlusModal.Transaction(new WebpayOptions(IntegrationCommerceCodes.WEBPAY_PLUS_MODAL, IntegrationApiKeys.WEBPAY, IntegrationType.TEST));
final ModalTransactionRefundResponse response = tx.refund(token, amount);
```

```javascript
const WebpayPlusModal = require('transbank-sdk').WebpayPlusModal;

const response = await WebpayPlusModal.Transaction.refund(token, amount);
```

<strong>Parámetros Transaction.refund</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
amount  <br> <i> Formato número entero para transacciones en peso. Sólo en caso de dólar acepta dos decimales. </i> | Monto que se desea anular o reversar de la transacción. Largo máximo: 17.

<strong>Respuesta Transaction.refund</strong>

En el caso de que la transacción corresponda a una Reversa solo se retorna el parámetro <i>type<i> (REVERSED).

```php 
$response->getAuthorizationCode();
$response->getAuthorizationDate();
$response->getBalance();
$response->getNullifiedAmount();
$response->getResponseCode();
$response->getType();
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

```java
response.getType();
response.getBalance();
response.getAuthorizationCode();
response.getResponseCode();
response.getAuthorizationDate();
response.getNullifiedAmount();
response.getPrepaidBalance();
```

```javascript
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSED o NULLIFIED).  Si es REVERSED no se devolverán datos de la transacción (authorization code, etc). Largo máximo: 10
authorization_code  <br> <i> String </i> | (Solo si es NULLIFIED) Código de autorización de la anulación. Largo máximo: 6
authorization_date  <br> <i> String </i> | (Solo si es NULLIFIED) Fecha y hora de la autorización.
nullified_amount  <br> <i> Decimal </i> | (Solo si es NULLIFIED) Monto anulado. Largo máximo: 17
balance  <br> <i> Decimal </i> | (Solo si es NULLIFIED) Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17
response_code <br> <i> Number </i> | (Solo si es NULLIFIED) Código de resultado de la reversa/anulacion. Si es exitoso es 0, de lo contrario la reversa/anulación no fue realizada Largo Máximo: 2
