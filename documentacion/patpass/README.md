# PatPass

## PatPass by Webpay
<div class="pos-title-nav">
  <div tbk-link='/referencia/patpass' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugin/patpass' tbk-link-name='Plugins'></div>
</div>

### Crear una suscripción

Para una transacción PatPass by Webpay que registre una suscripción, lo primero que necesitas es preparar una instancia de `WebpayNormal`
con la `Configuration` que incluye el código de comercio y los certificados a
usar.

Una forma fácil de comenzar es usar la configuración para pruebas que viene
incluida en el SDK, indicando el correo que quieres que se use para las
notificaciones de PatPass

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.webpay.configuration.Configuration;
import cl.transbank.webpay.Webpay;
import cl.transbank.patpass.PatPassByWebpayNormal;
// ...

PatPassByWebpayNormal transaction = new Webpay(
        Configuration.forTestingPatPassByWebpayNormal("testpatpassbywebpay@mailinator.com")
    ).getPatPassByWebpayTransaction();
```

```php
PENDIENTE
```

```csharp
PENDIENTE
```

<aside class="notice">
Como necesitarás ese objeto `transaction` en múltiples ocasiones, es buena idea
encapsular la lógica que lo genera en algún método que puedas reutilizar.
</aside>

Una vez que ya cuentas con esa preparación, puedes iniciar transacciones:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpay.wswebpay.service.WsInitTransactionOutput;
// ...
double amount = 1000; // monto a cobrar cada mes
String sessionId = "identificador que será retornado en el callback de resultado";
String buyOrder = "identificador único de orden de compra";
String returnUrl = "https://callback/resultado/de/transaccion";
String finalUrl = "https://callback/final/post/comprobante/webpay";
PatPassInfo patPassInfo = new PatPassInfo()
    .setServiceId("335456675433")
    .setCardHolderId("11.111.111-1")
    .setCardHolderName("Juan Pedro")
    .setCardHolderLastName1("Alarcón")
    .setCardHolderLastName2("Perez")
    .setCardHolderMail("example@example.com")
    .setCellPhoneNumber("1234567")
    .setExpirationDate(new GregorianCalendar(2019, 1, 1));
WsInitTransactionOutput initResult = transaction.initTransaction(
        amount, sessionId, buyOrder, returnUrl, finalUrl, patPassInfo);

String formAction = initResult.getUrl();
String wsToken = initResult.getToken();
```

```php
PENDIENTE
```

```csharp
PENDIENTE
```

Todo es muy similar a Webpay Normal, con la única diferencia de la información
específica de PatPass que incluye los datos personales del usuario (donde es
importante el correo electrónico para las notificaciones de patmass) y la fecha
de expiración de la suscripción.

La URL y el token retornados te indican donde debes redirigir al usuario para
que comience el flujo de pago. Esta redirección debe ser vía `POST` por lo que
deberás crear un formulario web con un campo `token_ws` hidden y enviarlo
programáticamente para entregar el control a Webpay.

<aside class="notice">
Tip: En el ambiente de integración puedes usar la tarjeta VISA 4051885600446623
para hacer pruebas simples de éxito. El CVV es 123 y la fecha de vencimiento
cualquiera superior a la fecha actual. Luego para la autenticación bancaria usa
el RUT 11.111.111-1 y la clave 123. Para pruebas exhaustivas [consulta todas las
tarjetas de prueba en la sección de
Ambientes](/documentacion/como_empezar#ambientes).
</aside>


### Confirmar una suscripción

Una vez que el tarjetahabiente ha pagado (o declinado, o ha ocurrido un error),
Webpay retornará el control vía `POST` a la URL que indicaste en el `returnUrl`.
Recibirás también el parámetro `token_ws` que te permitirá conocer el resultado
de la transacción:

<div class="language-simple" data-multiple-language></div>

```java
import com.transbank.webpay.wswebpay.service.TransactionResultOutput;
import com.transbank.webpay.wswebpay.service.WsTransactionDetailOutput;
// ...
TransactionResultOutput result =
    transaction.getTransactionResult(request.getParameter("token_ws"));
WsTransactionDetailOutput output = result.getDetailOutput().get(0);
if (output.getResponseCode() == 0) {
    // Suscripción exitosa, puedes procesar el resultado con el contenido de
    // las variables result y output.
}
```

```php
PENDIENTE
```

```csharp
PENDIENTE
```

> Los SDKs se encarga de que al mismo tiempo que se obtiene el resultado de la
> transacción se haga el _acknowledge_ a Transbank de manera que no haya
> posibilidad de que la transacción se revierta.

En el caso exitoso deberás llevar el control vía `POST` nuevamente a Webpay para
que el tarjetahabiente vea el comprobante que le deja claro que se ha realizado
el cargo en su tarjeta. Nuevamente deberás generar un formulario con el
`token_ws` como un campo hidden. La URL para redirigir la debes obtener desde
`result.getUrlRedirection()`.

Finalmente después del comprobante Webpay redirigirá otra vez (vía `POST`) a tu
sitio, esta vez a la URL que indicaste en el `finalUrl` cuando iniciaste la
transacción. Tal como antes, recibirás el `token_ws` que te permitirá
identificar la transacción y mostrar un comprobante o página de éxito a tu
usuario.

## Credenciales y Ambiente

Las credenciales de PatPass by Webpay se configuran de igual forma a las Webpay
transacciones Webpay: en base a un objeto `Configuration`. Y si bien para hacer
pruebas iniciales pueden usarse las credenciales pre-configuradas (como se puede
ver en todos los ejemplos anteriores), para poder superar el [proceso de
validación](/documentacion/como_empezar#el-proceso-de-validaci-n-y-puesta-en-producci-n)
en el ambiente de integración será necesario configurar explícitamente tu código
de comercio y certificados:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.webpay.configuration.Configuration;
import cl.transbank.webpay.Webpay;
import cl.transbank.patpass.PatPassByWebpayNormal;

//...

Configuration configuration = new Configuration();
configuration.setCommerceCode("12345"); // acá va tu código de comercio
configuration.setPrivateKey( // pega acá la llave privada de tu certificado
    "-----BEGIN RSA PRIVATE KEY-----\n" +
    "MIIEpQIBAAKCAQEA0ClVcH8RC1u+KpCPUnzYSIcmyXI87REsBkQzaA1QJe4w/B7g\n" +
    //....
    "MtdweTnQt73lN2cnYedRUhw9UTfPzYu7jdXCUAyAD4IEjFQrswk2x04=\n" +
    "-----END RSA PRIVATE KEY-----");
configuration.setPublicCert( // pega acá tu certificado público
    "-----BEGIN CERTIFICATE-----\n" +
    "MIIDujCCAqICCQCZ42cY33KRTzANBgkqhkiG9w0BAQsFADCBnjELMAkGA1UEBhMC\n" +
    //....
    "-----END CERTIFICATE-----");

// Default significa usar pesos o dólares (dependiendo del código de comercio).
configuration.setPatPassCurency(PatPassByWebpayNormal.Currency.DEFAULT);
// Si se quiere usar UFs, especificar PatPassByWebpayNormal.Currency.UF.
configuration.commerceMail("mail-para-notificaciones-patpass@micomercio.cl");

Webpay webpay = new Webpay(configuration);
// Ahora puedes obtener las instancias de las transacciones
// que usarás, por ejemplo:
PatPassByWebpayNormal patPassTransaction = webpay.getPatPassByWebpayTransaction();
```

```php
PENDIENTE
```

```csharp
PENDIENTE
```

### Apuntar a producción

Para cambiar el ambiente al que apunta el SDK (que por defecto es integración),
también debes usar el objeto `Configuration` (antes de crear una instancia de
`Webpay`):

<div class="language-simple" data-multiple-language></div>

```java
Configuration configuration = new Configuration();
configuration.setEnvironment(Webpay.Environment.PRODUCCION);
// agregar también configuración del código de comercio y certificados
```

```php
PENDIENTE
```

```csharp
PENDIENTE
```

<aside class="warning">
Los SDKs se encarga de configurar el certificado público de Transbank
correspondiente al ambiente seleccionado. Pero es tu responsabilidad
actualizar periódicamente la versión del SDK para recibir actualizaciones a
dicho certificado. De lo contrario, expirará el certificado y dejarás de poder
operar con Transbank.
</aside>

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
            <div class="fo-size-20"><i class="fas fa-envelope"></i> Envianos un mensaje..</div>
            <div class='td_pa-txt'>
              Si necesitas resolver algún tipo de incidencia en el portal o si existe algún problema con tu integración y  que no has podido resolver, contáctanos a través de nuestro formulario.
            </div>
          </div>
        </a>
      </div>
    </div>
  </div>
</div>
