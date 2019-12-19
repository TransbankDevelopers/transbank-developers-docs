#

<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-webpay/releases/latest">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>VirtueMart >= 3.x</li>
      <li>PHP >= 5.6 and <= 7.2.1</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu llave privada y pública</li>
      <li>Tener correctamente configurado el Ecommerce</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Webpay Virtuemart</h1>
<h1 style="display: none;">Webpay</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Webpay fácilmente en tu comercio, basado en Virtuemart.

## Requisitos

Debes tener instalado previamente Virtuemart.

Habilitar los siguientes módulos / extensiones para PHP:

- Soap
- OpenSSL 1.0.1 o superior
- SimpleXML
- DOM 2.7.8 o superior

## Instalación de Plugin

1. Dirígete a [https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-webpay/releases/latest](https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-webpay/releases/latest), y descargue la última versión disponible del plugin.

  Una vez descargado el plugin, ingresa a la página de administración de VirtueMart (usualmente en http://misitio.com/administrator, http://localhost/administrator) y dirígete a (Extensions / Manage / Install), indicado a continuación:

<img src="/images/plug/virtue/webpay/paso1.png" class="rounded mx-auto d-block"/>

2. Arrastra o selecciona el archivo que descargaste en el paso anterior. Al finalizar aparecerá que fue instalado exitosamente.

<img src="/images/plug/virtue/webpay/paso2.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/virtue/webpay/paso3.png" class="rounded mx-auto d-block"/>

## Configuración

Este plugin posee un sitio de configuración que te permitirá ingresar credenciales que Transbank te otorgará y además podrás generar un documento de diagnóstico en caso que Transbank te lo pida.

**IMPORTANTE:** El plugin solamente funciona con moneda chilena (CLP). Dado esto VirtueMart debe estar configurado con moneda Peso Chileno y país Chile para que se pueda usar Webpay.

Para acceder a la configuración, debes seguir los siguientes pasos:

1. Dirígete a la página de administración de VirtueMart (usualmente en http://misitio.com/administrator, http://localhost/administrator) e ingresa usuario y clave.

2. Dentro del sitio de administración dirígete a (VirtueMart / Payment Methods).

<img src="/images/plug/virtue/webpay/paso4.png" class="rounded mx-auto d-block"/>

3. Debes crear un nuevo medio de pago presionando el botón [New].

<img src="/images/plug/virtue/webpay/paso5.png" class="rounded mx-auto d-block"/>

4. Ingresar los datos para "Transbank Webpay" como se muestra en la siguiente imagen.

    - Payment Name: Transbank Webpay
    - Sef Alias: transbank_webpay
    - Published: Yes
    - Payment Method: Transbank Webpay
    - Currency: Chilean peso

<img src="/images/plug/virtue/webpay/paso6.png" class="rounded mx-auto d-block"/>

5. Presiona el botón [Save] para guardar el nuevo medio de pago. Se informará que ha sido guardado.

<img src="/images/plug/virtue/webpay/paso7.png" class="rounded mx-auto d-block"/>

6. Selecciona la sección "Configuration" para configurar el plugin.

<img src="/images/plug/virtue/webpay/paso8.png" class="rounded mx-auto d-block"/>

7. ¡Ya está! Estás en la pantalla de configuración del plugin, debes ingresar la siguiente información:
   - **Ambiente**: Ambiente hacia donde se realiza la transacción.
   - **Código de comercio**: Es lo que te identifica como comercio. Si el código que posees es de 8 dígitos debes anteponer `5970`.
   - **Llave Privada**: Llave secreta que te autoriza y valida a hacer transacciones.
   - **Certificado Público**: Llave pública que te autoriza y valida a hacer transacciones.
   - **Certificado Transbank**: Llave secreta de webpay que te autoriza y valida a hacer transacciones.

  Las opciones disponibles para _Ambiente_ son: "Integración" para realizar pruebas y certificar la instalación con Transbank, y "Producción" para hacer transacciones reales una vez que Transbank ha aprobado el comercio.

### Credenciales de Prueba

Para el ambiente de Integración, puedes utilizar las siguientes credenciales para realizar pruebas:

- Código de comercio: `597020000540`
- Llave Privada: Se puede encontrar [aquí - private_key](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.key)
- Certificado Público: Se puede encontrar [aquí - public_cert](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.crt)
- Certificado Webpay: Se puede encontrar [aquí - webpay_cert](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/tbk.pem.crt)

<aside class="notice">
  Tip: Al momento de pasar al ambiente de producción será necesario generar tu propia llave privada, puedes ver las instrucciones para crearlas [aquí](https://www.transbankdevelopers.cl/documentacion/como_empezar#credenciales-en-webpay)
</aside>

8. Guardar los cambios presionando el botón [Guardar]

9. Además, puedes generar un documento de diagnóstico en caso que Transbank te lo pida. Para ello, haz click en el botón "Información" ahí podrás descargar un pdf.

<img src="/images/plug/virtue/webpay/paso9.png" class="rounded mx-auto d-block"/>

## Prueba de instalación con transacción

En ambiente de integración es posible realizar una prueba de transacción utilizando un emulador de pagos online.

- Ingresa al comercio

<img src="/images/plug/virtue/webpay/demo1.png" class="rounded mx-auto d-block"/>

- Ya con la sesión iniciada, ingresa a cualquier sección para agregar productos

<img src="/images/plug/virtue/webpay/demo2.png" class="rounded mx-auto d-block"/>

- Agrega al carro de compras un producto, selecciona el carro de compras y selecciona Transbank Webpay, selecciona términos de servicio y luego presiona el botón [Confirm Purchase]:

<img src="/images/plug/virtue/webpay/demo3.png" class="rounded mx-auto d-block"/>

- Una vez presionado el botón para iniciar la compra, se mostrará la ventana de pago Webpay y deberás seguir el proceso de pago.

Para pruebas puedes usar los siguientes datos:

- Número de tarjeta: `4051885600446623`
- Rut: `11.111.111-1`
- Cvv: `123`

<img src="/images/plug/virtue/webpay/demo4.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/virtue/webpay/demo5.png" class="rounded mx-auto d-block"/>

Para pruebas puedes usar los siguientes datos:

- Rut: `11.111.111-1`
- Clave: `123`

<img src="/images/plug/virtue/webpay/demo6.png" class="rounded mx-auto d-block"/>

Puedes aceptar o rechazar la transacción

<img src="/images/plug/virtue/webpay/demo7.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/virtue/webpay/demo8.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/virtue/webpay/demo9.png" class="rounded mx-auto d-block"/>

- Serás redirigido a Virtuemart y podrás comprobar que el pago ha sido exitoso.

<img src="/images/plug/virtue/webpay/demo10.png" class="rounded mx-auto d-block"/>

- Además si accedes al sitio de administración sección (VirtueMart / Orders) se podrá ver la orden creada y el detalle de los datos entregados por Webpay.

<img src="/images/plug/virtue/webpay/order1.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/virtue/webpay/order2.png" class="rounded mx-auto d-block"/>
