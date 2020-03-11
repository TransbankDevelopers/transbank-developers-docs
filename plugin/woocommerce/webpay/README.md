<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank"  href="https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay/releases/latest">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>Woocommerce >= 3.2</li>
      <li>PHP >= 5.6</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu llave privada y pública</li>
      <li>Contar con WooCommerce instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Webpay WooCommerce</h1>
<h1 style="display: none;">Webpay</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Webpay fácilmente en tu comercio, basado en Woocommerce.

<div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/NrvBtGFgA-8" >
  <div class="container-embed">
    <div class="data-info-url">
      <b>Video tutorial de integración WooCommerce</b>
    </div>
    <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
  </div>
</div>

## Requisitos

Debes tener instalado previamente Woocommerce.

Habilitar los siguientes módulos / extensiones para PHP:

- Soap
- OpenSSL 1.0.1 o superior
- SimpleXML
- DOM 2.7.8 o superior

## Instalación de Plugin

1. Dirígete a [https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay/releases/latest](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay/releases/latest), y descargue la última versión disponible del plugin.

  Una vez descargado el plugin, ingresa a la página de administración de Woocommerce (usualmente en <http://misitio.com/wp-admin>, <http://localhost/wp-admi>) y dirígete a (Plugins / Add New), indicado a continuación:

<img src="/images/plug/woo/webpay/paso1.png" class="rounded mx-auto d-block"/>

2. Selecciona el archivo que descargaste en el paso anterior. Al finalizar aparecerá que fue instalado exitosamente.

<img src="/images/plug/woo/webpay/paso2.png" class="rounded mx-auto d-block"/>

3. Además debes `Activar Plugin`.

<img src="/images/plug/woo/webpay/paso3.png" class="rounded mx-auto d-block"/>

## Configuración

Este plugin posee un sitio de configuración que te permitirá ingresar credenciales que Transbank te otorgará y además podrás generar un documento de diagnóstico en caso que Transbank te lo pida.

**IMPORTANTE:** El plugin solamente funciona con moneda chilena (CLP). Dado esto Woocommerce debe estar configurado con moneda Peso Chileno y país Chile para que se pueda usar Webpay.

Para acceder a la configuración, debes seguir los siguientes pasos:

1. Dirígete a la página de administración de Woocommerce (usualmente en <http://misitio.com/wp-admin>, <http://localhost/wp-admin>) e ingresa usuario y clave.

2. Dentro del sitio de administración dirígete a (Plugins / Installed Plugins) y busca `Transbank Webpay Plus`.

<img src="/images/plug/woo/webpay/paso4.png" class="rounded mx-auto d-block"/>

3. Presiona el link `Settings` del plugin y te llevará a la pagina de configuración del plugin.

<img src="/images/plug/woo/webpay/paso5.png" class="rounded mx-auto d-block"/>

4. ¡Ya está! Estás en la pantalla de configuración del plugin, debes ingresar la siguiente información:
   - **Ambiente**: Ambiente hacia donde se realiza la transacción.
   - **Código de comercio**: Es lo que te identifica como comercio. Si el código que posees es de 8 dígitos debes anteponer `5970`.
   - **Llave privada**: Llave secreta que te autoriza y valida a hacer transacciones.
   - **Certificado**: Llave pública que te autoriza y valida a hacer transacciones.
   - **Estado de pedido después del pago**: Por defecto este debe ser `Procesando`.

  Las opciones disponibles para _Ambiente_ son: "Integración" para realizar pruebas y certificar la instalación con Transbank, y "Producción" para hacer transacciones reales una vez que Transbank ha aprobado el comercio.

<aside class="notice">
  Tip: Ya no es necesario configurar el **Certificado Transbank**, en versiones anteriores era requerido, actualmente este se reemplaza al seleccionar **ambiente**.
</aside>

### Credenciales de Prueba

Para el ambiente de Integración, puedes utilizar las siguientes credenciales para realizar pruebas:

- Código de comercio: `597020000540`
- Llave Privada: Se puede encontrar [aquí - private_key](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.key)
- Certificado público: Se puede encontrar [aquí - public_cert](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.crt)

<aside class="notice">
  Tip: Al momento de pasar al ambiente de producción será necesario generar tu propia llave privada, puedes ver las instrucciones para crearlas [aquí](https://www.transbankdevelopers.cl/documentacion/como_empezar#credenciales-en-webpay)
</aside>

5. Guardar los cambios presionando el botón [Save changes]

6. Además, puedes generar un documento de diagnóstico en caso que Transbank te lo pida. Para ello, haz click en el botón "Información" ahí podrás descargar un pdf.

<img src="/images/plug/woo/webpay/paso6.png" class="rounded mx-auto d-block"/>

## Prueba de instalación con transacción

En ambiente de integración es posible realizar una prueba de transacción utilizando un emulador de pagos online.

- Ingresa al comercio

<img src="/images/plug/woo/webpay/demo1.png" class="rounded mx-auto d-block"/>

- Ingresa a cualquier sección para agregar productos

<img src="/images/plug/woo/webpay/demo2.png" class="rounded mx-auto d-block"/>

- Agrega al carro de compras un producto, selecciona el carro de compras y luego presiona el botón [Proceed to checkout]:

<img src="/images/plug/woo/webpay/demo3.png" class="rounded mx-auto d-block"/>

- Agrega los datos que te pida y selecciona `Transbank Webpay` luego presiona el botón [Place order]

<img src="/images/plug/woo/webpay/demo4.png" class="rounded mx-auto d-block"/>

- Una vez presionado el botón para iniciar la compra, se mostrará la ventana de pago Webpay y deberás seguir el proceso de pago.

Para pruebas puedes usar los siguientes datos:

- Número de tarjeta: `4051885600446623`
- Rut: `11.111.111-1`
- Cvv: `123`

<img src="/images/plug/woo/webpay/demo5.png" class="rounded mx-auto d-block"/>

Para pruebas puedes usar los siguientes datos:

- Rut: `11.111.111-1`
- Clave: `123`

<img src="/images/plug/woo/webpay/demo6.png" class="rounded mx-auto d-block"/>

Puedes aceptar o rechazar la transacción

<img src="/images/plug/woo/webpay/demo7.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/woo/webpay/demo8.png" class="rounded mx-auto d-block"/>

- Serás redirigido a Woocommerce y podrás comprobar que el pago ha sido exitoso.

<img src="/images/plug/woo/webpay/demo9.png" class="rounded mx-auto d-block"/>

- Además si accedes al sitio de administración sección (Woocommerce / Orders) se podrá ver la orden creada y el detalle de los datos entregados por Webpay.

<img src="/images/plug/woo/webpay/order1.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/woo/webpay/order2.png" class="rounded mx-auto d-block"/>
