# Patpass Rest

## Patpass Trade

### Environments and credentials

The Patpass REST API is protected to ensure that only merchants authorized by Transbank make use of the available operations. Security is implemented through the following mechanisms:

* Secure channel through TLSv1.2 for customer communication with Webpay.
* Authentication and authorization by exchanging `Tbk-Api-Key-Id` and` Tbk-Api-Key-Secret` headers.

 <strong> Production Environment </strong>

Production endpoint URLs are hosted within
 <https://www.pagoautomaticocontarjetas.cl//>.

java
// Host: https://www.pagoautomaticocontardamientos.cl/
`` ''

`` `php
// Host: https://www.pagoautomaticocontardamientos.cl/
`` ''

csharp
// Host: https://www.pagoautomaticocontardamientos.cl/
`` ''

ruby
# Host: https://www.pagoautomaticocontardamientos.cl/
`` ''

python
# Host: https://www.pagoautomaticocontardamientos.cl/
`` ''

http
Host: https://www.pagoautomaticocontardamientos.cl/
`` ''

 <strong> Integration Environment </strong>

Integration endpoint URLs are hosted within
 <https://pagoautomaticocontarjetasint.transbank.cl//>.

See [documentation for test cards that work on
the integration environment] (/ documentation / how to start # environments).

java
// Host: https://pagoautomaticocontardamientosint.transbank.cl/
`` ''

`` `php
// Host: https://pagoautomaticocontardamientosint.transbank.cl/
`` ''

csharp
// Host: https://pagoautomaticocontardamientosint.transbank.cl/
`` ''

ruby
# Host: https://pagoautomaticocontardamientosint.transbank.cl/
`` ''

python
# Host: https://pagoautomaticocontardamientosint.transbank.cl/
`` ''

http
Host: https://pagoautomaticocontardamientosint.transbank.cl/
`` ''

### Trade Credentials

java
// Tbk-Api-Key-Id: 27082157
// Tbk-Api-Key-Secret: J7xYiUS7xqD7LkbWSUHI
`` ''

`` `php
// Tbk-Api-Key-Id: 27082157
// Tbk-Api-Key-Secret: J7xYiUS7xqD7LkbWSUHI
`` ''

csharp
// Tbk-Api-Key-Id: 27082157
// Tbk-Api-Key-Secret: J7xYiUS7xqD7LkbWSUHI
`` ''

ruby
# Tbk-Api-Key-Id: 27082157
# Tbk-Api-Key-Secret: J7xYiUS7xqD7LkbWSUHI
`` ''

python
# Tbk-Api-Key-Id: 27082157
# Tbk-Api-Key-Secret: J7xYiUS7xqD7LkbWSUHI
`` ''

http
Tbk-Api-Key-Id: 27082157
Tbk-Api-Key-Secret: J7xYiUS7xqD7LkbWSUHI
Content-Type: application / json
`` ''

### Start Registration

To start an enrollment you must call the `Inscription.start` method

 <strong> Inscription.start () </strong>

It allows to trigger the start of the registration process. The data entered will be preloaded in the form when it is displayed.

 <aside class="notice">
It is important to consider that once this method is invoked, the token that is delivered has a reduced life span of 5 minutes, after which the token is expired and cannot be used in a payment.
 </aside>

java
final PatpassComercioInscriptionStartResponse response = PatpassComercio.Inscription.start (
    'http://misitio.cl/finalizar_suscripcion', // URL
    'Diego', // Name
    'Sanchez', // First surname
    'Valdovinos', // Second surname
    '12345678-9', // Ruth
    '323123', // Service id
    'http://misitio.cl/voucher', // Final URL
    0, // Maximum amount
    '57508624', // Landline
    '57508624', // Cell phone
    'Help - 8050014', // Name PatPass
    'persona@test.cl', // Email Person
    'Comercio@test.cl', // Email Comercio
    'Merced 156, Santiago, Chile', // Address
    'Santiago' // City
);
`` ''

`` `php
$ response = PatpassTrade \ Inscription :: start (
    'http://misitio.cl/finalizar_suscripcion', // URL
    'Diego', // Name
    'Sanchez', // First surname
    'Valdovinos', // Second surname
    '12345678-9', // Ruth
    '323123', // Service id
    'http://misitio.cl/voucher', // Final URL
    0, // Maximum amount
    '57508624', // Landline
    '57508624', // Cell phone
    'Help - 8050014', // Name PatPass
    'persona@test.cl', // Email Person
    'Comercio@test.cl', // Email Comercio
    'Merced 156, Santiago, Chile', // Address
    'Santiago' // City
);
`` ''

csharp
var response = Inscription.Start (
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
    commerceEmail: 'Comercio@test.cl',
    address: 'Merced 156, Santiago, Chile',
    city: 'Santiago'
);
`` ''

ruby
@resp = Transbank :: Patpass :: PatpassTrade :: Inscription :: start (
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
    commerce_email: 'Comercio@test.cl',
    address: 'Merced 156, Santiago, Chile',
    city: 'Santiago'
)
`` ''

python
response = Inscription.start (
    url = 'http://misitio.cl/finalizar_suscripcion',
    name = 'Diego',
    first_last_name = 'Sanchez',
    second_last_name = 'Valdovinos',
    rut = '12345678-9',
    service_id = '323123',
    final_url = 'http://mysite.cl/voucher',
    max_amount = 0,
    phone_number = '57508624',
    mobile_number = '57508624',
    patpass_name = 'Help - 8050014',
    person_email = 'person@test.cl',
    commerce_email = 'Comercio@test.cl',
    address = 'Merced 156, Santiago, Chile',
    city = 'Santiago'
)
`` ''

http
POST restpatpass / v1 / services / patInscription
Tbk-Api-Key-Id: 27082157
Tbk-Api-Key-Secret: J7xYiUS7xqD7LkbWSUHI
Content-Type: application / json
{
    "url": "http://misitio.cl/finalizar_suscripcion",
    "name": "Diego",
    "pSurname": "Sanchez",
    "sSurname": "Valdovinos",
    "rut": "12345678-9",
    "serviceId": "323123",
    "finalUrl": "http://misitio.cl/voucher",
    "Maximum amount": "",
    "telefonoFijo": "57508624",
    "telefonoCelular": "57508624",
    "nombrePatPass": "Help - 8050014",
    "CorreoPersona": "persona@test.cl",
    "CorreoComercio": "Comercio@test.cl",
    "direction": "Merced 156, Santiago, Chile",
    "city": "Santiago"
}
`` ''

 <strong> Parameters Inscription.start </strong>

Name <br> <i> type </i> | Description
------   | -----------
url <br> <i> String </i> | Trade return URL
firstName <br> <i> String </i> | Cardholder name
fLastname <br> <i> String </i> | Cardholder's paternal last name
sLastname <br> <i> String </i> | Cardholder's maternal last name
rut <br> <i> String </i> | Rut of the cardholder
serviceId <br> <i> String </i> | Cardholder service ID
finalUrl <br> <i> String </i> | End of registration url
maxAmount <br> <i> String </i> | Maximum amount of the PAT to register
phoneNumber <br> <i> String </i> | Cardholder's landline
mobileNumber <br> <i> String </i> | Cardholder's cell phone
patPassName <br> <i> String </i> | PAT name
userEmail <br> <i> String </i> | Mail to cardholder with subscription
commerceEmail <br> <i> String </i> | Mail for commerce with proof of subscription
userAddress <br> <i> String </i> | Cardholder address
userCity <br> <i> String </i> | Cardholder City

 <strong> Response Inscription.start </strong>

java
response.getToken ();
response.getUrl ();
`` ''

`` `php
$ response-> token;
$ response-> url;
`` ''

csharp
response.Token;
response.Url;
`` ''

ruby
response.token
response.url
`` ''

python
response.token
respone.url
`` ''

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> xs: string </i> | Token of the transaction. Length: 64.
url <br> <i> xs: string </i> | Patpass Commerce payment form URL. Maximum length: 256.

 <strong> Error messages </strong>

Code | Description
------   | -----------
-1   | Internal error
-101 | Wrong final url
-102 | Wrong return url
-103 | Wrong name
-104 | Wrong Paternal Last Name
-105 | Incorrect Maternal Last Name
-106 | Invalid rut
-107 | Incorrect Service Id
-108 | Invalid commercial code
-109 | Invalid cap amount
-110 | Authentication headers required
-111 | Bad authentication headers

The response of this method must be used to create a field named `tokenComercio` in a form, which is assigned the value of` token` and must be sent to `url`.

html
 <form action="<Insertar URL aquí> "method =" post "name =" tokenForm ">
     <input type="hidden" name="tokenComercio" value="<Insertar token aquí> ">
     <input type="submit" class="btn btn-success" value="Inscribirse en Patpass Comercio">
 </form>
`` ''

### Status of an Enrollment

To finish the registration process you must call `Inscription.status`

 <strong> Inscription.status </strong>

This method allows finalizing the registration process of the PAT associated with the
token that was generated on enrollment

The method response contains the status and URL to deploy the voucher.

java
final PatpassComercioTransactionStatusResponse response =
    PatpassComercio.Transaction.status (token);
`` ''

`` `php
$ response = TradePass \ Inscription :: getStatus ($ token);
`` ''

csharp
var response = Inscription.Status (token);
`` ''

ruby
@response = Transbank :: Patpass :: PatpassTrade :: Inscription :: status (token)
`` ''

python
response = Inscription.status (token)
`` ''

http
POST restpatpass / v1 / services / status
Tbk-Api-Key-Id: 27082157
Tbk-Api-Key-Secret: J7xYiUS7xqD7LkbWSUHI
Content-Type: application / json
{
    "token": "21383fe8ba4c4cdd9e18518daf4e9bcbaffd9e8e3ad4ec36f66ae2b4e80cc4b5"
}
`` ''

 <strong> Inscription.status parameters </strong>

Name <br> <i> type </i> | Description
token <br> <i> String </i> | Token delivered when starting registration

 <strong> Response Inscription.status </strong>

java
response.getVoucherUrl ();
response.getAuthorized ();
`` ''

`` `php
$ response -> $ status;
$ response -> $ urlVoucher;
`` ''

csharp
response.Status;
response.UrlVoucher;
`` ''

ruby
response.status
response.voucher_url
`` ''

python
response.status
response.voucher_url
`` ''

Name <br> <i> type </i> | Description
------   | -----------
authorized <br> <i> string </i> | Boolean represents state of the transaction.
voucherUrl <br> <i> string </i> | URL to show the voucher. Maximum length: 256.

The response of this method must be used to create a field named `tokenComercio` in a form, which is assigned the value of` token` and must be sent to `voucherUrl`.

html
 <form action="<Insertar URL aquí> "method =" post "name =" tokenForm ">
     <input type="hidden" name="tokenComercio" value="<Insertar token aquí> ">
     <input type="submit" class="btn btn-success" value="Inscribirse en Patpass Comercio">
 </form>
`` ''
