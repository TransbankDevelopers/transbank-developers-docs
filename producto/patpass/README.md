# PatPass

PatPass es el servicio de Transbank que permite el pago automático de cuentas mediante tarjetas de crédito. Es la solución ideal para pago de cuentas, centros educativos, aportes a fundaciones, y otros comercios e instituciones.

PatPass **opera exclusivamente con tarjetas de crédito (nacionales e internacionales)**. No se aceptan tarjetas de débito Redcompra. No se contempla el uso de cuotas.

PatPass se relaciona con los comercios y emisores mediante las siguientes plataformas:

- **Transdata** es la plataforma que Transbank pone a disposición de los comercios para que puedan revisar el resultado de los cobros generados a sus clientes suscritos a PatPass, permitiendo además la administración de su universo de suscripciones.

- **PatPass Emisor** es la plataforma que le permite al tarjetahabiente gestionar los pagos automáticos existentes en su tarjeta desde el sitio web de su banco emisor.

De manera complementaria a esas plataformas, **PatPass by Webpay** permite a los comercios disponibilizar las inscripciones de clientes de manera directa al tarjetahabiente, aprovechando la infraestructura y seguridad ofrecida por la pasarela Webpay.

## PatPass by Webpay

<div class="pos-title-nav">
  <div tbk-link='/documentacion/patpass' tbk-link-name='Documentación'></div>
  <div tbk-link='/referencia/patpass' tbk-link-name='Referencia Api'></div>
  <div tbk-link='/plugins/patpass' tbk-link-name='Plugins'></div>
</div>

Mediante PatPass by Webpay el tarjetahabiente genera un pago automático con Tarjeta de Crédito de un producto y/o servicio por un monto fijo, efectuando **el primer pago en línea a través de Webpay Plus y luego efectuándose el cobro mensualmente de forma automática** a través del motor de PatPass, con una fecha de término definida por el comercio.

En cuanto a monedas, PatPass by Webpay permite realizar cargos en Peso, UF y Dólar. Para cobros en pesos y UF PatPass asignará un mismo código de comercio. Para Dólar (en el caso en que el comercio haya firmado el *Anexo de Contrato Dólar*) se asignará un código de comercio diferente. Los cobros en UF serán valorizado al mismo día efectuado el cobro. En el caso que el cobro sea un día no hábil, éste se cobrará al día hábil siguiente.

PatPass by Webpay es un canal utilizado sólo para dar de alta pagos automáticos. Si el comercio requiere dar de baja un pago automático, debe utilizar los otros canales existentes (como Transdata).

**El comercio deberá almacenar la información de todas las transacciones efectuadas a través del producto PatPass by Webpay**, debiendo mantener los respectivos registros a lo menos por el plazo de un año desde la fecha de cada operación, a modo de respaldo de las ventas efectuadas y de los servicios prestados. Esta información deberá ser puesta a disposición de Transbank cada vez y tan pronto éste la requiera. La información mínima que deberá almacenar el Establecimiento Comercial por cada una de las transacciones es la siguiente:

- Código de Comercio asignado por Transbank.
- Número de Orden de Pedido.
- Fecha y hora de la transacción.
- Monto y moneda de la operación.
- Número de Código de Autorización entregado por Transbank.
- Identificación de Servicio.
- Descripción de bienes vendidos o servicios prestados.
- Nombre del tarjetahabiente que efectúa la compra.
- Campo de autenticación emisor.
- Fecha de vencimiento de la suscripción.
- Mail de contacto de Tarjetahabiente.

### Flujo de una transacción PatPass by Webpay

El flujo de una transacción PatPass by Webpay se puede resumir en los siguientes pasos:

1. El tarjetahabiente navega dentro de la página del comercio.
2. El tarjetahabiente selecciona la opción de pago PatPass by Webpay y completa la información requerida por el comercio (RUT, nombre, email, teléfono)
3. Luego de completar la información, el tarjetahabiente es redirigido a Webpay donde se le informa el monto y la fecha de término de su suscripción mensual. En el formulario de Webpay el tarjetahabiente ingresa la información de su tarjeta.
4. El emisor de la tarjeta autentica al tarjetahabiente antes de realizar la transacción financiera, con el objetivo de validar que la tarjeta este siendo utilizada por el titular.
5. Si la autenticación es exitosa, PatPass by Webpay contrasta la información del cliente enviada por el emisor con la información provista por el comercio para asegurar que coincidan. De ser así, se procede con la autorización y captura del monto cobrado.
6. Si dicha autorización y captura es exitosa, Transbank procede de forma automática y transparente (tanto para el comercio como para el tarjetahabiente) a realizar la inscripción de la recurrencia en la Plataforma PatPass.
7. Finalmente se enviará un correo electrónico al comercio y otro correo electrónico al tarjetahabiente con la información del pago automático activado.

Una vez completada la operación, el comercio podrá visualizar el detalle de las transacciones y consultar los cobros recurrentes mediante Transdata. Por parte del tarjetahabiente, podrá visualizar la inscripción de este cobro mensual a través del PatPass Emisor (HomeBanking) en su respectivo Banco. Allí podrá observar el canal de origen que corresponde a “Webpay Mensual” y la fecha de término de este cobro recurrente.

En caso que un cobro no sea exitoso o el pago automático sea dado de baja, el tarjetahabiente también recibirá una alerta por correo electrónico (tal como la recibe en la inscripción descrita anteriormente en el punto 7).

## Anulaciones PatPass

Una Anulación corresponde a la revocación de una venta ya autorizada y capturada por Transbank.

Las anulaciones de ventas podrán solicitarse por:

- Ingresando a portal web de Transbank.
- Llamando al Servicio al Cliente.

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