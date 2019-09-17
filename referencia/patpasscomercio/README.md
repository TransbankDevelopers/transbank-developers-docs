# PatPass Comercio

## Ambientes y Credenciales

La API REST de PatPass Comercio está protegida para garantizar solamente comercios autorizados por Transbank hagan uso de las operaciones disponibles. La seguridad esta implementada mediante los siguientes mecanismos:

* Canal seguro a través de TLSv1.2 para la comunicación del cliente con Webpay.
* Autenticación y autorización mediante el intercambio de headers Authorization y commerceCode.

### Ambiente de Producción

Las URLs de endpoints de producción están alojados dentro de <https://pagoautomaticocontarjetas.cl/>

```java

```

```php

```

```csharp

```

```ruby

```

```python

```

```http
Host: https://pagoautomaticocontarjetas.cl/
```

### Ambiente de Integración

```java

```

```php

```

```csharp

```

```ruby

```

```python

```

```http
Host: https://pagoautomaticocontarjetasint.transbank.cl
```

Las URLs de endpoints de integración están alojados dentro de
<https://pagoautomaticocontarjetasint.transbank.cl>.

Consulta [la documentación para conocer las tarjetas de prueba que funcionan en el ambiente de integración](/documentacion/como_empezar#ambientes).

### Credenciales del Comercio

```java
// Este SDK aún no se encuentra disponible
```

```php
// Este SDK aún no se encuentra disponible
```

```csharp
// Este SDK aún no se encuentra disponible
```

```ruby
# Este SDK aún no se encuentra disponible
```

```python
# Este SDK aún no se encuentra disponible
```

```http
Authorizacion: Próximamente...
commerceCode: Próximamente...
Content-Type: application/json
```

Todas las peticiones que hagas deben incluir el código de comercio y la llave secreta entregada por Transbank, actuando ambas como las credenciales que autorizan distintas operaciones.

<aside class="notice">
Ten en cuenta que tu(s) código(s) de comercio en ambiente de producción no son iguales a los entregados para el ambiente de integración.
</aside>

En [el repositorio GitHub
`transbank-webpay-credenciales`](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/)
podrás encontrar [códigos de comercios y llaves secretas para probar
en integración aunque aún no tengas tu propio código de
comercio](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/tree/master/integracion).

> Los SDKs ya incluyen esos códigos de comercio y llaves secretas que funcionan en el ambiente de integración, por lo que puedes obtener rápidamente una configuración lista para hacer tus primeras pruebas en dicho ambiente:

```java
// Este SDK aún no se encuentra disponible
```

```php
// Este SDK aún no se encuentra disponible
```

```csharp
// Este SDK aún no se encuentra disponible
```

```ruby
# Este SDK aún no se encuentra disponible
```

```python
# Este SDK aún no se encuentra disponible
```

```http

```

### PatPass Comercio Start

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
PENDIENTE
```
```python
PENDIENTE
```

```http
POST /restpatpass/v1/services/patInscription

Content-Type:application/json
Authorization: API-key
commercecode: codigo de comercio

{
    "url": "/urlRetorno",
    "nombre": "nombre",
    "pApellido": "apellido",
    "sApellido": "sapellido",
    "rut": "14959787-6",
    "serviceId": "76654654654",
    "finalUrl": "/urlrfinal",
    "commerceCode": "codigo de comerico",
    "montoMaximo": 1500,
    "telefonoFijo": "012356545",
    "telefonoCelular": "99999999",
    "nombrePatPass": "nombre del patpass",
    "correoPersona": "persona@persona.cl",
    "correoComercio": "comercio@comercio.cl",
    "direccion": "huerfanos 101",
    "ciudad": "Santiago"
}

```
Una suscripción de PatPass Comercio corresponde a una solicitud de pago recurrente con tarjeta de crédito, en donde el primer pago se resuelve al instante, y los subsiguientes quedan programados para ser ejecutados mes a mes, PatPass Comercio cuenta con fecha de caducidad o término, la cual debe ser proporcionada junto a otros datos para esta transacción. La transacción solo puede ser realizada en pesos.

EL flujo de pago de PatPass Comercio se inicia desde el comercio, en donde el tarjetahabiente selecciona el producto o servicio. Una vez realizado esto, elige pagar con PatPass Comercio, en donde se despliega el formulario de pago y se completan los datos requeridos, una vez enviado los datos se realiza un proceso de autentificacion y confirmacion de datos de la suscripcion. Una vez resuelta la autentificacion, se procede a aprobar la suscripcion. Posterior a eso se genera un compobante de la suscripcion en si.

Dentro de los atributos más relevantes se puede mencionar:

- Permite realizar transacciones seguras y en línea a través de Internet.
- En transacciones con PatPass Comercio se solicita al tarjetahabiente autenticarse con su emisor, protegiendo de esta forma al comercio por eventuales fraudes o desconocimientos de compra.
- La seguridad es reforzada por medio de la utilización de servidores seguros, protegidos con TLS 1.2


### Flujo en caso de éxito

De cara al tarjetahabiente, el flujo de páginas para la transacción es el siguiente:

![Flujo de páginas PatPass Comercio](/images/flujo-paginas-webpay.png)

Desde el punto de vista técnico, la secuencia es la siguiente:

![Diagrama de secuencia PatPass Comercio](/images/referencia/patpasscomercio/flujo-patpass-normal.png)
