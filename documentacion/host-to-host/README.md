# Host to Host

¿Manejas un comercio con un alto número de transacciones y gran cantidad de locales, que requieren gestionar de manera
centralizada las cuadraturas internas? Transbank tiene una buena alternativa para estos requerimientos.
Host to Host, solución que implica un desarrollo en la aplicación de las cajas de tu comercio para integrarla a los
equipos provistos por Transbank, conectándose vía enlace dedicado a nuestros servidores, lo que entregará robustez y
gran velocidad al proceso de pago y flujo de información.

## Cómo funciona

Host to Host requiere de un nivel de integración alto, que consta de las siguientes etapas:

1. **Desarrollo**
Tu comercio deberá realizar el desarrollo según las especificaciones que te entregará Transbank.

2. **Control de Calidad**
Este proceso verificará que el desarrollo de integración que hiciste,
cumple con los que te entregamos en las especificaciones, o si requiere alguna corrección o mejora.

3. **Etapa piloto**
En un lugar que acordemos juntos, llevamos a producción tu punto de venta pinpad host, donde con cliente reales haremos monitoreo en conjunto de su funcionamiento, por un periodo acotado de 2 semanas. Evaluamos los resultados, y acordamos masificación, tambien si se requiere algún ajuste.

4. **Masificación**
Construimos en conjunto un plan de instalación de los puntos de ventas.

<aside class="notice">
Para el desarrollo y pruebas del comercio Transbank entrega un set de pruebas que se deben ejecutar y un set de tarjetas físicas para estos casos, estas tarjetas solo funcionan en ambiente de desarrollo, certificación o integración.
</aside>

### **Flujo de venta en Pinpad Host:**

1. El cliente entrega los productos o servicio que desea comprar al vendedor.
2. El vendedor realiza la venta en su sistema de caja.
3. El vendedor pregunta al cliente como desea pagar, efectivo, tarjeta crédito, tarjeta débito y lo selecciona en la caja.
4. La caja invoca al pinpad, pasándole el monto de la venta y la forma de pago crédito o débito
5. El cliente opera tarjeta, selecciona cuotas si corresponde, e ingresa su pin
6. El pinpad arma el mensaje de requerimiento de venta y lo entrega a la caja.
7. La caja toma el mensaje lo entrega al host de comercio y este a su vez al host de Transbank (De ahí su nombre de host to host)
8. El mensaje de respuesta recorre el camino inverso hasta llegar al pinpad
9. El pinpad verifica la respuesta aprobada o rechaza e integridad del mensaje y entrega resultado a la caja
10. La caja imprime los comprobantes de venta.

## Equipos y conexiones disponibles

### Verifone vx805

Equipo Fijo USB o Serial Pinpad host
<img src="/images/documentacion/host2host/vx805.png" alt="Verifone vx805">

### Verifone vx680

Equipo Movil wifi Pinpad host
<img src="/images/documentacion/host2host/vx680.png" alt="Verifone vx680">

### Verifone e355

Equipo Movil Bluetooth Pinpad host
<img src="/images/documentacion/host2host/e355.png" alt="Verifone e355">

## Funciones de pago disponibles en HOST

* Venta Crédito, con o sin cuotas, propina o donación
* Anulación de venta Crédito.
* Venta Débito, débito con vuelto, propina o donación
* Ventas en modalidad ONUS (tarjetas propias del comercio)

## Cómo empezar

Estamos trabajando en integrar la documentación en un formato más amigable en esta misma sección.
De momento, ponemos a tu disposición los siguientes documentos en formato PDF

### Documentos disponibles

* **Manual de comandos HOST** - _RS20.1_ | [Descargar](/files/manual-comandos-2-9.pdf) <br />
_Este documento describe la forma de operar, la funcionalidad y el detalle de la mensajería entre el PINPAD de TRANSBANK y tu sistema de caja mediante comandos por puerto USB o SERIAL._
* **Manual de comandos ONUS HOST** - _2018.06.01 v1.6_ | [Descargar](/files/manual-comandos-pinpad-anexo-onus-1-6.pdf) <br />
_Este documento describe la forma de operar, la funcionalidad y el detalle de la mensajería de un PINPAD TRANSBANK trabajando bajo la modalidad OnUs._
* **Manual de comandos HOST Wifi** - _18.2 v2.4_ | [Descargar](/files/manual-comandos-pinpad-host-wifi.pdf) <br />
_Este documento describe la forma de operar, la funcionalidad y el detalle de la mensajería de un PINPAD TRANSBANK mediante protocolo TCP/IP._
* **Manual de voucher HOST** - _RS20.1_ | [Descargar](/files/manual-voucher-con-surcharge.pdf) <br />
_El pinpad entregará el voucher listo para imprimir, pero también entregará todos los datos necesarios para que la misma caja construya el voucher por sí misma._
* **Manual de especificaciones técnicas de salidas especiales** - _20201112_ | [Descargar](/files/manual-especificaciones-tecnicas-de-salidas-especiales-20201112.pdf) <br />
_En este anexo se describen los archivos de cuadratura (conciliación) y liquidación que se entregan al cliente Host.
Los archivos de cuadratura (conciliación) son archivos planos que contienen los registros de las transacciones
(de venta y anulación)_
* **Manual para enlazar Pinpad Bluetooth** - _v3.0_ | [Descargar](/files/manual-enlazar-pinpad-bluetooth-3-0.pdf) <br />
_Procedimiento enlace y conexión Pinpad Bluetooth e355_

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
