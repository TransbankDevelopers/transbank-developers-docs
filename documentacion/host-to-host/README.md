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


## Cuadratura y liquidación
En esta sección se describen los archivos de cuadratura y liquidación que se entregan al
cliente que utiliza el servicio de Salidas Especiales.
Los archivos de cuadratura (conciliación) son archivos planos que contienen los registros
de las transacciones (de venta y anulación) efectuadas en un comercio dentro de un
periodo determinado.
Los archivos de liquidación son archivos planos que contienen los registros de los abonos
y retenciones hechas sobre la cuenta del comercio dentro de un período determinado.

### Distribución
Los archivos de cuadratura y liquidación son colocados periódicamente en una casilla
SFTP creada en forma exclusiva para el respectivo comercio, para que éste los descargue
a su propio sistema.
La siguiente tabla indica la frecuencia y horario de colocación de archivos de cuadratura
y liquidación en la casilla:

Archivo     | Periodo abarcado | Colocación en casilla | Horario de copia de archivos
------      | -----------      | -----------           | ----------- 
Cuadratura  | 00ºº hrs día – 00ºº hrs día siguiente | Todos los días | 12:00 Martes 14:00
Liquidación  | 00ºº hrs día – 00ºº hrs día siguiente | Día hábil  | 12:00 Martes 14:00   

### Acceso a la casilla
Los comercios deberán utilizar alguna aplicación del tipo cliente SFTP disponible en el
mercado para el acceso a la casilla, algunas de estas pueden ser Filezilla, WinSCP, PuTTY,
MobaxTerm

El nombre de los archivos de las Salidas especiales (SSEE) se generará en forma dinámica a partir de los
datos ingresados en el enrolamiento más la fecha de proceso

```
<Formato de Salida>_<Periodo>_<Agrupación Archivo de Salidas>_<RutComercio ó CódigoComercio>_<Tipo Conexion>_<Agrupación de Transacciones>_<Número Agrupación>
```

**Formato de Salida**
* LDN: Liquidación de débito
* LCN: Liquidación de crédito
* CDN: Cuadratura de débito
* CCN: Cuadratura de crédito

**Periodo**
* Diaria: Se genera un archivo diario en base a la fecha de proceso
* Mensual: Se genera un archivo por mes. Fecha asociada al penúltimo día hábil
del mes

**Agrupación Archivo de Salidas**
* RE: Archivo con información única asociada al RUT enrolado (Genera un archivo)
* CC: Archivo con información separada por código de comercio de la transacción
(genera un archivo diferente para cada código de comercio)

**Tipo Conexión**
* Se genera un único archivo. Información de archivos no es separada por tipo de
conexión (cuando es un archivo único no es parte del nombre)
* Cuando se genera un archivo para transacciones con tipo de conexión presencial
y no presencial
o NP: No presencial
o PR: presencial

**Agrupación de transacciones**
* Cuando esta agrupado por RUT no se incluye en el nombre del archivo
* RB: Rubro

**Número Agrupación**
* Corresponderá al número del Rubro enrolado.

### Cuadratura crédito
A continuación, se describe en detalle la estructura de los registros de los archivos de
cuadratura crédito. Cada archivo tiene un registro de encabezamiento (“header”), luego
un registro por cada transacción efectuada en el periodo abarcado y al final un registro
de pie (“footer”).
El archivo de cuadraturas crédito contiene el registro al detalle de las transacciones
financieras efectuadas con tarjeta de crédito. Contiene tanto las transacciones
procesadas en forma presencial como no presencial.

**Header**

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Dktt-Hr-Reg | "HR" | Alfanumérico | 2 | 2
Dktt-Hr-Fech-Proc | Fecha De Proceso <br > Formato "AAMMDD" El día en el que se procesa el archivo es el día hábil inmediato al período abarcado. | Numérico | 6 | 8
Dktt-Hr-Hora-Proc | Hora De Proceso Formato "HHMMSS" | Numérico | 6 | 14
Dktt-Hr-Glosa | Nombre Del Comercio <br > Nombre de Fantasía del comercio | Alfanumérico | 25 | 39
Filler | Disponible |  Alfanumérico | 282 | 321

**Footer**
Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Dktt-Tr-Reg | Valor fijo "TR" | Alfanumérico | 2 | 2
Dktt-Tr-Fech-Proc | Fecha De Proceso Formato "AAMMDD" | Numérico | 6 | 8
Dktt-Tr-Hora-Proc | Hora De Proceso Formato "HHMMSS" | Numérico | 6 | 14
Dktt-Tr-Cant-Reg | Total De Registros | Numérico | 7 | 21
Dktt-Tr-Acum-Monto Monto | Total. 11 enteros 2 decimales Numérico 13 34
Dktt-Tr-Fech-Desde | Fecha menor de Txs <br> Formato "AAMMDD". De entre las transacciones registradas en el archivo indica la fecha en la que se realizó la más temprana.  | Numérico | 6 | 40
Dktt-Tr-Hora-Desde | Hora menor de Txs <br> Formato "HHMMSS". De entre las transacciones registradas en el archivo indica la hora en la que se realizó la más temprana.  | Numérico | 6 | 46
Dktt-Tr-Fech-Hasta | Fecha mayor de Txs. <br> Formato "AAMMDD". De entre las transacciones registradas en el archivo indica la fecha en la que se realizó la más tardía. | Numérico | 6 | 52
Dktt-Tr-Hora-Hasta | Hora mayor de Txs. <br> Formato "HHMMSS". De entre las transacciones registradas en el archivo indica la hora en la que se realizó la más tardía | Numérico | 6 | 58
Filler | Disponible | Alfanumérico | 263 | 321

**Detalle**
La siguiente tabla describe cada uno de sus campos:

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
DSK-DT-REG | Tipo de Registro Valor fijo “DT” | Alfanumérico | 2 | 2
DSK-TYP Tipo | de Transacción 0210 : venta en línea 0420 : reversa | Numérico | 4 | 6
DSK-TC Código | de Transacción4 "10" : Compra "18" : Compra con vuelto "30" : Retención | Numérico | 2 | 8
DSK-TRAN-DAT | Fecha de la Transacción <br>Formato “AAMMDD”. Corresponde a la fecha en la cual Transbank emitió respuesta a la transacción | Numérico | 6 | 14
DSK-TRAN-TIM |  Hora de la Transacción <br> Formato “HHMMSS”. Corresponde a la hora en la cual Transbank emitió respuesta a la transacción | Numérico | 6 | 20
DSK-ID-RETAILER | Código del Comercio Prestador | Numérico | 12 | 32
DSK-NAME-RETAILER | Nombre del Comercio <br>Nombre de fantasía del comercio | Alfanumérico | 20 | 52
DSK-CARD | Número de Tarjeta <br> Sólo aparecen los últimos 4 dígitos, los demás están enmascarados con * | Alfanumérico | 19 | 71
DSK-AMT-1 | Monto de la Compra. 11 enteros 2 decimales | Numérico | 13 | 84
DSK-AMT-2 | Monto del Vuelto. 11 enteros 2 decimales | Numérico | 13 | 97
DSK-AMT-PROPINA | Monto Propina. 7 enteros 2 decimales | Numérico | 9 | 106
DSK-RESP-CDE | Código de Respuesta <br>Emitido por Base-24. Los valores entre 000 y 009 indican transacción aprobada. |  Alfanumérico |  |3 109
DSK-APPRV-CDE | Código de Aprobación <br>“O De Autorización”. Entregado por el Emisor. | Alfanumérico | 8 | 117
DSK-TERM-NAME | Código del terminal POS Terminal ID | Alfanumérico | 16 | 133
DSK-ID-CAJA | Identificador de la Caja | Alfanumérico | 16 | 149
DSK-NUM-BOLETA | Número de Boleta | Alfanumérico | 10 | 159
DSK-FECHA-PAGO | Fecha de Pago <br> 1 día hábil después de la fecha de Proceso del archivo | Numérico | 6 | 165
DSK-IDENT | identificador del Host <br>Corresponde al “Identificador del Comercio” que aparece al principio del nombre del nombre del archivo | Alfanumérico | 2 | 167
DSK-ID-RETAILER | Código del Comercio Responsable <br>Código del comercio intermediario (si lo hubiese) entre el Comercio y la tarjeta habiente | Numérico | 8 | 175
DSK-ID-COD-SERVI | Código de Servicio | Alfanumérico | 20 | 195
DSK-ID-NRO-UNICO | Número único <br>Asignado por el comercio | Alfanumérico | 26 | 221
DSK-PREPAGO | Prepago <br>Valor “P” para registros que provienen de prepago. En blanco si se trata de una transacción de otro producto. | Alfanumérico | 1 | 222
FILLER | Disponible | Alfanumérico | 18 | 240

### Cuadratura débito (L3)
El archivo de cuadraturas débito contiene el registro al detalle de las transacciones
financieras efectuadas con tarjeta de débito. Contiene tanto las transacciones
procesadas en forma presencial como no presencial.


Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Dktt-Hr-Reg | Tipo de registro | Alfanumérico | 2 | 2
Dktt-Hr-Fech-Proc | Fecha De Proceso | Numérico | 6 | 8
Dktt-Hr-Hora-Proc | Hora De Proceso | Numérico | 6 | 14
Dktt-Hr-Glosa | Nombre Del Comercio <br > Nombre de Fantasía del comercio | Alfanumérico | 25 | 39
Filler | Disponible |  Alfanumérico | 201 | 240




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
