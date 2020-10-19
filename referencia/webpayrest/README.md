___

<aside class="warning">
Estás viendo la <strong>nueva referencia REST</strong> que aún no es oficial, por lo tanto no existen
herramientas de desarrollo creadas. Si quieres volver a la referencia oficial
(SOAP) haz [click aquí](/referencia/webpay)
</aside>

# Webpay Rest

## Ambientes y Credenciales

La API REST de Webpay está protegida para garantizar que solamente comercios autorizados por Transbank hagan uso de las operaciones disponibles. La seguridad esta implementada mediante los siguientes mecanismos:

* Canal seguro a través de TLSv1.2 para la comunicación del cliente con Webpay.
* Autenticación y autorización mediante el intercambio de headers `Tbk-Api-Key-Id` y `Tbk-Api-Key-Secret`.

### Ambiente de Producción

Las URLs de endpoints de producción están alojados dentro de
<https://webpay3g.transbank.cl/>.

```java
// Host: https://webpay3g.transbank.cl
```

```php
// Host: https://webpay3g.transbank.cl
```

```csharp
// Host: https://webpay3g.transbank.cl
```

```ruby
# Host: https://webpay3g.transbank.cl
```

```python
# Host: https://webpay3g.transbank.cl
```

```http
Host: https://webpay3g.transbank.cl
```

### Ambiente de Integración

```
Host: https://webpay3gint.transbank.cl
```

Las URLs de endpoints de integración están alojados dentro de
<https://webpay3gint.transbank.cl/>.

Consulta [la documentación para conocer las tarjetas de prueba que funcionan en
el ambiente de integración](/documentacion/como_empezar#ambientes).

### Credenciales del Comercio

```java
// Tbk-Api-Key-Id: Código de comercio
// Tbk-Api-Key-Secret: Secreto
// Content-Type: application/json
```

```php
// Tbk-Api-Key-Id: Código de comercio
// Tbk-Api-Key-Secret: Secreto
// Content-Type: application/json
```

```csharp
// Tbk-Api-Key-Id: Código de comercio
// Tbk-Api-Key-Secret: Secreto
// Content-Type: application/json
```

```ruby
# Tbk-Api-Key-Id: Código de comercio
# Tbk-Api-Key-Secret: Secreto
# Content-Type: application/json
```

```python
# Tbk-Api-Key-Id: Código de comercio
# Tbk-Api-Key-Secret: Secreto
# Content-Type: application/json
```

```http
Tbk-Api-Key-Id: Código de comercio
Tbk-Api-Key-Secret: Secreto
Content-Type: application/json
```

Todas las peticiones que hagas deben incluir el código de comercio y la llave
secreta entregada por Transbank, actuando ambas como las credenciales que autorizan
distintas operaciones.

<aside class="notice">
Ten en cuenta que tu(s) código(s) de comercio en ambiente de producción no son
iguales a los entregados para el ambiente de integración.
</aside>

En [el repositorio GitHub
`transbank-webpay-credenciales`](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/)
podrás encontrar [códigos de comercios y llaves secretas para probar
en integración aunque aún no tengas tu propio código de
comercio](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/tree/master/integracion). Alternativamente puedes revisar la siguiente tabla con credenciales de integración para hacer pruebas.

Producto | Código de Comercio | Secreto |
-------- | ------------ | -------------|
Webpay Plus | `597055555532` | `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`
Webpay Plus Mall | `597055555535 ` Mall <br> `597055555536` Tienda 1 <br> `597055555537 ` Tienda 2 | `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`
Oneclick Mall | `597055555541` Mall <br> `597055555542` Tienda 1 <br> `597055555543` Tienda 2 | `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`
Oneclick Mall Captura Diferida | `597055555547` Mall <br> `597055555548` Tienda 1 <br> `597055555549` Tienda 2 | `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`
Transacción Completa | `597055555530` | `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`
Transacción Completa sin CVV | `597055555557` | `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`
Transacción Completa Diferida | `597055555531` | `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`
Transacción Completa Diferida sin CVV | `597055555556` | `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`
Transacción Completa Mall | `597055555551` Mall <br> `597055555552` Tienda 1 <br> `597055555553` Tienda 2 | `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`
> Los SDKs ya incluyen esos códigos de comercio y llaves secretas
> que funcionan en el ambiente de integración, por lo que puedes obtener
> rápidamente una configuración lista para hacer tus primeras pruebas en dicho
> ambiente:

```java
// El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
WebpayPlus.Transaction.setCommerceCode(597055555532);
WebpayPlus.Transaction.setApiKey('579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C');
WebpayPlus.Transaction.setIntegrationType(IntegrationType.TEST);
```

```php
// El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
Transbank\Webpay\WebpayPlus::configureForTesting();
Transbank\Webpay\WebpayPlus::configureMallForTesting();
Transbank\Webpay\WebpayPlus::configureDeferredForTesting();
```

```csharp
// El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
WebpayPlus.CommerceCode = 597055555532;
WebpayPlus.ApiKey = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C";
WebpayPlus.IntegrationType = WebpayIntegrationType.Test;
```

```ruby
# El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
Transbank::Webpay::WebpayPlus.commerce_code = 597055555532;
Transbank::Webpay::WebpayPlus.api_key = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C";
Transbank::Webpay::WebpayPlus.integration_type = "TEST";

```

```python
# El SDK apunta por defecto al ambiente de pruebas, no es necesario configurar lo siguiente
transbank.webpay.webpay_plus.webpay_plus_default_commerce_code = 597055555532
transbank.webpay.webpay_plus.default_api_key = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C"
transbank.webpay.webpay_plus.default_integration_type = IntegrationType.TEST
```

```http
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
```

## Webpay Plus

Una transacción de autorización normal (o transacción normal), corresponde a
una solicitud de autorización financiera de un pago con tarjetas de crédito o
débito, en donde quién realiza el pago ingresa al sitio del comercio,
selecciona productos o servicio, y el ingreso asociado a los datos de la tarjeta
de crédito o débito lo realiza en forma segura en Webpay.

### Flujo en caso de éxito

De cara al tarjetahabiente, el flujo de páginas para la transacción es el
siguiente:

<img class="td_img-night" src="/images/referencia/webpayrest/flujo-paginas-webpayrest.png" alt="Flujo de páginas Webpay Plus">

Desde el punto de vista técnico, la secuencia es la siguiente:

<img class="td_img-night" src="/images/referencia/webpayrest/diagrama-secuencia-webpayrest.png" alt="Diagrama de secuencia Webpay Plus">

1. Una vez seleccionado los bienes o servicios, tarjetahabiente decide pagar a
   través de Webpay.
2. El comercio inicia una transacción en Webpay.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el
   token de la transacción y URL de redireccionamiento a la cual se deberá
   redirigir al tarjetahabiente.
4. Comercio redirecciona al tarjetahabiente hacia Webpay, con el token de la
   transacción a la URL indicada en punto 3. La redirección se realiza
   enviando por método POST el token en variable `token_ws`.
5. El navegador Web del tarjetahabiente realiza una petición HTTPS a Webpay, en
   base al redireccionamiento generado por el comercio en el punto 4.
6. Webpay responde al requerimiento desplegando el formulario de pago de Webpay.
   Desde este punto la comunicación es entre Webpay y el tarjetahabiente, sin
   interferir el comercio. El formulario de pago de Webpay despliega, entre
   otras cosas, el monto de la transacción, información del comercio como
   nombre y logotipo, las opciones de pago a través de crédito o débito.
7. Tarjetahabiente ingresa los datos de la tarjeta, hace clic en pagar en
   formulario Webpay.
8. Webpay procesa la solicitud de autorización (primero autenticación bancaria
   y luego la autorización de la transacción).
9. Una vez resuelta la autorización, Webpay retorna el control al comercio,
   realizando un redireccionamiento HTTPS hacia la página de transición
   del comercio, en donde se envía por método POST el token de la transacción
   en la variable `token_ws`. El comercio debe implementar la recepción de esta
   variable.
10. El navegador Web del tarjetahabiente realiza una petición HTTPS al
    sitio del comercio, en base a la redirección generada por Webpay en el
    punto 9.
11. El sitio del comercio recibe la variable `token_ws` e invoca el segundo
    método Web para confirmar y obtener el resultado de la autorización.
    El resultado de la autorización podrá ser consultado posteriormente con la
    variable anteriormente mencionada.
12. Comercio recibe el resultado de la confirmación.

<aside class="warning">
En la versión anterior de WebPay, había que invocar `acknowledgeTransaction()`
para informar a WebPay que se había recibido el resultado la transacción sin
problemas. Ahora no es necesario, ya que ésto se realiza de forma automática
una vez que se confirma la transacción.  Además ya no se debe mostrar el voucher
de Transbank, solo debe mostrarse desde el sitio del comercio.
</aside>

13. Sitio del comercio despliega voucher con los datos de la transacción.

### Flujo si usuario aborta el pago

<img class="td_img-night" src="/images/referencia/webpayrest/diagrama-secuencia-webpayrest-abortar.png" alt="Diagrama de secuencia si usuario aborta el pago">

Si el tarjetahabiente anula la transacción en el formulario de pago de Webpay,
el flujo cambia y los pasos son los siguientes:

1. Una vez seleccionado los bienes o servicios, tarjetahabiente decide pagar a
   través de Webpay.
2. El comercio inicia una transacción en Webpay.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el
   token de la transacción y URL de redireccionamiento a la cual se deberá
   redirigir al tarjetahabiente.
4. Comercio redirecciona al tarjetahabiente hacia Webpay, con el token de la
   transacción a la URL indicada en punto 3. La redirección se realiza
   enviando por método POST el token en variable `token_ws`.
5. El navegador Web del tarjetahabiente realiza una petición HTTPS a Webpay, en
   base al redireccionamiento generado por el comercio en el punto 4.
6. Webpay responde al requerimiento desplegando el formulario de pago de Webpay.
   Desde este punto la comunicación es entre Webpay y el tarjetahabiente, sin
   interferir el comercio. El formulario de pago de Webpay despliega, entre
   otras cosas, el monto de la transacción, información del comercio como
   nombre y logotipo, las opciones de pago a través de crédito o débito.
7. Tarjetahabiente hace clic en “anular”, en formulario Webpay.
8. Webpay retorna el control al comercio, realizando un redireccionamiento
   HTTPS hacia la página de **retorno del comercio**, en donde se envía por
   método POST el token de la transacción en la variable `TBK_TOKEN` además de las variables `TBK_ORDEN_COMPRA` y `TBK_ID_SESION`.

<aside class="warning">
Nota que el nombre de las variables recibidas es diferente. En lugar de `token_ws` acá el token viene en la variable `TBK_TOKEN`.
</aside>

9. El comercio con la variable `TBK_TOKEN` debe invocar el método
   de confirmación de transacción para obtener el resultado de la autorización. En
   este caso debe obtener una excepción, pues el pago fue abortado.
10. El comercio debe informar al tarjetahabiente que su pago no se completó.

### Crear una transacción Webpay Plus

Para crear una transacción basta llamar al método `Transaction.create()`

#### `Transaction.create()`

Permite inicializar una transacción en Webpay. Como respuesta a la invocación
se genera un token que representa en forma única una transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

```java
import cl.transbank.webpay.webpayplus.WebpayPlus;	  buyOrder, sessionId, amount, returnUrl
import cl.transbank.webpay.webpayplus.model.CreateWebpayPlusTransactionResponse;

final WebpayPlusTransactionCreateResponse response = WebpayPlus.Transaction.create(
  buyOrder, sessionId, amount, returnUrl
);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::create($buy_order, $session_id, $amount, $return_url);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Create(buyOrder, sessionId, amount, returnUrl);
```

```ruby
response = Transbank::Webpay::WebpayPlus::Transaction::create(
  buy_order: buy_order,
  session_id: session_id,
  amount: amount,
  return_url: return_url
)
```

```python
response = transbank.webpay.webpay_plus.create(buy_order, session_id, amount, return_url)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions

Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
 "buy_order": "ordenCompra12345678",
 "session_id": "sesion1234557545",
 "amount": 10000,
 "return_url": "http://www.comercio.cl/webpay/retorno"
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la tienda. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>
session_id  <br> <i> String </i> | Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
amount  <br> <i> Decimal </i> | Monto de la transacción. Máximo 2 decimales para USD. Largo máximo: 17
return_url  <br> <i> String </i> | URL del comercio, a la cual Webpay redireccionará posterior al proceso de autorización. Largo máximo: 256

**Respuesta**

```java
response.getUrl();
response.getToken();
```

```php
response.getUrl();
response.getToken();
```

```csharp
response.Url;
response.Token;
```

```ruby
response.url
response.token
```

```python
response.url
response.token
```

```http
200 OK
Content-Type: application/json

{
 "token": "e9d555262db0f989e49d724b4db0b0af367cc415cde41f500a776550fc5fddd3",
 "url": "https://webpay3gint.transbank.cl/webpayserver/initTransaction"
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.
url  <br> <i> String </i> | URL de formulario de pago Webpay. Largo máximo: 255.

### Confirmar una transacción Webpay Plus

Cuando el comercio retoma el control mediante `return_url` debes confirmar y obtener
el resultado de una transacción usando el método  `Transaction.commit()`.

#### `Transaction.commit()`

Permite confirmar y obtener el resultado de la transacción una vez que Webpay ha resuelto su autorización financiera.


```java
final CreateWebpayPlusTransactionResponse response = WebpayPlus.Transaction.commit(token);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::commit($token);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Commit(token);
```

```ruby
response = Transbank::Webpay::WebpayPlus::Transaction::commit(token: @token)
```

```python
response = transbank.webpay.webpay_plus.transaction.commit(token)
```

```http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

**Respuesta**

```java
response.getVci();
response.getAmount();
response.getStatus();
response.getBuyOrder();
response.getSessionId();
response.getCardDetail();
response.getAccountingDate();
response.getTransactionDate();
response.getAuthorizationCode();
response.getPaymentTypeCode();
response.getResponseCode();
response.getInstallmentsAmount();
response.getInstallmentsNumber();
response.getBalance();
```

```php
response->getVci();
response->getAmount();
response->getStatus();
response->getBuyOrder();
response->getSessionId();
response->getCardDetail();
response->getAccountingDate();
response->getTransactionDate();
response->getAuthorizationCode();
response->getPaymentTypeCode();
response->getResponseCode();
response->getInstallmentsAmount();
response->getInstallmentsNumber();
response->getBalance();
```

```csharp
response.Vci;
response.Amount;
response.Status;
response.BuyOrder;
response.SessionId;
response.CardDetail;
response.AccountingDate;
response.TransactionDate;
response.AuthorizationCode;
response.PaymentTypeCode;
response.ResponseCode;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.Balance;
```

```ruby
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
```

```python
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
```

```http
200 OK
Content-Type: application/json

{
  "vci": "TSY",
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
      "card_number": "6623"
  },
  "accounting_date": "0522",
  "transaction_date": "2019-05-22T16:41:21.063Z",
  "authorization_code": "1213",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor TSY (Autenticación exitosa), TSN (Autenticación fallida), TO (Tiempo máximo excedido para autenticación), ABO (Autenticación abortada por tarjetahabiente), U3 (Error interno en la autenticación), NP (No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure), ACS2 (Autenticación fallida extranjera). Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
amount  <br> <i> Decimal </i> | Formato número entero para transacciones en peso y decimal para transacciones en dólares. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
buy_order  <br> <i> String </i> | Orden de compra de la tienda indicado en `Transaction.create()`. Largo máximo: 26
session_id  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `Transaction.create()`. Largo máximo: 61.
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accounting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br> 
installments_amount <br> <i> Number </i> | Monto de las cuotas. Largo máximo: 17
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
balance  <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Obtener estado de una transacción Webpay Plus

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

#### `Transaction.status()`

Obtiene resultado de transacción a partir de un token.


```java
final StatusWebpayPlusTransactionResponse response = WebpayPlus.Transaction.status(token);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::getStatus($token);  
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Status(token);
```

```ruby
response = Transbank::Webpay::WebpayPlus::Transaction::status(token: @token)
```

```python
response = transbank.webpay.webpay_plus.transaction.status(token)
```

```http
GET /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

**Respuesta**

```java
response.getVci();
response.getAmount();
response.getStatus();
response.getBuyOrder();
response.getSessionId();
response.getCardDetail();
response.getAccountingDate();
response.getTransactionDate();
response.getAuthorizationCode();
response.getPaymentTypeCode();
response.getResponseCode();
response.getInstallmentsAmount();
response.getInstallmentsNumber();
response.getBalance();
```

```php
response->getVci();
response->getAmount();
response->getStatus();
response->getBuyOrder();
response->getSessionId();
response->getCardDetail();
response->getAccountingDate();
response->getTransactionDate();
response->getAuthorizationCode();
response->getPaymentTypeCode();
response->getResponseCode();
response->getInstallmentsAmount();
response->getInstallmentsNumber();
response->getBalance();
```

```csharp
response.Vci;
response.Amount;
response.Status;
response.BuyOrder;
response.SessionId;
response.CardDetail;
response.AccountingDate;
response.TransactionDate;
response.AuthorizationCode;
response.PaymentTypeCode;
response.ResponseCode;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.Balance;
```

```ruby
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
```

```python
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
```

```http
200 OK
Content-Type: application/json

{
  "vci": "TSY",
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
      "card_number": "6623"
  },
  "accounting_date": "0522",
  "transaction_date": "2019-05-22T16:41:21.063Z",
  "authorization_code": "1213",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor TSY (Autenticación exitosa), TSN (Autenticación fallida), TO (Tiempo máximo excedido para autenticación), ABO (Autenticación abortada por tarjetahabiente), U3 (Error interno en la autenticación), NP (No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure), ACS2 (Autenticación fallida extranjera). Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
buy_order  <br> <i> String </i> | Orden de compra de la tienda indicado en `Transaction.create()`. Largo máximo: 26
session_id  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `Transaction.create()`. Largo máximo: 61.
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accounting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br> 
installments_amount <br> <i> Number </i> | Monto de las cuotas. Largo máximo: 17
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
balance  <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Reversar o Anular un pago Webpay Plus

Este método permite a todo comercio habilitado, reembolsar o anular una
transacción que fue generada en Webpay Plus. El método permite generar el
reembolso del total o parte del monto de una transacción.
Dependiendo de la siguiente lógica de negocio la invocación a esta
operación generará una reversa o una anulación:
- Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.
- Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.

La anulación puede realizarse máximo 90 días después de la fecha de la
transacción original.

Puedes [leer más sobre la anulación en la información del
producto Webpay](/producto/webpay#anulaciones) para conocer
más detalles y restricciones.

Para anular una transacción se debe invocar al método `Transaction.refund()`.

#### `Transaction.refund()`

Permite solicitar a Webpay la anulación de una transacción realizada previamente y que se encuentra vigente.

> Los SDKs permiten indicar opcionalmente el código de comercio de la
> transacción a anular, para soportar la anulación en comercios Webpay Plus
> Mall. En comercios Webpay Plus, no es necesario especificar el código
> de comercio pues se usa el indicado en la configuración.

<aside class="notice">
El método `Transaction.refund()` debe ser invocado siempre indicando el código del comercio que realizó la transacción. En el caso de comercios Webpay Plus Mall, el código debe ser el código de la tienda virtual específica.
</aside>

```java
final RefundWebpayPlusTransactionResponse response = WebpayPlus.Transaction.refund(token, amount);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::refund($token, $amount);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Refund(token, amount);
```

```ruby
response = Transbank::Webpay::WebpayPlus::Transaction::refund(token: @token, amount: @amount)
```

```python
response = Transbank.webpay.webpay_plus.refund(token, amount)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/refunds
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "amount": 1000
}
```
**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
amount  <br> <i> Decimal </i> | (Opcional) Monto que se desea anular de la transacción. Largo máximo: 10.

**Respuesta**

```java
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getBalance();
response.getNullifiedAmount();
response.getResponseCode();
response.getType();
```

```php
response->getAuthorizationCode();
response->getAuthorizationDate();
response->getBalance();
response->getNullifiedAmount();
response->getResponseCode();
response->getType();
```

```csharp
response.AuthorizationCode;
response.AuthorizationDate;
response.Balance;
response.NullifiedAmount;
response.ResponseCode;
response.Type;
```

```ruby
response.authorization_code;
response.authorization_date;
response.balance;
response.nullified_amount;
response.response_code;
response.type;
```

```python
response.authorization_code;
response.authorization_date;
response.balance;
response.nullified_amount;
response.response_code;
response.type;
```

```http
200 OK
Content-Type: application/json
{
  "type": "NULLIFY",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20:18:20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSE o NULLIFY). Largo máximo: 10
authorization_code  <br> <i> String </i> | Código de autorización de la anulación. Largo máximo: 6
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización.
balance  <br> <i> Decimal </i> | Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17
response_code <br> <i> Number </i> | Código de resultado de la reversa/anulacion. Si es exitoso es 0, de lo contrario la reversa/anulación no fue realizada Largo Máximo: 2

En caso de error pueden aparecer los siguientes códigos de error comunes para el método `Transaction.refund()`:

Código | Descripción
------ | -----------
304 | Validación de campos de entrada nulos
245 | Código de comercio no existe
22 | El comercio no se encuentra activo
316 | El comercio indicado no corresponde al certificado o no es hijo del comercio MALL en caso de transacciones MALL
308 | Operación no permitida
274 | Transacción no encontrada
16 | La transacción no permite anulación
292 | La transacción no está autorizada
284 | Periodo de anulación excedido
310 | Transacción anulada previamente
311 | Monto a anular excede el saldo disponible para anular
312 | Error genérico para anulaciones
315 | Error del autorizador
53 | La transacción no permite anulación parcial de transacciones con cuotas

## Webpay Plus Mall

Una transacción Mall Normal corresponde a una solicitud de autorización
financiera de un conjunto de pagos con tarjetas de crédito o débito, en donde
quién realiza el pago ingresa al sitio del comercio, selecciona productos o
servicios, y el ingreso asociado a los datos de la tarjeta de crédito o débito
lo realiza una única vez en forma segura en Webpay para el conjunto de pagos.
Cada pago tendrá su propio resultado, autorizado o rechazado.

![Desagregación de un pago Webpay Mall](/images/pago-webpay-mall.png)

El Mall Webpay agrupa múltiples tiendas, son estas últimas las que pueden
generar transacciones. Tanto el mall como las tiendas asociadas son
identificadas a través de un número denominado código de comercio.

#### Flujo Webpay Plus Mall

El flujo de Webpay Plus Mall es en general el mismo que el de [Webpay Plus
Normal](#webpay-plus-normal) tanto de cara al tarjeta habiente como de cara al
integrador.

Las diferencias son:

- Se debe usar un código de comercio configurado para modalidad Mall en
  Transbank, el cual debe ser indicado al iniciar la transacción.
- Se pueden indicar múltiples transacciones, cada una asociada a un código de
  comercio de tienda (que debe estar configurada en Transbank como perteneciente
  al mall).
- Se debe verificar por separado el resultado de cada una de esas transacciones
  individualmente, pues es posible que el emisor de la tarjeta autorice algunas
  y otras no.

### Crear una transacción Webpay Plus Mall

Para crear una transacción basta llamar al método `Transaction.create()`

#### `Transaction.create()`

Permite inicializar una transacción en Webpay. Como respuesta a la invocación
se genera un token que representa en forma única una transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

```java
CreateMallTransactionDetails transactionDetails = CreateMallTransactionDetails.build()
    .add(amountMallOne, commerceCodeMallOne, buyOrderMallOne)
    .add(amountMallTwo, commerceCodeMallTwo, buyOrderMallTwo);

final CreateWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.create(buyOrder, sessionId, returnUrl, transactionDetails);
```

```php
use Transbank\Webpay\WebpayPlus;
use Transbank\Webpay\WebpayPlus\Transaction;

WebpayPlus::configureMallForTesting();

$transaction_details = [
  {
      "amount": 10000,
      "commerce_code": 597055555536,
      "buy_order": "ordenCompraDetalle1234"
  },
  {     
     "amount": 12000,
     "commerce_code": 597055555537,
     "buy_order": "ordenCompraDetalle4321"
  },
];
  
$response = Transaction::createMall($buy_order, $session_id, $return_url, $transaction_details);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var transactionDetails = new List<TransactionDetail>();
transactions.Add(new TransactionDetail(
    amountMallOne,
    commerceCodeMallOne,
    buyOrderMallOne
));
transactions.Add(new TransactionDetail(
    amountMallTwo,
    comerceCodeMallTwo,
    buyOrderMallTwo
));

var result = MallTransaction.Create(buyOrder, sessionId, returnUrl, transactionDetails);
```

```ruby
transaction_details = [
  {
      amount: 10000,
      commerce_code: 597055555536,
      buy_order: "ordenCompraDetalle1234"
  },
  {     
     amount: 12000,
     commerce_code: 597055555537,
     buy_order: "ordenCompraDetalle4321"
  },
]

response = = Transbank::Webpay::WebpayPlus::MallTransaction::create(
  buy_order: buy_order,
  session_id: session_id,
  return_url: return_url,
  details: transaction_details
)
```

```python
transaction_details = MallTransactionCreateDetails(
  amount_child_1, commerce_code_child_1, buy_order_child_1
).add(
  amount_child_2, commerce_code_child_2, buy_order_child_2
)

response = MallTransaction.create(
    buy_order=buy_order,
    session_id=session_id,
    return_url=return_url,
    details = transaction_details,
)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions

Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
 "buy_order": "ordenCompra12345678",
 "session_id": "sesion1234557545",
 "return_url": "http://www.comercio.cl/webpay/retorno",
 "details": [
     {
         "amount": 10000,
         "commerce_code": 597055555536,
         "buy_order": "ordenCompraDetalle1234"
     },
     {     
        "amount": 12000,
        "commerce_code":  597055555537,
        "buy_order": "ordenCompraDetalle4321"
     },
 ]
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Es el código único de la orden de compra generada por el comercio mall.
session_id  <br> <i> String </i> |  Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
return_url  <br> <i> String </i> | URL del comercio, a la cual Webpay redireccionará posterior al proceso de autorización Largo máximo: 256.
details  <br> <i> Array </i> | Lista de objetos, uno por cada tienda diferente del mall que participa en la transacción.
details [].amount  <br> <i> Decimal </i> | Monto de la transacción de una tienda del mall. Máximo 2 decimales para USD. Largo máximo: 10.
details [].commerce_code  <br> <i>String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo: 12.
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda del mall. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>.

**Respuesta**

```java
response.getToken();
response.getUrl();
```

```php
response->getToken();
response->getUrl();
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
response.url
```

```http
200 OK
Content-Type: application/json

{
 "token": "e9d555262db0f989e49d724b4db0b0af367cc415cde41f500a776550fc5fddd3",
 "url": "https://webpay3gint.transbank.cl/webpayserver/initTransaction"
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.
url  <br> <i> String </i> | URL de formulario de pago Webpay. Largo máximo: 256.

### Confirmar una transacción Webpay Plus Mall

Para confirmar una transacción y obtener el resultado, se debe usar el método `Transaction.commit()`

#### `Transaction.commit()`

Permite confirmar una tranascción y obtener el resultado de la transacción
una vez que Webpay ha resueltosu autorización financiera.

```java
final CommitWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.commit(token);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::commitMall($token);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = MallTransaction.commit(token);
```

```ruby
response = MallTransaction.commit(token)
```

```python
response = MallTransaction.commit(token)
```

```http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

**Respuesta**

```java
response.getAccountingDate();
response.getBuyOrder();
final CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
response.getSessionId();
response.getTransactionDate();
response.getVci();
final List<Detail> details = response.getDetails();
for (Detail detail : details) {
    detail.getAmount();
    detail.getAuthorizationCode();
    detail.getBuyOrder();
    detail.getCommerceCode();
    detail.getInstallmentsNumber();
    detail.getPaymentTypeCode();
    detail.getResponseCode();
    detail.getStatus();
}
```

```php
$response->getAccountingDate();
$response->getBuyOrder();
$card_detail = response->getCardDetail();
$card_detail->getCardNumber();
$response->getSessionId();
$response->getTransactionDate();
$response->getVci();
$details = response->getDetails();
foreach($details as $detail){
    detail->getAmount();
    detail->getAuthorizationCode();
    detail->getBuyOrder();
    detail->getCommerceCode();
    detail->getInstallmentsNumber();
    detail->getPaymentTypeCode();
    detail->getResponseCode();
    detail->getStatus();
}
```

```csharp
response.AccountingDate;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.SessionId;
response.TransactionDate;
response.Vci;
var details = response.Details;
foreach (var detail in details) {
    detail.Amount;
    detail.AuthorizationCode;
    detail.BuyOrder;
    detail.CommerceCode;
    detail.InstallmentsNumber;
    detail.PaymentTypeCode;
    detail.ResponseCode;
    detail.Status;
}
```

```ruby
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.each do |detail|
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
end
```

```python
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for detail in details:
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
```

```http
200 OK
Content-Type: application/json

{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15:43:48.523Z",
  "details": [
    {
      "amount": 10000,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "Próximamente...",
      "buy_order": "505479072",
      "status": "AUTHORIZED"
  }]
}

```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra del mall. Largo máximo: 26
session_id  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `Transaction.create()`. Largo máximo: 61.
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente.Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 16.
accouting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor TSY (Autenticación exitosa), TSN (Autenticación fallida), TO (Tiempo máximo excedido para autenticación), ABO (Autenticación abortada por tarjetahabiente), U3 (Error interno en la autenticación), NP (No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure), ACS2 (Autenticación fallida extranjera). Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
details  <br> <i> Array </i> | Lista con resultado de cada una de las transacciones enviados en `Transaction.create()`.
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
details [].payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
details [].response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br> 
details [].amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 10
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 12
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 26
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 26
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Obtener estado de una transacción Webpay Plus Mall

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

#### `Transaction.status()`

Obtiene resultado de transacción a partir de un token.

```java
final StatusWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.status(token);
```

```php
use Transbank\Webpay\WebpayPlus\Transaction;

$response = Transaction::getMallStatus($token);
```

```csharp
using Transbank.Webpay.WebpayPlus;

var response = MallTransaction.status(token)
```

```ruby
response = MallTransaction.status(token)
```

```python
response = MallTransaction.status(token)
```

```http
GET /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

**Respuesta**

```java
response.getAccountingDate();
response.getBuyOrder();
final CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
response.getSessionId();
response.getTransactionDate();
response.getVci();
final List<Detail> details = response.getDetails();
for (Detail detail : details) {
    detail.getAmount();
    detail.getAuthorizationCode();
    detail.getBuyOrder();
    detail.getCommerceCode();
    detail.getInstallmentsNumber();
    detail.getPaymentTypeCode();
    detail.getResponseCode();
    detail.getStatus();
}
```

```php
$response->getAccountingDate();
$response->getBuyOrder();
$card_detail = response->getCardDetail();
$card_detail->getCardNumber();
$response->getSessionId();
$response->getTransactionDate();
$response->getVci();
$details = response->getDetails();
foreach($details as $detail){
    detail->getAmount();
    detail->getAuthorizationCode();
    detail->getBuyOrder();
    detail->getCommerceCode();
    detail->getInstallmentsNumber();
    detail->getPaymentTypeCode();
    detail->getResponseCode();
    detail->getStatus();
}
```

```csharp
response.AccountingDate;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.SessionId;
response.TransactionDate;
response.Vci;
var details = response.Details;
foreach (var detail in details) {
    detail.Amount;
    detail.AuthorizationCode;
    detail.BuyOrder;
    detail.CommerceCode;
    detail.InstallmentsNumber;
    detail.PaymentTypeCode;
    detail.ResponseCode;
    detail.Status;
}
```

```ruby
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.each do |detail|
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
end
```

```python
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for detail in details:
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
```

```http
200 OK
Content-Type: application/json
{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15:43:48.523Z",
  "vci": "TSY",
  "details": [
    {
      "amount": 10000,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "Próximamente...",
      "buy_order": "505479072",
      "status": "AUTHORIZED"
  }]
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra del mall. Largo máximo: 26
session_id  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `Transaction.create()`. Largo máximo: 61.
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente.Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 16.
accouting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor TSY (Autenticación exitosa), TSN (Autenticación fallida), TO (Tiempo máximo excedido para autenticación), ABO (Autenticación abortada por tarjetahabiente), U3 (Error interno en la autenticación), NP (No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure), ACS2 (Autenticación fallida extranjera). Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
details  <br> <i> Array </i> | Lista con resultado de cada una de las transacciones enviados en `Transaction.create()`.
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
details [].payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
details [].response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br> 
details [].amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 10
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 12
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 26
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 26
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Reversar o Anular un pago Webpay Plus Mall

Este método permite a todo comercio habilitado reversar o anular una transacción
que fue generada en Webpay Plus Mall. El método permite generar el reembolso del
total o parte del monto de una transacción. Dependiendo de la siguiente lógica
de negocio la invocación a esta operación generará una reversa o una anulación:
- Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.
- Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.

La anulación puede realizarse máximo 90 días después de la fecha de la
transacción original.

Puedes [leer más sobre la anulación en la información del
producto Webpay](/producto/webpay#anulaciones) para conocer
más detalles y restricciones.

Para anular una transacción se debe invocar al método `Transaction.refund()`.

#### `Transaction.refund()`

Permite solicitar a Webpay la anulación de una transacción realizada previamente y que se encuentra vigente.

> Los SDKs permiten indicar opcionalmente el código de comercio de la
> transacción a anular, para soportar la anulación en comercios Webpay Plus
> Mall. En comercios Webpay Plus, no es necesario especificar el código
> de comercio pues se usa el indicado en la configuración.

<aside class="notice">
El método `Transaction.refund()` debe ser invocado siempre indicando el código del comercio que realizó la transacción. En el caso de comercios Webpay Plus Mall, el código debe ser el código de la tienda virtual específica.
</aside>

```java
final RefundWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.refund(token, buyOrder, commerceCode, amount);
```

```php
$response = Transaction::refundMall($token, $buy_order, $commerce_code, $amount);
```

```csharp
var response = Transaction.refund(token, buyOrder, commerceCode, amount);
```

```ruby
response = Transaction.refund(token, buy_order, commerce_code, amount)
```

```python
response = Transaction.refund(token, buy_order, commerce_code, amount)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/refunds
Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "commerce_code": "597055555536",
  "buy_order": "ordenCompra12345678",
  "authorization_code": "123456",
  "capture_mount": 1000
}
```
**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere anular. Si la transacción es de captura diferida, se debe usar el código obtenido al llamar a `Transaction.capture()`. Largo máximo: 6.
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere anular. Largo máximo: 26.
amount  <br> <i> Decimal </i> | (Opcional) Monto que se desea anular de la transacción. Largo máximo: 10.
commerce_id  <br> <i> Number </i> | (Opcional) Tienda mall que realizó la transacción. Largo: 12.

**Respuesta**

```java
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getBalance();
response.getNullifiedAmount();
response.getResponseCode();
response.getType();
```

```php
$response->getAuthorizationCode();
$response->getAuthorizationDate();
$response->getBalance();
$response->getNullifiedAmount();
$response->getResponseCode();
$response->getType();
```

```csharp
response.AuthorizationCode;
response.AuthorizationDate;
response.Balance;
response.NullifiedAmount;
response.ResponseCode;
response.Type;
```

```ruby
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
```

```python
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
```

```http
200 OK
Content-Type: application/json
{
  "type": "NULLIFY",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20:18:20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSE. NULLIFY). Largo máximo: 10
authorization_code  <br> <i> String </i> | Código de autorización de la anulación. Largo máximo: 6
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización.
balance  <br> <i> Decimal </i> | Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17
response_code <br> <i> Number </i> | Código de resultado de la reversa/anulación. Si es exitoso es 0, de lo contrario la reversa/anulación no fue realizada. Largo máximo: 2

En caso de error pueden aparecer los siguientes códigos de error comunes para el método `Transaction.refund()`:

Código | Descripción
------ | -----------
304 | Validación de campos de entrada nulos
245 | Código de comercio no existe
22 | El comercio no se encuentra activo
316 | El comercio indicado no corresponde al certificado o no es hijo del comercio MALL en caso de transacciones MALL
308 | Operación no permitida
274 | Transacción no encontrada
16 | La transacción no permite anulación
292 | La transacción no está autorizada
284 | Periodo de anulación excedido
310 | Transacción anulada previamente
311 | Monto a anular excede el saldo disponible para anular
312 | Error genérico para anulaciones
315 | Error del autorizador
53 | La transacción no permite anulación parcial de transacciones con cuotas

## OneClick Mall

#### Flujo de inscripción y pago

El flujo de OneClick Mall es en general el mismo que el de [Webpay
OneClick Normal](#webpay-oneclick-normal) tanto de cara al tarjeta habiente como
de cara al integrador.

Las diferencias son:

- El usuario debe estar registrado en la página del comercio "mall" agrupador,
  pero las transacciones son a nombre de las "tiendas" del mall.
- Se pueden indicar múltiples transacciones a autorizar en una misma operación.
- Se debe verificar por separado el resultado de cada una de esas transacciones
  individualmente, pues es posible que el emisor de la tarjeta autorice algunas
  y otras no.

### Crear una inscripción OneClick Mall

Para iniciar la inscripción debe usarse el método `Inscription.start()`

#### `Inscription.start()`

Permite gatillar el inicio del proceso de inscripción.

```java
OneclickMallInscriptionStartResponse response = OneclickMall.Inscription.start(userName, email, responseUrl);
```

```php
use Transbank\Webpay\Oneclick;

$response = MallInscription::start($userName, $email, $responseUrl);
```

```csharp
using Transbank.Webpay.Oneclick;

var response = Inscription.Start(userName, email, returnUrl);
```

```ruby
response = Transbank::Webpay::Oneclick::MallInscription::start(
  user_name: user_name,
  email: email,
  response_url: response_url
)
```

```python
MallInscription.start(
        user_name=user_name,
        email=email,
        response_url=response_url)
```

```http
POST /rswebpaytransaction/api/oneclick/v1.0/inscriptions

Tbk-Api-Key-Id: 597055555541 
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
 "username": "juanperez",
 "email": "juan.perez@gmail.com",
 "response_url": "http://www.comercio.cl/return_inscription"
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
username  <br> <i> String </i> | Identificador del usuario registrado en el comercio. Largo máximo: 40.
email  <br> <i> String </i> | Email del usuario registrado en el comercio. Largo máximo: 100.
response_url  <br> <i> String </i> | URL del comercio a la cual Webpay redireccionará posterior al proceso de inscripción. Largo máximo: 255.

**Respuesta**

```java
response.getToken();
response.getUrlWebpay();
```

```php
$response->getToken();
$response->getUrlWebpay();
```

```csharp
response.Token;
response.UrlWebpay
```

```ruby
response.token
response.url_webpay
```

```python
response.token
response.url_webpay
```

```http
200 OK
Content-Type: application/json

{
  "token": "e128a9c24c0a3cbc09223973327b97c8c474f6b74be509196cce4caf162a016a",
  "url_webpay": "https://webpay3g.transbank.cl/webpayserver/bp_inscription.cgi"
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Identificador, único, del proceso de inscripción. Largo: 64.
url_webpay  <br> <i> String </i> | URL de Webpay para iniciar la inscripción. Largo: 255.

<aside class="notice">
Una vez que se llama a este webservice el usuario debe ser redireccionado vía
POST a `urlInscriptionForm` con parámetro `TBK_TOKEN` igual al token.
</aside>

### Confirmar una inscripción OneClick Mall

Una vez terminado el flujo de inscripción en Transbank el usuario es enviado a
la URL de fin de inscripción que definió el comercio (`responseURL`). En ese
instante el comercio debe llamar a `Inscription.finish()`.

<aside class="warning">
El comercio tendrá un máximo de 60 segundos para llamar a este método luego
de recibir el token en la URL de fin de inscripción (`returnUrl`). Pasados los
60 segundos sin llamada a finishInscription, la inscripción en curso junto con
el usuario serán eliminados.
</aside>

#### `Inscription.finish()`

Permite finalizar el proceso de inscripción obteniendo el usuario tbk.

```java
final OneclickMallInscriptionFinishResponse response = OneclickMall.Inscription.finish(token);
```

```php
use Transbank\Webpay\Oneclick;

$response = MallInscription::finish($token);
```

```csharp
using Transbank.Webpay.Oneclick;

var response = Inscription.Finish(token);

```

```ruby
response = Transbank::Webpay::Oneclick::MallInscription::finish(token: token)
```

```python
response = MallInscription.finish(token=token)
```

```http
PUT /rswebpaytransaction/api/oneclick/v1.0/inscriptions/{token}

Tbk-Api-Key-Id: 597055555541 
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Identificador del proceso de inscripción. Es entregado por Webpay en la respuesta del método `Inscription.start()`. (See envía en la URL, no en el body)

**Respuesta**

```java
response.getAuthorizationCode();
response.getCardType();
response.getCardNumber();
response.getResponseCode();
response.getTbkUser();
```

```php
response->getAuthorizationCode();
response->getCardType();
response->getCardNumber();
response->getResponseCode();
response->getTbkUser();
```

```csharp
response.ResponseCode;
response.TransbankUser;
response.AuthorizationCode;
response.CardType;
response.CardNumber;
```

```ruby
response.response_code
response.transbank_user
response.authorization_code
response.card_type
response.card_number
```

```python
response.response_code
response.transbank_user
response.authorization_code
response.card_type
response.card_number
```

```http
200 OK
Content-Type: application/json

{
  "response_code": 0,
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "authorization_code": "123456",
  "card_type": "Visa",
  "card_number": "XXXXXXXXXXXX6623"
}

```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
response_code  <br> <i> Number </i> | Código de retorno del proceso de la autorización, donde 0 (cero) es aprobado. Largo: 2.
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente en OneClick, que debe ser usado para realizar pagos o borrar la inscripción. Largo: 40.
authorization_code  <br> <i> String </i> | Código que identifica la autorización de la inscripción. Largo: 6.
card_type  <br> <i> cardType </i> | Indica el tipo de tarjeta inscrita por el cliente (Visa, AmericanExpress, MasterCard, Diners, Magna). Largo: 10.
card_number  <br> <i> String </i> | Últimos 4 dígitos de la tarjeta inscrito: Largo: 4.

### Eliminar una inscripción con Oneclick Mall
Una vez finalizado el proceso de inscripción es posible eliminarla de ser necesario. Para esto debes usar el método llamado `Inscription.remove()`.

#### `Inscription.remove()`
Permite eliminar un usuario enrolado a Oneclick Mall.

```java
Oneclick.MallInscription.delete(tbkUser, userName)
```

```php
use Transbank\Webpay\Oneclick;

MallInscription::delete($tbkUser, $userName);
```

```csharp
using Transbank.Webpay.Oneclick;

Inscription.Delete(userName, tbkUser);
```

```ruby
MallInscription::delete(user_name: user_name, tbk_user: tbk_user)
```

```python
MallInscription.delete(tbk_user, user_name)
```

```http
DELETE /rswebpaytransaction/api/oneclick/v1.0/inscriptions

Tbk-Api-Key-Id: 597055555541 
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json


{
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "username": "juanperez",
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente (devuelto por `Inscription.finish()`). Largo: 40.
username  <br> <i> String </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `Inscription.start()`). Largo máximo: 40.

**Respuesta**

```java
// 204 OK
```

```php
// 204 OK
```

```csharp
// 204 OK
```

```ruby
# 204 OK
```

```python
# 204 OK
```

```http
204 OK
Content-Type: application/json
```

### Autorizar un pago con OneClick Mall

Una vez realizada la inscripción, el comercio puede usar el `tbkUser` recibido
para realizar transacciones. Para eso debes usar el método `Transaction.authorize()`.

#### `Transaction.authorize()`

Permite autorizar un pago.

```java
MallTransactionCreateDetails transactionDetails = MallTransactionCreateDetails.build()
  .add(amountMallOne, commerceCodeMallOne, buyOrderMallOne, installmentsNumberMallOne)
  .add(amountMallTwo, commerceCodeMallTwo, buyOrderMallTwo, installmentsNumberMallTwo);

final OneclickMallTransactionAuthorizeResponse response = OneclickMall.Transaction.authorize(username, tbkUser, buyOrder, transactionDetails);
```

```php
use Transbank\Webpay\Oneclick;

$details = [
    [
        "commerce_code" => "597055555542",
        "buy_order" => "ordenCompra123445",
        "amount" => 1000,
        "installments_number" => 5
    ]
];


$response = MallTransaction::authorize($userName, $tbkUser, $parentBuyOrder, $details);
```

```csharp
using Transbank.Webpay.Oneclick;

List<PaymentRequest> details = new List<PaymentRequest>();
details.Add(new PaymentRequest(
  childCommerceCodeOne, buyOrderMallOne, amountMallOne, installmentsNumber
));
details.Add(new PaymentRequest(
  childCommerceCodeTwo, buyOrderMallTwo, amountMallTwo, installmentsNumber
));

var result = MallTransaction.Authorize(userName, tbkUser, buyOrder, details);
```

```ruby
details = [
  {
    commerce_code: amountMallOne,
    buy_order: buyOrderMallOne,
    amount: amountMallOne,
    installments_number: installmentsNumberMallOne
  },
  {
    commerce_code: amountMallTwo,
    buy_order: buyOrderMallTwo,
    amount: amountMallTwo,
    installments_number: installmentsNumberMallTwo
  }
]

Transbank::Webpay::Oneclick::MallTransaction::authorize(username: username,
                                                       tbk_user: tbk_user,
                                                       parent_buy_order: buy_order,
                                                       details: details)
```

```python
details = MallTransactionAuthorizeDetails(
  commerce_code, buy_order_child, installments_number, amount
).add(
  commerce_code2, buy_order_child2, installments_number2, amount2
)

MallTransaction.authorize(
  user_name=user_name,
  tbk_user=tbk_user,
  buy_order=buy_order,
  details=details
)
```

```http
POST /rswebpaytransaction/api/oneclick/v1.0/transactions

Tbk-Api-Key-Id: 597055555541 
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "username": "juanperez",
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "buy_order": "ordenCompra123456789",
  "details": [
    {
      "commerce_code": "597055555542",
      "buy_order": "ordenCompra123445",
      "amount": 1000,
      "installments_number": 5
  }]
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
username  <br> <i> String </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `Inscription.start()`). Largo máximo: 40.
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente (devuelto por `Inscription.finish()`). Largo: 40.
buy_order  <br> <i> Number </i> | Identificador único de la compra generado por el comercio. Largo máximo: 26.
details  <br> <i> Array </i> | Lista de objetos, uno por cada tienda diferente del mall que participa en la transacción. 
details [].commerce_code  <br> <i>String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo: 12.
details [].buy_order  <br> <i> String </i> | Identificador único de la compra generado por el comercio hijo (tienda). Largo máximo: 26.
details [].amount  <br> <i> Decimal </i> | Monto de la transacción de pago. Largo máximo: 17.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la transacción de pago. Largo 2. No obligatorio.

**Respuesta**

```java
response.getAccountingDate();
response.getBuyOrder();
response.getTransactionDate();
final CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
final List<Detail> detailsResp = response.getDetails();
for (Detail detail : detailsResp) {
    detail.getAmount();
    detail.getAuthorizationCode();
    detail.getBuyOrder();
    detail.getCommerceCode();
    detail.getInstallmentsNumber();
    detail.getPaymentTypeCode();
    detail.getStatus();
}
```

```php
$response->getAccountingDate();
$response->getBuyOrder();
$card_detail = response->getCardDetail();
$card_detail->getCardNumber();
$response->getTransactionDate();
$response->getVci();
$details = response->getDetails();
foreach($details as $detail){
    detail->getAmount();
    detail->getAuthorizationCode();
    detail->getBuyOrder();
    detail->getCommerceCode();
    detail->getInstallmentsNumber();
    detail->getPaymentTypeCode();
    detail->getResponseCode();
    detail->getStatus();
}
```

```csharp
response.AccountingDate;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.SessionId;
response.TransactionDate;
response.Vci;
var details = response.Details;
foreach (var detail in details) {
    detail.Amount;
    detail.AuthorizationCode;
    detail.BuyOrder;
    detail.CommerceCode;
    detail.InstallmentsNumber;
    detail.PaymentTypeCode;
    detail.ResponseCode;
    detail.Status;
}
```

```ruby
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.each do |detail|
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
end
```

```python
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for detail in details:
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
```

```http
200 OK
Content-Type: application/json

{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15:43:48.523Z",
  "details": [
    {
      "amount": 500,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "Próximamente...",
      "buy_order": "505479072"
  }]
}

```
Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra generada por el comercio padre.
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_detail.card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción.
accounting_date  <br> <i> String </i> | Fecha contable de la autorización del pago.
transaction_date  <br> <i> DateTime </i> | Fecha completa (timestamp) de la autorización del pago. ISO 8601
details  <br> <i> Array </i> | Lista con el resultado de cada transacción de las tiendas hijas.
details [].amount  <br> <i> Decimal </i> | Monto de la transacción de pago.
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED).
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción de pago.
details [].payment_type_code  <br> <i> String </i> | [Tipo](/producto/webpay#tipos-de-pago) de pago de la transaccion. <br> VD = Venta Débito.<br>VN = Venta Normal.<br>VC = Venta en cuotas.<br>SI = 3 cuotas sin interés.<br>S2 = 2 cuotas sin interés.<br>NC = N Cuotas sin interés<br>
details [].response_code  <br> <i> Number </i> | Código del resultado del pago, donde: 0 (cero) es aprobado. <br> -1, -2, -3, -4, -5, -6, -7, -8: Rechazo <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la transacción de pago.
details [].commerce_code  <br> <i> Number </i> | Código de comercio del comercio hijo (tienda).
details [].buy_order  <br> <i> String </i> | Orden de compra generada por el comercio hijo para la transacción de pago.

<aside class="warning">
Cualquier valor distinto de número en `installmentsNumber` (incluyendo letras,
inexistencia del campo o nulo) será asumido como cero, es decir "Sin cuotas".
</aside>

### Consultar un pago realizado con OneClick Mall

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

#### `Transaction.status()`

Permite consultar el estado de pago realizado a través de Oneclick.
Retorna el resultado de la autorización.

```java
final OneclickMallTransactionStatusResponse response =
  OneclickMall.Transaction.status(buyOrder);
```

```php
use Transbank\Webpay\Oneclick;

$response = MallTransaction::getStatus($buyOrder);
```

```csharp
using Transbank.Webpay.Oneclick;

var result = MallTransaction.Status(buyOrder);
```

```ruby
response = Transbank::Webpay::Oneclick::MallTransaction::status(buy_order: buy_order)
```

```python
var response = MallTransaction.status(buy_order)
```

```http
GET /rswebpaytransaction/api/oneclick/v1.0/transactions/{buyOrder}

Tbk-Api-Key-Id: 597055555541 
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la transacción a consultar (se envía en la URL, no en el body).

**Respuesta**

```java
response.getAccountingDate();
response.getBuyOrder();
response.getTransactionDate();
final CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
final List<Detail> detailsResp = response.getDetails();
for (Detail detail : detailsResp) {
    detail.getAmount();
    detail.getAuthorizationCode();
    detail.getBuyOrder();
    detail.getCommerceCode();
    detail.getInstallmentsNumber();
    detail.getPaymentTypeCode();
    detail.getStatus();
}
```

```php
$response->getAccountingDate();
$response->getBuyOrder();
$card_detail = response->getCardDetail();
$card_detail->getCardNumber();
$response->getTransactionDate();
$response->getVci();
$details = response->getDetails();
foreach($details as $detail){
    detail->getAmount();
    detail->getAuthorizationCode();
    detail->getBuyOrder();
    detail->getCommerceCode();
    detail->getInstallmentsNumber();
    detail->getPaymentTypeCode();
    detail->getResponseCode();
    detail->getStatus();
}
```

```csharp
response.AccountingDate;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.SessionId;
response.TransactionDate;
response.Vci;
var details = response.Details;
foreach (var detail in details) {
    detail.Amount;
    detail.AuthorizationCode;
    detail.BuyOrder;
    detail.CommerceCode;
    detail.InstallmentsNumber;
    detail.PaymentTypeCode;
    detail.ResponseCode;
    detail.Status;
}
```

```ruby
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.each do |detail|
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
end
```

```python
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for detail in details:
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
```

```http
200 OK
Content-Type: application/json

{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15:43:48.523Z",
  "details": [
    {
      "amount": 500,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "Próximamente...",
      "buy_order": "505479072"
  }]
}

```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra generada por el comercio padre.
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_detail.card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción.
accounting_date  <br> <i> String </i> | Fecha contable de la autorización del pago.
transaction_date  <br> <i> DateTime </i> | Fecha completa (timestamp) de la autorización del pago.
details  <br> <i> Array </i> | Lista con el resultado de cada transacción de las tiendas hijas.
details [].amount  <br> <i> Decimal </i> | Monto de la sub-transacción de pago.
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED).
details [].authorization_code  <br> <i> String </i> | Código de autorización de la sub-transacción de pago.
details [].payment_type_code  <br> <i> String </i> | [Tipo](/producto/webpay#tipos-de-pago) (VC, NC, SI, etc.) de la sub-transacción de pago.
details [].response_code  <br> <i> Number </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. <br> -1, -2, -3, -4, -5, -6, -7, -8: Rechazo <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la sub-transacción de pago.
details [].commerce_code  <br> <i> Number </i> | Código de comercio del comercio hijo (tienda).
details [].buy_order  <br> <i> String </i> | Orden de compra generada por el comercio hijo para la sub-transacción de pago.
status  <br> <i> Text </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
balance  <br> <i> Decimal </i> | Monto restante de la sub-transacción de pago original: monto inicial – monto anulado. Largo máximo: 17


### Reversar o Anular un pago OneClick Mall

Para OneClick Mall hay dos operaciones diferentes para dejar sin efecto
transacciones autorizadas: La reversa y la anulación.

**La reversa** se aplica para **problemas operacionales (lado comercio) o de
comunicación entre comercio y Transbank que impidan recibir a tiempo la
respuesta de una autorización**. En tal caso el comercio **debe** intentar
reversar la transacción de autorización para evitar un posible descuadre entre
comercio y Transbank. La reversa funciona sobre la operación completa del mall,
lo que significa que **todas las transacciones realizadas en la operación mall
serán reversadas**. 

**La anulación**, en cambio, actúa individualmente sobre las transacciones de
las _tiendas_ de un mall. Por ende, **la anulación es la operación correcta a
utilizar para fines financieros**, de manera de anular un cargo ya realizado.
Permite generar el reembolso del total o parte del monto de una transacción completa. 
Dependiendo de la siguiente lógica de negocio la invocación a esta operación generará una 
reversa o una anulación:

<strong>Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.

Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.</strong>

#### `Transaction.refund()`

Permite reversar o anular una transacción de venta autorizada con anterioridad.
Este método retorna como respuesta un identificador único de la transacción de reversa/anulación.

```java
final OneclickMallTransactionRefundResponse response =
  OneclickMall.Transaction.refund(buyOrder, childCommerceCode, childBuyOrder, amount);
```

```php
use Transbank\Webpay\Oneclick;

$response = MallTransaction::refund($buyOrder, $childCommerceCode, $childBuyOrder, $amount);
```

```csharp
using Transbank.Webpay.Oneclick;

var response = MallTransaction.Refund(buyOrder, childCommerceCode,childBuyOrder,amount);
```

```ruby
response = Transbank::Webpay::Oneclick::MallTransaction::refund(
  buy_order: buy_order,
  child_commerce_code: child_commerce_code,
  child_buy_order: child_buy_order,
  amount: amount
)
```

```python
var response = MallTransaction.refund(buy_order, child_commerce_code, child_buy_order, amount)
```

```http
POST /rswebpaytransaction/api/oneclick/v1.0/transactions/{buyOrder}/refunds

Tbk-Api-Key-Id: 597055555541 
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "commerce_code": "597055555542",
  "detail_buy_order": "ordenCompra12345",
  "amount": 1000
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la transacción a  reversar o anular. Se envía en la URL, no en el body. Largo máximo: 26. 
commerce_code  <br> <i> String </i> | Código de comercio hijo. Largo máximo: 12.
detail_buy_order  <br> <i> String </i> | Orden de compra hija de la transacción a  reversar o anular. Largo máximo: 26.
amount  <br> <i> Number </i> | (Opcional) Monto a anular. Si está presente se ejecuta una anulación, en caso contrario se ejecuta una reversa (a menos que haya pasado el tiempo máximo para reversar).

**Respuesta**

```java
response.getAuthorizationCode();
response.getAuthorizationDate();
response.getBalance();
response.getNullifiedAmount();
response.getResponseCode();
response.getType();
```

```php
response->getAuthorizationCode;
response->getAuthorizationDate;
response->getBalance;
response->getNullifiedAmount;
response->getResponseCode;
response->getType;
```

```csharp
response.AuthorizationCode;
response.AuthorizationDate;
response.Balance;
response.NullifiedAmount;
response.ResponseCode;
response.Type;
```

```ruby
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
```

```python
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
```

```http
200 OK
Content-Type: application/json

{
  "type": "NULLIFY",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20:18:20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSE o NULLIFY). Largo máximo: 10
authorization_code  <br> <i> Boolean </i> | Código de autorización. Largo máximo: 6
authorization_date  <br> <i> ISO8601 </i> | Fecha de la autorización de la transacción.
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17
balance  <br> <i> Decimal </i> | Monto restante de la transacción de pago original: monto inicial – monto anulado. Largo máximo: 17
response_code <br> <i> Number </i> | Código del resultado del pago, donde: 0 (cero) es aprobado. Largo máximo: 2
buy_order  <br> <i> String </i> | Orden de compra generada por el comercio hijo para la transacción de pago. Largo máximo: 26.

### Captura Diferida OneClick Mall

Una transacción OneClick Mall permite que el tarjetahabiente registre su
tarjeta de la misma forma en que ocurre con una transacción OneClick, asociando
dicha inscripción a un comercio padre. Ahora, una vez realizada la inscripción, 
el comercio padre tiene permitido autorizar transacciones sin captura para
los comercios “hijo” registrados que tengan habilitado captura diferida.
Además posterior a la autorización tiene permitido capturar dicho monto reservado.
La autorización se encarga de validar si es posible realizar el cargo a la
cuenta asociada a la tarjeta de crédito realizando en el mismo acto la reserva
de monto de la transacción.
La captura hace efectiva la reserva hecha previamente o cargo en la cuenta de
crédito asociada a la tarjeta del titular.
Estas modalidades, por separado, solo son válidas para tarjetas de crédito.

Para realizar esa captura explícita debe usarse el método `capture()`

#### `capture()`

Este método permite a los comercios OneClick Mall habilitados, poder
realizar capturas diferidas de una transacción previamente autorizada. El método
contempla una única captura por cada autorización. Para ello se deberá indicar los
datos asociados a la transacción de venta y el monto requerido para capturar, el cual
debe ser menor o igual al monto originalmente autorizado.
Para capturar una transacción, ésta debe haber sido creada por un código de
comercio configurado para captura diferida. De esta forma la transacción estará
autorizada pero requerirá una captura explícita posterior para confirmar la
transacción.

```java
// Esta funcion aun no se encuentra disponible en el SDK
```

```php
// Esta funcion aun no se encuentra disponible en el SDK
```

```csharp
// Esta funcion aun no se encuentra disponible en el SDK
```

```ruby
# Esta funcion aun no se encuentra disponible en el SDK
```

```python
# Esta funcion aun no se encuentra disponible en el SDK
```

```http
PUT /rswebpaytransaction/api/oneclick/mall/v1_0/transactions/capture
Tbk-Api-Key-Id: 597055555547
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
{
    "commerce_code": 597055555548,
    "buy_order": "OCDT12345678",
    "capture_amount": 50,
    "authorization_code": "1213"
}
```
**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
commerce_code  <br> <i> Number </i> | Tienda hija que realizó la transacción. Largo: 6.
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
capture_amount  <br> <i> Decimal </i> | Monto que se desea capturar. Largo máximo: 17.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere capturar Largo máximo: 6.

<aside class="notice">
El método `capture()` debe ser invocado siempre indicando el código del
comercio de la tienda virtual específica.
</aside>

**Respuesta**

```java
// Esta función aun no se encuentra disponible en el SDK
```

```php
// Esta función aun no se encuentra disponible en el SDK
```

```csharp
// Esta función aun no se encuentra disponible en el SDK
```

```ruby
# Esta función aun no se encuentra disponible en el SDK
```

```python
# Esta función aun no se encuentra disponible en el SDK
```

```http
200 OK
Content-Type: application/json
{
    "authorization_code": "152759",
    "authorization_date": "2020-04-03T01:49:50.181Z",
    "captured_amount": 50,
    "response_code": 0
}
```
Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
authorization_code  <br> <i> String </i> | Código de autorización de la captura diferida. Largo máximo: 6
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización.
captured_amount  <br> <i> Decimal </i> | Monto capturado. Largo máximo: 6
response_code  <br> <i> Number </i> | Código de resultado de la captura. Si es exitoso es 0,de lo contrario la captura no fue realizada. Largo máximo: 2

<aside class="notice">
En caso de error apareceran los mismos códigos exclusivos del método `capture()`
para captura simpultanea.
</aside>

## Transacción Completa {data-submenuhidden=true}

Una transacción completa permite al comercio presentar al tarjetahabiente un
formulario propio para almacenar los datos de la tarjeta, fecha de vencimiento
y cvv (no necesario para comercios con la opción `sin cvv` habilitada) . 


### Crear una Transacción Completa

Para crear una transacción completa basta llamar al método `Transaction.create()`

#### `Transaction.create()`

Permite inicializar una transacción completa en Webpay. Como respuesta a la
invocación se genera un token que representa en forma única una transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

```java
final FullTransactionCreateResponse response =  FullTransaction.Transaction.create(
  buyOrder,                         // ordenCompra12345678
  sessionId,                        // sesion1234564
  amount,                           // 10000
  cardNumber,                       // 123
  cardExpirationDate,               // 4239000000000000
  cvv                               // 22/10
);
```

```php
use Transbank\TransaccionCompleta\Transaction;

$response = Transaction::create(
  $buy_order,                       // ordenCompra12345678
  $session_id,                      // sesion1234564
  $amount,                          // 10000
  $cvv,                             // 123
  $card_number,                     // 4239000000000000
  $card_expiration_date             // 22/10
);
```

```csharp
FullTransaction.Create(
  buyOrder: buy_order,                        // ordenCompra12345678
  sessionId: session_id,                      // sesion1234564
  amount: amount,                             // 10000
  cvv: cvv,                                   // 123
  cardNumber: card_number,                    // 4239000000000000
  cardExpirationDate: card_expiration_date    // 22/10
);
```

```ruby
Transbank::TransaccionCompleta::Transaction::create(
  buy_order: 'ordenCompra12345678',                 
  session_id: 'sesion1234564',
  amount: 10000,
  card_number: 4239000000000000,
  cvv: 123,
  card_expiration_date: '22/10'
)
```

```python
from transbank.transaccion_completa.transaction import Transaction

Transaction.create(
    buy_order = 'ordenCompra12345678',
    session_id = 'sesion1234564',
    amount = 10000,
    card_number = 4239000000000000,
    cvv = 123,
    card_expiration_date = '22/10'
)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions

Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234564",
  "amount": 10000,
  "cvv": 123,
  "card_number": "4239000000000000",
  "card_expiration_date": "22/10"
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la tienda. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>
session_id  <br> <i> String </i> | Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
amount  <br> <i> Decimal </i> | Monto de la transacción. Máximo 2 decimales para USD. Largo máximo: 17
cvv  <br> <i> String </i> | (Opcional) Código que se utiliza como método de seguridad en transacciones en las que la tarjeta no está físicamente presente. Largo máximo: 4. No se debe enviar para comercios con la opción `sin cvv` habilitada. 
card_number  <br> <i> String </i> | Número de tarjeta. Largo máximo: 16
card_expiration_date  <br> <i> String </i> | Fecha de expiración de la tarjeta con la que se realiza la transacción. Largo máximo: 5

**Respuesta**

```java
response.getToken();
```

```php
$response->getToken();
```

```csharp
response.Token;
```

```ruby
response.token
```

```python
response.token
```

```http
200 OK
Content-Type: application/json

{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.


#### Modalidad `sin cvv`
Para modalidad del producto Transacción completa `sin CVV`, este campo **no** debe ser enviado.
```json
{
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234564",
  "amount": 10000,
  "card_number": "4239000000000000",
  "card_expiration_date": "22/10"
}
```

### Consulta de cuotas

Para consultar el valor de las cuotas que pagará el tarjeta habiente en una
transacción completa, es necesario llamar al método `Transaction.installments()`

#### `Transaction.installments()`

Operación que permite obtener el monto de la cuota a partir del número de cuotas.
El id de la consulta que selecciona el tarjetahabiente debe ser informado en la
invocación de la confirmación.

```java
final FullTransactionInstallmentsResponse response =  FullTransaction.Transaction.installment(token, installments_number);
```

```php
use Transbank\TransaccionCompleta\Transaction;

$installments = Transaction::installments($token_ws, $installments_number);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Installments(
                token,
                installments_number);
```

```ruby
 Transbank::TransaccionCompleta::Transaction::installments(token: token,
                                                           installments_number: installments_number)
```

```python
from transbank.transaccion_completa.transaction import Transaction

Transaction.installments(token=token, installments_number=installments_number)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/installments

Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "installments_number": 10
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2

**Respuesta**

```java
response.getInstallmentsAmount();
response.getIdQueryInstallments();
DeferredPeriod deferredPeriod = response.getDeferredPeriods()[0];
deferredPeriod.getAmount();
deferredPeriod.getPeriod();
```

```php
response->getInstallmentsAmount();
response->getIdQueryInstallments();
response->getDeferredPeriods();
```

```csharp
response.InstallmentsAmount;
respone.IdQueryInstallments;
response.DeferredPeriods;
```

```ruby
response.installments_amount
response.id_query_installments
response.deferred_periods
```

```python
response.installments_amount
response.id_query_installments
response.deferred_periods
```

Si el comercio no tiene configurado periodos diferidos, la respuesta de `deferred_periods` será `[]`:   
```json
{
  "installments_amount": 20,
  "id_query_installments": 2190124,
  "deferred_periods": []
}
```


```http
200 OK
Content-Type: application/json

{
  "installments_amount": 3334,
  "id_query_installments": 11,
  "deferred_periods": [
    {
      "amount": 1000,
      "period": 1
    }
  ]
}
```


Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
installments_amount  <br> <i> String </i> | Monto de cada cuota. Largo: 17.
id_query_installments  <br> <i> String </i> | Identificador de las cuotas. Largo: 19.
deferred_periods  <br> <i> Array </i> | Arreglo con periodos diferidos.
deferred_periods [].amount  <br> <i> String </i> | Monto. Largo: 17.
deferred_periods [].period  <br> <i> String </i> | Índice de periodo. Largo: 2.

### Confirmación de la transacción

Una vez iniciada la transacción y consultado el monto de las cuotas, puedes
confirmar y obtener el resultado de una transacción completa usando el metodo
`Transaction.commit()`.

#### `Transaction.commit()`

Operación que permite confirmar una transacción. Retorna el estado de la
transacción.

```java
final FullTransactionCommitResponse response = FullTransaction.Transaction.commit(
  token, idQueryInstallments, deferredPeriodIndex, gracePeriod
);
```

```php
use Transbank\TransaccionCompleta\Transaction;

Transaction::commit(
  $token_ws,
  $id_query_installments,
  $deferred_period_index,
  $grace_period
);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Commit(
  token, idQueryInstallments, deferredPeriodsIndex, gracePeriods
);
```

```ruby
Transbank::TransaccionCompleta::Transaction::commit(
  token: token,
  id_query_installments: id_query_installments,
  deferred_period_index: deferred_period_index,
  grace_period: grace_period
)
```

```python
from transbank.transaccion_completa.transaction import Transaction

Transaction.commit(
  token=token,
  id_query_installments=id_query_installments,
  deferred_period_index=deferred_period_index,
  grace_period=grace_period
)
```

```http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}

Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "id_query_installments": 15,
  "deferred_period_index": 1,
  "grace_period": false
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
id_query_installments  <br> <i> Number </i> | (Opcional) Identificador de cuota. Largo máximo: 19. Solo enviar si el pago es en cuotas
deferred_period_index  <br> <i> Number </i> | (Opcional) Cantidad de periodo diferido. Largo máximo: 2. Solo enviar si el pago es en cuotas
grace_period  <br> <i> Boolean </i> | (Opcional) Indicador de periodo de gracia. Solo enviar si el pago es en cuotas

**Respuesta**

```java
response.getVci();
response.getAccountingDate();
response.getAmount();
response.getAuthorizationCode();
response.getBuyOrder();
CardDetail cardDetail = response.getCardDetail();
cardDetail.getCardNumber();
response.getInstallmentsAmount();
response.getInstallmentsNumber();
response.getPaymentCodeType();
response.getResponseCode();
response.getSessionId();
response.getTransactionDate();
```

```php
response->getVci();
response->getAccountingDate();
response->getAmount();
response->getAuthorizationCode();
response->getBuyOrder();
cardDetail = response->getCardDetail();
cardDetail->getCardNumber();
response->getInstallmentsAmount();
response->getInstallmentsNumber();
response->getPaymentCodeType();
response->getResponseCode();
response->getSessionId();
response->getTransactionDate();
```

```csharp
{
  Vci: "123",
  AccountingDate: "1219",
	Amount: 10000,
	AuthorizationCode: "1213",
	BuyOrder: "543769748",
	CardDetail: {,
    CardNumber: 6623
  }
	InstallmentsAmount: 1000,
	InstallmentsNumber: 0,
	PaymentTypeCode: "NC",
	ResponseCode: 0,
	SessionId: "105360",
	Status: "AUTHORIZED",
	TransactionDate: "2019-12-19T19:46:19.352Z"
}
```ruby
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
```

```python
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
```

```http
200 OK
Content-Type: application/json

{
  "vci": "TSY",
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
    "card_number": "1234"
  },
  "accounting_date": "0320",
  "transaction_date": "2019-03-20T20:18:20Z",
  "authorization_code": "877550",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0,
  "installmentsAmount": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
vci  <br> <i> String </i> | Código de respuesta de la autenticación bancaria. Largo máximo: 4. Este campo solo se recibe cuando la transacción es autenticada.
amount  <br> <i> Number </i> | Monto de la transacción. Sólo en caso de dolar acepta dos decimales. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
buy_order  <br> <i> String </i> | Número de orden de compra. Largo máximo: 26
session_id  <br> <i> String </i> | ID de sesión de la compra. Largo máximo: 61
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción, solo si el comercio tiene configurado el poder recibir el número de tarjeta. Largo máximo: 19
accounting_date  <br> <i> String </i> | Fecha contable de la transacción en formato MMYY.
transactionD_date  <br> <i> ISO8601 </i> | Fecha de la transacción.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción de pago. Largo máximo: 6
payment_type_code  <br> <i> String </i> | Indica el tipo de tarjeta utilizada. Largo máximo: 2
response_code  <br> <i> Number </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br> 
installments_number <br> <i> Number </i> | Número de cuotas de la transacción. Largo máximo: 2

### Consultar estado de una transacción completa

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

#### `Transaction.status()`

Obtiene resultado de transacción a partir de un token.


```java

```

```php
use Transbank\TransaccionCompleta\Transaction;

Transaction::getStatus($token_ws);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Status(token);
```

```ruby
Transbank::TransaccionCompleta::Transaction::status(token: token)
```

```python
from transbank.transaccion_completa.transaction import Transaction

Transaction.status(token=token)
```

```http
GET /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

**Respuesta**

```java
import cl.transbank.webpay.exception.RefundTransactionException;
import cl.transbank.webpay.oneclick.OneclickMall;
import cl.transbank.webpay.oneclick.model.RefundOneclickMallTransactionResponse;

public class IntegrationExample {
    public static void main(String[] args) {
      String token = "a token obtained through create transaction";
        try {
            final FullTransactionStatusResponse response = FullTransaction.Transaction.status(token);
        } catch (TransactionStatusException | IOException e) {
            return new ErrorController().error();
        }
    }
}
```

```php
object(Transbank\TransaccionCompleta\TransactionStatusResponse)#303 (13) 
{
  ["vci"]=> NULL
  ["amount"]=> int(1000)
  ["status"]=> string(10) "AUTHORIZED"
  ["buyOrder"]=> string(6) "123456"
  ["sessionId"]=> string(13) "session123456"
  ["cardDetail"]=> array(1) {
    ["card_number"]=> string(4) "6623"
  }
  ["accountingDate"]=> string(4) "1219"
  ["transactionDate"]=> string(24) "2019-12-19T14:55:52.190Z"
  ["authorizationCode"]=> string(4) "1213"
  ["paymentTypeCode"]=> string(2) "VN"
  ["responseCode"]=> int(0)
  ["installmentsNumber"]=> int(0)
  ["installmentsAmount"]=> NULL
}
```

```csharp
{
	AccountingDate: "1219",
	Amount: 10000,
	AuthorizationCode: "1213",
	BuyOrder: "327648047",
	CardDetail: (null),
	InstallmentsAmount: 1000,
	InstallmentsNumber: 10,
	PaymentTypeCode: "NC",
	ResponseCode: 0,
	SessionId: "750819",
	Status: "AUTHORIZED",
	TransactionDate: "2019-12-19T19:50:34.411Z"
}
```

```ruby
<Transbank::TransaccionCompleta::TransactionStatusResponse:0x00007f90f5c395f0 
    @vci=nil,
    @amount=1000,
    @status="AUTHORIZED",
    @buy_order="buyorder1567451528",
    @session_id="session1567451528",
    @card_number=nil,
    @accounting_date="0902",
    @transaction_date="2019-09-02T20:20:39.377Z",
    @authorization_code="1213",
    @payment_type_code="VN",
    @response_code=0, 
    @installments_number=0,
    @installments_amount=nil,
    @balance=nil>
```

```python
{
  vci: None, amount: 1000.0, status: "AUTHORIZED", buy_order: "buyorder1577202376",
  session_id: "session1577202376", card_detail: {'card_number': '6623'},
  accounting_date: 1224, transaction_date: "2019-12-24T15:46:22.392Z",
  authorization_code: 1213, payment_type_code: "VN", response_code: 0.0,
  installments_number: 0.0, installments_amount: None balance:None
}
```

```http
200 OK
Content-Type: application/json

{
  "vci": "TSY",
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
    "card_number": "1234"
  },
  "accounting_date": "0320",
  "transaction_date": "2019-03-20T20:18:20Z",
  "authorization_code": "877550",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
vci  <br> <i> String </i> | Código de respuesta de la autenticación bancaria. Largo máximo: 4
amount  <br> <i> Number </i> | Monto de la transacción. Sólo en caso de dolar acepta dos decimales. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
buy_order  <br> <i> String </i> | Número de orden de compra. Largo máximo: 26
session_id  <br> <i> String </i> | ID de sesión de la compra. Largo máximo: 61
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_detail.card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción. Largo máximo: 19
accounting_date  <br> <i> String </i> | Fecha contable de la transacción.
transaction_date  <br> <i> ISO8601 </i> | Fecha de la transacción.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción de pago. Largo máximo: 6
payment_type_code  <br> <i> String </i> | Indica el tipo de tarjeta utilizada.
response_code  <br> <i> Number </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br> 
installments_number <br> <i> Number </i> | Número de cuotas de la transacción. Largo máximo: 2
balance <br> <i> Number </i> | Monto restante. Largo máximo: 17. Este campo solo viene cuando la transacción fue anulada

### Reversar o Anular un pago Transacción Completa

Este método permite a todo comercio habilitado reversar o anular una transacción
completa. El método permite generar el reembolso del total o parte del monto de
una transacción dependiendo de la siguiente lógica de negocio la invocación a
esta operación generará una reversa o una anulación:
- Si el monto enviado es menor al monto total entonces se ejecutará una anulación parcial.
- Si el monto enviado es igual al total, entonces se evaluará una anulación o reversa. Será reversa si el tiempo para ejecutarla no ha terminado, de lo contrario se ejecutará una anulación.

La anulación puede realizarse máximo 90 días después de la fecha de la
transacción original.

Puedes [leer más sobre la anulación en la información del
producto Webpay](/producto/webpay#anulaciones) para conocer
más detalles y restricciones.

Para anular una transacción se debe invocar al método `Transaction.refund()`.


Permite generar el reembolso del total o parte del monto de una transacción completa.
Dependiendo de la siguiente lógica de negocio la invocación a esta operación generará una reversa o una anulación:

* Si se especifica un valor en el campo “amount” se ejecutará siempre una anulación.
* Si se supera el tiempo máximo para ejecutar una reversa se ejecutará una anulación.
* Si no se ha dado ninguno de los casos anteriores se ejecutará una reversa.

#### `Transaction.refund()`

Permite solicitar a Webpay la anulación de una transacción realizada previamente y que se encuentre vigente.

```java

```

```php
use Transbank\TransaccionCompleta\Transaction;

Transaction::refund($token_ws, $amount);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Refund(token, amount);
```

```ruby
Transbank::TransaccionCompleta::Transaction::refund(token: token, amount: amount)
```

```python
from transbank.transaccion_completa.transaction import Transaction

Transaction.refund(token=token, amount=amount)
```

```http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/refund
Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
 "amount": 1000
}
```
**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
amount  <br> <i> Decimal </i> | (Opcional) Monto que se desea anular de la transacción. Largo máximo: 17.

**Respuesta**

```java
import cl.transbank.webpay.exception.RefundTransactionException;
import cl.transbank.webpay.oneclick.OneclickMall;
import cl.transbank.webpay.oneclick.model.RefundOneclickMallTransactionResponse;

public class IntegrationExample {
    public static void main(String[] args) {
    String token = "a token obtained through create transaction";
    double amount = 10000; //example
    try {
            final FullTransactionRefundResponse response = FullTransaction.Transaction.refund(token,amount);
        } catch (IOException | TransactionRefundException e) {
            return new ErrorController().error();
        }
    }
}
```

```php
object(Transbank\TransaccionCompleta\TransactionRefundResponse)#305 (6) 
{
  ["type"]=> string(8) "REVERSED"
  ["authorizationCode"]=> NULL
  ["authorizationDate"]=> NULL
  ["nullifiedAmount"]=> NULL
  ["balance"]=> NULL
  ["responseCode"]=> NULL
}
```

```csharp
{
	AuthorizationCode: (null),
	AuthorizationDate: (null),
	Balance: 0,
	NullifiedAmount: 0,
	ResponseCode: 0,
	Type: "REVERSED"
}
```

```ruby
<Transbank::TransaccionCompleta::TransactionRefundResponse:0x00007f90f55f6468 
    @type="REVERSED",
    @authorization_code=nil,
    @authorization_date=nil,
    @nullified_amount=nil,
    @balance=nil,
    @response_code=nil>
```

```python
{
  type: "REVERSED", authorization_code: None, authorization_date: None, nullified_amount: None, balance: None, response_code: None
}
```

```http
200 OK
Content-Type: application/json
{
  "type": "NULLIFY",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20:18:20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSE. NULLIFY). Largo máximo: 10
authorization_code  <br> <i> String </i> | Código de autorización de la anulación. Largo máximo: 6. Solo viene en caso de anulación.
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización. Solo viene en caso de anulación.
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17. Solo viene en caso de anulación.
balance  <br> <i> Decimal </i> | Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17. Solo viene en caso de anulación.
response_code  <br> <i> Number </i> | Código de resultado de la anulación. Si es exitoso es 0, de lo contrario la anulación no fue realizada. Largo máximo: 2. Solo viene en caso de anulación.
## Transacción Mall Completa {data-submenuhidden=true}

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

Una transacción Mall Completa corresponde a una solicitud de transacciones completas
de un conjunto de pagos con tarjetas de crédito, en donde quién realiza el pago
ingresa al sitio del comercio, selecciona productos o
servicios, y el ingreso asociado a los datos de la tarjeta de crédito
lo realiza una única vez en forma segura en Webpay para el conjunto de pagos.
Cada pago tendrá su propio resultado, autorizado o rechazado.

![Desagregación de un pago Webpay Mall](/images/pago-webpay-mall.png)

El Mall Webpay agrupa múltiples tiendas, son estas últimas las que pueden
generar transacciones. Tanto el mall como las tiendas asociadas son
identificadas a través de un número denominado código de comercio.

#### Flujo Transacción Mall Completa

El flujo de Transacción Mall Completa es en general el mismo que el de [Transacción Completa](#webpay-transaccion-completa) tanto de cara al tarjeta habiente como de cara al integrador.

Las diferencias son:

- Se debe usar un código de comercio configurado para modalidad Mall en
  Transbank, el cual debe ser indicado al iniciar la transacción.
- Se pueden indicar múltiples transacciones, cada una asociada a un código de
  comercio de tienda (que debe estar configurada en Transbank como perteneciente
  al mall).
- Se debe verificar por separado el resultado de cada una de esas transacciones
  individualmente, pues es posible que el emisor de la tarjeta autorice algunas
  y otras no.


### Crear una Transacción Mall Completa

Para crear una Transacción Mall Completa basta llamar al método `Transaction.create()`

#### `Transaction.create()`

Permite inicializar una transacción mall completa en Webpay. Como respuesta a la
invocación se genera un token que representa en forma única una transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

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
POST /rswebpaytransaction/api/webpay/v1.0/transactions

Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234564",
  "card_number": "4239000000000000",
  "card_expiration_date": "22/10",
  "details": [
    {
      "amount": 10000,
      "commerce_code": "597055555552",
      "buy_order": "123456789"
    },
    {
      "amount": 10000,
      "commerce_code": "597055555553",
      "buy_order": "123456790"
    }
  ]
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Es el código único de la orden de compra generada por el comercio mall. Largo máximo: 26
session_id  <br> <i> String </i> |  Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
card_number  <br> <i> String </i> | Número de la tarjeta con la que se debe hacer la transacción. Largo máximo: 19
card_expiration_date  <br> <i> String </i> | Fecha de expiración de la tarjeta con la que se realiza la transacción. Largo máximo: 5
details  <br> <i> Array </i> | Lista de objetos, uno por cada tienda diferente del mall que participa en la transacción.
details [].amount  <br> <i> Decimal </i> | Monto de la transacción de una tienda del mall. Máximo 2 decimales para USD. Largo máximo: 17.
details [].commerce_code  <br> <i>String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo: 12.
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda del mall. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>. Largo máximo: 26

**Respuesta**

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
200 OK
Content-Type: application/json

{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token identificador de la transacción. Largo: 64.

### Consulta de cuotas

Para consultar el valor de las cuotas que pagará el tarjeta habiente en cada transacción dentro transacción mall completa, es necesario llamar al método `Transaction.installments()`

#### `Transaction.installments()`

Operación que permite obtener el monto de la cuota a partir del número de cuotas.
El id de la consulta que selecciona el tarjetahabiente debe ser informado en la
invocación de la confirmación.

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
POST /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/installments

Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "commerce_code": 597055555552,
  "buy_order": "123456789",
  "installments_number": 10
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
commerce_code  <br> <i> String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo: 12
buy_order  <br> <i> String </i> | Orden de compra de la tienda del mall. Largo máximo: 26 
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2

**Respuesta**

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
200 OK
Content-Type: application/json

{
  "installments_amount": 3334,
  "id_query_installments": 11,
  "deferred_periods": [
    {
      "amount": 1000,
      "period": 1
    }
  ]
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
installments_amount  <br> <i> String </i> | Monto de cada cuota. Largo: 17.
id_query_installments  <br> <i> String </i> | Identificador de las cuotas. Largo: 19.
deferred_periods  <br> <i> Array </i> | Arreglo con periodos diferidos.
deferred_periods [].amount  <br> <i> String </i> | Monto. Largo: 17.
deferred_periods [].period  <br> <i> String </i> | Índice de periodo. Largo: 2.

### Confirmación de la transacción

Una vez iniciada la transacción y consultado el monto de las cuotas por cada subtransacción, puedes confirmar y obtener el resultado de una transacción completa usando el metodo `Transaction.commit()`.

#### `Transaction.commit()`

Operación que permite confirmar una transacción. Retorna el estado de la
transacción.

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
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}

Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "details": [
    {
      "commerce_code": 597055555552,
      "buy_order": "ordenCompra1234",
      "id_query_installments": 12,
      "deferred_period_index": 1,
      "grace_period": false
    }
  ]
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
details <br> <i> details </i> | Listado con las transacciones mall. 
commerce_code  <br> <i> String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo máximo: 12
buy_order  <br> <i> String </i> | Orden de compra de la tienda del mall. Largo máximo: 26
id_query_installments  <br> <i> Number </i> | (Opcional) Identificador de cuota. Largo máximo: 19. Solo enviar si el pago es en cuotas
deferred_period_index  <br> <i> Number </i> | (Opcional) Cantidad de periodo diferido. Largo máximo: 2. Solo enviar si el pago es en cuotas
grace_period  <br> <i> Boolean </i> | (Opcional) Indicador de periodo de gracia. Solo enviar si el pago es en cuotas

**Respuesta**

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
200 OK
Content-Type: application/json

{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15:43:48.523Z",
  "details": [
    {
      "amount": 500,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555552",
      "buy_order": "505479072"
    }
  ]
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra del mall. Largo máximo: 26
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accounting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMYY
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
details  <br> <i> Array </i> | Lista con resultado de cada una de las transacciones enviadas.
details [].amount  <br> <i> Number </i> | Monto de la transacción. Largo máximo: 17
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
details [].payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés.
details [].responseCode  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br> 
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 6
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 255
status <br> <i> Text </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 17

### Consultar estado de una transacción mall completa

Esta operación permite obtener el estado de la transacción mall completa en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

#### `Transaction.status()`

Obtiene resultado de transacción a partir de un token.


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
GET /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)

**Respuesta**

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
200 OK
Content-Type: application/json

{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15:43:48.523Z",
  "details": [
    {
      "amount": 500,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555552",
      "buy_order": "505479072"
    }
  ]
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra del mall. Largo máximo: 26
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accouting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
details  <br> <i> Array </i> | Lista con resultado de cada una de las transacciones enviadas.
details [].amount  <br> <i> Number </i> | Monto de la transacción. Largo máximo: 17
details [].status  <br> <i> String </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 64
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
details [].payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés.
details [].responseCode  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada <br> -1 = Rechazo de transacción - Reintente <i>(Posible error en el ingreso de datos de la transacción)</i> <br> -2 = Rechazo de transacción <i>(Se produjo fallo al procesar la transacción. Este mensaje de rechazo está relacionado a parámetros de la tarjeta y/o su cuenta asociada)</i> <br> -3 = Error en transacción <i>(Interno Transbank)</i> <br> -4 = Rechazo emisor <i>(Rechazada por parte del emisor)</i><br> -5 = Rechazo - Posible Fraude <i>(Transacción con riesgo de posible fraude)</i> <br> 
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 6
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 255
status <br> <i> Text </i> | Estado de la transacción (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Largo máximo: 17
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 64

### Anulación Transacción Completa

Permite generar el reembolso del total o parte del monto de una transacción completa.
Dependiendo de la siguiente lógica de negocio la invocación a esta operación generará una reversa o una anulación:

- Si se especifica un valor en el campo “amount” se ejecutará siempre una anulación.
- Si se supera el tiempo máximo para ejecutar una reversa se ejecutará una anulación.
- Si no se ha dado ninguno de los casos anteriores se ejecutará una reversa.

#### `Transaction.refund()`

Permite solicitar a Webpay la anulación de una transacción realizada previamente y que se encuentre vigente.

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
POST /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/refunds
Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "buy_order": "415034240",
  "commerce_code": "597055555552",
  "amount": 1000
}
```
**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
commerce_code  <br> <i> Number </i> | Tienda hija que realizó la transacción. Largo: 12.
amount  <br> <i> Number </i> | Monto a anular. Largo máximo: 17


**Respuesta**

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
200 OK
Content-Type: application/json
{
  "type": "NULLIFY",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20:18:20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSE o NULLIFY). Si es REVERSE no se devolverán datos de la transacción. Largo máximo: 10
authorization_code  <br> <i> String </i> | Código de autorización de la anulación. Largo máximo: 6. Solo viene en caso de anulación.
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización. Solo viene en caso de anulación.
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17. Solo viene en caso de anulación.
balance  <br> <i> Decimal </i> | Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17. Solo viene en caso de anulación.
response_code <br> <i> Number </i> | Código del resultado del pago. Si es exitoso es 0, de lo contrario el pago no fue realizado. Largo máximo: 2. Solo viene en caso de anulación.


## Captura Diferida

Los comercios que están configurados para operar con captura diferida deben ejecutar el método de captura para realizar el cargo el cargo al tarjetahabiente. 

**Válido para :** 
- Webpay Plus Captura Diferida
- Transacción Completa Captura Diferida

### Ejecutar captura diferida

Este método permite a todo comercio habilitado realizar capturas de una
transacción autorizada sin captura generada en Webpay Plus o Transacción Completa.
El método contempla una única captura por cada autorización. Para ello se
deberá indicar los datos asociados a la transacción de venta con autorización
sin captura y el monto requerido para capturar el cual debe ser menor o igual al
monto originalmente autorizado.

Para capturar una transacción, ésta debe haber sido creada (según lo visto
anteriormente para Webpay Plus o Webpay Plus Mall) por un código de
comercio configurado para captura diferida. De esa forma la transacción estará
autorizada pero requerirá una captura explícita posterior para confirmar la
transacción.

Puedes [leer más sobre la captura en la información del
producto Webpay](/producto/webpay#autorizacion-y-captura)
para conocer más detalles y restricciones.

Para realizar esa captura explícita debe usarse el método `Transaction.capture()`

#### `Transaction.capture()`

Permite solicitar a Webpay la captura diferida de una transacción con
autorización y sin captura simultánea.

> Los SDKs permiten indicar opcionalmente el código de comercio de la
> transacción a capturar, para soportar la captura en comercios Webpay Plus
> Mall o Transacción Completa Mall. En comercios sin modalidad Mall no es necesario especificar el código
> de comercio, ya que se usa el indicado en la configuración.

<aside class="notice">
El método `Transaction.capture()` debe ser invocado siempre indicando el código del
comercio que realizó la transacción. En el caso de comercios modalidad Mall,
el código debe ser el código de la tienda virtual específica.
</aside>

```java
import cl.transbank.webpay.exception.CaptureTransactionException;
import cl.transbank.webpay.webpayplus.WebpayPlus;
import cl.transbank.webpay.webpayplus.model.CaptureWebpayPlusTransactionResponse;
import java.util.Random;
public class IntegrationExample {
    public static void main(String[] args) {
        String token = "ef6bf196cae9d38744974e4da61e004135ea2d8774a063d33d2c58ea5730a0d2";
        String buyOrder = String.valueOf(new Random().nextInt(Integer.MAX_VALUE));
        String authorizationCode = "some_valid_authorization_code";
        double amount = 1000;
        try {
            final CaptureWebpayPlusTransactionResponse response = WebpayPlus.DeferredTransaction.capture(token, buyOrder, authorizationCode, amount);
            final String authorizationCodeResp = response.getAuthorizationCode();
            final String authorizationDate = response.getAuthorizationDate();
            final double capturedAmount = response.getCapturedAmount();
            final byte responseCode = response.getResponseCode();
        } catch (CaptureTransactionException e) {
            e.printStackTrace();
        }
    }
}
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
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/capture
Tbk-Api-Key-Id: 597055555531 
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "commerce_code": "597055555531", 
  "buy_order": "415034240",
  "authorization_code": "12345",
  "capture_amount": 1000
}
```
**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64. (Se envía en la URL, no en el body)
commerce_code  <br> <i> Number </i> | (Opcional, solo usar en caso Mall) Tienda hija que realizó la transacción. Largo: 12.
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere capturar Largo máximo: 6.
capture_amount  <br> <i> Decimal </i> | Monto que se desea capturar. Largo máximo: 17.

**Respuesta**

```java

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
200 OK
Content-Type: application/json
{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20:18:20Z",
  "captured_amount": 1000,
  "response_code": 0
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo máximo: 64
authorization_code  <br> <i> String </i> | Código de autorización de la captura diferida. Largo máximo: 6
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización.
captured_amount  <br> <i> Decimal </i> | Monto capturado. Largo máximo: 6
response_code  <br> <i> Number </i> | Código de resultado de la captura. Si es exitoso es 0,de lo contrario la captura no fue realizada. Largo máximo: 2

En caso de error pueden aparecer los siguientes códigos exclusivos del método
`Transaction.capture()`:

Código | Descripción
------ | -----------
304 | Validación de campos de entrada nulos
245 | Código de comercio no existe
22 | El comercio no se encuentra activo
316 | El comercio indicado no corresponde al certificado o no es hijo del comercio Mall en caso de transacciones MALL
308 | Operación no permitida
274 | Transacción no encontrada
16 | La transacción no es de captura diferida
292 | La transacción no está autorizada
284 | Periodo de captura excedido
310 | Transacción reversada previamente
309 | Transacción capturada previamente
311 | Monto a capturar excede el monto autorizado
315 | Error del autorizador


## Códigos y mensajes de error

Al realizar cualquier solicitud al API REST, además de los datos de respuesta, se incluirá uno de los siguientes códigos de estado de respuesta HTTP dependiendo del resultado obtenido:  

### Solicitud exitosa 
Cuando la operación solcitada es ejecutada correctamente, se pueden recibir estos status HTTP:

Código de estado HTTP | Descripción
------ | -----------
200 | La operación se ha ejecutado exitosamente
204 | La operación DELETE se ha ejecutado exitosamente

#### Códigos de error
Todos los errores reportados por la API REST de Webpay despliegan un mensaje JSON con una descripción del error.

```json
{
  "error_message": "token is required"
}
```

Código de estado HTTP | Descripción
------ | -----------
400 | El mensaje JSON es inválido. Puedes ser que no corresponda a un mensaje bien estructurado o que contenga un campo no esperado. 
401 | No autorizado. API Key y/o API Secret inválidos 
404 | La transacción no ha sido encontrada.
405 | Método no permitido. 
406 | No fue posible procesar la respuesta en el formato que el cliente indica. 
415 | Tipo de mensaje no permitido. 
422 | El requerimiento no ha podido ser procesado ya sea por validaciones de datos o por lógica de negocios. 
500 | Ha ocurrido un error inesperado. 
