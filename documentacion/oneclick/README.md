# Oneclick

<div class="pos-title-nav">
  <div tbk-link='/referencia/oneclick' tbk-link-name='Referencia API'></div>
</div>

La modalidad de pago Oneclick permite al tarjetahabiente realizar pagos en el
comercio sin la necesidad de ingresar cada vez información de la tarjeta de
crédito al momento de realizar la compra. El modelo de pago contempla un
proceso previo de inscripción o enrolamiento del tarjetahabiente, a través del
comercio, que desee utilizar el servicio. Este tipo de pago facilita la venta,
disminuye el tiempo de la transacción y reduce los riesgos de ingreso erróneo
de los datos del medio de pago.

El proceso de integración con Oneclick consiste en desarrollar por parte
del comercio las llamadas a los servicios web dispuestos por Transbank para la
inscripción de los tarjetahabientes, así como para la realización de los
pagos.

<aside class="notice">
Oneclick solo opera en modalidad Mall. 
</aside>

## Flujo de inscripción y pago

La inscripción es el proceso en el cual el tarjetahabiente registra los datos
de su tarjeta en Oneclick para usarlo en compras futuras. Estos datos son
almacenados de forma segura en Transbank, y nunca son conocidos por el comercio.
Este proceso debe ser iniciado por la tienda del comercio y es requisito que el
cliente esté autenticado (haya iniciado sesión) en la página del comercio antes de iniciar la
inscripción.

<img class="td_img-night" src="/images/diagrama-secuencia-oneclick-inscripcion.png" alt="Diagrama de secuencia inscripción Oneclick">

1. El cliente se conecta y autentica en la página del comercio, mediante su
  nombre de usuario y clave.
2. El cliente selecciona la opción de inscripción, la cual debe estar explicada
  en la página del comercio.
3. El comercio consume un servicio web para crear una inscripción, donde entrega los
  datos del cliente y la URL de término; obtiene un token y URL de Webpay.
4. El comercio envía el browser del cliente a la URL y pasa por
  parámetro el token (método POST).
5. Webpay presenta el formulario de inscripción, este es similar al formulario
  de pago actual de Webpay Plus, para que el cliente ingrese los datos de su
  tarjeta. El tiempo en el cual permanece el formulario de Webpay en espera para
   incluir los datos de la tarjeta es de 5 minutos, en caso extender dicho plazo
   y no haber terminado la transacción, esta será abortada automáticamente.
   
   <aside class="warning">
   En caso de que se cumpla el tiempo máximo para completar el formulario o el tarjetahabiente presione el botón para anular la inscripción, 
   el comercio recibirá las variables TBK_ID_SESSION y TBK_ORDEN_COMPRA.
   </aside>
6. El cliente será autenticado por su banco emisor, de forma similar al flujo
  normal de pago. En este punto se realiza una transacción de $50 pesos, la cual
  no se captura (no se verá reflejada en su estado de cuenta).
7. Finalizada la inscripción, Webpay envía el browser del cliente a la URL
  entregada por el comercio, pasando por parámetro el token.
8. El comercio debe consumir otro servicio web de Transbank para finalizar la inscripción enviando el token, para
  obtener el resultado de la inscripción y el identificador de usuario (`tbkUser`), que
  debe utilizar en el futuro para realizar los pagos.
9. El comercio presenta al cliente el resultado de la inscripción.

## Autorización (proceso de pago)

Después de realizado el proceso de inscripción, el comercio puede iniciar el proceso de pago cuando corresponda.

El pago es el proceso donde el comercio solicita el cargo de una compra a la
tarjeta de crédito de un usuario inscrito anteriormente, usando el
identificador entregado por Transbank al momento de la inscripción.

Los pagos en esta modalidad no requieren necesariamente la intervención del
usuario.

El monto del pago debe estar dentro de los límites establecidos para este tipo
de transacciones, el proceso interno es similar a un cargo normal de Webpay.
Existe un máximo de transacciones diarias que puede realizar un solo usuario,
además de un monto máximo por transacción y un monto máximo acumulado diario.
Estos valores se definen en el proceso de afiliación comercial del producto.  

<img class="td_img-night" src="/images/diagrama-secuencia-oneclick-pago.png" alt="Diagrama de secuencia inscripción Oneclick">

1. El cliente se conecta y autentica en la página o aplicación del comercio
  mediante su nombre de usuario y clave.
2. El cliente selecciona la opción de pagar con Oneclick.
3. El comercio usa el servicio web de pago para autorizar pagos, entregando
  el identificador de usuario (que se obtuvo durante la inscripción: `tbkUser`), el monto del
  pago y la orden de compra. Obtiene la respuesta con el código de
  respuesta.
4. El comercio presenta el resultado del pago al cliente.

Adicionalmente, este proceso puede suceder sin la intervención directa del usuario:

1. El comercio, teniendo el identificador del usuario (`tbkUser`), puede usar el
servicio web de pago en un `cronjob` o algún proceso programado que
tenga el comercio en sus sistemas. Ejemplo: El comercio puede crear un proceso que
corre automáticamente una vez al mes por cada cliente, donde se realiza la llamada
al servicio web de pago para cobrar una mensualidad.

## Modalidad Mall

En la modalidad Oneclick Mall, existe un código de comercio "mall" que agrupa una serie de códigos de comercio "tienda".

* El usuario inscribe su tarjeta en la página del comercio "mall" agrupador, pero las transacciones son a nombre de las "tiendas" del mall.
* Se pueden indicar múltiples transacciones a autorizar en una misma operación con diferentes códigos de comercio tienda.
* Se debe verificar por separado el resultado de cada una de esas transacciones, validando el código de respuesta (`responseCode`),
pues es posible que el emisor de la tarjeta autorice algunas y otras no.

## Operaciones

### Crear una inscripción

<div class="pos-title-nav">
  <div tbk-link='/referencia/oneclick#crear-una-inscripcion' tbk-link-name='Referencia API'></div>
</div>

Permite comenzar con la inscripción de de la tarjeta del usuario. Retorna como respuesta un token, que representa la 
inscripción, y una URL (`urlWebpay`). 

El usuario debe ser redireccionado a `urlWebpay`, enviando como parámetro `TBK_TOKEN` el token recibido (Puede ser una redirección por POST o GET).

<aside class="notice">
Nota que a diferencia de Webpay Plus, donde el parámetro se llama `token_ws`, en
Oneclick el parámetro se llama `TBK_TOKEN`.
</aside>

<aside class="notice">
Un mismo `username` solo puede tener una tarjeta inscrita en tu comercio. Si eventualmente se solicita una nueva 
inscripción a un username previamente registrado, al finalizar la nueva inscripción el `tbk_user` de la tarjeta anterior 
será inválido. 
Como recomendación, si un usuario en tu sistema ya tiene una tarjeta inscrita, no debería poder volver a inscribir una nueva a menos que elimine la anterior.
</aside>

<div class="language-simple" data-multiple-language></div>

```java
//...
// Identificador del usuario en el comercio
String username = "nombre_de_usuario";
// Correo electrónico del usuario
String email = "nombre_de_usuario@gmail.com";
// URL donde llegará el usuario con su token luego de finalizar la inscripción
String response_url = "https://callback/resultado/de/inscripcion";

OneclickMallInscriptionStartResponse response = Oneclick.MallInscription.start(username, email, response_url);

String url_webpay = response.getUrlWebpay();
String tbk_token = response.getToken();
```

```php
// -----------------------
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

// -----------------------
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
```

```csharp
//...
// Identificador del usuario en el comercio
var username = "nombre_de_usuario";
// Correo electrónico del usuario
var email = "nombre_de_usuario@gmail.com";
var response_url = "https://callback/resultado/de/inscripcion";

var response = MallInscription.start(userame, email, response_url);

var url_webpay = response.Url;
var tbk_token = response.Token;
```

```ruby
@username = "nombre_de_usuario"
@email = "nombre_de_usuario@gmail.com"
@response_url = "https://callback/resultado/de/inscripcion"

@resp = Transbank::Webpay::Oneclick::MallInscription::start(user_name: @username,email: @email,response_url: @response_url)

@url_webpay = @resp.url_webpay
@tbk_token = @resp.token
```

```python
username = "nombre_de_usuario"
email = "nombre_de_usuario@gmail.com"
response_url = "https://callback/resultado/de/inscripcion"

resp = MallInscription.start(user_name=username,email=email,response_url=response_url)

url_webpay = resp.url_webpay
tbk_token = resp.token
```

```javascript
const username = 'nombre_de_usuario';
const email = 'nombre_de_usuario@email.com';
const responseUrl = 'https://callback/resultado/de/inscripcion';

const response = await Oneclick.MallInscription.start(username, email, responseUrl);
```

<strong>Respuesta de crear una inscripción</strong>

<div class="language-simple" data-multiple-language></div>

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

Tal como en el caso de Oneclick Normal, debes redireccionar vía `POST` el navegador del usuario a la url retornada en `url_webpay`. **Recordando que el nombre del parámetro que contiene el token se debe llamar `TBK_TOKEN`**.

### Confirmar una inscripción

<div class="pos-title-nav">
  <div tbk-link='/referencia/oneclick#confirmar-una-inscripcion' tbk-link-name='Referencia API'></div>
</div>

Una vez terminado el flujo de inscripción en Transbank el usuario es enviado a
la URL de fin de inscripción que definió el comercio (`responseURL`). En ese
momento el comercio debe confirmar la inscripción.

<aside class="warning">
El comercio tendrá un máximo de 60 segundos para llamar a este método luego
de recibir el token en la URL de fin de inscripción (`returnUrl`). Pasados los
60 segundos sin llamada a el método de confirmar una inscripción, la inscripción en curso junto con
el usuario serán eliminados.
</aside>

Una vez que se autorice la inscripción del usuario, se retornará el control al comercio vía `POST` (`GET` si usas el API 1.1 o superior) en la url indicada en `response_url`, con el parámetro `TBK_TOKEN` identificando la transacción. Con esa información se puede finalizar la inscripción:

<div class="language-simple" data-multiple-language></div>

```java
//...
String tbk_token = "elTokenQueLlegaPorPOST"; // token que llega por POST en el parámetro "TBK_TOKEN"
OneclickMallInscriptionFinishResponse response = Oneclick.MallInscription.finish(tbk_token);
String tbkUser = response.getTbkUser();
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
//...
var token = "tbkToken"; // token que llega por POST en el parámetro "TBK_TOKEN"
var result = MallInscription.Finish(tbk_token);
var tbkUser = result.TbkUser;

```

```ruby
#...
@tbk_token = "tbkToken"; # // token que llega por POST en el parámetro "TBK_TOKEN"
@resp = Transbank::Webpay::Oneclick::MallInscription::finish(token: @tbk_token)
@tbkUser = @resp.tbk_user
```

```python
#...
tbk_token = "tbkToken" # token que llega por POST en el parámetro "TBK_TOKEN"
resp = MallInscription.finish(token=tbk_token)
tbkUser = resp.tbk_user
```

```javascript
//...
const token = 'tbkToken' // token que llega por POST en el parámetro "TBK_TOKEN"
const response = Oneclick.MallInscription.finish(token);
```

<strong>Respuesta de confirmar una inscripción</strong>

<div class="language-simple" data-multiple-language></div>

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

### Eliminar una inscripción

<div class="pos-title-nav">
  <div tbk-link='/referencia/oneclick#eliminar-una-inscripcion' tbk-link-name='Referencia API'></div>
</div>

En el caso que el comercio requiera eliminar la inscripción de un usuario en OneClick Mall ya sea por la eliminación
de un cliente en su sistema o por la solicitud de este para no operar con esta forma de pago,
el comercio deberá invocar a removeInscription() con el identificador de usuario entregado en la inscripción.

<div class="language-simple" data-multiple-language></div>

```java
//...
// Identificador del usuario en el comercio
String username = "nombre_de_usuario";
String tbkUser = "tbkUserRetornadoPorInscriptionFinish";
Oneclick.MallInscription.delete(username, tbkUser);
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
//...
// Identificador del usuario en el comercio
var username = "nombre_de_usuario";
var tbkUser = "tbkUserRetornadoPorInscriptionFinish";

var result = MallInscription.Delete(username, tbkUser);
```

```ruby
#...

@username = "nombre_de_usuario"
@tbkUser = "tbkUserRetornadoPorInscriptionFinish"

@resp = Transbank::Webpay::Oneclick::MallInscription::delete(user_name: @username,tbk_user: @tbkUser)
```

```python
#...
username = "nombre_de_usuario"
tbkUser = "tbkUserRetornadoPorInscriptionFinish"

resp = MallInscription.delete(tbk_user=tbkUser, user_name=username)
```

```javascript
username = 'nombre_de_usuario';
tbkUser = 'tbkUserRetornadoPorInscriptionFinish';
const response = await Oneclick.MallInscription.delete(tbkUser, username);
```

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

Si se quiere comprobar si se eliminó correctamente, la función retorna un boolean, el cual será `true` en caso de éxito y `false` en otro caso.
Recuerda que por cada transacción que hayas enviado en el arreglo (array de `details`) recibiras una respuesta.
Debes validarlas de manera independiente, ya que unas podrías estar aprobadas y otras no.

### Autorizar una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/oneclick#autorizar-una-transaccion' tbk-link-name='Referencia API'></div>
</div>

Con el `tbkUser` retornado de la confirmación (`PUT /inscriptions/{token}`) puedes autorizar transacciones:

Una vez que ya tienes la tarjeta del usuario inscrita (ya tienes el token `tbk_user` para ese usuario), puedes realizar 
cargos a esa tarjeta en cualquier momento, solo llamando a este método de autorización, enviando el `tbk_user`, el 
`username` del usuario, un identificador de compra `parentBuyOrder` y los datos de la transacción.

Como Oneclick opera en modalidad Mall, en una misma autorización puedes realizar varios cobros, cada uno a códigos de 
comercio **tienda** diferente. No olvidar que para realizar el cargo correctamente, esos código de comercio tienda deben 
"pertenecer" o estar asociados a tu código de comercio Mall cuando se contratan.
Al tarjetahabiente se le realizará un solo cobro por la suma del monto de todas las "sub-transacciones" solicitadas, pero
el dinero será procesado y enviado a cada código de comercio por separado. 
Para aclararlo con un ejemplo, si solicito una autorización por $1.500 para el comercio A, y $2.500 para el comercio B, el 
tarjetahabiente verá un único cobro de $4.000 en su cartola, pero el comercio A recibirá los $1.500 y el comercio B los 
$2.500 (en ambos casos, restando la comisión). 

Cada "sub-transacción" de la autorización requiere de un código de comercio, un identificador de compra (ojalá único y 
diferente al identificador de compra padre), un monto y, opcionalmente, el número de cuotas en que se realizará el cobro. 


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


OneclickMallTransactionAuthorizeResponse response = Oneclick.Transaction.authorize(username, tbkUser, buyOrder, details);
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

```javascript
const details = [
  new TransactionDetail(amount, commerceCode, childBuyOrder),
  new TransactionDetail(amount2, commerceCode2, childBuyOrder2)
];

const response = await Oneclick.MallTransaction.authorize(
  userName, tbkUser, buyOrder, details
);
```

<strong>Respuesta Autorizar un pago</strong>

Luego de llamar a este método, se obtendrá el estado del cobro inmediatamente, sin que el usuario tenga que pasar por el 
proceso de autorización bancaria. 

En este punto, ya puedes saber si la transacción fue aprobada o rechazada.

<aside class="notice">
Para verificar si una transacción fue aprobada, debes confirmar el que código de respuesta `response_code` sea 
exactamente `0` y que el estado `status` sea exactamente `AUTHORIZED` por cada una de las "sub-transacciones" recibidas.
</aside>

Verás que el mensaje de respuesta devuelve datos generales sobre la operación, y un detalle del cobro por cada una de 
las "sub-transacciones" enviadas. Es importante verificar cada una de ellas, ya que eventualmente (poco probable pero 
posible), una podría salir aprobada y otra rechazada. 

Revisa los posibles códigos de respuesta y el detalle de los parametros en [la referencia](/referencia/oneclick#autorizar-una-transaccion)

<div class="language-simple" data-multiple-language></div>

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
for(let detail on details) {
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

### Obtener estado de una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/oneclick#obtener-estado-de-una-transaccion' tbk-link-name='Referencia API'></div>
</div>

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.
Revisa la [referencia](/referencia/oneclick#obtener-estado-de-una-transaccion) de este método para mayor detalle en los parámetros de entrada y respuesta.

<strong>Transaction.status()</strong>

Permite consultar el estado de pago realizado a través de Oneclick.
Retorna el resultado de la autorización.

<div class="language-simple" data-multiple-language></div>

```java
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

var result = MallTransaction.Status(buyOrder);
```

```ruby
response = Transbank::Webpay::Oneclick::MallTransaction::status(buy_order: buy_order)
```

```python
var response = MallTransaction.status(buy_order)
```

```javascript
const response = await Oneclick.MallTransaction.status(token);
```

<strong>Respuesta de consulta de estado</strong>

<div class="language-simple" data-multiple-language></div>

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

### Reversar o anular una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/oneclick#reversar-o-anular-una-transaccion' tbk-link-name='Referencia API'></div>
</div>

Para Oneclick Mall hay dos operaciones diferentes para dejar sin efecto
transacciones autorizadas: La reversa y la anulación.

**La reversa** se aplica para **problemas operacionales (lado comercio) o de
comunicación entre comercio y Transbank que impidan recibir a tiempo la
respuesta de una autorización**. En tal caso el comercio **debe** intentar
reversar la transacción de autorización para evitar un posible descuadre entre
comercio y Transbank. La reversa funciona sobre la operación completa del mall,
lo que significa que **todas las transacciones realizadas en la operación mall**
**serán reversadas**.

**La anulación**, en cambio, actúa individualmente sobre las transacciones de
las _tiendas_ de un mall. Por ende, **la anulación es la operación correcta a
utilizar para fines financieros**, de manera de anular un cargo ya realizado.
Permite generar el reembolso del total o parte del monto de una transacción completa.
Dependiendo de la siguiente lógica de negocio la invocación a esta operación generará una
reversa o una anulación:

<strong>* Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial. </strong>

<strong>* Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado **(una hora)**, de lo contrario se ejecutará una anulación.</strong>

<strong>Transaction.refund()</strong>

Permite reversar o anular una transacción de venta autorizada con anterioridad.
Este método retorna como respuesta un identificador único de la transacción de reversa/anulación.

<div class="language-simple" data-multiple-language></div>

```java
//...
String buyOrder = "buyOrderIndicadoEnTransactionAuthorize";
String childCommerceCode = "childCommerceCodeIndicadoEnTransactionAuthorize";
String childBuyOrder = "childBuyOrderIndicadoEnTransactionAuthorize";
double amount = 10000;

OneclickMallTransactionRefundResponse response = Oneclick.Transaction.refund(buyOrder, childCommerceCode, childBuyOrder, amount);
```

```php
// Versión 2.x del SDK
// ----------------------- 
use Transbank\Webpay\Oneclick\MallTransaction;

$buyOrder = "buyOrderIndicadoEnTransactionAuthorize";
$childCommerceCode = "childCommerceCodeIndicadoEnTransactionAuthorize";
$childBuyOrder = "childBuyOrderIndicadoEnTransactionAuthorize";
$amount = 10000;

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
$buyOrder = "buyOrderIndicadoEnTransactionAuthorize";
$childCommerceCode = "childCommerceCodeIndicadoEnTransactionAuthorize";
$childBuyOrder = "childBuyOrderIndicadoEnTransactionAuthorize";
$amount = 10000;

//Parámetro opcional
$options = new Options($apiKey, $parentCommerceCode);

$response = MallTransaction::refund($buyOrder, $childCommerceCode, $childBuyOrder, $amount);
```

```csharp
//...

var buyOrder = "buyOrderIndicadoEnTransactionAuthorize";
var childCommerceCode = "childCommerceCodeIndicadoEnTransactionAuthorize";
var childBuyOrder = "childBuyOrderIndicadoEnTransactionAuthorize";
var amount = 10000;

var result = MallTransaction.Refund(buyOrder, childCommerceCode, childBuyOrder, amount);
```

```ruby
#...

@buy_order = "buyOrderIndicadoEnTransactionAuthorize"
@child_commerce_code = "childCommerceCodeIndicadoEnTransactionAuthorize"
@child_buy_order = "childBuyOrderIndicadoEnTransactionAuthorize"
@amount = 10_000

@resp = Transbank::Webpay::Oneclick::MallTransaction::refund(buy_order: @buy_order, child_commerce_code: @child_commerce_code, child_buy_order: @child_buy_order, amount: @amount)
```

```python
#...

buy_order = "buyOrderIndicadoEnTransactionAuthorize"
child_commerce_code = "childCommerceCodeIndicadoEnTransactionAuthorize"
child_buy_order = "childBuyOrderIndicadoEnTransactionAuthorize"
amount = 10000

resp = MallTransaction.refund(buy_order, child_commerce_code, child_buy_order, amount)
```

```javascript
const buyOrder = "buyOrderIndicadoEnTransactionAuthorize";
const childCommerceCode = "childCommerceCodeIndicadoEnTransactionAuthorize";
const childBuyOrder = "childBuyOrderIndicadoEnTransactionAuthorize";
const amount = 10000;
const response = await Oneclick.MallTransaction.refund(buyOrder, childCommerceCode, buyOrderChild, amount);
```

### Capturar una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/oneclick#captura-diferida-de-una-transaccion' tbk-link-name='Referencia API'></div>
</div>

En el caso de que tengas contratada la modalidad de Captura diferida, necesitas llamar al método `capture` después
de llamar a `authorize` para finalizar la transacción.

Para capturar una transacción, esta debe haber sido creada por un código de
comercio configurado para captura diferida. De esa forma la transacción estará
autorizada pero requerirá una captura explícita posterior para confirmar la
transacción.

Para realizar esa captura explícita debe usarse el método `capture()`

Una inscripción Oneclick Mall permite que el tarjetahabiente registre su
tarjeta, asociando dicha inscripción a un comercio **padre**. Una vez realizada la inscripción,
el comercio padre autoriza transacciones para los comercios “hijo” que tiene registrados.
La autorización se encarga de validar si es posible realizar el cargo a la tarjeta de crédito, débtio o prepago realizando
en el mismo acto la reserva del monto de la transacción.
La posterior captura hace efectiva dicha reserva y "captura" el monto "reservado" previamente.

Este método permite a los comercios Oneclick Mall habilitados, poder
realizar capturas diferidas de una transacción previamente autorizada. El método
contempla una única captura por cada autorización. Para ello se deberá indicar los
datos asociados a la transacción de venta y el monto requerido para capturar, el cual
debe ser menor o igual al monto originalmente autorizado.
Para capturar una transacción, ésta debe haber sido creada por un código de
comercio configurado para captura diferida. De esta forma la transacción estará
autorizada pero requerirá una captura explícita posterior para confirmar la
transacción.

<aside class="notice">
En esta modalidad no se aceptan tarjetas de débito ni prepago.
</aside>

<div class="language-simple" data-multiple-language></div>

```java
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
$response = MallTransaction::capture($commerce_code, $buy_order, $authorization_code, $amount);
```

```csharp
// Este SDK aún no tiene implementada esta funcionalidad. Se puede consumir el método del API REST directamente, sin usar el SDK de momento.
```

```ruby
response = Transbank::Webpay::Oneclick::MallDeferredTransaction::capture(
  child_commerce_code: @commerce_code, child_buy_order: @buy_order,
  amount: @capture_amount, authorization_code: @authorization_code
)
```

```python
# Este SDK aún no tiene implementada esta funcionalidad. Se puede consumir el método del API REST directamente, sin usar el SDK de momento.
```

```javascript
const response = await Oneclick.DeferredTransaction.capture(commerceCode, buyOrder, amount, authorizationCode);
```

<strong>Respuesta de captura diferida</strong>

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
// Esta función aun no se encuentra disponible en el SDK
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

## Credenciales y Ambientes

### Ambiente de integración

Puede encontrar más información al respecto [en este link](/documentacion/como_empezar#ambiente-de-integracion)

### Configuración SDK

Los SDK vienen preconfigurados para operar con Oneclick Mall captura simultanea. Si necesitas operar con otra modalidad,
como captura diferida, debes configurar explícitamente el [código de comercio que usarás](/documentacion/como_empezar#codigos-de-comercio).
No es necesario definir el Api Key ya que en este ambiente, todos los productos usan la misma y
ya viene preconfigurada.

Puede encontrar más información al respecto [en este link](/documentacion/como_empezar#b-utilizando-los-sdk)

### Puesta en Producción

Puede encontrar más información al respecto [en este link](/documentacion/como_empezar#puesta-en-produccion)

## Conciliación de Transacciones

Una vez hayas realizado transacciones en producción quedará un historial de
transacciones que puedes revisar entrando a
[www.transbank.cl](https://www.transbank.cl/). Si lo deseas  puedes realizar una
conciliación entre tu sistema y el reporte que entrega el portal.

Para realizar la conciliación debes seguir los siguientes pasos:

1. Iniciar sesión con tu usuario y contraseña en [www.transbank.cl](https://www.transbank.cl/)

2. Una vez entras al portal, en el menú principal presiona "Webpay", y luego "Reporte Transaccional"
![Paso 2](/images/documentacion/conciliacion1.png)

3. En la parte superior de la ventana puedes encontrar un buscador que te ayudará
a filtrar, según los parámetros que gustes, las transacciones que quieras cuadrar.
Para filtrar por las transacciones de Oneclick, en el campo "Producto" debes
seleccionar **Oneclick**.
![Paso 3](/images/documentacion/conciliacion2.png)

4. Dentro de la tabla en la imagen anterior puedes presionar el número de orden de
compra para abrir los detalles de la transacción. Es en esta sección donde podrás
encontrar y conciliar los parámetros devueltos por el SDK al confirmar una transacción.
![Paso 4](/images/documentacion/conciliacion3.png)

5. Sólo queda realizar la conciliación. A continuación puedes ver una lista de
parámetros que recibirás al momento de confirmar una transacción y a que fila
de la tabla "Detalles de la transacción" corresponden (la lista completa de
parámetros de Oneclick la puedes encontrar
[acá](/referencia/oneclick#autorizar-una-transacción))

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorizationCode  <br> <i> xs:string </i> | Código de autorización
creditCardType  <br> <i> creditCardType </i> | Medio de Pago
last4CardDigits  <br> <i> xs:string </i> | Final número tarjeta
responseCode  <br> <i> xs:int </i> | Código de respuesta

## Ejemplos de integración

Ponemos a tu disposición una serie de repositorios en nuestro Github para ayudarte a entender la integración de mejor forma.
Puedes encontrar una lista de [proyectos de ejemplo acá](/documentacion/como_empezar#ejemplos).

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
