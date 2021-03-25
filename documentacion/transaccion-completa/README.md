# Transacción Completa

<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa' tbk-link-name='Referencia API'></div>
</div>

Una Transacción Completa permite al comercio presentar al tarjetahabiente un
formulario propio para almacenar los datos de la tarjeta, fecha de vencimiento
y cvv (no necesario para comercios con la opción `sin cvv` habilitada).

<aside class="warning">
Este producto tiene requerimientos comerciales más estrictos que el resto de los productos.  
No inicies la integración si aún no completan la afiliación comercial.
</aside>

## Transacción Completa
### Crear una transacción
<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#crear-una-transaccion-completa' tbk-link-name='Referencia API'></div>
</div>

Esta operación te permite iniciar o crear una transacción, Transbank procesa el requerimiento y entrega
como resultado de la operación el token de la transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

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

```javascript
const TransaccionCompleta = require('transbank-sdk').TransaccionCompleta; // CommonJS
import { TransaccionCompleta } from 'transbank-sdk'; // ES6 Modules

const buyOrder = 'Orden de compra de la transaccion';
const sessionId = 'Identificador del servicio unico de transacción';
const amount = 10000; // Monto en CLP
const cardNumber = 'Numero de Tarjeta';
const cvv = 123; // CVV de la tarjeta, parametro opcional si tu código de comercio es sin CVV, en cuyo caso debes enviar null o undefined
const cardExpirationDate = '21/12'; // Fecha de expiración en formato AA/MM

const response = await TransaccionCompleta.Transaction.create(
  buyOrder, sessionId, amount, cvv, cardNumber, cardExpirationDate
);
```

<strong>Respuesta Transaction.create</strong>

<div class="language-simple" data-multiple-language></div>

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

```javascript
response.token
```

### Consulta de cuotas
<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#cconsulta-de-cuotas' tbk-link-name='Referencia API'></div>
</div>

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

```javascript
const TransaccionCompleta = require('transbank-sdk').TransaccionCompleta; // CommonJS
import { TransaccionCompleta } from 'transbank-sdk'; // ES6 Modules

const installmentsNumber = 10; // Número de Cuotas
const response = await TransaccionCompleta.Transaction.installments(token, installmentsNumber);
```
<strong>Respuesta consulta de cuotas</strong>

<div class="language-simple" data-multiple-language></div>

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

### Confirmar una transacción
<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#confirmacion-de-la-transaccion' tbk-link-name='Referencia API'></div>
</div>

Una vez obtenido la respuesta de la consulta de cuotas, con los datos de esta se puede realizar la 
confirmación de la transaccion utilizando el metodo `commit`

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
token = "token obtenido como respuesta de la creacion de transaccion"
id_query_installments = 12345679 # numero identificador de las cuotas.
deferred_period_index = 1
grace_period = false

response = Transbank::TransaccionCompleta::Transaction::commit(
  token: token,
  id_query_installments: id_query_installments,
  deferred_period_index:deferred_period_index,
  grace_period: grace_period
)
```

```python
from transbank.transaccion_completa.transaction import Transaction

#token obtenido como respuesta de la creacion de transaccion
token = "token obtenido como respuesta de la creacion de transaccion"
id_query_installments = 12345679 # numero identificador de las cuotas.
deferred_period_index = 1
grace_period = 'false'

response = Transaction.commit(token=token,
                          id_query_installments=id_query_installments,
                          deferred_period_index=deferred_period_index,
                          grace_period=grace_period)

```

```javascript
const TransaccionCompleta = require('transbank-sdk').TransaccionCompleta; // CommonJS
import { TransaccionCompleta } from 'transbank-sdk'; // ES6 Modules

const token = "token obtenido como respuesta de la creacion de transaccion";
const idQueryInstallments = 123456789 // Número identificador de las cuotas
const deferredPeriodIndex = 1;
const gracePeriod = false;

const response = await TransaccionCompleta.Transaction.commit(
  token, idQueryInstallments, deferredPeriodIndex, gracePeriod
);
```

<strong>Respuesta confirmación</strong>

<div class="language-simple" data-multiple-language></div>

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


### Obtener estado de una transacción
<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#consultar-estado-de-una-transaccion-completa' tbk-link-name='Referencia API'></div>
</div>

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

Este método puede ser invocado los 7 días siguientes luego de realizada la transacción. Después de esto no será posible.

<div class="language-simple" data-multiple-language></div>

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

```javascript
const TransaccionCompleta = require('transbank-sdk').TransaccionCompleta; // CommonJS
import { TransaccionCompleta } from 'transbank-sdk'; // ES6 Modules

const response = await TransaccionCompleta.Transaction.status(token);
```

<strong>Respuesta estado</strong>

<div class="language-simple" data-multiple-language></div>

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

### Reversar o anular una transacción
<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#reversar-o-anular-un-pago-transaccion-completa' tbk-link-name='Referencia API'></div>
</div>

Este método permite a todo comercio habilitado reversar o anular una transacción
completa. El método permite generar el reembolso del total o parte del monto de
una transacción dependiendo de la siguiente lógica de negocio la invocación a
esta operación generará una reversa o una anulación:

* Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.
* Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.

La anulación puede realizarse máximo 90 días después de la fecha de la
transacción original.

Para anular una transacción se debe invocar al método `Transaction.refund()`.

Permite generar el reembolso del total o parte del monto de una transacción completa.
Dependiendo de la siguiente lógica de negocio la invocación a esta operación generará una reversa o una anulación:

* Si se especifica un valor en el campo “amount” se ejecutará siempre una anulación.
* Si se supera el tiempo máximo para ejecutar una reversa se ejecutará una anulación.
* Si no se ha dado ninguno de los casos anteriores se ejecutará una reversa.

<div class="language-simple" data-multiple-language></div>

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

```javascript
const TransaccionCompleta = require('transbank-sdk').TransaccionCompleta; // CommonJS
import { TransaccionCompleta } from 'transbank-sdk'; // ES6 Modules

const response = await TransaccionCompleta.Transaction.refund(token, amount);
```

<strong>Respuesta reembolso</strong>

<div class="language-simple" data-multiple-language></div>

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

```javascript
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
```

### Captura diferida Transacción completa
<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#ejecutar-captura-diferida-transaccion-completa' tbk-link-name='Referencia API'></div>
</div>

Este método permite a todo comercio habilitado realizar capturas de una
transacción autorizada sin captura generada en Transacción Completa.
El método contempla una única captura por cada autorización. Para ello se
deberá indicar los datos asociados a la transacción de venta con autorización
sin captura y el monto requerido para capturar el cual debe ser menor o igual al
monto originalmente autorizado.

Para capturar una transacción, ésta debe haber sido creada por un código de
comercio configurado para captura diferida. De esa forma la transacción estará
autorizada pero requerirá una captura explícita posterior para confirmar la
transacción.

Puedes [leer más sobre la captura en la información del
producto Webpay](/producto/webpay#autorizacion-y-captura)
para conocer más detalles y restricciones.

Para realizar esa captura explícita debe usarse el método `DeferredTransaction.capture()`

<div class="language-simple" data-multiple-language></div>

```java
// Aun no está disponible en este SDK
```

```php
// Aun no está disponible en este SDK
```

```csharp
// Aun no está disponible en este SDK
```

```ruby
# Aun no está disponible en este SDK
```

```python
# Aun no está disponible en este SDK
```

```javascript
const response = TransaccionCompleta.DeferredTransaction.capture(
  token, buyOrder, authorizationCode, amount
);
```

<strong>Respuesta captura</strong>

```java
// Aun no está disponible en este SDK
```

```php
// Aun no está disponible en este SDK
```

```csharp
// Aun no está disponible en este SDK
```

```ruby
# Aun no está disponible en este SDK
```

```python
# Aun no está disponible en este SDK
```

```javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
```

## Transaccción Completa Mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#transaccion-completa-mall' tbk-link-name='Referencia API'></div>
</div>

Una transacción Mall corresponde a una solicitud de autorización financiera de un conjunto de pagos con tarjetas de crédito, en donde quién realiza el pago ingresa al sitio del comercio, selecciona productos o servicios, y el ingreso asociado a los datos de la tarjeta de crédito lo realiza una única vez para el conjunto de pagos. 
Cada pago tendrá su propio resultado, autorizado o rechazado.

![Desagregación de un pago Mall](/images/pago-webpay-mall.png)

Es la tienda Mall la que agrupa múltiples tiendas, son estas últimas las que pueden
generar transacciones. Tanto el mall como las tiendas asociadas son
identificadas a través de un número denominado código de comercio.

### Flujo

El flujo de Transaccion Completa Mall es en general el mismo que el de Transaccion Completa
tanto de cara al tarjeta habiente como de cara al integrador.

Las diferencias son:

* Se debe usar un código de comercio configurado para modalidad Mall en
  Transbank, el cual debe ser indicado al iniciar la transacción.
* Se pueden indicar múltiples transacciones, cada una asociada a un código de
  comercio de tienda (que debe estar configurada en Transbank como perteneciente
  al mall).
* Se debe verificar por separado el resultado de cada una de esas transacciones
  individualmente, pues es posible que el emisor de la tarjeta autorice algunas
  y otras no.

### Crear una transacción mall
<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#crear-una-transaccion-completa-mall' tbk-link-name='Referencia API'></div>
</div>

Esta operación te permite iniciar o crear varias transacciones de una sola vez, Transbank procesa el requerimiento y entrega
como resultado de la operación el token de la transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

<div class="language-simple" data-multiple-language></div>

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

<strong>Respuesta creación</strong>

<div class="language-simple" data-multiple-language></div>

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

```javascript
response.token
```

### Consulta de cuotas mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#consulta-de-cuotas-completa-mall' tbk-link-name='Referencia API'></div>
</div>

Para consultar el valor de las cuotas que pagará el tarjeta habiente en cada transacción dentro transacción completa mall, es necesario llamar al método `Transaction.installments()`

Operación que permite obtener el monto de la cuota a partir del número de cuotas.
El id de la consulta que selecciona el tarjetahabiente debe ser informado en la
invocación de la confirmación.

<div class="language-simple" data-multiple-language></div>

```java
MallFullTransactionInstallmentsDetails installmentsDetails = 
  MallFullTransactionInstallmentsDetails.build().add(commerceCode, buyOrder, installmentsNumber);
final MallFullTransactionInstallmentsResponse response = 
  MallFullTransaction.Transaction.installment(token, installmentsDetails);
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

```javascript
// Installments Number == Número de cuotas
const details = [
  new InstallmentDetail(childCommerceCode, childBuyOrder, installmentsNumber),
  new InstallmentDetail(childCommerceCode2, childBuyOrder2, installmentsNumber2)
  ];		

const installmentsResponse = await TransaccionCompleta.MallTransaction.installments(		
  token,		
  details
);
```

### Confirmar una transacción mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#confirmacion-de-la-transaccion-completa-mall' tbk-link-name='Referencia API'></div>
</div>

Una vez iniciada la transacción y consultado el monto de las cuotas por cada subtransacción, puedes confirmar y obtener el resultado de una transacción completa usando el metodo `Transaction.commit()`.

Es una operación que permite confirmar una transacción. Retorna el estado de la
transacción.

<div class="language-simple" data-multiple-language></div>

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

<strong>Respuesta confirmación mall</strong>

<div class="language-simple" data-multiple-language></div>

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

### Obtener estado de una transacción mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#consultar-estado-de-una-transaccion-completa-mall' tbk-link-name='Referencia API'></div>
</div>

Esta operación permite obtener el estado de la transacción Completa Mall en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

Obtiene resultado de transacción a partir de un token.

<div class="language-simple" data-multiple-language></div>

```java
MallFullTransaction.Transaction.status(token)
```

```php
use Transbank\TransaccionCompleta\Transaction;

$response = MallTransaction::status(token)
```

```csharp
using Transbank.Webpay.TransaccionCompletaMall;

var response = MallFullTransaction.Status(token);
```

```ruby
response = Transbank::TransaccionCompleta::MallTransaction::status(token)
```

```python
response = Transaction.status(token)
```

```javascript
const response = await TransaccionCompleta.Transaction.status(token);
```

<strong>Respuesta consulta de estado</strong>

<div class="language-simple" data-multiple-language></div>

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

### Reversar o anular una transacción mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#anulacion-transaccion-completa-mall' tbk-link-name='Referencia API'></div>
</div>

Permite generar el reembolso del total o parte del monto de una transacción completa.
Dependiendo de la siguiente lógica de negocio la invocación a esta operación generará una reversa o una anulación:

* Si se especifica un valor en el campo “amount” se ejecutará siempre una anulación.
* Si se supera el tiempo máximo para ejecutar una reversa se ejecutará una anulación.
* Si no se ha dado ninguno de los casos anteriores se ejecutará una reversa.

Permite solicitar a Webpay la anulación de una transacción realizada previamente y que se encuentre vigente.

<div class="language-simple" data-multiple-language></div>

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

```javascript
const refundResponse = await TransaccionCompleta.MallTransaction.refund(		
  token,	
  buyOrder,		
  commerceCode,
  amount		
);
```

<strong>Respuesta reembolso mall</strong>

<div class="language-simple" data-multiple-language></div>

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

```javascript
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
```

### Captura diferida de una transacción mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa#ejecutar-captura-diferida-mall' tbk-link-name='Referencia API'></div>
</div>

Este método permite a todo comercio habilitado realizar capturas de una
transacción autorizada sin captura generada en Transacción Completa.
El método contempla una única captura por cada autorización. Para ello se
deberá indicar los datos asociados a la transacción de venta con autorización
sin captura y el monto requerido para capturar el cual debe ser menor o igual al
monto originalmente autorizado.

Para capturar una transacción, ésta debe haber sido creada por un código de
comercio configurado para captura diferida. De esa forma la transacción estará
autorizada pero requerirá una captura explícita posterior para confirmar la
transacción.

Puedes [leer más sobre la captura en la información del
producto Webpay](/producto/webpay#autorizacion-y-captura)
para conocer más detalles y restricciones.

<div class="language-simple" data-multiple-language></div>

```java
// Aun no está disponible en este SDK
```

```php
// Aun no está disponible en este SDK
```

```csharp
// Aun no está disponible en este SDK
```

```ruby
# Aun no está disponible en este SDK
```

```python
# Aun no está disponible en este SDK
```

```javascript
const response = TransaccionCompleta.MallDeferredTransaction.capture(
  token, commerceCode, buyOrder, authorizationCode, amount
);
```

<strong></strong>

```java
// Aun no está disponible en este SDK
```

```php
// Aun no está disponible en este SDK
```

```csharp
// Aun no está disponible en este SDK
```

```ruby
# Aun no está disponible en este SDK
```

```python
# Aun no está disponible en este SDK
```

```javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
```

## Credenciales y Ambientes

### Ambiente de integración

En el ambiente de integración existen códigos de comercio previamente creados para todos los productos (Webpay Plus,
Oneclick, Transacción Completa, etc), para cada una de sus variaciones (Captura Diferida, Mall, Mall Captura Diferida, etc) y dependiendo de
la moneda que acepten (USD o CLP).

Asegúrate de que estés usando el código de comercio de integración que tenga la misma configuración del producto que contrataste.

Puedes revisar los códigos de comercio del ambiente de integración de todos nuestros productos y variaciones
[en este link](/documentacion/como_empezar#ambiente-de-integracion).

### Configuración SDK

Los SDK vienen preconfigurados para operar con Oneclick Mall captura simultanea. Si necesitas operar con otra modalidad,
como captura diferida, debes configurar explícitamente el [código de comercio que usarás](/documentacion/como_empezar#ambiente-de-integracion).
No es necesario definir el Api Key ya que en este ambiente, todos los productos usan la misma y
ya viene preconfigurada.

```java
MallFullTransaction.setCommerceCode("Pon el Código de Comercio");
```

```php
Transbank\TransaccionCompleta::setCommerceCode("Pon el Código de Comercio");
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

TransaccionCompleta.CommerceCode = "Pon el Código de Comercio";
```

```ruby
Transbank::Webpay::TransaccionCompleta::Base.commerce_code = "Pon el Código de Comercio"
```

```python
from transbank import transaccion_completa as BaseTransaccionCompleta

BaseTransaccionCompleta.commerce_code = "Pon el Código de Comercio"
```

```javascript
// Este SDK posee métodos para configurar las distintas modalidades
TransaccionCompleta.configureForIntegration(commerceCode, apiKey);
TransaccionCompleta.configureTransaccionCompletaForTesting();
TransaccionCompleta.configureTransaccionCompletaNoCvvForTesting();
TransaccionCompleta.configureTransaccionCompletaDeferredForTesting();
TransaccionCompleta.configureTransaccionCompletaDeferredNoCvvForTesting();
TransaccionCompleta.configureTransaccionCompletaMallForTesting();
TransaccionCompleta.configureTransaccionCompletaMallNoCvvForTesting();
TransaccionCompleta.configureTransaccionCompletaMallDeferredForTesting();
TransaccionCompleta.configureTransaccionCompletaMallDeferredNoCvvForTesting();
```

### Apuntar a producción

Antes de operar en el ambiente de producción, debes pasar por un [proceso de validación](/documentacion/como_empezar#el-proceso-de-validacion), luego del cual te entregaremos tu Api Key.  

Si ya tienes tu Api Key, puedes revisar como configurar el SDK para usar este ambiente de producción en [esta sección](/documentacion/como_empezar#configuracion-de-produccion)

## Ejemplos de integración

Ponemos a tu disposición una serie de repositorios en nuestro Github para ayudarte a entender la integración de mejor forma.
Puedes encontrar una lista de [proyectos de ejemplo acá](/documentacion/como_empezar#ejemplos).

<aside class="notice">
Si deseas revisar la documentación anterior (SOAP), puedes revisarla [acá](/documentacion/webpay#transaccion-completa)
</aside>

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
