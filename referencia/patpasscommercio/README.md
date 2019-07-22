# Patpass Comercio

## API y SDKs Backend

### Ambientes y Credenciales

#### Integracion / Test

-   `TEST` **(Integración)** se utiliza para el desarrollo y la validación de una integración. La URL Base es `[url patpass]`.

```java
PENDIENTE
```

```php
PatPassCommerce::setIntegrationType('TEST');
```

```csharp
PENDIENTE
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
PENDIENTE
```

```php
PatPassBase::setIntegrationType('LIVE');
```

```csharp
PENDIENTE
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
PENDIENTE
```

```php
PatPassBase::setApiKey("API Key entregado por transbank");
```

```csharp
PENDIENTE
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
