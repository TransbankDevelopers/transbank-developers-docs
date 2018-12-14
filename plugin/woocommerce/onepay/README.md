<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-onepay/releases/latest">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>Woocommerce >= 3.2</li>
      <li>PHP >= 5.6 y PHP <= 7.1</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu Api Key y Shared Secret</li>
      <li>Contar con WooCommerce instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Onepay WooCommerce</h1>
<h1 style="display: none;">Onepay</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Onepay fácilmente en tu comercio, basado en WooCommerce.

## Requisitos

Ud. debe tener instalado previamente Wordpress, y WooCommerce.

## Instalación de Plugin

1. Dirígete a [https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-onepay/releases](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-onepay/releases), y descargue la última versión disponible del plugin.

  Una vez descargado el plugin, ingrese a la página de administración de Wordpress (usualmente en _misitio.com_/wp-admin), y dirígete a Plugins, Añadir nuevo, indicado a continuación:

  ![Paso 1](/images/plug/woo/onepay/paso1.png)

2. Haz click sobre el botón "Subir plugin", y selecciona el archivo que descargaste en el paso anterior. Luego de eso, presiona el botón "Instalar Ahora".

  ![Paso 2](/images/plug/woo/onepay/paso2.png)

3. Una vez realizado el paso anterior, Wordpress realizará la instalación del plugin Onepay. Cuando finalice, debes activar el plugin haciendo click en el botón "Activar plugin".

  ![Paso 3](/images/plug/woo/onepay/paso3.png)

## Configuración

Este plugin posee un sitio de configuración que te permitirá ingresar credenciales que Transbank te otorgará, y además podrás generar un documento de diagnóstico en caso que Transbank te lo pida.

Para acceder a la configuración, debes seguir los siguientes pasos:

1. Dirígete a la página de administración de Wordpress (usualmente en _misitio.com_/wp-admin), y luego anda a WooCommerce, Ajustes.

  ![Paso 1](/images/plug/woo/onepay/paso4.png)

2. Llegarás a la página de configuración de WooCommerce, ahora debes hacer click en "Pagos".

  ![Paso 2](/images/plug/woo/onepay/paso5.png)

3. En esta pantalla podrás ver todos los medios de pago disponibles.  Busca el plugin "Onepay", y haz click sobre el título.

  ![Paso 3](/images/plug/woo/onepay/paso6.png)

4. ¡Ya está! Estás en la pantalla de configuración del plugin, debes ingresar la siguiente información:
  * **Activar Onepay**: Al activarlo, Onepay estará disponible como medio de pago. Ten la precaución de que se encuentre marcada esta opción cuando quieras que los usuarios paguen con Onepay.
  * **APIKey**: Es lo que te identifica como comercio.
  * **Shared Secret**: Llave secreta que te autoriza y valida a hacer transacciones.
  * **Endpoint**: Ambiente hacia donde se realiza la transacción.

  Las opciones disponibles para _Endpoint_ son: "Integración" para realizar pruebas y certificar la instalación con Transbank, y "Producción" para hacer transacciones reales una vez que Transbank ha aprobado el comercio.

  Además, puedes generar un documento de diagnóstico en caso que Transbank te lo pida. Para ello, haz click en "Generar PDF de Diagnóstico", y automáticamente se descargará dicho documento.

  ![Paso 4](/images/plug/woo/onepay/paso7.png)

## Credenciales de Prueba

Para el ambiente de Integración, puedes utilizar las siguientes credenciales para realizar pruebas:

* APIKey: `dKVhq1WGt_XapIYirTXNyUKoWTDFfxaEV63-O5jcsdw`
* Shared Secret: `?XW#WOLG##FBAGEAYSNQ5APD#JF@$AYZ`


## Prueba de instalación con transacción

En ambiente de integración es posible realizar una pruebas de transacción utilizando un emulador de pagos online.

* Para ello, agrega al carro de compras un producto:
  ![Paso 1](/images/plug/woo/onepay/emu1.png)

* Luego anda al checkout para iniciar el flujo de pago:
  ![Paso 2](/images/plug/woo/onepay/emu2.png)

* En la página de checkout, primero verifica que Onepay esté disponible como medio de pago. Selecciónalo, y luego realiza la orden de compra para iniciar el pago.
  ![Paso 3](/images/plug/woo/onepay/emu3.png)

* Una vez presionado el botón para iniciar la compra, se mostrará la ventana de pago Onepay, tal como se ve en la imagen. Toma nota del número que aparece como "Código de compra", ya que lo necesitarás para emular el pago en el siguiente paso:
  ![Paso 4](/images/plug/woo/onepay/emu4.png)

* En otra ventana del navegador, ingresa al emulador de pagos desde [https://onepay.ionix.cl/mobile-payment-emulator/](https://onepay.ionix.cl/mobile-payment-emulator/), utiliza test@onepay.cl como correo electrónico, y el código de compra obtenido desde la pantalla anterior. Una vez ingresado los datos solicitados, presiona el botón "Iniciar Pago":
  ![Paso 5](/images/plug/woo/onepay/emu5.png)

* Si todo va bien, el emulador mostrará opciones para simular situaciones distintas. Para simular un pago exitoso, presiona el botón `PRE_AUTHORIZED`. En caso de querer simular un pago fallido, presiona le botón `REJECTED`. Simularemos un pago exitóso presionando el botón `PRE_AUTHORIZED`.
  ![Paso 6](/images/plug/woo/onepay/emu6.png)

* Vuelve a la ventana del navegador donde se encuentra WooCommerce, y podrás comprobar que el pago ha sido exitoso.
 ![Paso 6](/images/plug/woo/onepay/emu7.png)

* Automáticamente serás redirigido a WooCommerce, con la comprobación final del pago.
 ![Paso 7](/images/plug/woo/onepay/emu8.png)
