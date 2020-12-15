
<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank"  href="https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/releases/latest">Descargar Plugin</a>
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

<aside class="notice">
Estás viendo la <strong>nueva documentación REST</strong> de este plugin. Si quieres ver referencia de la versión anterior
(SOAP) haz [click aquí](/plugin/woocommerce/webpay-soap)
</aside>

<h1 class="toc-ignore">Webpay REST WooCommerce</h1>
<h1 style="display: none;">Webpay REST</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Webpay fácilmente en tu comercio, basado en WooCommerce.

## Requisitos

Debes tener instalado previamente [WooCommerce](https://woocommerce.com/).
Asegúrate de tener habilitados los siguientes módulos / extensiones para PHP:

* Soap
* OpenSSL 1.0.1 o superior
* SimpleXML
* DOM 2.7.8 o superior
* PHP 5.6 o superior

Al instalar el plugin, podrás revisar si todas estos requisitos se cumplen, a través de la pantalla de diagnóstico que se incluye.

_Esta pantalla de diagnóstico se encuentra en la sección de configuración del plugin (en donde configuras tu código de comercio y tu **llave secreta**). Ahí debes presionar el botón "Información del sistema"_

## Instalación

1. [Descargar el archivo .zip del plugin](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/releases/latest)
2. Sube el archivo zip en la sección Plugin > Subir nuevo plugin en el administrador de tu Wordpress

Las instrucciones detalladas de instalación las puedes encontrar en el [siguiente link](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/blob/master/docs/INSTALLATION.md).

## Cómo usar
Una vez instalado agregará un nuevo método de pago disponible en tu WooCommerce. Si no se activa correctamente, revisa que el plugin esté activo y que tu tienda está configurada en Pesos Chilenos. 

Como nuevo método de pago, tus clientes podrán pagar por sus pedidos usando la opción Webpay Plus en el proceso de Checkout, y una vez finalizado el pago la ordfen será aprobada automáticamente (si el pago fue realizado correctamente).
Dentro de la lista de órdenes verás cuales están pagadas. En el detalle de una orden verás también algunas notas (Notas de la orden) donde puedes verificar cual fue el resultado detallado de la transacción (código de autorización, tipo de tarjeta, etc )  

### Anulaciones
Dentro del detalle de una orden, podrás realizar una anulación o reversa de un pago. 
Puedes ver el detalle de una reversa o anulación ("refund") [en este link](https://transbankdevelopers.cl/documentacion/webpay-plus#reversar-o-anular-una-transaccion)

Las tarjetas de débito solo soportan Reversas (refunds realizados dentro de los primeros 30 minutos de aprobada la transacción). 
En tarjetas de crédito se permiten reversas, anulaciones y anulaciones parciales. 

En el siguiente video te mostramos como realizar el proceso: 
<div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/ogw2yHcaZ4M-8" >
  <div class="container-embed">
    <div class="data-info-url">
      <b>Tutorial de "refunds" (41s)</b>
    </div>
    <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
  </div>
</div>

### Consultar estado 

También en el detalle de una orden, se puede revisar el estado actual de una transacción en todo momento. 
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

## Ambiente de pruebas

Una vez instalado el plugin, este viene configurado en el ambiente de **Integración** de Transbank, por lo que puedes realizar todas las pruebas de pago que necesites, ya que no se usa dinero real.
En este ambiente solo funcionan las tarjetas de crédito y débito de prueba que puedes [encontrar acá](/documentacion/como_empezar#ambiente-de-integracion).

## Obtener tu llave secreta (proceso de validación)

Para usar el plugin en el ambiente de producción (donde se utiliza dinero real), necesitas tener tu **llave secreta**, que es un código especial que está asociado a tu código de comercio.
Para obtenerla necesitas pasar un proceso de validación, que está [explicado acá](/documentacion/como_empezar#el-proceso-de-validacion).

Al finalizar este proceso de validación, obtendrás tu **llave secreta**.

Nota: Esta **llave secreta** es como la contraseña de tu código de comercio, por lo que no debes compartirla. Se usa para identificar que tu comercio es quién realmente está realizando cada operación (transacción, anulación de un pago, etc).

## El proceso de validación

Este proceso pretende verificar que el comercio transacciona de manera segura y sin problemas. Esta validación es un requisito para dejar al comercio en producción y no se permitirá que un comercio utilice productivamente el servicio Webpay sin poseer esta validación.

En esta etapa, debes envíar las evidencias a [soporte@transbank.cl](mailto:soporte@transbank.cl).

Planilla de validación para plugins oficiales: [Descargar](https://transbankdevelopers.cl/files/evidencia-integracion-webpay-plugins-rest.docx)

Soporte validará el formulario enviado y, de estar todo correcto, se te notificará la conformidad para pasar a producción, recibiendo tu **llave secreta** (_Api Key Secret_) de producción y algunas instrucciones.

## Puesta en producción

Si ya tienes tu código de comercio de producción y llave secreta, solo debes entrar a la configuración de tu plugin ([instrucciones en este link](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/blob/master/docs/INSTALLATION.md#configuraci%C3%B3n)) y colocar:

* Ambiente: Producción
* Código de comercio: tu código de comercio de producción
* Api Key: Tu llave secreta

Al guardar, el plugin funcionará inmediatamente en ambiente de producción y podrás operar con tarjetas y transacciones reales. Se te solicitará realizar una transacción real en este ambiente de producción por $50 para finalizar tu proceso.

## Problemas, Dudas, Sugerencias

Si tienes algún problema, duda o sugerencia, puedes contactarnos en nuestra comunidad de Slack, a la que puedes [unirte acá](https://join-transbankdevelopers-slack.herokuapp.com/)

Adicionalmente, puedes revisar si más comercios han presentado algún error o dudas similares en los [_issues_ del repositorio github](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/issues). Si nadie ha comentado algo similar, puedes [crear un nuevo _issue_](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/issues/new) con tu sugerencia, bug, problema, etc.
