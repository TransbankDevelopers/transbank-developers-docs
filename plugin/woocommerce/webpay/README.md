
<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank"  href="https://wordpress.org/plugins/transbank-webpay-plus-rest/">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>WooCommerce >= 3.2</li>
      <li>PHP >= 5.6</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu propio código de comercio y llave secreta</li>
      <li>Contar con WooCommerce instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

___

<h1 class="toc-ignore">WooCommerce | Plugin Webpay Plus </h1>
<h1 style="display: none;">Webpay REST</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Webpay fácilmente en tu comercio, basado en WooCommerce.

Estás viendo la nueva documentación REST de este plugin. Si aún estás usando la versión anterior SOAP (la que te solicita certificados y una llave privada), revisa este video para actualizarte. 

<div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube.com/embed/9--NHgh07Fw" >
  <div class="container-embed">
    <div class="data-info-url">
      <b>Cómo pasar de SOAP a REST</b>
    </div>
    <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
  </div>
</div>


## Requisitos

Debes tener instalado previamente [WooCommerce](https://woocommerce.com/).
Asegúrate de tener habilitados los siguientes módulos / extensiones para PHP:

* Extensión de PHP JSON 
* OpenSSL 1.0.1 o superior
* DOM 2.7.8 o superior
* PHP 7.0 o superior

Al instalar el plugin, podrás revisar si todas estos requisitos se cumplen, a través de la pantalla de diagnóstico que se incluye.

_Esta pantalla de diagnóstico se encuentra en la sección de configuración del plugin (en donde configuras tu código de comercio y tu **llave secreta**). Ahí verás un tab "Diagnóstico"._

## Instalación

Como se puede ver en el video, los pasos para instalar el plugin son los siguientes:  
1. Entra al panel de administración de tu Wordpress > Plugins > Agregar Nuevo
2. Busca el plugin "Transbank Webpay Plus REST", instálalo y actívalo.

<div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube.com/embed/IeKb8RreF08" >
  <div class="container-embed">
    <div class="data-info-url">
      <b>Cómo instalar el plugin de WooCommerce</b>
    </div>
    <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
  </div>
</div>

### Instalación manual (obsoleta)
Si no puedes instalarlo de manera automática, también puedes descargar el archivo .zip del plugin y cargarlo manualmente en tu Wordpress. 

1. [Descargar el archivo .zip del plugin](https://wordpress.org/plugins/transbank-webpay-plus-rest/)
2. Sube el archivo zip en la sección Plugin > Subir nuevo plugin en el administrador de tu Wordpress
Las instrucciones detalladas de instalación las puedes encontrar en el [siguiente link](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/blob/master/docs/INSTALLATION.md).
   
## Cómo usar

Una vez instalado, se agregará un nuevo método de pago al proceso de checkout. Si no se activa correctamente, revisa que el plugin esté activo y que tu tienda está configurada en Pesos Chilenos.

### Ambiente de pruebas

Una vez instalado el plugin, este viene configurado en el ambiente de **Integración** de Transbank, por lo que puedes realizar todas las pruebas de pago que necesites, ya que no se usa dinero real.
En este ambiente solo funcionan las tarjetas de crédito y débito de prueba que puedes [encontrar acá](/documentacion/como_empezar#ambiente-de-integracion).

### Anulaciones

Dentro del detalle de una orden, podrás realizar una anulación o reversa de un pago.
Puedes ver el detalle de una reversa o anulación ("refund") [en este link](https://transbankdevelopers.cl/documentacion/webpay-plus#reversar-o-anular-una-transaccion)

Las tarjetas de débito solo soportan Reversas (refunds realizados dentro de los primeros 30 minutos de aprobada la transacción). 
En tarjetas de crédito se permiten reversas, anulaciones y anulaciones parciales.

En el siguiente video te mostramos como realizar el proceso: 
<div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/ogw2yHcaZ4M" >
  <div class="container-embed">
    <div class="data-info-url">
      <b>Tutorial de "refunds" (41s)</b>
    </div>
    <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
  </div>
</div>

### Consultar estado

También en el detalle de una orden, se puede revisar el estado actual de una transacción (hasta por 7 días de realizada la transacción).
Se puede revisar si una transacción está autorizada, anulada, parcialmente anulada o reversada.

En el siguiente video te dejamos las instrucciones para realizar este proceso
<div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/AB9eh7BTJUE" >
  <div class="container-embed">
    <div class="data-info-url">
      <b>Tutorial de consulta de transacción (02:07)</b>
    </div>
    <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
  </div>
</div>

## Obtener tu llave secreta (proceso de validación)

Para usar el plugin en el ambiente de producción (donde se utiliza dinero real), necesitas tener tu **llave secreta**, que es un código especial que está asociado a tu código de comercio.
Para obtenerla necesitas pasar un proceso de validación, que está [explicado acá](/documentacion/como_empezar#el-proceso-de-validacion).

Este proceso pretende verificar que el comercio transacciona de manera segura y sin problemas. Esta validación es un requisito para dejar al comercio en producción y no se permitirá que un comercio utilice productivamente el servicio Webpay sin poseer esta validación.
En esta etapa, debes envíar las evidencias a [soporte@transbank.cl](mailto:soporte@transbank.cl).

Planilla de validación para plugins oficiales: [Descargar](https://transbankdevelopers.cl/files/evidencia-integracion-webpay-plugins-rest.docx)

Soporte validará el formulario enviado y, de estar todo correcto, se te notificará la conformidad para pasar a producción, recibiendo tu **llave secreta** (_Api Key Secret_) de producción y algunas instrucciones.

Nota: Esta **llave secreta** es una contraseña de tu código de comercio, por lo que no debes compartirla.

## Puesta en producción

<div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube.com/embed/B9sb7SyROVk" >
  <div class="container-embed">
    <div class="data-info-url">
      <b>Cómo pasar a producción</b>
    </div>
    <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
  </div>
</div>

Si ya tienes tu código de comercio de producción y llave secreta, solo debes entrar a la configuración de tu plugin ([instrucciones en este link](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/blob/master/docs/INSTALLATION.md#configuraci%C3%B3n)) y colocar:

* Ambiente: Producción
* Código de comercio: tu código de comercio de producción
* Api Key: Tu llave secreta

Al guardar, el plugin funcionará inmediatamente en ambiente de producción y podrás operar con tarjetas y transacciones reales. Se te solicitará realizar una transacción real en este ambiente de producción por $50 para finalizar tu proceso.

## Problemas, Dudas, Sugerencias

Si tienes algún problema, duda o sugerencia, puedes contactarnos en nuestra comunidad de Slack, a la que puedes [unirte acá](https://join-transbankdevelopers-slack.herokuapp.com/)

Adicionalmente, puedes revisar si más comercios han presentado algún error o dudas similares en los [_issues_ del repositorio github](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/issues). Si nadie ha comentado algo similar, puedes [crear un nuevo _issue_](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/issues/new) con tu sugerencia, bug, problema, etc.
