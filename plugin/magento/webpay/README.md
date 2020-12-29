
<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank"  href="https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/releases/latest">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>Magento2 2.2.X y 2.3.X</li>
      <li>PHP >= 5.6</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu propio código de comercio y llave secreta</li>
      <li>Contar con Magento2 instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

___

<aside class="notice">
Estás viendo la <strong>nueva documentación REST</strong> de este plugin. Si quieres ver referencia de la versión anterior
(SOAP) haz [click aquí](/plugin/magento/webpay-soap)
</aside>

<h1 class="toc-ignore">Webpay REST Magento2</h1>
<h1 style="display: none;">Webpay REST</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Webpay fácilmente en tu comercio, basado en Magento2.
Puedes encontrar el repositorio Open Source de este plugin [acá](https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest).

## Requisitos

Debes tener instalado previamente [Magento2](https://magento.com/).
Asegúrate de tener habilitados los siguientes módulos / extensiones para PHP:

* Soap
* OpenSSL 1.0.1 o superior
* SimpleXML
* DOM 2.7.8 o superior
* PHP 5.6 o superior

Al instalar el plugin, podrás revisar si todas estos requisitos se cumplen, a través de la pantalla de diagnóstico que se incluye.

_Esta pantalla de diagnóstico se encuentra en la sección de configuración del plugin (en donde configuras tu código de comercio y tu **llave secreta**). Ahí debes presionar el botón "Información del sistema"_

## Instalación

### A) Con composer (recomendado)

Esta es la opción recomendada para instalar el paquete en Magento.

Primero, entrar con la terminal al directorio raíz de Magento e instalar el paquete usando [Composer](https://getcomposer.org/).

```bash
composer require transbank/webpay-magento2-rest
```

Cuando finalice, ejecutar el comando:

```bash
magento module:enable Transbank_Webpay --clear-static-content
```

y finalmente:

```bash
magento setup:upgrade && magento setup:di:compile && magento setup:static-content:deploy
```

Puedes encontrar las instrucciones detalladas de instalación las puedes encontrar en el [siguiente link](https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/blob/master/docs/INSTALLATION.md) del repositorio.

### B) Como archivo

1. [Descargar el archivo .zip del plugin](https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/releases/latest)
2. Descomprimir el archivo en la carpeta app/code/Transbank/WebpayRest
3. Edita el archivo app/etc/config.php y agrega  `Transbank_Webpay_Rest => 1` a la lista de módulos.
4. Entra al admin de magento y elimina el caché. (System > Cache Management)
5. El módulo debería estar instalado.

## Ambiente de pruebas

Una vez instalado el plugin, este viene configurado en el ambiente de **Integración** de Transbank, por lo que puedes realizar todas las pruebas de pago que necesites, ya que no se usa dinero real.
En este ambiente solo funcionan las tarjetas de crédito y débito de prueba que puedes [encontrar acá](/documentacion/como_empezar#ambiente-de-integracion).

## Obtener tu llave secreta (proceso de validación)

Para usar el plugin en el ambiente de producción (donde se utiliza dinero real), necesitas tener tu **llave secreta**, que es un código especial que está asociado a tu código de comercio.
Para obtenerla necesitas pasar un proceso de validación, que está [explicado acá](https://transbankdevelopers.cl/documentacion/como_empezar#puesta-en-produccion).

Al finalizar este proceso de validación, obtendrás tu **llave secreta**.
Nota: Esta **llave secreta** es como la contraseña de tu código de comercio, por lo que no debes compartirla. Se usa para identificar que tu comercio es quién realmente está realizando cada operación (transacción, anulación de un pago, etc).

## Puesta en producción

<div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube.com/watch?v=B9sb7SyROVk" >
  <div class="container-embed">
    <div class="data-info-url">
      <b>Cómo pasar a producción</b>
    </div>
    <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
  </div>
</div>

Si ya tienes tu código de comercio de producción y llave secreta, solo debes entrar a la configuración de tu plugin ([instrucciones en este link](https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/blob/master/docs/INSTALLATION.md#configuraci%C3%B3n)) y colocar:

* Ambiente: Producción
* Código de comercio: tu código de comercio de producción
* Api Key: Tu llave secreta

Al guardar, el plugin funcionará inmediatamente en ambiente de producción y podrás operar con tarjetas y transacciones reales.

## Problemas, Dudas, Sugerencias

Si tienes algún problema, duda o sugerencia, puedes contactarnos en nuestra comunidad de Slack, a la que puedes [unirte acá](https://join-transbankdevelopers-slack.herokuapp.com/)
Adicionalmente, puedes revisar si más comercios han presentado algún error/dudas similares en los [_issues_ del repositorio github](https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/issues). Si nadie ha comentado algo similar, puedes [crear un nuevo _issue_](https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/issues/new) con tu sugerencia, bug, problema, etc.
