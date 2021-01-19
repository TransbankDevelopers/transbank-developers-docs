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

### Flujo de venta en Pinpad Host:

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


## Voucher
El pinpad entregará el voucher listo para imprimir, pero también entregará todos los datos
necesarios para que la misma caja construya el voucher por sí misma.
Si se requiere obtener el voucher duplicado la caja debe construir el voucher, sin los datos de PEL y
agregando la glosa duplicado

### Consideraciones
**Tipo de transacciones**
Estas son las glosas que se deberían desplegar en la cabecera del voucher:

Tipo de Tarjeta         | Tipo de transacción
------                  | -----------
CR                      | VENTA CREDITO
CR                      | ANULACION CREDITO
DB                      | VENTA DEBITO
NB                      | VENTA NO BANCARIA
NB                      | ANULACION NO BANCARIA
DB                      | VENTA PREPAGO
CR                      | RESERVA
CR                      | ANULACION RESERVA
CR                      | NO SHOW
CR                      | ANULACION NO SHOW
CR                      | CHECK IN
CR                      | ANULACION CHECK IN
CR                      | REAUTORIZACION
CR                      | ANULACION REAUTORIZACION
CR                      | CHECK OUT
CR                      | ANULACION CHECK OUT
CR                      | CARGO DEMORADO
CR                      | ANULACION CARGO DEMORADO
DB                      | RETIRO EFECTIVO DEBITO
CR                      | AVANCE EFECTIVO CREDITO
CR                      | ANULACION AVANCE EFECTIVO CREDITO

**Tipos de tarjeta**
Considerar que las tarjetas emitidas por las casas comerciales bajo una marca como VISA o Master
se consideran bancarias ya no tarjetas no bancarias.
Las tarjetas de prepago se considerarán tarjetas de débito.

Código    | Glosa
------    | -----------
CR        | CREDITO
DB        | DEBITO
NB        | NO BANCARIA
PR        | PREPAGO

**Marcas de tarjetas**

Nombre     | Abreviación
------     | -----------
VISA       | VI
MASTERCARD | MC
AMEX       | AX
DINERS     | DC
DISCOVER   | DC
MAGNA      | MG
PRESTO     | TP
MAS        | TM
RIPLEY     | TR
RIPLEY     | RP
CMR        | TC
DEBITO     | DB
ELECTRON   | EL
MAESTRO    | MT
PREPAGO    | PR


### Orden de impresión
La idea de este voucher es que sea la única salida para cualquier tipo de voucher de Transbank, al
ser un voucher único, independiente del tipo de transacción siempre se hacen las mismas
validaciones, para ver si se imprime o no cada campo, sea voucher de crédito, débito o anulación
de crédito.

Siempre se deben imprimir dos copias de voucher una para el comercio y otra para el cliente.
En el caso de voucher con firma si el comercio lo desea puede no imprimir en la copia del
comercio, las líneas de firmas, nombre del cliente y rut, en el voucher del comercio debe estar
firmado si aplica.

El orden de impresión debe ser:
1. Voucher de premio de TBK
2. Voucher del cliente de TBK
3. Voucher del comercio de TBK

Cualquier variación al orden o formato del voucher debe ser autorizada por Transbank

### Otras consideraciones
Algunas consideraciones en la impresión de los voucher de transbank
1. Los vouchers deben contener exclusivamente la información solicitada en este manual. Cualquier excepción debe ser autorizada y formalizada por Transbank.
2. El nombre del cliente se puede imprimir solo en voucher con firma
3. Las líneas de vuelto no deben imprimirse si no están siendo utilizadas.
4. Las líneas de donación o propina no deben imprimirse si no están siendo utilizadas.
5. Los vouchers de premio NO deben ser impresos en papel auto copiativo. En caso de que
   el cliente solamente posea impresoras que utilicen papel autocopiativo, debe notificarse a
   Transbank de esta situación, y es este último quien debe autorizar el que sea aceptado
   este tipo de impresión, pudiendo quedar fuera la funcionalidad de promociones en
   línea.
6. Solo debe generarse una copia de un voucher de premio, frente a cualquier condición de
   borde si no se pudo imprimir el voucher, se pierde el premio, en el duplicado no debe
   figurar nada al respecto.
7. NO es posible reimprimir el voucher de premio ni realizar alusiones de premio en los
   vouchers de Transbank en caso de la impresión de un duplicado.
8. El código del premio y las glosas son entregadas por el PINPAD luego de validar la
   respuesta del requerimiento.
9. La impresión de voucher DEBE ser realizada en forma automática, al aprobarse la
   transacción, no opcional ni manual. No debe depender de un enter en caja para liberar la
   impresión
10. Para los casos en que sea necesaria la firma del tarjeta habiente, la impresión DEBE
    suponer un espacio adecuado para dicha operación.
11. El inicio de una nueva transacción DEBE realizarse SOLO al momento de finalizada la
    impresión del voucher de la transacción anterior, NUNCA durante su impresión o
    reimpresión, dado que la transacción se da por finalizada al momento de la impresión del
    voucher o su reimpresión.
12. La separación entre los vouchers (de transacciones (original/copia), de premio) puede ser
    física (corte) o delimitada por líneas punteadas.
13. Lo último que debe ser impreso es el voucher Transbank
14. El voucher en impresora fiscal va entre las glosas. “INICIO COMENTARIO” y “FIN
    COMENTARIO”. Dado el número de líneas posibles de imprimir en este tipo de impresoras,
    está permitido la eliminación de líneas en blanco para que el voucher pueda ser impreso
    correctamente. Cada voucher podría ir separado por un inicio y fin de comentario, esto es
    el voucher de pel el de cliente y el del comercio.

### Voucher de venta o anulaciones
![](/images/documentacion/host2host/voucher-venta.png)

### Voucher de venta en ingles con surcharge
![](/images/documentacion/host2host/voucher-venta-surcharge.png)

### Voucher Pel y reimpresión
Solo se permite imprimir 1 vez el voucher original y el voucher de PEL, si existe algún problema con
la impresión del voucher original se debe abortar la impresión y permitir obtener duplicado.
Original:

1. Voucher de PEL
2. Voucher 1 comercio/cliente
3. Voucher 2 comercio/cliente

Duplicados:
1. Voucher 1 comercio/cliente “duplicado sin glosa pel”
2. Voucher 2 comercio/cliente “duplicado sin glosa pel”

### Voucher pel 1 - Premiación en línea
Se debe imprimir este voucher solo si:
**COMANDO** H505 **CAMPO** TIPO DE VOUCHER = Entrega Pto. de Venta

![](/images/documentacion/host2host/voucher-pel-1.png)

### Voucher pel 2 - Premiación en línea
Se debe imprimir este voucher solo si:
**COMANDO** H505 **CAMPO** TIPO DE VOUCHER = |2| Entrega Diferida

![](/images/documentacion/host2host/voucher-pel-2.png)

### Voucher pel 3 - Premiación en línea
Se debe imprimir este voucher solo si:
**COMANDO** H505 **CAMPO** TIPO DE VOUCHER =  |3| Devolución al Tarjeta Habiente

![](/images/documentacion/host2host/voucher-pel-3.png)


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
