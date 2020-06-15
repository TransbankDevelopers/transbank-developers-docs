# PatPass

## PatPass by Webpay %<span class='tbk-tagTitleDesc'>SOAP</span>%

<div class="pos-title-nav">
  <div tbk-link='/referencia/patpass' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugin/patpass' tbk-link-name='Plugins'></div>
</div>

### Crear una suscripción

Para una transacción PatPass by Webpay que registre una suscripción, lo primero que necesitas es preparar una instancia de `WebpayNormal`
con la `Configuration` que incluye el código de comercio y los certificados a
usar.

Una forma fácil de comenzar es usar la configuración para pruebas que viene
incluida en el SDK, indicando el correo que quieres que se use para las
notificaciones de PatPass

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.webpay.configuration.Configuration;
import cl.transbank.webpay.Webpay;
import cl.transbank.patpass.PatPassByWebpayNormal;
// ...

PatPassByWebpayNormal transaction = new Webpay(
        Configuration.forTestingPatPassByWebpayNormal("testpatpassbywebpay@mailinator.com")
    ).getPatPassByWebpayTransaction();
```

```php
PENDIENTE
```

```csharp
using Transbank.PatPass;
using Transbank.Webpay;
using Transbank.Webpay.Wsdl.Normal;
//...

PatPassByWebpayNormal transaction = new Webpay(
    Configuration.ForTestingPatPassByWebpayNormal("testpatpassbywebpay@mailinator.com")).
        PatPassByWebpayTransaction;
```

<aside class="notice">
Como necesitarás ese objeto `transaction` en múltiples ocasiones, es buena idea
encapsular la lógica que lo genera en algún método que puedas reutilizar.
</aside>

Una vez que ya cuentas con esa preparación, puedes iniciar transacciones:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpay.wswebpay.service.WsInitTransactionOutput;
// ...
double amount = 1000; // monto a cobrar cada mes
String sessionId = "identificador que será retornado en el callback de resultado";
String buyOrder = "identificador único de orden de compra";
String returnUrl = "https://callback/resultado/de/transaccion";
String finalUrl = "https://callback/final/post/comprobante/webpay";
PatPassInfo patPassInfo = new PatPassInfo()
    .setServiceId("335456675433")
    .setCardHolderId("11.111.111-1")
    .setCardHolderName("Juan Pedro")
    .setCardHolderLastName1("Alarcón")
    .setCardHolderLastName2("Perez")
    .setCardHolderMail("example@example.com")
    .setCellPhoneNumber("1234567")
    .setExpirationDate(new GregorianCalendar(2019, 1, 1));
WsInitTransactionOutput initResult = transaction.initTransaction(
        amount, sessionId, buyOrder, returnUrl, finalUrl, patPassInfo);

String formAction = initResult.getUrl();
String wsToken = initResult.getToken();
```

```php
PENDIENTE
```

```csharp
using Transbank.PatPass;
using Transbank.Webpay;
using Transbank.Webpay.Wsdl.Normal;
//...

decimal amount = 1000; // monto a cobrar cada mes
string sessionId = "identificador que será retornado en el callback de resultado";
string buyOrder = "identificador único de orden de compra";
string returnUrl = "https://callback/resultado/de/transaccion";
string finalUrl = "https://callback/final/post/comprobante/webpay";

PatPassInfo info = new PatPassInfo
{
    ServiceId = "335456675433",
    CardHolderId = "11.111.111-1",
    CardHolderName = "Juan Pedro",
    CardHolderLastName1 = "Alarcón",
    CardHolderLastName2 = "Perez",
    CardHolderMail = "example@example.com",
    CellPhoneNumber = "1234567",
    ExpirationDate = new System.DateTime(2019, 01, 01)
};
wsInitTransactionOutput initResult = transaction.initTransaction(
    amount, buyOrder, sessionId, returnUrl, finalUrl, info);

string formAction = initResult.url;
string wsToken = initResult.token;
```

Todo es muy similar a Webpay Normal, con la única diferencia de la información
específica de PatPass que incluye los datos personales del usuario (donde es
importante el correo electrónico para las notificaciones de PatPass) y la fecha
de expiración de la suscripción.

La URL y el token retornados te indican donde debes redirigir al usuario para
que comience el flujo de pago. Esta redirección debe ser vía `POST` por lo que
deberás crear un formulario web con un campo `token_ws` hidden y enviarlo
programáticamente para entregar el control a Webpay.

<aside class="notice">
Tip: En el ambiente de integración puedes usar la tarjeta VISA 4051885600446623
para hacer pruebas simples de éxito. El CVV es 123 y la fecha de vencimiento
cualquiera superior a la fecha actual. Luego para la autenticación bancaria usa
el RUT 11.111.111-1 y la clave 123. Para pruebas exhaustivas [consulta todas las
tarjetas de prueba en la sección de
Ambientes](/documentacion/como_empezar#ambientes).
</aside>

### Confirmar una suscripción

Una vez que el tarjetahabiente ha pagado (o declinado, o ha ocurrido un error),
Webpay retornará el control vía `POST` a la URL que indicaste en el `returnUrl`.
Recibirás también el parámetro `token_ws` que te permitirá conocer el resultado
de la transacción:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpay.wswebpay.service.TransactionResultOutput;
import com.transbank.webpay.wswebpay.service.WsTransactionDetailOutput;
// ...
TransactionResultOutput result =
    transaction.getTransactionResult(request.getParameter("token_ws"));
WsTransactionDetailOutput output = result.getDetailOutput().get(0);
if (output.getResponseCode() == 0) {
    // Suscripción exitosa, puedes procesar el resultado con el contenido de
    // las variables result y output.
}
```

```php
PENDIENTE
```

```csharp
using Transbank.PatPass;
using Transbank.Webpay;
using Transbank.Webpay.Wsdl.Normal;
//..

transactionResultOutput result =
    transaction.getTransactionResult(Request.Form["token_ws"]);
wsTransactionDetailOutput output = result.detailOutput[0];
if (output.responseCode == 0){
    // Suscripción exitosa, puedes procesar el resultado con el contenido de
    // las variables result y output.
}
```

> Los SDKs se encarga de que al mismo tiempo que se obtiene el resultado de la
> transacción se haga el _acknowledge_ a Transbank de manera que no haya
> posibilidad de que la transacción se revierta.

En el caso exitoso deberás llevar el control vía `POST` nuevamente a Webpay para
que el tarjetahabiente vea el comprobante que le deja claro que se ha realizado
el cargo en su tarjeta. Nuevamente deberás generar un formulario con el
`token_ws` como un campo hidden. La URL para redirigir la debes obtener desde:

<div class="language-simple" data-multiple-language></div>

```java
result.getUrlRedirection()
```

```php
PENDIENTE
```

```csharp
result.urlRedirection
```

Finalmente después del comprobante Webpay redirigirá otra vez (vía `POST`) a tu
sitio, esta vez a la URL que indicaste en el `finalUrl` cuando iniciaste la
transacción. Tal como antes, recibirás el `token_ws` que te permitirá
identificar la transacción y mostrar un comprobante o página de éxito a tu
usuario.

## Patpass Comercio %<span class='tbk-tagTitleDesc'>REST</span>%

<div class="pos-title-nav">
<div tbk-link='/referencia/patpass' tbk-link-name='Referencia Api'></div>
</div>

### Crear una suscripción

Para crear una transaccion **PatPass Comercio** que registre una suscripción, lo primero que necesitas es una instancia de `WebpayPatpassComercio` con la `Configuration` que incluye el código de comercio y el `Api Key` a usar.

Una forma fácil de comenzar es utilizar la configuración para pruebas que viene incluida en el SDK.

```java
import cl.transbank.patpass.PatpassComercio;
import cl.transbank.patpass.model.PatpassComercioInscriptionStartResponse;
import cl.transbank.patpass.model.PatpassComercioTransactionStatusResponse;

PatpassCommercio = new PatpassOptions();

```

```php

use Transbank\Patpass\Options;
use Transbank\Patpass\PatpassComercio;

$commerceCode = "codigo de comercio en String";
$apiKey = "Api Key en String";
$integrationType = "TEST / LIVE dependiendo de tu ambiente de integracion";

PatpassComercio::setCommerceCode($commerceCode);
PatpassComercio::setApiKey($apiKey);
PatpassComercio::setIntegrationType($integrationType);

```
```csharp
using Transbank.Common;
using Transbank.Patpass.PatpassComercio;

PatpassComercio.CommerceCode = "codigo de comercio en String";
PatpassComercio.ApiKey = "Api Key en String";
PatpassComercio.IntegrationType = "TEST / LIVE dependiendo de tu ambiente de integracion";


```
```ruby

```
```python

```

Te recomendamos encapsular estas asignaciones en una función para que puedas reutilizarlas en los demás métodos.

Una vez este preparado el ambiente y la integracion, puedes iniciar la transacción sin problemas.

```java
import cl.transbank.patpass.PatpassComercio;
import cl.transbank.patpass.model.PatpassComercioInscriptionStartResponse;
import cl.transbank.patpass.model.PatpassComercioTransactionStatusResponse;

String url = request.getRequestURL().toString().replace("start","end-subscription");
String name = "nombre";
String firstLastName = "apellido";
String secondLastName = "sapellido";
String rut = "14140066-5";
String serviceId = String.valueOf(new Random().nextInt(Integer.MAX_VALUE));;
String finalUrl = request.getRequestURL().toString().replace("start","voucher-generated");
String commerceCode = "28299257";
double maxAmount = 1500;
String phoneNumber = "123456734";
String mobileNumber = "123456723";
String patpassName = "nombre del patpass";
String personEmail = "otromail@persona.cl";
String commerceEmail = "otrocomercio@comercio.cl";
String address = "huerfanos 101";
String city = "Santiago";

final PatpassComercioInscriptionStartResponse response = PatpassComercio.Inscription.start(url,
                    name,
                    firstLastName,
                    secondLastName,
                    rut,
                    serviceId,
                    finalUrl,
                    commerceCode,
                    maxAmount,
                    phoneNumber,
                    mobileNumber,
                    patpassName,
                    personEmail,
                    commerceEmail,
                    address,
                    city);
```
```php
use Transbank\Patpass\Options;
use Transbank\Patpass\PatpassComercio;

$returnUrl = "https://callback_url/resultado/de/la/transaccion";
$name = "Nombre";
$lastname1 = "Primer Apellido";
$lastname2 = "Segundo Apellido";
$rut = "11111111-1";
$serviceId = "Identificador del servicio unico de suscripción";
$finalUrl = "https://callback/final/comprobante/transacción";
$commerceCode = "Código de comercio";
$maxAmount = 10000; # monto máximo de la suscripción
$telephone = "numero telefono fijo de suscrito";
$mobilePhone = "numero de telefono móvil de suscrito";
$patpassName = "Nombre asignado a la suscripción";
$clientEmail = "Correo de suscrito";
$commerceEmail = "Correo de comercio";
$address = "Dirección de Suscrito";
$city = "Ciudad de suscrito";

$response = PatpassComercio\Inscription::Start(
  $returnUrl,
  $name,
  $lastname1,
  $lastname2,
  $rut,
  $serviceId,
  $finalUrl,
  $commerceCode,
  $maxAmount,
  $telephone,
  $mobilePhone,
  $patpassName,
  $clientEmail,
  $commerceEmail,
  $address,
  $city
);

```
```csharp
using Transbank.Patpass.PatpassComercio;
// ...

var returnUrl = "https://callback_url/resultado/de/la/transaccion";
var name = "Nombre";
var lastname1 = "Primer Apellido";
var lastname2 = "Segundo Apellido";
var rut = "11111111-1";
var serviceId = "Identificador del servicio unico de suscripción";
var finalUrl = "https://callback/final/comprobante/transacción";
var commerceCode = "Código de comercio";
var maxAmount = 10000; # monto máximo de la suscripción
var telephone = "numero telefono fijo de suscrito";
var mobilePhone = "numero de telefono móvil de suscrito";
var patpassName = "Nombre asignado a la suscripción";
var clientEmail = "Correo de suscrito";
var commerceEmail = "Correo de comercio";
var address = "Dirección de Suscrito";
var city = "Ciudad de suscrito";


var response = Inscription.Start(
    returnUrl,
    name,
    lastname1,
    lastname2,
    rut,
    serviceId,
    finalUrl,
    commerceCode,
    maxAmount,
    telephone,
    mobilePhone,
    patpassName,
    clientEmail,
    commerceEmail,
    address,
    city
);

```
```ruby
@url = "https://callback_url/resultado/de/la/transaccion"
@name = "Nombre"
@first_last_name = "Primer Apellido"
@second_last_name = "Segundo Apellido"
@rut = "11111111-1"
@service_id = "Identificador del servicio unico de suscripción"
@final_url = "https://callback/final/comprobante/transacción"
@max_amount = 10000; # monto máximo de la suscripció
@phone_number = "numero telefono fijo de suscrito"
@mobile_number = "numero de telefono móvil de suscrito"
@patpass_name = "Nombre asignado a la suscripción"
@person_email = "Correo de suscrito"
@commerce_email = "Correo de comercio"
@address = "Dirección de Suscrito"
@city = "Ciudad de suscrito"

@resp  = Transbank::Patpass::PatpassComercio::Inscription::start(
                                                    url: @url,
                                                    name: @name,
                                                    first_last_name: @first_last_name,
                                                    second_last_name: @second_last_name,
                                                    rut: @rut,
                                                    service_id: @service_id,
                                                    final_url: @final_url,
                                                    max_amount: @max_amount,
                                                    phone_number: @phone_number,
                                                    mobile_number: @mobile_number,
                                                    patpass_name: @patpass_name,
                                                    person_email: @person_email,
                                                    commerce_email: @commerce_email,
                                                    address: @address,
                                                    city: @city
                                                    )
```
```python
from transbank.error.transbank_error import TransbankError
from transbank.patpass_comercio.inscription import Inscription

return_url = "https://callback_url/resultado/de/la/transaccion"
name = "Nombre"
first_last_name = "Primer Apellido"
second_last_name = "Segundo Apellido"
rut = "11111111-1"
service_id = "Identificador del servicio unico de suscripción"
final_url = "https://callback/final/comprobante/transacción"
max_amount = 10000; # monto máximo de la suscripció
phone_number = "numero telefono fijo de suscrito"
mobile_number = "numero de telefono móvil de suscrito"
patpass_name = "Nombre asignado a la suscripción"
person_email = "Correo de suscrito"
commerce_mail = "Correo de comercio"
address = "Dirección de Suscrito"
city = "Ciudad de suscrito"

response = Inscription.start(return_url, name, first_last_name, second_last_name, rut, service_id, final_url,
                                       max_amount, phone_number, mobile_number, patpass_name,
                                       person_email, commerce_mail, address, city)

```


### Confirmar suscripción

Para confirmar la suscripción se debe enviar el token generado en la respuesta a la url tambien generada en la respuesta con el metodo post en un formulario.

```java
<form action="${model.url_webpay}" method="post" name="tokenForm">
    <input type="hidden" name="tokenComercio" value="${model.tbk_token}">
    <input type="submit" class="btn btn-success" value="Start Patpass comercio">
</form>
```

```php
<form method="POST" action="$res['url']">
  <input type="hidden" name="tokenComercio" value="$res['token']" />
  <input type="submit" value="Finalizar La Inscripcion" />
</form>
```

```csharp
<form method="POST" action="@ViewBag.Url">
  <input type="hidden" name="tokenComercio" value="@ViewBag.Token" />
  <input type="submit" value="Finalizar La Inscripcion" />
</form>

```

```ruby
<form action="<%= @resp.url %>" method="post">
  <input type="hidden" name="tokenComercio" value="<%= @resp.token %>">
  <button type="submit">Finalizar La Inscripcion</button>
</form>
```

```python
<form action="{{ response.url }}" method="POST">
  <input type="hidden" name="tokenComercio" value="{{ response.token }}">
  <input type="submit" value="Finalizar La Inscripcion">
</form>
```

### Estado de la suscripción

Una vez que el tarjetahabiente ha pagado (o declinado, o ha ocurrido un error), Webpay retornará el control vía POST a la URL que indicaste en el returnUrl. Recibirás también el parámetro token_ws que te permitirá conocer el resultado de la transacción:

```java
import cl.transbank.patpass.model.PatpassComercioTransactionStatusResponse;

final WebpayApiRequest request = new PatpassComercioTransactionStatusRequest(token);
```

```php
use Transbank\Patpass\PatpassComercio;

$response = PatpassComercio\Inscription::Status($token);
```

```csharp
using Transbank.Patpass.PatpassComercio;

var token = Request.Form["token_ws"];
var result = Inscription.Status(token);

```

```ruby
@req = params.as_json
@token = @req['j_token']
@resp = Transbank::Patpass::PatpassComercio::Inscription::status(token: @token)
```

```python
from transbank.error.transbank_error import TransbankError
from transbank.patpass_comercio.inscription import Inscription

token = request.form.get("tokenComercio")
try:
    response = Inscription.status(token)
except TransbankError as e:
    print(e.message)
```


## Credenciales y Ambiente

Las credenciales de PatPass by Webpay se configuran de igual forma a las
transacciones Webpay: en base a un objeto `Configuration`. Y si bien para hacer
pruebas iniciales pueden usarse las credenciales pre-configuradas (como se puede
ver en todos los ejemplos anteriores), para poder superar el [proceso de
validación](/documentacion/como_empezar#el-proceso-de-validacion-y-puesta-en-produccion)
en el ambiente de integración será necesario configurar explícitamente tu código
de comercio y certificados:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.webpay.configuration.Configuration;
import cl.transbank.webpay.Webpay;
import cl.transbank.patpass.PatPassByWebpayNormal;

//...

Configuration configuration = new Configuration();
configuration.setCommerceCode("12345"); // acá va tu código de comercio
configuration.setPrivateKey( // pega acá la llave privada de tu certificado
    "-----BEGIN RSA PRIVATE KEY-----\n" +
    "MIIEpQIBAAKCAQEA0ClVcH8RC1u+KpCPUnzYSIcmyXI87REsBkQzaA1QJe4w/B7g\n" +
    //....
    "MtdweTnQt73lN2cnYedRUhw9UTfPzYu7jdXCUAyAD4IEjFQrswk2x04=\n" +
    "-----END RSA PRIVATE KEY-----");
configuration.setPublicCert( // pega acá tu certificado público
    "-----BEGIN CERTIFICATE-----\n" +
    "MIIDujCCAqICCQCZ42cY33KRTzANBgkqhkiG9w0BAQsFADCBnjELMAkGA1UEBhMC\n" +
    //....
    "-----END CERTIFICATE-----");

// Default significa usar pesos o UF (dependiendo del código de comercio).
configuration.setPatPassCurency(PatPassByWebpayNormal.Currency.DEFAULT);
// Si se quiere usar UFs, especificar PatPassByWebpayNormal.Currency.UF.
configuration.commerceMail("mail-para-notificaciones-patpass@micomercio.cl");

Webpay webpay = new Webpay(configuration);
// Ahora puedes obtener las instancias de las transacciones
// que usarás, por ejemplo:
PatPassByWebpayNormal patPassTransaction = webpay.getPatPassByWebpayTransaction();
```

```php
PENDIENTE
```

```csharp
using Transbank.PatPass;
using Transbank.Webpay;
using Transbank.Webpay.Wsdl.Normal;
//..

Configuration configuration = new Configuration()
{
    CommerceCode = "12345", // acá va tu código de comercio
    PrivateCertPfxPath = @"C:\Certs\certificado.pfx", // pega acá la ruta a tu archivo pfx o p12
    Password = "secret123" // pega acá el secreto con el cual se genero el archivo pfx o p12
    CommerceMail = "mail-para-notificaciones-patpass@micomercio.cl",
    PatPassCurrency = PatPassByWebpayNormal.Currency.DEFAULT
};

Webpay webpay = new Webpay(configuration);
// Ahora puedes obtener las instancias de las transacciones
// que usarás, por ejemplo:
PatPassByWebpayNormal patPassTransaction = webpay.PatPassByWebpayTransaction;
```

<aside class="warning">
A diferencia de otros SDK, en .NET debes especificar la ruta a un archivo pfx o p12
el cual debes generar tu a partir de tu llave privada y certificado público.

Puedes mirar el siguiente enlace para obtener una guía rápida de como generar tu
propio archivo: [Crear archivo pfx usando openssl](https://www.ssl.com/how-to/create-a-pfx-p12-certificate-file-using-openssl/)
</aside>

### Apuntar a producción

Para cambiar el ambiente al que apunta el SDK (que por defecto es integración),
también debes usar el objeto `Configuration` (antes de crear una instancia de
`Webpay`):

<div class="language-simple" data-multiple-language></div>

```java
Configuration configuration = new Configuration();
configuration.setEnvironment(Webpay.Environment.PRODUCCION);
// agregar también configuración del código de comercio y certificados
```

```php
PENDIENTE
```

```csharp
Configuration configuration = new Configuration();
configuration.Environment("PRODUCCION");
// agregar también configuración del código de comercio y certificados
```

<aside class="warning">
Los SDKs se encarga de configurar el certificado público de Transbank
correspondiente al ambiente seleccionado. Pero es tu responsabilidad
actualizar periódicamente la versión del SDK para recibir actualizaciones a
dicho certificado. De lo contrario, expirará el certificado y dejarás de poder
operar con Transbank.
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
