# Plugin Opencart Onepay

<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-opencart-onepay/releases/latest">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>OpenCart >= 3.x</li>
      <li>PHP >= 5.6 and <= 7.0</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu Api Key y Shared Secret</li>
      <li>Contar con OpenCart instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Onepay OpenCart</h1>
<h1 style="display: none;">Onepay</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Onepay fácilmente en tu comercio, basado en OpenCart 3.x.

## Requisitos

1. Debes tener instalado previamente alguna versión de OpenCart 3

## Instalación del Plugin

1. Dirígete a [https://github.com/TransbankDevelopers/transbank-plugin-opencart-onepay/releases](https://github.com/TransbankDevelopers/transbank-plugin-opencart-onepay/releases) y descarga la última versión disponible del plugin.

    Una vez descargado el plugin, ingresa a la página de administración de OpenCart (usualmente en [http://misitio.com/admin](http://misitio.com/admin), [http://localhost/admin](http://localhost/admin)) y dirígete a Extensions / Installer, indicado a continuación:

    ![Paso 1](/images/plug/open/onepay/paso1.png)

2. Haz click sobre el botón "Upload" y selecciona el archivo que descargaste en el paso anterior. Al finalizar aparecerá que fue instalado exitosamente.

  ![Paso 2](/images/plug/open/onepay/paso2.png)

## Configuración

Este plugin posee un sitio de configuración que te permitirá ingresar credenciales que Transbank te otorgará y además podrás generar un documento de diagnóstico en caso que Transbank te lo pida.

Para acceder a la configuración, debes seguir los siguientes pasos:

1. Dirígete a la página de administración de OpenCart (usualmente en [http://misitio.com/admin](http://misitio.com/admin), [http://localhost/admin](http://localhost/admin)) e ingresa usuario y clave.

2. Dentro del sitio de administración dirígete a (Extensions / Extensions) y filtra por "Payments".

    ![Paso 3](/images/plug/open/onepay/paso3.png)

3. Busca hacia abajo el plugin "Transbank Onepay".

    ![Paso 4](/images/plug/open/onepay/paso4.png)

4. Presiona el botón verde "+" para instalar el plugin.

    ![Paso 5](/images/plug/open/onepay/paso5.png)

5. Cambiará a color rojo.

    ![Paso 6](/images/plug/open/onepay/paso6.png)

6. Presiona el botón para editar el plugin

7. ¡Ya está! Estás en la pantalla de configuración del plugin, debes ingresar la siguiente información:
   * **Estado**: Al activarlo, Onepay estará disponible como medio de pago. Ten la precaución de que se encuentre seleccionada esta opción cuando quieras que los usuarios paguen con Onepay.
   * **Endpoint**: Ambiente hacia donde se realiza la transacción.
   * **APIKey**: Es lo que te identifica como comercio.
   * **Shared Secret**: Llave secreta que te autoriza y valida a hacer transacciones.

  Las opciones disponibles para _Endpoint_ son: "Integración" para realizar pruebas y certificar la instalación con Transbank, y "Producción" para hacer transacciones reales una vez que Transbank ha aprobado el comercio. Dependiendo de cual Endpoint se ha seleccionado el plugin usará uno de los dos set de APIKey y Shared Secret según corresponda.

### Credenciales de Prueba

Para el ambiente de Integración, puedes utilizar las siguientes credenciales para realizar pruebas:

* APIKey: `dKVhq1WGt_XapIYirTXNyUKoWTDFfxaEV63-O5jcsdw`
* Shared Secret: `?XW#WOLG##FBAGEAYSNQ5APD#JF@$AYZ`

1. Guardar los cambios presionando el botón ![save](/images/plug/open/onepay/save.png)

   ![paso7](/images/plug/open/onepay/paso7.png)

2. Además, puedes generar un documento de diagnóstico en caso que Transbank te lo pida. Para ello, haz click en "Generar PDF de Diagnóstico" y automáticamente se descargará dicho documento.

  ![Paso 8](/images/plug/open/onepay/paso8.png)

### Refrescar el sistema de modificaciones de OpenCart

1. Dirígete a (Extensions / Modifications) y selecciona el plugin "Transbank Onepay" indicado a continuación:

    ![Paso 9](/images/plug/open/onepay/paso9.png)

2. Con el plugin "Transbank Onepay" seleccionado presiona el botón "Refresh" ![save](/images/plug/open/onepay/mod_refresh.png) de la parte superior derecha.

OpenCart indicará que las modificaciones han sido exitosas sobre el plugin:

  ![Paso 10](/images/plug/open/onepay/paso10.png)

## Prueba de instalación con transacción

En ambiente de integración es posible realizar una prueba de transacción utilizando un emulador de pagos online.

* Ingresa al comercio

  ![demo 1](/images/plug/open/onepay/demo1.png)

* Ya con la sesión iniciada, ingresa a cualquier sección para agregar productos

  ![demo 2](/images/plug/open/onepay/demo2.png)

* Agrega al carro de compras un producto:

  ![demo 3](/images/plug/open/onepay/demo3.png)

* Selecciona el carro de compras y luego presiona el botón [Checkout]:

  ![demo 4](/images/plug/open/onepay/demo4.png)

* Ingresa los datos que te pida como dirección, método de envío y luego selecciona método de pago Transbank Onepay, luego presiona el botón [Continue]

  ![demo 5](/images/plug/open/onepay/demo5.png)

* Luego presiona el botón [Confirm Order]

  ![demo 6](/images/plug/open/onepay/demo6.png)

* Una vez presionado el botón para iniciar la compra, se mostrará la ventana de pago Onepay, tal como se ve en la imagen. Toma nota del número que aparece como "Código de compra", ya que lo necesitarás para emular el pago en el siguiente paso (En este ejemplo 8660 - 7579):

  ![demo 7](/images/plug/open/onepay/demo7.png)

* En otra ventana del navegador, ingresa al emulador de pagos desde [https://onepay.ionix.cl/mobile-payment-emulator/](https://onepay.ionix.cl/mobile-payment-emulator/), utiliza test@onepay.cl como correo electrónico, y el código de compra obtenido desde la pantalla anterior. Una vez ingresado los datos solicitados, presiona el botón "Iniciar Pago":

  ![demo 8](/images/plug/open/onepay/demo8.png)

* Si todo va bien, el emulador mostrará opciones para simular situaciones distintas. Para simular un pago exitoso, presiona el botón `PRE_AUTHORIZED`. En caso de querer simular un pago fallido, presiona le botón `REJECTED`. Simularemos un pago exitoso presionando el botón `PRE_AUTHORIZED`.

  ![demo 9](/images/plug/open/onepay/demo9.png)

* Vuelve a la ventana del navegador donde se encuentra OpenCart y podrás comprobar que el pago ha sido exitoso.

 ![demo 10](/images/plug/open/onepay/demo10.png)

 ![demo 11](/images/plug/open/onepay/demo11.png)

* Además si accedes al sitio de administración sección (Sales / Orders) se podrá ver la orden creada y el detalle de los datos entregados por Onepay.

 ![demo 12](/images/plug/open/onepay/demo12.png)

 ![demo 13](/images/plug/open/onepay/demo13.png)
