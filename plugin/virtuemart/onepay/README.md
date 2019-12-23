<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-onepay/releases/latest">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>VirtueMart >= 3.x</li>
      <li>PHP >= 5.6 and <= 7.2.1</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu Api Key y Shared Secret</li>
      <li>Contar con VirtueMart instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Onepay VirtueMart</h1>
<h1 style="display: none;">Onepay</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Onepay fácilmente en tu comercio, basado en Virtuemart.

## Instalación

1. Dirígete a [https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-onepay/releases](https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-onepay/releases) y descarga la última versión disponible del plugin.

  Una vez descargado el plugin, ingresa a la página de administración de VirtueMart (usualmente en http://misitio.com/administrator, http://localhost/administrator) y dirígete a (Extensions / Manage / Install), indicado a continuación:

  ![Paso 1](/images/plug/virtue/onepay/paso1.png)

2. Arrastra o selecciona el archivo que descargaste en el paso anterior. Al finalizar aparecerá que fue instalado exitosamente.

  ![Paso 2](/images/plug/virtue/onepay/paso2.png)

  ![Paso 3](/images/plug/virtue/onepay/paso3.png)

## Configuración

Este plugin posee un sitio de configuración que te permitirá ingresar credenciales que Transbank te otorgará y además podrás generar un documento de diagnóstico en caso que Transbank te lo pida.

**IMPORTANTE:** El plugin solamente funciona con moneda chilena (CLP). Dado esto VirtueMart debe estar configurado con moneda Peso Chileno y país Chile para que se pueda usar Onepay.

Para acceder a la configuración, debes seguir los siguientes pasos:

1. Dirígete a la página de administración de VirtueMart (usualmente en http://misitio.com/administrator, http://localhost/administrator) e ingresa usuario y clave.

2. Dentro del sitio de administración dirígete a (VirtueMart / Payment Methods).

  ![Paso 4](/images/plug/virtue/onepay/paso4.png)

3. Debes crear un nuevo medio de pago presionando el botón [New].

  ![Paso 5](/images/plug/virtue/onepay/paso5.png)

4. Ingresar los datos para "Transbank Onepay" como se muestra en la siguiente imagen.

    - Payment Name: Transbank Onepay
    - Sef Alias: transbank_onepay
    - Published: Yes
    - Payment Method: Transbank Onepay
    - Currency: Chilean peso

  ![Paso 6](/images/plug/virtue/onepay/paso6.png)

5. Presiona el botón [Save] para guardar el nuevo medio de pago. Se informará que ha sido guardado.

  ![Paso 7](/images/plug/virtue/onepay/paso7.png)

6. Selecciona la sección "Configuration" para configurar el plugin.

  ![Paso 8](/images/plug/virtue/onepay/paso8.png)

7. ¡Ya está! Estás en la pantalla de configuración del plugin, debes ingresar la siguiente información:

   - **Endpoint**: Ambiente hacia donde se realiza la transacción.
   - **APIKey**: Es lo que te identifica como comercio.
   - **Shared Secret**: Llave secreta que te autoriza y valida a hacer transacciones.

  Las opciones disponibles para _Endpoint_ son: "Integración" para realizar pruebas y certificar la instalación con Transbank, y "Producción" para hacer transacciones reales una vez que Transbank ha aprobado el comercio. Dependiendo de cual Endpoint se ha seleccionado el plugin usará uno de los dos set de APIKey y Shared Secret según corresponda.

  Además puedes cambiar los valores de los estados pre-establecidos de las ordenes de acuerdo a tu comercio.

### Credenciales de Prueba

Para el ambiente de Integración, puedes utilizar las siguientes credenciales para realizar pruebas:

- APIKey: `dKVhq1WGt_XapIYirTXNyUKoWTDFfxaEV63-O5jcsdw`
- Shared Secret: `?XW#WOLG##FBAGEAYSNQ5APD#JF@$AYZ`

8. Guardar los cambios presionando el botón [Save]

  ![paso7](/images/plug/virtue/onepay/paso7.png)

9. Además, puedes generar un documento de diagnóstico en caso que Transbank te lo pida. Para ello, haz click en "Generar PDF de Diagnóstico" y automáticamente se descargará dicho documento.

  ![Paso 8](/images/plug/virtue/onepay/paso8.png)

## Prueba de instalación con transacción

En ambiente de integración es posible realizar una prueba de transacción utilizando un emulador de pagos online.

- Ingresa al comercio

  ![demo 1](/images/plug/virtue/onepay/demo1.png)

- Agrega al carro de compras un producto

  ![demo 2](/images/plug/virtue/onepay/demo2.png)

- Selecciona el carro de compras, ingresa los datos que te pida como dirección, método de envío, selecciona método de pago Transbank Onepay, luego presiona el botón [Confirm Purchase]

  ![demo 3](/images/plug/virtue/onepay/demo3.png)

- Una vez presionado el botón para iniciar la compra, se mostrará la ventana de pago Onepay, tal como se ve en la imagen. Toma nota del número que aparece como "Código de compra", ya que lo necesitarás para emular el pago en el siguiente paso (En este ejemplo 8430 - 7696):

  ![demo 4](/images/plug/virtue/onepay/demo4.png)

- En otra ventana del navegador, ingresa al emulador de pagos desde [https://onepay.ionix.cl/mobile-payment-emulator/](https://onepay.ionix.cl/mobile-payment-emulator/), utiliza test@onepay.cl como correo electrónico, y el código de compra obtenido desde la pantalla anterior. Una vez ingresado los datos solicitados, presiona el botón "Iniciar Pago":

  ![demo 5](/images/plug/virtue/onepay/demo5.png)

- Si todo va bien, el emulador mostrará opciones para simular situaciones distintas. Para simular un pago exitoso, presiona el botón `PRE_AUTHORIZED`. En caso de querer simular un pago fallido, presiona le botón `REJECTED`. Simularemos un pago exitoso presionando el botón `PRE_AUTHORIZED`.

  ![demo 6](/images/plug/virtue/onepay/demo6.png)

- Vuelve a la ventana del navegador donde se encuentra VirtueMart y podrás comprobar que el pago ha sido exitoso.

 ![demo 7](/images/plug/virtue/onepay/demo7.png)

- Además si accedes al sitio de administración sección (VirtueMart / Orders) se podrá ver la orden creada y el detalle de los datos entregados por Onepay.

 ![demo 8](/images/plug/virtue/onepay/demo8.png)

 ![demo 9](/images/plug/virtue/onepay/demo9.png)
