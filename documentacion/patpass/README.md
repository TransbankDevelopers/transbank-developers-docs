# PatPass

## Patpass Comercio %<span class='tbk-tagTitleDesc'>REST</span>%

### Crear una suscripción

Para crear una transacción **PatPass Comercio** que registre una suscripción, lo primero que necesitas es una instancia de `WebpayPatpassComercio` con la `Configuration` que incluye el código de comercio y el `Api Key` a usar.

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
# ...
```

```python
# ...
```

Te recomendamos encapsular estas asignaciones en una función para que puedas reutilizarlas en los demás métodos.

Una vez este preparado el ambiente y la integración, puedes iniciar la transacción sin problemas.

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
