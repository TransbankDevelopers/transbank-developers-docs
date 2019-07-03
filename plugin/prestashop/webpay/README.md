#

<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay/releases/latest">Descargar Plugin</a>
    <br>
    <h4>Compatibilidad</h4>
    <ul>
      <li>Prestashop >= 1.6</li>
      <li>PHP >= 5.6 y PHP <= 7.2</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu llave privada y pública</li>
      <li>Contar con Prestashop instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Webpay Prestashop</h1>
<h1 style="display: none;">Webpay</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Webpay fácilmente en tu comercio, basado en Prestashop.

## Requisitos

Debes tener instalado previamente Prestashop.

Habilitar los siguientes módulos / extensiones para PHP:

- Soap
- OpenSSL 1.0.1 o superior
- SimpleXML
- DOM 2.7.8 o superior

## Instalación de Plugin

1. Dirígete a [https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay/releases/latest](https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay/releases/latest), y descargue la última versión disponible del plugin.

  Una vez descargado el plugin, ingresa a la página de administración de Prestashop (usualmente en _misitio.com_/admin), y dirígete a Módulos, Módulos y Servicios, indicado a continuación:

<img src="/images/plug/prestashop/webpay/paso1.png" class="rounded mx-auto d-block"/>

2. Haz click sobre el botón "Subir un módulo":

<img src="/images/plug/prestashop/webpay/paso2.png" class="rounded mx-auto d-block"/>

3. Se abrirá un cuadro para subir el módulo descargado previamente. Procede a arrastrar el archivo, o haz click en "selecciona el archivo" y selecciónalo desde tu computador:

<img src="/images/plug/prestashop/webpay/paso3.png" class="rounded mx-auto d-block"/>

4. Prestashop procederá a instalar el módulo. Una vez finalizado, se te indicará que el módulo fue instalado, y cuando esto suceda debes hacer click en el botón "Configurar":

<img src="/images/plug/prestashop/webpay/paso4.png" class="rounded mx-auto d-block"/>

## Configuración

Este plugin posee un sitio de configuración que te permitirá ingresar credenciales que Transbank te otorgará, y además podrás generar un documento de diagnóstico en caso que Transbank te lo pida.

Para acceder a la configuración, debes seguir los siguientes pasos:

1. Dirígete a la página de administración de Prestashop (usualmente en _misitio.com_/admin), y luego anda a Módulos, Módulos y Servicios.

<img src="/images/plug/prestashop/webpay/paso1.png" class="rounded mx-auto d-block"/>

2. Busca "Webpay", y presiona el botón "Configurar":

<img src="/images/plug/prestashop/webpay/paso5.png" class="rounded mx-auto d-block"/>

3. ¡Ya está! Estás en la pantalla de configuración del plugin, debes ingresar la siguiente información:
   - **Ambiente**: Ambiente hacia donde se realiza la transacción.
   - **Código de comercio**: Es lo que te identifica como comercio.
   - **Llave Privada**: Llave secreta que te autoriza y valida a hacer transacciones.
   - **Certificado**: Llave pública que te autoriza y valida a hacer transacciones.

  Las opciones disponibles para _Ambiente_ son: "Integración" para realizar pruebas y certificar la instalación con Transbank, y "Producción" para hacer transacciones reales una vez que Transbank ha aprobado el comercio.

<aside class="notice">
  Tip: No es necesario que cambies el certificado de Transbank, ya que este se reemplaza al seleccionar el **ambiente**.
</aside>

### Credenciales de Prueba

Para el ambiente de Integración, puedes utilizar las siguientes credenciales para realizar pruebas:

- Código de comercio: `597020000540`
- Llave Privada: Se puede encontrar [aquí - private_key](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.key)
- Certificado Público: Se puede encontrar [aquí - public_cert](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.crt)

<aside class="notice">
  Tip: Al momento de pasar al ambiente de producción será necesario generar tu propia llave privada, puedes ver las instrucciones para crearlas [aquí](https://www.transbankdevelopers.cl/documentacion/como_empezar#credenciales-en-webpay)
</aside>

4. Guardar los cambios presionando el botón [Guardar]

5. Además, puedes generar un documento de diagnóstico en caso que Transbank te lo pida. Para ello, haz click en el botón "Información" ahí podrás descargar un pdf.

<img src="/images/plug/prestashop/webpay/paso6.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/prestashop/webpay/paso7.png" class="rounded mx-auto d-block"/>

## Prueba de instalación con transacción

En ambiente de integración es posible realizar una prueba de transacción utilizando un emulador de pagos online.

- Ingresa al comercio

<img src="/images/plug/prestashop/webpay/demo1.png" class="rounded mx-auto d-block"/>

- Ya con la sesión iniciada, ingresa a cualquier sección para agregar productos

<img src="/images/plug/prestashop/webpay/demo2.png" class="rounded mx-auto d-block"/>

- Agrega al carro de compras un producto, selecciona el carro de compras y luego presiona el botón [Pasar por caja]:

<img src="/images/plug/prestashop/webpay/demo3.png" class="rounded mx-auto d-block"/>

- Presiona el botón [Pasar por caja]:

<img src="/images/plug/prestashop/webpay/demo4.png" class="rounded mx-auto d-block"/>

- Selecciona método de envío y presiona el botón [Continuar]

  Debes asegurarte que tu dirección de envío sea en Chile.

<img src="/images/plug/prestashop/webpay/demo5.png" class="rounded mx-auto d-block"/>

- Selecciona método de Pago con Tarjetas de Crédito o Redcompra, selecciona los "términos de servicio" y luego presiona el botón [Pedido con obligación de pago] y luego el botón [Pagar]

<img src="/images/plug/prestashop/webpay/demo6.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/prestashop/webpay/demo6.1.png" class="rounded mx-auto d-block"/>

- Una vez presionado el botón para iniciar la compra, se mostrará la ventana de pago Webpay y deberás seguir el proceso de pago.

Para pruebas puedes usar los siguientes datos:

- Número de tarjeta: `4051885600446623`
- Rut: `11.111.111-1`
- Cvv: `123`

<img src="/images/plug/prestashop/webpay/demo7.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/prestashop/webpay/demo8.png" class="rounded mx-auto d-block"/>

Para pruebas puedes usar los siguientes datos:

- Rut: `11.111.111-1`
- Clave: `123`

<img src="/images/plug/prestashop/webpay/demo9.png" class="rounded mx-auto d-block"/>

Puedes aceptar o rechazar la transacción

<img src="/images/plug/prestashop/webpay/demo10.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/prestashop/webpay/demo11.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/prestashop/webpay/demo12.png" class="rounded mx-auto d-block"/>

- Serás redirigido a Prestashop y podrás comprobar que el pago ha sido exitoso.

<img src="/images/plug/prestashop/webpay/demo13.png" class="rounded mx-auto d-block"/>

- Además si accedes al sitio de administración sección (Pedidos / Pedido) se podrá ver la orden creada y el detalle de los datos entregados por Webpay.

<img src="/images/plug/prestashop/webpay/order1.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/prestashop/webpay/order2.png" class="rounded mx-auto d-block"/>
