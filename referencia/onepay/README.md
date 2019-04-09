# Onepay

## API y SDKs Backend

### Ambientes y Credenciales

```java
Onepay.setIntegrationType(Onepay.IntegrationType.LIVE);
```

```php
OnepayBase::setCurrentIntegrationType('LIVE');
```

```csharp
Onepay.IntegrationType = Transbank.Onepay.Enums.OnepayIntegrationType.LIVE;
```

```ruby
Transbank::Onepay::Base.integration_type = :LIVE
```

```python
onepay.integration_type = onepay.IntegrationType.LIVE
```

```http
Host: www.onepay.cl
```

- `LIVE` (Producción) corresponde al sistema productivo con transacciones
  reales. La URL Base es `https://www.onepay.cl`

```java
Onepay.setIntegrationType(Onepay.IntegrationType.TEST);
```

```php
OnepayBase::setCurrentIntegrationType('TEST');
```

```csharp
Onepay.IntegrationType = Transbank.Onepay.Enums.OnepayIntegrationType.TEST;
```

```ruby
Transbank::Onepay::Base.integration_type = :TEST
```

```python
onepay.integration_type = onepay.IntegrationType.TEST
```

```http
Host: onepay.ionix.cl
```


- `TEST` (Integración) se utiliza para el desarrollo y la validación de una
  integración. La URL Base es `https://onepay.ionix.cl`.


En cuanto a credenciales, todos los métodos del API requieren tres parámetros:

> Los SDKs manejan automáticamente el parámetro `appKey`.

- `appKey`: Identifica el lenguaje de programación usado.

Lenguaje | App Key TEST | App Key LIVE |
-------- | ------------ | -------------|
PHP | `1a0c0639-bd2f-4846-8d26-81f43187e797` | `2B571C49-C1B6-4AD1-9806-592AC68023B7`
Java | `fe9d371d-10ae-4138-8cfb-e2215b82c0d3` | `7968CDF8-F4CC-4BC5-8E27-D0513B88EB95`
Ruby | `760272c4-9950-4bf3-be16-a6729800231a` | `152C392E-F77C-426A-8667-55BEBB00EF1A`
Python | `8e279b4e-917d-4cbf-b0e3-9432adefff6a` | `66535F26-5918-435C-ACAB-F628F4CC65EF`
.NET | `297a620c-c776-4dd6-a42c-8669c6a4f2c5` | `81A33064-26DC-4267-8616-C97D252E7378`


```java
Onepay.setApiKey("api-key-entregado-por-transbank");
Onepay.setSharedSecret("secreto-entregado-por-transbank");
```

```php
OnepayBase::setApiKey("api-key-entregado-por-transbank");
OnepayBase::setSharedSecret("secreto-entregado-por-transbank");
```

```csharp
Onepay.ApiKey = "api-key-entregado-por-transbank";
Onepay.SharedSecret = "secreto-entregado-por-transbank";
```

```ruby
Transbank::Onepay::Base.api_key = "entregado por transbank"
Transbank::Onepay::Base.shared_secret = "entregado por transbank"
```

```python
onepay.api_key = "api-key-entregado-por-transbank"
onepay.shared_secret = "secreto-entregado-por-transbank"
```

- `apiKey`: Identificador del comercio, entregado por transbank.

> Los SDKs manejan automáticamente el cálculo del parámetro `signature`

- `signature`: Corresponde a la representación `base64` de un hash `HMAC-SHA256`
  usando como llave un _shared secret_ entregado por transbank al comercio. El
  texto a firmar con esa llave es la concatenación ciertos parámetros de negocio
  (documentados en cada _endpoint_) precedidos el largo de cada string.

  Por ejemplo, si el valor de los parámetros a concatenar fuera `"foobar"`, `"baz"`
  y `"quux"` entonces el string a firmar sería `"6foobar3baz4quux"`.

> Los SDKs por defecto se conectan al ambiente `TEST` usando estas credenciales de prueba.


<aside class="success">
  Para hacer tus primera pruebas en el ambiente <code>TEST</code> puedes utilizar
  <code>dKVhq1WGt_XapIYirTXNyUKoWTDFfxaEV63-O5jcsdw</code> como API Key y <code>?XW#WOLG##FBAGEAYSNQ5APD#JF@$AYZ</code> como el <i>shared secret</i> para crear el parámetro <code>signature</code>.
</aside>

### Simulador de Transacciones
Dentro del flujo de venta, puedes utilizar un simulador de transacciones donde podrás generar casos de éxito y fracaso, para utilizarlo sigue los siguientes pasos:

* Una vez presionado el botón para iniciar la compra, se mostrará la ventana de pago Onepay, tal como se ve en la imagen. Toma nota del número que aparece como "Código de compra", ya que lo necesitarás para emular el pago en el siguiente paso.
  ![Paso 1](/images/referencia/onepay/sim1.png)

* En otra ventana del navegador, ingresa al emulador de pagos desde [https://onepay.ionix.cl/mobile-payment-emulator/](https://onepay.ionix.cl/mobile-payment-emulator/), utiliza test@onepay.cl como correo electrónico, y el código de compra obtenido desde la pantalla anterior. Una vez ingresado los datos solicitados, presiona el botón "Iniciar Pago":
  ![Paso 2](/images/referencia/onepay/sim2.png)

* Si todo va bien, el emulador mostrará opciones para simular situaciones distintas. Para simular un pago exitoso, presiona el botón `PRE_AUTHORIZED`. En caso de querer simular un pago fallido, presiona le botón `REJECTED`. Simularemos un pago exitoso presionando el botón `PRE_AUTHORIZED`.
  ![Paso 3](/images/referencia/onepay/sim3.png)

* Vuelve a la ventana del navegador donde comenzaste, y podrás comprobar que el pago ha sido exitoso.
  ![Paso 4](/images/referencia/onepay/sim4.png)

* Alternativamente, en vez de simular un pago exitoso puedes simular un pago rechazado utilizando el botón `REJECTED` en el emulador. Si vuelves a la ventana del comercio, podrás ver que la operación ha sido cancelada.
  ![Paso 5](/images/referencia/onepay/sim5.png)

### Códigos de Error Comunes

Todos los endpoints pueden entregar como respuesta los siguientes `responseCode`:

- `"INVALID_PLUGIN_VERSION"`(la appKey no se encuentra registrada en la base de datos).
- `"INVALID_APP_KEY"` (la appKey no está activa).
- `"COMMERCE_NOT_FOUND"` (apiKey no corresponde a ningún comercio registrado).
- `"SIGN_VALIDATION_ERROR"` (falla la validación de la firma por motivos de encriptación, no por su contenido).
- `"INVALID_TRANSACTION_SIGN"` (falla la validación del contenido de la firma).

Además, cada endpoint puede tener condiciones adicionales de error que serán
representadas por otros valores de `responseCode`.

### Crear una Transacción

Crea una transacción a partir de un carro de compras.

#### `POST /ewallet-plugin-api-services/services/transactionservice/sendtransaction`

```java
Onepay.setAppScheme("mi-app://mi-app/onepay-result");
Onepay.setCallbackUrl("https://miapp.cl/endPayment");
Onepay.setQrWidthHeight(100); // Opcional
Onepay.setCommerceLogoUrl("http://some.url/logo"); // Opcional

ShoppingCart cart = new ShoppingCart();
cart.add(
    new Item()
        .setDescription("Producto de prueba")
        .setQuantity(1)
        .setAmount(9900)
        .setAdditionalData(null)
        .setExpire(-1));
TransactionCreateResponse response = Transaction.create(
  cart, Onepay.Channel.WEB);

// O, si lo deseas, puedes configurar opciones por transacción
TransactionCreateResponse response = Transaction.create(
  cart, Onepay.Channel.WEB,
  new Options().setQrWidthHeight(200)
               .setCommerceLogoUrl("http://commerce.cl/logo")
)
```

```php
OnepayBase::setAppScheme("mi-app://mi-app/onepay-result");
OnepayBase::setCallbackUrl("https://miapp.cl/endPayment");
OnepayBase::setQrWidthHeight(100); // Opcional
OnepayBase::setCommerceLogoUrl("http://some.url/logo"); // Opcional

$cart = new ShoppingCart();
$cart->add(
  new Item('Producto de prueba', 1, 9900, null, null));
$response = Transaction::create($cart, $channel, 'WEB');

// O, si lo deseas, puedes configurar opciones por transacción
$options = new options()
$options->setQrWidthHeight(200);
$options->setCommerceLogoUrl("http://commerce.cl/logo");
$response = Transaction::create($cart, $channel, 'WEB', $options);
```

```csharp
Onepay.AppScheme = "mi-app://mi-app/onepay-result";
Onepay.CallbackUrl = "https://miapp.cl/endPayment";
Onepay.QrWidthHeight = 100; // Opcional
Onepay.CommerceLogoUrl = "http://some.url/logo"; // Opcional

ShoppingCart cart = new ShoppingCart();
cart.Add(new Item(
    description: "Producto de prueba",
    quantity: 1,
    amount: 9900,
    additionalData: null,
    expire: -1));
var response = Transaction.Create(cart, ChannelType.Web);

// O, si lo deseas, puedes configurar opciones por transacción
var response = Transaction.Create(cart, ChannelType.Web, new Options(
  commerceLogoUrl: 'http://some.url/logo',
  qrWidthHeight: 200
));
```

```ruby
Transbank::Onepay::Base.app_scheme = "mi-app://mi-app/onepay-result"
Transbank::Onepay::Base.callback_url = "https://miapp.cl/endPayment"
Transbank::Onepay::Base.qr_width_height = 100 # Opcional
Transbank::Onepay::Base.commerce_logo_url = "http://some.url/logo" # Opcional


cart = Transbank::Onepay::ShoppingCart.new
cart.add(Transbank::Onepay::Item.new(amount: 9000,
                  quantity: 1,
                  description: "Producto de prueba",
                  additional_data: nil,
                  expire: -1))
channel = Transbank::Onepay::Channel::WEB
response = Transbank::Onepay::Transaction.create(shopping_cart: cart, channel: channel)

# O, si lo deseas, puedes configurar opciones por transacción
response = Transbank::Onepay::Transaction.create(
  shopping_cart: cart, channel: channel, options: {
    qr_width_height: 200, commerce_logo_url: 'http://commerce.cl/logo'
  }
)
```

```python
onepay.app_scheme = "mi-app://mi-app/onepay-result"
onepay.callback_url = "https://miapp.cl/endPayment"
onepay.qr_width_height = 100 # Opcional
onepay.commerce_logo_url = "http://some.url/logo" # Opcional

cart = ShoppingCart()
cart.add(Item(description="Producto de prueba", 
              quantity=1, amount=9900, 
              additional_data=None, expire=None))
result = Transaction.create(cart, Channel.WEB)

# O, si lo deseas, puedes configurar opciones por transacción
result = Transaction.create(cart, Channel.WEB, Options(commerce_logo_url="http://commerce.cl/logo", qr_width_height=200))
```

```http
POST /ewallet-plugin-api-services/services/transactionservice/sendtransaction
Content-Type: application/json

{
  "appKey": "04533c31-fe7e-43ed-bbc4-1c8ab1538afp",
  "apiKey": "Y12GCTN6PEB24N$C8S9QSSJT#1#GVXRF",
  "externalUniqueNumber": 1470696668,
  "signature": "YOKQ+vIZnkzbzmGxw8geEhzIQFv2RyVoUdJZF1qaWYg=",
  "total": "9900",
  "itemsQuantity": 1,
  "issuedAt": 1470696658,
  "items": [
    {
      "description": "Producto de prueba",
      "quantity": "1",
      "amount": "9900",
      "additionalData": null,
      "expire": null
    }
  ],
  "callbackUrl": "https://miapp.cl/endPayment",
  "channel": "WEB",
  "appScheme": "mi-app://mi-app/onepay-result",
  "generateOttQrCode": "false",
  "widthHeight": 200,
  "commerceLogoUrl":"https://www.ionix.cl/img/logo_ionix.png"
}
```


**Parámetros**


Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
------ | -----------
appKey<br><i>String</i> | Identificador del lenguaje de programación usado. Los SDKs lo manejan internamente.
apiKey <br> <i>  String  </i> | Identificador del comercio. Los SDK lo toman desde la configuración global de Onepay o desde el parámetro `options`.
signature <br> <i>  String  </i> | Firma creada a partir de los parámetros `externalUniqueNumber`, `total`, `itemsQuantity`, `issuedAt` y `callbackUrl` (en ese orden). Los parámetros deben ser convertidos a string y concatenados, prefijando cada parámetro con el número de caracteres que le corresponde. Luego se debe aplicar HMAC-SHA256 sobre ese string, usado el _shared secret_ entregado por Transbank como llave. Los SDKs manejan este parámetro automáticamente en función del _shared secret_ provisto en la configuración global o desde el parámetro `options`.
externalUniqueNumber <br> <i>  String  </i> | Identificador de la transacción en el sistema del comercio. Debe ser único para el comercio. Si la transacción falla, no se puede reutilizar en una nueva transacción (aunque sea un reintento). Los SDKs generan un identificador único automáticamente, a menos que se proveea explícitamente un tercer parámetro con el external unique number.
total <br> <i>  Number  </i> | Monto total de la compra. Los SDKs lo calculan automáticamente a partir del `cart` pasado como primer parámetro.
itemsQuantity <br> <i>  Integer  </i> | Cantidad de objetos comprados. Debe ser igual al largo del array `items`. Los SDKs lo calculan automáticamente a partir del `cart` pasado como primer parámetro.
issuedAt <br> <i>  Number  </i> | Fecha de creación de la transacción expresado en unix timestamp. Los SDKs lo calculan automáticamente.
channel <br> <i>  String  </i> | Canal por el que se está interactuando con el usuario final. Puede tomar el valor `"WEB"` (cuando la transacción se está realizando a través de un navegador en un PC de escritorio), `"MOBILE"` (para transacciones realizadas a través de por un navegador web móvil) o `"APP"` (cuando la transacción se lleva a cabo en una aplicación móvil). Los SDKs reciben el canal como un segundo parámetro, separado del carro de compra.
generateOttQrCode <br> <i>  Boolean  </i> | Indica si es necesario crear el código QR para la transacción actual. Su valor por defecto es `false`. Valor fijado siempre como `true` en los SDKs.
widthHeight <br> <i>  Number  </i> | Valor en pixeles para el tamaño que tendrá la imagen del código QR como ancho y alto. En este momento, solo los SDK de PHP y Java soportan estos parámetros.
commerceLogoUrl <br> <i>  String  </i> | Indica la dirección al logo del comercio. Se utiliza en la aplicación OnePay para descargar y mostrar dicha imagen durante el proceso de compra. En este momento, solo los SDK de PHP y Java soportan estos parámetros.
callbackUrl <br> <i>  String  </i> | Url de retorno al comercio. Se utiliza para devolverle el control al comercio cuando la transacción es de tipo `WEB` (modalidad checkout) o `MOBILE`. Los SDKs obtienen este valor desde la configuración global de Onepay.
appScheme <br> <i>  String  </i> | Esquema de retorno a la aplicación del comercio. Es obligatorio sólo cuando el channel es `APP`, en caso contrario puede ser `null`. Los SDKs obtienen este valor desde la configuración global de Onepay.
items <br> <i>  Array[Object]  </i> | Lista de items en el carro de compra. Corresponde al primer parámetro en los SDKs.
items[].description <br> <i>  String  </i> | Descripción del ítem.
items[].quantity <br> <i>  Number  </i> | Cantidad para el ítem descrito.
items[].amount <br> <i>  Number  </i> | Precio unitario. Puede ser negativo para representar un descuento, siempre que el monto total del carro (sumando todos los items) sea positivo.
items[].additionalData <br> <i>  String  </i> | 
items[].expire <br> <i>  Number  </i> |

**Respuesta**

```java
response.getOcc();
response.getOtt();
response.getExternalUniqueNumber();
response.getIssuedAt();
response.getQrCodeAsBase64();
```
```php
response->getOcc();
response->getOtt();
response->getExternalUniqueNumber();
response->getIssuedAt();
response->getQrCodeAsBase64();
```
```csharp
response.Occ;
resppnse.Ott;
response.ExternalUniqueNumber;
response.IssuedAt;
response.QrCodeAsBase64;
```
```ruby
response.occ
response.ott
response.external_unique_number
response.issued_at
response.qr_code_as_base64
```
```python
response.occ
response.ott
response.external_unique_number
response.issued_at
response.qr_code_as_base64
```

```http
200 OK
Content-Type: application/json

{
  "responseCode": "OK",
  "description": "OK",
  "result": {
    "occ": "1608630732695575",
    "ott": 12345678,
    "signature": "fnFy6sFPW0NHFXIagH9xyt2iQe3wf5PaXr8oSgI3/h0=",
    "externalUniqueNumber": "1470696668",
    "issuedAt": 1470696663,
    "qrCodeAsBase64": "jkdjiojijiuh768ygYTBUHGYGYYTRF788989878y876777fgfgfgfghretgyguyo8njhjhugyug6766668yuui"
  }
}

```

> Los SDKs sólo retornan los campos presentes dentro de `result`. Si el
> `responseCode` es diferente de `"OK"` lanzan una excepción incluyendo el
> `responseCode` y `description`.

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
-------|------------
responseCode<br> <i>  String  </i> |Código de respuesta que representa si la operación resultó exitosa o no. Puede tomar el valor `"OK"` (resultado exitoso),  `"INVALID_TRANSACTION"` (inconsistencia en el carro de compra en montos o cantidades), `"INVALID_PARAMS"` (otros parámetros incorrectos), `"TRANSACTION_ALREADY_EXISTS"` (External unique number ya había sido usado previamente), `"ILCAT_SERVICE_ERROR"` o `"ILCAT_RESPONSE_ERROR"` o `"ILCAT_ERROR"` (todos fallos internos). También puede tomar [uno de los valores comunes de error](#codigos-de-error-comunes).
description <br> <i>  String  </i> | Contiene una descripción del resultado. Se utiliza para informar por qué ocurrio un error.
result <br> <i>  Object  </i> | Tiene todos los datos de la transacción recién creada. Es `null` si el `responseCode` es diferente a `"OK"`.
result.occ <br> <i>  String  </i> | Identificador único de la transacción en Onepay.
result.ott <br> <i>  String  </i> | Identificador temporal de la transacción (8 dígitos)
result.signature <br> <i>  String  </i> | Firma creada a partir de los campos `response.occ`, `response.externalUniqueNumber` y `response.issuedAt` (en ese orden). Para validar la firma los parámetros deben ser convertidos a string y concatenados, prefijando cada parámetro con el número de caracteres que le corresponde. Luego se debe aplicar HMAC-SHA256 sobre ese string, usado el _shared secret_ entregado por Transbank como llave. Los SDKs validan este parámetro automáticamente en función del _shared secret_ provisto en la configuración global o desde el parámetro `options` y lanzan una excepción en caso que la firma no sea correcta.
result.externalUniqueNumber <br> <i>  String  </i> | Identificador de la transacción en el sistema del comercio.
result.issuedAt <br> <i>  Number  </i> | Fecha de creación de la transacción en unix timestamp.
result.qrCodeAsBase64 <br> <i>  String  </i> | Base64 que contiene el QR con la ott de la transacción.

**Callbacks**

Cuando la transacción sea aprobada o rechazada, se invocará vía método `GET` la
URL `callbackUrl` (en canal `WEB` modalidad checkout y en canal `MOBILE`) o la
URL `appScheme` (en canal `APP`) con los siguientes parámetros.


Parámetro de callback | Descripción |
----------------------|-------------|
occ | Identificador de la transacción Onepay.
externalUniqueNumber | Identificador de la transacción en el sistema del comercio.
status | Estado resultante de la transacción. Puede ser `"PRE_AUTHORIZED"` (transacción autorizada por el usuario), `"CANCELLED_BY_USER"`(el usuario abortó el pago) o `"REJECTED"` (transacción rechazada por el autorizador). También puede tomar el valor `"REVERSED"` (la transacción se reversó internamente después de no poder confirmar el pago) o `"REVERSE_NOT_COMPLETE"` (se intentó reversar, pero falló por alguna razón), pero estos casos deben ser manejados de igual forma que `"REJECTED"`.

Los parámetros son todos String (pues son parte de una URL), excepto en Android donde los parámetros de un intent son tipados. Ver la referencia del SDK móvil para más detalles.


##### Especificar tus propios external unique numbers


```java

TransactionCreateResponse response = Transaction.create(
  cart, Onepay.Channel.WEB, myOwnExternalUniqueNumber);
```

```php
$response = Transaction::create(
  $cart, $channel, 'WEB', myOwnExternalUniqueNumber);
```

```csharp
var response = Transaction.Create(
  cart, ChannelType.Web, myOwnExternalUniqueNumber);
```

```ruby
response = Transbank::Onepay::Transaction.create(shopping_cart: cart,
                                                 channel: channel,
                                                 external_unique_number: my_external_unique_number)
```

```python
response = Transaction.create(shopping_cart, Channel.WEB,
  my_own_external_unique_number)
```

Al usar los SDKs si no provees un external unique number, será generado automáticamente. Pero siempre puedes especificarlo manualmente.

### Confirmar una Transacción

A través del servicio `gettransactionnumber` se obtiene la información de una
transacción y al mismo tiempo se confirma por parte del comercio.


#### `POST /ewallet-plugin-api-services/services/transactionservice/gettransactionnumber`
```java
TransactionCommitResponse response =
  Transaction.commit(occ, externalUniqueNumber);
```

```php
$response = Transaction::commit($occ, $externalUniqueNumber);
```

```csharp
var response = Transaction.Commit(occ, externalUniqueNumber);
```

```ruby
response = Transbank::Onepay::Transaction.commit(occ: occ,
                                                 external_unique_number: external_unique_number)
```

```python
response = Transaction.commit(occ, external_unique_number)
```

```http
POST /ewallet-plugin-api-services/services/transactionservice/gettransactionnumber
Content-Type: application/json

{
  "appKey": "04533c31-fe7e-43ed-bbc4-1c8ab1538afp",
  "apiKey": "Y12GCTN6PEB24N$C8S9QSSJT#1#GVXRF",
  "occ": "1608630732695575",
  "externalUniqueNumber": "1470696668",
  "issuedAt": 1470696704,
  "signature": "Pomj0wYjR0WahF1Xwe1xmVGWd4VvU3n4oMzwSkn4/44="
}

```

**Parámetros**

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
------ | -----------
appKey <br> <i>  String  </i> | Identificador del lenguaje de programación usado. Los SDKs lo manejan internamente.
apiKey <br> <i>  String  </i> | Identificador del comercio. Los SDK lo toman desde la configuración global de Onepay o desde el parámetro `options`.
signature <br> <i>  String  </i> | Firma creada a partir de los parámetros `occ`, `externalUniqueNumber` e `issuedAt` (en ese orden). Los parámetros deben ser convertidos a string y concatenados, prefijando cada parámetro con el número de caracteres que le corresponde. Luego se debe aplicar HMAC-SHA256 sobre ese string, usado el _shared secret_ entregado por Transbank como llave. Los SDKs manejan este parámetro automáticamente en función del _shared secret_ provisto en la configuración global o desde el parámetro `options`.
occ <br> <i>  String  </i> | Identificador único de la transacción Onepay. Es el primer parámetro para esta funcionalidad en los SDK.
externalUniqueNumber <br> <i>  String  </i> | Identificador de la transacción en el sistema del comercio. Es el segundo parámetro para esta funcionalidad en los SDK.
issuedAt <br> <i>  Number  </i> | Fecha de creación de la petición en unix timestamp. Los SDKs lo manejan automáticamente.

**Respuesta**

```java
response.getOcc();
response.getAuthorizationCode();
response.getIssuedAt();
response.getAmount();
response.getTransactionDesc();
response.getInstallmentsAmount();
response.getInstallmentsNumber();
response.getBuyOrder();
```
```php
response->getOcc();
response->getAuthorizationCode();
response->getIssuedAt();
response->getAmount();
response->getTransactionDesc();
response->getInstallmentsAmount();
response->getInstallmentsNumber();
response->getBuyOrder();
```
```csharp
response.Occ;
response.AuthorizationCode;
response.IssuedAt;
response.Amount;
response.TransactionDesc;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.BuyOrder;
```

```ruby
response.occ
response.authorization_code
response.issued_at
response.amount
response.transaction_desc
response.installments_amount
response.installments_number
response.buy_order
```

```python
response.occ
response.authorization_code
response.issued_at
response.amount
response.transaction_desc
response.installments_amount
response.installments_number
response.buy_order
```

```http
200 OK
Content-Type: application/json

{
  "responseCode": "OK",
  "description": "OK",
  "result": {
    "occ": "1608630732695575",
    "authorizationCode": "768136",
    "issuedAt": 1470696663,
    "signature": "q1K/7JSIVcfq/PN5ruwTnfLRbUaYROX+3P3h4uCw1x4=",
    "amount": 9900,
    "transactionDesc": "Venta normal sin cuotas",
    "installmentsAmount": 9900,
    "installmentsNumber": 1,
    "buyOrder": "123456",
  }
}
```

> Los SDKs sólo retornan los campos presentes dentro de `result`. Si el
> `responseCode` es diferente de `"OK"` lanzan una excepción incluyendo el
> `responseCode` y `description`.


Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
------ | -----------
responseCode <br> <i>  String  </i> | Puede tomar el valor `"OK"` (resultado exitoso), `"INVALID_PARAMS"` (cuando los parámetros son incorrectos), `"INVALID_TRANSACTION"` o `"INVALID_TRANSACTION_STATUS"` (ambos indicando que la transacción no está autorizada y no se puede confirmar) También puede tomar [uno de los valores comunes de error](#codigos-de-error-comunes).
description <br> <i>  String  </i> | Contiene una descripción del resultado. Se utiliza para informar por qué ocurrio un error.
result <br> <i>  Object  </i> | Tiene todos los datos de la transacción recién creada. Es `null` si el `responseCode` es diferente a `"OK"`.
result.occ <br> <i>  String  </i> | Identificador único de la transacción Onepay.
result.authorizationCode <br> <i>  String  </i> | Código de autorización del pago.
result.issuedAt <br> <i>  Number  </i> | Fecha de creación de la transacción como unix timestamp.
result.signature <br> <i>  String  </i> | Firma creada a partir de los campos `response.occ`, `response.authorizationCode`, `response.issuedAt`, `response.amount`, `response.installmentsAmount`, `response.installmentsNumber` y `buyOrder.issuedAt` (en ese orden). Para validar la firma los parámetros deben ser convertidos a string y concatenados, prefijando cada parámetro con el número de caracteres que le corresponde. Luego se debe aplicar HMAC-SHA256 sobre ese string, usado el _shared secret_ entregado por Transbank como llave. Los SDKs validan este parámetro automáticamente en función del _shared secret_ provisto en la configuración global o desde el parámetro `options` y lanzan una excepción en caso que la firma no sea correcta.
result.amount <br> <i>  Number  </i> | Monto de la transacción.
result.transactionDesc <br> <i>  String  </i> | Descripción del tipo de venta.
result.installmentsAmount <br> <i>  Number  </i> | Monto de cada cuota.
result.installmentsNumber <br> <i>  Number  </i> | Cantidad de cuotas.
result.buyOrder <br> <i>  Number  </i> | Orden de compra Onepay.

### Anular una Transacción

A través del servicio nullifyTransaction se puede anular una transacción
previamente autorizada y confirmada.

#### `POST /ewallet-plugin-api-services/services/transactionservice/gettransactionnumber`

```java
RefundCreateResponse response =
  Refund.create(amount, occ, externalUniqueNumber,
  authorizationCode);
```
```php
$response = Refund::create(
  $amount, $occ, $externalUniqueNumber,$authorizationCode);
```

```csharp
var response = Refund.Create(
  amount, occ, externalUniqueNumber, authorizationCode);
```

```ruby
response = Transbank::Onepay::Refund.create(nullify_amount: amount,
                                            occ: occ,
                                            external_unique_number: external_unique_number,
                                            authorization_code: authorization_code)
```

```python
response = Refund.create(amount, occ, external_unique_number,
  authorization_code)
```

```http
POST /ewallet-plugin-api-services/services/transactionservice/gettransactionnumber
Content-Type: application/json

{
  "appKey": "04533c31-fe7e-43ed-bbc4-1c8ab1538afp",
  "apiKey": "Y12GCTN6PEB24N$C8S9QSSJT#1#GVXRF",
  "occ": "1608630732695575",
  "externalUniqueNumber": "1470696668",
  "authorizationCode": "768136",
  "nullifyAmount": 9900,
  "issuedAt": 1470696708,
  "signature": "NEKbDrujSy7D3fj4k/2JxEurQCVWSqQLWDm43n+C/uA="
}
```


Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
------ | -----------
appKey <br> <i>  String  </i> | Identificador del lenguaje de programación usado. Los SDKs lo manejan internamente.
apiKey <br> <i>  String  </i> | Identificador del comercio. Los SDK lo toman desde la configuración global de Onepay o desde el parámetro `options`.
signature <br> <i>  String  </i> | Firma creada a partir de los parámetros `occ`, `externalUniqueNumber`, `authorizationCode`, `issuedAt` y `nullifyAmount` (en ese orden). Los parámetros deben ser convertidos a string y concatenados, prefijando cada parámetro con el número de caracteres que le corresponde. Luego se debe aplicar HMAC-SHA256 sobre ese string, usado el _shared secret_ entregado por Transbank como llave. Los SDKs manejan este parámetro automáticamente en función del _shared secret_ provisto en la configuración global o desde el parámetro `options`.
occ <br> <i>  String  </i> | Identificador único de la transacción Onepay. Es el primer parámetro para esta funcionalidad en los SDK.
externalUniqueNumber <br> <i>  String  </i> | Identificador de la transacción en el sistema del comercio. Es el segundo parámetro para esta funcionalidad en los SDK.
authorizationCode <br> <i>  String  </i> | Código de autorización recibido al confirmar la transacción.
nullifyAmount <br> <i>  Number  </i> | Monto de la transacción a anular.
issuedAt <br> <i>  Number  </i> | Fecha de creación de la petición en unix timestamp. Los SDKs lo manejan automáticamente.

**Respuesta**

```java
response.getOcc();
response.getExternalUniqueNumber();
response.getReverseCode();
response.getIssuedAt();
```
```php
response->getOcc();
response->getExternalUniqueNumber();
response->getReverseCode();
response->getIssuedAt();
```
```csharp
response.Occ;
response.ExternalUniqueNumber;
response.ReverseCode;
response.IssuedAt;
```
```ruby
response.occ
response.external_unique_number
response.reverse_code
response.issued_at
```
```python
response.occ
response.external_unique_number
response.reverse_code
response.issued_at
```

```http
200 OK
Content-Type: application/json

{
  "responseCode": "OK",
  "description": "OK",
  "result": {
    "occ": "1608630732695575",
    "externalUniqueNumber": "1470696668",
    "reverseCode": "768136",
    "issuedAt": 1470696708,
    "signature": "Y09ESVxkx/xcAUt+FhVnvMMVvHE7ZTFEkZr1LDt0TAo="
  }
}
```

> Los SDKs sólo retornan los campos presentes dentro de `result`. Si el
> `responseCode` es diferente de `"OK"` lanzan una excepción incluyendo el
> `responseCode` y `description`.

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
------ | -----------
responseCode <br> <i>  String  </i> | Puede tomar el valor `"OK"` (resultado exitoso), `"INVALID_PARAMS"` (cuando los parámetros son incorrectos), `"INVALID_TRANSACTION"` o (la transacción no está autorizada y no se puede anular), `"INVALID_NULLIFY_AMOUNT`" (el monto no coincide con el de la transacción), `"NULLIFICATION_PERIOD_HAS_EXPIRED"` (ya no se puede anular la transacción), `"NONEXISTENT_TRANSACTION"` (la transacción no fue encontrada),  `"REVERSED_NULLIFY"` o `"FAILED_REVERSE_NULLIFY"` o  `"NONEXISTENT_USER"` o `"INVALID_USER"` o  `"NOT_ACTIVE_COMMERCE"` o `"ERROR"` (todos errores inesperados), También puede tomar [uno de los valores comunes de error](#codigos-de-error-comunes).
description <br> <i>  String  </i> | Contiene una descripción del resultado. Se utiliza para informar por qué ocurrio un error.
result <br> <i>  Object  </i> | Tiene todos los datos de la transacción recién creada. Es `null` si el `responseCode` es diferente a `"OK"`.
result.occ <br> <i>  String  </i> | Identificador único de la transacción Onepay.
result.reverseCode <br> <i>  String  </i> | Código de autorización de la anulación.
result.issuedAt <br> <i>  Number  </i> | Fecha de creación de la transacción como unix timestamp.
result.signature <br> <i>  String  </i> | Firma creada a partir de los campos `response.occ`, `response.externalUniqueNumber`, `response.reverseCode`, `response.issuedAt` (en ese orden). Para validar la firma los parámetros deben ser convertidos a string y concatenados, prefijando cada parámetro con el número de caracteres que le corresponde. Luego se debe aplicar HMAC-SHA256 sobre ese string, usado el _shared secret_ entregado por Transbank como llave. Los SDKs validan este parámetro automáticamente en función del _shared secret_ provisto en la configuración global o desde el parámetro `options` y lanzan una excepción en caso que la firma no sea correcta.
result.externalUniqueNumber <br> <i>  String  </i> | Identificador de la transacción en el sistema del comercio.

## SDK Frontend Javascript

### Modalidad Checkout

La modalidad checkout del SDK Javascript permite integrar fácilmente Onepay, simplemente invocando a una función javascript e implementando dos _endpoints_
en tu backend. Onepay checkout se encarga de levantar un "modal" que toma el control de toda la experiencia del usuario y la sincronización con la app Onepay.

#### `Onepay.checkout(options)`

Realiza una integración simple de Onepay a través de un modal que mantiene
informado al usuario en todo momento de los siguientes pasos a seguir para
completar su pago con Onepay. Si el usuario está en un dispositivo móvil esta
integración transparentemente usa la app nativa de Onepay en lugar del modal.

Es la integración recomendada, por su facilidad de integración y su
versatilidad.

**Parámetros**

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
-------|------------
options <br> <i>  Object  </i> | Parámetros para arrancar la integración checkout.
options.endpoint <br> <i>  String  </i> | URL del endpoint de creación de la transacción.
options.commerceLogo <br> <i>  String  </i> | (Opcional) URL del logo del comercio que se dibujará dentro del modal de Onepay checkout.
options.callbackUrl <br> <i>  String  </i> | URL del endpoint que será el callback cuando la transacción finalice.
options.transactionDescription <br> <i>  String  </i> | (Opcional) Texto que representa la descripción general de la compra, se dibujará en el modal sobre el valor del precio.


> Ejemplo:
> ```javascript
> Onepay.checkout({
>  endpoint: './transaction-create',
>  commerceLogo: 'https://tu-url.com/images/logo-01.png',
>  callbackUrl: './onepay-result',
>  transactionDescription: 'Set de pelotas'
> });
> ```

**Respuesta**

Ninguna.

**Parámetros endpoint**

El endpoint de creación de transacción será invocado via `POST` y recibirá los siguientes parámetros:

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
-------|------------
channel <br> <i>  String  </i> | Tendrá el valor `"WEB"` si el navegador donde se está ejecutando checkout es desktop. Si es un navegador móvil tomará el valor `"MOBILE"`.

**Respuesta endpoint**

Se espera que el endpoint responda a checkout con el siguiente resultado como una respuesta JSON:

> Ejemplo de respuesta:
> ```json
> {
> "externalUniqueNumber":"38bab443-c55b-4d4e-86fa-8b9f4a2d2d13",
> "amount":88000,
> "qrCodeAsBase64":"QRBASE64STRING",
> "issuedAt":1534216134,
> "occ":"1808534370011282",
> "ott":89435749
> }
> ```

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
-------|------------
externalUniqueNumber <br> <i>  String  </i> | Identificador único de la transacción en los sistemas del comercio.
amount <br> <i>  Number  </i> | Monto de la transacción.
qrCodeAsBase64 <br> <i>  String  </i> | Código QR tal como lo devuelve [el API de creación de transacción](#post-ewallet-plugin-api-services-services-transactionservice-sendtransaction).
issuedAt <br> <i>  Number  </i> | Unix timestamp de la creación de la transacción.
occ <br> <i>  String  </i> | Identificador único de la transacción en Onepay.
ott <br> <i>  Number  </i> | Identificador temporal de la transacción (8 dígitos).

**Parámetros callbackUrl**

El callback final será invocado via `GET` y recibirá los siguientes parámetros:

Parámetro de callback | Descripción |
----------------------|-------------|
occ | Identificador de la transacción Onepay.
externalUniqueNumber | Identificador de la transacción en el sistema del comercio.
status | Estado resultante de la transacción. Puede ser `"PRE_AUTHORIZED"` (transacción autorizada por el usuario), `"CANCELLED_BY_USER"`(el usuario abortó el pago) o `"REJECTED"` (transacción rechazada por el autorizador). También puede tomar el valor `"REVERSED"` (la transacción se reversó internamente después de no poder confirmar el pago) o `"REVERSE_NOT_COMPLETE"` (se intentó reversar, pero falló por alguna razón), pero estos casos deben ser manejados de igual forma que `"REJECTED"`.

Los parámetros son todos String (pues son parte de una URL).

**Respuesta callbackUrl**

El callback *NO* debe entregar JSON como resultado, pues tiene el control del
navegador del usuario. Por lo tanto el callback debe entregar una página web (sea de éxito o fracaso) con la información pertinente.

### Modalidad QR Directo

En la modalidad QR Directo tienes todas las herramientas a tu disposición para controlar tú mismo la experiencia del usuario, la sincronización con la app Onepay y la comunicación con tu backend. Por lo tanto, lo que se ofrecen son un set de funciones que te brindan dichas herramientas.

#### `Onepay.getChannel()`

Entrega el canal apropiado para iniciar una transacción Onepay en el backend. Es
tú responsabilidad llevar este valor hacia tu backend para crear la transacción
con el canal entregado.

**Parámetros**

Ninguno.

**Respuesta**

Un String con valor `"WEB"` (si el usuario está en un navegador desktop) o
`"MOBILE"` (si se encuentra en un navegador de smartphone).

#### `Onepay.isMobile()`

Indica si el usuario está usando un smartphone para navegar la página actual.
Una vez que hayas creado la transacción (o en el momento que prefieras) te
permite saber si el usuario está en un smartphone (y por tanto no tiene sentido dibujar un QR)

**Parámetros**

Ninguno.

**Respuesta**

Un Boolean con valor `true` si el usuario está usando un smartphone y `false`
en caso contrario.

#### `Onepay.redirectToApp(occ)`

> Ejemplo:
> ```javascript
> if (Onepay.isMobile()) {
>  Onepay.redirectToApp(transaction.occ);
>  return;
> }
> ```

Lleva al usuario a la app nativa de Onepay par autorizar una transacción. La
interfaz que le indique al usuario que puede abrir dicha app (y/o lo invite a
instalar Onepay) es tu responsabilidad.

**Parámetros**

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
-------|------------
occ <br> <i>  String  </i> | Identificador de la transacción en Onepay


**Respuesta**

Ninguna. Al final del flujo de aprobación del pago recibirás el callback que
hayas configurado al invocar el API de creación de transacción (o que hayas
configurado en tu SDK de backend).

#### `Onepay.directQr(transaction, htmlTagId)`

Dibuja el código QR y te permite escuchar los eventos de Onepay para sincronizarte con lo que ocurra en la app del usuario.

**Parámetros**

> Ejemplo:
>```javascript
>var transaction = {
>    occ:1808696602719171,
>    ott:60361166,
>    externalUniqueNumber:"cf734d22-550c-449b-aa68-a57d64831b41",
>    qrCodeAsBase64:"iVBORw0KGgoAAAANSUhEUgAAAMgAAADICAYAAACtWK6eAAADkklEQVR42u3dQW7CQB>BE0bn/peEEWUSy6aqe96XsEDJ4HouO" +
>     "7TkfSX92fAUSIBIgEiASIBIgEiASIBIgEiCSAJEAkQCRAJEAkQCRAJEAkQCRAJEEiASIBIgEiHQLkHNO>xd9/j7/l9bedL0AAAQQQQAABBBBAAA" +
>     "EEEEAAAQSQZ7/wsdHdQ8fTsjDSADatH0AAAQQQQAABBBBAAAEEEEAAAQSQ8/rCazmeKchbzxcggAACCC>CAAAIIIIAAAggggAACCCC/PJ609wEE" +
>     "EEAAAQQQQAABBBBAAAEEEEAAAaQRyFPfw9axMCCAAAIIIIAAAggggAACCCCAAALIbiDtx9MCwfkCBBDn>CxBAAAEEEEAAAQQQQAABBJA3xoC2P5" +
>     "h9ve0PAAEEEEAseEAAAQQQQAABBBBAANkN5LbaN/EUIIAIEEAECCCAAAIIIIAAAsidQG7b1LJlvJm2mW>niDwIggAACCCCAAAIIIIAAAggggAAC" +
>     "SN6D19LGvGlAbhwvAwIIIIAAAggggAACCCCAAAIIIIDMAWm5pbRlQbYfPyCAAAIIIIAAAggggAACCCCA>AALIb+Ckvf/UwmgZd7tYERBAAAEEEE" +
>     "AAAQQQQAABBBBAANkBZOs496kF0PK9bRgLAwIIIIAAAggggAACCCCAAAIIIIDMwdk69mx5EFwTHEAAAQ>QQQAABBBBAAAEEEEAAAQSQ/m0Ftm7W" +
>     "mTam9n8QQAABBBBAAAEEEEAAAQQQQAABxPYHyT8ILXDaPxcggAACCCCAAAIIIIAAAggggAACSObCa7kF>Ne34Wz4vIIAAAggggAACCCCAAAIIII" +
>     "AAAsidtW8rkLbNBCCAAAIIIIAAIkAAESCAAAIIIJlA0rYhaLm4rn2MPPV5AQEEEEAAAQQQQAABBBBAAA>EEEEB2j0lbFnbamHTzrbiAAAIIIIAA" +
>     "AggggAACCCCAAAIIID23uN4GLW2cPhkggAACCCCAAAIIIIAAAggggAACSP>+D2raOr9sfTAcIIIAAAggggAACCCCAAAIIIIAAAkjCiUsbL7dcVA" +
>     "kIIIAAAggggAACCCCAAAIIIIAAMntCt44xty7IposYAQEEEEAAAQQQQAABBBBAAAEEEED2LqSWzTTfPo>9TY3xAAAEEEEAAAQQQQAABBBBAAAEE" +
>     "EOmuAJEAkQCRAJEAkQCRAJEAkQCRAJEEiASIBIgEiASIBIgEiASIBIgEiCRAJEAkQCRApIS>+jaaPZv11nXQAAAAASUVORK5CYII\u003d",
>
>    paymentStatusHandler: {
>        ottAssigned: function () {
>            // callback transacción asignada
>            console.log("Transacción asignada.");
>
>        },
>        authorized: function (occ, externalUniqueNumber) {
>            // callback transacción autorizada
>            console.log("occ: " + occ);
>            console.log("externalUniqueNumber: " +
>              externalUniqueNumber);
>
>            // funcion no incluida en sdk
>            sendRedirect("./transaction-commit",
>              occ, externalUniqueNumber);
>        },
>        canceled: function () {
>            // callback rejected by user
>            console.log("Transacción cancelada por el usuario.");
>
>        },
>        authorizationError: function () {
>            // cacllback authorization error
>            console.log("Error de autorizacion.");
>
>        },
>        unknown: function () {
>            // callback to any unknown status recived
>            console.log("Estado desconocido.");
>
>        }
>    }
>};
>var htmlTagId = 'qr-image';
>Onepay.directQr(transaction, htmlTagId);
>```
>

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
-------|------------
htmlTagId <br> <i>  String  </i> | Id del elemento HTML donde se dibujará el código QR.
transaction <br> <i>  Object  </i> | Información de la transacción y de los callbacks para los eventos Onepay.
transaction.occ <br> <i>  Number  </i> | Identificador de la transacción ya creada en Onepay.
transaction.ott <br> <i>  Number  </i> | Identificador temporal de 8 dígitos de la transacción ya creada en Onepay.
transaction.externalUniqueNumber <br> <i>  String  </i> | Identificador de la transacción en los sistemas del comercio.
transaction.qrCodeAsBase64 <br> <i>  String  </i> | Código QR tal como lo devuelve [el API de creación de transacción](#post-ewallet-plugin-api-services-services-transactionservice-sendtransaction).
transaction.paymentStatusHandler <br> <i>  Object  </i> | Funciones de escucha para los evetos de Onepay

**Callbacks**

Los siguientes elementos dentro de `transaction.paymentStatusHandler` serán llamados cuando ocurran los eventos correspondientes:

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
------ | ------------
ottAssigned <br> <i>  function()  </i> | Invocado cuando la app de Onepay ha sido enlazada con la transacción (se ha escaneado el código QR).
authorized <br> <i>  function(occ, externalUniqueNumber)  </i> | Invocado cuando la transacción ha tenido éxito y ha sido autorizada. Incluye la occ y el externalUniqueNumber de la transacción como parámetros. Una vez que este evento es gatillado dispones de sólo 30 segundos para poder confirmar la transacción, de lo contrario esta se reversa en forma automática de la tarjeta del cliente. Por esta razón, en este callback debes invocar a tu backend para confirmar rápidamente la transacción.
canceled <br> <i>  function()  </i> | Invocado cuando el usuario ha abortado la transacción.
authorizationError <br> <i>  function()  </i> | Invocado cuando el autorizador de la transacción ha indicado un error.
unknown <br> <i>  function()  </i> | Invocado cuando ocurre un evento no reconocido.

## SDK para Apps Móviles

Los SDK para apps móviles permiten integrar directo en tu app. Sin navegadores
webs ni webviews, tus usuarios tendrán la mejor experiencia de pago app-to-app.

Todas las funciones de los SDK Móviles son métodos de una instancia `onepay`.

> En iOS:
> ```swift
> let onepay = OnePay()
> ```


> En Android:
> ```
> OnePay onepay = new OnePay(this);
> ```

### Detectar e instalar la app Onepay

En Android debes pasar un contexto como parámetro al constructor de `OnePay` (típicamente el activity actual desde el que usas Onepay)

> En iOS:
> ```swift
> if (onepay.isOnePayInstalled()) {
>      // Todo OK, sigue adelante
> } else {
>     // Ofrece al usuario si quiere instalar Onepay.
>     // En caso que esté de acuerdo, usa lo siguiente para iniciar la instalación:
>     onepay.installOnePay()
> }
> ```

> En Android:
> ```
> if (onepay.isOnePayInstalled()) {
>   // Todo OK, sigue adelante
> } else {
>   // Ofrece al usuario si quiere instalar Onepay.
>   // En caso que esté de acuerdo, usa lo siguiente para
>   // iniciar la instalación:
>   onepay.installOnePay();
> }
> ```
#### `isOnePayInstalled()`

Este método te permite saber si la app Onepay ya se encuentra instalada en el
dispositivo del usaurio.

**Parámetros**

Ninguno.

**Respuesta**.

Retorna `true` si la app Onepay está instalada, `false` en caso contrario.

#### `installOnePay()`

Si has detectado que Onepay no está presente puedes invitar al usuario a probar
Onepay y luego con este método gatillas la instalación de Onepay en el
dispositivo del usuario.

**Parámetros**

Ninguno.

**Respuesta**.

Ninguna.

### Iniciar una transacción app-to-app

El uso principal del SDK móvil es invocar a la app de Onepay para completar una transacción ya iniciada en tu backend.

#### `initPayment(occ, callback)`

> En iOS:
> ```swift
> onepay.initPayment(occ,
>     callback: {(statusCode, description) in
>         switch statusCode {
>         case OnePayState.occInvalid:
>             // Algo anda mal con el occ que obtuviste
>             // desde el backend.
>             // Debes reintentar obtener el occ o abortar
>             os_log("OCC inválida")
>         case OnePayState.notInstalled:
>             // Onepay no está instalado.
>             // Debes abortar o pedir al usuario instalar
>             // Onepay (y luego reintentar initPayment)
>             os_log("Onepay no instalado")
>         }
>     }
> )
> ```

> En Android:
> ```
> onepay.initPayment(occ, new OnePayCallback() {
>     @Override
>     public void failure(Error error, String s) {
>         switch (error) {
>             case INVALID_OCC:
>                 // Algo anda mal con el occ que obtuviste
>                 // desde el backend.
>                 // Debes reintentar obtener el occ o abortar
>                 break;
>             case ONE_PAY_NOT_INSTALLED:
>                 // Onepay no está instalado.
>                 // Debes abortar o pedir al usuario
>                 // instalar Onepay (y luego reintentar
>                 // initPayment)
>                 break;
>             default:
>                 Log.e(TAG, "Error inesperado al iniciar pago Onepay " +
>                     error.toString> () + ":" + s);
>                 // Aborta o reintenta
>         }
>     }
> });
> ```


Si Onepay está instalado y creaste en tu backend una transacción para el canal `APP`, sólo necesitas la `occ` para iniciar el pago:

**Parámetros**

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
------ | -----------
occ <br> <i>  String  </i> | Identificador único de la transacción en Onepay.
callback <br> <i>  OnePayCallback  </i> | Callback que se invocará en caso de error.

**Respuesta**

Ninguna. En caso de error será invocado el callback y en caso de que el control
pase exitosamente a Onepay retomarás el control mediante el `appScheme` que
hayas enviado en el API cuando creaste la transacción (o el que hayas
configurado en tu SDK).

**callback**

El callback de ser invocado recibirá dos parámetros:

Nombre <br> <i>tipo</i>| Descripción<br>&nbsp;
------ | -----------
status[iOS] o error[Android] <br> </i>OnePayState[iOS] o Error[Android]</i>  | Indica que situación de error ha ocurrido. Las opciones posibles son que hayas indicado una occ inváida (`OnePayState.occInvalid`[iOS] o `INVALID_OCC`[Android]) o que Onepay no está instalado (`OnePayState.notInstalled`[iOS] o `ONE_PAY_NOT_INSTALLED`[Android])
description <br> <i>  String  </i> | Información adicional sobre el error.

### Retomar el control en tu app iOS

En iOS la URL que hayas indicado en `appSchema` (al crear la transacción con el
API i en la configuración de tu SDK backend) será invocada por Onepay para
retomar el control. Debes manejar esta url en tu AppDelegate. Los parámetros que
te permitirán saber el resultado de la transacción los recibirás en la _query
string_ de esta URL:

Parámetro de callback | Descripción |
----------------------|-------------|
occ | Identificador de la transacción Onepay.
externalUniqueNumber | Identificador de la transacción en el sistema del comercio.
status | Estado resultante de la transacción. Puede ser `"PRE_AUTHORIZED"` (transacción autorizada por el usuario), `"CANCELLED_BY_USER"`(el usuario abortó el pago) o `"REJECTED"` (transacción rechazada por el autorizador). También puede tomar el valor `"REVERSED"` (la transacción se reversó internamente después de no poder confirmar el pago) o `"REVERSE_NOT_COMPLETE"` (se intentó reversar, pero falló por alguna razón), pero estos casos deben ser manejados de igual forma que `"REJECTED"`.
issuedAt | Fecha de creación de la transacción como timestamp unix.
Los parámetros son todos String (pues son parte de una URL),

Si quieres saber más sobre cómo manejar la invocación a tu  `appScheme`, [consulta la gúia de Onepay en la sección de integración para app móvil](/documentacion/onepay#integracion-en-app-movil-comercio).

Finalmente deberás enviar a tu backend la información que recibiste para que [confirme la transacción usando el API o uno de nuestros SDK backend](#confirmar-una-transaccion).

### Retomar el control en tu app Android

En Android se invocará tu `appSchema` mediante un `Intent` que debes configurar
para que esté asociado a la misma URL de tu `appSchema` e invoque a un
`Activity` de tu app.

Cuando Onepay realice la invocación podrás leer los parámetros via
`getIntent().getStringExtra()` (para Strings, el método cambia para otros tipos
de datos, por ejemplo `getIntent().getLongExtra()` para campos de tipo `Long`
como `issuedAt`).

Parámetro de callback <br> <i>tipo</i>| Descripción<br>&nbsp;
----------------------| -------|
occ <br> <i>  String  </i> | Identificador de la transacción Onepay.
externalUniqueNumber <br> <i>  String  </i> | Identificador de la transacción en el sistema del comercio.
status <br> <i>  String  </i> | Estado resultante de la transacción. Puede ser `"PRE_AUTHORIZED"` (transacción autorizada por el usuario), `"CANCELLED_BY_USER"`(el usuario abortó el pago) o `"REJECTED"` (transacción rechazada por el autorizador). También puede tomar el valor `"REVERSED"` (la transacción se reversó internamente después de no poder confirmar el pago) o `"REVERSE_NOT_COMPLETE"` (se intentó reversar, pero falló por alguna razón), pero estos casos deben ser manejados de igual forma que `"REJECTED"`.
issuedAt <br> <i>  Long  </i> | Fecha de creación de la transacción como timestamp unix.

Si quieres saber más sobre cómo configurar tu `Activity` para que reciba la invocación de Onepay, [consulta la gúia de Onepay en la sección de integración para app móvil](/documentacion/onepay#integracion-en-app-movil-comercio).

Finalmente deberás enviar a tu backend la información que recibiste para que [confirme la transacción usando el API o uno de nuestros SDK backend](#confirmar-una-transaccion).

### Dibujar QR para implementar modalidad Cortafilas en Android

Para la [modalidad Cortafilas](/documentacion/onepay#integracion-cortafila), hemos dispuesto en el SDK Android un componente que dibujará el QR a partir de la ott.

Para integrar este componente, debes agregar en el archivo build.gradle del módulo que contiene tu proyecto Android (generalmente en app/build.gradle).

> En el archivo build.gradle:
> ```
> dependencies {
> 	implementation 'com.google.zxing:core:3.3.0'
> 	implementation('com.journeyapps:zxing-android-embedded:3.6.0') { transitive = false }
> }
> ```

También debes agregar el componente en tu archivo layout.

> En el archivo xml del layout:
> ```xml
> <cl.ionix.tbk_ewallet_sdk_android.ui.QROnepayView
>                 android:id="@+id/qr_imageView"
>                 android:layout_width="150dp"
>                 android:layout_height="150dp" />
> ```

Y en la clase Java del Activity o Fragment en donde estás agregando el componente, debes obtener el elemento del layout para setear el ott.

> En la clase encargada del layout:
> ```
> QROnepayView imageViewQrCode = inflatedView.findViewById(R.id.qr_imageView);
> imageViewQrCode.setOtt(ott);
> ```

### Dibujar QR para implementar modalidad Cortafilas en iOS

Para integrar este componente, puedes hacerlo por una de las dos formas:

* Programáticamente. Dentro la inicialización de tu UIViewController, debes inicializar la clase QROnepayView y agregarlo a tu vista.

> Para incorporarlo programáticamente:
> ```swift
> let qrOnepayView = QROnepayView(ott: "12345678")
> qrOnepayView.frame = CGRect(x: 0, y: 0, width: 200, height: 200)
> self.view.addSubview(qrOnepayView)
> ```

* Usando Storyboards. En el UIViewController, agrega un View para indicar como Custom Class la clase `QROnepayView`. En el código de la implementación del UIViewController, sobre la variable que posee el Outlet de la clase, debes setear el ott.

> Al usar Storyboard:
> ```swift
> qrOnepayView.setOtt("12345678")
> ```
