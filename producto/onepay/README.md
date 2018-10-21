# Onepay

<div class="pos-title-nav">

  <div class="video" data-toggle="modal" data-src="/public/resourse/mooc/onepay/menu/index.html" data-target="#ModalCenterData">Introducción Elearning <i class="op-link"></i></div>

  <div tbk-link='/documentacion/onepay' tbk-link-name='Documentación'></div>
  <div tbk-link='/referencia/onepay' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugin/onepay' tbk-link-name='Plugins'></div>
</div>

Onepay es una billetera digital que permite realizar compras en tiendas de comercios electrónicos con Tarjeta de Crédito,  sin necesidad de ingresar la información de la tarjeta en cada compra. La billetera funciona en *smartphones* Android o iOS. Este tipo de pago facilita la venta, disminuye el tiempo de la transacción y reduce los riesgos de ingreso erróneo de los datos del medio de pago.

El producto es seguro, pues los datos del tarjetahabiente no son almacenados en el dispositivo móvil. Y el comercio queda protegido ante fraude o desconocimientos de compra.

Onepay opera con los emisores locales de tarjeta de crédito y opera con todos los tipos de cuotas disponbiles.

De cara al tarjetahabiente se pueden identificar dos flujos de pago dependiendo de si la compra es realizada en el mismo dispositivo donde reside su billetera Onepay (Pago Mobile) o si la compra es realizada en otro dispositivo (Pago Desktop). Sin embargo para la integración de Onepay web la diferencia es totalmente transparente para el comercio.

## Onepay Pago Mobile

Onepay es el producto ideal para que tus clientes paguen en su dispositivo móvil. Cuando el tarjetahabiente tiene instalada su billetera virtual, el flujo de pago con Onepay consta de los siguientes pasos:

1. El tarjetahabiente decide adquirir un producto o servicio dentro de la web o app del comercio.
2. El tarjetahabiente selecciona la opción de pagar con Onepay.
3. El tarjetahabiente es redirigido automáticamente a la app de Onepay.
4. Seleccionando una de sus tarjetas ya registradas e ingresando el PIN de su billetera, el tarjetahabiente confirma el pago.
5. El tarjetahabiente es redirigido de regreso a la web o app del comercio, quien recibe la información del pago autorizado.
6. El comercio se comunica con Transbank para confirmar la transacción.

Si el tarjetahabiente no tiene instalada la aplicación Onepay, al momento del checkout se le indican las instrucciones para instalar y configurar la billetera.

## Onepay Pago Desktop

En el caso de que el tarjetahabiente esté operando en un laptop o computador de escritorio, el flujo de pago con la billetera Onepay es el siguiente:

1. El tarjetahabiente decide adquirir un producto o servicio dentro de la web del comercio.
2. El tarjetahabiente selecciona la opción de pagar con Onepay.
3. La web del comercio le presenta al tarjetahabiente un código QR que debe escanear usando la billetera Onepay con la cámara de su teléfono.
4. El tarjetahabiente escanea el código, selecciona una de sus tarjetas ya registradas e ingresa el PIN de su billetera para confirmar el pago.
5. La web del comercio (a través de la integración Onepay) detecta automáticamente que el pago ha sido autorizado en la billetera.
6. El comercio se comunica con Transbank para confirmar la transacción.

De esta forma, el pago se realiza con mínima fricción para el tarjetahabiente y permite una mayor conversión en las ventas del comercio.

<div class="container slate">
  <div class='slate-after-footer'>
    <div class='row d-flex align-items-stretch'>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22'>¿Tienes alguna duda de integración?</h3>
        <a href='https://join-transbankdevelopers-slack.herokuapp.com/' target='_blank'>
          <div class='td_block_gray'>
            <img src="https://p9.zdassets.com/hc/theme_assets/138842/200037786/logo.png" alt="" style="width: 90px; min-width: 100px;">
            <div class='td_pa-txt'>
              Únete a la comunidad de integradores en Slack y comparte buenos tips ayudando a los nuevos o buscando soluciones alternativas. Nuestro equipo está ahí para ayudarte.
            </div>
          </div>
        </a>
      </div>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22'>Si aún tienes dudas envíanos un mensaje</h3>
        <a class="pointer magenta" data-toggle='modal' data-target='#modalContactForm'>
          <div class='td_block_gray'>
            <div class="fo-size-20"><i class="fas fa-envelope"></i> Envianos un mensaje..</div>
            <div class='td_pa-txt'>
              Si necesitas resolver algún tipo de incidencia en el portal o si existe algún problema con tu integración y  que no has podido resolver, contáctanos a través de nuestro formulario.
            </div>
          </div>
        </a>
      </div>
    </div>
  </div>
</div>
