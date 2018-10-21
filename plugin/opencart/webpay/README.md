<script>$(function () {$('[data-toggle="popover"]').popover()});</script>

<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    {% for key, x in tool %}
      {% if x.fileActive %}
        <span class="btn-download" data-target='#modalDownloadPlugins' id="press-p-btn-{{x.ident}}"
              data-plugin="download?type_d=plugin_v&f={{x.fileActive}}&v={{x.ident}}&tool={{x.tool}}&type_t={{x.type_t}}&pr={{x.pr}}">
          <b class="td_btn-more sm">Versión {{x.version}}</b>
        </span>
        <h4>Compatibilidad</h4>
        <ul>
          <li>{{x.ecommerce}}</li>
          <li>{{x.lenguaje}}</li>
          <li><span data-container="body" data-toggle="popover" data-placement="top" data-content="{{x.md5}}">md5</span></li>
          <li><span data-container="body" data-toggle="popover" data-placement="top" data-content="{{x.sha1}}">sha1</span></li>
        </ul>
      {% endif %}
    {% endfor %}
    <span class="btn-download top-x2 bottom-x2" data-toggle="modal" data-target="#modalChangelogPlugins"><b>Changelog</b></span>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu llave privada y pública</li>
      <li>Tener correctamente configurado el Ecommerce</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Webpay Opencart</h1>
<h1 style="display: none;">Webpay</h1>

## Requisitos
Tener instalado y configurado previamente tu E-commerce.

Habilitar los siguientes modulos para PHP:
+ Soap
+ OpenSSL 1.0.1 o superior
+ SimpleXML
+ DOM 2.7.8 o superior
+ Mcrypt
+ Sockets
+ Único protocolo de comunicación aceptado: TLS 1.2

## Instalación

1. Una vez descargado el Plugin, ingresa al panel de administración de extensiones de Opencart ubicado en la ruta **"Extensions → Extension Installer"**. El botón Upload nos permitirá subir nuestros archivos al servidor.

<img src="/images/plug/open/webpay/01.png" class="rounded mx-auto d-block">

2. Recuerda tener activado y configurado correctamente tú FTP para que la subida de archivos se realice correctamente. Esto lo podemos verificar ingresando desde nuestro panel de administración en la ruta **"System → Settings → Edit Setting → FTP"**.

<img src="/images/plug/open/webpay/02.png" class="rounded mx-auto d-block">

3. Opencart puede presentar ciertas dificultades con algunas configuraciones del servidor de FTP, por lo que te recomendamos, para casos como el de la imagen a continuación, deshabilitar el soporte FTP mediante una extensión gratuita llamada QuickFix, la cual puedes descargar desde el siguiente enlace [http://www.opencart.com/](http://www.opencart.com/)

<img src="/images/plug/open/webpay/03.png" class="rounded mx-auto d-block">

La instalación se realiza de la misma forma que los otros Plugins, la diferencia está en que QuickFix es un archivo XML que no presenta problemas al hacer el upload, además deshabilita el soporte FTP de Opencart, por lo que los Plugins que se suban posteriormente no deberían presentar problemas.

4. Al instalar la extensión deberíamos verla instalada en la ruta **"Extension Installer → Modifications"**

<img src="/images/plug/open/webpay/04.png" class="rounded mx-auto d-block"/>

5. Una vez instalado el Plugin, debemos activar y configurar nuestro plugin desde la ruta: **"Extensions → Payments → Install"**

<img src="/images/plug/open/webpay/05.png" class="rounded mx-auto d-block"/>

## Configuración

### Observaciones
<dl>
  <dt>Los ambientes que encontrarás en la configuración del plugin son:</dt>

  <dd>**Integración**: Modo de prueba, tanto código de comercio, llaves y certificados, vienen dadas por defecto en el plugin y te permitirá generar simulaciones de compra para verificar la conexión con webpay de Transbank.</dd>

  <dd>**Certificación**: Etapa en la que Transbank está validando tu e-commerce (proceso QA).</dd>

  <dd>**Producción**: Una vez que Transbank certifica tu e-commerce, estarás en condiciones de vender a través de tu sitio.</dd>
</dl>

Para acceder a los ambientes de Certificación y Producción, debes afiliar tu comercio a Transbank. Una vez realizado, se te entregará un código de comercio, con el que deberás crear la llave y el certificado público. El certificado público debes enviarlo a Transbank para validar tu comercio.

Para conocer los requisitos de afiliación.  <a href="https://portaltransbank.cl/afiliacion/" target="blank">https://portaltransbank.cl/afiliacion/</a>

Para verificar la configuración debemos ingresar desde el panel de administración siguiendo la ruta Extensions → Payments → Edit.

<img src="/images/plug/open/webpay/06.png" class="rounded mx-auto d-block"/>

Si te encuentras en la primera etapa (Integración) y sólo deseas verificar que tu instalación funciona correctamente, deberás utilizar la información que la extensión trae por defecto y verificar que los campos, **total, estado completado, estado rechazado, Zona Geográfica, Estado** (Enabled habilita el Plugin) y **Orden** correspondan a tus preferencias. Luego debes guardar la configuración. Con lo anterior estarás listo para realizar las primeras pruebas de compra en tu tienda OnLine.

<img src="/images/plug/open/webpay/07.png" class="rounded mx-auto d-block"/>

<dl>
  <dt>Descripción de Opciones: </dt>

  <dd>**Código de Comercio, Llave Privada, Certificado, Certificado Transbank, Ambiente**: Información proporcionada por Transbank para ambiente Certificación o Producción.</dd>

  <dd>**Total**: Compra mínima para que aparezca como medio de pago.</dd>

  <dd>**Estado Completado**: Corresponde al estado con el que quedará la orden, al momento de ser finalizada.</dd>

  <dd>**Estado rechazado**: Corresponde al estado con el que quedará la orden, al momento de ser rechazada.</dd>

  <dd>**Estado Anulado**: Corresponde al estado con el que quedará la orden, al momento de ser anulada.</dd>

  <dd>**Zona geográfica**: All Zones</dd>

  <dd>**Estado**: Permite activar o desactivar el medio de pago.</dd>

  <dd>**Orden**: Orden en el que aparecerá el medio de pago como opción.</dd>
</dl>

### Ejemplo de Compra

A modo de ejemplo realizaremos la compra de un producto de una tienda y su interacción con el Plugin de Webpay Plus, bajo ambiente de Integración.

<img src="/images/plug/open/webpay/08.png" class="rounded mx-auto d-block"/>

Como se puede verificar en la imagen, el **"Pago con Tarjetas de Crédito o Redcompra"** se encuentra disponible como medio de pago dentro del eCommerce.

<img src="/images/plug/open/webpay/09.png" class="rounded mx-auto d-block"/>

El siguiente paso es la presentación del formulario de Webpay Plus, lo que inicia la interacción entre Transbank y tu ECommerce.

<img src="/images/plug/webpay_form/form_01.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/webpay_form/form_02.png" class="rounded mx-auto d-block"/>

Luego se presenta el formulario del emisor correspondiente a la tarjeta ingresada para validar las claves del usuario. Los datos a ingresar para finalizar la compra son:
+ Rut: 11.111.111-1
+ Clave: 123

Si lo haces con Tarjeta de Crédito, ingresa en el campo asignado los siguientes datos:
+ Nº tarjeta: 4051885600446623
+ Rut: 11.111.111-1
+ Clave: 123
+ Vencimiento: Cualquiera
+ Sin cuotas

<img src="/images/plug/webpay_form/form_03.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/webpay_form/form_04.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/webpay_form/form_05.png" class="rounded mx-auto d-block"/>

Finalmente vemos el Voucher de WebPay Plus con el detalle de la transacción exitosa por parte de Webpay, el que al cabo de algunos segundos nos enviará al resumen de compra de tu eCommerce.

<img src="/images/plug/open/webpay/14.png" class="rounded mx-auto d-block"/>
