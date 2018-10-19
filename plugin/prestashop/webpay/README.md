<script>$(function () {$('[data-toggle="popover"]').popover()});</script>

<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugins.</h4>
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

<h1 class="toc-ignore">Webpay Prestashop</h1>
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

1. Una vez descargado el plugin ingresa al panel de administración de PrestaShop a la opción de **"Módulos y Servicios**.

<img src="/images/plug/prestashop/webpay/01.png" class="rounded mx-auto d-block">

2. Dentro de **"Módulos y Servicios"** ingresa a **"Añadir nuevo módulo"** y selecciona el plugin recién descargado:

<img src="/images/plug/prestashop/webpay/02.png" class="rounded mx-auto d-block">

<img src="/images/plug/prestashop/webpay/03.png" class="rounded mx-auto d-block">

<img src="/images/plug/prestashop/webpay/04.png" class="rounded mx-auto d-block">

## Configuración

1. Una vez instalado el plugin de Webpay Plus es momento de configurarlo. Para lo cual ingresa a **"Módulos y Servicios → Módulos instalados → WebPay Plus (CONFIGURAR)"**

<img src="/images/plug/prestashop/webpay/05.png" class="rounded mx-auto d-block"/>

### Observaciones
<dl>
  <dt>Los ambientes que encontrarás en la configuración del plugin son:</dt>

  <dd>**Integración**: Modo de prueba, tanto código de comercio, llaves y certificados, vienen dadas por defecto en el plugin y te permitirá generar simulaciones de compra para verificar la conexión con webpay de Transbank.</dd>

  <dd>**Certificación**: Etapa en la que Transbank está validando tu e-commerce (proceso QA).</dd>

  <dd>**Producción**: Una vez que Transbank certifica tu e-commerce, estarás en condiciones de vender a través de tu sitio.</dd>
</dl>

Para acceder a los ambientes de Certificación y Producción, debes afiliar tu comercio a Transbank. Una vez realizado, se te entregará un código de comercio, con el que deberás crear la llave y el certificado público. El certificado público debes enviarlo a Transbank para validar tu comercio.

Para conocer los requisitos de afiliación.  <a href="https://portaltransbank.cl/afiliacion/" target="blank">https://portaltransbank.cl/afiliacion/</a>

<img src="/images/plug/prestashop/webpay/06.png" class="rounded mx-auto d-block"/>

- Si presionamos el botón **"Información"** tendremos acceso a las herramientas de diagnóstico y de registro.

**Información**

En la pestaña **"Información"** encontraremos información básica de nuestro servidor, ecommerce y del plugin Webpay instalado. También con el botón **"Crear PDF"** generamos un informe en formato pdf con información útil para el soporte y diagnóstico de problemas.

<img src="/images/plug/prestashop/webpay/07.png" class="rounded mx-auto d-block"/>

**PHP Info**

En la pestaña **"PHP info"** nos permite ver el reporte php_info generado por el servidor, con información útil para el soporte y diagnóstico de problemas. Con el botón **"Crear PHP info"** generamos este reporte en formato PDF.

<img src="/images/plug/prestashop/webpay/08.png" class="rounded mx-auto d-block"/>

**Registros**

En la pestaña **"Registros"** Podemos activar, desactivar y configurar el tiempo y peso de los registros de comunicación con Transbank a guardarse. También se nos permite descargarlos y ver el último registro.

<img src="/images/plug/prestashop/webpay/09.png" class="rounded mx-auto d-block"/>

### Ejemplo de Compra

A modo de ejemplo realizaremos la compra de un producto de una tienda y su interacción con el Plugin de Webpay Plus.

<img src="/images/plug/prestashop/webpay/10.png" class="rounded mx-auto d-block"/>

Como vemos Webpay Plus se encuentra disponible como medio de pago dentro del eCommerce creado gracias al plugin instalado.

<img src="/images/plug/prestashop/webpay/11.png" class="rounded mx-auto d-block"/>

Al realizar el pedido, nos aparecerá un pequeño resumen, previo a generar el inicio del pago.

<img src="/images/plug/prestashop/webpay/12.png" class="rounded mx-auto d-block"/>

El siguiente paso es la presentación del formulario de Webpay Plus, lo que inicia la interacción entre Transbank y tu ECommerce

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

Para presentar el voucher de WebPay Plus con el detalle de la transacción exitosa.

<img src="/images/plug/webpay_form/form_05.png" class="rounded mx-auto d-block"/>

Para finalizar con la presentación en el eCommerce del detalle de la transacción.

<img src="/images/plug/prestashop/webpay/16.png" class="rounded mx-auto d-block"/>
