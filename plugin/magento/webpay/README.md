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

<h1 class="toc-ignore">Webpay Magento</h1>
<h1 style="display: none;">Webpay</h1>

## Requisitos
Tener instalado previamente Magento2 y asegurarte de tener Composer instalado.

Habilitar los siguientes modulos para PHP:
+ Soap
+ OpenSSL 1.0.1 o superior
+ SimpleXML
+ DOM 2.7.8 o superior
+ Mcrypt
+ Sockets
+ Único protocolo de comunicación aceptado: TLS 1.2

## Instalación

1. La instalación del plugin se debe realizar directo en el servidor donde está instalado el E-commerce. Se debe ingresar a la ruta raíz del sitio.

```shell
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$ pwd /var/www/html/entornos/magento2.x/2.1.12_2
```

2. En esta ruta se debe copiar el archivo descargado.

```shell
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$ ls -ltr
total 1424
drwxrwxrwx  6 soporte soporte   4096 ene 17  2017 update
-rwxrwxrwx  1 soporte soporte 362987 ene 17  2017 composer.lock
-rwxrwxrwx  1 soporte soporte   3000 ene 17  2017 composer.json
-rwxrwxrwx  1 soporte soporte   3935 ago 24 20:05 magento2__install.sh
drwxrwxrwx  2 soporte soporte   4096 ago 24 20:06 magento-2-community-sample-data-2.1.2
-rwxrwxrwx  1 soporte soporte    631 ago 24 20:07 COPYING.txt
-rwxrwxrwx  1 soporte soporte   3381 ago 24 20:07 CONTRIBUTING.md
-rwxrwxrwx  1 soporte soporte 435065 ago 24 20:07 CHANGELOG.md
-rwxrwxrwx  1 soporte soporte  10364 ago 24 20:07 LICENSE.txt
-rwxrwxrwx  1 soporte soporte  10376 ago 24 20:07 LICENSE_AFL.txt
-rwxrwxrwx  1 soporte soporte    315 ago 24 20:07 ISSUE_TEMPLATE.md
-rwxrwxrwx  1 soporte soporte   2854 ago 24 20:07 Gruntfile.js.sample
drwxrwxrwx  5 soporte soporte   4096 ago 24 20:07 dev
drwxrwxrwx  2 soporte soporte   4096 ago 24 20:07 bin
drwxrwxrwx  4 soporte soporte   4096 ago 24 20:07 app
drwxrwxrwx  4 soporte soporte   4096 ago 24 20:07 lib
-rwxrwxrwx  1 soporte soporte   1358 ago 24 20:07 index.php
drwxrwxrwx  2 soporte soporte   4096 ago 24 20:07 phpserver
-rwxrwxrwx  1 soporte soporte    804 ago 24 20:07 php.ini.sample
-rwxrwxrwx  1 soporte soporte   1427 ago 24 20:07 package.json.sample
-rwxrwxrwx  1 soporte soporte   5233 ago 24 20:07 nginx.conf.sample
drwxrwxrwx  6 soporte soporte   4096 ago 24 20:07 pub
drwxrwxrwx  7 soporte soporte   4096 ago 24 20:07 setup
drwxrwxrwx 29 soporte soporte   4096 ago 24 20:07 vendor
drwxrwxrwx  9 soporte soporte   4096 ago 24 20:19 var
-rwxr-xr-x  1 soporte soporte 539682 sep 26 16:36 webpay_magento2.tgz
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$
```

3. Descomprimir el archivo.

```shell
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$ tar -xzvf webpay_magento2.tgz
app/
app/code/
app/code/Transbank/
app/code/Transbank/Webpay/
.
.
.
.
.
app/code/Transbank/Webpay/view/frontend/web/template/payment/
app/code/Transbank/Webpay/view/frontend/web/template/payment/webpay.html
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$
```

4. Actualizar la instalación de Magento para que reconozca la instalación del plugin.

```shell
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$ bin/magento setup:upgrade
Cache cleared successfully
File system cleanup:
/var/www/html/entornos/magento2.x/2.1.12_2/var/generation/Composer
/var/www/html/entornos/magento2.x/2.1.12_2/var/generation/Magento
/var/www/html/entornos/magento2.x/2.1.12_2/var/generation/Symfony
.
.
.
.
.
Module 'Magento_WishlistSampleData':
Module 'Transbank_Webpay':
Please re-run Magento compile commandsoporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$
```

5. Recompilar la instalación de Magento para que active el plugin.

```shell
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$ bin/magento setup:di:compile
Compilation was started.
Interception cache generation... 7/7 [============================] 100% 7 mins 420.2 MiBB MiB
Generated code and dependency injection configuration successfully.
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$
```

6. Finalizada la instalación se debe refrescar el cache del sitio para que considere los cambios realizados.

```shell
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$ bin/magento cache:flush
Flushed cache types:
config
layout
block_html
collections
reflection
db_ddl
eav
customer_notification
full_page
config_integration
config_integration_api
translate
config_webservice
soporte@debian:/var/www/html/entornos/magento2.x/2.1.12_2$
```

## Configuración

1. Dentro del panel de administración del sitio dirigirse al menú **"STORES → Settings → Configuration"**:

<img src="/images/plug/mage/webpay/emu_01.png" class="rounded mx-auto d-block"/>

2. Ingresar en el menú a **"SALES → Payment Methods":**

<img src="/images/plug/mage/webpay/emu_02.png" class="rounded mx-auto d-block"/>

Será necesario habilitar el plugin para que este se encuentre disponible como método de pago en el sitio.

### Observaciones
<dl>
  <dt>Los ambientes que encontrarás en la configuración del plugin son:</dt>

  <dd>**Integración**: Modo de prueba, tanto código de comercio, llaves y certificados, vienen dadas por defecto en el plugin y te permitirá generar simulaciones de compra para verificar la conexión con webpay de Transbank.</dd>

  <dd>**Certificación**: Etapa en la que Transbank está validando tu e-commerce (proceso QA).</dd>

  <dd>**Producción**: Una vez que Transbank certifica tu e-commerce, estarás en condiciones de vender a través de tu sitio.</dd>
</dl>

Para acceder a los ambientes de Certificación y Producción, debes afiliar tu comercio a Transbank. Una vez realizado, se te entregará un código de comercio, con el que deberás crear la llave y el certificado público. El certificado público debes enviarlo a Transbank para validar tu comercio.

Para conocer los requisitos de afiliación.  <a href="https://portaltransbank.cl/afiliacion/" target="blank">https://portaltransbank.cl/afiliacion/</a>

<img src="/images/plug/mage/webpay/emu_03.png" class="rounded mx-auto d-block"/>

- Si presionamos el botón "Información" tendremos acceso a las herramientas de diagnóstico y de registro.

**Información**

En la pestaña **"Información"** encontraremos información básica de nuestro servidor, ecommerce y del plugin Webpay instalado. También con el botón **"Crear PDF"** generamos un informe en formato pdf con información útil para el soporte y diagnóstico de problemas.

<img src="/images/plug/mage/webpay/emu_04.png" class="rounded mx-auto d-block"/>

**PHP Info**

En la pestaña **"PHP info"** nos permite ver el reporte php_info generado por el servidor, con información útil para el soporte y diagnóstico de problemas. Con el botón **"Crear PHP info"** generamos este reporte en formato PDF.

<img src="/images/plug/mage/webpay/emu_05.png" class="rounded mx-auto d-block"/>

**Registros**

En la pestaña **"Registros"** Podemos activar, desactivar y configurar el tiempo y peso de los registros de comunicación con Transbank a guardarse. También se nos permite descargarlos y ver el último registro.

<img src="/images/plug/mage/webpay/emu_06.png" class="rounded mx-auto d-block"/>

### Ejemplo de Compra

A modo de ejemplo, realizaremos la compra de un producto y su interacción con el Plugin de Webpay, bajo ambiente de Integración.

<img src="/images/plug/mage/webpay/emu_07.png" class="rounded mx-auto d-block"/>

El siguiente paso es la presentación del formulario de Webpay Plus, lo que inicia la interacción entre Transbank y tu eCommerce.

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

Finalmente vemos el Voucher de Webpay Plus con el detalle de la transacción exitosa por parte de Webpay, el que al cabo de algunos segundos nos enviará al resumen de compra de tu eCommerce.

<img src="/images/plug/mage/webpay/emu_08.png" class="rounded mx-auto d-block"/>
