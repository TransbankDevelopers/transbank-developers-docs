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

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK Java](https://github.com/TransbankDevelopers/transbank-sdk-java#instalaci%C3%B3n) para mas opciones e información de la última versión disponible.

[En
**PHP**](https://github.com/TransbankDevelopers/transbank-sdk-php#instalaci%C3%B3n)
puedes usar composer para descargar la última versión del SDK,
ejecutando esto en la línea de comandos cuando estés en la raíz de tu proyecto:

```bash
composer require transbank/transbank-sdk
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK PHP](https://github.com/TransbankDevelopers/transbank-sdk-php#instalaci%C3%B3n) para mas opciones de instalación.


[En
**.NET**](https://github.com/TransbankDevelopers/transbank-sdk-dotnet#instalaci%C3%B3n)
puedes instalar el SDK desde la línea de comandos del Package Manager de Visual
Studio:

```
PM> Install-Package TransbankSDK
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK .NET](https://github.com/TransbankDevelopers/transbank-sdk-dotnet#instalaci%C3%B3n) para mas opciones de instalación.


[En 
**Ruby**](https://github.com/TransbankDevelopers/transbank-sdk-ruby#instalaci%C3%B3n) puedes instalar el SDK como una gema:

```bash
gem install transbank-sdk
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK Ruby](https://github.com/TransbankDevelopers/transbank-sdk-ruby#instalaci%C3%B3n) para mas opciones de instalación.

(Para webpay en Ruby puedes seguir usando [libwebpay](https://github.com/TransbankDevelopers/libwebpay-ruby) u otra alternativa)


[En 
**Python**](https://github.com/TransbankDevelopers/transbank-sdk-python#instalaci%C3%B3n) puedes instalar el SDK desde PyPI:

```bash
pip install transbank-sdk
```

Te recomendamos leer [las instrucciones de instalación detalladas para el SDK Python](https://github.com/TransbankDevelopers/transbank-sdk-python#instalaci%C3%B3n) para mas opciones de instalación.

(Para webpay en Python puedes seguir usando [libwebpay](https://github.com/TransbankDevelopers/libwebpay-python), pero te recomendamos usar [python-tbk, creada por Cornershop](https://github.com/cornershop/python-tbk) que será la base de lo que integremos finalmente en transbank-sdk)


## Ambientes

Transbank provee dos ambientes para todos sus productos:

**Ambiente de Integración y Validación**: En este ambiente el comercio realiza
la integración a Webpay y testea su solución de medio pago. Asimismo, en éste
ambiente es que se valida la integración del comercio.

Para las transaccciones Webpay en estos ambientes se deben usar estas
tarjetas:

- VISA 4051885600446623, CVV 123, cualquier fecha de expiración. Esta tarjeta
genera transacciones aprobadas.
- MASTERCARD 5186059559590568, CVV 123, cualquier fecha de expiración. Esta
tarjeta genera transacciones rechazadas.
- Redcompra 4051885600446623 genera transacciones aprobadas (para operaciones
que permiten débito Redcompra)
- Redcompra 5186059559590568 genera transacciones rechazadas (para operaciones
que permiten débito Redcompra)

Cuando aparece un formulario de autenticación con RUT y clave, se debe
usar el RUT 11.111.111-1 y la clave 123.

En cuanto al comercio, en este ambiente puedes usar las credenciales genéricas
provistas por Transbank que te permiten hacer pruebas rápidamente (pues vienen pre-configuradas en nuestros SDKs). Si tienes prisa, puedes ir directo a ver
como probar nuestros productos en este ambiente:

- [Webpay Plus](webpay#webpay-plus)
- [Webpay OneClick](webpay#webpay-oneclick)
- [Onepay Checkout](onepay#integracion-checkout)

Después de haber realizado esas pruebas iniciales y antes del paso a producción
tendrás que usar credenciales que identifiquen a tu comercio. De esa forma
podrás realizar [la validación que te permitirá acceder a credenciales de
producción](#el-proceso-de-validacion-y-puesta-en-produccion).


**Ambiente de producción**: Este ambiente es en el cual finalmente operará
productivamente el comercio. En este ambiente puede hacer pruebas con tarjetas
de crédito o débito reales. Las credenciales de este ambiente son entregadas
al momento que se coordina el paso a producción.

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
Shoppping Cart, modificaciones por seguridad, adición de propiedades o
funciones, o correcciones a las comunicaciones. La comunicación oficial siempre
se realizará a través del sitio http://www.transbankdevelopers.cl.

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
informados por Transbank (monto de la compra, _buyOrder_, etc) coinciden con los
valores entregados por el comercio al principio del flujo transaccional.

## Puesta en Producción

Para el paso a producción el comercio debe realizar un [proceso de validación](#el-proceso-de-validacion-y-puesta-en-produccion)
en el ambiente de integración. En este ambiente de integración Transbank le
solicitará usar credenciales específicas para el comercio, de manera de simular
lo mejor posible el ambiente productivo.

### Credenciales en Onepay

En el caso de Onepay las credenciales consisten en:

- Un API Key
- Un secreto ("shared secret").

Estos valores serán provistos por transbank y en su conjunto permiten hacer
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
disponer de OpenSSL. Los pasos son los siguientes (debes reemplazar
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
Common Name (eg, your name or your server’s hostname) []:*597029124456*
Email Address []:
Please enter the following ‘extra’ attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

<aside class="notice">
Tip: Nota como en el ejemplo se ha ingresado un código de comercio en el
momento en que openssl solicita un "Common Name". Acá debes ingresar el código
de comercio que te entregó Transbank.
</aside>

3. Crear certificado autofirmado:

```bash
openssl x509 -req -days 1460 -in 597029124456.csr \
             -signkey 597029124456.key -out 597029124456.crt
```

<aside class="notice">
Se recomienda que los certificados públicos tengan vigencia
de al menos 4 años (1460 días).
</aside>

Finalmente debes enviar a Transbank el certificado público (`597029124456.crt`)
y guardar el certificado y también su llave privada (`597029124456.key`), los
que junto a tu código de comercio te permitirán transaccionar. **Debes custodiar
esa llave privada para evitar que caiga en manos de terceros**.


### El proceso de Validación y puesta en Producción

Durante la validación de la integración se pretende verificar que el comercio
transacciona de manera segura y sin problemas, por lo que se solicitarán una
serie de pruebas y su posterior envío de evidencias para validar la
integración. Esta validación es requisito necesario para dejar al comercio en
producción y no se permitirá que un comercio utilice productivamente el
servicio Webpay sin poseer una validación.

Por otro lado, Transbank no validará ninguna integración a algún comercio que
no posea código de comercio productivo. Para obtenerlo, sigue las instrucciones
en cómo hacerse cliente en el portal http://www.transbank.cl o contacte a su
ejecutivo comercial.

En esta etapa, el comercio envía las evidencias a soporte@transbank.cl mediante
el formulario correspondiente para [**Webpay**](https://transbankdevelopers.cl/files/evidencia-integracion-webpay.docx)
o [**Onepay**](https://transbankdevelopers.cl/files/evidencia-de-integracion-onepay.docx), 
indicando claramente las órdenes de compra, fecha
y hora de las transacciones.

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

```
Orden de Compra XXXXXXX rechazada
Las posibles causas de este rechazo son:
* Error en el ingreso de los datos de su tarjeta de Crédito o Débito (fecha y/o código de seguridad).
* Su tarjeta de Crédito o Débito no cuenta con saldo suficiente.
* Tarjeta aún no habilitada en el sistema financiero.
```

## Pruebas de validación efectuada por Transbank

Pruebas de validación para Webpay Normal, modalidad plugin:

- Pago crédito exitoso sin cuotas
- Pago crédito exitoso con cuotas
- Pago crédito denegado
- Pago débito exitoso
- Pago débito denegado
- Pago cancelado (abortado en formulario Webpay)

Pruebas de validación para Webpay Normal:

- Pago crédito exitoso sin cuotas
- Pago crédito exitoso con cuotas
- Pago crédito denegado
- Pago débito exitoso
- Pago débito denegado
- Anulación parcial (solo si integra el método)
- Anulación total (solo si integra el método)
- Pago cancelado (abortado en formulario Webpay)

Pruebas de validación para Webpay Normal más captura diferida:

- Pago crédito exitoso sin cuotas
- Pago crédito exitoso con cuotas
- Pago crédito denegado
- Pago cancelado (abortado en formulario Webpay) - Anulación parcial (solo si
  integra el método)
- Anulación total (solo si integra el método)

Pruebas de validación para Transacción Mall:

- Pago crédito exitoso sin cuotas
- Pago crédito exitoso con cuotas
- Pago crédito denegado
- Pago débito exitoso
- Pago débito denegado
- Anulación parcial (solo si integra el método)
- Anulación total (solo si integra el método)
- Pago cancelado (abortado en formulario Webpay)

Pruebas de validación para Transacción OneClick:

- Inscripción rechazada
- Inscripción exitosa
- Autorización
- Reversa
- Remover usuario

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
