# Onepay

<div class="pos-title-nav">
  <div tbk-link='/referencia/onepay' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugin/onepay' tbk-link-name='Plugins'></div>
</div>

## Conceptos y SDKs

Onepay puede ser usado en diversos canales y en múltiples modalidades de pago.
Por ende es importante entender esos canales y modalidades a la hora de realizar
una integración.

En cuanto a modalidades, Onepay puede usarse para ventas por internet y también
para ventas presenciales. Veamos primero los conceptos necesarios para entender
el pago por internet, lo que te permitirá también entender la forma de ocuparlo
para modalidades presenciales ([como el caso de un
cortafila](#integracion-en-modalidad-quotcortafilaquot)).

**En las ventas por internet** el flujo de Onepay varía ligeramente según el
*canal* mediante el cual se enlaza la transacción con la app Onepay en el
teléfono del usuario. Los canales son:

- `WEB`: Web desktop. En este caso aparece un QR en la pantalla del comercio que
le permite al usuario escanearlo con la app Onepay y confirmar la transacción.
El comercio es notificado cuando el usuario ha completado la transacción.

- `MOBILE`: Web móvil. Aquí el navegador móvil abre la app Onepay, donde
el usuario confirma directamente la transacción. Luego la app Onepay abre *una
nueva pestaña del navegador del sistema* para notificar al comercio que se ha
completado la transacción.

- `APP`: Integración app-to-app. Aquí el comercio cuenta con una app móvil que
abre la app Onepay cuando el usuario desea pagar usando este medio de pago. Una
vez se completa la transacción, la app Onepay invoca nuevamente la app del
comercio.

Para los dos primeros canales (`WEB` y `MOBILE`) se ofrece un [SDK Javascript](https://github.com/TransbankDevelopers/transbank-sdk-js-onepay#integraci%C3%B3n-checkout)
con dos modalidades de integración:

- **Checkout**: Integración recomendada que maneja toda la comunicación con Onepay
e informa al usuario de los pasos a seguir a través de un diálogo "modal". En
esta modalidad el comercio indica dos urls que conectan el frontend del comercio
con el backend. La primera url es la encargada de crear la transacción. La
segunda url es la que toma el control después de que el usuario completa el
flujo en la app. **Checkout se encarga de hacer invisibles las diferencias entre
el canal `WEB` y `MOBILE`.**

- **QR Directo**: La versión "hágalo usted mismo" para usuarios avanzados.
Provee más flexibilidad pero es más compleja y requiere más trabajo la
integración. Se le entregan al comercio las herramientas para dibujar el
código QR y para "escuchar" los eventos que van ocurriendo en la app del
usuario. También se incluye una forma de abrir la app de Onepay en caso de
canal `MOBILE`, pero toda la lógica debe ser manejada por el comercio.

Para el canal `APP` se ofrecen SDKs [iOS](https://github.com/TransbankDevelopers/transbank-sdk-ios-onepay)
y [Android](https://github.com/TransbankDevelopers/transbank-sdk-android-onepay)

Finalmente, para las labores del backend, se ofrecen SDKs para [Java](https://github.com/TransbankDevelopers/transbank-sdk-java), [.NET](https://github.com/TransbankDevelopers/transbank-sdk-dotnet), [PHP](https://github.com/TransbankDevelopers/transbank-sdk-php), [Ruby](https://github.com/TransbankDevelopers/transbank-sdk-ruby) y [Python](https://github.com/TransbankDevelopers/transbank-sdk-python).
Estos SDKs se utilizan en todos los casos, independiente del
canal y de la modalidad de integración web.

**En la venta presencial** se utilizan las mismas herramientas pero de diferente
forma, pues ahora el dispositivo que genera la transacción (ej: totem de auto
atención o un tablet operado como cortafila) no es propiedad del
tarjetahabiente. En este caso se usa el canal `MOBILE`, pues será una pestaña
del navegador móvil en el teléfono del cliente donde se notificará al comercio
el resultado de la transacción.

A continuación veremos el paso a paso para algunas integraciones tanto para
venta por internet como presencial.

## Integración Web (Checkout)

### 1. Front-end: Configuración

Para realizar la integración checkout debes utilizar [nuestro SDK Javascript](https://github.com/TransbankDevelopers/transbank-sdk-js-onepay) siguiendo las
[instrucciones de instalación](https://github.com/TransbankDevelopers/transbank-sdk-js-onepay#instalaci%C3%B3n).

Luego en tu frontend debes invocar a `Onepay.checkout` de la siguiente forma:

```javascript
Onepay.checkout({
  endpoint: './transaction-create',
  commerceLogo: 'https://tu-url.com/images/icons/logo-01.png',
  callbackUrl: './onepay-result',
  transactionDescription: 'Set de pelotas',
  onclose: function (status) {
      console.log('el estado recibido es: ', status);
  }
});
```

Esto abrirá un modal que se encargará de toda la interacción con el usuario.

A continuación una descripción de los parámetros recibidos por esta función:

- `endpoint` : corresponde a la URL de tu backend que tiene la lógica de crear
la transacción usando alguno de nuestros SDK disponibles o invocando
directamente al API de Onepay.

El SDK enviara el parámetro `channel` a tu endpoint, cuyo valor podría ser
`"WEB"` o `"MOBILE"`. Debes asegurarte de capturar este parámetro para poder
enviar el channel adecuado al API de Onepay.

Se espera que el `endpoint` retorne un JSON como el del siguiente ejemplo:

```json
{
 "externalUniqueNumber":"38bab443-c55b-4d4e-86fa-8b9f4a2d2d13",
 "amount":88000,
 "qrCodeAsBase64":"QRBASE64STRING",
 "issuedAt":1534216134,
 "occ":"1808534370011282",
 "ott":89435749
}
```

En el paso 2 más abajo podrás ver más información sobre este `endpoint`.

- `commerceLogo`: (Opcional) Corresponde a la URL full del logo de comercio que se mostrará
  en el modal. Como el modal reside en un dominio externo, no puede ser una URL
  relativa (a diferencia de los otros parámetros). El logo se redimensionará a
  125 pixeles de ancho y la altura se calcula automáticamente para mantener las
  proporciones de la imagen.

- `callbackUrl` : URL que se invocará desde el SDK una vez que la transacción
ha sido autorizada por el comercio. En este callback el comercio debe hacer la
confirmación de la transacción, para lo cual dispone de 30 segundos desde que
la transacción se autorizó, de lo contrario esta sera automáticamente reversada.

En el paso 3 más abajo podrás ver más sobre cómo se invoca este _callback_.

- `transactionDescription` : (Opcional) Texto que representa la descripción general de la compra, se dibujará en el modal sobre el valor del precio.

- `onclose` : (Opcional) Función de callback que será invocada cuando el usuario
cierre el modal ya sea porque se arrepintió de realizar el pago o porque hubo un error
en este último y el usuario presionó el botón "Entendido". Esto puede ser útil
para el flujo de tu comercio en caso que necesites de alguna forma poder saber y
tomar control de la acción del usuario al cerrar el modal. Los valores que puede
tener el parámetro `status` son `CANCELED` en caso que usuario cierre al presionar
el link `No pagar, volver a la pagina del comercio`, o `ERROR` en caso que el
usuario presione el botón `Entendido` si un error ocurre en el proceso de pago.

<aside class="notice">
Tip: En desarrollo puedes comenzar tus URLs de callback con // en lugar de
http:// o https:// para evitar problemas al mezclar páginas seguras con
inseguras. Pero en producción siempre debieras usar https.
</aside>

### 2. Back-end: Crear una Transacción

Como puedes imaginar, para completar la integración necesitarás programar el
código que maneja las llamadas en las URLs indicadas por `endpoint` y
`callbackUrl`.

Primero que nada, deberás configurar también el `callbackUrl` en el backend.
La razón detrás de esto es que si el canal es `MOBILE` no es el componente
javascript quien invocará el _callback_, sino que la propia app Onepay que
reside en el teléfono del usuario. La configuración del callback en el backend
le permite a la app Onepay saber donde redireccionar en ese caso:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.onepay.Onepay;


// URL de retorno para canal MOBILE (web móvil). También será usada en canal WEB
// si integras la modalidad checkout del SDK javascript.
Onepay.setCallbackUrl("https://www.misitioweb.com/onepay-result");
```

```php
use Transbank\Onepay\OnepayBase;

OnepayBase::setCallbackUrl('https://www.misitioweb.com/onepay-result');
```

```csharp
using Transbank.Onepay;

Onepay.CallbackUrl = "https://www.misitioweb.com/onepay-result";
```

```ruby
require 'transbank/sdk'

Transbank::Onepay::Base.callback_url = "https://miapp.cl/endPayment"
```

```python
from transbank import onepay
onepay.callback_url = "https://www.misitioweb.com/onepay-result"
```

Con eso estás preparado para crear una transacción en el código backend que será invocado por la url del `endpoint`.

Para esto se debe crear en primera instancia un objeto `ShoppingCart` que se debe llenar un `Item` (o varios):

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.onepay.model.*;

//...

ShoppingCart cart = new ShoppingCart();
cart.add(
    new Item()
        .setDescription("Zapatos")
        .setQuantity(1)
        .setAmount(15000)
        .setAdditionalData(null)
        .setExpire(-1));
```

```php
use Transbank\Onepay\ShoppingCart;
use Transbank\Onepay\Item;
use Transbank\Onepay\Transaction;

$cart = new ShoppingCart();
$cart->add(new Item('Zapatos', 1, 15000));
```

```csharp
using Transbank.Onepay:
using Transbank.Onepay.Model:

//...

ShoppingCart cart = new ShoppingCart();
cart.Add(new Item(
    description: "Zapatos",
    quantity: 1,
    amount: 15000,
    additionalData: null,
    expire: -1));
```

```ruby
require 'transbank/sdk'

cart = Transbank::Onepay::ShoppingCart.new
cart.add(Transbank::Onepay::Item.new(amount: 15000,
                  quantity: 1,
                  description: "Zapatos",
                  additional_data: nil,
                  expire: -1))
```

```python
from transbank.onepay.cart import ShoppingCart, Item

cart = ShoppingCart()
cart.add(Item(description="Zapatos",
              quantity=1, amount=15000,
              additional_data=None, expire=None))
```

Luego que el carro de compras contiene todos los ítems, se crea la transacción:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.onepay.model.*;

// ...

Onepay.Channel channel = Onepay.Channel.valueOf(request.getParameter("channel"));
TransactionCreateResponse response = Transaction.create(cart, channel);
```

```php
/*
 * IMPORTANTE: El ejemplo está pensado en la forma más simple de integrar que
 * es apuntando a integración. El SDK asume que usarás integración si no has
 * seteado el ambiente, razón por la cual no es necesario que lo configures.
 * Sin embargo, debes tener presente que al momento de apuntar a producción
 * deberás configurar el ambiente así como tu API_KEY y SHARED_SECRET antes
 * de realizar la llamada a Transaction::create, ya que de lo contrario estarás
 * apuntando a INTEGRACION
 *
 * ejemplo:
 * use Transbank\Onepay\OnepayBase;
 * OnepayBase::setCurrentIntegrationType('LIVE');
 * OnepayBase::setApiKey('api-key-entregado-por-transbank');
 * OnepayBase::setSharedSecret('secreto-entregado-por-transbank');
 */

$channel = $request->input("channel");
$response = Transaction::create($cart, $channel);

```

```csharp
using Transbank.Onepay;
using Transbank.Onepay.Model:

// ...
ChannelType channelType = ChannelType.Parse(channel);
var response = Transaction.Create(cart, channelType);
```

```ruby
require 'transbank/sdk'

channel = params["channel"]
response = Transbank::Onepay::Transaction.create(shopping_cart: cart,
                                                 channel: channel)
```

```python
from transbank.onepay.transaction import Transaction, Channel

channel = Channel(request.form.get("channel"))
response = Transaction.create(cart, channel)
```

El segundo parámetro en el ejemplo corresponde al `channel` y debes capturar el
valor que el frontend envió cuando invoco al endpoint que está creando la transacción para
fijar el valor correcto ("WEB" si se trata de web desktop, "MOBILE" si se trata
de web móvil).

El resultado entregado contiene la confirmación de la creación de la transacción, en la forma de un objeto `TransactionCreateResponse`.

```json
"occ": "1807983490979289",
"ott": 64181789,
"signature": "USrtuoyAU3l5qeG3Gm2fnxKRs++jQaf1wc8lwA6EZ2o=",
"externalUniqueNumber": "f506a955-800c-4185-8818-4ef9fca97aae",
"issuedAt": 1532103896,
"qrCodeAsBase64": "QRBASE64STRING"
```

El `externalUniqueNumber` corresponde a un UUID generado por SDK que identifica
la transacción en el lado del comercio. Opcionalmente, puedes [especificar tus
propios external unique numbers](/referencia/onepay#especificar-tus-propios-external-unique-numbers).

En el caso que no se pueda completar la transacción o `responseCode` en la
respuesta del API sea distinto de `ok` se lanzará una excepción.

Con eso ya tienes la información suficiente para responder el JSON que espera la
modalidad checkout de parte de la url configurada como `endpoint`. Sólo debes
eliminar `signature` y agregar `amount` desde el `ShoppingCart` que construiste
anteriormente. Con eso retornas algo como esto:

```json
{
"occ": "1807983490979289",
"ott": 64181789,
"externalUniqueNumber": "f506a955-800c-4185-8818-4ef9fca97aae",
"issuedAt": 1532103896,
"qrCodeAsBase64": "QRBASE64STRING",
"amount":88000
}
```

Y en este punto el SDK frontend toma el control de la coordinación.  Si el canal
es `WEB` se le mostrará un "modal" al usuario donde podrá escanear el código QR
y también recibirá las instrucciones de cada paso a realizar para autorizar el
pago. Si el canal es `MOBILE` el SDK javascript automáticamente abrirá la app
Onepay en el mismo teléfono del usuario.

Existe un [Simulador de transacciones](/referencia/onepay#simulador-de-transacciones) que puedes utilizar para generar transacciones exitosas o fallidas.

### 3. Back-end: Confirmar la Transacción

Eventualmente la transacción llegará a término y el control retornará al backend
mediante el método `GET` en la url indicada en el `callbackUrl` en el paso 1
(frontend) y 2 (backend). Es muy importante que pongas la misma URL en ambos
pasos. De esta forma sólo tienes que escribir un único código que maneje la
confirmación de la transacción independiente si el canal es `WEB` o `MOBILE`.

Para el canal `WEB` (desktop) en la integración checkout el resultado de éxito o
fracaso le aparecerá comunicado al usuario dentro del diálogo modal, para luego
redirigir al usuario al _callback_ especificado en el paso 1. Para el canal
`MOBILE` la app de Onepay abrirá una **nueva pestaña** en el navegador móvil del
usuario y en esa pestaña se invocará la url de _callback_ especificado en el
paso 2.

En este callback el comercio debe hacer la confirmación de la transacción, para
lo cual dispone de 30 segundos desde que la transacción se autorizó. De lo
contrario esta sera automáticamente reversada.

El callback será invocado vía `GET` e irán los parámetros `occ` y
`externalUniqueNumber` con los cuales podrás invocar la confirmación de la
transacción desde tu backend. Adicionalmente se envía el parámetro `status`,
que puede tomar los siguientes valores:

- `PRE_AUTHORIZED`: El estado de éxito. Debes confirmar la transacción para
  completar el flujo.
- `CANCELLED_BY_USER`: El tarjetahabiente decidió abortar la autorización.
- `REJECTED`: La transacción fue rechazada.
- `REVERSED`: No se pudo confirmar el pago.
- `REVERSE_NOT_COMPLETED`: Se intentó reversar (ver estado anterior) pero falló
  por alguna razón interna.

Si se recibe cualquier otro valor, el comercio debe asumir que ocurrió un error.

Finalmente, se debe confirmar la transacción:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.onepay.model.*;

String status = request.getParameter("status");
String externalUniqueNumber = request.getParameter("externalUniqueNumber");
String occ = request.getParameter("occ");

if (null != status && status.equalsIgnoreCase("PRE_AUTHORIZED")) {
    try {
        TransactionCommitResponse response =
            Transaction.commit(occ, externalUniqueNumber);
        // Procesar response
    } catch (TransbankException e) {
        // Error al confirmar la transaccion
    }
} else {
    // Mostrar página de error
}
```

```php
/*
 * IMPORTANTE: El ejemplo está pensado en la forma más simple de integrar que
 * es apuntando a integración. El SDK asume que usarás integración si no has
 * seteado el ambiente, razón por la cual no es necesario que lo configures.
 * Sin embargo, debes tener presente que al momento de apuntar a producción
 * deberás configurar el ambiente así como tu API_KEY y SHARED_SECRET antes
 * de realizar la llamada a Transaction::commit, ya que de lo contrario estarás
 * apuntando a INTEGRACION
 *
 * ejemplo:
 * use Transbank\Onepay\OnepayBase;
 * OnepayBase::setCurrentIntegrationType('LIVE');
 * OnepayBase::setApiKey("api-key-entregado-por-transbank");
 * OnepayBase::setSharedSecret("secreto-entregado-por-transbank");
 */

use Transbank\Onepay\Transaction;

$status = $request->input("status");
$externalUniqueNumber = $request->input("externalUniqueNumber");
$occ = $request->input("occ");

if (!empty($status) && strcasecmp($status,"PRE_AUTHORIZED") == 0) {
    try {
        $response = Transaction::commit($occ, $externalUniqueNumber);
        // Procesar $response
    } catch (Exception $e) {
        // Error al confirmar la transaccion
    }
} else {
    // Mostrar página de error
}
```

```csharp
using Transbank.Onepay;

 if (null != status && status.Equals("PRE_AUTHORIZED", StringComparison.InvariantCultureIgnoreCase))
 {
    try
    {
        var response = Transaction.Commit(occ, externalUniqueNumber);
        // Procesar response
    }
    catch (TransbankException e)
    {
        // Error al confirmar la transaccion
    }
}
else
{
    // Mostrar página de error
}
```

```ruby
require 'transbank/sdk'

if params["status"] == "PRE_AUTHORIZED"
  response = Transbank::Onepay::Transaction.commit(
    occ: params["occ"],
    external_unique_number: params["external_unique_number"]
  )
  # Procesar response

else
  # Mostrar página de error
end

rescue Transbank::Onepay::Errors::TransactionCommitError => e
  # Manejar el error de confirmación de transacción

```

```python
if (status and status.upper() == "PRE_AUTHORIZED"):
  try:
    response = Transaction.commit(occ, external_unique_number)
    # Procesar response
  except TransactionCommitError:
    # Error al confirmar la transacción
else:
  # Mostrar página de error
```

El resultado obtenido en `response` tiene una forma como la de este ejemplo:

```json
"occ": "1807983490979289",
"authorizationCode": "623245",
"issuedAt": 1532104549,
"signature": "FfY4Ab89rC8rEf0qnpGcd0L/0mcm8SpzcWhJJMbUBK0=",
"amount": 27500,
"transactionDesc": "Venta Normal: Sin cuotas",
"installmentsAmount": 27500,
"installmentsNumber": 1,
"buyOrder": "20180720122456123"
```

Para completar el flujo, solo queda que le des la noticia al usuario de que el
pago ha sido exitoso y le muestres un comprobante de ser necesario.

<aside class="notice">
Como el _callback_ final está implementado usando el método
HTTP GET, debe ser idempotente: Debe funcionar correctamente aunque sea
ejecutado repetidas veces.
</aside>

## Integración Mobile (app-to-app)

Si tu comercio posee una app móvil, entonces debes integrar también los SDKs
móviles. Comienza revisando la manera de [instalar el SDK
Android](https://github.com/TransbankDevelopers/transbank-sdk-android-onepay#instalaci%C3%B3n)
y [el SDK
iOS](https://github.com/TransbankDevelopers/transbank-sdk-ios-onepay#instalaci%C3%B3n).

A diferencia de la modalidad Checkout, en la integración con tu app tú debes
encargarte directamente de la comunicación entre tu app y tu backend.

<aside class="warning">
Importante: Las transacciones deben ser siempre creadas y confirmadas por
el backend, pues es en tus servidores donde puedes validar que se estén
generando transacciones por los montos correctos (y no seas víctima de un
ataque malicioso).
</aside>

Pero como ya tienes creado el código que maneja la transacción, lo que te falta
por hacer eso sólo conectar las partes.

Primero tienes en el backend que configurar el equivalente al `callbackUrl` pero
para apps. Se trata del `appScheme`, que le dice a la app de Onepay cómo
entregarle de vuelta el control a tu propia aplicación:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.onepay.Onepay;

Onepay.setAppScheme("mi-app://mi-app/onepay-result");
```

```php
use Transbank\Onepay\OnepayBase;

OnepayBase::setAppScheme("mi-app://mi-app/onepay-result");
```

```csharp
using Transbank.Onepay:

Onepay.AppScheme = "mi-app://mi-app/onepay-result";
```

```ruby
require 'transbank/sdk'

Transbank::Onepay::Base.app_scheme = "mi-app://mi-app/onepay-result"
```

```python
from transbank import onepay

onepay.app_scheme = "mi-app://mi-app/onepay-result"
```

Luego puedes invocar al mismo endpoint que construiste para Checkout, pero ahora
lo llamas desde tu app pasando `APP` en el parámetro `channel` (via POST).

Con eso obtendrás en tu app los datos de la nueva transacción. Usando el `occ`
puedes usar nuestro SDK para invocar la app de Onepay:

```swift
import OnePaySDK
import os.log
//...

let onepay = OnePay();
onepay.initPayment("aqui-va-la-occ",
    callback: {(statusCode, description) in
        switch statusCode {
        case OnePayState.occInvalid:
            // Algo anda mal con el occ que obtuviste desde el backend
            // Debes reintentar obtener el occ o abortar
            os_log("OCC inválida")
        case OnePayState.notInstalled:
            // Onepay no está instalado.
            // Debes abortar o pedir al usuario instalar Onepay (y luego reintentar initPayment)
            os_log("Onepay no instalado")
        }
    }
)
```

En Android:

```java
import cl.ionix.tbk_ewallet_sdk_android.Error;
import cl.ionix.tbk_ewallet_sdk_android.OnePay;
import cl.ionix.tbk_ewallet_sdk_android.callback.OnePayCallback;
//...

OnePay onepay = new OnePay(this);
onepay.initPayment("occ", new OnePayCallback() {
    @Override
    public void failure(Error error, String s) {
        switch (error) {
            case INVALID_OCC:
                // Algo anda mal con el occ que obtuviste desde el backend
                // Debes reintentar obtener el occ o abortar
                break;
            case ONE_PAY_NOT_INSTALLED:
                // Onepay no está instalado.
                // Debes abortar o pedir al usuario instalar Onepay (y luego reintentar initPayment)
                break;
            default:
                Log.e(TAG, "Error inesperado al iniciar pago Onepay " + error.toString() + ":" + s);
                // Aborta o reintenta
        }
    }
});
```

Si todo funciona OK, el control pasará a la app Onepay donde el usuario podrá autorizar la transacción.

Una vez el usuario realice el pago, el control volverá a tu app usando el
`appScheme` que configuraste anteriormente.

Para eso, en iOS debes agregar una entrada a `Info.plist` para majar el scheme
indicado. Por ejemplo, si tu appScheme fuera mi-app://mi-app/onepay-result,
agregarías lo siguiente:

```xml
	<key>CFBundleURLTypes</key>
	<array>
		<dict>
			<key>CFBundleURLSchemes</key>
			<array>
				<string>mi-app</string>
			</array>
			<key>CFBundleURLName</key>
			<string>cl.micomercio.mi-app</string>
		</dict>
	</array>
```

Y en tu `AppDelegate` deberás manejar esas urls que comiencen con mi-app:

```swift
func application(_ app: UIApplication, open url: URL,
                 options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {

    let sendingAppID = options[.sourceApplication]
    print("source application = \(sendingAppID ?? "Unknown")")
    guard
        let components = NSURLComponents(url: url, resolvingAgainstBaseURL: true),
        let host = components.host,
        let path = components.path,
        let params = components.queryItems
        else {
            os_log("URL, host, path o params inválidos: %s", url.absoluteString)
            return false
    }
    if (host == "mi-app" && path == "onepay-result") {
        guard
            let occ = params.first(where: { $0.name == "occ" })?.value,
            let externalUniqueNumber = params.first(where: { $0.name == "externalUniqueNumber" })?.value
            else {
                os_log("Falta un parámetro occ o externalUniqueNumber: %s", url.absoluteString)
                return false
        }
        // Envia el occ y el externalUniqueNumber a tu backend para que
        // confirme la transacción
        print(occ, externalUniqueNumber)

    } else {
        // Otras URLs que quizás maneja tu app
    }
    return false

}
```

En Android debes registrar un Activity que responda a la URL registrada en dicho appScheme. Para eso debes configurar un intent filter a tu activity.

```xml
<activity ...>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="mi-app" android:host="mi-app" android:path="onepay-result" />
  </intent-filter>
</activity>
```

Luego en ese Activity podrás obtener el resultado de la transacción a través del Intent:

```java
Intent intent = getIntent();
String occ = intent.getStringExtra("occ");
String externalUniqueNumber = intent.getStringExtra("externalUniqueNumber");
```

Finalmente deberás enviar el occ y el externalUniqueNumber a tu backend (quizás
quieras reusar el mismo `callbackUrl` que confirma transacciones en modo
checkout, pasando el status `"PRE_AUTHORIZED"` y algún parámetro adicional que
le haga saber a tu backend que devuelva JSON en lugar de dibujar una página web
con el comprobante).

Con eso has concluido la integración de Onepay, incluyendo sus cuatro componentes:
backend, frontend-web (soportando canales WEB y MOBILE) y las dos apps móviles (para el canal APP). ¡Felicitaciones!

## Integración Cortafila

Las modalidades vistas hasta ahora contemplan que tu aplicación es usada
directamente por el comprador (tarjetahabiente), quien compra directamente desde
tu app móvil o desde tu web (y es dirigido a la app Onepay de la manera más
apropiada).

Onepay ofrece la flexibilidad para ser usado también en la modalidad cortafila
en tus tiendas físicas. Allí un vendedor usando por ejemplo un tablet puede
acercarse a los compradores y procesar rápidamente su compra usando Onepay.

Para hacer funcionar este proceso la integración es un poco diferente a las
vistas anteriormente. Suponiendo que la app para el vendedor es una app móvil,
ahora verás los pasos necesarios para realizar la integración.

### 1. Tu app móvil inicia el flujo

Para comenzar, la app móvil del vendedor será la que inicie la transacción.
Esta app debe invocar a tu backend (por ejemplo a través de un API REST).

### 2. Tu backend crea la transacción Onepay

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.onepay.Onepay;
import cl.transbank.onepay.model.*;

// ...

Onepay.setCallbackUrl("https://miapp.cl/endPayment");
TransactionCreateResponse response = Transaction.create(cart, Channel.MOBILE);
```

```php
/*
 * IMPORTANTE: El ejemplo está pensado en la forma más simple de integrar que
 * es apuntando a integración. El SDK asume que usarás integración si no has
 * seteado el ambiente, razón por la cual no es necesario que lo configures.
 * Sin embargo, debes tener presente que al momento de apuntar a producción
 * deberás configurar el ambiente así como tu API_KEY y SHARED_SECRET antes
 * de realizar la llamada a Transaction::create, ya que de lo contrario estarás
 * apuntando a INTEGRACION
 *
 * ejemplo:
 * use Transbank\Onepay\OnepayBase;
 * OnepayBase::setCurrentIntegrationType('LIVE');
 * OnepayBase::setApiKey("api-key-entregado-por-transbank");
 * OnepayBase::setSharedSecret("secreto-entregado-por-transbank");
 */

use Transbank\Onepay;

OnepayBase::setCallbackUrl("https://miapp.cl/endPayment");
$channel = ChannelEnum::MOBILE();
$response = Transaction::create($cart, $channel);

```

```csharp
using Transbank.Onepay;
using Transbank.Onepay.Model;

// ...
Onepay.CallbackUrl = "https://miapp.cl/endPayment";
var response = Transaction.Create(cart, ChannelType.MOBILE);
```

```ruby
require 'transbank/sdk'

Transbank::Onepay::Base.callback_url = "https://miapp.cl/endPayment"
channel = Transbank::Onepay::Channels::MOBILE
response = Transbank::Onepay::Transaction.create(
    shopping_cart: cart, channel: channel)
```

```python
from transbank import onepay
from transbank.onepay.transaction import Transaction, Channel

onepay.callback_url = "https://miapp.cl/endPayment"
response = Transaction.create(cart, Channel.MOBILE)
```

A partir de la información entregada por la app móvil del vendedor (información que debe estar autenticada y autorizada), deberás crear una transacción usando `MOBILE` como channel. Ten en cuenta que el callback que indiques será invocado en el dispositivo móvil del comprador y no del vendedor (como verás en el paso 4).

Luego en la respuesta que le enviarás a la app móvil del vendedor debes incluir la respuesta de la transacción Onepay.

### 3. Dibujar el código QR en la app móvil

En base a lo retornado por tu backend (que a su vez retornó la transacción Onepay creada), debes dibujar el código QR para que el vendedor le permita al comprador usar su app Onepay. Para eso puedes usar el campo `qrCodeAsBase64` de la respuesta a la [creación de la transacción onepay](/referencia/onepay/#crear-una-transaccion).

Te recomendamos también implementar un timeout y/o una interfaz para que el vendedor aborte la operación. Porque desde este momento el control lo tendrá el comprador en su propio dispositivo móvil.

### 4. Retomar el control en el navegador móvil del comprador

Cuando el comprador escanee el QR con su app Onepay, no tendrás manera de saber el progreso de la operación. Pero cuando el comprador termine (exitosamente o con error), tu callback será invocado **en el navegador móvil del comprador**.

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.onepay.model.*;

String status = request.getParameter("status");
String externalUniqueNumber = request.getParameter("externalUniqueNumber");
String occ = request.getParameter("occ");

if (null != status && status.equalsIgnoreCase("PRE_AUTHORIZED")) {
    try {
        TransactionCommitResponse response =
            Transaction.commit(occ, externalUniqueNumber);
        // Procesar response
        // Si response es exitoso, renderear página de exito
        // en el navegador web del tarjetahabiente y despachar
        // una push notification al dispositivo móvil que inició
        // esta transacción.
    } catch (TransbankException e) {
        // Error al confirmar la transaccion
    }
} else {
    // Mostrar página de error
}
```

```php
/*
 * IMPORTANTE: El ejemplo está pensado en la forma más simple de integrar que
 * es apuntando a integración. El SDK asume que usarás integración si no has
 * seteado el ambiente, razón por la cual no es necesario que lo configures.
 * Sin embargo, debes tener presente que al momento de apuntar a producción
 * deberás configurar el ambiente así como tu API_KEY y SHARED_SECRET antes
 * de realizar la llamada a Transaction::commit, ya que de lo contrario estarás
 * apuntando a INTEGRACION
 *
 * ejemplo:
 * use Transbank\Onepay\OnepayBase;
 * OnepayBase::setCurrentIntegrationType('LIVE');
 * OnepayBase::setApiKey("api-key-entregado-por-transbank");
 * OnepayBase::setSharedSecret("secreto-entregado-por-transbank");
 */

use Transbank\Onepay\Transaction;

$status = $request->input("status");
$externalUniqueNumber = $request->input("externalUniqueNumber");
$occ = $request->input("occ");

if (!empty($status) && strcasecmp($status,"PRE_AUTHORIZED") == 0) {
    try {
        $response = Transaction::commit($occ, $externalUniqueNumber);
        // Procesar $response.
        // Si $response es exitoso, renderear página de exito
        // en el navegador web del tarjetahabiente y despachar
        // una push notification al dispositivo móvil que inició
        // esta transacción.
    } catch (Exception $e) {
        // Error al confirmar la transaccion
    }
} else {
    // Mostrar página de error
}
```

```csharp
using Transbank.Onepay;

 if (null != status && status.Equals("PRE_AUTHORIZED", StringComparison.InvariantCultureIgnoreCase))
 {
    try
    {
        var response = Transaction.Commit(occ, externalUniqueNumber);
        // Procesar response
        // Si response es exitoso, renderear página de exito
        // en el navegador web del tarjetahabiente y despachar
        // una push notification al dispositivo móvil que inició
        // esta transacción.
    }
    catch (TransbankException e)
    {
        // Error al confirmar la transaccion
    }
}
else
{
    // Mostrar página de error
}
```

```ruby
require 'transbank/sdk'

if params["status"] == "PRE_AUTHORIZED"
  response = Transbank::Onepay::Transaction.commit(
    occ: params["occ"],
    external_unique_number: params["external_unique_number"]
  )
  # Procesar response
  # Si response es exitoso, renderear página de exito
  # en el navegador web del tarjetahabiente y despachar
  # una push notification al dispositivo móvil que inició
  # esta transacción.
else
  # Mostrar página de error
end

rescue Transbank::Onepay::Errors::TransactionCommitError => e
  # Manejar el error de confirmación de transacción

```

```python
if (status and status.upper() == "PRE_AUTHORIZED"):
  try:
    response = Transaction.commit(occ, external_unique_number)
    # Procesar response
    # Si response es exitoso, renderear página de exito
    # en el navegador web del tarjetahabiente y despachar
    # una push notification al dispositivo móvil que inició
    # esta transacción.
  except TransactionCommitError:
    # Error al confirmar la transacción
else:
  # Mostrar página de error
```

En ese callback debes [confirmar la
transacción](/referencia/onepay#confirmar-una-transaccion). Y si tiene éxito la
transacción debes mostrar al comprador que todo salió ok **al mismo tiempo que
notificas a tu app del vendedor** (por ejemplo con una _push notification_) para
que esa app también muestre a su usuario que la venta realmente se ha cobrado.

<aside class="notice">
Como el _callback_ final está implementado usando el método
HTTP GET, debe ser idempotente: Debe funcionar correctamente aunque sea
ejecutado repetidas veces.
</aside>

### 5. Aplicación demostrativa

Si quieres ver el ejemplo funcional de una aplicación integrando Cortafilas, puedes revisar las demostraciones disponibles. Descarga la [aplicación  Android](https://play.google.com/store/apps/details?id=cl.transbank.onepay.pos) mirando su [código fuente](https://github.com/TransbankDevelopers/demo-onepay-cortafila-android), o dirígete a [la aplicación web](http://cortafilas-onepay.herokuapp.com) con su respectivo [código fuente](https://github.com/TransbankDevelopers/demo-onepay-cortafila-backend).

## Credenciales del comercio

Para establecer las credenciales de tu comercio (y no seguir usando las
credenciales pre-configuradas que solo funcionan en TEST), puedes realizarlo
así:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.onepay.Onepay;
//...

Onepay.setApiKey("api-key-entregado-por-transbank");
Onepay.setSharedSecret("secreto-entregado-por-transbank");
```

```php
use Transbank\Onepay\OnepayBase;

OnepayBase::setApiKey("api-key-entregado-por-transbank");
OnepayBase::setSharedSecret("secreto-entregado-por-transbank");
```

```csharp
using Transbank.Onepay;

Onepay.ApiKey = "api-key-entregado-por-transbank";
Onepay.SharedSecret = "secreto-entregado-por-transbank";
```

```ruby
require 'transbank/sdk'

Transbank::Onepay::Base.api_key = "entregado por transbank"
Transbank::Onepay::Base.shared_secret = "entregado por transbank"
```

```python
from transbank import onepay

onepay.api_key = "api-key-entregado-por-transbank"
onepay.shared_secret = "secreto-entregado-por-transbank"
```

También puedes configurar el api key y secret de una petición específica:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.onepay.model.Options;
//...

Options options = new Options()
                  .setApiKey("api-key-entregado-por-transbank")
                  .setSharedSecret("secreto-entregado-por-transbank");
```

```php
use Transbank\Onepay\Options;

$options = new Options(
    'api-key-entregado-por-transbank',
    'secreto-entregado-por-transbank');
```

```csharp
var options = new Options()
        {
            ApiKey = "api-key-entregado-por-transbank",
            SharedSecret = "secreto-entregado-por-transbank"
        }
```

```ruby
require 'transbank/sdk'

options = { api_key: 'api-key-entregado-por-transbank',
            shared_secret: 'shared-secret-entregado-por-transbank' }
```

```python
from transbank.onepay import Options

options = Options("api-key-entregado-por-transbank",
                  "secreto-entregado-por-transbank")
```

Esas opciones puedes pasarlas como parámetro a cualquier método
transaccional de Onepay (`Transaction.create`, `Transaction.commit` y
`Refund.create`).

### Apuntar a producción

Puedes configurar el SDK para utilizar los servicios del ambiente de `LIVE`
(Producción) de la siguiente forma:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.onepay.Onepay;
//...
Onepay.setIntegrationType(Onepay.IntegrationType.LIVE);
```

```php
use Transbank\Onepay\OnepayBase;

OnepayBase::setCurrentIntegrationType('LIVE');
```

```csharp
using Transbank.Onepay;

Onepay.IntegrationType = Transbank.Onepay.Enums.OnepayIntegrationType.LIVE;
```

```ruby
require 'transbank/sdk'

Transbank::Onepay::Base.integration_type = :LIVE
```

```python
from transbank import onepay

onepay.integration_type = onepay.IntegrationType.LIVE
```

## Conciliación de Transacciones

Una vez hayas realizado transacciones en producción quedará un historial de transacciones que puedes revisar entrando a [www.transbank.cl](https://www.transbank.cl/). Si lo deseas puedes realizar una conciliación entre tu sistema y el reporte que entrega el portal.

Para realizar la conciliación debes seguir los siguientes pasos:

1. Iniciar sesión con tu usuario y contraseña en [www.transbank.cl](https://www.transbank.cl/)

2. Luego, en el menú principal presionar "Webpay" y luego "Reporte transaccional". ![Paso 2](/images/documentacion/conciliacion1.png)

3. En la parte superior de la ventana puedes encontrar un buscador que te ayudará a filtrar, según los parámetros que gustes, las transacciones que quieras cuadrar. Para encontrar las transacciones de Onepay, en producto, debes seleccionar Webpay3G ![Paso 3](/images/documentacion/conciliacion2.png)

4. Dentro de la tabla en la imagen anterior puedes presionar el número de orden de compra para abrir los detalles de la transacción. Es en esta sección donde podrás encontrar y conciliar la mayoría de los parámetros devueltos al confirmar una transacción utilizando `getTransactionResult()`. ![Paso 4](/images/documentacion/conciliacion3.png)

5. Sólo queda realizar la conciliación. A continuación puedes ver una lista de parámetros que recibirás al momento de confirmar una transacción y a que fila de la tabla "Detalles de la transacción" corresponden (la lista completa de parámetros la puedes encontrar [acá](/referencia/onepay#confirmar-una-transaccion)).

Nombre parámetro  <br> <i> tipo </i>  | Fila en tabla
------   | -----------
responseCode <br> <i>  String  </i> | Código de respuesta
result.authorizationCode <br> <i>  String  </i> | Código de autorización
result.issuedAt <br> <i>  Number  </i> | Fecha de creación (viene con formato unix timestamp)
result.amount <br> <i>  Number  </i> | Monto de la transacción.
result.transactionDesc <br> <i>  String  </i> | Tipo de producto
result.installmentsNumber <br> <i>  Number  </i> | Número de cuotas.
result.buyOrder <br> <i>  Number  </i> | Orden de compra.

## Más funcionalidades

Consulta la [referencia del API](/referencia/onepay) para más funcionalidades ofrecidas por Onepay:

- [Especificar tus propios external unique numbers](/referencia/onepay#especificar-tus-propios-external-unique-numbers) si quieres
usarlos para asociarlos a un número que ya exista en tu aplicación (pero
recuerda que si la transacción falla y quieres reintentar debes usar otro
external unique number).

- [Anular una transacción Onepay](/referencia/onepay#anular-una-transaccion) para revertir completamente una
transacción.

- [QR Directo](/referencia/onepay#modalidad-qr-directo) para controlar tú mismo
  el frontend web (javascript) durante todo el flujo. Necesitarás hacer mucho
  más trabajo, pero tendrás total control de la interacción con el usuario, con Onepay y con tu backend.

- [Detectar y/o instalar la app Onepay desde tu app](/referencia/onepay#detectar-e-instalar-la-app-onepay) para asegurar la
  mejor experiencia de pago en apps móviles.

## Ejemplos de integración

Ponemos a tu disposición una serie de repositorios en nuestro Github para ayudarte a entender la integración mejor.

- [Ejemplo de Onepay en PHP](https://github.com/TransbankDevelopers/transbank-sdk-php-onepay-example)
- [Ejemplo de Onepay en .Net](https://github.com/TransbankDevelopers/transbank-sdk-dotnet-onepay-example)
- [Ejemplo de Onepay en Java](https://github.com/TransbankDevelopers/transbank-sdk-java-onepay-example)
- [Ejemplo de Onepay en Python](https://github.com/TransbankDevelopers/transbank-sdk-python-onepay-example)
- [Ejemplo de Onepay en Ruby](https://github.com/TransbankDevelopers/transbank-sdk-ruby-onepay-example)
- [Ejemplo de Onepay Cortafila en Android](https://github.com/TransbankDevelopers/demo-onepay-cortafila-android)
- [Ejemplo de Onepay Cortafila Backend](https://github.com/TransbankDevelopers/demo-onepay-cortafila-backend)

<div class="container slate">
  <div class='slate-after-footer'>
    <div class='row d-flex align-items-stretch'>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22'>¿Tienes alguna duda de integración?</h3>
        <a href='https://join-transbankdevelopers-slack.herokuapp.com/' target='_blank'>
          <div class='td_block_gray'>
            <img src="https://p9.zdassets.com/hc/theme_assets/138842/200037786/logo.png" alt="" style="width: 90px; min-width: 100px;">
            <div class='td_pa-txt'>
              Únete a la comunidad de integradores en Slack y comparte buenos tips ayudando a los nuevos o buscando soluciones alternativas. Nuestro equipo está ahí para ayudarte.
            </div>
          </div>
        </a>
      </div>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22'>Si aún tienes dudas envíanos un mensaje</h3>
        <a class="pointer magenta" data-toggle='modal' data-target='#modalContactForm'>
          <div class='td_block_gray'>
            <div class="fo-size-20"><i class="fas fa-envelope"></i> Envíanos un mensaje..</div>
            <div class='td_pa-txt'>
              Si necesitas resolver algún tipo de incidencia en el portal o si existe algún problema con tu integración y  que no has podido resolver, contáctanos a través de nuestro formulario.
            </div>
          </div>
        </a>
      </div>
    </div>
  </div>
</div>
