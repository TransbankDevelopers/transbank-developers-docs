# Plugin Opencart Webpay SOAP

<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-opencart-webpay/releases/latest">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>OpenCart >= 3.x</li>
      <li>PHP >= 5.6 y PHP <= 7.1</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu llave privada y pública</li>
      <li>Contar con Prestashop instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Webpay Opencart</h1>
<h1 style="display: none;">Webpay</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Webpay fácilmente en tu comercio, basado en Opencart.

## Requisitos

Debes tener instalado previamente Opencart.

Habilitar los siguientes módulos / extensiones para PHP:

* Soap
* OpenSSL 1.0.1 o superior
* SimpleXML
* DOM 2.7.8 o superior

## Instalación de Plugin

1. Dirígete a [https://github.com/TransbankDevelopers/transbank-plugin-opencart-webpay/releases/latest](https://github.com/TransbankDevelopers/transbank-plugin-opencart-webpay/releases/latest), y descargue la última versión disponible del plugin.

    Una vez descargado el plugin, ingresa a la página de administración de Opencart (usualmente en _misitio.com_/admin), y dirígete a (Extensions / Installer) indicado a continuación:

    <img src="/images/plug/open/webpay/paso1.png" class="rounded mx-auto d-block"/>

2. Haz click sobre el botón [Upload], selecciona el archivo del plugin y Opencart procederá a instalar el plugin. Una vez finalizado, se te indicará que el módulo fue instalado:

    <img src="/images/plug/open/webpay/paso2.png" class="rounded mx-auto d-block"/>

### Refrescar el sistema de modificaciones de OpenCart

1. Dirígete a (Extensions / Modifications) y selecciona el plugin "Transbank Webpay" indicado a continuación:

   <img src="/images/plug/open/webpay/paso3.png" class="rounded mx-auto d-block"/>

2. Con el plugin "Transbank Webpay" seleccionado presiona el botón "Refresh" ![save]<img src="/images/plug/open/webpay/mod_refresh.png" class="rounded mx-auto d-block"/> de la parte superior derecha.

    OpenCart indicará que las modificaciones han sido exitosas sobre el plugin:

    <img src="/images/plug/open/webpay/paso4.png" class="rounded mx-auto d-block"/>

3. Dentro del sitio de administración dirígete a (Extensions / Extensions) y filtra por "Payments".

   <img src="/images/plug/open/webpay/paso5.png" class="rounded mx-auto d-block"/>

4. Busca hacia abajo el plugin "Webpay Plus".

    <img src="/images/plug/open/webpay/paso6.png" class="rounded mx-auto d-block"/>

5. Presiona el botón verde "+" para instalar el plugin.

    <img src="/images/plug/open/webpay/paso7.png" class="rounded mx-auto d-block"/>

6. Cambiará a color rojo.

  <img src="/images/plug/open/webpay/paso8.png" class="rounded mx-auto d-block"/>

## Configuración

Este plugin posee un sitio de configuración que te permitirá ingresar credenciales que Transbank te otorgará, y además podrás generar un documento de diagnóstico en caso que Transbank te lo pida.

Para acceder a la configuración, debes seguir los siguientes pasos:

1. Dirígete a la página de administración de Opencart (usualmente en _misitio.com_/admin), y luego dirígete a (Extensions / Extensions) , filtra por "Payments", busca "Webpay Plus" y presiona el botón "Edit" del plugin:

   <img src="/images/plug/open/webpay/paso8.png" class="rounded mx-auto d-block"/>

2. ¡Ya está! Estás en la pantalla de configuración del plugin, debes ingresar la siguiente información:
   * **Estado**: Habilita o deshabilita el plugin, debes habilitarlo para que funcione como medio de pago en tu comercio.
   * **Ambiente**: Ambiente hacia donde se realiza la transacción.
   * **Código de comercio**: Es lo que te identifica como comercio. Si el código que posees es de 8 dígitos debes anteponer `5970`.
   * **Llave Privada**: Llave secreta que te autoriza y valida a hacer transacciones.
   * **Certificado**: Llave publica que te autoriza y valida a hacer transacciones.

  Las opciones disponibles para _Ambiente_ son: "Integración" para realizar pruebas y certificar la instalación con Transbank, y "Producción" para hacer transacciones reales una vez que Transbank ha aprobado el comercio.

<aside class="notice">
  Tip: Ya no es necesario configurar el **Certificado Transbank**, en versiones anteriores era requerido, actualmente este se reemplaza al seleccionar **ambiente**.
</aside>

### Configuración de los estados de la orden

Asegurate de configurar correctamente los estados de la orden según corresponda a tu comercio:

* **Estado completado**: Estado de una orden exitosa.
* **Estado rechazado**: Estado de una orden rechazada.
* **Estado cancelado**: Estado de una orden cancelada.

<img src="/images/plug/open/webpay/paso9.png" class="rounded mx-auto d-block"/>

### Credenciales de Prueba

Para el ambiente de Integración, puedes utilizar las siguientes credenciales para realizar pruebas:

* Código de comercio: `597020000540`
* Llave Privada: Se puede encontrar [aquí - private_key](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.key)
* Certificado Publico: Se puede encontrar [aquí - public_cert](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.crt)

<aside class="notice">
  Tip: Al momento de pasar al ambiente de producción será necesario generar tu propia llave privada, puedes ver las instrucciones para crearlas [aquí](https://www.transbankdevelopers.cl/documentacion/como_empezar#credenciales-en-webpay)
</aside>

1. Guardar los cambios presionando el botón [Guardar]

2. Además, puedes generar un documento de diagnóstico en caso que Transbank te lo pida. Para ello, haz click en el botón "Información" ahí podrás descargar un pdf.

  <img src="/images/plug/open/webpay/paso10.png" class="rounded mx-auto d-block"/>

## Prueba de instalación con transacción

En ambiente de integración es posible realizar una prueba de transacción utilizando un emulador de pagos online.

* Ingresa al comercio y con la sesión iniciada, ingresa a cualquier sección para agregar productos

  <img src="/images/plug/open/webpay/demo1.png" class="rounded mx-auto d-block"/>

* Agrega al carro de compras un producto, selecciona el carro de compras y luego presiona el botón [Checkout]:

  <img src="/images/plug/open/webpay/demo2.png" class="rounded mx-auto d-block"/>

* Ingresa todos los datos requeridos:

  <img src="/images/plug/open/webpay/demo3.png" class="rounded mx-auto d-block"/>

* Selecciona método de pago "Pago con Tarjetas de Crédito o Redcompra":

  <img src="/images/plug/open/webpay/demo4.png" class="rounded mx-auto d-block"/>

* Presiona el botón [Continuar a Webpay]

  <img src="/images/plug/open/webpay/demo5.png" class="rounded mx-auto d-block"/>

* Una vez presionado el botón para iniciar la compra, se mostrará la ventana de pago Webpay y deberás seguir el proceso de pago.

Para pruebas puedes usar los siguientes datos:

* Número de tarjeta: `4051885600446623`
* Rut: `11.111.111-1`
* Cvv: `123`

<img src="/images/plug/open/webpay/demo6.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/open/webpay/demo7.png" class="rounded mx-auto d-block"/>

Para pruebas puedes usar los siguientes datos:

* Rut: `11.111.111-1`
* Clave: `123`

<img src="/images/plug/open/webpay/demo8.png" class="rounded mx-auto d-block"/>

Puedes aceptar o rechazar la transacción

<img src="/images/plug/open/webpay/demo9.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/open/webpay/demo10.png" class="rounded mx-auto d-block"/>

* Serás redirigido a Opencart y podrás comprobar que el pago ha sido exitoso.

<img src="/images/plug/open/webpay/demo11.png" class="rounded mx-auto d-block"/>

* Además si accedes al sitio de administración sección (Sales / Orders) se podrá ver la orden creada y el detalle de los datos entregados por Webpay.

<img src="/images/plug/open/webpay/order1.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/open/webpay/order2.png" class="rounded mx-auto d-block"/>
