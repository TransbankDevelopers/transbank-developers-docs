# Webpay

## Webpay Plus

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-plus' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugin/webpay' tbk-link-name='Plugins'></div>
</div>

Webpay Plus permite realizar una solicitud de autorización financiera de un pago con tarjetas de crédito o débito Redcompra en donde quién realiza el pago ingresa al sitio del comercio, selecciona productos o servicio, y el ingreso asociado a los datos de la tarjeta de crédito o débito Redcompra lo realiza en forma segura en Webpay Plus. El comercio que recibe pagos mediante Webpay Plus es identificado mediante un código de comercio.

Es el tipo de transacción mas común, usada para un pago puntual en una tienda simple. Se generará un único cobro para todos los productos o servicios adquiridos por el tarjetahabiente.

### Flujo en caso de éxito

De cara al tarjetahabiente, el flujo de páginas para la transacción es el
siguiente:

<img class="td_img-night" src="/images/referencia/webpayrest/flujo-paginas-webpayrest.png" alt="Flujo de páginas Webpay Plus">

Desde el punto de vista técnico, la secuencia es la siguiente:

<img class="td_img-night" src="/images/referencia/webpayrest/diagrama-secuencia-webpayrest.png" alt="Diagrama de secuencia Webpay Plus">

1. Una vez seleccionado los bienes o servicios, el tarjetahabiente decide pagar a
   través de Webpay.
2. El comercio inicia una transacción en Webpay.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el
   token de la transacción y URL de redireccionamiento a la cual se deberá
   redirigir al tarjetahabiente.
4. Comercio redirecciona al tarjetahabiente hacia Webpay, con el token de la
   transacción a la URL indicada en punto 3. La redirección se realiza
   enviando por método POST el token en variable `token_ws`. (En la API 1.1 en adelante, esta redirección es por GET)
5. El navegador Web del tarjetahabiente realiza una petición HTTPS a Webpay, en
   base al redireccionamiento generado por el comercio en el punto 4.
6. Webpay responde al requerimiento desplegando el formulario de pago de Webpay.
   Desde este punto la comunicación es entre Webpay y el tarjetahabiente, sin
   interferir el comercio. El formulario de pago de Webpay despliega, entre
   otras cosas, el monto de la transacción, información del comercio como
   nombre y logotipo, las opciones de pago a través de crédito o débito.
7. Tarjetahabiente ingresa los datos de la tarjeta, hace clic en pagar en
   formulario Webpay. El tiempo en el cual permanece el formulario de Webpay en espera para
   incluir los datos de la tarjeta es de 5 minutos, en caso extender dicho plazo
   y no haber terminado la transacción, esta será abortada automáticamente.
   
    <aside class="warning">
   En caso de que se cumpla el tiempo máximo para completar el formulario, el comercio recibirá las variables `TBK_ID_SESSION` y `TBK_ORDEN_COMPRA`.
    </aside>
8. Webpay procesa la solicitud de autorización (primero autenticación bancaria
   y luego la autorización de la transacción).
9. Una vez resuelta la autorización, Webpay retorna el control al comercio,
   realizando un redireccionamiento HTTPS hacia la página de transición
   del comercio, en donde se envía por método POST el token de la transacción  
   en la variable `token_ws` (en la versión 1.1 y superior del API la redirección
   es por GET). El comercio debe implementar la recepción de esta variable.
10. El navegador Web del tarjetahabiente realiza una petición HTTPS al
    sitio del comercio, en base a la redirección generada por Webpay en el
    punto 9.
11. El sitio del comercio recibe la variable `token_ws` e invoca el segundo
    método Web para confirmar y obtener el resultado de la autorización.
    El resultado de la autorización podrá ser consultado posteriormente con la
    variable anteriormente mencionada.
12. Comercio recibe el resultado de la confirmación.
    <aside class="warning">
    En la versión anterior de Webpay, había que invocar `acknowledgeTransaction()`
    para informar a WebPay que se había recibido el resultado la transacción sin
    problemas. Ahora no es necesario, ya que esto se realiza de forma automática
    una vez que se confirma la transacción.  Además ya no se debe mostrar el voucher
    de Transbank, solo debe mostrarse desde el sitio del comercio.
    </aside>
13. Sitio del comercio despliega voucher con los datos de la transacción.

### Flujo si usuario aborta el pago

Si el tarjetahabiente anula la transacción en el formulario de pago de Webpay,
el flujo cambia y los pasos son los siguientes:

<img class="td_img-night" src="/images/referencia/webpayrest/diagrama-secuencia-webpayrest-abortar.png" alt="Diagrama de secuencia si usuario aborta el pago">

1. Una vez seleccionado los bienes o servicios, tarjetahabiente decide pagar a
   través de Webpay.
2. El comercio inicia una transacción en Webpay.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el
   token de la transacción y URL de redireccionamiento a la cual se deberá
   redirigir al tarjetahabiente.
4. Comercio redirecciona al tarjetahabiente hacia Webpay, con el token de la
   transacción a la URL indicada en punto 3. La redirección se realiza
   enviando por método POST el token en variable `token_ws`.
5. El navegador Web del tarjetahabiente realiza una petición HTTPS a Webpay, en
   base al redireccionamiento generado por el comercio en el punto 4.
6. Webpay responde al requerimiento desplegando el formulario de pago de Webpay.
   Desde este punto la comunicación es entre Webpay y el tarjetahabiente, sin
   interferir el comercio. El formulario de pago de Webpay despliega, entre
   otras cosas, el monto de la transacción, información del comercio como
   nombre y logotipo, las opciones de pago a través de crédito o débito.
7. Tarjetahabiente hace clic en “anular”, en formulario Webpay.
8. Webpay retorna el control al comercio, realizando un redireccionamiento
   HTTPS hacia la página de **retorno del comercio**, en donde se envía por
   método POST el token de la transacción en la variable `TBK_TOKEN` además de las variables `TBK_ORDEN_COMPRA` y `TBK_ID_SESION`.
   
    <aside class="warning">
    Nota que el nombre de las variables recibidas es diferente. En lugar de `token_ws` acá el token viene en la variable `TBK_TOKEN`.
    </aside>
9. El comercio con la variable `TBK_TOKEN` debe invocar el método
   de confirmación de transacción para obtener el resultado de la autorización. En
   este caso debe obtener una excepción, pues el pago fue abortado.
10. El comercio debe informar al tarjetahabiente que su pago no se completó.

### Resumen de flujos
A la URL de `return_url` siempre se llega por POST, aunque desde la versión 1.1 del API, en adelante, la redirección es por GET.
Para resumir los diferentes flujos que pueden existir, y las diferentes respuestas que se pueden esperar:
Hay 4 diferentes flujos, donde cada uno llega con datos distintos:

1. **Flujo normal**: El usuario al finalizar la transacción (tanto si es un rechazo o una aprobación) llegará solamente `token_ws`. 
2. **Timeout (más de 10 minutos en el formulario de Transbank)**: Llegará solamente `TBK_ID_SESION` que contiene el `session_id` enviado al crear la transacción, `TBK_ORDEN_COMRA` que representa el `buy_order` enviado. No llegará token. 
3. **Pago abortado (con botón anular compra en el formulario de Webpay)**: Llegará `TBK_TOKEN` (notar que no se llama `token_ws`, pero igualmente contiene el token de la transacción), `TBK_ID_SESION`, `TBK_ORDEN_COMRA`
4. **Si ocurre un error en el formulario de pago, y hace click en el link de "volver al sitio" de la pantalla de error*: (replicable solo en producción si inicias una transacción, abres el formulario de pago, cierras el tab de Chrome y luego lo recuperas) Llegará `token_ws`, `TBK_TOKEN`, `TBK_ID_SESION`, `TBK_ORDEN_COMRA`. 


### Crear una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#crear-una-transaccion' tbk-link-name='Referencia Api'></div>
</div>

Esta operación te permite iniciar o crear una transacción, Webpay Plus procesa el requerimiento y entrega
como resultado de la operación el token de la transacción y URL de redireccionamiento a la cual
se deberá redirigir al tarjetahabiente.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
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

// Versión 2.x del SDK
$transaction = new Transaction();
$response = $transaction->create($buy_order, $session_id, $amount, $return_url);

// Adicionalmente, ahora existen varias formas de crear una instancia del Objeto "Transaction". Todas son igual de válidas.    
// Opción 2: 
$response = WebpayPlus::transaction()->create($buy_order, $session_id, $amount, $return_url);

//Opción 3: Usar el método estático build()
$response = Transaction::build()->create($buy_order, $session_id, $amount, $return_url);


// En el fondo, se puede ver que se puede crear una instancia así, que funcionan exactamente igual: 
$transaction = new Transaction(); // A
$transaction = Transaction::build(); // B
$transaction = WebpayPlus::transaction(); // C

////////////////////////////////////////////////////////////
// Configuración del Ambiente y Credenciales
////////////////////////////////////////////////////////////

// Por defecto, la clase Transaction viene configurada para integración con un código Webpay Plus Captura simultanea
// También se puede configurar WebpayPlus::configureForTesting() para captura simultanea y  WebpayPlus::configureForTestingDeferred() para captura diferida


// Para configurar el ambiente y Api Key, también existen varias opciones:
// Opción A: 
$transaction->configureForIntegration($commerceCode, $apiKeySecret);
$transaction->configureForProduction($commerceCode, $apiKeySecret);

// Opción B
WebpayPlus::configureForIntegration($commerceCode, $apiKeySecret);
WebpayPlus::configureForProduction($commerceCode, $apiKeySecret);
// Crear instancia de Transaction con cualquier método. 

// Opción C: Crear objeto Options
$options = Transbank\Webpay\Options::forIntegration($commerceCode, $apiKeySecret);
$transaction = new Transaction($options); //Pasarle el objeto $options a la instancia

// -------------------
// SDK 1.X - No hay más opciones. Solo método estático, no recomendado. 
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

```javascript
const response = await WebpayPlus.Transaction.create(buyOrder, sessionId, amount, returnUrl);
```

<strong>Respuesta Crear una transacción</strong>

La respuesta de Webpay Plus a la creación es el token de la transacción y la URL a la cual debes redirigir al tarjetahabiente.

<div class="language-simple" data-multiple-language></div>

```java
response.getUrl();
response.getToken();
```

```php
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
response.url
response.token
```

```javascript
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

### Confirmar una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#confirmar-una-transaccion' tbk-link-name='Referencia Api'></div>
</div>

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


// SDK Versión 2.x
$response = (new Transaction)->commit($token); // O cualquiera de los métodos detallados en el ejemplo anterior del método create.

// SDK Versión 1.x
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

```javascript
const response = await WebpayPlus.Transaction.commit(token);
```

<strong>Respuesta Confirmar una transacción</strong>

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
$response->getVci();
$response->getAmount();
$response->getStatus();
$response->getBuyOrder();
$response->getSessionId();
$response->getCardDetail();
$response->getCardNumber(); // Solo en SDK 2.x
$response->getAccountingDate();
$response->getTransactionDate();
$response->getAuthorizationCode();
$response->getPaymentTypeCode();
$response->getResponseCode();
$response->getInstallmentsAmount();
$response->getInstallmentsNumber();
$response->getBalance();

// Adicionalmente en el SDK 2.x existe
$response->isApproved(); // Devuelve true si response_code es 0 y status es AUTHORIZED, y false en caso contrario.
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

### Obtener estado de una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#obtener-estado-de-una-transaccion' tbk-link-name='Referencia Api'></div>
</div>

Esta operación permite obtener el estado de la transacción en los siguientes 7 días desde su creación.
En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error
inesperado permite conocer el estado y tomar las acciones que correspondan.

Debes enviar el `token` dela transacción de la cual desees obtener el estado.

<div class="language-simple" data-multiple-language></div>

```java
final StatusWebpayPlusTransactionResponse response = WebpayPlus.Transaction.status(token);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

// SDK 2.x
$response = (new Transaction)->status($token);

// SDK 1.x
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

```javascript
const response = await WebpayPlus.Transaction.status(token);
```

<strong>Respuesta estado de una transacción</strong>

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
$response->getVci();
$response->getAmount();
$response->getStatus();
$response->getBuyOrder();
$response->getSessionId();
$response->getCardDetail();
$response->getCardNumber(); // Solo SDK 2.x
$response->getAccountingDate();
$response->getTransactionDate();
$response->getAuthorizationCode();
$response->getPaymentTypeCode();
$response->getResponseCode();
$response->getInstallmentsAmount();
$response->getInstallmentsNumber();
$response->getBalance();


// Adicionalmente en el SDK 2.x existe: 
$response->isApproved(); // Devuelve true si response_code es 0 y status es AUTHORIZED, y false en caso contrario.
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

### Reversar o Anular una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#reversar-o-anular-un-pago' tbk-link-name='Referencia Api'></div>
</div>

Esta operación permite a todo comercio habilitado, reembolsar o anular una
transacción que fue generada en Webpay Plus.
Puedes generar el reembolso del total o parte del monto de una transacción, dependiendo de la
siguiente lógica de negocio la invocación a esta operación generará una reversa o una anulación:

* Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.
* Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.

La anulación puede realizarse máximo 90 días después de la fecha de la
transacción original.

Puedes [leer más sobre la anulación en la información del
producto Webpay](/producto/webpay#anulaciones) para conocer
más detalles y restricciones.

<aside class="notice">
El método `Transaction.refund()` debe ser invocado siempre indicando el código del comercio que realizó la transacción.
</aside>

<div class="language-simple" data-multiple-language></div>

```java
final RefundWebpayPlusTransactionResponse response = WebpayPlus.Transaction.refund(token, amount);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;
// SDK 2.x
$response = (new Transaction)->refund($token, $amount);

// SDK 1.x
$response = Transaction::refund($token, $amount);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Refund(token, amount);
```

```ruby
response = Transbank::Webpay::WebpayPlus::Transaction::refund(token: @token, amount: @amount)
```

```javascript
const response = await WebpayPlus.Transaction.refund(token, amount);
```

<strong>Respuesta Reversa o Anulación</strong>

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

### Capturar una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#capturar-una-transaccion' tbk-link-name='Referencia Api'></div>
</div>

Los comercios que están configurados para operar con captura diferida deben ejecutar el método de captura para realizar el cargo al tarjetahabiente.

Este método permite a todo comercio habilitado realizar capturas de una
transacción autorizada sin captura generada en Webpay Plus. 
El método contempla una única captura por cada autorización. Para ello se
deberá indicar los datos asociados a la transacción de venta con autorización
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

<strong>Transaction.capture()</strong>


> Los SDKs permiten indicar opcionalmente el código de comercio de la
> transacción a capturar, para soportar la captura en comercios Webpay Plus
> Mall o Transacción Completa Mall. En comercios sin modalidad Mall no es necesario especificar el código
> de comercio, ya que se usa el indicado en la configuración.

<aside class="notice">
El método `Transaction.capture()` debe ser invocado siempre indicando el código del
comercio que realizó la transacción. En el caso de comercios modalidad Mall,
el código debe ser el código de la tienda virtual específica.
</aside>

<div class="language-simple" data-multiple-language></div>

```java
final CaptureWebpayPlusTransactionResponse response = WebpayPlus.DeferredTransaction.capture(token, buyOrder, authorizationCode, amount);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

// SDK 2.x
$response = (new Transaction)->capture($token, $buyOrder, $authCode, $amount);

// SDK 1.x
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
response = DeferredTransaction.capture(
  token=token, buy_order=buy_order, authorization_code=authorization_code, capture_amount=amount
)
```

```javascript
const response = await WebpayPlus.DeferredTransaction.capture(token, buyOrder, authorizationCode, captureAmount);
```

Una vez realizada la captura, recibirás un objeto con la respuesta. Revisa la [referencia API](/referencia/webpay) para conocer más detalles 
sobre los posibles resultados.  

<strong>Respuesta de una captura</strong>

<div class="language-simple" data-multiple-language></div>

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

## Webpay Plus Mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-plus-mall' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugin/webpay' tbk-link-name='Plugins'></div>
</div>

Una transacción Mall Normal corresponde a una solicitud de autorización financiera de un conjunto de pagos con tarjetas de crédito o débito, en donde quién realiza el pago ingresa al sitio del comercio, selecciona productos o servicios, y el ingreso asociado a los datos de la tarjeta de crédito o débito lo realiza una única vez en forma segura en Webpay para el conjunto de pagos. Cada pago tendrá su propio resultado, autorizado o rechazado.

![Desagregación de un pago Webpay Mall](/images/pago-webpay-mall.png)

Es la tienda Mall la que agrupa múltiples tiendas, son estas últimas las que pueden
generar transacciones. Tanto el mall como las tiendas asociadas son
identificadas a través de un número denominado código de comercio.

### Flujo

El flujo de Webpay Plus Mall es en general el mismo que el de [Webpay Plus](#webpay-plus-normal)
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
  <div tbk-link='/referencia/webpay#crear-una-transaccion-mall' tbk-link-name='Referencia Api'></div>
</div>

Esta operación te permite iniciar o crear varias transacciones de una sola vez, Webpay Plus Mall procesa el requerimiento y entrega
como resultado de la operación el token de la transacción y URL de redireccionamiento a la cual
se deberá redirigir al tarjetahabiente.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
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
use Transbank\Webpay\WebpayPlus\MallTransaction;
use Transbank\Webpay\WebpayPlus; // opcional
// SDK 2.x
// Por defecto, la clase MallTransaction viene configurada para integración con un código Webpay Plus Mall Captura simultanea
// También se puede configurar WebpayPlus::configureForTestingMall() y  WebpayPlus::configureForTestingMallDeferred()
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

// SDK 1.x
use Transbank\Webpay\WebpayPlus;
use Transbank\Webpay\WebpayPlus\Transaction;

WebpayPlus::configureMallForTesting();

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

```javascript
const TransactionDetail = require("transbank-sdk").TransactionDetail;

let details = [
  new TransactionDetail(
    amount, commerceCode, buyOrder
  ),
  new TransactionDetail(
    amount2, commerceCode2, buyOrder2
  ),
];

const createResponse = await WebpayPlus.MallTransaction.create(
  buyOrder,
  sessionId,
  returnUrl,
  details
);
```

<aside class="notice">
Observar que existe un <code>buyOrder</code> generado para el comercio mall y un <code>buyOrder</code> para cada una de las tiendas.
</aside>

<strong>Respuesta Crear una transacción mall</strong>

La respuesta de Webpay Plus Mall a la creación de es el token de la transacción y la URL a la cual debes redirigir al tarjetahabiente.

<div class="language-simple" data-multiple-language></div>

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
response.token
response.url
```

```javascript
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

### Confirmar una transacción mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#confirmar-una-transaccion-mall' tbk-link-name='Referencia Api'></div>
</div>

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
// SDK 2.x
use Transbank\Webpay\WebpayPlus\MallTransaction;
$response = (new MallTransaction)->commit($token);

//SDK 1.x
use Transbank\Webpay\WebpayPlus\Transaction;
$response = Transaction::commitMall($token);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = MallTransaction.commit(token);
```

```ruby
response = MallTransaction.commit(token: token)
```

```python
response = MallTransaction.commit(token)
```

```javascript
const response = await WebpayPlus.MallTransaction.commit(token);
```

<strong>Respuesta Confirmar una transacción mall</strong>

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

### Obtener estado de una transacción mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#obtener-estado-de-una-transaccion-mall' tbk-link-name='Referencia Api'></div>
</div>

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

<div class="language-simple" data-multiple-language></div>

```java
final StatusWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.status(token);
```

```php
// SDK 2.x
use Transbank\Webpay\WebpayPlus\MallTransaction;
$response = (new MallTransaction)->status($token);

// SDK 1.x
use Transbank\Webpay\WebpayPlus\Transaction;
$response = Transaction::getMallStatus($token);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = MallTransaction.status(token)
```

```ruby
response = MallTransaction.status(token: token)
```

```python
response = MallTransaction.status(token)
```

```javascript
const response = await WebpayPlus.MallTransaction.status(token);
```

<strong>Respuesta estado de una transacción mall</strong>

Para obtener la información contenida en la respuesta puedes hacerlo de la siguiente manera.

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

### Reversar o Anular una transacción mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#reversar-o-anular-una-transaccion-mall' tbk-link-name='Referencia Api'></div>
</div>

Esta operación permite a todo comercio habilitado reversar o anular una transacción
que fue generada en Webpay Plus Mall. El método permite generar el reembolso del
total o parte del monto de una transacción. Dependiendo de la siguiente lógica
de negocio la invocación a esta operación generará una reversa o una anulación:

* Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.
* Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.

La anulación puede realizarse máximo 90 días después de la fecha de la
transacción original.

Puedes [leer más sobre la anulación en la información del
producto Webpay](/producto/webpay#anulaciones) para conocer
más detalles y restricciones.

<div class="language-simple" data-multiple-language></div>

```java
final RefundWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.refund(token, buyOrder, commerceCode, amount);
```

```php
//SDK 2.x
$response = (new MallTransaction)->refund($token, $buy_order, $commerce_code, $amount);

// SDK 1.x
$response = Transaction::refundMall($token, $buy_order, $commerce_code, $amount);
```

```csharp
var response = Transaction.refund(token, buyOrder, commerceCode, amount);
```

```ruby
response = Transaction.refund(token: token, buy_order: buy_order, commerce_code: commerce_code, amount: amount)
```

```python
response = Transaction.refund(token, buy_order, commerce_code, amount)
```

```javascript
const response = await WebpayPlus.MallTransaction.refund(token, buyOrder, commerceCode, amount);
```

<strong>Respuesta Reversa o Anulacion mall</strong>

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

```csharp
  response.AuthorizationCode;
  response.AuthorizationDate;
  response.Balance;
  response.NullifiedAmount;
  response.ResponseCode;
  response.Type;
```

```php
  $response->getAuthorizationCode();
  $response->getAuthorizationDate();
  $response->getBalance();
  $response->getNullifiedAmount();
  $response->getResponseCode();
  $response->getType();
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

### Capturar una transacción mall
<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#capturar-una-transaccion-mall' tbk-link-name='Referencia Api'></div>
</div>

<div class="language-simple" data-multiple-language></div>

```java
final WebpayPlusMallTransactionCaptureResponse response = WebpayPlus.MallDeferredTransaction.capture(token, childCommerceCode, buyOrder, authorizationCode, amount);
```

```php

//SDK 2.x
use Transbank\Webpay\WebpayPlus\MallTransaction;
$response = (new MallTransaction)->capture($childCommerceCode, $token, $buyOrder, $authorizationCode, $captureAmount);

// SDK 1.x
use Transbank\Webpay\WebpayPlus\Transaction;
$response = Transaction::captureMall($childCommerceCode, $token, $buyOrder, $authorizationCode, $captureAmount);
```

```csharp
var response = Transbank.Webpay.WebpayPlus.MallDeferredTransaction.Capture(token, childCommerceCode, buyOrder, authorizationCode, captureAmount);
```

```ruby
response = Transbank::Webpay::WebpayPlus::MallDeferredTransaction::capture(
  token: @token,
  child_commerce_code: @child_commerce_code,
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

<strong>Respuesta de una captura mall</strong>

<div class="language-simple" data-multiple-language></div>

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
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
````

```javascript
  response.authorization_code
  response.authorization_date
  response.balance
  response.nullified_amount
  response.response_code
  response.type
```

Esta operación funciona de la misma manera que la [captura de Webpay Plus normal](#capturar-una-transaccion).

## Credenciales y Ambiente

Para Webpay Plus, las credenciales del comercio (código de comercio y API Key) varían según el la modalidad del producto usado (Webpay Plus, Webpay Plus Mall. Webpay Plus Diferido, Webpay Plus Mall Diferido). También varían si la moneda a manejar es pesos chilenos (CLP) o dólares (USD).

Por lo tanto, es clave que antes de operar con las clases que permiten realizar transacciones se configure correctamente el SDK para con estas credenciales para utilizar el producto correcto

Los SDK pueden ser configurados para apuntar a un ambiente por defecto, estos ya vienen apuntados al ambiente de integración. Si quieres apuntar a producción tienes dos opciones; puedes re-configurar el SDK para que apunte a producción utilizando el código de comercio y API Key. O puedes configurar cada llamada a las operaciones siguientes para pasar un objeto `Options` en el cual configuras donde quieres apuntar.

Puede encontrar más información al respecto [en este link](/documentacion/como_empezar#b-utilizando-los-sdk)

## Conciliación de Transacciones

Una vez hayas realizado transacciones en producción quedará un historial de
transacciones que puedes revisar entrando a
[www.transbank.cl](https://www.transbank.cl/). Si lo deseas  puedes realizar una
conciliación entre tu sistema y el reporte que entrega el portal.

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
de la tabla "Detalles de la transacción" corresponden (la lista detallada de
parámetros de Webpay Plus la puedes encontrar
[acá](/referencia/webpay#obtener-estado-de-una-transaccion))

Nombre | Descripción
------ | -----------
vci | Resultado de la autenticación del tarjetahabiente.
amount | Monto de la transacción.
status | Estado de la transacción
buy_order | Orden de compra de la tienda
session_id | Identificador de sesión
card_detail | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo.
accounting_date | Fecha de la autorización.
transaction_date | Fecha y hora de la autorización.
authorization_code | Código de autorización de la transacción
payment_type_code  | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.
installments_amount | Monto de las cuotas.
installments_number | Cantidad de cuotas.
balance | Monto restante para un detalle anulado.

## Ejemplos de integración

Ponemos a tu disposición una serie de repositorios en nuestro Github para ayudarte a entender la integración de mejor forma.

[Puedes verlos acá](/documentacion/como_empezar#ejemplos)

<aside class="notice">
Si deseas revisar la documentación anterior (SOAP), puedes revisarla [acá](/documentacion/webpay)
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
