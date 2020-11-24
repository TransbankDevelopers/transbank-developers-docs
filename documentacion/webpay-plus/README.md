# Webpay Plus

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-plus' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugin/webpay' tbk-link-name='Plugins'></div>
</div>

Webpay Plus permite realizar una solicitud de autorización financiera de un pago con tarjetas de crédito o débito Redcompra en donde quién realiza el pago ingresa al sitio del comercio, selecciona productos o servicio, y el ingreso asociado a los datos de la tarjeta de crédito o débito Redcompra lo realiza en forma segura en Webpay. El comercio que recibe pagos mediante Webpay Plus es identificado mediante un código de comercio.

Es el tipo de transacción mas común, usada para un pago puntual en una tienda simple. Se generará un único cobro para todos los productos o servicios adquiridos por el tarjetahabiente.

Los SDK pueden ser configurados para apuntar a un ambiente por defecto, estos ya vienen apuntados al ambiente de integración. Si quieres apuntar a producción tienes dos opciones; puedes re-configurar el SDK para que apunte a producción utilizando el código de comercio y API Key. O puedes configurar cada llamada a las operaciones siguientes para pasar un objeto `Options` en el cual configuras donde quieres apuntar.

### Flujo en caso de éxito

De cara al tarjetahabiente, el flujo de páginas para la transacción es el
siguiente:

<img class="td_img-night" src="/images/referencia/webpayrest/flujo-paginas-webpayrest.png" alt="Flujo de páginas Webpay Plus">

Desde el punto de vista técnico, la secuencia es la siguiente:

<img class="td_img-night" src="/images/referencia/webpayrest/diagrama-secuencia-webpayrest.png" alt="Diagrama de secuencia Webpay Plus">

1. Una vez seleccionado los bienes o servicios, el tarjetahabiente decide pagar a
   través de Webpay.
2. El comercio inicia una transacción en Webpay.
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
   en la variable `token_ws` (en la versión 1.1 y superior del API la redirección 
   es por GET). El comercio debe implementar la recepción de esta variable.
10. El navegador Web del tarjetahabiente realiza una petición HTTPS al
    sitio del comercio, en base a la redirección generada por Webpay en el
    punto 9.
11. El sitio del comercio recibe la variable `token_ws` e invoca el segundo
    método Web para confirmar y obtener el resultado de la autorización.
    El resultado de la autorización podrá ser consultado posteriormente con la
    variable anteriormente mencionada.
12. Comercio recibe el resultado de la confirmación.

<aside class="warning">
En la versión anterior de WebPay, había que invocar `acknowledgeTransaction()`
para informar a WebPay que se había recibido el resultado la transacción sin
problemas. Ahora no es necesario, ya que ésto se realiza de forma automática
una vez que se confirma la transacción.  Además ya no se debe mostrar el voucher
de Transbank, solo debe mostrarse desde el sitio del comercio.
</aside>

13. Sitio del comercio despliega voucher con los datos de la transacción.

### Flujo si usuario aborta el pago

<img class="td_img-night" src="/images/referencia/webpayrest/diagrama-secuencia-webpayrest-abortar.png" alt="Diagrama de secuencia si usuario aborta el pago">

Si el tarjetahabiente anula la transacción en el formulario de pago de Webpay,
el flujo cambia y los pasos son los siguientes:

1. Una vez seleccionado los bienes o servicios, tarjetahabiente decide pagar a
   través de Webpay.
2. El comercio inicia una transacción en Webpay.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el
   token de la transacción y URL de redireccionamiento a la cual se deberá
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
   HTTPS hacia la página de **retorno del comercio**, en donde se envía por
   método POST el token de la transacción en la variable `TBK_TOKEN` además de las variables `TBK_ORDEN_COMPRA` y `TBK_ID_SESION`.

<aside class="warning">
Nota que el nombre de las variables recibidas es diferente. En lugar de `token_ws` acá el token viene en la variable `TBK_TOKEN`.
</aside>

9. El comercio con la variable `TBK_TOKEN` debe invocar el método
   de confirmación de transacción para obtener el resultado de la autorización. En
   este caso debe obtener una excepción, pues el pago fue abortado.
10. El comercio debe informar al tarjetahabiente que su pago no se completó.

## Crear una transacción

Esta operación te permite iniciar o crear una transacción, Webpay Plus procesa el requerimiento y entrega 
como resultado de la operación el token de la transacción y URL de redireccionamiento a la cual 
se deberá redirigir al tarjetahabiente.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.webpay.webpayplus.WebpayPlus;
import cl.transbank.webpay.webpayplus.model.CreateWebpayPlusTransactionResponse;

final WebpayPlusTransactionCreateResponse response = WebpayPlus.Transaction.create(
  buyOrder, sessionId, amount, returnUrl
);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::create($buy_order, $session_id, $amount, $return_url);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Create(buyOrder, sessionId, amount, returnUrl);
```

```ruby
response = Transbank::Webpay::WebpayPlus::Transaction::create(
  buy_order: buy_order,
  session_id: session_id,
  amount: amount,
  return_url: return_url
)
```

```python
response = transbank.webpay.webpay_plus.create(buy_order, session_id, amount, return_url)
```

**Respuesta**

La respuesta de Webpay Plus a la creación es el token de la transacción y la URL a la cual debes redirigir al tarjetahabiente.

<div class="language-simple" data-multiple-language></div>

```java
response.getUrl();
response.getToken();
```

```php
response.getUrl();
response.getToken();
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
response.url
response.token
```

Con estos datos debes crear un formulario en el cual debes poner un `input` de nombre `token_ws` 
y en su valor debes insertar el token devuelto. El formulario debe usar el método `POST` y su acción (o URL) 
debe ser la URL devuelta por Webpay Plus.

```html
<form method="post" action="Inserta aquí la url entregada">
  <input type="hidden" name="token_ws" value="Inserte aquí el token entregado" />
  <input type="submit" value="Ir a pagar" />
</form>
```

<aside class="warning">
Debido a la posibilidad que el formulario de WebPay no se despliegue, no se
recomienda el uso de Iframe para WebPay Plus.
</aside>

<aside class="notice">
Tip: En el ambiente de integración puedes usar la tarjeta VISA 4051885600446623
para hacer pruebas simples de éxito. El CVV es 123 y la fecha de vencimiento
cualquiera superior a la fecha actual. Luego para la autenticación bancaria usa
el RUT 11.111.111-1 y la clave 123. Para pruebas exhaustivas <a href="https://www.transbankdevelopers.cl/documentacion/como_empezar#ambientes">consulta todas las
tarjetas de prueba en la sección de Ambientes</a>.
</aside>

## Confirmar una transacción

Una vez que el tarjetahabiente ha pagado, Webpay Plus retornará 
el control vía `POST` a la `URL` que indicaste en el `return_url`. 
Recibirás también el parámetro `token_ws` que te permitirá conocer el resultado de la transacción.

En caso de que el tarjetahabiente haya declinado, o haya ocurrido un error, recibirás la variable `TBK_TOKEN` 
además de las variables `TBK_ORDEN_COMPRA` y `TBK_ID_SESION`.

<div class="language-simple" data-multiple-language></div>

```java
final CreateWebpayPlusTransactionResponse response = WebpayPlus.Transaction.commit(token);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::commit($token);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Commit(token);
```

```ruby
response = Transbank::Webpay::WebpayPlus::Transaction::commit(token: @token)
```

```python
response = transbank.webpay.webpay_plus.transaction.commit(token)
```

**Respuesta**

Utilizando la respuesta de la confirmación podrás mostrar un comprobante o página de éxito a tu usuario. 
Con eso habrás completado el flujo "feliz" en que todo funciona.

<div class="language-simple" data-multiple-language></div>

```java
response.getVci();
response.getAmount();
response.getStatus();
response.getBuyOrder();
response.getSessionId();
response.getCardDetail();
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
response->getVci();
response->getAmount();
response->getStatus();
response->getBuyOrder();
response->getSessionId();
response->getCardDetail();
response->getAccountingDate();
response->getTransactionDate();
response->getAuthorizationCode();
response->getPaymentTypeCode();
response->getResponseCode();
response->getInstallmentsAmount();
response->getInstallmentsNumber();
response->getBalance();
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

## Obtener estado de una transacción

Esta operación permite obtener el estado de la transacción en los siguientes 7 días desde su creación. 
En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error 
inesperado permite conocer el estado y tomar las acciones que correspondan.

Debes enviar el `token` dela transacción de la cual desees obtener el estado.

<div class="language-simple" data-multiple-language></div>

```java
final StatusWebpayPlusTransactionResponse response = WebpayPlus.Transaction.status(token);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::getStatus($token);  
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Status(token);
```

```ruby
response = Transbank::Webpay::WebpayPlus::Transaction::status(token: @token)
```

```python
response = transbank.webpay.webpay_plus.transaction.status(token)
```

Para obtener la información contenida en la respuesta puedes hacerlo de la siguiente manera.

<div class="language-simple" data-multiple-language></div>

```java
response.getVci();
response.getAmount();
response.getStatus();
response.getBuyOrder();
response.getSessionId();
response.getCardDetail();
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
response->getVci();
response->getAmount();
response->getStatus();
response->getBuyOrder();
response->getSessionId();
response->getCardDetail();
response->getAccountingDate();
response->getTransactionDate();
response->getAuthorizationCode();
response->getPaymentTypeCode();
response->getResponseCode();
response->getInstallmentsAmount();
response->getInstallmentsNumber();
response->getBalance();
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

## Reversar o Anular una transacción

Esta operación permite a todo comercio habilitado, reembolsar o anular una
transacción que fue generada en Webpay Plus. 
Puedes generar el reembolso del total o parte del monto de una transacción, dependiendo de la 
siguiente lógica de negocio la invocación a esta operación generará una reversa o una anulación:
- Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.
- Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.

La anulación puede realizarse máximo 90 días después de la fecha de la
transacción original.

Puedes [leer más sobre la anulación en la información del
producto Webpay](/producto/webpay#anulaciones) para conocer
más detalles y restricciones.

<aside class="notice">
El método `Transaction.refund()` debe ser invocado siempre indicando el código del comercio que realizó la transacción.
</aside>

<div class="language-simple" data-multiple-language></div>

```java
final RefundWebpayPlusTransactionResponse response = WebpayPlus.Transaction.refund(token, amount);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::refund($token, $amount);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Refund(token, amount);
```

```ruby
response = Transbank::Webpay::WebpayPlus::Transaction::refund(token: @token, amount: @amount)
```

**Respuesta**

Para obtener la información contenida en la respuesta puedes hacerlo de la siguiente manera.

<div class="language-simple" data-multiple-language></div>

```java
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getBalance();
response.getNullifiedAmount();
response.getResponseCode();
response.getType();
```

```php
response->getAuthorizationCode();
response->getAuthorizationDate();
response->getBalance();
response->getNullifiedAmount();
response->getResponseCode();
response->getType();
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
response.authorization_code;
response.authorization_date;
response.balance;
response.nullified_amount;
response.response_code;
response.type;
```

# Webpay Plus Mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-plus' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugin/webpay' tbk-link-name='Plugins'></div>
</div>

Webpay Plus Mall permite realizar una solicitud de autorización financiera de varios pagos con tarjetas de crédito o débito Redcompra en donde quién realiza el pago ingresa al sitio del comercio, selecciona productos o servicio, y el ingreso asociado a los datos de la tarjeta de crédito o débito Redcompra lo realiza en forma segura en Webpay Plus Mall. Los comercios que reciben pagos mediante Webpay Plus Mall son identificados mediante un código de comercio.

Este tipo de pago se utiliza para recibir pagos en distintas tiendas de una sola vez.

Los SDK pueden ser configurados para apuntar a un ambiente por defecto, estos ya vienen apuntados al ambiente de integración. Si quieres apuntar a producción tienes dos opciones; puedes re-configurar el SDK para que apunte a producción utilizando el código de comercio y API Key. O puedes configurar cada llamada a las operaciones siguientes para pasar un objeto `Options` en el cual configuras donde quieres apuntar.

### Flujo Webpay Plus Mall

El flujo de Webpay Plus Mall es en general el mismo que el de [Webpay Plus](#webpay-plus-normal) 
tanto de cara al tarjeta habiente como de cara al integrador.

Las diferencias son:

- Se debe usar un código de comercio configurado para modalidad Mall en
  Transbank, el cual debe ser indicado al iniciar la transacción.
- Se pueden indicar múltiples transacciones, cada una asociada a un código de
  comercio de tienda (que debe estar configurada en Transbank como perteneciente
  al mall).
- Se debe verificar por separado el resultado de cada una de esas transacciones
  individualmente, pues es posible que el emisor de la tarjeta autorice algunas
  y otras no.

## Crear una transacción

Esta operación te permite iniciar o crear varias transacciones de una sola vez, Webpay Plus Mall procesa el requerimiento y entrega 
como resultado de la operación el token de la transacción y URL de redireccionamiento a la cual 
se deberá redirigir al tarjetahabiente.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

<div class="language-simple" data-multiple-language></div>

```java
CreateMallTransactionDetails transactionDetails = CreateMallTransactionDetails.build()
    .add(amountMallOne, commerceCodeMallOne, buyOrderMallOne)
    .add(amountMallTwo, commerceCodeMallTwo, buyOrderMallTwo);

final CreateWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.create(buyOrder, sessionId, returnUrl, transactionDetails);
```

```php
use Transbank\Webpay\WebpayPlus;
use Transbank\Webpay\WebpayPlus\Transaction;

WebpayPlus::configureMallForTesting();

$transaction_details = [
  {
      "amount": 10000,
      "commerce_code": 597055555536,
      "buy_order": "ordenCompraDetalle1234"
  },
  {     
     "amount": 12000,
     "commerce_code": 597055555537,
     "buy_order": "ordenCompraDetalle4321"
  },
];
  
$response = Transaction::createMall($buy_order, $session_id, $return_url, $transaction_details);
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

response = MallTransaction.create(
    buy_order=buy_order,
    session_id=session_id,
    return_url=return_url,
    details = transaction_details,
)
```

<aside class="notice">
Observar que existe un <code>buyOrder</code> generado para el comercio mall y un <code>buyOrder</code> para cada una de las tiendas.
</aside>

**Respuesta**

La respuesta de Webpay Plus Mall a la creación de es el token de la transacción y la URL a la cual debes redirigir al tarjetahabiente.

```java
response.getToken();
response.getUrl();
```

```php
response->getToken();
response->getUrl();
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
response.token
response.url
```

Con estos datos debes crear un formulario en el cual debes poner un `input` de nombre `token_ws` 
y en su valor debes insertar el token devuelto. El formulario debe usar el método `POST` y su acción (o URL) 
debe ser la URL devuelta por Webpay Plus.

```html
<form method="post" action="Inserta aquí la url entregada">
  <input type="hidden" name="token_ws" value="Inserte aquí el token entregado" />
  <input type="submit" value="Ir a pagar" />
</form>
```

<aside class="warning">
Debido a la posibilidad que el formulario de WebPay no se despliegue, no se
recomienda el uso de Iframe para WebPay Plus.
</aside>

<aside class="notice">
Tip: En el ambiente de integración puedes usar la tarjeta VISA 4051885600446623
para hacer pruebas simples de éxito. El CVV es 123 y la fecha de vencimiento
cualquiera superior a la fecha actual. Luego para la autenticación bancaria usa
el RUT 11.111.111-1 y la clave 123. Para pruebas exhaustivas <a href="https://www.transbankdevelopers.cl/documentacion/como_empezar#ambientes">consulta todas las
tarjetas de prueba en la sección de Ambientes</a>.
</aside>

## Confirmar una transacción

Una vez que el tarjetahabiente ha pagado, Webpay Plus retornará 
el control vía `POST` a la `URL` que indicaste en el `return_url`. 
Recibirás también el parámetro `token_ws` que te permitirá conocer el resultado de la transacción.

En caso de que el tarjetahabiente haya declinado, o haya ocurrido un error, recibirás la variable `TBK_TOKEN` 
además de las variables `TBK_ORDEN_COMPRA` y `TBK_ID_SESION`.

<div class="language-simple" data-multiple-language></div>

```java
final CommitWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.commit(token);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::commitMall($token);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = MallTransaction.commit(token);
```

```ruby
response = MallTransaction.commit(token)
```

```python
response = MallTransaction.commit(token)
```

**Respuesta**

Utilizando la respuesta de la confirmación podrás mostrar un comprobante o página de éxito a tu usuario. 
Con eso habrás completado el flujo "feliz" en que todo funciona.

<div class="language-simple" data-multiple-language></div>

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
$card_detail = response->getCardDetail();
$card_detail->getCardNumber();
$response->getSessionId();
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


## Obtener estado de una transacción

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

<div class="language-simple" data-multiple-language></div>

```java
final StatusWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.status(token);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::getMallStatus($token);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = MallTransaction.status(token)
```

```ruby
response = MallTransaction.status(token)
```

```python
response = MallTransaction.status(token)
```

## Reversar o Anular una transacción

Esta operación permite a todo comercio habilitado reversar o anular una transacción
que fue generada en Webpay Plus Mall. El método permite generar el reembolso del
total o parte del monto de una transacción. Dependiendo de la siguiente lógica
de negocio la invocación a esta operación generará una reversa o una anulación:
- Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.
- Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.

La anulación puede realizarse máximo 90 días después de la fecha de la
transacción original.

Puedes [leer más sobre la anulación en la información del
producto Webpay](/producto/webpay#anulaciones) para conocer
más detalles y restricciones.

<div class="language-simple" data-multiple-language></div>

```java
final RefundWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.refund(token, buyOrder, commerceCode, amount);
```

```php
$response = Transaction::refundMall($token, $buy_order, $commerce_code, $amount);
```

```csharp
var response = Transaction.refund(token, buyOrder, commerceCode, amount);
```

```ruby
response = Transaction.refund(token, buy_order, commerce_code, amount)
```

```python
response = Transaction.refund(token, buy_order, commerce_code, amount)
```
