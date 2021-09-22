# Patpass Rest

## Patpass Comercio

### Ambientes y credenciales

La API REST de Patpass está protegida para garantizar que solamente comercios autorizados por Transbank hagan uso de las operaciones disponibles. La seguridad esta implementada mediante los siguientes mecanismos:

* Canal seguro a través de TLSv1.2 para la comunicación del cliente con Webpay.
* Autenticación y autorización mediante el intercambio de headers `Tbk-Api-Key-Id` y `Tbk-Api-Key-Secret`.
  
<strong>Ambiente de Producción</strong>

Las URLs de endpoints de producción están alojados dentro de
<https://www.pagoautomaticocontarjetas.cl//>.

```java
// Host: https://www.pagoautomaticocontarjetas.cl/
```

```php
// Host: https://www.pagoautomaticocontarjetas.cl/
```

```csharp
// Host: https://www.pagoautomaticocontarjetas.cl/
```

```ruby
# Host: https://www.pagoautomaticocontarjetas.cl/
```

```python
# Host: https://www.pagoautomaticocontarjetas.cl/
```

```http
Host: https://www.pagoautomaticocontarjetas.cl/
```

<strong>Ambiente de Integración</strong>

Las URLs de endpoints de integración están alojados dentro de
<https://pagoautomaticocontarjetasint.transbank.cl//>.

Consulta [la documentación para conocer las tarjetas de prueba que funcionan en
el ambiente de integración](/documentacion/como_empezar#ambientes).

```java
// Host: https://pagoautomaticocontarjetasint.transbank.cl/
```

```php
// Host: https://pagoautomaticocontarjetasint.transbank.cl/
```

```csharp
// Host: https://pagoautomaticocontarjetasint.transbank.cl/
```

```ruby
# Host: https://pagoautomaticocontarjetasint.transbank.cl/
```

```python
# Host: https://pagoautomaticocontarjetasint.transbank.cl/
```

```http
Host: https://pagoautomaticocontarjetasint.transbank.cl/
```

### Credenciales del Comercio

```java
// Tbk-Api-Key-Id: 28299257
// Tbk-Api-Key-Secret: cxxXQgGD9vrVe4M41FIt
```

```php
// Tbk-Api-Key-Id: 28299257
// Tbk-Api-Key-Secret: cxxXQgGD9vrVe4M41FIt
```

```csharp
// Tbk-Api-Key-Id: 28299257
// Tbk-Api-Key-Secret: cxxXQgGD9vrVe4M41FIt
```

```ruby
# Tbk-Api-Key-Id: 28299257
# Tbk-Api-Key-Secret: cxxXQgGD9vrVe4M41FIt
```

```python
# Tbk-Api-Key-Id: 28299257
# Tbk-Api-Key-Secret: cxxXQgGD9vrVe4M41FIt
```

```http
Tbk-Api-Key-Id: 28299257
Tbk-Api-Key-Secret: cxxXQgGD9vrVe4M41FIt
Content-Type: application/json
```

### Iniciar Inscripción

Para iniciar una inscripción debes llamar al método `Inscription.start`

<strong>Inscription.start()</strong>

Permite gatillar el inicio del proceso de inscripción. Los datos ingresados serán precargados en el formulario al momento de mostrarse.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el token es caducado y no podrá ser utilizado en un pago.
</aside>

```java
final PatpassComercioInscriptionStartResponse response = PatpassComercio.Inscription.start(
    'http://misitio.cl/finalizar_suscripcion',          // URL
    'Diego',                                            // Nombre
    'Sanchez',                                          // Primer apellido
    'Valdovinos',                                       // Segundo apellido
    '12345678-9',                                       // Rut
    '323123',                                           // Id de servicio
    'http://misitio.cl/voucher',                        // URL Final
    0,                                                  // Monto máximo
    '57508624',                                         // Teléfono fijo
    '57508624',                                         // Teléfono celular
    'Help - 8050014',                                   // Nombre PatPass
    'persona@test.cl',                                  // Email Persona
    'comercio@test.cl',                                 // Email Comercio
    'Merced 156, Santiago, Chile',                      // Dirección
    'Santiago'                                          // Ciudad
);
```

```php
$response = PatpassComercio\Inscription::start(  
    'http://misitio.cl/finalizar_suscripcion',          // URL
    'Diego',                                            // Nombre
    'Sanchez',                                          // Primer apellido
    'Valdovinos',                                       // Segundo apellido
    '12345678-9',                                       // Rut
    '323123',                                           // Id de servicio
    'http://misitio.cl/voucher',                        // URL Final
    0,                                                  // Monto máximo
    '57508624',                                         // Teléfono fijo
    '57508624',                                         // Teléfono celular
    'Help - 8050014',                                   // Nombre PatPass
    'persona@test.cl',                                  // Email Persona
    'comercio@test.cl',                                 // Email Comercio
    'Merced 156, Santiago, Chile',                      // Dirección
    'Santiago'                                          // Ciudad
);
```

```csharp
var response = Inscription.Start(
    url: 'http://misitio.cl/finalizar_suscripcion',
    name: 'Diego',
    fLastname: 'Sanchez',
    sLastname: 'Valdovinos',
    rut: '12345678-9',
    serviceId: '323123',
    finalUrl: 'http://misitio.cl/voucher',
    maxAmount: 0,
    phoneNumber: '57508624',
    mobileNumber: '57508624',
    patpassName: 'Help - 8050014',
    personEmail: 'persona@test.cl',
    commerceEmail: 'comercio@test.cl',
    address: 'Merced 156, Santiago, Chile',
    city: 'Santiago'
);
```

```ruby
@resp  = Transbank::Patpass::PatpassComercio::Inscription::start(
    url: 'http://misitio.cl/finalizar_suscripcion',
    name: 'Diego',
    first_last_name: 'Sanchez',
    second_last_name: 'Valdovinos',
    rut: '12345678-9',
    service_id: '323123',
    final_url: 'http://misitio.cl/voucher',
    max_amount: 0,
    phone_number: '57508624',
    mobile_number: '57508624',
    patpass_name: 'Help - 8050014',
    person_email: 'persona@test.cl',
    commerce_email: 'comercio@test.cl',
    address: 'Merced 156, Santiago, Chile',
    city: 'Santiago'
)
```

```python
response = Inscription.start(
    url = 'http://misitio.cl/finalizar_suscripcion',
    name = 'Diego',
    first_last_name = 'Sanchez',
    second_last_name = 'Valdovinos',
    rut = '12345678-9',
    service_id = '323123',
    final_url = 'http://misitio.cl/voucher',
    max_amount = 0,
    phone_number = '57508624',
    mobile_number = '57508624',
    patpass_name = 'Help - 8050014',
    person_email = 'persona@test.cl',
    commerce_email = 'comercio@test.cl',
    address = 'Merced 156, Santiago, Chile',
    city = 'Santiago'
)
```

```http
POST restpatpass/v1/services/patInscription
Tbk-Api-Key-Id: 28299257
Tbk-Api-Key-Secret: cxxXQgGD9vrVe4M41FIt
Content-Type: application/json
{
    "url": "http://misitio.cl/finalizar_suscripcion",
    "nombre": "Diego",
    "pApellido": "Sanchez",
    "sApellido": "Valdovinos",
    "rut": "12345678-9",
    "serviceId": "323123",
    "finalUrl": "http://misitio.cl/voucher",
    "commerceCode": "28299257",
    "montoMaximo": "",
    "telefonoFijo": "57508624",
    "telefonoCelular": "57508624",
    "nombrePatPass": "Help - 8050014",
    "correoPersona": "persona@test.cl",
    "correoComercio": "comercio@test.cl",
    "direccion": "Merced 156, Santiago, Chile",
    "ciudad": "Santiago"
}
```

<strong>Parámetros Inscription.start</strong>

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
url <br> <i> String </i> | URL de retorno del comercio
firstName <br> <i> String </i> | Nombre del tarjetahabiente
fLastname <br> <i> String </i> | Apellido paterno del tarjetahabiente  
sLastname <br> <i> String </i> | Apellido materno del tarjetahabiente
rut <br> <i> String </i> | Rut del tarjetahabiente
serviceId <br> <i> String </i> | ID del servicio del tarjetahabiente  
finalUrl <br> <i> String </i> | Url final de la inscripción
maxAmount <br> <i> String </i> | Monto máximo del PAT a inscribir  
phoneNumber <br> <i> String </i> | Teléfono fijo del tarjetahabiente
mobileNumber <br> <i> String </i> | Teléfono celular del tarjetahabiente
patPassName <br> <i> String </i> | Nombre del PAT  
userEmail <br> <i> String </i> | Correo al tarjetahabiente con la suscripción
commerceEmail <br> <i> String </i> | Correo para el comercio con el comprobante de suscripción
userAddress <br> <i> String </i> | Dirección del tarjetahabiente
userCity <br> <i> String </i> | Ciudad del tarjetahabiente

<strong>Respuesta Inscription.start</strong>

```java
response.getToken();
response.getUrl();
```

```php
$response->token;
$response->url;
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
respone.url
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> |  Token de la transacción. Largo: 64.
url  <br> <i> xs:string </i> | URL de formulario de pago Patpass Comercio. Largo máximo: 256.

<strong>Mensajes de error</strong>

Código | Descripción
------   | -----------
-1   | Error interno
-101 | URL final incorrecta
-102 | URL retorno incorrecta
-103 | Nombre Incorrecto
-104 | Apellido Paterno incorrecto
-105 | Apellido Materno incorrecto
-106 | Rut inválido
-107 | Id de Servicio incorrecto
-108 | Código de comercio inválido
-109 | Monto tope inválido
-110 | Headers de autentificación requeridos
-111 | Headers de autentificación erróneos

La respuesta de este método se debe utilizar para crear un campo de nombre `tokenComercio` en un formulario, al cual se  le asigna el valor de `token` y debe ser enviado a `url`.

```html
<form action="<Insertar URL aquí>" method="post" name="tokenForm">
    <input type="hidden" name="tokenComercio" value="<Insertar token aquí>">
    <input type="submit" class="btn btn-success" value="Inscribirse en Patpass Comercio">
</form>
```

### Estado de una Inscripción

Para finalizar el proceso de inscripción se debe llamar a `Inscription.status`

<strong>Inscription.status</strong>

Este método permite finalizar el proceso de inscripción del PAT asociado al
token que se generó en la inscripción

La respuesta del método contiene el estado y la URL para desplegar el voucher.

```java
final PatpassComercioTransactionStatusResponse response =
    PatpassComercio.Transaction.status(token);
```

```php
$response = PatpassComercio\Inscription::getStatus($token);
```

```csharp
var response = Inscription.Status(token);
```

```ruby
@response = Transbank::Patpass::PatpassComercio::Inscription::status(token)
```

```python
response = Inscription.status(token)
```

```http
POST restpatpass/v1/services/status
Tbk-Api-Key-Id: 28299257
Tbk-Api-Key-Secret: cxxXQgGD9vrVe4M41FIt
Content-Type: application/json
{
    "token": "21383fe8ba4c4cdd9e18518daf4e9bcbaffd9e8e3ad4ec36f66ae2b4e80cc4b5"
}
```

<strong>Parámetros Inscription.status</strong>

Nombre  <br> <i> tipo </i> | Descripción
token <br> <i> String </i> | Token entregado al iniciar inscripción

<strong>Respuesta Inscription.status</strong>

```java
response.getVoucherUrl();
response.getAuthorized();
```

```php
$response->$status;
$response->$urlVoucher;
```

```csharp
response.Status;
response.UrlVoucher;
```

```ruby
response.status
response.voucher_url
```

```python
response.status
response.voucher_url
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorized  <br> <i> string </i> |  Boolean representa estado de la transacción.
voucherUrl  <br> <i> string </i> | URL para mostrar el voucher. Largo máximo: 256.

La respuesta de este método se debe utilizar para crear un campo de nombre `tokenComercio` en un formulario, al cual se  le asigna el valor de `token` y debe ser enviado a `voucherUrl`.

```html
<form action="<Insertar URL aquí>" method="post" name="tokenForm">
    <input type="hidden" name="tokenComercio" value="<Insertar token aquí>">
    <input type="submit" class="btn btn-success" value="Inscribirse en Patpass Comercio">
</form>
```
