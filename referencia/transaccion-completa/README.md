# Transacción Completa

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
// Host: https://webpay3gint.transbank.cl
```

### Tarjetas y usuarios de prueba

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

En la documentación puedes revisar [todos los códigos de comercio](/documentacion/como_empezar#codigos-de-comercio) del ambiente de integración

> Los SDKs ya incluyen esos códigos de comercio y llaves secretas
> que funcionan en el ambiente de integración, por lo que puedes obtener
> rápidamente una configuración lista para hacer tus primeras pruebas en dicho
> ambiente.

## Transacción Completa

<aside class="warning">
Este producto tiene requerimientos comerciales más estrictos que el resto de los productos.  
No inicies la integración si aún no completan la afiliación comercial.
</aside>

### Crear una Transacción Completa

Puedes revisar más detalles de esta operación en [su documentación](/documentacion/transaccion-completa#crear-una-transaccion)

Para crear una transacción completa basta llamar al método `Transaction.create()`

<strong>Transaction.create()</strong>

Permite inicializar una transacción completa en Webpay. Como respuesta a la
invocación se genera un token que representa en forma única una transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

```java
final FullTransactionCreateResponse response =  FullTransaction.Transaction.create(
  buyOrder,                         // ordenCompra12345678
  sessionId,                        // sesion1234564
  amount,                           // 10000
  cardNumber,                       // 123
  cardExpirationDate,               // 4239000000000000
  cvv                               // 22/10
);
```

```php
use Transbank\TransaccionCompleta\Transaction;

$response = Transaction::create(
  $buy_order,                       // ordenCompra12345678
  $session_id,                      // sesion1234564
  $amount,                          // 10000
  $cvv,                             // 123
  $card_number,                     // 4239000000000000
  $card_expiration_date             // 22/10
);
```

```csharp
FullTransaction.Create(
  buyOrder: buy_order,                        // ordenCompra12345678
  sessionId: session_id,                      // sesion1234564
  amount: amount,                             // 10000
  cvv: cvv,                                   // 123
  cardNumber: card_number,                    // 4239000000000000
  cardExpirationDate: card_expiration_date    // 22/10
);
```

```ruby
Transbank::TransaccionCompleta::Transaction::create(
  buy_order: 'ordenCompra12345678',
  session_id: 'sesion1234564',
  amount: 10000,
  card_number: 4239000000000000,
  cvv: 123,
  card_expiration_date: '22/10'
)
```

```python
from transbank.transaccion_completa.transaction import Transaction
#...
Transaction.create(
    buy_order = 'ordenCompra12345678',
    session_id = 'sesion1234564',
    amount = 10000,
    card_number = 4239000000000000,
    cvv = 123,
    card_expiration_date = '22/10'
)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions

Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234564",
  "amount": 10000,
  "cvv": 123,
  "card_number": "4239000000000000",
  "card_expiration_date": "22/10"
}
```

```javascript
const response = await TransaccionCompleta.transaction.create(buyOrder, sessionId, amount, cvv, cardNumber, cardExpirationDate);

```


<strong>Parámetros Transaction.create</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la tienda. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>
session_id  <br> <i> String </i> | Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
amount  <br> <i> Decimal </i> | Monto de la transacción. Máximo 2 decimales para USD. Largo máximo: 17
cvv  <br> <i> String </i> | (Opcional) Código que se utiliza como método de seguridad en transacciones en las que la tarjeta no está físicamente presente. Largo máximo: 4. No se debe enviar para comercios con la opción `sin cvv` habilitada.
card_number  <br> <i> String </i> | Número de tarjeta. Largo máximo: 16
card_expiration_date  <br> <i> String </i> | Fecha de expiración de la tarjeta con la que se realiza la transacción. Largo máximo: 5

<strong>Respuesta Transaction.create</strong>

```java
response.getToken();
```

```php
$response->getToken();
```

```csharp
response.Token;
```

```ruby
response.token
```

```python
response.token
```

```http
200 OK
Content-Type: application/json

{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
}
```

```javascript
response.token
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.

<strong>Modalidad sin cvv</strong>

Para modalidad del producto Transacción completa `sin CVV`, este campo **no** debe ser enviado.

```http
200 OK
Content-Type: application/json
{
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234564",
  "amount": 10000,
  "card_number": "4239000000000000",
  "card_expiration_date": "22/10"
}
```

### Consulta de cuotas

Para consultar el valor de las cuotas que pagará el tarjeta habiente en una
transacción completa, es necesario llamar al método `Transaction.installments()`

<strong>Transaction.installments()</strong>

Operación que permite obtener el monto de la cuota a partir del número de cuotas.
El id de la consulta que selecciona el tarjetahabiente debe ser informado en la
invocación de la confirmación.

```java
final FullTransactionInstallmentsResponse response =  FullTransaction.Transaction.installment(token, installments_number);
```

```php
use Transbank\TransaccionCompleta\Transaction;

$installments = Transaction::installments($token_ws, $installments_number);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Installments(
  token,
  installments_number
);
```

```ruby
Transbank::TransaccionCompleta::Transaction::installments(
   token: token,
   installments_number: installments_number
)
```

```python
from transbank.transaccion_completa.transaction import Transaction

Transaction.installments(
  token=token,
  installments_number=installments_number
)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/installments

Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "installments_number": 10
}
```

```javascript
const response = await TransaccionCompleta.Transaction.installments(token, installmentsNumber);
```

<strong>Parámetros Transaction.installments</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2

<strong>Respuesta Transaction.installments</strong>

```java
response.getInstallmentsAmount();
response.getIdQueryInstallments();
DeferredPeriod deferredPeriod = response.getDeferredPeriods()[0];
deferredPeriod.getAmount();
deferredPeriod.getPeriod();
```

```php
response->getInstallmentsAmount();
response->getIdQueryInstallments();
response->getDeferredPeriods();
```

```csharp
response.InstallmentsAmount;
respone.IdQueryInstallments;
response.DeferredPeriods;
```

```ruby
response.installments_amount
response.id_query_installments
response.deferred_periods
```

```python
response.installments_amount
response.id_query_installments
response.deferred_periods
```

```javascript
response.installments_amount
response.id_query_installments
response.deferred_periods
```

Si el comercio no tiene configurado periodos diferidos, la respuesta de `deferred_periods` será `[]`:

```http
200 OK
Content-Type: application/json

{
  "installments_amount": 3334,
  "id_query_installments": 11,
  "deferred_periods": [
    {
      "amount": 1000,
      "period": 1
    }
  ]
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
installments_amount  <br> <i> String </i> | Monto de cada cuota. Largo: 17.
id_query_installments  <br> <i> String </i> | Identificador de las cuotas. Largo: 19.
deferred_periods  <br> <i> Array </i> | Arreglo con periodos diferidos.
deferred_periods [].amount  <br> <i> String </i> | Monto. Largo: 17.
deferred_periods [].period  <br> <i> String </i> | Índice de periodo. Largo: 2.

### Confirmación de la transacción

Una vez iniciada la transacción y consultado el monto de las cuotas, puedes
confirmar y obtener el resultado de una transacción completa usando el metodo
`Transaction.commit()`.

<strong>Transaction.commit()</strong>

Operación que permite confirmar una transacción. Retorna el estado de la
transacción.

```java
import cl.transbank.transaccioncompleta.FullTransaction;

final FullTransactionCommitResponse response = FullTransaction.Transaction.commit(
  token, idQueryInstallments, deferredPeriodIndex, gracePeriod
);
```

```php
use Transbank\TransaccionCompleta\Transaction;
//...
Transaction::commit(
  $token_ws,
  $id_query_installments,
  $deferred_period_index,
  $grace_period
);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Commit(
  token, idQueryInstallments, deferredPeriodsIndex, gracePeriods
);
```

```ruby
Transbank::TransaccionCompleta::Transaction::commit(
  token: token,
  id_query_installments: id_query_installments,
  deferred_period_index: deferred_period_index,
  grace_period: grace_period
)
```

```python
from transbank.transaccion_completa.transaction import Transaction

Transaction.commit(
  token=token,
  id_query_installments=id_query_installments,
  deferred_period_index=deferred_period_index,
  grace_period=grace_period
)
```

```http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}

Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "id_query_installments": 15,
  "deferred_period_index": 1,
  "grace_period": false
}
```

```javascript
const response = await TransaccionCompleta.Transaction.commit(
    token, idQueryInstallments, deferredPeriodIndex, gracePeriod);
```

<strong>Parámetros Transaction.commit</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
id_query_installments  <br> <i> Number </i> | (Opcional) Identificador de cuota. Largo máximo: 19. Solo enviar si el pago es en cuotas
deferred_period_index  <br> <i> Number </i> | (Opcional) Cantidad de periodo diferido. Largo máximo: 2. Solo enviar si el pago es en cuotas
grace_period  <br> <i> Boolean </i> | (Opcional) Indicador de periodo de gracia. Solo enviar si el pago es en cuotas

<strong>Respuesta Transaction.commit</strong>

```java
response.getAccountingDate();
response.getAmount();
response.getAuthorizationCode();
response.getBuyOrder();
CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
response.getInstallmentsAmount();
response.getInstallmentsNumber();
response.getPaymentCodeType();
response.getResponseCode();
response.getSessionId();
response.getTransactionDate();
```

```php
response->getAccountingDate();
response->getAmount();
response->getAuthorizationCode();
response->getBuyOrder();
cardDetail = response->getCardDetail();
cardDetail->getCardNumber();
response->getInstallmentsAmount();
response->getInstallmentsNumber();
response->getPaymentCodeType();
response->getResponseCode();
response->getSessionId();
response->getTransactionDate();
```

```csharp
response.AccountingDate;
response.Amount;
response.AuthorizationCoda;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.PaymentTypeCode;
response.ResponseCode;
response.SessionId;
response.Status;
response.TransactionDate;
```

```ruby
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
```

```python
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
```

```http
200 OK
Content-Type: application/json

{
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
  "installments_number": 0,
  "installmentsAmount": 0
}
```

```javascript
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
amount  <br> <i> Number </i> | Monto de la transacción. Sólo en caso de dolar acepta dos decimales. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
buy_order  <br> <i> String </i> | Número de orden de compra. Largo máximo: 26
session_id  <br> <i> String </i> | ID de sesión de la compra. Largo máximo: 61
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción, solo si el comercio tiene configurado el poder recibir el número de tarjeta. Largo máximo: 19
accounting_date  <br> <i> String </i> | Fecha contable de la transacción en formato MMYY.
transaction_date  <br> <i> ISO8601 </i> | Fecha de la transacción.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción de pago. Largo máximo: 6
payment_type_code  <br> <i> String </i> | Indica el tipo de tarjeta utilizada. Largo máximo: 2 <br> VD = Venta Débito. <i> (Próximamente) </i> <br> VN = Venta Normal. <br> VP = Venta Prepago.<br> <i> (Próximamente)</i> <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés
response_code  <br> <i> Number </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br>
installments_amount <br> <i> Number </i> | Monto de la cuota. Se envía solo si tiene valor cuota. <br> <i> Largo máximo: 17 </i>
installments_number <br> <i> Number </i> | Número de cuotas de la transacción. <br> <i>Largo máximo: 2 </i>
prepaid_balance <br> <i> Number </i> | Saldo de la tarjeta de prepago. Se envía solo si se informa saldo. <br> <i> Largo máximo: 17 </i>

### Consultar estado de una transacción completa

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

<strong>Transaction.status()</strong>

Obtiene resultado de transacción a partir de un token.

```java
import cl.transbank.transaccioncompleta.FullTransaction;

final FullTransactionStatusResponse response = FullTransaction.Transaction.status(token);
```

```php
use Transbank\TransaccionCompleta\Transaction;

Transaction::getStatus($token_ws);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Status(token);
```

```ruby
Transbank::TransaccionCompleta::Transaction::status(token: token)
```

```python
from transbank.transaccion_completa.transaction import Transaction

Transaction.status(token=token)
```

```http
GET /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

```javascript
const response = await TransaccionCompleta.Transaction.status(token);
```

<strong>Parámetros Transaction.status</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

<strong>Respuesta Transaction.status</strong>

```java
response.getAccountingDate();
response.getAmount();
response.getAuthorizationCode();
response.getBuyOrder();
CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
response.getInstallmentsAmount();
response.getInstallmentsNumber();
response.getPaymentCodeType();
response.getResponseCode();
response.getSessionId();
response.getTransactionDate();
```

```php
response->getAccountingDate();
response->getAmount();
response->getAuthorizationCode();
response->getBuyOrder();
cardDetail = response->getCardDetail();
cardDetail->getCardNumber();
response->getInstallmentsAmount();
response->getInstallmentsNumber();
response->getPaymentCodeType();
response->getResponseCode();
response->getSessionId();
response->getTransactionDate();
```

```csharp
response.AccountingDate;
response.Amount;
response.AuthorizationCoda;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.PaymentTypeCode;
response.ResponseCode;
response.SessionId;
response.Status;
response.TransactionDate;
```

```ruby
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
```

```python
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
```

```http
200 OK
Content-Type: application/json

{
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
  "installments_number": 0
}
```

```javascript
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
amount  <br> <i> Number </i> | Monto de la transacción. Sólo en caso de dolar acepta dos decimales. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
buy_order  <br> <i> String </i> | Número de orden de compra. Largo máximo: 26
session_id  <br> <i> String </i> | ID de sesión de la compra. Largo máximo: 61
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_detail.card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción. Largo máximo: 19
accounting_date  <br> <i> String </i> | Fecha contable de la transacción.
transaction_date  <br> <i> ISO8601 </i> | Fecha de la transacción.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción de pago. Largo máximo: 6
payment_type_code  <br> <i> String </i> | Indica el tipo de tarjeta utilizada. Largo máximo: 2 <br> VD = Venta Débito. <i> (Próximamente) </i> <br> VN = Venta Normal. <br> VP = Venta Prepago.<br> <i> (Próximamente)</i> <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés
response_code  <br> <i> Number </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br>
installments_number <br> <i> Number </i> | Número de cuotas de la transacción. Largo máximo: 2
installments_amount <br> <i> Number </i> | Monto de la cuota. Se envía solo si tiene valor cuota. <br> <i> Largo máximo 17 </i>
balance <br> <i> Number </i> | Monto restante. Largo máximo: 17. Este campo solo viene cuando la transacción fue anulada
prepaid_balance <br> <i> Number </i> | Saldo de la tarjeta de prepago. Se envía solo si se informa saldo. <br> <i> Largo máximo: 17 </i>

### Reversar o Anular un pago Transacción Completa

Este método permite a todo comercio habilitado reversar o anular una transacción
completa. El método permite generar el reembolso del total o parte del monto de
una transacción dependiendo de la siguiente lógica de negocio la invocación a
esta operación generará una reversa o una anulación:

* Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.
* Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.

La anulación puede realizarse máximo 90 días después de la fecha de la
transacción original.

Puedes [leer más sobre la anulación en la información del
producto Webpay](/producto/webpay#anulaciones) para conocer
más detalles y restricciones.

Para anular una transacción se debe invocar al método `Transaction.refund()`.

Permite generar el reembolso del total o parte del monto de una transacción completa.
Dependiendo de la siguiente lógica de negocio la invocación a esta operación generará una reversa o una anulación:

* Si se especifica un valor en el campo “amount” se ejecutará siempre una anulación.
* Si se supera el tiempo máximo para ejecutar una reversa se ejecutará una anulación.
* Si no se ha dado ninguno de los casos anteriores se ejecutará una reversa.

<strong>Transaction.refund()</strong>

Permite solicitar a Webpay la anulación de una transacción realizada previamente y que se encuentre vigente.

```java
import cl.transbank.transaccioncompleta.FullTransaction;

final FullTransactionRefundResponse response = FullTransaction.Transaction.refund(token,amount);
```

```php
use Transbank\TransaccionCompleta\Transaction;

Transaction::refund($token, $amount);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Refund(token, amount);
```

```ruby
Transbank::TransaccionCompleta::Transaction::refund(token: token, amount: amount)
```

```python
from transbank.transaccion_completa.transaction import Transaction

Transaction.refund(token=token, amount=amount)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/refunds
Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
 "amount": 1000
}
```

```javascript
const response = await TransaccionCompleta.Transaction.refund(token, amount);
```

<strong>Parámetros Transaction.refund</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
amount  <br> <i> Formato número entero para transacciones en peso. Sólo en caso de dólar acepta dos decimales. </i> |  Monto que se desea anular o reversar de la transacción. Largo máximo: 17.

<strong>Respuesta Transaction.refund</strong>

```java
response.getType();
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getNullifiedAmount();
response.getBalance();
response.getResponse();
```

```php
response->getType();
response->getAuthorizationCode();
response->getAuthorizationDate();
response->getNullifiedAmount();
response->getBalance();
response->getResponse();
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
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
```

```python
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
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
type  <br> <i> String </i> | Tipo de reembolso (REVERSE. NULLIFY). Largo máximo: 10
authorization_code  <br> <i> String </i> | Código de autorización de la anulación. Largo máximo: 6. Solo viene en caso de anulación.
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización. Solo viene en caso de anulación.
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17. Solo viene en caso de anulación.
balance  <br> <i> Decimal </i> | Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17. Solo viene en caso de anulación.
response_code  <br> <i> Number </i> | Código de resultado de la anulación. Si es exitoso es 0, de lo contrario la anulación no fue realizada. Largo máximo: 2. Solo viene en caso de anulación.

## Captura Diferida

Los comercios que están configurados para operar con captura diferida deben ejecutar el método de captura para realizar el cargo al tarjetahabiente.

**Válido para :**

* Webpay Plus Captura Diferida
* Transacción Completa Captura Diferida

### Ejecutar captura diferida Transaccion Completa

Puedes [leer más sobre la captura en la información del
producto Webpay](/producto/webpay#autorizacion-y-captura)
para conocer más detalles y restricciones.

Para realizar esa captura explícita debe usarse el método `Transaction.capture()`

<strong>Transaction.capture()</strong>

Permite solicitar a Webpay la captura diferida de una transacción con
autorización y sin captura simultánea.

```http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/capture
Tbk-Api-Key-Id: 597055555531
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "buy_order": "415034240",
  "authorization_code": "12345",
  "capture_amount": 1000
}
```

```javascript
const response = TransaccionCompleta.DeferredTransaction.capture(
  token, buyOrder, authorizationCode, amount
);
```
<strong>Parámetros Transaction.capture</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere capturar Largo máximo: 6.
capture_amount  <br> <i> Decimal </i> | Monto que se desea capturar. Largo máximo: 17.

<strong>Respuesta Transaction.capture</strong>

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

```javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
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

Código  | Descripción
------  | -----------
304     | Validación de campos de entrada nulos
245     | Código de comercio no existe
22      | El comercio no se encuentra activo
316     | El comercio indicado no corresponde al certificado o no es hijo del comercio Mall en caso de transacciones MALL
308     | Operación no permitida
274     | Transacción no encontrada
16      | La transacción no es de captura diferida
292     | La transacción no está autorizada
284     | Periodo de captura excedido
310     | Transacción reversada previamente
309     | Transacción capturada previamente
311     | Monto a capturar excede el monto autorizado
315     | Error del autorizador


## Transacción Completa Mall

<aside class="warning">
Este producto tiene requerimientos comerciales más estrictos que el resto de los productos.  
No inicies la integración si aún no completan la afiliación comercial.
</aside>

Puedes revisar más detalles de esta operación en [su documentación](/documentacion/transaccion-completa#)

### Crear una Transacción Completa Mall

Para crear una Transacción Completa Mall basta llamar al método `Transaction.create()`

<strong>Transaction.create() Completa Mall</strong>

Permite inicializar una transacción Completa Mall en Webpay. Como respuesta a la
invocación se genera un token que representa en forma única una transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

```java
MallTransactionCreateDetails transactionDetails = MallTransactionCreateDetails.build()
  .add(amountMallOne, commerceCodeMallOne, buyOrderMallOne, installmentsNumberMallOne)
  .add(amountMallTwo, commerceCodeMallTwo, buyOrderMallTwo, installmentsNumberMallTwo);

final MallFullTransactionCreateResponse response = MallFullTransaction.Transaction.create(
  buyOrder,                           // ordenCompra12345678
  sessionId,                          // sesion1234564
  cardNumber,                         // 4239000000000000
  cardExpirationDate,                 // 22/10
  transactionDetails
);
```

```php
use Transbank\TransaccionCompleta\MallTransaccionCompleta;

$transaction_details = [
  {
      "amount": 10000,
      "commerce_code": 597055555552,
      "buy_order": "123456789"
  },
  {
     "amount": 12000,
     "commerce_code": 597055555553,
     "buy_order": "123456790"
  },
];

$response = MallTransaction::create(
  $buy_order,                         // ordenCompra12345678
  $session_id,                        // sesion1234564
  $card_number,                       // 4239000000000000
  $card_expiration_date,              // 22/10
  $transaction_details
);
```

```csharp
using Transbank.Webpay.TransaccionCompletaMall;

var transactionDetails = new List<CreateDetails>();
transactionDetails.Add(new CreateDetails(
    amountMallOne,
    commerceCodeMallOne,
    buyOrderMallOne
));
transactionDetails.Add(new CreateDetails(
    amountMallTwo,
    comerceCodeMallTwo,
    buyOrderMallTwo
));

var response = MallFullTransaction.Create(
  buyOrder,
  sessionId,
  cardNumber,
  cardExpirationDate,
  transactionDetails
);
```

```ruby
details = [
  {
    amount: 10000,
    commerce_code: 597055555552,
    buy_order: '123456789'
  },
  {
    amount: 12000,
    commerce_code: 597055555553,
    buy_order: '123456790'
  }
]
response = Transbank::TransaccionCompleta::MallTransaction::create(
  buy_order: 'ordenCompra12345678',
  session_id: 'sesion1234564',
  card_number: 4239000000000000,
  card_expiration_date: '22/10',
  details: details
)
```

```python
details = [
  {
      'commerce_code': 597055555552,
      'buy_order': '123456789',
      'amount': 10000
  },
  {
      'commerce_code': 597055555553,
      'buy_order': '123456790',
      'amount': 12000
  }
]

response = Transaction.create(
  buy_order=buy_order,
  session_id=session_id,
  card_number=card_number, card_expiration_date=card_expiration_date, details=details
)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions

Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234564",
  "card_number": "4239000000000000",
  "card_expiration_date": "22/10",
  "details": [
    {
      "amount": 10000,
      "commerce_code": "597055555552",
      "buy_order": "123456789"
    },
    {
      "amount": 10000,
      "commerce_code": "597055555553",
      "buy_order": "123456790"
    }
  ]
}
```

```javascript
const details = [
  new TransactionDetail(amount, commerceCode, childBuyOrder),
  new TransactionDetail(amount2, commerceCode2, childBuyOrder2)
];

const response = await TransaccionCompleta.MallTransaction.create(		
  parentBuyOrder,		
  sessionId,		
  cvv,		
  cardNumber,		
  cardExpirationDate,		
  details
);
```

<strong>Parámetros Transaction.create Completa Mall</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Es el código único de la orden de compra generada por el comercio mall. Largo máximo: 26
session_id  <br> <i> String </i> |  Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
card_number  <br> <i> String </i> | Número de la tarjeta con la que se debe hacer la transacción. Largo máximo: 19
card_expiration_date  <br> <i> String </i> | Fecha de expiración de la tarjeta con la que se realiza la transacción. Largo máximo: 5
details  <br> <i> Array </i> | Lista de objetos, uno por cada tienda diferente del mall que participa en la transacción.
details [].amount  <br> <i> Decimal </i> | Monto de la transacción de una tienda del mall. Máximo 2 decimales para USD. Largo máximo: 17.
details [].commerce_code  <br> <i>String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo: 12.
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda del mall. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>. Largo máximo: 26

<strong>Respuesta Transaction.create Completa Mall</strong>

```java
response.getToken();
```

```php
response->getToken();
```

```csharp
response.Token;
```

```ruby
response.token
```

```python
response.token
```

```http
200 OK
Content-Type: application/json

{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
}
```

```javascript
response.token
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token identificador de la transacción. Largo: 64.

### Consulta de cuotas Completa Mall

Para consultar el valor de las cuotas que pagará el tarjeta habiente en cada transacción dentro transacción Completa Mall, es necesario llamar al método `Transaction.installments()`

<strong>Transaction.installments() Completa Mall</strong>

Operación que permite obtener el monto de la cuota a partir del número de cuotas.
El id de la consulta que selecciona el tarjetahabiente debe ser informado en la
invocación de la confirmación.

```java
MallFullTransactionInstallmentsDetails installmentsDetails = MallFullTransactionInstallmentsDetails.build().add(commerceCode, buyOrder, installmentsNumber);
final MallFullTransactionInstallmentsResponse response = MallFullTransaction.Transaction.installment(token, installmentsDetails);
```

```php
$installments_details = [
  {
    "commerce_code": 597055555552,
    "buy_order": "123456789",
    "installments_number": 2
  },
  {
    "commerce_code": 597055555553,
    "buy_order": "123456790",
    "installments_number": 2
  },
];

$res = MallTransaction::installments(
  $token,
  $installments_details
);
```

```csharp
using Transbank.Webpay.TransaccionCompletaMall;

var installmentsDetails = new List<MallInstallmentsDetails>();
installmentsDetails.Add(new CreateDetails(
    commerceCodeMallOne,
    buyOrderMallOne,
    installmentsNumberMallOne
));
installmentsDetails.Add(new CreateDetails(
    comerceCodeMallTwo,
    buyOrderMallTwo,
    installmentsNumberMallTwo
));

var response = MallFullTransaction.Installments(
  token, installmentDetails
);
```

```ruby
installment_details = [
  {
    commerce_code: 597055555552,
    buy_order: '123456789',
    installments_number: 2
  },
  {
    commerce_code: 597055555553,
    buy_order: '123456790',
    installments_number: 2
  },
]

response = Transbank::TransaccionCompleta::MallTransaction::installments(token, installment_details)
```

```python
from transbank.transaccion_completa_mall.transaction import Transaction

details = [
  {
    'commerce_code': 597055555552,
    'buy_order': '123456789',
    'installments_number': 2
  },
  {
    'commerce_code': 597055555553,
    'buy_order': '123456790',
    'installments_number': 2
  }
]

response = Transaction.installments(token=token, details=details)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/installments

Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "commerce_code": 597055555552,
  "buy_order": "123456789",
  "installments_number": 10
}
```

```javascript
const details = [
  new InstallmentDetail(childCommerceCode, childBuyOrder, installmentsNumber),
  new InstallmentDetail(childCommerceCode2, childBuyOrder2, installmentsNumber2)
  ];		

const response = await TransaccionCompleta.MallTransaction.installments(		
  token,		
  details
);
```

<strong>Parámetros Transaction.installments Completa Mall</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
commerce_code  <br> <i> String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo: 12
buy_order  <br> <i> String </i> | Orden de compra de la tienda del mall. Largo máximo: 26
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2

<strong>Respuesta Transaction.installments Completa Mall</strong>

```java
response.getInstallmentsAmount();
response.getIdQueryInstallments();
DeferredPeriod deferredPeriod = response.getDeferredPeriods()[0];
deferredPeriod.getAmount();
deferredPeriod.getPeriod();
```

```php
$response->getInstallmentsAmount();
$response->getIdQueryInstallments();
$deferredPeriod = response->getDeferredPeriods()[0];
$deferredPeriod->getAmount();
$deferredPeriod->getPeriod();
```

```csharp
response.InstallmentsAmount;
response.IdQueryInstallments;
var deferredPeriod = response.DeferredPeriods[0];
deferredPeriod.Amount;
deferredPeriod.Period;
```

```ruby
response.installments_amount
response.id_query_installments
deferred_period = response.deferred_periods[0]
deferred_period.amount
deferred_period.period
```

```python
response.installments_amount
response.id_query_installments
deferred_period = response.deferred_periods[0]
deferred_period.amount
deferred_period.period
```

```http
200 OK
Content-Type: application/json

{
  "installments_amount": 3334,
  "id_query_installments": 11,
  "deferred_periods": [
    {
      "amount": 1000,
      "period": 1
    }
  ]
}
```

```javascript
response.installment_amount
response.id_query_installment
deferred_period = response.deferred_periods[0]
deferred_period.amount
deferred_period.period
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
installments_amount  <br> <i> String </i> | Monto de cada cuota. Largo: 17.
id_query_installments  <br> <i> String </i> | Identificador de las cuotas. Largo: 19.
deferred_periods  <br> <i> Array </i> | Arreglo con periodos diferidos.
deferred_periods [].amount  <br> <i> String </i> | Monto. Largo: 17.
deferred_periods [].period  <br> <i> String </i> | Índice de periodo. Largo: 2.

### Confirmación de la transacción Completa Mall  

Una vez iniciada la transacción y consultado el monto de las cuotas por cada subtransacción, puedes confirmar y obtener el resultado de una transacción completa usando el metodo `Transaction.commit()`.

<strong>Transaction.commit() Completa Mall</strong>

Operación que permite confirmar una transacción. Retorna el estado de la
transacción.

```java
MallTransactionCommitDetails details = MallTransactionCommitDetails.build().add(
  commerceCode,buyOrder,idQueryInstallments,deferredPeriodIndex,gracePeriod
);

final MallFullTransactionCommitResponse response = MallFullTransaction.Transaction.commit(token, details);
```

```php
use Transbank\TransaccionCompleta\Transaction;

$details = [
  {
    "commerce_code": '597055555552',
    "buy_order": 'ordenCompra1234',
    "id_query_installments": 12,
    "deferred_period_index": 1,
    "grace_period": false
  },
  {
    "commerce_code": '597055555553',
    "buy_order": 'ordenCompra12345',
    "id_query_installments": 12,
    "deferred_period_index": 1,
    "grace_period": false
  }
]

$res = MallTransaction::commit(
  $token,
  $details
);
```

```csharp
using Transbank.Webpay.TransaccionCompletaMall;

var transactionDetails = new List<MallCommitDetails>();
transactionDetails.Add(new MallCommitDetails(
    commerceCodeMallOne,
    buyOrderMallOne,
    idQueryInstallmentsOne,
    deferredPeriodIndexOne,
    gracePeriodOne
));
transactionDetails.Add(new MallCommitDetails(
    commerceCodeMallTwo,
    buyOrderMallTwo,
    idQueryInstallmentsTwo,
    deferredPeriodIndexTwo,
    gracePeriodTwo
));

var response = MallFullTransaction.Commit(
  token, transactionDetails
);
```

```ruby
details = [
  {
    commerce_code: '597055555552',
    buy_order: 'ordenCompra1234',
    id_query_installments: 12,
    deferred_period_index: 1,
    grace_period: false
  },
  {
    commerce_code: '597055555553',
    buy_order: 'ordenCompra12345',
    id_query_installments: 12,
    deferred_period_index: 1,
    grace_period: false
  }
]

response = Transbank::TransaccionCompleta::MallTransaction::commit(
  token: token, details: details
)
```

```python
details = [
  {
    "commerce_code": '597055555552',
    "buy_order": 'ordenCompra1234',
    "id_query_installments": 12,
    "deferred_period_index": 1,
    "grace_period": false
  },
  {
    "commerce_code": '597055555553',
    "buy_order": 'ordenCompra12345',
    "id_query_installments": 12,
    "deferred_period_index": 1,
    "grace_period": false
  }
]

response = Transaction.commit(
  token=token, details=details
)
```

```http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}

Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "details": [
    {
      "commerce_code": 597055555552,
      "buy_order": "ordenCompra1234",
      "id_query_installments": 12,
      "deferred_period_index": 1,
      "grace_period": false
    }
  ]
}
```

```javascript
let commitDetails = [
  new CommitDetail(commerceCode, childBuyOrder),
  new CommitDetail(commerceCode2, childBuyOrder2)
];		
const response = await TransaccionCompleta.MallTransaction.commit(		
  token,		
  commitDetails
);
```

<strong>Parámetros Transaction.commit Completa Mall</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
details <br> <i> details </i> | Listado con las transacciones mall.
commerce_code  <br> <i> String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo máximo: 12
buy_order  <br> <i> String </i> | Orden de compra de la tienda del mall. Largo máximo: 26
id_query_installments  <br> <i> Number </i> | (Opcional) Identificador de cuota. Largo máximo: 19. Solo enviar si el pago es en cuotas
deferred_period_index  <br> <i> Number </i> | (Opcional) Cantidad de periodo diferido. Largo máximo: 2. Solo enviar si el pago es en cuotas
grace_period  <br> <i> Boolean </i> | (Opcional) Indicador de periodo de gracia. Solo enviar si el pago es en cuotas

<strong>Respuesta Transaction.commit Completa Mall</strong>

```java
response.getBuyOrder();
response.getCardNumber();
response.getAccountingDate();
response.getTransactionDate();
Detail detail = response.getDetails()[0];
detail.getAuthorizationCode();
detail.getPaymentCodeType();
detail.getResponseCode();
detail.getInstallmentsAmount();
detail.getInstallmentsNumber();
detail.getAmount();
detail.getCommerceCode();
detail.getBuyOrder();
detail.getStatus();
detail.getBalance();
```

```php
$response.getBuyOrder();
$response.getCardNumber();
$response.getAccountingDate();
$response.getTransactionDate();
$detail = response.getDetails()[0];
$detail.getAuthorizationCode();
$detail.getPaymentCodeType();
$detail.getResponseCode();
$detail.getInstallmentsAmount();
$detail.getInstallmentsNumber();
$detail.getAmount();
$detail.getCommerceCode();
$detail.getBuyOrder();
$detail.getStatus();
$detail.getBalance();
```

```csharp
response.BuyOrder;
response.CardNumber;
response.AccountingDate;
response.TransactionDate;
detail = response.Details[0];
detail.AuthorizationCode;
detail.PaymentCodeType;
detail.ResponseCode;
detail.InstallmentsAmount;
detail.InstallmentsNumber;
detail.Amount;
detail.CommerceCode;
detail.BuyOrder;
detail.Status;
detail.Balance;
```

```ruby
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details[0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
```

```python
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details[0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
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
      "commerce_code": "597055555552",
      "buy_order": "505479072"
    }
  ]
}
```

```javascript
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details[0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra del mall. Largo máximo: 26
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accounting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMYY
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. formato: ISO8601
details  <br> <i> Array </i> | Lista con resultado de cada una de las transacciones enviadas.
details [].amount  <br> <i> Number </i> | Monto de la transacción. Largo máximo: 17 <br> <i> Acepta decimales en caso de ser operación en dolares </i>
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 2
details [].payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés. <br> VD = Venta Débito. <i> (Próximamente) </i> <br> VP = Venta Prepago. <i> (Próximamente) </i>
details [].responseCode  <br> <i> Number </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br>
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 12
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 26
prepaid_balance <br> <i> Number </i> | Saldo de la tarjeta de prepago. Se envía solo si se informa saldo. <br> <i> Largo máximo 17 </i>

### Consultar estado de una Transacción Completa Mall

Esta operación permite obtener el estado de la transacción Completa Mall en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

<strong>Transaction.status() Completa Mall</strong>

Obtiene resultado de transacción a partir de un token.

```java
MallFullTransaction.Transaction.status(token)
```

```php
use Transbank\TransaccionCompleta\Transaction;

$response = MallTransaction::status(token)
```

```csharp
using Transbank.Webpay.TransaccionCompletaMall;

MallFullTransaction.Status(token);
```

```ruby
Transbank::TransaccionCompleta::MallTransaction::status(token)
```

```python
Transaction.status(token)
```

```http
GET /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

```javascript
const response = await TransaccionCompleta.MallTransaction.status(token);
```

<strong>Parámetros Transaction.status Completa Mall</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

<strong>Respuesta Transaction.status Completa Mall</strong>

```java
response.getBuyOrder();
response.getCardNumber();
response.getAccountingDate();
response.getTransactionDate();
Detail detail = response.getDetails()[0];
detail.getAuthorizationCode();
detail.getPaymentCodeType();
detail.getResponseCode();
detail.getInstallmentsAmount();
detail.getInstallmentsNumber();
detail.getAmount();
detail.getCommerceCode();
detail.getBuyOrder();
detail.getStatus();
detail.getBalance();
```

```php
$response.getBuyOrder();
$response.getCardNumber();
$response.getAccountingDate();
$response.getTransactionDate();
$detail = response.getDetails()[0];
$detail.getAuthorizationCode();
$detail.getPaymentCodeType();
$detail.getResponseCode();
$detail.getInstallmentsAmount();
$detail.getInstallmentsNumber();
$detail.getAmount();
$detail.getCommerceCode();
$detail.getBuyOrder();
$detail.getStatus();
$detail.getBalance();
```

```csharp
response.BuyOrder;
response.CardNumber;
response.AccountingDate;
response.TransactionDate;
detail = response.Details[0];
detail.AuthorizationCode;
detail.PaymentCodeType;
detail.ResponseCode;
detail.InstallmentsAmount;
detail.InstallmentsNumber;
detail.Amount;
detail.CommerceCode;
detail.BuyOrder;
detail.Status;
detail.Balance;
```

```ruby
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details[0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
```

```python
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details[0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
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
      "commerce_code": "597055555552",
      "buy_order": "505479072"
    }
  ]
}
```
```javascript
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details[0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra del mall. Largo máximo: 26
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accouting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: ISO8601
details  <br> <i> Array </i> | Lista con resultado de cada una de las transacciones enviadas.
details [].amount  <br> <i> Number </i> | Monto de la transacción. Largo máximo: 17 <br> <i> Acepta decimales en caso de ser operación en dolares </i>
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
details [].payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés. <br> VD = Venta Débito. <i> (Próximamente) </i> <br> VP = Venta Prepago. <i> (Próximamente) </i>
details [].responseCode  <br> <i> Number </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br>
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 12
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 26
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17
prepaid_balance <br> <i> Number </i> | Saldo de la tarjeta de prepago. Se envía solo si se informa saldo. <br> <i> Largo máximo 17 </i>

### Anulación Transacción Completa Mall

Permite generar el reembolso del total o parte del monto de una transacción completa.
Dependiendo de la siguiente lógica de negocio la invocación a esta operación generará una reversa o una anulación:

* Si se especifica un valor en el campo “amount” se ejecutará siempre una anulación.
* Si se supera el tiempo máximo para ejecutar una reversa se ejecutará una anulación.
* Si no se ha dado ninguno de los casos anteriores se ejecutará una reversa.

<strong>Transaction.refund() Completa Mall</strong>

Permite solicitar a Webpay la anulación de una transacción realizada previamente y que se encuentre vigente.

```java
final MallFullTransactionRefundResponse response = MallFullTransaction.Transaction.refund(
  token, amount, commerceCode, buyOrder
);
```

```php
use Transbank\TransaccionCompleta\Transaction;

MallTransaction::refund(
  token,
  child_buy_order,
  child_commerce_code,
  amount
);
```

```csharp
using Transbank.Webpay.TransaccionCompletaMall;

MallRefundRequest(
  token,
  childBuyOrder,
  childCommerceCode,
  amount
);
```

```ruby
Transbank::TransaccionCompleta::MallTransaction::refund(
  token: token,
  child_buy_order: child_buy_order,
  child_commerce_code: child_commerce_code,
  amount: amount
)
```

```python
from transbank.transaccion_completa_mall.transaction import Transaction

Transaction.refund(
  token=token, amount=amount, child_commerce_code=child_commerce_code, child_buy_order=child_buy_order
)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/refunds
Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "buy_order": "415034240",
  "commerce_code": "597055555552",
  "amount": 1000
}
```

```javascript
const refundResponse = await TransaccionCompleta.MallTransaction.refund(		
  token,	
  buyOrder,		
  commerceCode,
  amount		
);
```

<strong>Parámetros Transaction.refund Completa Mall</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
commerce_code  <br> <i> Number </i> | Tienda hija que realizó la transacción. Largo: 12.
amount  <br> <i> Formato número entero para transacciones en peso. Sólo en caso de dólar acepta dos decimales. </i> |  Monto que se desea anular o reversar de la transacción. Largo máximo: 17

<strong>Respuesta Transaction.refund Completa Mall</strong>

```java
response.getType();
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getNullifiedAmount();
response.getBalance();
response.getResponseCode();
```

```php
response->getType();
response->getAuthorizationCode();
response->getAuthorizationDate();
response->getNullifiedAmount();
response->getBalance();
response->getResponseCode();
```

```csharp
response.Type;
response.AuthorizationCode;
response.AuthorizationDate;
response.NullifiedAmount;
response.Balance;
response.ResponseCode;
```

```ruby
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
```

```python
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
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
type  <br> <i> String </i> | Tipo de reembolso (REVERSE o NULLIFY). Si es REVERSE no se devolverán datos de la transacción. Largo máximo: 10
authorization_code  <br> <i> String </i> | Código de autorización de la anulación. Largo máximo: 6. Solo viene en caso de anulación.
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización. Solo viene en caso de anulación.
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17. Solo viene en caso de anulación.
balance  <br> <i> Decimal </i> | Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17. Solo viene en caso de anulación.
response_code <br> <i> Number </i> | Código del resultado del pago. Si es exitoso es 0, de lo contrario el pago no fue realizado. Largo máximo: 2. Solo viene en caso de anulación.

## Captura Diferida mall

Los comercios que están configurados para operar con captura diferida deben ejecutar el método de captura para realizar el cargo al tarjetahabiente.

**Válido para :**

* Webpay Plus Captura Diferida
* Transacción Completa Captura Diferida

### Ejecutar captura diferida mall

Puedes [leer más sobre la captura en la información del
producto Webpay](/producto/webpay#autorizacion-y-captura)
para conocer más detalles y restricciones.

Para realizar esa captura explícita debe usarse el método `Transaction.capture()`

<strong>Transaction.capture()</strong>

> Los SDKs permiten indicar opcionalmente el código de comercio de la
> transacción a capturar, para soportar la captura en comercios Webpay Plus
> Mall o Transacción Completa Mall. En comercios sin modalidad Mall no es necesario especificar el código
> de comercio, ya que se usa el indicado en la configuración.


<aside class="notice">
El método `Transaction.capture()` debe ser invocado siempre indicando el código del
comercio que realizó la transacción. En el caso de comercios modalidad Mall,
el código debe ser el código de la tienda virtual específica.
</aside>


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

```javascript
const response = TransaccionCompleta.MallDeferredTransaction.capture(
  token, commerceCode, buyOrder, authorizationCode, amount
);
```

<strong>Parámetros Transaction.capture</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
commerce_code  <br> <i> Number </i> | (Opcional, solo usar en caso Mall) Tienda hija que realizó la transacción. Largo: 12.
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere capturar Largo máximo: 6.
capture_amount  <br> <i> Decimal </i> | Monto que se desea capturar. Largo máximo: 17.

<strong>Respuesta Transaction.capture</strong>

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

```javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
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

Código  | Descripción
------  | -----------
304     | Validación de campos de entrada nulos
245     | Código de comercio no existe
22      | El comercio no se encuentra activo
316     | El comercio indicado no corresponde al certificado o no es hijo del comercio Mall en caso de transacciones MALL
308     | Operación no permitida
274     | Transacción no encontrada
16      | La transacción no es de captura diferida
292     | La transacción no está autorizada
284     | Periodo de captura excedido
310     | Transacción reversada previamente
309     | Transacción capturada previamente
311     | Monto a capturar excede el monto autorizado
315     | Error del autorizador

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

Puedes revisar el proceso necesario para operar en el ambiente de producción en [la documentación](/documentacion/como_empezar#puesta-en-produccion)

### Configuración para producción utilizando los SDK

Si estas utilizando algún SDK oficial de Transbank, entonces debes seguir los siguientes pasos.

<aside class="warning">
Nunca dejes tu código de comercio y secreto compartido directamente en tu código, te recomendamos utilizar variables de entorno u otro método que te permita mantener tus credenciales seguras.
</aside>

Revisa [esta sección](/documentacion/como_empezar#b-utilizando-los-sdk) de la documentación para ver el código necesario para configurar tu propio código de comercio y Api Key Secret. 

