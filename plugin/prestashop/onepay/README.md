<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-prestashop-onepay/releases/latest">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>Prestashop >= 1.7</li>
      <li>PHP >= 5.6 y PHP <= 7.2</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu Api Key y Shared Secret</li>
      <li>Contar con Prestashop instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Onepay Prestashop</h1>
<h1 style="display: none;">Onepay</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Onepay fácilmente en tu comercio, basado en Prestashop.

## Requisitos

Debes tener instalado previamente Prestashop.

## Instalación de Plugin

1. Dirígete a [https://github.com/TransbankDevelopers/transbank-plugin-prestashop-onepay/releases](https://github.com/TransbankDevelopers/transbank-plugin-prestashop-onepay/releases), y descargue la última versión disponible del plugin.

  Una vez descargado el plugin, ingresa a la página de administración de Prestashop (usualmente en _misitio.com_/admin), y dirígete a Módulos, Módulos y Servicios, indicado a continuación:

  ![Paso 1](/images/plug/prestashop/onepay/paso1.png)

2. Haz click sobre el botón "Subir un módulo":

  ![Paso 2](/images/plug/prestashop/onepay/paso2.png)

3. Se abrirá un cuadro para subir el módulo descargado previamente. Procede a arrastrar el archivo, o haz click en "selecciona el archivo" y selecciónalo desde tu computador:

  ![Paso 3](/images/plug/prestashop/onepay/paso3.png)

4. Prestashop procederá a instalar el módulo. Una vez finalizado, se te indicará que el módulo fue instalado, y cuando esto suceda debes hacer click en el botón "Configurar":

  ![Paso 4](/images/plug/prestashop/onepay/paso4.png)

5. Serás redirigido al listado de módulos disponibles. Busca "Onepay", y presiona el botón "Configurar":

  ![Paso 5](/images/plug/prestashop/onepay/paso5.png)

## Configuración

Este plugin posee un sitio de configuración que te permitirá ingresar credenciales que Transbank te otorgará, y además podrás generar un documento de diagnóstico en caso que Transbank te lo pida.

Para acceder a la configuración, debes seguir los siguientes pasos:

1. Dirígete a la página de administración de Prestashop (usualmente en _misitio.com_/admin), y luego anda a Módulos, Módulos y Servicios.

  ![Paso 1](/images/plug/prestashop/onepay/paso1.png)

2. Busca "Onepay", y presiona el botón "Configurar":

  ![Paso 2](/images/plug/prestashop/onepay/paso5.png)

3. ¡Ya está! Estás en la pantalla de configuración del plugin, debes ingresar la siguiente información:
   - **Activar Onepay**: Al activarlo, Onepay estará disponible como medio de pago. Ten la precaución de que se encuentre marcada esta opción cuando quieras que los usuarios paguen con Onepay.
   - **APIKey**: Es lo que te identifica como comercio.
   - **Shared Secret**: Llave secreta que te autoriza y valida a hacer transacciones.
   - **Endpoint**: Ambiente hacia donde se realiza la transacción.

  Las opciones disponibles para _Endpoint_ son: "Integración" para realizar pruebas y certificar la instalación con Transbank, y "Producción" para hacer transacciones reales una vez que Transbank ha aprobado el comercio.

  ![Paso 3](/images/plug/prestashop/onepay/paso6.png)

4. Además, puedes generar un documento de diagnóstico en caso que Transbank te lo pida. Para ello, haz click en "Generar PDF de Diagnóstico", y automáticamente se descargará dicho documento.

  ![Paso 4](/images/plug/prestashop/onepay/paso7.png)

## Credenciales de Prueba

Para el ambiente de Integración, puedes utilizar las siguientes credenciales para realizar pruebas:

- APIKey: `dKVhq1WGt_XapIYirTXNyUKoWTDFfxaEV63-O5jcsdw`
- Shared Secret: `?XW#WOLG##FBAGEAYSNQ5APD#JF@$AYZ`

## Prueba de instalación con transacción

En ambiente de integración es posible realizar una pruebas de transacción utilizando un emulador de pagos online.

- Para ello, agrega al carro de compras un producto:
  ![Paso 1](/images/plug/prestashop/onepay/emu1.png)

- Luego anda al checkout para iniciar el flujo de pago, haciendo click en "Pasar por caja":
  ![Paso 2](/images/plug/prestashop/onepay/emu2.png)
  ![Paso 3](/images/plug/prestashop/onepay/emu3.png)

- En la página de checkout, ingresa los datos requeridos para realizar la compra:
  ![Paso 4](/images/plug/prestashop/onepay/emu4.png)

- Selecciona la opción "Pagar con Onepay", y presiona el botón "Pedido con obligación de pago".
  ![Paso 5](/images/plug/prestashop/onepay/emu5.png)

- Una vez presionado el botón para iniciar la compra, se mostrará la ventana de pago Onepay, tal como se ve en la imagen. Toma nota del número que aparece como "Código de compra", ya que lo necesitarás para emular el pago en el siguiente paso:
  ![Paso 6](/images/plug/prestashop/onepay/emu6.png)

- En otra ventana del navegador, ingresa al emulador de pagos desde [https://onepay.ionix.cl/mobile-payment-emulator/](https://onepay.ionix.cl/mobile-payment-emulator/), utiliza test@onepay.cl como correo electrónico, y el código de compra obtenido desde la pantalla anterior. Una vez ingresado los datos solicitados, presiona el botón "Iniciar Pago":
  ![Paso 7](/images/plug/prestashop/onepay/emu7.png)

- Si todo va bien, el emulador mostrará opciones para simular situaciones distintas. Para simular un pago exitoso, presiona el botón `PRE_AUTHORIZED`. En caso de querer simular un pago fallido, presiona le botón `REJECTED`. Simularemos un pago exitoso presionando el botón `PRE_AUTHORIZED`.
  ![Paso 8](/images/plug/prestashop/onepay/emu8.png)

- Vuelve a la ventana del navegador donde se encuentra Prestashop, y podrás comprobar que el pago ha sido exitoso.
 ![Paso 9](/images/plug/prestashop/onepay/emu9.png)

- Automáticamente serás redirigido a Prestashop, con la comprobación final del pago.
 ![Paso 10](/images/plug/prestashop/onepay/emu10.png)
