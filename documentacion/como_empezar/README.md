# Cómo empezar

Para empezar a integrar los productos de Transbank, te recomendamos usar
nuestros SDK y plugins, disponibles para múltiples lenguajes de programación y
plataformas. En general, existe un único Transbank SDK para el _backend_
de tu e-commerce, el cual te permite operar con todos nuestros productos.

Si quieres implementar Webpay Plus, te recomendamos revisar [nuestros plugins oficiales](/plugin).  

Adicionalmente, existen SDKs especializados para algunos productos y
plataformas que requieren integraciones más allá del _backend_, como pasa por
ejemplo con Onepay.

En esta sección veremos los pasos para comenzar con el SDK que corresponda al
lenguaje de programación que utilices en tu backend. Para los SDKs
específicos a Onepay, visita la sección dedicada a ese producto.

## Flujo de Integración

Inicialmente, el comercio tendrá algunas tareas comerciales que realizar mientras ocurre el proceso de integración.
Este proceso de afiliación comercial se puede realizar en paralelo al proceso técnico de integración.  
A continuación, puedes conocer el flujo completo.
<img class="td_img-night" src="/images/documentacion/flujo-integracion.svg" alt="Flujo de integración">

## Proceso técnico de integración

Este proceso contempla todas las tareas necesarias que debe realizar el comercio para integrar el producto contratado
dentro de sus sistemas.

### A) Usando un plugin

Si quieres implementar Webpay Plus con alguno de nuestros plugins oficiales,
revisa [su documentación](/plugin) específica. En ese caso, el proceso es más simple y no requiere escribir
código como en el caso de los SDK, ya que basta con realizar la instalación y configuración del plugin en la plataforma
que estés utilizando.

### B) Usando un SDK

Para instalar el SDK, debes agregarlo al gestor de dependencias de tu lenguaje:

[En **Java**](https://github.com/TransbankDevelopers/transbank-sdk-java#instalaci%C3%B3n)
debes agregar esta entrada en tu archivo `pom.xml` de Maven:

```xml
<dependency>
    <groupId>com.github.transbankdevelopers</groupId>
    <artifactId>transbank-sdk-java</artifactId>
    <version>{mira-en-github-la-ultima-version-disponible}</version>
</dependency>
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK Java](https://github.com/TransbankDevelopers/transbank-sdk-java#instalaci%C3%B3n) para más opciones e información de la última versión disponible.

[En
**PHP**](https://github.com/TransbankDevelopers/transbank-sdk-php#instalaci%C3%B3n)
puedes usar composer (si no lo tienes, puedes instalarlo desde [acá](https://getcomposer.org/)) para descargar la última versión del SDK,
ejecutando esto en la línea de comandos cuando estés en la raíz de tu proyecto:

```bash
composer require transbank/transbank-sdk:^1.8
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK PHP](https://github.com/TransbankDevelopers/transbank-sdk-php#instalaci%C3%B3n) para más opciones de instalación.

[En
**.NET**](https://github.com/TransbankDevelopers/transbank-sdk-dotnet#instalaci%C3%B3n)
puedes instalar el SDK desde la línea de comandos del Package Manager de Visual Studio:

```bash
PM> Install-Package TransbankSDK
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK .NET](https://github.com/TransbankDevelopers/transbank-sdk-dotnet#instalaci%C3%B3n) para más opciones de instalación.

[En
**Ruby**](https://github.com/TransbankDevelopers/transbank-sdk-ruby#instalaci%C3%B3n) puedes instalar el SDK como una gema:

```bash
gem install transbank-sdk
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK Ruby](https://github.com/TransbankDevelopers/transbank-sdk-ruby#instalaci%C3%B3n) para más opciones de instalación.

[En
**Python**](https://github.com/TransbankDevelopers/transbank-sdk-python#instalaci%C3%B3n) puedes instalar el SDK desde PyPI:

```bash
pip install transbank-sdk
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK Python](https://github.com/TransbankDevelopers/transbank-sdk-python#instalaci%C3%B3n) para más opciones de instalación.

### C) Usando el API REST

También puedes consumir el API REST de los productos directamente.
Si usas un lenguaje de programación que no tiene un SDK oficial o
simplemente quieres conectarte directamente al API, debes revisar la [Referencia del API REST](/referencia/webpay?l=http)
en el tab "http" para conocer los diferentes endpoints de cada producto, sus parámetros de entrada y parámetros de respuesta.

### Ejemplos

Ponemos a tu disposición una serie de repositorios en nuestro Github para ayudarte a entender la integración de mejor forma.

* [Ejemplo en PHP](https://github.com/TransbankDevelopers/transbank-sdk-php-webpay-rest-example)
* [Ejemplo en .Net](https://github.com/TransbankDevelopers/transbank-sdk-dotnet-webpay-rest-example)
* [Ejemplo en Java](https://github.com/TransbankDevelopers/transbank-sdk-java-webpay-rest-example)
* [Ejemplo en Ruby](https://github.com/TransbankDevelopers/transbank-sdk-ruby-webpay-rest-example)
* [Ejemplo en Python](https://github.com/TransbankDevelopers/transbank-sdk-python-webpay-rest-example)
* [Ejemplo en NodeJS](https://github.com/TransbankDevelopers/transbank-sdk-nodejs-webpay-rest-example)

Adicionalmente, puedes revisar el proyecto de ejemplo de NodeJS [funcionando acá](https://transbank-rest-demo.herokuapp.com/)

En el caso de integrar webpay en una aplicación móvil Android, usando webview, debes tener presente la siguiente configuración:

1. Al momento de abrir el webview.

   ```js
    // habilitar el Cookie Manager. Depende del nivel de la API de Android que se utilice se habilita de diferente forma
    if (android.os.Build.VERSION.SDK_INT >= 21)
        CookieManager.getInstance().setAcceptThirdPartyCookies(myWebPayView, true); // myWebPayView es el WebView
    else
        CookieManager.getInstance().setAcceptCookie(true);
    // Asignar el caché en el webview
    webPayView.getSettings().setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);
    ```

2. Al momento de cerrar el webview

  ```js
    // Remover Cookies
    if (android.os.Build.VERSION.SDK_INT >= 21)
      CookieManager.getInstance().removeAllCookies(null);
    else
      CookieManager.getInstance().removeAllCookie();
    // Borrar caché
    myWebPayView.clearCache(true);
  ```

## Ambientes

Transbank provee dos ambientes: **Integración** y **Producción**.

**Ambiente de integración**: En este ambiente el comercio realiza la integración del producto a contratar y testea
su solución de medio pago.
`HOST: https://webpay3gint.transbank.cl`

**Ambiente de producción**: En este ambiente el comercio operará luego de finalizar el [proceso de puesta en producción](#puesta-en-produccion) y realizará transacciones
con tarjetas de crédito, débito o prepago **reales**.  
`HOST: https://webpay3g.transbank.cl`

### Ambiente de integración

Para las transacciones Webpay en este ambiente se deben usar estas
tarjetas:

Tipo de tarjeta | Detalle | Resultado
--------------- | ------- | ----------
VISA            | 4051 8856 0044 6623<br />CVV 123<br />cualquier fecha de expiración | Genera transacciones **aprobadas**.
MASTERCARD      | 5186 0595 5959 0568<br /> CVV 123<br />cualquier fecha de expiración | Genera transacciones **rechazadas**.
Redcompra       | 4051 8842 3993 7763 | Genera transacciones aprobadas (para operaciones que permiten débito Redcompra y prepago)
Redcompra       | 5186 0085 4123 3829 | Genera transacciones rechazadas (para operaciones que permiten débito Redcompra y prepago)

Cuando aparece un formulario de autenticación con RUT y clave, se debe
usar el RUT **11.111.111-1** y la clave **123**.

### Códigos de comercio

<aside class="notice">
Tip: Los SDK y plugins provistos por Transbank tienen pre-configuradas
las credenciales para el ambiente de integración. Puedes ver como usarlas
en la sección de [documentación de los SDK](/referencia/webpay#webpay) y [Plugins](/plugin)
</aside>

A continuación encontrarás todos los códigos de comercio disponibles en el ambiente de integración.
Para todos los código de comercio, la **llave secreta (Api Key Secret)** es `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`  

Producto | Código de Comercio |
-------- | ------------ |
Webpay Plus | `597055555532`
Webpay Plus Captura Diferida | `597055555540`
Webpay Plus Mall | `597055555535` Mall <br> `597055555536` Tienda 1 <br> `597055555537` Tienda 2
Webpay Plus Mall Captura Diferida| `597055555581` Mall <br> `597055555582` Tienda 1 <br> `597055555583` Tienda 2
Oneclick Mall | `597055555541` Mall <br> `597055555542` Tienda 1 <br> `597055555543` Tienda 2
Oneclick Mall Captura Diferida | `597055555547` Mall <br> `597055555548` Tienda 1 <br> `597055555549` Tienda 2
Transacción Completa | `597055555530`
Transacción Completa sin CVV | `597055555557`
Transacción Completa Diferida | `597055555531`
Transacción Completa Diferida sin CVV | `597055555556`
Transacción Completa Mall | `597055555573` Mall <br> `597055555574` Tienda 1 <br> `597055555575` Tienda 2
Transacción Completa Mall sin CVV| `597055555551` Mall <br> `597055555552` Tienda 1 <br> `597055555553` Tienda 2
Transacción Completa Mall Captura Diferida | `597055555576` Mall <br> `597055555577` Tienda 1 <br> `597055555578` Tienda 2
Transacción Completa Mall Captura Diferida sin CVV | `597055555561` Mall <br> `597055555562` Tienda 1 <br> `597055555563` Tienda 2
Patpass Comercio | `27082157` Código de comercio <br> `J7xYiUS7xqD7LkbWSUHI` Lave secreta (Api Key Secret)

## Puesta en Producción

1. Una vez que el comercio determine que ha finalizado su integración, se debe realizar un [proceso de validación](#el-proceso-de-validacion). Si realizaste la integración con un plugin, considera que junto con la planilla de integración debes enviar el logo (GIF, 130x59) a soporte@transbank.cl.
2. Transbank informará via correo electrónico el resultado de la validación enviado por el comercio. En caso de que la validación sea aprobada, Transbank indicará la **llave secreta** (_API Key Secret_) para poder usar el ambiente de producción. Posterior a ello, será necesario [cambiar la configuración del e-commerce para funcionar en producción](#configuracion-de-produccion)
3. Con la configuración del ambiente de producción ya lista, será necesario realizar una compra de $50 para validar el correcto funcionamiento.
4. Ya estás operando en producción.

### Onepay

En el caso de Onepay las credenciales consisten en:

* Un API Key
* Un secreto ("shared secret").

Estos valores serán provistos por Transbank y en su conjunto permiten hacer
transacciones a nombre del comercio. **Debes custodiar estas credenciales para
evitar que caigan en manos de terceros**.

### Webpay, Oneclick y Transacción Completa

En el caso de Webpay, las credenciales consisten en:

* Un código de comercio (Api-Key-Id).
* Una llave secreta (Api-Key-Secret).

### Obtener tu llave secreta (proceso de validación)

Para usar el ambiente de producción (donde se utiliza dinero real), necesitas tener tu **llave secreta**, que es un código especial que está asociado a tu código de comercio.
Para obtenerla necesitas pasar un proceso de validación, que está [explicado a continuación](#el-proceso-de-validacion).

Al finalizar este proceso de validación, obtendrás tu **llave secreta**.
<aside class="notice">
Nota: Esta **llave secreta** es como la contraseña de tu código de comercio, por lo que no debes compartirla. Se usa para identificar que tu comercio es quién realmente está realizando cada operación (transacción, anulación de un pago, etc).
</aside>

## El proceso de validación

Durante la validación de la integración se pretende verificar que el comercio
transacciona de manera segura y sin problemas, por lo que se solicitarán una
serie de pruebas y su posterior envío de evidencias para validar la
integración. Esta validación es requisito necesario para dejar al comercio en
producción y no se permitirá que un comercio utilice productivamente el
servicio Webpay sin poseer una validación.

Transbank solo validará las integraciones de aquellos comercios que tengan un código de comercio productivo.
Para obtenerlo, sigue las instrucciones en cómo hacerse cliente en el portal <http://www.transbank.cl> o contacte a su
ejecutivo comercial.

En esta etapa, el comercio envía las evidencias a [soporte@transbank.cl](mailto:soporte@transbank.cl) empleando el formulario correspondiente al
producto integrado indicando claramente las órdenes de compra, fecha y hora de las transacciones. Para integraciones
Webpay que utilicen algún [plugin oficial](/plugin) existe un formulario especial.

**Para integración con plugins**, creamos un nuevo formulario online para que comiences el proceso de validación: <a class="typeform-share button" href="https://form.typeform.com/to/fZqOJyFZ?typeform-medium=embed-snippet" data-mode="popup" data-size="100" target="_blank"> Comenzar formulario de integración </a> 

Descargar el formulario de envidencias: 

* Para integraciones Webpay que usen un plugin oficial: usa el formulario indicado más arriba. 
* Para integraciones Webpay (Webpay Plus, Webpay OneClick, Webpay transacción completa): [**Descargar**](https://transbankdevelopers.cl/files/evidencia-integracion-webpay-rest.docx)
* Para integraciones Onepay: [**Descargar**](https://transbankdevelopers.cl/files/evidencia-de-integracion-onepay.docx)
* Para integraciones Patpass Comercio: [**Descargar**](https://transbankdevelopers.cl/files/evidencia-de-integracion-patpass-comercio.docx)

Soporte validará que los casos de prueba sean consistentes con los registrados
en los sistemas de Webpay y, de estar todo correcto, se le notificará al
comercio la conformidad para pasar a producción, recibiendo las instrucciones
para ello. De no estar consistentes las pruebas, se le hará alcances al
comercio respecto de su integración, para que realices las correcciones
correspondientes y vuelvas a enviar las evidencias una vez terminadas dichas
correcciones.

Durante el paso a producción se te exigirá realizar, al menos, una
transacción de prueba real, con la que finalizará oficialmente la puesta en
producción.

## Configuración de producción

### A) Utilizando Plugins

Si ya tienes tu código de comercio de producción y llave secreta, solo debes entrar a la configuración de tu plugin ([ver documentacion de plugins](/plugin)) y colocar:

* Ambiente: Producción
* Código de comercio: tu código de comercio de producción
* Api Key: Tu llave secreta

Al guardar, el plugin funcionará inmediatamente en ambiente de producción y podrás operar con tarjetas y transacciones reales.

### B) Utilizando los SDK

Si ya tienes tu código de comercio de producción y llave secreta, ahora solo debes configurar tu proyecto para que use
el ambiente de producción, proporcionándole tus credenciales. Te explicamos como hacerlo en los diferentes SDK realizando  
los siguientes pasos:  

Definir que se usará el ambiente de producción y pasar el Api Key (Código de comercio) y el Api Key Secret (Llave secreta)

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.common.IntegrationType;
import cl.transbank.patpass.PatpassComercio;
import cl.transbank.transaccioncompleta.FullTransaction;
import cl.transbank.webpay.oneclick.OneclickMall;
import cl.transbank.webpay.webpayplus.WebpayPlus;

//WebpayPlus Live config
WebpayPlus.Transaction.setCommerceCode("pon-tu-codigo-de-comercio-aca");
WebpayPlus.Transaction.setApiKey("pon-tu-llave-secreta-aca");
WebpayPlus.Transaction.setIntegrationType(IntegrationType.LIVE);

//WebpayPlus.MallTransaction Live config
WebpayPlus.MallTransaction.setCommerceCode("pon-tu-codigo-de-comercio-aca");
WebpayPlus.MallTransaction.setApiKey("pon-tu-llave-secreta-aca");
WebpayPlus.MallTransaction.setIntegrationType(IntegrationType.LIVE);

// OneclickMall Live Config
OneclickMall.setCommerceCode("pon-tu-codigo-de-comercio-aca");
OneclickMall.setApiKey("pon-tu-llave-secreta-aca");
OneclickMall.setIntegrationType(IntegrationType.LIVE);

//PatpassComercio Live Config
PatpassComercio.setCommerceCode("pon-tu-codigo-de-comercio-aca");
PatpassComercio.setApiKey("pon-tu-llave-secreta-aca");
PatpassComercio.setIntegrationType(IntegrationType.LIVE);

//FullTransaction Live Config
FullTransaction.Transaction.setCommerceCode("pon-tu-codigo-de-comercio-aca");
FullTransaction.Transaction.setApiKey("pon-tu-llave-secreta-aca");
FullTransaction.Transaction.setIntegrationType(IntegrationType.LIVE);
```

```php
// Webpay Plus
\Transbank\Webpay\WebpayPlus::setIntegrationType("LIVE");
\Transbank\Webpay\WebpayPlus::setCommerceCode("{commerce-code}");
\Transbank\Webpay\WebpayPlus::setApiKey("{llave-secreta}");

// Oneclick Mall
\Transbank\Webpay\OneClick::setIntegrationType("LIVE");
\Transbank\Webpay\OneClick::setCommerceCode("{commerce-code}");
\Transbank\Webpay\OneClick::setApiKey("{llave-secreta}");

// Transacción Completa
\Transbank\TransaccionCompleta::setIntegrationType("LIVE");
\Transbank\TransaccionCompleta::setCommerceCode("{commerce-code}");
\Transbank\TransaccionCompleta::setApiKey("{llave-secreta}");
```

```csharp
using Transbank.Webpay.Common;
// Webpay Plus
using Transbank.Webpay.WebpayPlus;

Transaction.CommerceCode = "5970TuCodigo";
Transaction.ApiKey = "VeryLongKey";
Transaction.IntegrationType = WebpayIntegrationType.Live;

// Oneclick
using Transbank.Webpay.Oneclick;

MallTransaction.CommerceCode = "5970TuCodigo";
MallTransaction.ApiKey = "VeryLongKey";
MallTransaction.IntegrationType = WebpayIntegrationType.Live;

// TransacciónCompleta
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.CommerceCode = "5970TuCodigo";
FullTransaction.ApiKey = "VeryLongKey";
FullTransaction.IntegrationType = WebpayIntegrationType.Live;
```

```ruby
# Webpay Plus
Transbank::Webpay::WebpayPlus::Base.commerce_code = "commercecode"
Transbank::Webpay::WebpayPlus::Base.api_key = "apikey"
Transbank::Webpay::WebpayPlus::Base.integration_type = :LIVE

# Oneclick
Transbank::Webpay::Oneclick::Base.commerce_code = "commercecode"
Transbank::Webpay::Oneclick::Base.api_key = "apikey"
Transbank::Webpay::Oneclick::Base.integration_type = :LIVE
```

```python
# Oneclick
from transbank import oneclick as BaseOneClick
from transbank.common.integration_type import IntegrationType

BaseOneClick.commerce_code = "commercecode"
BaseOneClick.api_key = "apikey"
BaseOneClick.integration_type = IntegrationType.LIVE

# Transaccion Completa
from transbank import transaccion_completa as BaseTransaccionCompleta
from transbank.common.integration_type import IntegrationType

BaseTransaccionCompleta.commerce_code = "commercecode"
BaseTransaccionCompleta.api_key = "apikey"
BaseTransaccionCompleta.integration_type = IntegrationType.LIVE
```

```javascript
// Webpay Plus
WebpayPlus.configureForIntegration(commerceCode, apiKey);
WebpayPlus.configureForProduction(commerceCode, apiKey);
WebpayPlus.configureWebpayPlusForTesting();
WebpayPlus.configureWebpayPlusDeferredForTesting();
WebpayPlus.configureWebpayPlusMallForTesting();
WebpayPlus.configureWebpayPlusMallDeferredForTesting();

// Oneclick
Oneclick.configureForIntegration(commerceCode, apiKey);
Oneclick.configureForProduction(commerceCode, apiKey);
Oneclick.configureOneclickMallForTesting();
Oneclick.configureWebpayPlusMallDeferredForTesting();

// Transacción Completa
TransaccionCompleta.configureForIntegration(commerceCode, apiKey);
TransaccionCompleta.configureForProduction(commerceCode, apiKey);
TransaccionCompleta.configureTransaccionCompletaForTesting();
TransaccionCompleta.configureTransaccionCompletaNoCvvForTesting();
TransaccionCompleta.configureTransaccionCompletaDeferredForTesting();
TransaccionCompleta.configureTransaccionCompletaDeferredNoCvvForTesting();
TransaccionCompleta.configureTransaccionCompletaMallForTesting();
TransaccionCompleta.configureTransaccionCompletaMallNoCvvForTesting();
TransaccionCompleta.configureTransaccionCompletaMallDeferredForTesting();
TransaccionCompleta.configureTransaccionCompletaMallDeferredNoCvvForTesting();
```

### C) Utilizando el API

Si estás consumiendo el API directamente, solo debes de preocuparte de usar el
[host correspondiente al ambiente de producción](/referencia/webpay#ambiente-de-produccion), el código de comercio productivo y llave
secreta obtenida en el proceso de validación.

## Requerimientos de página de resultado

### Webpay Plus

<aside class="notice">
Para Webpay Plus REST, a diferencia de la versión anterior (SOAP), ya no se cuenta con una página de Voucher de Transbank.
De esta forma, al finalizar el proceso de pago, el usuario llega directamente al sitio del comercio, en donde este último
debe presentarle un comprobante de pago (una pantalla donde quede claro que el pago fue exitoso o fallido).  
</aside>

**La página de resultado** de comercio, es la página que muestra el comercio
cuando Webpay le entrega el control, después del proceso de autorización. Aplica para todos los tipos de transacciones.

**Una vez finalizada a transacción**, el comercio debe presentar una página al
tarjetahabiente para que este se informe del resultado de la transacción. La
información a presentar dependerá de si la transacción fue autorizada o no.

Se recomienda, como mínimo, que posea:

* Número de orden de pedido
* Nombre del comercio (Tienda de Mall)
* Monto y moneda de la transacción
* Código de autorización de la transacción
* Fecha de la transacción
* Tipo de pago realizado (Débito o Crédito)
* Tipo de cuota
* Cantidad de cuotas
* Monto de cada cuota
* Cuatro últimos dígitos de la tarjeta bancaria
* Descripción de los bienes y/o servicios

**Cuando la transacción no sea autorizada**, se recomienda informar al tarjetahabiente al respecto. Puede presentar un texto explicativo como:

```bash
Orden de Compra XXXXXXX rechazada

Las posibles causas de este rechazo son:
* Error en el ingreso de los datos de su tarjeta de crédito o débito (fecha y/o código de seguridad).
* Su tarjeta de crédito o débito no cuenta con saldo suficiente.
* Tarjeta aún no habilitada en el sistema financiero.
```

## Productos disponibles

 Los siguientes productos están disponibles para que puedas realizar la integración. Revisa su documentación acá:
  
* [Webpay Plus](/referencia/webpay#webpay-plus)
* [Oneclick Mall](/referencia/oneclick)
* [Transacción Completa](/referencia/webpay#transaccion-completa)

## Seguridad

Los servicios Web de Transbank están protegidos para garantizar que solamente
miembros autorizados por Transbank hagan uso de las operaciones disponibles.

El mecanismo de seguridad implementado está basado en un canal de comunicación
seguro TLS 1.2 sumado a WS-Security (para servicios SOAP) o API Keys + Mensajes
firmados (para servicios REST). Esto provee autenticación, confidencialidad e
integridad a los Servicios Web.

Los plugins y SDK para Webpay que distribuye Transbank ya están construidos con
las librerías necesarias para realizar las validaciones requeridas, pero es
deber del comercio asegurarse que la solución o desarrollo de medio de pago que
utilice, cumpla con los protocolos de seguridad.

## Deberes del Comercio

### Actualizaciones de plugins y SDK

Si el comercio está utilizando una solución basada en Plugins o SDK, debe
estar atento a las actualizaciones que periódicamente Transbank realizará.
Estas actualizaciones pueden responder a mantener compatibilidad con los CMS o
Shopping Cart, modificaciones por seguridad, adición de propiedades o
funciones, o correcciones a las comunicaciones. La comunicación oficial siempre
se realizará a través del sitio <http://www.transbankdevelopers.cl>.

### Uso de HTTPS

Los servidores del comercio tanto en ambientes de
integración como en ambiente de producción deben hacer uso
de HTTPS para sus _endpoints_ web (tanto de cara al
tarjetahabiente como en los _callbacks_ expuestos a
Transbank). El no uso de HTTPS puede provocar problemas en
las redirecciones en navegadores modernos (ej: Safari en iOS
11.3 o superior), impidiendo que se complete el flujo de
pago.

<aside class="warning">
**Advertencia**: A partir del 1 de Diciembre de 2018 Transbank no aceptará
integraciones que no operen con HTTPS.
</aside>

### Validación de montos y órdenes de compra

El comercio debe verificar al completar cualquier transacción que los valores
informados por Transbank (monto de la compra, _buyOrder_, etc.) coinciden con los
valores entregados por el comercio al principio del flujo transaccional.

<aside class="notice">
Esta nueva documentación hace referencia a los nuevos servicios REST de Transbank.
Si deseas revisar la documentación de los productos SOAP,
[haz click aquí](/documentacion/como_empezar_soap)
</aside>

<div class="container slate">
  <div class='slate-after-footer'>
    <div class='row d-flex align-items-stretch'>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22 text-center'>¿Tienes alguna duda de integración?</h3>
        <a href='https://join-transbankdevelopers-slack.herokuapp.com/' target='_blank'>
          <div class='td_block_gray'>
            <img src="https://p9.zdassets.com/hc/theme_assets/138842/200037786/logo.png" alt="" >
            <div class='td_pa-txt'>
              Únete a la comunidad de integradores en Slack y comparte buenos tips ayudando a los nuevos o buscando soluciones alternativas. Nuestro equipo está ahí para ayudarte.
            </div>
          </div>
        </a>
      </div>
      <div class='mt-3 mt-lg-0 col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22 text-center'>Si aún tienes dudas envíanos un mensaje</h3>
        <a class="pointer magenta" data-toggle='modal' data-target='#modalContactForm'>
          <div class='td_block_gray'>
            <div class="fo-size-20 text-center sub-title_bloq"><i class="fas fa-envelope"></i> Envíanos un mensaje.</div>
            <div class='td_pa-txt'>
              Si necesitas resolver algún tipo de incidencia en el portal o si existe algún problema con tu integración y  que no has podido resolver, contáctanos a través de nuestro formulario.
            </div>
          </div>
        </a>
      </div>
    </div>
  </div>
</div>
