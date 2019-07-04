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

```java
// Host: https://webpay3gint.transbank.cl
```

```php
// Host: https://webpay3gint.transbank.cl
```

```csharp
// Host: https://webpay3gint.transbank.cl
```

```ruby
# Host: https://webpay3gint.transbank.cl
```

```python
# Host: https://webpay3gint.transbank.cl
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
// Tbk-Api-Key-Id: Próximamente...
// Tbk-Api-Key-Secret: Próximamente...
// Content-Type: application/json
```

```php
// Tbk-Api-Key-Id: Próximamente...
// Tbk-Api-Key-Secret: Próximamente...
// Content-Type: application/json
```

```csharp
// Tbk-Api-Key-Id: Próximamente...
// Tbk-Api-Key-Secret: Próximamente...
// Content-Type: application/json
```

```ruby
# Tbk-Api-Key-Id: Próximamente...
# Tbk-Api-Key-Secret: Próximamente...
# Content-Type: application/json
```

```python
# Tbk-Api-Key-Id: Próximamente...
# Tbk-Api-Key-Secret: Próximamente...
# Content-Type: application/json
```

```http
Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
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
Webpay Plus | `Próximamente...` | `Próximamente...`
Webpay Plus Mall | `Próximamente...` | `Próximamente...`
Webpay Oneclick | `Próximamente...` | `Próximamente...`
Webpay Oneclick Mall | `Próximamente...` | `Próximamente...`
Webpay Transacción Completa | `Próximamente...` | `Próximamente...`
Webpay Transacción Completa Diferida | `Próximamente...` | `Próximamente...`

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

## Webpay Plus

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

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
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
session_id  <br> <i> String </i> | (Opcional) Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
amount  <br> <i> Decimal </i> | Monto de la transacción. Máximo 2 decimales para USD. Largo máximo: 17
return_url  <br> <i> String </i> | URL del comercio, a la cual Webpay redireccionará posterior al proceso de autorización. Largo máximo: 256

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
url  <br> <i> String </i> | URL de formulario de pago Webpay. Largo máximo: 255.

### Confirmar una transacción Webpay Plus

Cuando el comercio retoma el control mediante `return_url` debes confirmar y obtener
el resultado de una transacción usando el método  `Transaction.commit()`.

#### `Transaction.commit()`

Permite confirmar y obtener el resultado de la transacción una vez que Webpay ha resuelto su autorización financiera.


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
Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
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
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor TSY (Autenticación exitosa), TSN (Autenticación fallida), TO (Tiempo máximo excedido para autenticación), ABO (Autenticación abortada por tarjetahabiente), U3 (Error interno en la autenticación), NP (No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure), ACS2 (Autenticación fallida extranjera). Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
amount  <br> <i> Decimal </i> | Formato número entero para transacciones en peso y decimal para transacciones en dólares. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 64
buy_order  <br> <i> String </i> | Orden de compra de la tienda indicado en `Transaction.create()`. Largo máximo: 26
session_id  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `Transaction.create()`. Largo máximo: 61.
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accounting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada.<br> -1 = Rechazo de transacción.<br> -2 =  Transacción debe reintentarse. <br> -3 = Error en transacción. <br> -4 = Rechazo de transacción.<br> -5 = Rechazo por error de tasa. <br> -6 = Excede cupo máximo mensual. <br> -7 = Excede límite diario por transacción. <br> -8 = Rubro no autorizado.
installments_amount <br> <i> Number </i> | Monto de las cuotas. Largo máximo: 17
installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
balance  <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Obtener estado de una transacción Webpay Plus

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

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
Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
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
vci  <br> <i> String </i> | Resultado de la autenticación del tarjetahabiente. Puede tomar el valor TSY (Autenticación exitosa), TSN (Autenticación fallida), TO (Tiempo máximo excedido para autenticación), ABO (Autenticación abortada por tarjetahabiente), U3 (Error interno en la autenticación), NP (No Participa, probablemente por ser una tarjeta extranjera que no participa en el programa 3DSecure), ACS2 (Autenticación fallida extranjera). Puede ser vacío si la transacción no se autenticó. Largo máximo: 3. Este campo es información adicional suplementaria al `responseCode` pero el comercio **no** debe validar este campo. Porque constantemente se agregan nuevos mecanismos de autenticación que se traducen en nuevos valores para este campo que no están necesariamente documentados. (En el caso de tarjetas internacionales que no proveen 3D-Secure, la decisión del comercio de aceptarlas o no se realiza a nivel de configuración del comercio en Transbank y debe ser conversada con el ejecutivo del comercio)
amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 64
buy_order  <br> <i> String </i> | Orden de compra de la tienda indicado en `Transaction.create()`. Largo máximo: 26
session_id  <br> <i> String </i> | Identificador de sesión, el mismo enviado originalmente por el comercio en `Transaction.create()`. Largo máximo: 61.
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente. Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accounting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada.<br> -1 = Rechazo de transacción.<br> -2 =  Transacción debe reintentarse. <br> -3 = Error en transacción. <br> -4 = Rechazo de transacción.<br> -5 = Rechazo por error de tasa. <br> -6 = Excede cupo máximo mensual. <br> -7 = Excede límite diario por transacción. <br> -8 = Rubro no autorizado.
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
token  <br> <i> String </i> | Token de la transacción. Largo: 64.
amount  <br> <i> Decimal </i> | (Opcional) Monto que se desea anular de la transacción. Largo máximo: 10.

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
type  <br> <i> String </i> | Tipo de reembolso (REVERSE. NULLIFY). Largo máximo: 10
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
         "commerce_code": "Próximamente...",
         "buy_order": "ordenCompraDetalle1234"
     },
     {     
        "amount": 12000,
        "commerce_code": "Próximamente...",
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
url  <br> <i> String </i> | URL de formulario de pago Webpay. Largo máximo: 256.

### Confirmar una transacción Webpay Plus Mall

Para confirmar una transacción y obtener el resultado, se debe usar el método `Transaction.commit()`

#### `Transaction.commit()`

Permite confirmar una tranascción y obtener el resultado de la transacción
una vez que Webpay ha resueltosu autorización financiera.

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
Tbk-Api-Key-Id: 597055555535
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
details [].response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada.<br> -1 = Rechazo de transacción.<br> -2 =  Transacción debe reintentarse. <br> -3 = Error en transacción. <br> -4 = Rechazo de transacción.<br> -5 = Rechazo por error de tasa. <br> -6 = Excede cupo máximo mensual. <br> -7 = Excede límite diario por transacción. <br> -8 = Rubro no autorizado.
details [].amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 10
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 12
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 26
details [].status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 26
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Obtener estado de una transacción Webpay Plus Mall

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

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
Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
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
details [].response_code  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada.<br> -1 = Rechazo de transacción.<br> -2 =  Transacción debe reintentarse. <br> -3 = Error en transacción. <br> -4 = Rechazo de transacción.<br> -5 = Rechazo por error de tasa. <br> -6 = Excede cupo máximo mensual. <br> -7 = Excede límite diario por transacción. <br> -8 = Rubro no autorizado.
details [].amount  <br> <i> Formato número entero para transacciones en peso y decimal para transacciones en dólares. </i> | Monto de la transacción. Largo máximo: 10
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 12
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 26
details [].status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 26
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
Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application/json

{
  "commerce_code": "Próximamente...",
  "buy_order": "ordenCompra12345678",
  "authorization_code": "123456",
  "capture_mount": 1000
}
```
**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere anular. Si la transacción es de captura diferida, se debe usar el código obtenido al llamar a `Transaction.capture()`. Largo máximo: 6.
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere anular. Largo máximo: 26.
amount  <br> <i> Decimal </i> | (Opcional) Monto que se desea anular de la transacción. Largo máximo: 10.
commerce_id  <br> <i> Number </i> | (Opcional) Tienda mall que realizó la transacción. Largo: 12.

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

## Webpay Plus Captura Diferida

### Ejecutar captura diferida Webpay Plus

Este método permite a todo comercio habilitado realizar capturas de una
transacción autorizada sin captura generada en Webpay Plus.
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
> Mall. En comercios Webpay Plus, no es necesario especificar el código
> de comercio pues se usa el indicado en la configuración.

<aside class="notice">
El método `Transaction.capture()` debe ser invocado siempre indicando el código del
comercio que realizó la transacción. En el caso de comercios Webpay Plus Mall,
el código debe ser el código de la tienda virtual específica.
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
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/capture
Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "commerce_code": "Próximamente...",
  "buy_order": "415034240",
  "authorization_code": "12345",
  "capture_amount": 1000
}
```
**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.
commerce_code  <br> <i> Number </i> | (Opcional, solo usar en caso Mall) Tienda hija que realizó la transacción. Largo: 6.
buy_order  <br> <i> String </i> | Orden de compra de la transacción que se requiere capturar. Largo máximo: 26.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción que se requiere capturar Largo máximo: 6.
capture_amount  <br> <i> Decimal </i> | Monto que se desea capturar. Largo máximo: 17.

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
316 | El comercio indicado no corresponde al certificado o no es hijo del comercio MALL en caso de transacciones MALL
308 | Operación no permitida
274 | Transacción no encontrada
16 | La transacción no es de captura diferida
292 | La transacción no está autorizada
284 | Periodo de captura excedido
310 | Transacción reversada previamente
309 | Transacción capturada previamente
311 | Monto a capturar excede el monto autorizado
315 | Error del autorizador

## Webpay OneClick Normal

La modalidad de pago OneClick permite al tarjetahabiente realizar pagos en el
comercio sin la necesidad de ingresar cada vez información de la tarjeta de
crédito al momento de realizar la compra. El modelo de pago contempla un
proceso previo de inscripción o enrolamiento del tarjetahabiente, a través del
comercio, que desee utilizar el servicio. Este tipo de pago facilita la venta,
disminuye el tiempo de la transacción y reduce los riesgos de ingreso erróneo
de los datos del medio de pago.

El proceso de integración con Webpay OneClick consiste en desarrollar por parte
del comercio las llamadas a los servicios web dispuestos por Transbank para la
inscripción de los tarjetahabientes, así como para la realización de los
pagos.

#### Flujo de inscripción y pago

La inscripción es el proceso en el cual el tarjetahabiente registra los datos
de su tarjeta en Webpay OneClick para usarlo en compras futuras. Estos datos son
almacenados de forma segura en Transbank, y nunca son conocidos por el comercio.
Este proceso debe ser iniciado por la tienda del comercio y es requisito que el
cliente esté autenticado en la página del comercio antes de iniciar la
inscripción.

Proceso:

<img class="td_img-night" src="/images/diagrama-secuencia-oneclick-inscripcion.png" alt="Diagrama de secuencia inscripción Oneclick">

- El cliente se conecta y autentica en la página del comercio, mediante su
  nombre de usuario y clave.
- El cliente selecciona la opción de inscripción, la cual debe estar explicada
  en la página del comercio.
- El comercio consume un servicio web publicado por Transbank, donde entrega los
  datos del cliente y la URL de término; obtiene un token y URL de Webpay.
- El comercio envía el browser del cliente a la URL obtenida y pasa por
  parámetro el token (método POST).
- Webpay presenta el formulario de inscripción, este es similar al formulario
  de pago actual de Webpay Plus, para que el cliente ingrese los datos de su
  tarjeta.
- El cliente será autenticado por su banco emisor, de forma similar al flujo
  normal de pago. En este punto se realiza una transacción de $1 peso, la cual
  no se captura (no se verá reflejada en su estado de cuenta).
- Finalizada la inscripción, Webpay envía el browser del cliente a la URL
  entregada por el comercio, pasando por parámetro el token.
- El comercio debe consumir otro servicio web de Transbank, con el token, para
  obtener el resultado de la inscripción y el identificador de usuario, que
  debe utilizar en el futuro para realizar los pagos.
- El comercio presenta al cliente el resultado de la inscripción.

Después de realizado el proceso de inscripción, el comercio puede iniciar el proceso de pago cuando corresponda.

El pago es el proceso donde el comercio solicita el cargo de una compra a la
tarjeta de crédito de un usuario inscrito anteriormente, usando el
identificador entregado por Transbank al momento de la inscripción.

Los pagos en esta modalidad no requieren necesariamente la intervención del
usuario.

El monto del pago debe estar dentro de los límites establecidos para este tipo
de transacciones, el proceso interno es similar a un cargo normal de Webpay.
Proceso:

<img class="td_img-night" src="/images/diagrama-secuencia-oneclick-pago.png" alt="Diagrama de secuencia inscripción Oneclick">

- El cliente se conecta y autentica en la página o aplicación del comercio
  mediante su nombre de usuario y clave.
- El cliente selecciona la opción de pagar con Webpay Oneclick.
- El comercio usa el servicio web de pago, publicado por Transbank, entregando
  el identificador de usuario (que se obtuvo en la inscripción), el monto del
  pago y la orden de compra. Obtiene la respuesta con el código de
  autorización.
- El comercio presenta el resultado del pago al cliente.

### Crear una inscripción Webpay OneClick

Para realizar el primero de los procesos descritos (la inscripción), debe llamarse al método `Inscription.start()`

#### `Inscription.start()`

Permite realizar la inscripción del tarjetahabiente e información de su
tarjeta de crédito. Retorna como respuesta un token que representa la
transacción de inscripción y una URL (`urlWebpay`), que corresponde a la URL
de inscripción de OneClick.

Una vez que se llama a este servicio Web, el usuario debe ser redireccionado
vía POST a `urlWebpay` con parámetro `TBK_TOKEN` igual al token obtenido.

<aside class="notice">
Nota que a diferencia de Webpay Plus, donde el parámetro se llama `token_ws`, en
Webpay OneClick el parámetro se llama `TBK_TOKEN`.
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
POST /rswebpaytransaction/api/oneclick/v1.0/inscriptions

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
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
username  <br> <i> String </i> | Identificador del usuario registrado en el comercio. Largo máximo: 256.
email  <br> <i> String </i> | Email del usuario registrado en el comercio. Largo máximo: 256.
response_url  <br> <i> String </i> | URL del comercio a la cual Webpay redireccionará posterior al proceso de inscripción. Largo máximo: 256.

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
  "token": "e128a9c24c0a3cbc09223973327b97c8c474f6b74be509196cce4caf162a016a",
  "url_webpay": "https://webpay3g.transbank.cl/webpayserver/bp_inscription.cgi"
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la inscripción.
url_webpay  <br> <i> String </i> | URL de formulario Webpay.

<aside class="notice">
Una vez que se llama a este webservice el usuario debe ser redireccionado vía
POST a `urlWebpay` con parámetro `TBK_TOKEN` igual al token.
</aside>

### Confirmar una inscripción Webpay OneClick

Una vez terminado el flujo de inscripción en Transbank el usuario es enviado a
la URL de fin de inscripción que definió el comercio. En ese instante el
comercio debe llamar a `Inscription.finish()`.

#### `Inscription.finish()`

Permite finalizar el proceso de inscripción del tarjetahabiente en OneClick.
Retorna el identificador del usuario en OneClick, el cual será utilizado para
realizar las transacciones de pago.

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
POST /rswebpaytransaction/api/oneclick/v1.0/inscriptions/{token}

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
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
token  <br> <i> String </i> | Token de la inscripción.

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
  "response_code": 0,
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "authorization_code": 123456,
  "credit_card_type": "Visa",
  "last_four_card_digits": "4092"
}

```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
response_code  <br> <i> Number </i> | Código de retorno del proceso de inscripción, donde 0 (cero) es aprobado.
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente en Webpay OneClick, que debe ser usado para realizar pagos o borrar la inscripción.
authorization_code  <br> <i> String </i> | Código que identifica la autorización de la inscripción.
credit_card_type  <br> <i> creditCardType </i> | Indica el tipo de tarjeta inscrita por el cliente (Visa, AmericanExpress, MasterCard, Diners, Magna).
last_four_card_digits  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta ingresada por el cliente en la inscripción.

### Eliminar una inscripción con Webpay Oneclick
Una vez finalizado el proceso de inscripción es posible eliminarla de ser necesario. Para esto debes usar el método llamado `Inscription.remove()`.

#### `Inscription.remove()`
Permite eliminar un usuario enrolado a Oneclick.

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
DELETE /rswebpaytransaction/api/oneclick/v1.0/inscriptions

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "username": "juanperez",
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente (devuelto por `Inscription.finish()`).
username  <br> <i> String </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `Inscription.start()`).

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
```

### Autorizar un pago con Webpay Oneclick

Una vez realizada la inscripción, el comercio puede usar el `tbkUser` recibido
para realizar transacciones. Para eso debe usar el método `Transaction.authorize()`.

#### `Transaction.authorize()`

Permite realizar transacciones de pago. Retorna el resultado de la
autorización. Este método debe ser ejecutado cada vez que el usuario
selecciona pagar con OneClick en el comercio.

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
POST /rswebpaytransaction/api/oneclick/v1.0/transactions

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "username": "juanperez",
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "amount": 1000,
  "buy_order": "ordenCompra123456789"
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
username  <br> <i> String </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `Inscription.start()`).
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente (devuelto por `Inscription.finish()`).
amount  <br> <i> Decimal </i> | Monto del pago en pesos.
buy_order  <br> <i> Number </i> | Identificador único de la compra generado por el comercio.

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
vci  <br> <i> String </i> | Código de respuesta de la autenticación bancaria
amount  <br> <i> Number </i> | Monto de la transacción. Sólo en caso de dolar acepta dos decimales.
status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED).
buy_order  <br> <i> String </i> | Número de orden de compra.
session_id  <br> <i> String </i> | ID de sesión de la compra.
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_detail.card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción.
accounting_date  <br> <i> String </i> | Fecha contable de la transacción.
transaction_date  <br> <i> ISO8601 </i> | Fecha de la transacción.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción de pago.
payment_type_code  <br> <i> String </i> | Indica el tipo de tarjeta utilizada.
response_code  <br> <i> Number </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. <br> -1, -2, -3, -4, -5, -6, -7, -8: Rechazo <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
installments_number <br> <i> Number </i> | Número de cuotas de la transacción.

### Consultar un pago realizado con Webpay OneClick

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

#### `Transaction.status()`

Permite consultar el estado d epago realizado a través de Webpay Oneclick.
Retorna el resultado de la autorización.

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
GET /rswebpaytransaction/api/oneclick/v1.0/transactions/{buyOrder}

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la transacción a  consultar.

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
vci  <br> <i> String </i> | Código de respuesta de la autenticación bancaria
amount  <br> <i> Number </i> | Monto de la transacción. Sólo en caso de dolar acepta dos decimales.
status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED).
buy_order  <br> <i> String </i> | Número de orden de compra.
session_id  <br> <i> String </i> | ID de sesión de la compra.
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_detail.card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción.
accounting_date  <br> <i> String </i> | Fecha contable de la transacción.
transaction_date  <br> <i> ISO8601 </i> | Fecha de la transacción.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción de pago.
payment_type_code  <br> <i> String </i> | Indica el tipo de tarjeta utilizada.
response_code  <br> <i> Number </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. <br> -1, -2, -3, -4, -5, -6, -7, -8: Rechazo <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
installments_number <br> <i> Number </i> | Número de cuotas de la transacción.
balance  <br> <i> Decimal </i> | Monto restante de la sub-transacción de pago original: monto inicial – monto anulado. Largo máximo: 17

### Reversar o Anular un pago Webpay OneClick

Este proceso permite reversar una venta cuando esta no pudo concretarse dentro del mismo día contable, o anularlo.

#### `Transaction.refund()`

Permite reversar o anular una transacción de venta autorizada con anterioridad.
Este método retorna como respuesta un identificador único de la transacción de reversa/anulación.

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
POST /rswebpaytransaction/api/oneclick/v1.0/transactions/{buyOrder}/refunds

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
 "amount": 1000
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la transacción a reversar o anular.
amount  <br> <i> Number </i> | (Opcional) Monto a anular. Si está presente se ejecuta una anulación, en caso contrario se ejecuta una reversa (a menos que haya pasado el tiempo máximo para reversar).

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
  "balance": 0.00
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSE. NULLIFY). Largo máximo: 10
authorization_code  <br> <i> Boolean </i> | Código de autorización. Largo máximo: 6
authorization_date  <br> <i> ISO8601 </i> | Fecha de la autorización de la transacción.
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17
balance  <br> <i> Decimal </i> | Monto restante. Largo máximo: 17

## Webpay OneClick Mall

#### Flujo de inscripción y pago

El flujo de Webpay OneClick Mall es en general el mismo que el de [Webpay
OneClick Normal](#webpay-oneclick-normal) tanto de cara al tarjeta habiente como
de cara al integrador.

Las diferencias son:

- El usuario debe estar registrado en la página del comercio "mall" agrupador,
  pero las transacciones son a nombre de las "tiendas" del mall.
- Se pueden indicar múltiples transacciones a autorizar en una misma operación.
- Se debe verificar por separado el resultado de cada una de esas transacciones
  individualmente, pues es posible que el emisor de la tarjeta autorice algunas
  y otras no.

### Crear una inscripción Webpay OneClick Mall

Para iniciar la inscripción debe usarse el método `Inscription.start()`

#### `Inscription.start()`

Permite gatillar el inicio del proceso de inscripción.

> Los SDKs no soportan aún los servicios Webpay OneClick Mall.

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
POST /rswebpaytransaction/api/oneclick/v1.0/inscriptions

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
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
username  <br> <i> String </i> | Identificador del usuario registrado en el comercio. Largo máximo: 256.
email  <br> <i> String </i> | Email del usuario registrado en el comercio. Largo máximo: 256.
response_url  <br> <i> String </i> | URL del comercio a la cual Webpay redireccionará posterior al proceso de inscripción. Largo máximo: 256.

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
  "token": "e128a9c24c0a3cbc09223973327b97c8c474f6b74be509196cce4caf162a016a",
  "url_webpay": "https://webpay3g.transbank.cl/webpayserver/bp_inscription.cgi"
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Identificador, único, del proceso de inscripción. Debe ser enviado por parámetro (`TBK_TOKEN`) a la URL `urlWebpay`.
url_webpay  <br> <i> String </i> | URL de Webpay para iniciar la inscripción.

<aside class="notice">
Una vez que se llama a este webservice el usuario debe ser redireccionado vía
POST a `urlInscriptionForm` con parámetro `TBK_TOKEN` igual al token.
</aside>

### Confirmar una inscripción Webpay OneClick Mall

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
POST /rswebpaytransaction/api/oneclick/v1.0/inscriptions/{token}

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
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
token  <br> <i> String </i> | Identificador del proceso de inscripción. Es entregado por Webpay en la respuesta del método `Inscription.start()`.

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
  "response_code": 0,
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "authorization_code": 123456,
  "credit_card_type": "Visa",
  "last_four_card_digits": "4092"
}

```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
response_code  <br> <i> Number </i> | Código de retorno del proceso de inscripción, donde 0 (cero) es aprobado.
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente en Webpay OneClick, que debe ser usado para realizar pagos o borrar la inscripción.
authorization_code  <br> <i> String </i> | Código que identifica la autorización de la inscripción.
credit_card_type  <br> <i> creditCardType </i> | Indica el tipo de tarjeta inscrita por el cliente (Visa, AmericanExpress, MasterCard, Diners, Magna).
last_four_card_digits  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta ingresada por el cliente en la inscripción.

### Eliminar una inscripción con Webpay Oneclick Mall
Una vez finalizado el proceso de inscripción es posible eliminarla de ser necesario. Para esto debes usar el método llamado `Inscription.remove()`.

#### `Inscription.remove()`
Permite eliminar un usuario enrolado a Oneclick Mall.

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
DELETE /rswebpaytransaction/api/oneclick/v1.0/inscriptions

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "username": "juanperez",
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente (devuelto por `Inscription.finish()`).
username  <br> <i> String </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `Inscription.start()`).

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
```

### Autorizar un pago con Webpay OneClick Mall

Una vez realizada la inscripción, el comercio puede usar el `tbkUser` recibido
para realizar transacciones. Para eso debes usar el método `Transaction.authorize()`.

#### `Transaction.authorize()`

Permite autorizar un pago.

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
POST /rswebpaytransaction/api/oneclick/v1.0/transactions

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "username": "juanperez",
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "buy_order": "ordenCompra123456789",
  "details": [
    {
      "commerce_code": Próximamente...,
      "buy_order": "ordenCompra123445",
      "amount": 1000,
      "installments_number": 5
  }]
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
username  <br> <i> String </i> | Identificador del usuario en los sistemas del comercio (el mismo indicado en `Inscription.start()`).
tbk_user  <br> <i> String </i> | Identificador único de la inscripción del cliente (devuelto por `Inscription.finish()`).
buy_order  <br> <i> Number </i> | Identificador único de la compra generado por el comercio.
details  <br> <i> Array </i> | Lista de objetos, uno por cada tienda diferente del mall que participa en la transacción.
details [].commerce_code  <br> <i>String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo: 12.
details [].buy_order  <br> <i> String </i> | Identificador único de la compra generado por el comercio hijo (tienda).
details [].amount  <br> <i> Decimal </i> | Monto de la sub-transacción de pago. En pesos o dólares según configuración comercio mall padre.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la sub-transacción de pago.

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
details [].status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED).
details [].authorization_code  <br> <i> String </i> | Código de autorización de la sub-transacción de pago.
details [].payment_type_code  <br> <i> String </i> | [Tipo](/producto/webpay#tipos-de-pago) (VC, NC, SI, etc.) de la sub-transacción de pago.
details [].response_code  <br> <i> Number </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. <br> -1, -2, -3, -4, -5, -6, -7, -8: Rechazo <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la sub-transacción de pago.
details [].commerce_code  <br> <i> Number </i> | Código de comercio del comercio hijo (tienda).
details [].buy_order  <br> <i> String </i> | Orden de compra generada por el comercio hijo para la sub-transacción de pago.

<aside class="warning">
Cualquier valor distinto de número en `installmentsNumber` (incluyendo letras,
inexistencia del campo o nulo) será asumido como cero, es decir "Sin cuotas".
</aside>

### Consultar un pago realizado con Webpay OneClick Mall

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

#### `Transaction.status()`

Permite consultar el estado de pago realizado a través de Webpay Oneclick.
Retorna el resultado de la autorización.

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
GET /rswebpaytransaction/api/oneclick/v1.0/transactions/{buyOrder}

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la transacción a  consultar.

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
details [].status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED).
details [].authorization_code  <br> <i> String </i> | Código de autorización de la sub-transacción de pago.
details [].payment_type_code  <br> <i> String </i> | [Tipo](/producto/webpay#tipos-de-pago) (VC, NC, SI, etc.) de la sub-transacción de pago.
details [].response_code  <br> <i> Number </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. <br> -1, -2, -3, -4, -5, -6, -7, -8: Rechazo <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas de la sub-transacción de pago.
details [].commerce_code  <br> <i> Number </i> | Código de comercio del comercio hijo (tienda).
details [].buy_order  <br> <i> String </i> | Orden de compra generada por el comercio hijo para la sub-transacción de pago.
status  <br> <i> Text </i> | Estado de la transacción (AUTHORIZED,FAILED). Largo máximo: 64
balance  <br> <i> Decimal </i> | Monto restante de la sub-transacción de pago original: monto inicial – monto anulado. Largo máximo: 17


### Reversar o Anular un pago Webpay OneClick Mall

Para Webpay OneClick Mall hay dos operaciones diferentes para dejar sin efecto
transacciones autorizadas: La reversa y la anulación.

**La reversa** se aplica para **problemas operacionales (lado comercio) o de
comunicación entre comercio y Transbank que impidan recibir a tiempo la
respuesta de una autorización**. En tal caso el comercio **debe** intentar
reversar la transacción de autorización para evitar un posible descuadre entre
comercio y Transbank. La reversa funciona sobre la operación completa del mall,
lo que significa que **todas las transacciones realizadas en la operación mall
serán reversadas**. Y sólo puede ser usada hasta 30 segundos después de la
llamada a `Transaction.authorize()`

**La anulación**, en cambio, actúa individualmente sobre las transacciones de
las _tiendas_ de un mall. Por ende, **la anulación es la operación correcta a
utilizar para fines financieros**, de manera de anular un cargo ya realizado.
También es posible *reversar una anulación* debido a problemas operacionales
(por ejemplo un error de comunicación al momento de anular, que le impida al
comercio saber si Transbank recibió la anulación).

Para llevar a cabo la reversa, el comercio debe usar el método para este caso
sin indicar el monto. Para la anulación, se debe usar el método indicando el monto
de la anulación.

#### `Transaction.refund()`

Permite reversar o anular una transacción de venta autorizada con anterioridad.
Este método retorna como respuesta un identificador único de la transacción de reversa/anulación.

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
POST /rswebpaytransaction/api/oneclick/v1.0/transactions/{buyOrder}/refunds

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "commerce_code": Próximamente...,
  "detail_buy_order": "ordenCompra12345",
  "amount": 1000
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra de la transacción a  reversar o anular.
commerce_code  <br> <i> String </i> | Código de comercio hijo.
detail_buy_order  <br> <i> String </i> | Orden de compra hija de la transacción a  reversar o anular.
amount  <br> <i> Number </i> | (Opcional) Monto a anular. Si está presente se ejecuta una anulación, en caso contrario se ejecuta una reversa (a menos que haya pasado el tiempo máximo para reversar).

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
  "balance": 0.00
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSE. NULLIFY). Largo máximo: 10
authorization_code  <br> <i> Boolean </i> | Código de autorización. Largo máximo: 6
authorization_date  <br> <i> ISO8601 </i> | Fecha de la autorización de la transacción.
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17
balance  <br> <i> Decimal </i> | Monto restante de la sub-transacción de pago original: monto inicial – monto anulado. Largo máximo: 17

## Webpay Transacción Completa

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

Una transacción completa permite al comercio presentar al tarjetahabiente un
formulario propio para almacenar los datos de la tarjeta, fecha de vencimiento
y cvv.


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

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
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
buy_order  <br> <i> String </i> | Orden de compra de la tienda. Este número debe ser único para cada transacción. Largo máximo: 26. La orden de compra puede tener: Números, letras, mayúsculas y minúsculas, y los signos <code>&#124;_=&%.,~:/?[+!@()>-</code> Largo máximo: 26
session_id  <br> <i> String </i> | (Opcional) Identificador de sesión, uso interno de comercio, este valor es devuelto al final de la transacción. Largo máximo: 61
amount  <br> <i> Decimal </i> | Monto de la transacción. Máximo 2 decimales para USD. Largo máximo: 17
cvv  <br> <i> String </i> | URL del comercio, a la cual Webpay redireccionará posterior al proceso de autorización. Largo máximo: 4
card_number  <br> <i> String </i> | Número de la tarjeta con la que se debe hacer la transacción. Largo máximo: 16
card_expiration_date  <br> <i> String </i> | Fecha de expiración de la tarjeta con la que se realiza la transacción. Largo máximo: 5

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
token  <br> <i> String </i> | Token de la transacción. Largo: 64.

### Consulta de cuotas

Para consultar el valor de las cuotas que pagará el tarjeta habiente en una
transacción completa, es necesario llamar al método `Transaction.installments()`

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

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "installments_number": 10
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Número de orden de compra. Largo máximo: 64
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
installments_amount  <br> <i> String </i> | Monto de cada cuota. Largo: 64.
id_query_installments  <br> <i> String </i> | Identificador de las cuotas. Largo: 2.
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

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
  "id_query_installments": 15,
  "deferred_period_index": 1,
  "grace_period": false
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token identificador de la transacción. Largo máximo: 64
id_query_installments  <br> <i> Number </i> | (Opcional) Identificador de cuota. Largo máximo: 2
deferred_period_index  <br> <i> Number </i> | (Opcional) Cantidad de periodo diferido. Largo máximo: 2
grace_period  <br> <i> Number </i> | (Opcional) Indicador de periodo de gracia.

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
vci  <br> <i> String </i> | Código de respuesta de la autenticación bancaria. Largo máximo: 64
amount  <br> <i> Number </i> | Monto de la transacción. Sólo en caso de dolar acepta dos decimales. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 64
buy_order  <br> <i> String </i> | Número de orden de compra. Largo máximo: 26
session_id  <br> <i> String </i> | ID de sesión de la compra. Largo máximo: 61
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción. Largo máximo: 19
accounting_date  <br> <i> String </i> | Fecha contable de la transacción.
transactionD_date  <br> <i> ISO8601 </i> | Fecha de la transacción.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción de pago. Largo máximo: 6
payment_type_code  <br> <i> String </i> | Indica el tipo de tarjeta utilizada. Largo máximo: 2
response_code  <br> <i> Number </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. <br> -1, -2, -3, -4, -5, -6, -7, -8: Rechazo <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
installments_number <br> <i> Number </i> | Número de cuotas de la transacción. Largo máximo: 2
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Consultar estado de una transacción completa

Esta operación permite obtener el estado de la transacción en cualquier momento. En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error inesperado permite conocer el estado y tomar las acciones que correspondan.

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
Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token identificador de la transacción. Largo: 64.

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
vci  <br> <i> String </i> | Código de respuesta de la autenticación bancaria. Largo máximo: 64
amount  <br> <i> Number </i> | Monto de la transacción. Sólo en caso de dolar acepta dos decimales. Largo máximo: 17
status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 64
buy_order  <br> <i> String </i> | Número de orden de compra. Largo máximo: 26
session_id  <br> <i> String </i> | ID de sesión de la compra. Largo máximo: 61
card_detail  <br> <i> cardDetail </i> | Objeto que contiene información de la tarjeta utilizado por el tarjetahabiente.
card_detail.card_number  <br> <i> String </i> | Los últimos 4 dígitos de la tarjeta usada en la transacción. Largo máximo: 19
accounting_date  <br> <i> String </i> | Fecha contable de la transacción.
transaction_date  <br> <i> ISO8601 </i> | Fecha de la transacción.
authorization_code  <br> <i> String </i> | Código de autorización de la transacción de pago. Largo máximo: 6
payment_type_code  <br> <i> String </i> | Indica el tipo de tarjeta utilizada.
response_code  <br> <i> Number </i> | Código de retorno del proceso de pago, donde: <br> 0 (cero) es aprobado. <br> -1, -2, -3, -4, -5, -6, -7, -8: Rechazo <br> -97: Límites Oneclick, máximo monto diario de pago excedido. <br> -98: Límites Oneclick, máximo monto de pago excedido <br> -99: Límites Oneclick, máxima cantidad de pagos diarios excedido.
installments_number <br> <i> Number </i> | Número de cuotas de la transacción. Largo máximo: 2
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 17

### Reversar o Anular un pago Transaccion Completa

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
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/{token}/refund
Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
 "amount": 1000
}
```
**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.
amount  <br> <i> Decimal </i> | (Opcional) Monto que se desea anular de la transacción. Largo máximo: 17.

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
  "balance": 0.00
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
type  <br> <i> String </i> | Tipo de reembolso (REVERSE. NULLIFY). Largo máximo: 10
authorization_code  <br> <i> String </i> | Código de autorización de la anulación. Largo máximo: 6
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización.
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17
balance  <br> <i> Decimal </i> | Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17
response_code  <br> <i> Number </i> | Código de resultado de la reversa/anulación. Si es exitoso es 0, de lo contrario la reversa/anulación no fue realizada. Largo máximo: 2


## Webpay Transacción Mall Completa

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

#### Flujo Webpay Transacción Mall Completa

El flujo de Transacción Mall Completa es en general el mismo que el de [Webpay Transacción Completa](#webpay-transaccion-completa) tanto de cara al tarjeta habiente como de cara al integrador.

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

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "buy_order": "ordenCompra12345678",
  "session_id": "sesion1234564",
  "card_number": "4239000000000000",
  "card_expiration_date": "22/10",
  "details": [
    {
      "amount": 10000,
      "commerce_code": "Próximamente...",
      "buy_order": "123456789"
    },
    {
      "amount": 10000,
      "commerce_code": "Próximamente...",
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

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "commerce_code": Próximamente...,
  "buy_order": "123456789",
  "installments_number": 10
}
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Número de orden de compra. Largo máximo: 64
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
installments_amount  <br> <i> String </i> | Monto de cada cuota. Largo: 64.
id_query_installments  <br> <i> String </i> | Identificador de las cuotas. Largo: 2.
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

Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "details": [
    {
      "commerce_code": Próximamente...,
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
token  <br> <i> String </i> | Token identificador de la transacción. Largo máximo: 64
details <br> <i> details </i> | Listado con las transacciones mall. 
commerce_code  <br> <i> String </i> | Código comercio asignado por Transbank para la tienda perteneciente al mall a la cual corresponde esta transacción. Largo máximo: 12
buy_order  <br> <i> String </i> | Orden de compra de la tienda del mall. Largo máximo: 26
id_query_installments  <br> <i> Number </i> | (Opcional) Identificador de cuota. Largo máximo: 2
deferred_period_index  <br> <i> Number </i> | (Opcional) Cantidad de periodo diferido. Largo máximo: 2
grace_period  <br> <i> Number </i> | (Opcional) Indicador de periodo de gracia.

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
      "commerce_code": "Próximamente...",
      "buy_order": "505479072"
    }
  ]
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra del mall. Largo máximo: 26
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente.Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accouting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
details  <br> <i> Array </i> | Lista con resultado de cada una de las transacciones enviadas.
details [].amount  <br> <i> Number </i> | Monto de la transacción. Largo máximo: 17
details [].status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 64
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
details [].payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
details [].responseCode  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada.<br> -1 = Rechazo de transacción.<br> -2 =  Transacción debe reintentarse. <br> -3 = Error en transacción. <br> -4 = Rechazo de transacción.<br> -5 = Rechazo por error de tasa. <br> -6 = Excede cupo máximo mensual. <br> -7 = Excede límite diario por transacción. <br> -8 = Rubro no autorizado.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 6
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 255
status <br> <i> Text </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 17
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 64

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
Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json
```

**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token identificador de la transacción. Largo: 64.

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
      "commerce_code": "Próximamente...",
      "buy_order": "505479072"
    }
  ]
}
```

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
buy_order  <br> <i> String </i> | Orden de compra del mall. Largo máximo: 26
card_detail  <br> <i> carddetails </i> | Objeto que representa los datos de la tarjeta de crédito del tarjeta habiente.
card_detail.card_number  <br> <i> String </i> | 4 últimos números de la tarjeta de crédito del tarjetahabiente.Solo para comercios autorizados por Transbank se envía el número completo. Largo máximo: 19.
accouting_date  <br> <i> String </i> | Fecha de la autorización. Largo: 4, formato MMDD
transaction_date  <br> <i> String </i> | Fecha y hora de la autorización. Largo: 6, formato: MMDDHHmm
details  <br> <i> Array </i> | Lista con resultado de cada una de las transacciones enviadas.
details [].amount  <br> <i> Number </i> | Monto de la transacción. Largo máximo: 17
details [].status  <br> <i> String </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 64
details [].authorization_code  <br> <i> String </i> | Código de autorización de la transacción Largo máximo: 6
details [].payment_type_code   <br> <i> String </i> | [Tipo de pago](/producto/webpay#tipos-de-pago) de la transacción.<br> VD = Venta Débito.<br> VN = Venta Normal. <br> VC = Venta en cuotas. <br> SI = 3 cuotas sin interés. <br> S2 = 2 cuotas sin interés. <br> NC = N Cuotas sin interés <br> VP = Venta Prepago.
details [].responseCode  <br> <i> String </i> | Código de respuesta de la autorización. Valores posibles: <br> 0 = Transacción aprobada.<br> -1 = Rechazo de transacción.<br> -2 =  Transacción debe reintentarse. <br> -3 = Error en transacción. <br> -4 = Rechazo de transacción.<br> -5 = Rechazo por error de tasa. <br> -6 = Excede cupo máximo mensual. <br> -7 = Excede límite diario por transacción. <br> -8 = Rubro no autorizado.
details [].installments_number  <br> <i> Number </i> | Cantidad de cuotas. Largo máximo: 2
details [].installments_amount  <br> <i> Number </i> | Monto de cada cuota. Largo máximo: 17
details [].commerce_code  <br> <i> String </i> | Código comercio de la tienda. Largo: 6
details [].buy_order  <br> <i> String </i> | Orden de compra de la tienda. Largo máximo: 255
status <br> <i> Text </i> | Estado de la transacción (AUTHORIZED, FAILED). Largo máximo: 17
balance <br> <i> Number </i> | Monto restante para un detalle anulado. Largo máximo: 64

### Anulación Webpay Transacción Completa

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
Tbk-Api-Key-Id: Próximamente...
Tbk-Api-Key-Secret: Próximamente...
Content-Type: application/json

{
  "buy_order": "415034240",
  "commerce_code": "Próximamente...",
  "amount": 1000
}
```
**Parámetros**

Nombre  <br> <i> tipo </i> | Descripción
------   | -----------
token  <br> <i> String </i> | Token de la transacción. Largo: 64.
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
type  <br> <i> String </i> | Tipo de reembolso (REVERSE. NULLIFY). Largo máximo: 10
authorization_code  <br> <i> String </i> | Código de autorización de la anulación. Largo máximo: 6
authorization_date  <br> <i> String </i> | Fecha y hora de la autorización.
nullified_amount  <br> <i> Decimal </i> | Monto anulado. Largo máximo: 17
balance  <br> <i> Decimal </i> | Saldo actualizado de la transacción (considera la venta menos el monto anulado). Largo máximo: 17
response_code <br> <i> Number </i> | Código del resultado del pago. Si es exitoso es 0, de lo contrario el pago no fue realizado. Largo máximo: 2
