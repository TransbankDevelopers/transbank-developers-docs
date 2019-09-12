# Patpass Comercio

## API y SDKs Backend

### Ambientes y Credenciales

#### Integracion / Test

-   `TEST` **(Integración)** se utiliza para el desarrollo y la validación de una integración. La URL Base es `[url patpass]`.

```java
PatpassComercio.setIntegrationType(IntegrationType.TEST);
```

```php
PatPassCommerce::setIntegrationType('TEST');
```

```csharp
Inscription.IntegrationType.set(PatpassComercioIntegrationType.Test);
```

```ruby
PENDIENTE
```

```python
PENDIENTE
```

```http
PENDIENTE
```

#### Producción / Live

-   `LIVE` **(Producción)** corresponde al sistema productivo con transacciones reales. La URL Base es `[url patpass]`

```java
PatpassComercio.setIntegrationType(IntegrationType.LIVE);
```

```php
PatPassBase::setIntegrationType('LIVE');
```

```csharp
Inscription.IntegrationType.set(PatpassComercioIntegrationType.Live);
```

```ruby
PENDIENTE
```

```python
PENDIENTE
```

```http
PENDIENTE
```

## Set API key

En cuanto a credenciales, todos los métodos del API requieren tres parámetros:

> Los SDKs manejan automáticamente el parámetro `appKey`.

| Lenguaje | Api-Key Test | Api-Key Live |
| :------- | :----------- | :----------- |
| java     | `codigo`     | `codigo`     |
| PHP      | `codigo`     | `codigo`     |
| csharp   | `codigo`     | `codigo`     |
| ruby     | `codigo`     | `codigo`     |
| python   | `codigo`     | `codigo`     |

```java
PatpassComercio.setApiKey("API Key entregado por transbank");
```

```php
PatPassBase::setApiKey("API Key entregado por transbank");
```

```csharp
Inscription.ApiKey.set("API Key entregado por transbank");
```

```ruby

```

```python
PENDIENTE
```

```http
PENDIENTE
```

-   `apiKey`: Identificador del comercio, entregado por Transbank.

> Los SDKs por defecto se conectan al ambiente `TEST` usando estas credenciales de prueba.

## API Rest

### Inscripción de método http: POST

**Endpoint**: `restpatpass/v1/services/patInscription`

Este endpoint permite el inicio de la transacción generando un token, a partir de los datos recibidos. Estos datos serán precargados en el formulario al momento de mostrarse. por medio de este token

#### Descripción Headers

| Parámetros   | Descripción                            |  Tipo  |
| :----------- | :------------------------------------- | :----: |
| commerceCode | codigo de comercio asociado al api-key | String |
| apiKey       | api-key                                | String |

#### Descripción Request Body

| Parámetros    | Descripción                                               |  Tipo  |
| :------------ | :-------------------------------------------------------- | :----: |
| url           | Url de retorno del comercio                               | String |
| firstName     | Nombre del tarjetahabiente                                | String |
| fLastname     | Apellido paterno del tarjetahabiente                      | String |
| sLastname     | Apellido materno del tarjetahabiente                      | String |
| rut           | Rut del tarjetahabiente                                   | String |
| serviceId     | ID del servicio del tarjetahabiente                       | String |
| finalUrl      | Url final de la inscripción                               | String |
| commerceCode  | Código del comercio 8 dígitos                             | String |
| maxAmount     | Monto máximo del PAT a inscribir                          | String |
| phoneNumber   | Teléfono fijo del tarjetahabiente                         | String |
| mobileNumber  | Teléfono celular del tarjetahabiente                      | String |
| patPassName   | Nombre del PAT                                            | String |
| userEmail     | Correo al tarjetahabiente con la suscripción              | String |
| commerceEmail | Correo para el comercio con el comprobante de suscripción | String |
| userAddress   | Dirección del tarjetahabiente                             | String |
| userCity      | Ciudad del tarjetahabiente                                | String |

#### Ejemplo Request body

```json
{
  "url":"https://www.comercio.com/urlretorno",
  "firstName":"nombre",
  "fLastname":"apellido",
  "sLastname":"sapellido",
  "rut":"14959787-6",
  "serviceId":"76",
  "finalUrl":"https://www.comercio.com/urlrfinal",
  "commerceCode":"28546637",
  "maxAmount":1500,
  "phoneNumber":"012356545",
  "mobileNumber":"99999999",
  "patPassName":"nombre del patpass",
  "userEmail":"persona@persona.cl",
  "commerceEmail":"comercio@comercio.cl",
  "userAddress":"huerfanos 101",
  "userCity":"Santiago"
}
```

#### Mensaje Response OK

```json
{
  "token": "21383fe8ba4c4cdd9e18518daf4e9bcbaffd9e8e3ad4ec36f66ae2b4e80cc4b5",
  "url": "www.urlFormulario.cl"
}
```

#### Mensaje Response bad request

```json
{
  "codigo":-101,
  "mensaje":"URLfinalincorrecta"   
}
```

#### Mensajes de Errores

| code | message                             |
| :--- | :---------------------------------- |
| -101 | URL final incorrecta                |
| -102 | URL retorno incorrecta              |
| -103 | Nombre Incorrecto                   |
| -104 | Apellido Paterno incorrecto         |
| -105 | Apellido Materno incorrecto         |
| -106 | Rut inválido                        |
| -107 | Id de Servicio incorrecto           |
| -108 | Código de commercio inválido        |
| -109 | Monto tope inválido                 |
| -110 | Headers autentificacion requeridos  |
| -111 | Headers de autentificacion erroneos |
| -1   | Error interno                       |

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el token es caducado y no podrá ser utilizado en un pago.
</aside>

### Codigo Inscripcion ejemplo

```java
try {
    // call the SDK
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
    logger.info(String.format("response : %s", response));

    if (null != response) {
        // add response to model in order to send it to the view
        addModel("response", response);


        // add necesary data to make form works
        addModel("tbk_token", response.getToken());
        addModel("url_webpay", response.getUrl());
    }
}  catch (InscriptionStartException | IOException e) {
    e.printStackTrace();
}
``` 
```php
$response = PatpassComercio\Inscription::start(
                    
            $req['url'],
            $req['nombre'],
            $req['pApellido'],
            $req['sApellido'],
            $req['rut'],
            $req['serviceId'] ,
            $req['finalUrl'],
            $req['commerceCode'],
            $req['montoMaximo'],
            $req['telefonoFijo'],
            $req['telefonoCelular'],
            $req['nombrePatPass'],
            $req['correoPersona'],
            $req['correoComercio'],
            $req['direccion'],
            $req['ciudad']
);
               
```

```csharp
var response = Inscription.Start(
                url: url,
                name: name,
                fLastname: f_lastname,
                sLastname: s_lastname,
                rut: rut,
                serviceId: service_id,
                finalUrl: final_url,
                commerceCode: commerce_code,
                maxAmount: max_amount,
                phoneNumber: phone_number,
                mobileNumber: mobile_number,
                patpassName: patpass_name,
                personEmail: client_email,
                commerceEmail: commerce_email,
                address: address,
                city: city);
```

```ruby
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
render 'inscription_started'
```

```python
# Este SDK aún no se encuentra disponible
```

**Respuesta**

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

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> xs:string </i> | Token de la transacción. Largo: 64.
url  <br> <i> xs:string </i> | URL de formulario de pago Patpass Comercio. Largo máximo: 256.

**Formulario Patpass Comercio**

El token y url recibidos como respuesta del proceso de inscripcion deben ser usados para continuar el proceso de pago

**Ejemplo**

```java
<form action="${model.url}" method="post" name="tokenForm">
    <input type="hidden" name="tokenComercio" value="${model.tbk_token}">
    <input type="submit" class="btn btn-success" value="Start Patpass comercio">
</form>
```


### getStatus método http: POST

**Endpoint**: `restpatpass/v1/services/status`

Por medio de este endpoint se finaliza la transacción, dando de alta el pat asociado al token. Como respuesta se entrega el estado final de la inscripción, y la url para desplegar el voucher. Con esta url y el token asociado.  

#### Descripción Headers

| Parámetros   | Descripción                            |  Tipo  |
| :----------- | :------------------------------------- | :----: |
| commerceCode | codigo de comercio asociado al api-key | String |
| apiKey       | api-key                                | String |


#### Descripción Request Body

| Parámetros    | Descripción                                               |  Tipo  |
| :------------ | :-------------------------------------------------------- | :----: |
| url           | Url de retorno del comercio                               | String |

#### Ejemplo de request body

```json

{
	"token": "21383fe8ba4c4cdd9e18518daf4e9bcbaffd9e8e3ad4ec36f66ae2b4e80cc4b5"
}
```

#### Mensaje Response

```json
{
    "status": true,
    "urlVoucher": "www.urlvoucher.cl"
}
```

### Codigo Inscripcion ejemplo

```java
try {
    final PatpassComercioTransactionStatusResponse response = PatpassComercio.Transaction.status(token);
    addModel("response", response);
    addModel("token", token);
} catch (Exception e) {
    logger.error(e.getLocalizedMessage(), e);
    return new ErrorController().error();
}
``` 
```php
try {
    $response = PatpassComercio\Inscription::getStatus($req['token']);
}catch(Exception $e){
    echo 'Exception caught: ',  $e->getMessage(), "\n";
}
               
```

```csharp
try{
    var token = Request.Form["token_ws"];
    var response = Inscription.Status(token);
}catch(InscriptionStatusException e){
    Console.WriteLine("{0} Exception caught.", e);
}
```

```ruby
@response = Transbank::Patpass::PatpassComercio::Inscription::status(token: @token)
render 'inscription_status'
```

```python
# Este SDK aún no se encuentra disponible
```

**Respuesta**

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
@response.voucher_url
@response.voucher_url
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorized  <br> <i> xs:string </i> | Boolean representa estado de la transaccion.
voucherUrl  <br> <i> xs:string </i> | URL para mostrar el voucher. Largo máximo: 256.

**Voucher Patpass Comercio**

El token recibido al inicio de la transaccion se debe usar para mostrar el voucher en conjunto con la "voucherUrl" que retorna el metodo status

**Ejemplo para mostrar voucher**

```java
<form action="${model.UrlVoucher}" method="post" name="tokenForm">
    <input type="hidden" name="tokenComercio" value="${model.tbk_token}">
    <input type="submit" class="btn btn-success" value="Start Patpass comercio">
</form>
```
