# PatPass Rest

## Ambientes y Credenciales

La API REST de PatPass by Webpay está protegida para garantizar que solamente comercios autorizados por Transbank hagan uso de las operaciones disponibles. La seguridad esta implementada mediante los siguientes mecanismos:

* Canal seguro a través de TLSv1.2 para la comunicación del cliente con Webpay.
* Autenticación y autorización mediante el intercambio de headers Tbk-Api-Key-Id y Tbk-Api-Key-Secret.

### Ambiente de Producción

Las URLs de endpoints de producción están alojados dentro de
<https://webpay3g.transbank.cl/>.

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
Host: https://webpay3g.transbank.cl
```

### Ambiente de Integración

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
Host: https://webpay3gint.transbank.cl
```

Las URLs de endpoints de integración están alojados dentro de
<https://webpay3gint.transbank.cl/>.

Consulta [la documentación para conocer las tarjetas de prueba que funcionan en
el ambiente de integración](/documentacion/como_empezar#ambientes).

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
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
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
comercio](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/tree/master/integracion).

> Los SDKs ya incluyen esos códigos de comercio y llaves secretas
> que funcionan en el ambiente de integración, por lo que puedes obtener
> rápidamente una configuración lista para hacer tus primeras pruebas en dicho
> ambiente:

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

## PatPass by Webpay Normal

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

Una transacción de autorización de PatPass by WebPay corresponde a una solicitud de inscripción de pago recurrente con tarjetas de crédito, en donde el primer pago se resuelve al instante, y los subsiguientes quedan programados para ser ejecutados mes a mes. PatPass by WebPay cuenta con fecha de caducidad o termino, la cual debe ser proporcionada junto a otros datos para esta transacción. La transacción puede ser realizada en Dólares y Pesos, para este último caso es posible enviar el monto en UF y WebPay realizará la conversión a pesos al momento de realizar el cargo al tarjetahabiente.

El flujo de pago en PatPass by WebPay se inicia desde el comercio, en donde el Tarjetahabiente selecciona el producto o servicio. Una vez realizado esto, elige pagar con PatPass by WebPay, en donde se despliega el formulario de pago y se completan los datos requeridos, dando paso al proceso de autenticación del Tarjetahabiente con el objetivo de validar que la tarjeta este siendo utilizada por el titular. Una vez resuelta la autenticación se procede a autorizar el pago y si esta es aprobada, se emite un comprobante electrónico de pago y el sistema procede a generar la inscripción encargada de la recurrencia mensual.

Dentro de los atributos más relevantes se pueden mencionar:

- Permite realizar transacciones seguras y en línea a través de Internet.
- En transacciones con PatPass by Webpay se solicita al tarjetahabiente autenticarse con su emisor, protegiendo de esta forma al comercio por eventuales fraudes o desconocimientos de compra.
- La seguridad es reforzada por medio de la utilización de servidores seguros, protegidos con TLS 1.2

### Flujo en caso de éxito

De cara al tarjetahabiente, el flujo de páginas para la transacción es el siguiente:

![Flujo de páginas PatPass by Webpay Normal](/images/flujo-paginas-webpay.png)

Desde el punto de vista técnico, la secuencia es la siguiente:

![Diagrama de secuencia PatPass by Webpay Normal](/images/diagrama-secuencia-webpay.png)

1. Una vez seleccionado los bienes o servicios, tarjetahabiente decide pagar a través de PatPass by WebPay.
2. El comercio inicia una transacción en Webpay, invocando el método `initTransaction()`.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el token de la transacción y URL de redireccionamiento a la cual se deberá redirigir al tarjetahabiente.
4. Comercio redirecciona al tarjetahabiente hacia Webpay, con el token de la transacción a la URL indicada en punto 3. La redirección se realiza enviando por método POST el token en variable `token_ws`.
5. El navegador Web del tarjetahabiente realiza una petición HTTPS a Webpay, en base al redireccionamiento generado por el comercio en el punto 4.
6. Webpay responde al requerimiento desplegando el formulario de pago de Webpay. Desde este punto la comunicación es entre Webpay y el tarjetahabiente, sin interferir el comercio. El formulario de pago de PatPass by Webpay despliega, entre otras cosas, el monto de la transacción, fecha de término de suscripción, información del comercio como nombre y logotipo, y la opción de pago a través de crédito.
7. Tarjetahabiente ingresa los datos de la tarjeta, hace clic en pagar.
8. WebPay procesa la solicitud de autorización (primero autenticación bancaria y luego la autorización de la transacción), y si todo resulta exitoso, realiza el proceso de inscripción de PatPass by WebPay.
9. Una vez resuelta la autorización, WebPay retorna el control al comercio, realizando un redireccionamiento HTTPS hacia la página de transición del comercio, en donde se envía por método POST el token de la transacción en la variable `token_ ws`. El comercio debe implementar la recepción de esta variable.
10. El navegador Web del tarjetahabiente realiza una petición HTTPS al sitio del comercio, en base a la redirección generada por WebPay en el punto 9.
11. El sitio del comercio recibe la variable `token_ws` e invoca el segundo
    método Web para confirmar y obtener el resultado de la inscripción. El resultado de la autorización podrá ser consultado posteriormente con la 
    variable anteriormente mencionada.
12. Comercio recibe el resultado de la confirmación.

<aside class="warning">
En la versión anterior de WebPay, había que invocar `acknowledgeTransaction()` 
para informar a WebPay que se había recibido el resultado la transacción sin
problemas. Ahora no es necesario, ya que ésto se realiza de forma automática
una vez que se confirma la transacción.  Además ya no se debe mostrar el voucher
de Transbank, solo debe mostrarse desde el sitio del comercio.
</aside>

13. Sitio del comercio despliega voucher con los datos de la inscripción.

### Flujo si usuario aborta el pago

![Diagrama de secuencia si usuario aborta el
pago](/images/diagrama-secuencia-webpay-abortar.png)

Si el tarjetahabiente anula la transacción en el formulario de pago de Webpay, el flujo cambia y los pasos son los siguientes:

1. Una vez seleccionado los bienes o servicios, tarjetahabiente decide pagar a través de PatPass by WebPay.
2. El comercio inicia una transacción en Webpay, invocando el método `initTransaction()`.
3. Webpay procesa el requerimiento y entrega como resultado de la operación el token de la transacción y URL de redireccionamiento a la cual se deberá redirigir al tarjetahabiente.
4. Comercio redirecciona al tarjetahabiente hacia Webpay, con el token de la transacción a la URL indicada en punto 3. La redirección se realiza enviando por método POST el token en variable `token_ws`.
5. El navegador Web del tarjetahabiente realiza una petición HTTPS a Webpay, en base al redireccionamiento generado por el comercio en el punto 4.
6. Webpay responde al requerimiento desplegando el formulario de pago de Webpay. Desde este punto la comunicación es entre Webpay y el tarjetahabiente, sin interferir el comercio. El formulario de pago de PatPass by Webpay despliega, entre otras cosas, el monto de la transacción, fecha de término de suscripción, información del comercio como nombre y logotipo, y la opción de pago a través de crédito.
7. Tarjetahabiente hace clic en anular, en formulario PatPass by WebPay.
8. Webpay retorna el control al comercio, realizando un redireccionamiento HTTPS hacia la página de **retorno del comercio**, en donde se envía por método POST el token de la transacción en la variable `TBK_TOKEN` además de las variables `TBK_ORDEN_COMPRA` y `TBK_ID_SESION`.

<aside class="warning">
Nota que el nombre de las variables recibidas es diferente. En lugar de `token_ws` acá el token viene en la variable `TBK_TOKEN`.
</aside>

9. El comercio con la variable `TBK_TOKEN` debe invocar el método
   `getTransactionResult()`, para obtener el resultado de la autorización. En
   este caso debe obtener una excepción, pues el pago fue abortado.
10. El comercio debe informar al tarjetahabiente que su pago no se completó.

### Crear una transacción PatPass by Webpay Normal

Para crear una transacción basta llamar al método `initTransaction()`

#### `initTransaction()`

Permite inicializar una transacción en PatPass by Webpay. Como respuesta a la invocación se genera un token que representa en forma única una transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el token es caducado y no podrá ser utilizado en un pago.
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

{
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234557545",
  "amount": 10000,
  "return_url": "http://www.comercio.cl/webpay/retorno",
  "wpm_detail": {
    "service_id": "123345567",
    "card_holder_id": "12345",
    "card_holder_name": "Juan",
    "card_holder_last_name1": "Perez",
    "card_holder_last_name2": "Gonzalez",
    "card_holder_mail": "juan.perez@gmail.com",
    "cellphone_number": "9912345678",
    "expiration_date": "2019-03-20T20:18:20Z",
    "commerce_mail": "contacto@comercio.cl",
    "uf_flag": false
  } 
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buyOrder  <br> <i> String </i> | Orden de compra de la tienda. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code>. Largo máximo: 26
sessionId <br><i> String </i> | (Opcional) Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
amount <br><i> Number </i> | Monto de la transacción. Máximo 2 decimales para USD y UF. Largo máximo: 17
returnURL <br><i> String </i> | URL del comercio, a la cual Webpay redireccionará posterior al proceso de autorización. Largo máximo: 255
wpmDetail <br><i> wpmDetail </i> | Objeto que contiene datos asociados a la inscripción de PatPass by WebPay.
wpmDetail.serviceId <br><i> String </i> | Corresponde al identificador de servicio con que el comercio identifica a su cliente. Largo máximo: 255
wpmDetail.cardHolderId <br><i> String </i> | RUT del tarjetahabiente. Largo máximo: 255. Formato: NNNNNNNNA
wpmDetail.cardHolderName <br><i> String </i> | Nombre tarjetahabiente. Largo máximo: 255
wpmDetail.cardHolderLastName1 <br><i> String </i> | Apellido paterno tarjetahabiente. Largo máximo: 255
wpmDetail.cardHolderLastName2 <br><i> String </i> | Apellido materno tarjetahabiente. Largo máximo: 255
wpmDetail.cardHolderMail <br><i> String </i> | Correo electrónico tarjetahabiente. Largo máximo: 255
wpmDetail.cellPhoneNumber <br><i> String </i> | Número teléfono celular tarjetahabiente. Largo máximo: 255
wpmDetail.expirationDate <br><i> DateTime </i> | Fecha expiración de PatPass by WebPay, corresponde al último pago. Largo: 10. Formato AAAA-MM-DD
wpmDetail.commerceMail <br><i> String </i> | Correo electrónico comercio. Largo máximo: 50. Los SDKs se encargan automáticamente de este parámetro a partir del email de comercio ingresado en la configuración usada para iniciar la transacción
wpmDetail.ufFlag <br><i> Boolean </i> | Valor en true indica que el monto enviado está expresado en UF, valor en false indica que valor esta expresado en Pesos o dólar según corresponda

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
 "token": "e9d555262db0f989e49d724b4db0b0af367cc415cde41f500a776550fc5fddd3",
 "url": "https://webpay3gint.transbank.cl/webpayserver/initTransaction"
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.
url  <br> <i> String </i> | URL de formulario de pago PatPass by Webpay. Largo máximo: 256.

### Confirmar una transacción PatPass by Webpay Normal

Cuando el comercio retoma el control mediante `returnURL` puedes confirmar una transacción usando los métodos  `getTransactionResult()` y `acknowledgeTransaction()`

#### `getTransactionResult()`

Permite obtener el resultado de la transacción una vez que Webpay ha resuelto su autorización financiera.

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
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.

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
VCI  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor TSY (Autenticación exitosa), TSN (Autenticación fallida), TO (Tiempo máximo excedido para autenticación), ABO (Autenticación abortada por tarjetahabiente), U3 (Error interno en la autenticación), NP (No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure), ACS2 (Autenticación fallida extranjera). Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio). Largo máximo: 64
amount  <br> <i> Number </i> | Monto de la transacción. Formato número entero para transacciones en peso y decimal para transacciones en dólares y UF máximo 2 decimales. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 64
buyOrder  <br> <i> String </i> | Orden de compra de la tienda indicado en `initTransaction()`. Largo máximo: 26
sessionId  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `initTransaction()`. Largo máximo: 61.
cardDetails  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
cardDetails.cardNumber  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accountingDate  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transactionDate  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
authorizationCode  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
paymentTypeCode   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
responseCode  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada.<br> -1 = Rechazo de transacción.<br> -2 =  Transacción debe reintentarse. <br> -3 = Error en transacción. <br> -4 = Rechazo de transacción.<br> -5 = Rechazo por error de tasa. <br> -6 = Excede cupo máximo mensual. <br> -7 = Excede límite diario por transacción. <br> -8 = Rubro no autorizado. <br> -100 Rechazo por inscripción de PatPass by Webpay.
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
