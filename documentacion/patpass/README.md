# PatPass

## Patpass Comercio %<span class='tbk-tagTitleDesc'>REST</span>%

### Crear una suscripción

Para crear una transacción **PatPass Comercio** que registre una suscripción, lo primero que necesitas es una instancia de `WebpayPatpassComercio` con la `Configuration` que incluye el código de comercio y el `Api Key` a usar.

Una forma fácil de comenzar es utilizar la configuración para pruebas que viene incluida en el SDK.

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.patpass.PatpassComercio;
import cl.transbank.patpass.model.PatpassComercioInscriptionStartResponse;
import cl.transbank.patpass.model.PatpassComercioTransactionStatusResponse;

// Versión 3.x del SDK
PatpassComercio.Inscription inscription = new PatpassComercio.Inscription(new PatpassOptions(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, IntegrationType.TEST));

// Versión 2.x del SDK
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

// Versión 4.x del SDK
var inscription = new Inscription(new Options(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, IntegrationType.TEST));

// Versión 3.x del SDK
PatpassComercio.CommerceCode = "codigo de comercio en String";
PatpassComercio.ApiKey = "Api Key en String";
PatpassComercio.IntegrationType = "TEST / LIVE dependiendo de tu ambiente de integracion";
```

```ruby
// Versión 2.x del SDK
@inscription = Transbank::Patpass::PatpassComercio::Inscription.new(::Transbank::Common::IntegrationCommerceCodes::PATPASS_COMERCIO, ::Transbank::Common::IntegrationApiKeys::PATPASS_COMERCIO, :integration)
```

```python
// Versión 3.x del SDK
ins = Inscription(PatpassComercioOptions(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, IntegrationType.TEST))
# ...
```

Te recomendamos encapsular estas asignaciones en una función para que puedas reutilizarlas en los demás métodos.

Una vez este preparado el ambiente y la integración, puedes iniciar la transacción sin problemas.

<div class="language-simple" data-multiple-language></div>

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
double maxAmount = 1500;
String phoneNumber = "123456734";
String mobileNumber = "123456723";
String patpassName = "nombre del patpass";
String personEmail = "otromail@persona.cl";
String commerceEmail = "otrocomercio@comercio.cl";
String address = "huerfanos 101";
String city = "Santiago";

// Versión 3.x del SDK
PatpassComercio.Inscription inscription = new PatpassComercio.Inscription(new PatpassOptions(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, IntegrationType.TEST));
final PatpassComercioInscriptionStartResponse response = inscription.start(url,
                    name,
                    firstLastName,
                    secondLastName,
                    rut,
                    serviceId,
                    finalUrl,
                    maxAmount,
                    phoneNumber,
                    mobileNumber,
                    patpassName,
                    personEmail,
                    commerceEmail,
                    address,
                    city);  

// Versión 2.x del SDK
final PatpassComercioInscriptionStartResponse response = PatpassComercio.Inscription.start(url,
                    name,
                    firstLastName,
                    secondLastName,
                    rut,
                    serviceId,
                    finalUrl,
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
var maxAmount = 10000; # monto máximo de la suscripción
var telephone = "numero telefono fijo de suscrito";
var mobilePhone = "numero de telefono móvil de suscrito";
var patpassName = "Nombre asignado a la suscripción";
var clientEmail = "Correo de suscrito";
var commerceEmail = "Correo de comercio";
var address = "Dirección de Suscrito";
var city = "Ciudad de suscrito";

// Versión 4.x del SDK
var inscription = new Inscription(new Options(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, PatpassComercioIntegrationType.Test));
var result = inscription.Start(
                url: url,
                name: name,
                lastName: f_lastname,
                secondLastName: s_lastname,
                rut: rut,
                serviceId: service_id,
                finalUrl: final_url,
                maxAmount: max_amount,
                phone: phone_number,
                cellPhone: mobile_number,
                patpassName: patpass_name,
                personEmail: client_email,
                commerceEmail: commerce_email,
                address: address,
                city: city);

// Versión 3.x del SDK
var response = Inscription.Start(
    returnUrl,
    name,
    lastname1,
    lastname2,
    rut,
    serviceId,
    finalUrl,
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

// Versión 2.x del SDK
@inscription = Transbank::Patpass::PatpassComercio::Inscription.new()
@resp = @inscription.start(
      @url,
      @name,
      @first_last_name,
      @second_last_name,
      @rut,
      @service_id,
      @final_url,
      @max_amount,
      @phone_number,
      @mobile_number,
      @patpass_name,
      @person_email,
      @commerce_email,
      @address,
      @city
    )

// Versión 1.x del SDK
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
// Versión 3.x del SDK
from transbank.patpass_comercio.inscription import Inscription

// Versión 2.x del SDK
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

// Versión 3.x del SDK
ins = Inscription(PatpassComercioOptions(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, IntegrationType.TEST))
resp = ins.start(return_url, name, first_last_name, second_last_name, rut, service_id, None,
                                       max_amount, phone_number, mobile_number, patpass_name,
                                       person_email, commerce_mail, address, city)
// Versión 2.x del SDK
resp = Inscription.start(return_url, name, first_last_name, second_last_name, rut, service_id, final_url,
                                       max_amount, phone_number, mobile_number, patpass_name,
                                       person_email, commerce_mail, address, city)

```

### Confirmar suscripción

Para confirmar la suscripción se debe enviar el token generado en la respuesta a la url tambien generada en la respuesta con el metodo post en un formulario.

<div class="language-simple" data-multiple-language></div>

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

<div class="language-simple" data-multiple-language></div>

```java
// Versión 3.x del SDK
import cl.transbank.patpass.responses.PatpassComercioTransactionStatusResponse;

PatpassComercio.Inscription inscription = new PatpassComercio.Inscription(new PatpassOptions(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, IntegrationType.TEST));
final PatpassComercioTransactionStatusResponse response = inscription.status(token);

// Versión 2.x del SDK
import cl.transbank.patpass.model.PatpassComercioTransactionStatusResponse;

final PatpassComercioTransactionStatusResponse response = PatpassComercio.Transaction.status(token);
```

```php
use Transbank\Patpass\PatpassComercio;

$response = PatpassComercio\Inscription::Status($token);
```

```csharp
using Transbank.Patpass.PatpassComercio;

var token = Request.Form["token_ws"];

// Versión 4.x del SDK
var inscription = new Inscription(new Options(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, PatpassComercioIntegrationType.Test));
var result = inscription.Status(token);

// Versión 3.x del SDK
var result = Inscription.Status(token);

```

```ruby
@req = params.as_json
@token = @req['j_token']

// Versión 2.x del SDK
@inscription = Transbank::Patpass::PatpassComercio::Inscription.new()
@resp = @inscription.status(token: @token)

// Versión 1.x del SDK
@resp = Transbank::Patpass::PatpassComercio::Inscription::status(token: @token)
```

```python
// Versión 3.x del SDK
from transbank.patpass_comercio.inscription import Inscription

// Versión 2.x del SDK
from transbank.patpass_comercio.inscription import Inscription

token = request.form.get("tokenComercio")

// Versión 3.x del SDK
ins = Inscription(PatpassComercioOptions(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, IntegrationType.TEST))
resp = ins.status(token)

// Versión 2.x del SDK
resp = Inscription.status(token)

```

## Credenciales y Ambiente

Las credenciales de PatPass Comercio se configuran de la siguiente forma:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.common.IntegrationType;
import cl.transbank.patpass.PatpassComercio;
//...

// Versión 3.x del SDK
PatpassComercio.Inscription inscription = new PatpassComercio.Inscription(new PatpassOptions(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, IntegrationType.TEST));

// Versión 2.x del SDK
PatpassComercio.setIntegrationType(IntegrationType.TEST);
PatpassComercio.setApiKey("******");
PatpassComercio.setCommerceCode("******");
```

```php
use Transbank\Patpass\PatpassComercio;
//...

PatpassComercio::configureForTesting();
```

```csharp
using Transbank.Patpass.PatpassComercio;
//..

// Versión 4.x del SDK
var inscription = new Inscription(new Options(IntegrationCommerceCodes.PATPASS_COMERCIO, IntegrationApiKeys.PATPASS_COMERCIO, PatpassComercioIntegrationType.Test));

// Versión 3.x del SDK
Inscription.IntegrationType = PatpassComercioIntegrationType.Test;
Inscription.CommerceCode = "*******";
Inscription.ApiKey = "*******";
```

### Apuntar a producción

Para cambiar el ambiente al que apunta el SDK (que por defecto es integración), debes hacer lo siguiente:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.common.IntegrationType;
import cl.transbank.patpass.PatpassComercio;
//...

// Versión 3.x del SDK
PatpassComercio.Inscription inscription = new PatpassComercio.Inscription(new PatpassOptions("***ApiKey****", "***CommerceCode****", IntegrationType.LIVE));

// Versión 2.x del SDK
PatpassComercio.setIntegrationType(IntegrationType.LIVE);
```

```php
use Transbank\Patpass\PatpassComercio;
//...

PatpassComercio::setCommerceCode('*********');
PatpassComercio::setApiKey('*********');
PatpassComercio::setIntegrationType('LIVE');
```

```csharp
using Transbank.Patpass.PatpassComercio;
//..
// Versión 4.x del SDK
var inscription = new Inscription(new Options("***ApiKey****", "***CommerceCode****", PatpassComercioIntegrationType.Live));

// Versión 3.x del SDK
Inscription.IntegrationType = PatpassComercioIntegrationType.Live;
```

## Ejemplos de integración

Ponemos a tu disposición una serie de repositorios en nuestro Github para ayudarte a entender la integración de mejor forma.
Puedes encontrar una lista de [proyectos de ejemplo acá](/documentacion/como_empezar#ejemplos).

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
