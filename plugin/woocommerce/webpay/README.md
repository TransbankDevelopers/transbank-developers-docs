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

<h1 class="toc-ignore">Webpay WooCommerce</h1>
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

1. Una vez descargado el plugin ingresa al panel de administración de WordPress y selecciona Plugins.
Dentro del módulo de "Plugins" selecciona Agregar nuevo.

<img src="/images/plug/woo/webpay/emu_01.png" class="rounded mx-auto d-block">

2. Se desplegará la siguiente pantalla y selecciona la opción subir plugin

<img src="/images/plug/woo/webpay/emu_02.png" class="rounded mx-auto d-block">

3. Busca el plugin recién descargado y selecciona **Instalar**

<img src="/images/plug/woo/webpay/emu_03.png" class="rounded mx-auto d-block">

4. Una vez cargado el plugin, aparecerá junto a los ya existentes y lo activamos.

<img src="/images/plug/woo/webpay/emu_04.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/woo/webpay/emu_05.png" class="rounded mx-auto d-block"/>

## Configuración

1. Una vez instalado el plugin de Webpay es momento de configurarlo. Para lo cual ingresa a "WooCommerce → Ajustes → Finalizar compra → Transbank":

<img src="/images/plug/woo/webpay/emu_06.png" class="rounded mx-auto d-block"/>

### Observaciones
<dl>
  <dt>Los ambientes que encontrarás en la configuración del plugin son:</dt>

  <dd>**Integración**: Modo de prueba, tanto código de comercio, llaves y certificados, vienen dadas por defecto en el plugin y te permitirá generar simulaciones de compra para verificar la conexión con webpay de Transbank.</dd>

  <dd>**Certificación**: Etapa en la que Transbank está validando tu e-commerce (proceso QA).</dd>

  <dd>**Producción**: Una vez que Transbank certifica tu e-commerce, estarás en condiciones de vender a través de tu sitio.</dd>
</dl>

Para acceder a los ambientes de Certificación y Producción, debes afiliar tu comercio a Transbank. Una vez realizado, se te entregará un código de comercio, con el que deberás crear la llave y el certificado público. El certificado público debes enviarlo a Transbank para validar tu comercio.

Para conocer los requisitos de afiliación.  <a href="https://portaltransbank.cl/afiliacion/" target="blank">https://portaltransbank.cl/afiliacion/</a>

<img src="/images/plug/woo/webpay/emu_07.png" class="rounded mx-auto d-block"/>

- Si presionamos el botón **"Información"** tendremos acceso a las herramientas de diagnóstico y de registro.

**Información**

En la pestaña **"Información"** encontraremos información básica de nuestro servidor, ecommerce y del plugin Webpay instalado. También con el botón **"Crear PDF"** generamos un informe en formato pdf con información útil para el soporte y diagnóstico de problemas.

<img src="/images/plug/woo/webpay/emu_08.png" class="rounded mx-auto d-block"/>

**PHP Info**

En la pestaña **"PHP info"** nos permite ver el reporte php_info generado por el servidor, con información útil para el soporte y diagnóstico de problemas. Con el botón **"Crear PHP info"** generamos este reporte en formato PDF.

<img src="/images/plug/woo/webpay/emu_09.png" class="rounded mx-auto d-block"/>

**Registros**

En la pestaña **"Registros"** Podemos activar, desactivar y configurar el tiempo y peso de los registros de comunicación con Transbank a guardarse. También se nos permite descargarlos y ver el último registro.

<img src="/images/plug/woo/webpay/emu_10.png" class="rounded mx-auto d-block"/>

### Ejemplo de Compra

A modo de ejemplo, realizaremos la compra de un producto y su interacción con el Plugin de Webpay, bajo ambiente de Integración.

<img src="/images/plug/woo/webpay/emu_11.png" class="rounded mx-auto d-block"/>

Como vemos Webpay se encuentra disponible como medio de pago dentro del ECommerce creado gracias al plugin instalado.

<img src="/images/plug/woo/webpay/emu_12.png" class="rounded mx-auto d-block"/>

Al realizar el pedido, aparecerá un pequeño resumen, si la información es correcta presionamos el botón **"Webpay"**.

<img src="/images/plug/woo/webpay/emu_13.png" class="rounded mx-auto d-block"/>

El siguiente paso es la presentación del formulario de Webpay, lo que inicia la interacción entre Transbank y tu ECommerce. En el emisor debes seleccionar "BANCO_CERTIFICACION", ingresando el número de tarjeta que aparece en la imagen.

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
<!-- <img src="/images/plug/woo/webpay/emu_15.png" class="rounded mx-auto d-block"/> -->

Para acceder al voucher de WebPay con el detalle de la transacción exitosa.

<!-- <img src="/images/plug/woo/webpay/emu_16.png" class="rounded mx-auto d-block"/> -->
<img src="/images/plug/webpay_form/form_05.png" class="rounded mx-auto d-block"/>

Y finalizar con la presentación en el ECommerce del detalle de la transacción.

<img src="/images/plug/woo/webpay/emu_17.png" class="rounded mx-auto d-block"/>
