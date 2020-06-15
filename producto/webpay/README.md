# Webpay

<div class="pos-title-nav">
  <div class="video" data-toggle="modal" data-src="/public/resourse/mooc/webpay/menu/index.html" data-target="#ModalCenterData">Introducción E-learning <i class="op-link"></i></div>
</div>

Webpay es la pasarela de pago de Transbank para realizar transacciones desde Internet con tarjetas bancarias de crédito y débito Redcompra de manera eficaz y segura.

Un flujo de pago en Webpay generalmente cuenta con los siguientes pasos:

1. El tarjetahabiente selecciona los productos o servicios a pagar en el sitio del comercio.

2. El tarjetahabiente elige pagar con Webpay en donde, dependiendo de los productos contratados por el comercio, se despliegan las alternativas de pago de crédito con productos cuotas y/o débito Redcompra.

3. Durante el proceso de pago el emisor de la tarjeta autentica al tarjetahabiente antes de realizar la transacción financiera, con el objetivo de validar que la tarjeta este siendo utilizada por el titular.

4. Una vez resuelta la autenticación se procede a autorizar el pago. Webpay entrega al sistema del comercio el resultado de la autorización.

## Productos Webpay

A continuación se describen los productos que se ofrecen en los servicios web de Webpay.

### Webpay Plus

<div class="pos-title-nav">
  <div tbk-link='/documentacion/webpay#webpay-plus' tbk-link-name='Documentación'></div>
  <div tbk-link='/referencia/webpay#webpay-plus-normal' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugin/webpay' tbk-link-name='Plugins'></div>
</div>

Webpay Plus permite realizar una solicitud de autorización financiera de **un pago** con tarjetas de crédito o débito Redcompra en donde quién realiza el pago ingresa al sitio del comercio, selecciona productos o servicio, y el ingreso asociado a los datos de la tarjeta de crédito o débito Redcompra lo realiza en forma segura en Webpay. El comercio que recibe pagos mediante Webpay Plus es identificado mediante un código de comercio.

Es el tipo de transacción mas común, usada para un pago puntual en una tienda simple. Se generará un único cobro para todos los productos o servicios adquiridos por el tarjetahabiente.

### Webpay Plus Mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-plus-mall' tbk-link-name='Referencia Api'></div>
</div>

Webpay Plus Mall permite realizar una solicitud de autorización financiera de **un conjunto de pagos** con tarjetas de crédito o débito, en donde quién realiza el pago ingresa al sitio del comercio, selecciona productos o servicios, y el ingreso asociado a los datos de la tarjeta de crédito o débito Redcompra lo realiza una única vez en forma segura en Webpay para el conjunto de pagos. **Cada pago tendrá su propio resultado, autorizado o rechazado.**

El concepto de "mall" agrupa múltiples tiendas. Son dichas tiendas las que pueden generar transacciones. Tanto el mall como cada una de las tiendas asociadas son identificadas por un código de comercio diferente.

El tipo de transacción Mall es útil para proveedores de servicios tecnológicos (PST) que pueden realizar una única integración con Transbank y realizar cobros a nombre de los clientes de dichos servicios tecnológicos. Por ejemplo una plataforma SaaS puede ser un mall y las empresas clientes de la plataforma pueden ser las tiendas de dicho mall. De esa manera, la recaudación que la plataforma realice irá directo al cliente a nombre del cual realizó cada cobro.

### OneClick Mall

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpayrest#webpay-oneclick-mall' tbk-link-name='Referencia Api'></div>
</div>

OneClick Mall permite agrupar pagos en una única transacción OneClick hacia múltiples códigos de comercios (similar a una transacción Mall). En una transacción de este tipo, al igual que en OneClick, el tarjetahabiente podrá realizar pagos sin la necesidad de ingresar la información de su tarjeta de crédito en cada uno de ellos. Este tipo de pago facilita la venta, centraliza los pagos, disminuye el tiempo de la transacción y reduce los riesgos de ingreso erróneo de los datos del medio de pago.

**OneClick Mall sólo opera con tarjetas de crédito.** No aplica para tarjetas de débito Redcompra. El modelo de pago contempla el mismo proceso de enrolamiento que la transacción OneClick.

De cara al comercio, OneClick Mall combina dos grupos de beneficios:

- Permite que un tarjetahabiente registre su tarjeta una única vez y pague frecuentemente en cualquiera de las tiendas del mall.

- Permite que un proveedor de servicios tecnológicos (PST) realice una única integración y haga cobros a nombre de múltiples tiendas clientes.

Al no contar con sistema de autenticación bancaria en los cargos que se realizan después de la autorización, será el comercio el responsable de asumir el riesgo de fraude o desconocimientos de compra que realice un tarjetahabiente.

### Webpay Transacción Completa {data-submenuhidden=true}

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#webpay-transaccion-completa' tbk-link-name='Referencia Api'></div>
</div>

Este producto permite realizar transacciones con tarjetas de crédito sin utilizar el formulario standard de Webpay. La mayor diferencia con Webpay Plus Tradicional es que el ingreso de los datos de la tarjeta de crédito, se realiza dentro del mismo sitio web del comercio, lo que permite personalizar, a su criterio, la manera en como pide y valida los datos del cliente.

- La modalidad de Transacción Completa solo funciona con tarjetas de crédito.

- El comercio puede personalizar el formulario en el que solicita los datos del tarjetahabiente para procesar una transacción.
El comercio que desee comenzar a operar con la modalidad de Transacción Completa, debe contar con la certificación de Normas PCI DSS (Payment Card Industry-Data Security Standard) y renovarlas anualmente, debido al manejo de data sensible que pueden procesar y que serán utilizados exclusivamente en su relación con Transbank.

Transacción Completa, le ofrece al comercio elegir una o ambas de las siguientes funcionalidades:

- Autorización en línea y captura simultánea, en la que se realiza el cargo al tarjetahabiente de inmediato.
- Autorización en línea y captura diferida, en la que se efectúa una reserva de crédito sobre un valor estimado del producto y/o servicio a adquirir por el tarjetahabiente, posteriormente el comercio define el monto de la transacción el cual será menor o igual al autorizado, en caso de ser superior la transacción será rechazada.

Por este motivo este producto no se encuentra dentro de la oferta publica de Transbank.


## Tipos de Pago

Los tipos de pago disponibles actualmente a través de Webpay dependen del tipo de tarjeta usada por el tarjetahabiente y los que tenga activado el comercio.

Para tarjeta de crédito pueden ser los siguientes tipos de pago (con las abreviaciones entre paréntesis):

- Venta Normal (VN): Pago en 1 cuota.
- 2 Cuotas sin interés (S2): El comercio recibe el pago en 2 cuotas iguales sin interés.
- 3 Cuotas sin interés (SI): El comercio recibe el pago en 3 cuotas iguales sin interés.
- N Cuotas sin interés (NC): El comercio recibe el pago en un número de cuotas iguales y sin interés que el tarjetahabiente puede elegir de entre un rango de 2 y N (el valor N es definido por el comercio y no puede ser superior a 12)
- Cuotas normales (VC): El emisor ofrece al tarjetahabiente entre 2 y 48 cuotas. El emisor define si son sin interés (si ha establecido un rango de cuotas en promoción) o con interés. El emisor también puede ofrecer de 1 hasta 3 meses de pago diferida. Todo esto sin impacto para el comercio que en esta modalidad de cuotas siempre recibe el pago en 48 horas hábiles.

Para tarjeta de débito Redcompra el tipo de pago siempre corresponde a:

- Venta débito Redcompra (VD): Pago con tarjeta de débito Redcompra.

- Venta Prepago (VP): Pago con tarjeta de débito Redcompra.

## Autorización y Captura

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#captura-diferida-webpay-plus' tbk-link-name='Referencia Api'></div>
</div>

Las transacciones Webpay cuentan con 2 fases: autorización y captura. La **autorización** se encarga de validar si es posible realizar el cargo a la cuenta asociada a la tarjeta de crédito realizando en el mismo acto la reserva de monto de la transacción. La **captura** hace efectiva la reserva hecha previamente o cargo en la cuenta de crédito asociada a la tarjeta del titular.

Para tarjetas de débito Redcompra, la autorización y captura es siempre simultánea. Para tarjetas de crédito, la autorización y captura puede ser simultánea o separada. En este último caso, se trata de una **captura diferida**.

Desde el punto de vista de la transacción, lo que ocurre es lo siguiente:

- **Autorización y captura simultánea:** La transacción es validada en línea por Transbank. El cargo del pago se hace simultáneamente en la tarjeta de crédito o débito Redcompra del cliente.

- **Autorización y captura diferida (Sólo válida para tarjetas de crédito):** Se retiene el valor de la compra del saldo de la tarjeta de crédito del cliente, reservando cupo pero sin realizar la transacción  definitivamente hasta que el comercio confirma la compra (vía captura diferida) y lo comunique a Transbank. Existe un **tiempo máximo de 7 días calendario** para realizar la captura. Superado ese tiempo la retención de la tarjeta de crédito será reversada y el cupo liberado. La captura puede realizarse a través del [portal Transbank](https://www.transbank.cl/web/login) o mediante [el servicio web de captura diferida](/referencia/webpay#captura-diferida-webpay-plus).

## Anulaciones

<div class="pos-title-nav">
  <div tbk-link='/referencia/webpay#anulacion-webpay-plus' tbk-link-name='Referencia Api'></div>
</div>

Las transacciones Webpay **realizadas con tarjeta de crédito** pueden ser anuladas mediante servicios web. Esta funcionalidad no aplica para tarjetas de débito Redcompra.

Las anulaciones pueden ser totales o parciales. **Las anulaciones parciales solo están permitidas en la modalidad de Venta Normal (VN)**. En caso que la transacción haya sido abonada al comercio, la anulación generará una retención en los siguientes abonos por el monto previamente autorizado.

El comercio tiene un plazo de 90 días desde la fecha de venta para anular transacciones vía servicios web.

<div class="container slate">
  <div class='slate-after-footer'>
    <div class='row d-flex align-items-stretch'>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22 text-center'>¿Tienes alguna duda de integración?</h3>
        <a href='https://join-transbankdevelopers-slack.herokuapp.com/' target='_blank'>
          <div class='td_block_gray'>
            <img src="https://p9.zdassets.com/hc/theme_assets/138842/200037786/logo.png" alt="" >
            <div class='td_pa-txt'>
              Únete a la comunidad de integradores en Slack y comparte buenos tips ayudando a los nuevos o buscando soluciones alternativas. Nuestro equipo está ahí para ayudarte.
            </div>
          </div>
        </a>
      </div>
      <div class='mt-3 mt-lg-0 col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22 text-center'>Si aún tienes dudas envíanos un mensaje</h3>
        <a class="pointer magenta" data-toggle='modal' data-target='#modalContactForm'>
          <div class='td_block_gray'>
            <div class="fo-size-20 text-center sub-title_bloq"><i class="fas fa-envelope"></i> Envíanos un mensaje.</div>
            <div class='td_pa-txt'>
              Si necesitas resolver algún tipo de incidencia en el portal o si existe algún problema con tu integración y  que no has podido resolver, contáctanos a través de nuestro formulario.
            </div>
          </div>
        </a>
      </div>
    </div>
  </div>
</div>
