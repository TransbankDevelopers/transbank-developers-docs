# OneClick

## OneClick Mall %<span class='tbk-tagTitleDesc'>REST</span>%

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#oneclick-mall' tbk-link-name='Referencia API'></div>
</div>

Para usar OneClick Mall en transacciones asociadas a varios comercios, lo primero que se debe hacer es definir las dependencias necesarias para poder realizar cualquier tipo de transacción.

<div class="language-simple" data-multiple-language></div>

```java

import cl.transbank.webpay.oneclick.OneClickMall;

// ...

```

```php

use Transbank\Webpay\Oneclick\MallInscription;
use Transbank\Webpay\Oneclick\MallTransaction;

// ...

```

```csharp

using Transbank.Webpay.Oneclick;

// ...

```

```ruby

// Incluir en el Gemfile
gem 'transbank-sdk', git: "https://github.com/TransbankDevelopers/transbank-sdk-ruby.git"


```

```python

from transbank.oneclick.mall_inscription import MallInscription
from transbank.oneclick.mall_transaction import MallTransaction

from transbank.oneclick.request import MallTransactionAuthorizeDetails

// ...

```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```


Una vez que ya cuentas con esa preparación, puedes iniciar transacciones:


### Crear una inscripción

Te permite realizar la inscripción del tarjetahabiente:

<div class="language-simple" data-multiple-language></div>

```java
//...


// Identificador del usuario en el comercio
String username = "nombre_de_usuario";
// Correo electrónico del usuario
String email = "nombre_de_usuario@gmail.com";
String response_url = "https://callback/resultado/de/transaccion";

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
$response_url = "https://callback/resultado/de/transaccion";

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
var response_url = "https://callback/resultado/de/transaccion";

var response = Inscription.start(username, email, response_url);

var url_webpay = response.Url;
var tbk_token = response.Token;

```

```ruby


@username = "nombre_de_usuario"
@email = "nombre_de_usuario@gmail.com"
@response_url = "https://callback/resultado/de/transaccion"


@resp = Transbank::Webpay::Oneclick::MallInscription::start(user_name: @username,email: @email,response_url: @response_url)

@url_webpay = @resp.url_webpay
@tbk_token = @resp.token

```

```python

username = "nombre_de_usuario"
email = "nombre_de_usuario@gmail.com"
response_url = "https://callback/resultado/de/transaccion"

resp = MallInscription.start(user_name=username,email=email,response_url=response_url)

url_webpay = resp.url_webpay
tbk_token = resp.token


```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```

Tal como en el caso de Oneclick Normal, debes redireccionar vía `POST` el navegador del usuario a la url retornada en `url_webpay`. **Recordando que el nombre del parámetro que contiene el token se debe llamar `TBK_TOKEN`**.

### Confirmar una inscripción

Una vez que se autorice la inscripción del usuario, se retornará el control al comercio vía `POST` en la url indicada en `response_url`, con el parámetro `TBK_TOKEN` identificando la transacción. Con esa información se puede finalizar la inscripción:

<div class="language-simple" data-multiple-language></div>

```java
//...

String tbk_token = "tbkTokenRetornadoPorInscriptionStart";

OneclickMallInscriptionFinishResponse response = OneclickMall.Inscription.finish(tbk_token);

String tbkUser = response.getTbkUser();

```

```php
//...

$tbk_token = "tbkTokenRetornadoPorInscriptionStart";

$response = MallInscription::finish($tbk_token);

$tbkUser = $resp->getTbkUser();


```

```csharp
//...

var token = "tbkTokenRetornadoPorInscriptionStart";

var result = Inscription.Finish(tbk_token);

var tbkUser = result.TbkUser;

```

```ruby
//...

@tbk_token = "tbkTokenRetornadoPorInscriptionStart";

@resp = Transbank::Webpay::Oneclick::MallInscription::finish(token: @tbk_token)

@tbkUser = @resp.tbk_user

```

```python
//...

tbk_token = "tbkTokenRetornadoPorInscriptionStart"

resp = MallInscription.finish(token=tbk_token)

tbkUser = resp.tbk_user

```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```

Con eso habrás completado el flujo "feliz" en que todo funciona OK. En [la referencia detallada de OneClick Mall puedes ver cada paso del flujo, incluyendo los casos de borde que también debes manejar](https://www.transbankdevelopers.cl/referencia/webpay#webpay-oneclick-mall).

### Eliminar una inscripción

Si en algún momento se quiere eliminar la inscripción de un usuario, se debe invocar a `Inscription.delete()`, con el identificador de inscripción `tbkUser` obtenido en `Inscription.finish()`.

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
$username = "nombre_de_usuario";
$tbkUser = $tbkUserRetornadoPorInscriptionFinish;

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
//...

@username = "nombre_de_usuario"
@tbkUser = "tbkUserRetornadoPorInscriptionFinish"

@resp = Transbank::Webpay::Oneclick::MallInscription::delete(user_name: @username,tbk_user: @tbkUser)

```

```python
//...

tbkUser = "tbkUserRetornadoPorInscriptionFinish"
username = "nombre_de_usuario"

resp = MallInscription.delete(tbk_user=tbkUser, user_name=username)

```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```

Si se quiere comprobar si se eliminó correctamente, la función retorna un boolean, el cual será `true` en caso de éxito y `false` en otro caso.

### Realizar transacciones

Con el `tbkUser` retornado de `Inscription.finish()` puedes autorizar transacciones:

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

### Anular una transacción

En el caso de que se quiera anular alguna transacción, se invoca a `Transaction.refund()`.

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
//...

@buy_order = "12345" + Time.now.to_i.to_s
@child_commerce_code = "597055555542"
@child_buy_order = "abcdef" + Time.now.to_i.to_s
@amount = 1000

@resp = Transbank::Webpay::Oneclick::MallTransaction::refund(buy_order: @buy_order, child_commerce_code: @child_commerce_code, child_buy_order: @child_buy_order, amount: @amount)

```

```python
//...


buy_order = str(random.randrange(1000000, 99999999))
child_commerce_code = '597055555542'
child_buy_order = str(random.randrange(1000000, 99999999))
amount = 10000

resp = MallTransaction.refund(buy_order, child_commerce_code, child_buy_order, amount)

```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```

## Credenciales y Ambiente

### Ambiente de integración
En el ambiente de integración existen códigos de comercio previamente creados para todos los productos (Webpay Plus, 
OneClick, etc), para cada una de sus variaciones (Captura Diferida, Mall, Mall Captura Diferida, etc) y dependiendo de 
la moneda que acepten (USD o CLP).

Esto permite que puedas operar en el ambiente de pruebas con un código de comercio que tenga la misma configuración contratada
en tu código de comercio productivo. (Si contrataste OneClick Mall Captura Diferida, debes usar ese código de comercio 
en integración para realizar las pruebas) 

Puedes revisar los códigos de comercio del ambiente de integración de todos nuestros productos y variaciones 
[en este link](/documentacion/como_empezar#ambiente-de-integracion).

### OneClick: Configuración SDK 
Los SDK vienen preconfigurados para operar con OneClick Mall captura simultanea. Si necesitas operar con otra modalidad, 
como captura diferida, debes configuirar explicitamente el [código de comercio que usarás](/documentacion/como_empezar#ambiente-de-integracion).
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
# OneClick
Transbank::Webpay::OneClick::Base.commerce_code = "commercecode"
```

```python
# OneClick
from transbank import oneclick as BaseOneClick
from transbank.common.integration_type import IntegrationType

BaseOneClick.commerce_code = "codigo-comercio-aca"
```

### Apuntar a producción

Antes de operar en el ambiente de producción, debes pasar por un [proceso de validación](/documentacion/como_empezar#el-proceso-de-validacion), luego del cual te entregaremos 
tu Api Key Secret (**llave secreta**).  

Si ya tienes tu llave secreta, puedes revisar como configurar el SDK para usar este ambiente de producción en [esta sección](/documentacion/como_empezar#configuracion-de-produccion)


## Conciliación de Transacciones

Una vez hayas realizado transacciones en producción quedará un historial de
transacciones que puedes revisar entrando a
[www.transbank.cl](https://www.transbank.cl/). Si lo deseas  puedes realizar una
conciliación entre tu sistema y el reporte que entrega el portal.

### OneClick

Para realizar la conciliación debes seguir los siguientes pasos:

1. Iniciar sesión con tu usuario y contraseña en [www.transbank.cl](https://www.transbank.cl/)

2. Una vez entras al portal, en el menú principal presiona "Webpay", y luego "Reporte Transaccional"
![Paso 2](/images/documentacion/conciliacion1.png)

3. En la parte superior de la ventana puedes encontrar un buscador que te ayudará
a filtrar, según los parámetros que gustes, las transacciones que quieras cuadrar.
Para filtrar por las transacciones de OneClick, en el campo "Producto" debes
seleccionar **OneClick**.
![Paso 3](/images/documentacion/conciliacion2.png)

4. Dentro de la tabla en la imagen anterior puedes presionar el número de orden de
compra para abrir los detalles de la transacción. Es en esta sección donde podrás
encontrar y conciliar los parámetros devueltos por el SDK al confirmar una transacción.
![Paso 4](/images/documentacion/conciliacion3.png)

5. Sólo queda realizar la conciliación. A continuación puedes ver una lista de
parámetros que recibirás al momento de confirmar una transacción y a que fila
de la tabla "Detalles de la transacción" corresponden (la lista completa de
parámetros de OneClick la puedes encontrar
[acá](/referencia/webpay-soap#autorizar-un-pago-con-webpay-oneclick))

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorizationCode  <br> <i> xs:string </i> | Código de autorización
creditCardType  <br> <i> creditCardType </i> | Medio de Pago
last4CardDigits  <br> <i> xs:string </i> | Final número tarjeta
responseCode  <br> <i> xs:int </i> | Código de respuesta

## Más Funcionalidades

Consulta la referencia del API para más funcionalidades ofrecidas por Webpay
Plus y OneClick:

- [Transacciones Webpay Plus Mall](/referencia/webpay-soap#webpay-plus-mall) para
  realizar cargos atribuibles a múltiples comercios dentro de una agrupación
  denominada _mall_. Con esto puedes realizar una sola integración y cobrar a
  nombre de tus clientes o partners.

- [Realizar Captura Diferida](/referencia/webpay-soap#captura-diferida-webpay-plus) de manera que la autorización de
Webpay Plus Normal solo reserve el cupo y la captura de la transacción se pueda
realizar posteriormente.

- [Anular Transacciones Webpay Plus](/referencia/webpay-soap#anulacion-webpay-plus) para devolver dinero parcial o
totalmente.

- [Reversar Transacciones OneClick](/referencia/webpay-soap#reversar-un-pago-webpay-oneclick) para dejar sin efecto una
transacción realizada durante el día contable actual.

- [Anular Transacciones Webpay
  OneClick](/referencia/webpay-soap#anular-un-pago-webpay-oneclick) para dejar sin
  efecto una transacción realizada en otra fecha distinta al día contable
  actual.

- [Eliminar Inscripciones OneClick](/referencia/webpay-soap#eliminar-una-inscripcion-webpay-oneclick) para eliminar el `tbkUser`
cuando tus usuarios no quieren continuar con el servicio.

- [Transacciones OneClick Mall](/referencia/webpay-soap#webpay-oneclick-mall).

## Ejemplos de integración

Ponemos a tu disposición una serie de repositorios en nuestro Github para ayudarte a entender la integración de mejor forma.
Puedes encontrar una lista de [proyectos de ejemplo acá](/documentacion/como_empezar#ejemplos). 

En el caso de integrar webpay en una aplicación móvil Android, usando webview, debes tener presente la siguiente configuración:
1. Al momento de abrir el webview.

```js
// habilitar el Cookie Manager. Depende del nivel de la API de Android que se utilice se habilita de diferente forma
if (android.os.Build.VERSION.SDK_INT >= 21)
    CookieManager.getInstance().setAcceptThirdPartyCookies(myWebPayView, true); // myWebPayView es el WebView
else
    CookieManager.getInstance().setAcceptCookie(true);

// Asignar el caché en el webview
webPayView.getSettings().setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);
```

2. Al momento de cerrar el webview

```js
// Remover Cookies
if (android.os.Build.VERSION.SDK_INT >= 21)
    CookieManager.getInstance().removeAllCookies(null);
else
    CookieManager.getInstance().removeAllCookie();

// Borrar caché
myWebPayView.clearCache(true);
```

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
