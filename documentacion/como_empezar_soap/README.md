# Cómo empezar

Para empezar a integrar los productos de Transbank, te recomendamos usar
nuestros SDK, disponibles para múltiples lenguajes de programación y
plataformas. En general, existe un único Transbank SDK para el _backend_
de tu e-commerce, el cual te permite operar con todos nuestros productos.

Adicionalmente, existen SDKs especializados para algunos productos y
plataformas que requieren integraciones más allá del _backend_, como pasa por
ejemplo con Onepay.

En esta sección veremos los pasos para comenzar con el SDK que corresponda al
lenguaje de programación que utilices en tu backend. Para los SDKs
específicos a Onepay, visita la sección dedicada a ese producto.

## Flujo de Integración

Inicialmente el comercio tendrá algunas tareas necesarias que realizar mientras ocurre el proceso de integración. A continuación, puedes conocer el flujo completo.


<img class="td_img-night" src="/images/documentacion/flujo-integracion.svg" alt="Flujo de integración">

## Instalación SDK

Para instalar el SDK, debes agregarlo al gestor de dependencias de tu lenguaje:

[En
**Java**](https://github.com/TransbankDevelopers/transbank-sdk-java#instalaci%C3%B3n)
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
composer require transbank/transbank-sdk
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK PHP](https://github.com/TransbankDevelopers/transbank-sdk-php#instalaci%C3%B3n) para más opciones de instalación.

[En
**Node.js**](https://github.com/TransbankDevelopers/transbank-sdk-nodejs#instalaci%C3%B3n)
se debe instalar por NPM o Yarn, ejecutando esto en la línea de comandos cuando estés en la raíz de tu proyecto:

NPM
```bash
npm install transbank-sdk --save
```

además, necesitar tener intalado openssl de manera global

```bash
npm install -g openssl
```

Yarn
```bash
yarn add transbank-sdk
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK Node.js](https://github.com/TransbankDevelopers/transbank-sdk-nodejs#instalaci%C3%B3n) para más opciones de instalación.

[En
**.NET**](https://github.com/TransbankDevelopers/transbank-sdk-dotnet#instalaci%C3%B3n)
puedes instalar el SDK desde la línea de comandos del Package Manager de Visual
Studio:

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

(Para Webpay en Ruby puedes seguir usando [libwebpay](https://github.com/TransbankDevelopers/libwebpay-ruby) u otra alternativa)

[En
**Python**](https://github.com/TransbankDevelopers/transbank-sdk-python#instalaci%C3%B3n) puedes instalar el SDK desde PyPI:

```bash
pip install transbank-sdk
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK Python](https://github.com/TransbankDevelopers/transbank-sdk-python#instalaci%C3%B3n) para más opciones de instalación.

(Para Webpay en Python puedes seguir usando [libwebpay](https://github.com/TransbankDevelopers/libwebpay-python), pero te recomendamos usar [python-tbk, creada por Cornershop](https://github.com/cornershop/python-tbk) que será la base de lo que integremos finalmente en transbank-sdk)

## Ambientes

Transbank provee dos ambientes: **Integración** y **Producción**.

**Ambiente de Integración**: En este ambiente el comercio realiza la integración del producto a contratar y testea 
su solución de medio pago.

**Ambiente de producción**: En este ambiente el comercio operará luego de finalizar el [proceso de puesta en producción](#puesta-en-produccion) y realizará transacciones 
con tarjetas de crédito o débito **reales**.  

### Ambiente de integración

Para las transacciones Webpay en estos ambientes se deben usar estas
tarjetas:

- VISA 4051885600446623, CVV 123, cualquier fecha de expiración. Esta tarjeta
genera transacciones aprobadas.
- MASTERCARD 5186059559590568, CVV 123, cualquier fecha de expiración. Esta
tarjeta genera transacciones rechazadas.
- Redcompra 4051884239937763 genera transacciones aprobadas (para operaciones
que permiten débito Redcompra y prepago)
- Redcompra 5186008541233829 genera transacciones rechazadas (para operaciones
que permiten débito Redcompra y prepago)

Cuando aparece un formulario de autenticación con RUT y clave, se debe
usar el RUT 11.111.111-1 y la clave 123.

En cuanto al comercio, en este ambiente puedes usar las credenciales genéricas
provistas por Transbank que te permiten hacer pruebas rápidamente (pues vienen pre-configuradas en nuestros SDKs). Si tienes prisa, puedes ir directo a ver
como probar nuestros productos en este ambiente:

- [Webpay Plus](webpay#webpay-plus)
- [Webpay OneClick](webpay#webpay-oneclick)
- [Onepay Checkout](onepay#integracion-checkout)

<aside class="notice">
Tip: Los SDK y plugins provistos por Transbank tiene pre-configuradas
las credenciales para el ambiente de integración. Puedes ver como usarlas
en la sección de [documentación de los SDK](https://www.transbankdevelopers.cl/documentacion/webpay#webpay-plus) y [Plugins](https://www.transbankdevelopers.cl/plugin)
</aside>

En el caso de requerir los códigos de comercio de integración son los siguientes:

Producto | Código de comercio
------   | -----------
Webpay Plus | 597020000540
webpay PLus Mall <br> <i>tienda 1</i> <br> <i>tienda 2</i> | 597044444401 <br> 597044444402 <br> 597044444403
Webpay Plus Captura Diferida | 597044444404
Oneclick | 597044444405
Oneclick Mall <br> <i>tienda 1</i> <br> <i>tienda 2</i> | 597044444429 <br> 597044444430 <br> 597044444431

Para mayor detalle de las credenciales puedes visitar [el repositorio GitHub
`transbank-webpay-credenciales`](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/)


<aside class="notice">
Tip: Cada uno de estos ambientes maneja distintas URLs (endpoints) y
distintos códigos de comercios. Tenlo presente cuando hagas cambios de un
ambiente a otro.
</aside>

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

### Actualizaciones de plugins y SDK

Si el comercio está utilizando una solución basada en Plugins o SDK, debe
estar atento a las actualizaciones que periódicamente Transbank realizará.
Estas actualizaciones pueden responder a mantener compatibilidad con los CMS o
Shopping Cart, modificaciones por seguridad, adición de propiedades o
funciones, o correcciones a las comunicaciones. La comunicación oficial siempre
se realizará a través del sitio <http://www.transbankdevelopers.cl>.

### Uso de HTTPS

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

### Validación de firmas

Todo comercio que utiliza Webpay mediante servicios web **DEBE** velar por la
seguridad de las transacciones, bajo el esquema del servicio ofertado por
Transbank. En cada operación que exista una firma electrónica es **OBLIGACIÓN**
de la parte receptora validar dicha firma.

### Validación de montos y órdenes de compra

El comercio debe verificar al completar cualquier transacción que los valores
informados por Transbank (monto de la compra, _buyOrder_, etc.) coinciden con los
valores entregados por el comercio al principio del flujo transaccional.

## Puesta en Producción

1. Una vez que el comercio determine que ha finalizado su integración, se debe realizar un [proceso de validación](#el-proceso-de-validacion-y-puesta-en-produccion). Si realizaste la integración con un plugin, considera que junto con la planilla de integración, debes generar tus credenciales y enviar el certificado público y el logo (GIF, 130x59) a soporte@transbank.cl. 

2. Posterior a que Transbank confirme que la planilla de integración se encuentra correcta (no aplica para plugins), se solicitará al comercio la [generación de las credenciales](#credenciales-en-onepay) (llave privada y certificado publico). El certificado público debe ser enviado junto al logo del comercio (GIF, 130x59) a soporte@transbank.cl para su registro. 

3. Transbank informará via correo electrónico el correcto registro del certificado público enviado por el comercio. Posterior a ello, será necesario [cambiar la configuración del e-commerce para funcionar en producción](#configuracion-para-produccion-utilizando-los-sdk)

4. Con la configuración del ambiente de producción ya lista, será necesario realizar una compra de $10 para validar el correcto funcionamiento.

### Credenciales en Onepay

En el caso de Onepay las credenciales consisten en:

- Un API Key
- Un secreto ("shared secret").

Estos valores serán provistos por Transbank y en su conjunto permiten hacer
transacciones a nombre del comercio. **Debes custodiar estas credenciales para
evitar que caigan en manos de terceros**.

### Credenciales en Webpay

En el caso de Webpay, las credenciales consisten en:

- Un código de comercio.
- Una llave privada.
- Un certificado público.

La llave y el certificado (autofirmado) deben ser generados por el comercio,
con la condición de que el common name del certificado debe coincidir con el
código de comercio indicado por Transbank.

Para generar estas credenciales tendrás que estar en una línea de comandos y
disponer de OpenSSL versión 1.0. Los pasos son los siguientes (debes reemplazar
"597029124456" por tu código de comercio):

1. Crear llave privada:

```bash
openssl genrsa -out 597029124456.key 2048
```

2. Crear requerimiento de certificado:

```bash
openssl req -new -key 597029124456.key -out 597029124456.csr

Country Name (2 letter code) []:CL
State or Province Name (full name) []:
Locality Name (eg, city) []:SANTIAGO
Organization Name (eg, company) []:
Organizational Unit Name (eg, section) []:
Common Name (eg, your name or your server’s hostname) []:597029124456
Email Address []:
Please enter the following ‘extra’ attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

<aside class="notice">
Tip: Nota como en el ejemplo se ha ingresado un código de comercio en el
momento en que openssl solicita un "Common Name". Acá debes ingresar el código
de comercio de **producción** que te entregó Transbank. Asegúrate de que el código de comercio tenga antepuesto el número 5970, como en el ejemplo superior. Además, es importante no ingresar caracteres especiales en los otros campos ("Organization Name", "Locality Name", etc).
</aside>

3. Crear certificado autofirmado:

```bash
openssl x509 -req -days 1460 -in 597029124456.csr -signkey 597029124456.key -out 597029124456.crt
```

<aside class="notice">
Se recomienda que los certificados públicos tengan vigencia
de al menos 4 años (1460 días).
</aside>

Finalmente debes enviar a Transbank el certificado público (`597029124456.crt`)
y guardar el certificado y también su llave privada (`597029124456.key`), los
que junto a tu código de comercio te permitirán transaccionar. **Debes custodiar
esa llave privada para evitar que caiga en manos de terceros**.

<aside class="warning">
A diferencia de otros SDK, en .NET debes especificar la ruta a un archivo `.pfx` o `.p12`
el cual debes generar tu a partir de tu llave privada y certificado público.

Puedes mirar el siguiente enlace para obtener una guía rápida de como generar tu
propio archivo: [Crear archivo pfx usando openssl](https://www.ssl.com/how-to/create-a-pfx-p12-certificate-file-using-openssl/)
`openssl pkcs12 -export -out 597029124456.pfx -inkey 597029124456.key -in 597029124456.crt`
</aside>

### El proceso de validación y puesta en producción

Durante la validación de la integración se pretende verificar que el comercio
transacciona de manera segura y sin problemas, por lo que se solicitarán una
serie de pruebas y su posterior envío de evidencias para validar la
integración. Esta validación es requisito necesario para dejar al comercio en
producción y no se permitirá que un comercio utilice productivamente el
servicio Webpay sin poseer una validación.

Transbank solo validará las integraciones de aquellos comercios que tengan un código de comercio productivo. 
Para obtenerlo, sigue las instrucciones en cómo hacerse cliente en el portal <http://www.transbank.cl> o contacte a su
ejecutivo comercial.


En esta etapa, el comercio envía las evidencias a soporte@transbank.cl empleando el formulario correspondiente al 
producto integrado indicando claramente las órdenes de compra, fecha y hora de las transacciones. Para integraciones 
Webpay que utilicen algún [plugin oficial](/plugin) existe un formulario especial.
 
Descargar el formulario de envidencias... 
- Para integraciones Webpay: [**Descargar**](https://transbankdevelopers.cl/files/evidencia-integracion-webpay.docx)
- Para integraciones Webpay que usen un plugin oficial: [**Descargar**](https://transbankdevelopers.cl/files/evidencia-integracion-webpay-plugins.docx)
- Para integraciones Onepay: [**Descargar**](https://transbankdevelopers.cl/files/evidencia-de-integracion-onepay.docx)

Soporte validará que los casos de prueba sean consistentes con los registrados
en los sistemas de Webpay y, de estar todo correcto, se le notificará al
comercio la conformidad para pasar a producción, recibiendo las instrucciones
para ello. De no estar consistentes las pruebas, se le hará alcances al
comercio respecto de su integración, para que realices las correcciones
correspondientes y vuelvas a enviar las evidencias una vez terminadas dichas
correcciones.

<aside class="notice">
Sabemos que este proceso manual puede ser tedioso y consumir más tiempo del
estrictamente necesario mientras una parte espera las respuestas o feedback de
la otra. **Por eso próximamente lanzaremos un portal de validación completamente
online** y auto-atendido en que podrás realizar este proceso mucho más rápido.
</aside>

Una vez que soporte te comunique formalmente que la integración está aprobada,
deberá seguir los pasos que le indican para pasar a producción y poder
comenzar a transaccionar de manera real.

Estos pasos incluyen cambio en el ambiente sobre el que transacciona, pasando de
integración a producción, además de las instrucciones de creación de
credenciales.

Durante el paso a producción se te exigirá realizar, al menos, una
transacción de prueba real, con la que finalizará oficialmente la puesta en
producción.

<aside class="warning">
Importante: Es responsabilidad del comercio considerar que el certificado
público que Transbank comparte con los comercios (y que es incluido en los SDKs)
para integraciones de Webpay tiene una fecha de caducidad, como
asimismo el certificado que el comercio genera y que comparte con Transbank para
realizar las transacciones sobre Webpay. El comercio es responsable por
resguardar su llave privada y su certificado público, como asimismo es
responsable por reemplazar estos cuando caduquen.
</aside>

### Configuración para producción utilizando los SDK

Si estás utilizando algún SDK oficial de Transbank y quieres pasa al ambiente de producción será necesario seguir los siguiente pasos.

1. Remover la configuración para el ambiente de pruebas

Antes de crear la nueva configuración para el ambiente de producción será necesario eliminar la actual comfiguración para el ambiente de pruebas que cumple con el siguiente formato `Configuration.ForTestingWebpayPlusNormal()`

2. Crear un nuevo elemento `Configuration`

<div class="language-simple" data-multiple-language></div>

<img src="/images/documentacion/configuracion/1-configuration-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/1-configuration-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/1-configuration-net.gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

```javascript
const configuration = new Transbank.Configuration()
```

3. Asignar el código de comercio productivo, entregado por Transbank al momento de contratar el producto.

<img src="/images/documentacion/configuracion/2-codigo-comercio-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/2-codigo-comercio-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/2-codigo-comercio-net.gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

```javascript
const configuration = new Transbank.Configuration()
                        .withCommerceCode(/* tu código de comercio */)
```

4. Configuración de la llave privada (`.key` para Java y PHP `.pfx` o `.p12` para .NET) 
generada por el comercio en la etapa previa.

<img src="/images/documentacion/configuracion/3-private-key-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/3-private-key-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/3-private-key-net.gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

```javascript
const configuration = new Transbank.Configuration()
                        .withCommerceCode(/* tu código de comercio */)
                        .withPrivateCert(/* tu certificado privado en forma de string */)
```

5. Configuración del certificado público (`.crt`) enviado a Transbank para su registro. En el caso de .NET no es 
necesario configurar el `.crt` pero se debe configurar el password asignado a la llave privada.

<img src="/images/documentacion/configuracion/4-certificate-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/4-certificate-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/4-certificate-net(password).gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

```javascript
const configuration = new Transbank.Configuration()
                        .withCommerceCode(/* tu código de comercio */)
                        .withPrivateCert(/* tu certificado privado en forma de string */)
                        .withPublicCert(/* tu certificado público en forma de string */)
```

6. Selección del ambiente productivo.

<img src="/images/documentacion/configuracion/5-environment-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/5-environment-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/5-environment-net.gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

```javascript
const configuration = new Transbank.Configuration()
                        .withCommerceCode(/* tu código de comercio */)
                        .withPrivateCert(/* tu certificado privado en forma de string */)
                        .withPublicCert(/* tu certificado público en forma de string */)
                        .usingEnvironment(Transbank.environments.production) // Se define el ambiente como producción
```

7. Crear elemento Webpay utilizando la configuración de producción.

<img src="/images/documentacion/configuracion/6-webpay-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/6-webpay-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
<img src="/images/documentacion/configuracion/6-webpay-net.gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

```javascript
const configuration = new Transbank.Configuration()
                        .withCommerceCode(/* tu código de comercio */)
                        .withPrivateCert(/* tu certificado privado en forma de string */)
                        .withPublicCert(/* tu certificado público en forma de string */)
                        .usingEnvironment(Transbank.environments.production) // Se define el ambiente como producción

const transaction = new Transbank.Webpay(configuration);
```

## Requerimientos de páginas de transición y de fin de transacción

### Webpay

**La página de transición** de comercio, es la página que muestra el comercio
cuando Webpay le entrega el control, después del proceso de autorización y
previo a redirigir al tarjeta habiente al comprobante de éxito de la
transacción. Aplica para todos los tipos de transacciones.

**Una vez finalizada a transacción**, el comercio debe presentar una página al
tarjetahabiente para que este se informe del resultado de la transacción. La
información a presentar dependerá de si la transacción fue autorizada o no.

Se recomienda, como mínimo, que posea:

- Número de orden de Pedido
- Nombre del comercio (Tienda de Mall)
- Monto y moneda de la transacción
- Código de autorización de la transacción
- Fecha de la transacción
- Tipo de pago realizado (Débito o Crédito)
- Tipo de cuota
- Cantidad de cuotas
- 4 últimos dígitos de la tarjeta bancaria
- Descripción de los bienes y/o servicios

**Cuando la transacción no sea autorizada**, se recomienda informar al tarjetahabiente al respecto. Puede presentar un texto explicativo como:

```bash
Orden de Compra XXXXXXX rechazada
Las posibles causas de este rechazo son:
* Error en el ingreso de los datos de su tarjeta de Crédito o Débito (fecha y/o código de seguridad).
* Su tarjeta de Crédito o Débito no cuenta con saldo suficiente.
* Tarjeta aún no habilitada en el sistema financiero.
```


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
