
<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank"  href="https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay-rest/releases/latest">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>Prestashop >= 3.2</li>
      <li>PHP >= 5.6</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu propio código de comercio y llave secreta</li>
      <li>Contar con Prestashop instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

___

<aside class="notice">
Estás viendo la <strong>nueva documentación REST</strong> de este plugin. Si quieres ver referencia de la versión anterior
(SOAP) haz [click aquí](/plugin/prestashop/webpay-soap)
</aside>

<h1 class="toc-ignore">Webpay REST Prestashop</h1>
<h1 style="display: none;">Webpay REST</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Webpay fácilmente en tu comercio, basado en Prestashop.

## Requisitos

Debes tener instalado previamente [Prestashop](https://prestashop.com/).
Asegúrate de tener habilitados los siguientes módulos / extensiones para PHP:

* PHP 5.6 o superior

Al instalar el plugin, podrás revisar si todas estos requisitos se cumplen, a través de la pantalla de diagnóstico que se incluye.

_Esta pantalla de diagnóstico se encuentra en la sección de configuración del plugin (en donde configuras tu código de comercio y tu **llave secreta**). Ahí debes presionar el botón "Información del sistema"_

## Instalación

1. [Descargar el archivo .zip del plugin](https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay-rest/releases/latest)
2. Sube el archivo zip en la sección Plugin > Subir nuevo plugin en el administrador de tu Prestashop

Las instrucciones detalladas de instalación las puedes encontrar en el [siguiente link](https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay-rest/blob/master/docs/INSTALLATION.md).

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

Si ya tienes tu código de comercio de producción y llave secreta, solo debes entrar a la configuración de tu plugin ([instrucciones en este link](https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay-rest/blob/master/docs/INSTALLATION.md#configuraci%C3%B3n)) y colocar:

* Ambiente: Producción
* Código de comercio: tu código de comercio de producción
* Api Key: Tu llave secreta

Al guardar, el plugin funcionará inmediatamente en ambiente de producción y podrás operar con tarjetas y transacciones reales. Se te solicitará realizar una transacción real en este ambiente de producción por $50 para finalizar tu proceso.

## Problemas, Dudas, Sugerencias

Si tienes algún problema, duda o sugerencia, puedes contactarnos en nuestra comunidad de Slack, a la que puedes [unirte acá](https://join-transbankdevelopers-slack.herokuapp.com/)

Adicionalmente, puedes revisar si más comercios han presentado algún error o dudas similares en los [_issues_ del repositorio github](https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay-rest/issues). Si nadie ha comentado algo similar, puedes [crear un nuevo _issue_](https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay-rest/issues/new) con tu sugerencia, bug, problema, etc.
