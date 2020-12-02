# Oneclick Mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#oneclick-mall' tbk-link-name='Referencia API'></div>
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
  tarjeta.
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
  <div tbk-link='/referencia/webpay#crear-una-inscripcion-oneclick-mall' tbk-link-name='Referencia API'></div>
</div>

Este permite realizar la inscripción del tarjetahabiente e información de su
tarjeta de crédito. Retorna como respuesta un token que representa la
transacción de inscripción y una URL (`urlWebpay`), que corresponde a la URL
de inscripción de Oneclick.

Una vez que se llama a este servicio Web, el usuario debe ser redireccionado
vía POST a `urlWebpay` con parámetro `TBK_TOKEN` igual al token obtenido.

<aside class="notice">
Nota que a diferencia de Webpay Plus, donde el parámetro se llama `token_ws`, en
Oneclick el parámetro se llama `TBK_TOKEN`.
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
// URL donde llegará el usuario con su token luego de finalizar la inscripción
$response_url = "https://callback/resultado/de/inscripcion";

$response = MallInscription::start($username, $email, $response_url);

$url_webpay = $resp->getUrlWebpay();
$tbk_token = $resp->getToken();
```

```csharp
//...
// Identificador del usuario en el comercio
var username = "nombre_de_usuario";
// Correo electrónico del usuario
var email = "nombre_de_usuario@gmail.com";
var response_url = "https://callback/resultado/de/inscripcion";

var response = Inscription.start(username, email, response_url);

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
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa.
```

Tal como en el caso de Oneclick Normal, debes redireccionar vía `POST` el navegador del usuario a la url retornada en `url_webpay`. **Recordando que el nombre del parámetro que contiene el token se debe llamar `TBK_TOKEN`**.

### Confirmar una inscripción

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#confirmar-una-inscripcion-oneclick-mall' tbk-link-name='Referencia API'></div>
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

Una vez que se autorice la inscripción del usuario, se retornará el control al comercio vía `POST` en la url indicada en `response_url`, con el parámetro `TBK_TOKEN` identificando la transacción. Con esa información se puede finalizar la inscripción:

<div class="language-simple" data-multiple-language></div>

```java
//...
String tbk_token = "elTokenQueLlegaPorPOST"; // token que llega por POST en el parámetro "TBK_TOKEN"
OneclickMallInscriptionFinishResponse response = OneclickMall.Inscription.finish(tbk_token);
String tbkUser = response.getTbkUser();
```

```php
//...
$tbk_token = "tbkToken"; // token que llega por POST en el parámetro "TBK_TOKEN"
$response = MallInscription::finish($tbk_token);
$tbkUser = $resp->getTbkUser();
```

```csharp
//...
var token = "tbkToken"; // token que llega por POST en el parámetro "TBK_TOKEN"
var result = Inscription.Finish(tbk_token);
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
tbk_token = "tbkToken" // token que llega por POST en el parámetro "TBK_TOKEN"
resp = MallInscription.finish(token=tbk_token)
tbkUser = resp.tbk_user
```

```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa.
```

### Eliminar una inscripción

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#eliminar-una-inscripcion-oneclick-mall' tbk-link-name='Referencia API'></div>
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
OneclickMall.Inscription.delete(username, tbkUser);
```

```php
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

var result = Inscription.Delete(username, tbkUser);
```

```ruby
#...

@username = "nombre_de_usuario"
@tbkUser = "tbkUserRetornadoPorInscriptionFinish"

@resp = Transbank::Webpay::Oneclick::MallInscription::delete(user_name: @username,tbk_user: @tbkUser)
```

```python
#...
tbkUser = "tbkUserRetornadoPorInscriptionFinish"
username = "nombre_de_usuario"

resp = MallInscription.delete(tbk_user=tbkUser, user_name=username)
```

```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa.
```

Si se quiere comprobar si se eliminó correctamente, la función retorna un boolean, el cual será `true` en caso de éxito y `false` en otro caso.
Recuerda que por cada transacción que hayas enviado en el arreglo (array de `details`) recibiras una respuesta.
Debes validarlas de manera independiente, ya que unas podrías estar aprobadas y otras no.

### Autorizar un pago

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#autorizar-un-pago-con-oneclick-mall' tbk-link-name='Referencia API'></div>
</div>

Con el `tbkUser` retornado de la confirmación (`PUT /inscriptions/{token}`) puedes autorizar transacciones:

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
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa.
```

### Obtener estado de una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#consultar-un-pago-realizado-con-oneclick-mall' tbk-link-name='Referencia API'></div>
</div>

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.
Revisa la [referencia](/referencia/webpay#consultar-un-pago-realizado-con-oneclick-mall) de este método para mayor detalle en los parámetros de entrada y respuesta.

<strong>Transaction.status()</strong>

Permite consultar el estado de pago realizado a través de Oneclick.
Retorna el resultado de la autorización.

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

### Reversar o anular una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#reversar-o-anular-un-pago-oneclick-mall' tbk-link-name='Referencia API'></div>
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

<strong>Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.

Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.</strong>

<strong>Transaction.refund()</strong>

Permite reversar o anular una transacción de venta autorizada con anterioridad.
Este método retorna como respuesta un identificador único de la transacción de reversa/anulación.

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

$response = MallTransaction::refund($buyOrder, $childCommerceCode, $childBuyOrder, $amount, $options);
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
#...

@buy_order = "12345" + Time.now.to_i.to_s
@child_commerce_code = "597055555542"
@child_buy_order = "abcdef" + Time.now.to_i.to_s
@amount = 1000

@resp = Transbank::Webpay::Oneclick::MallTransaction::refund(buy_order: @buy_order, child_commerce_code: @child_commerce_code, child_buy_order: @child_buy_order, amount: @amount)
```

```python
#...

buy_order = str(random.randrange(1000000, 99999999))
child_commerce_code = '597055555542'
child_buy_order = str(random.randrange(1000000, 99999999))
amount = 10000

resp = MallTransaction.refund(buy_order, child_commerce_code, child_buy_order, amount)
```

```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa.
```

### Capturar una transacción

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#captura-diferida-oneclick-mall' tbk-link-name='Referencia API'></div>
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
Tampoco se aceptan cuotas, solo ventas con tarjeta de crédito normales sin cuotas.
</aside>

```java
final OneclickMallTransactionCaptureResponse response = Oneclick.MallDeferredTransaction.capture(
  childCommerceCode, childBuyOrder, amount, authorizationCode
);
```

```php
//Este SDK aún no tiene implementada esta funcionalidad. Se puede consumir el método del API REST directamente, sin usar el SDK de momento.
```

```csharp
// Este SDK aún no tiene implementada esta funcionalidad. Se puede consumir el método del API REST directamente, sin usar el SDK de momento.
```

```ruby
# Este SDK aún no tiene implementada esta funcionalidad. Se puede consumir el método del API REST directamente, sin usar el SDK de momento.
```

```python
# Este SDK aún no tiene implementada esta funcionalidad. Se puede consumir el método del API REST directamente, sin usar el SDK de momento.
```

## Credenciales y Ambientes

### Ambiente de integración

En el ambiente de integración existen códigos de comercio previamente creados para todos los productos (Webpay Plus,
Oneclick, etc), para cada una de sus variaciones (Captura Diferida, Mall, Mall Captura Diferida, etc) y dependiendo de
la moneda que acepten (USD o CLP).

Asegúrate de que estés usando el código de comercio de integración que tenga la misma configuración del producto que contrataste.

Puedes revisar los códigos de comercio del ambiente de integración de todos nuestros productos y variaciones
[en este link](/documentacion/como_empezar#ambiente-de-integracion).

### Oneclick: Configuración SDK

Los SDK vienen preconfigurados para operar con Oneclick Mall captura simultanea. Si necesitas operar con otra modalidad,
como captura diferida, debes configurar explícitamente el [código de comercio que usarás](/documentacion/como_empezar#ambiente-de-integracion).
No es necesario definir el Api Key Secret (llave secreta) ya que en este ambiente, todos los productos usan la misma y
ya viene preconfigurada.

```java
// OneclickMall Live Config
OneclickMall.setCommerceCode("pon-tu-codigo-de-comercio-aca");
```

```php
\Transbank\Webpay\OneClick::setCommerceCode("{commerce-code}");
```

```csharp
using Transbank.Webpay.Oneclick;

Oneclick.CommerceCode = "5970TuCodigo";
```

```ruby
# Oneclick
Transbank::Webpay::OneClick::Base.commerce_code = "commercecode"
```

```python
# Oneclick
from transbank import oneclick as BaseOneClick
from transbank.common.integration_type import IntegrationType

BaseOneClick.commerce_code = "codigo-comercio-aca"
```

### Apuntar a producción

Antes de operar en el ambiente de producción, debes pasar por un [proceso de validación](/documentacion/como_empezar#el-proceso-de-validacion), luego del cual te entregaremos tu Secret Key (**llave secreta**).  

Si ya tienes tu llave secreta, puedes revisar como configurar el SDK para usar este ambiente de producción en [esta sección](/documentacion/como_empezar#configuracion-de-produccion)

## Conciliación de Transacciones

Una vez hayas realizado transacciones en producción quedará un historial de
transacciones que puedes revisar entrando a
[www.transbank.cl](https://www.transbank.cl/). Si lo deseas  puedes realizar una
conciliación entre tu sistema y el reporte que entrega el portal.

### Oneclick

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
[acá](/referencia/webpay-soap#autorizar-un-pago-con-webpay-oneclick))

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
