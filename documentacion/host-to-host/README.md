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
A continuación, verás la documentación asociada a como imprimir los voucher. 
Puedes revisar la [Referencia](/referencia/host-to-host) para ver la información asociada a la comunicación y los comandos.

## Voucher
El pinpad entregará el voucher listo para imprimir, pero también entregará todos los datos
necesarios para que la misma caja construya el voucher por sí misma.
Si se requiere obtener el voucher duplicado la caja debe construir el voucher, sin los datos de PEL y
agregando la glosa duplicado.

### Consideraciones
**Tipo de transacciones**  

Estas son las glosas que se deberían desplegar en la cabecera del voucher:

Tipo de Tarjeta         | Tipo de transacción
------                  | -----------
CR                      | VENTA CRÉDITO
CR                      | ANULACIÓN CRÉDITO
DB                      | VENTA DEBITO
NB                      | VENTA NO BANCARIA
NB                      | ANULACIÓN NO BANCARIA
DB                      | VENTA PREPAGO
CR                      | RESERVA
CR                      | ANULACIÓN RESERVA
CR                      | NO SHOW
CR                      | ANULACIÓN NO SHOW
CR                      | CHECK IN
CR                      | ANULACIÓN CHECK IN
CR                      | REAUTORIZACIÓN
CR                      | ANULACIÓN REAUTORIZACIÓN
CR                      | CHECK OUT
CR                      | ANULACIÓN CHECK OUT
CR                      | CARGO DEMORADO
CR                      | ANULACIÓN CARGO DEMORADO
DB                      | RETIRO EFECTIVO DEBITO
CR                      | AVANCE EFECTIVO CRÉDITO
CR                      | ANULACIÓN AVANCE EFECTIVO CRÉDITO

**Tipos de tarjeta**  

Considerar que las tarjetas emitidas por las casas comerciales bajo una marca como VISA o Master
se consideran bancarias ya no tarjetas no bancarias.
Las tarjetas de prepago se considerarán tarjetas de débito.

Código    | Glosa
------    | -----------
CR        | CRÉDITO
DB        | DÉBITO
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
Algunas consideraciones en la impresión de los voucher de Transbank
1. Los vouchers deben contener exclusivamente la información solicitada en este manual. **Cualquier excepción debe ser autorizada y formalizada por Transbank.**
2. El nombre del cliente se puede imprimir solo en voucher con firma
3. Las líneas de vuelto no deben imprimirse si no están siendo utilizadas.
4. Las líneas de donación o propina no deben imprimirse si no están siendo utilizadas.
5. **Los vouchers de premio NO deben ser impresos en papel auto copiativo**. En caso de que
   el cliente solamente posea impresoras que utilicen papel autocopiativo, **debe notificarse a
   Transbank de esta situación, y es este último quien debe autorizar el que sea aceptado
   este tipo de impresión, pudiendo quedar fuera la funcionalidad de promociones en
   línea**.
6. Solo debe generarse **una** copia de un voucher de premio, frente a cualquier condición de
   borde si no se pudo imprimir el voucher, se pierde el premio, en el duplicado no debe
   figurar nada al respecto.
7. **NO es posible reimprimir el voucher de premio** ni realizar alusiones de premio en los
   vouchers de Transbank en caso de la impresión de un duplicado.
8. El código del premio y las glosas son entregadas por el PINPAD luego de validar la
   respuesta del requerimiento.
9. La impresión de voucher **DEBE** ser realizada en forma automática, al aprobarse la
   transacción, no opcional ni manual. No debe depender de un enter en caja para liberar la
   impresión
10. Para los casos en que sea necesaria la firma del tarjetahabiente, la impresión **DEBE**
    suponer un espacio adecuado para dicha operación.
11. El inicio de una nueva transacción **DEBE** realizarse **SOLO** al momento de finalizada la
    impresión del voucher de la transacción anterior, **NUNCA** durante su impresión o
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

## ONUS Host

### Objetivos  

Esta documentación describe la forma de operar, la funcionalidad y el detalle de la mensajería de un PINPAD TRANSBANK trabajando bajo la modalidad OnUs.  
La aplicación del PINPAD, supone la existencia de un ECR inteligente (por ejemplo una caja registradora) que enviará los requerimientos al PINPAD, para que este los procese y entregue los resultados cuando corresponda.

### Audiencia  
Para entender completamente esta documentación es necesario tener conocimientos transaccionales y conocer las funciones implementadas habitualmente en los PINPAD usados en las transacciones bancarias.  
En particular este manual está dirigido exclusivamente a los comercio ONUS que tienen tarjetas propias y utilizan el pinpad de Transabank para leer sus tarjetas, pero las transacciones las realizan y autorizan ellos mismos. 

### Alcance
Aplica para el equipo Verifone vx805 (Conexión Serial o USB)  
Aplica para el equipo Verifone e355 (Conexión Bluetooth)

Puedes revisar la [Referencia](/referencia/host-to-host#anexo-onus) para ver la información asociada a la comunicación y los comandos.

## Host to Host Pinpad Wifi

### Objetivos
Esta documentación describe la forma de operar, la funcionalidad y el detalle de la mensajería
de un PINPAD TRANSBANK mediante protocolo TCP/IP.
La aplicación del PINPAD, supone la existencia de un ECR inteligente (por ejemplo una caja
registradora) que enviará los requerimientos al PINPAD, para que este los procese y
entregue los resultados cuando corresponda.

### Audiencia
Para entender completamente este documento es necesario tener conocimientos
transaccionales y conocer las funciones implementadas habitualmente en los PINPAD usados
en las transacciones bancarias.

Puedes revisar la [Referencia](/referencia/host-to-host-wifi) para ver la información asociada a la comunicación y los comandos.


# Especificaciones técnicas y salidas especiales

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
SFTP creada en forma exclusiva para el respectivo comercio, para que este los descargue
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

### Archivo de cuadratura crédito (L3)
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
Dktt-Tr-Acum-Monto | Total. 11 enteros 2 decimales | Numérico | 13 | 34
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
DSK-TYP | Tipo de Transacción 0210 : venta en línea 0420 : reversa | Numérico | 4 | 6
DSK-TC | Código de Transacción4 "10" : Compra "18" : Compra con vuelto "30" : Retención | Numérico | 2 | 8
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

### Archivo de cuadratura débito (L3)
El archivo de cuadraturas débito contiene el registro al detalle de las transacciones
financieras efectuadas con tarjeta de débito. Contiene tanto las transacciones
procesadas en forma presencial como no presencial.

**Header**

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Dktt-Hr-Reg | Tipo de registro | Alfanumérico | 2 | 2
Dktt-Hr-Fech-Proc | Fecha De Proceso | Numérico | 6 | 8
Dktt-Hr-Hora-Proc | Hora De Proceso | Numérico | 6 | 14
Dktt-Hr-Glosa | Nombre Del Comercio <br > Nombre de Fantasía del comercio | Alfanumérico | 25 | 39
Filler | Disponible |  Alfanumérico | 201 | 240

**Footer**

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
DSK-TR-REG | Tipo de Registro. Valor "TR" | Alfanumérico | 2 | 2
DSK-TR-FECHA-PROC | Fecha de Proceso | Numérico | 6 | 8
DSK-TR-HORA-PROC | Hora de proceso | Numérico | 6 | 14
DSK-TR-TOT-REG | Total Registros | Numérico | 7 | 21
DSK-TR-MONTO | Monto Total. 11 enteros 2 decimales | Numérico | 13 | 34
DSK-TR-MONTO-COM | Monto Comisiones Total. 11 enteros 2 | Numérico | 13 | 47
FILLER | | Alfanumérico | 193 | 240

**Detalle**

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
DSK-DT-REG | Tipo de Registro Valor fijo “DT” | Alfanumérico | 2 | 2
DSK-TYP | Tipo de Transacción 0210 : venta en línea 0420 : reversa | Numérico | 4 | 6
DSK-TC | Código de Transacción4 "10" : Compra "18" : Compra con vuelto "30" : Retención | Numérico | 2 | 8
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

### Archivo de liquidaciones crédito (L5)
El archivo de liquidaciones crédito contiene el registro al detalle de los abonos y
retenciones sobre la cuenta del comercio dentro de un período determinado por
transacciones con tarjeta de crédito.

**Header**

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Abono - Desde | Fecha inicio periodo de abono ddmmaaaa | Numérico | 8 | 8
Filler | Relleno | Alfanumérico | 1 | 9
Abono – Hasta | Fecha de término periodo de abono ddmmaaaa | Numérico | 8 | 17
Filler | Relleno | Alfanumérico | 1 | 18
Proceso – Fecha | Fecha de proceso, en formato ddmmaaaa | Numérico | 8 | 26
Filler | Relleno | Alfanumérico | 1 | 27
Abono - Fecha | Fecha de pago de las transacciones, en formato ddmmaaaa | Numérico | 8 | 35

SI largo de código cliente es menor que 4

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Código cliente |  Número interno de cliente | Numérico | 3 | 38
Nombre de cliente | Nombre del comercio | Alfanumérico | 20 | 58
Filler | Relleno | Alfanumérico | 76 | 134
Filler | Contiene " HEADER" | Alfanumérico | 6 | 140
Filler | Relleno | Alfanumérico | 89 | 229

SI largo de código cliente es mayor que 3

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Código cliente |  Número interno de cliente | Numérico | 5 | 40
Nombre de cliente | Nombre del comercio | Alfanumérico | 20 | 60
Filler | Relleno | Alfanumérico | 74 | 134
Filler | Contiene " HEADER" | Alfanumérico | 6 | 140
Filler | Relleno | Alfanumérico | 89 | 229

**Footer**

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Sal - Contador | Número de transacciones a abonar | Numérico | 10 | 10
Filler | Relleno  | Alfanumérico | 1 | 11
Sal - Monto | Monto total de las ventas a abonar (11 enteros, 2 decimales) | Numérico | 13 | 24
Filler | Relleno  | Alfanumérico | 1 | 25
Sal - Rret. | Número de retenciones | Numérico | 10 | 35
Filler | Relleno  | Alfanumérico | 1 | 36
Sal - Ret. | Monto total de retenciones (11 enteros, 2 decimales) | Numérico | 13 | 49
Filler | Relleno  | Alfanumérico | 1 | 50
Sal - Ceic | Monto total de comisión más IVA de la comisión (Venta) (11 enteros, 2 decimales) | Numérico | 13 | 63
Sal - caeica | Monto total de comisión adicional más IVA adicional de la comisión (11 enteros, 2 decimales) | Numérico | 13 | 76
Sal - dceic | Monto total de comisión más IVA de la comisión (Retenciones) (11 enteros, 2 decimales) | Numérico | 13 | 89
Sal - dcaeica | Monto total de comisión adicional más IVA adicional de la comisión (Retenciones) (11 enteros, 2 decimales). | Numérico | 13 | 102
Filler | Relleno  | Alfanumérico | 32 | 134
Filler | Contiene " FOOTER" | Alfanumérico | 6 | 140
Filler | Relleno  | Alfanumérico | 89 | 229

**Detalle**

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Liq-Numc | Código de Comercio | Numérico | 8 | 8
Liq-Fproc | Fecha de Proceso, en formato ddmmaaaa | Numérico | 8 | 16
Liq-Fcom | Fecha de la venta o retención, en formato ddmmaaaa | Numérico | 8 | 24
Liq-Micr | Número de Microfilm | Alfanumérico | 8 | 32
Liq-Numta | Número de Tarjeta SI numPedido es nulo | Alfanumérico | 19 | 51
Liq-Marca | Tipo de Tarjeta. Los valores son, VI: Visa; MC: Master; DI: Diners; AX: Amex | Alfanumérico | 2 | 53
Liq-Monto | Monto de la venta o retención (11 enteros, 2 decimales) | Numérico | 11 | 64
Liq-Moneda | Tipo de Moneda. 0: pesos; 1: dólar. | Numérico | 1 | 65
Liq-Txs | Tipo de transacción. Si es venta:”VA” Venta a abonar “VP” Venta pareada con retención (“RP”). Si es retención:<br>“RP” Retención pareada con venta (“VP”) <br> “RA” Retención total aplicada <br>“RC” Saldo de retención aplicada en forma parcial. <br>“RE” Retención Pendiente | Alfanumérico | 2 | 67
Liq-Rete | Atributo de la transacción. Si es venta a abonar o pareada va “0000”. Si es venta 3CSI, va “C3C*” donde * es el número de la cuota. Si es venta Cuotas Comercio, va “I&&&”, donde &&& es la cantidad de cuotas, alineada a la derecha. Si es venta NCuotas, va “nnSI”, donde nn indica el número de cuotas (02 a 24) y SI indica: cuotas sin interés.Si es retención, va el código de retención. | Alfanumérico | 4 | 71
Liq-Cprin | Código de casa matriz del comercio. Si LiqNumc es la matriz, lleva “99999999”. | Numérico | 8 | 79
Liq-Fpago | Fecha de pago, en formato ddmmaaaa | Numérico | 8 | 87
Liq-orpedi | Número de Orden de Pedido o Código de Barra | Alfanumérico | 26 | 113
Liq-codaut | Código de autorización de transacción | Alfanumérico | 6 | 119
Liq-cuotas | Número de cuota que se está abonando y que opera para ventas efectuadas con los productos Ncuotas o Ventas en cuotas sin intereses. | Numérico | 2 | 121
Liq-vci | Valor de autentificación de la transacción (Venta) | Numérico | 4 | 125
Liq-ceic | Valor de la comisión más IVA de la comisión(Venta) | Numérico | 11 | 136
Liq-caeica | Valor de la comisión adicional más IVA adicional de la comisión | Numérico | 11 | 147
Liq-dceic | Valor de la comisión más IVA de la comisión(Retenciones) | Numérico | 11 | 158
Liq-dcaeica | Valor de la comisión adicional más IVA adicional de la comisión (Retenciones) | Numérico | 11 | 169
Liq-ntc | número total de cuotas de la venta original y que opera para ventas efectuadas con los productos NCuotas o Ventas en cuotas sin intereses. | Numérico | 2 | 171
Liq_Nombre_banco | Nombre del banco indicará el nombre del banco | Alfabético | 35 | 206
Liq_Tipo_cuenta_banco | Tipo de la cuenta del banco asociada al abono | Alfabético | 2 | 208
Liq_Número_cuenta_banco | Número de la cuenta del banco asociada al abono | Alfabético | 18 | 226
Liq_Moneda_cuenta_banco | Moneda de la cuenta de abono seleccionada | Alfabético | 3 | 229

### Archivo de liquidaciones débito (L5)
El archivo de liquidaciones débito contiene el registro al detalle de los abonos y
retenciones sobre la cuenta del comercio dentro de un período determinado por
transacciones con tarjeta de débito.

**Header**

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Abono-Desde | Fecha inicio periodo de abono Formato ‘ddmmaa’ | Numérico | 6 | 6
Filler | Disponible | Alfanumérico | 1 | 7
Abono-Hasta | Fecha final periodo de abono Formato ‘ddmmaa’ | Numérico | 6 | 13
Filler | Disponible | Alfanumérico | 4 | 17
Liqu-Fecha | Fecha de liquidación Formato ‘ddmmaa’ | Numérico | 6 | 23
Filler | Disponible | Alfanumérico | 1 | 24
Nombre-Fan | Nombre de fantasía del comercio | Alfanumérico | 25 | 49
Header | Indicador de registros “HEADER” | Alfanumérico | 6 | 55
Filler | Disponible | Alfanumérico | 160 | 215

**Footer**

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Liq-Ncom | Sumatoria de registros de detalle Para registros de detalle con Liq-Ttra = “00” | Numérico | 10 | 10
Filler | Disponible | Numérico | 1 | 11
Liq-Tcom | Sumatoria de campo Liq-Amt1. 11 enteros 2 decimales <br> Para registros de detalle con Liq-Ttra = “00” | Numérico | 13 | 24
Filler | Disponible | Numérico | 1 | 25
Liq-Nret | Sumatoria de registros de detalle de retenciones | Numérico | 10 | 35
Filler | Disponible | Alfanumérico | 1 | 36
Liq-Mret | Sumatoria de campo Liq-Amt1, para registros de detalle con Liq-Ttra =”03”. 11 enteros 2 decimales | Numérico | 13 | 49
Liq-Vret | Valores fijos de “000000000” | Numérico | 9 | 58
Liq-Tret | Valores fijos de “0000000000” | Numérico | 10 | 68
Footer | Indicador del tipo de registros “FOOTER” | Alfanumérico | 6 | 74
Liq-Tot-Com-comiv | Sumatoria de la comisión más IVA de la comisión (Venta). 11 enteros 2 decimales | Numérico | 13 | 87
Filler | Disponible | Alfanumérico | 1 | 88
Liq-Tot-decom-ivcom | Sumatoria de la comisión más IVA de la comisión (Retenciones). 11 enteros 2 decimales | Numérico | 13 | 101
Liq-Fill  | | Alfanumérico | 114 | 215

**Detalle**

Nombre | Descripción | Formato | Largo | Total
------ | ----------- | ------- | ----- | -----
Liq-Ccre | Código de Comercio interno de Transbank Es el código asignado al comercio por Transbank. No incluye el prefijo 5970. | Numérico | 8 | 8
Liq-Fpro | Fecha de proceso del archivo Formato ‘ddmmaa’. Corresponde a la fecha de proceso de la respectiva transacción. | Numérico | 6 | 14
Liq-Fcom | Fecha de compra Formato ‘ddmmaa’. Corresponde a la fecha en la cual se realizó la transacción | Numérico | 6 | 20
Liq-Appr | Código de Autorización Entregado por el autorizador | Alfanumérico | 6 | 26
Liq-Pan | Número de la tarjeta Sólo últimos cuatro dígitos, demás caracteres con asteriscos | Alfanumérico | 19 | 45
Liq-Amt1 | Monto de la transacción. 11 enteros 2 decimales | Numérico | 13 | 58
Liq-Ttra | Tipo de transacción = 0 : compra abonada <br> mayor que 0 : retención | Numérico | 2 | 60
Liq-Cpri | Valor fijo “99999999” | Numérico | 8 | 68
Liq-Marc | Retenciones Valor “RE” : para retenciones En blanco si se trata de una transacción abonada. | Alfanumérico | 2 | 70
Liq-Fedi | Fecha de Liquidación Formato ‘dd/mm/aa’ Corresponde a la fecha de abono de la transacción respectiva | Alfanumérico | 8 | 78
Liq-nro-unico | Número único | Alfanumérico | 26 | 104
Liq-com-comiv | Valor de la comisión más IVA de la comisión (Venta). 11 enteros 2 decimales | Numérico | 13 | 117
Liq-cad-cadiva | N/A Se rellena el largo con “0”. 11 enteros 2 decimales | Numérico | 13 | 130
Liq-decom-ivcom | Valor de la comisión más IVA de la comisión(Retenciones). 11 enteros 2 decimales | Numérico | 13  |143
Liq-dcoad-ivcom | N/A Se rellena el largo con “0”. 11 enteros 2 decimales | Numérico | 13 | 156
Liq_prepago | Prepago Valor “P” para registros que provienen de prepago. En blanco si se trata de una transacción de otro producto. | Alfanumérico | 1 | 157
Liq_Nombre_banco | Nombre del banco se indicará el nombre del banco | Alfabético | 35 | 192
Liq_Tipo_cuenta_banco | Tipo de la cuenta del banco asociada al abono | Alfabético | 2 | 194
Liq_Número_cuenta_banco | Número de la cuenta del banco asociada al abono | Alfabético | 18 | 212
Liq_Moneda_cuenta_banco | Moneda de la cuenta de abono seleccionada | Alfabético | 3 | 215


### Notas

* Todos los campos “Fecha de Proceso” explicados en este documento aplican a la
Fecha de Proceso del archivo
* La siguiente tabla indica qué transacciones pueden ocurrir tanto a través de Host
como POS de contingencia, y cuáles solo a través de una de ambas vías:

<table>
<tr>
<td></td><td colspan="3">Tipo de transacción</td>
</tr>
<tr>
    <th>Código de Transacción</th>
    <td>210</td>
    <td>220</td>
    <td>420</td>
</tr>
<tr>
    <th>10</th>
    <td>Host - POS </td>
    <td>Host - POS </td>
    <td>Host - POS </td>
</tr>
<tr>
    <th>13</th>
    <td>Webpay</td>
    <td></td>
    <td>Webpay</td>
</tr>
<tr>
    <th>14</th>
    <td>POS - Webpay</td>
    <td>Host</td>
    <td>Host - POS - Webpay</td>
</tr>
<tr>
    <th>18</th>
    <td>Host - POS </td>
    <td></td>
    <td>Host - POS </td>
</tr>
</table>


* Para las aplicaciones con mensajería SPDH 3.1 las transacciones de anulación
efectuadas a través del Host son consideradas como FS04 (fuera de línea), lo que
en el archivo quedaría representado como una 220-14
* Para efectos de parear transacciones, solo se deben considerar los registros con
código de transacción “10” (Compras), “13” (WebPay) y “18” (Compra con
vuelto).
* Para los registros correspondientes a transacciones retenidas (Código de
Transacción “30”), se informarán solamente los siguientes campos:
o Fecha de Transacción (Dktt-Dt-Tran-Dat)
o Código de comercio (Dktt-Dt-Id-Retailer)
o Número de Único (DSK-ID-NUM-UNICO)
o Monto de la transacción (Dktt-Dt-Amt-1)
* Para el caso de Liquidación el campo (liq-vci), podrá contener los valores TSY
(correctamente autenticada), ya que muestras solo autorizaciones ok nacionales.
En el caso de una transacción internacional, podrás visualizar TSY y A (intento
de autenticación)
* En los casos de archivo de cuadratura, el campo (Dktt-Dt-VCI) además de los
casos anteriores, también existirán los rechazos con TO (TimeOut) y TSN
(Transacción no autenticada).

### Archivo de saldos
Considera los saldos contables pendientes de abono para los productos Crédito ($ y US$)
y Débito. Contiene todas las transacciones que se encuentran procesadas hasta el cierre
contable (último día de proceso hasta las 14:00 hrs.) y que se encuentran pendientes
de abono (ventas) o cargo (retenciones).

**Material disponible en la casilla**
* Detalle Saldos Contables Pendientes de abono; Archivo que contiene todas las
transacciones pendientes de abono que respaldan el informe disponible en portal
* Detalle transacciones al cierre de Mes; Archivo que presenta el detalle de
transacciones al cierre del mes, que no han sido procesadas dentro del mes y
que no conforman el informe de saldo pendientes de abono.
Para cada casilla de cliente se le creará una carpeta llamada CARTOLA_SALDOS, dentro
de ella estará todos los archivos en donde su nombre identificará con PREFIJO_FECHA_RUT (DET_DDMMYYY_XXXXXXXXX).
  
DET_31102018_761349465.* (.txt)
COL_31122018_761349465.* (.txt)

**Estructura de archivo de texto**

Campo | Tipo Dato | Largo
--- | ---- | ---
FECHA PROCESO | DATE | 
RUT COMERCIO | STRING | 9
CÓDIGO COMERCIO | STRING | 8
TIPO CONTRATO | STRING | 2
DESCRIPCIÓN TIPO CONTRATO | STRING | 13
TIPO FLUJO | STRING | 4
DESCRIPCIÓN TIPO FLUJO | STRING | 2
FECHA VENTA | DATE |
FECHA ABONO | DATE | 
TARJETA | STRING | 19
NRO CUOTA | NUMBER | 2
MONTO CUOTA | NUMBER | 17,2
LNIN SEC | NUMBER | 23
FECHA PROCESO TXS | DATE | 
MONTO VENTA | NUMBER | 17,2
CÓDIGO AUTORIZACIÓN | STRING | 6
ORDEN PEDIDO | STRING | 26
ID SERVICIO | STRING | 20
PAREADA | STRING | 2



### Estructura nombres archivos saldos
Los nombres de archivos tendrán una nueva estructura, según el siguiente detalle:
```
<Formato de Salida>_<Periodo>_<Agrupación Archivo de Salidas>_<Agrupación de Transacciones>_<RutEnrolado ó CódigoComercio>_<Tipo Conexion><Modo Agrupación>
```

**`<Formato de Salida>`** corresponde al formato de salida:
LDN = Liquidación de débito
LCN = Liquidación de crédito
CDN = Cuadratura de débito
CCN = Cuadratura de crédito

**`<Periodo>`** corresponde a la fecha en formato DDMMAAAA, según la frecuencia
configurada:
* Diaria: Se genera un archivo diario en base a la fecha de proceso; esta es la
  frecuencia por defecto
* Mensual: Se genera un archivo por mes. Fecha asociada al penúltimo día hábil del
  mes

**`<Agrupación Archivo de Salidas>`** corresponde a la naturaleza de agrupación:
* RE: Archivo con información única asociada al RUT enrolado (Genera un archivo)
* CC: Archivo con información separada por código de comercio de la transacción
  (genera un archivo diferente para cada código de comercio)

**`<Tipo Conexión>`** corresponde al tipo de conexión de los códigos de comercio:
* UN: Se genera un único archivo. Información de archivos no es separada por tipo
  de conexión (cuando es un archivo único no es parte del nombre)
* SE: Se genera un archivo para transacciones con tipo de conexión presencial y no
  presencial
* NP: No presencial
* PR: presencial

**`<Modo Agrupación>`** corresponde al modo de agrupación de los códigos de comercio
SC: Sucursal Matriz (cuando está agrupado por RUT no se incluye en el nombre
del archivo)
RB: Rubro


# Procedimiento enlace y conexión Pinpad Bluetooth e355

## Flujo de enlace Pinpad Bluetooth

![](/images/documentacion/host2host/flujo-pp-blue-e355.png)

## Conexion Pinpad Bluetooth E355


### Encendido de equipo

**Paso 1**  

Al encender el equipo entra a la pantalla principal del aplicativo.

**Paso 2**  
  
Se debe digitar secuencial mente las teclas “* 0 ” para ingresar al “Menú de Conexión Bluetooth.
![](/images/documentacion/host2host/conexion-paso-2.png)


### Enlazar dispositivos

**Paso 3**  
  
En el Menú de conexión bluetooth.
Se selecciona la opción **1> Enlazar Dispositivo**

**Paso 4**  

Si anteriormente se realizo una búsqueda preguntara si desea **Buscar nuevamente**, si presiona 
**2> NO** mostrara los dispositivos encontrados en la búsqueda anterior.  

![](/images/documentacion/host2host/conexion-paso-4a.png)

*El **BT Name**, que es el nombre que será visible al resto de dispositivos Bluetooth*

![](/images/documentacion/host2host/conexion-paso-4b.png)

**Paso 5**  

Al finalizar la búsqueda muestra los dispositivos encontrados

![](/images/documentacion/host2host/conexion-paso-5.png)  

**Paso 6**  

Al seleccionar un dispositivo aparecerá esta
ventana para confirmar el enlace.

![](/images/documentacion/host2host/conexion-paso-6.png)  

**Paso 7**  

Al seleccionar el dispositivo aparecerá esta ventana para
confirmar el enlace.

![](/images/documentacion/host2host/conexion-paso-7a.png)  


Un mensaje similar aparecerá en el otro dispositivo, el
código que aparece deberá ser el mismo en ambos
dispositivos;
En este caso **648005**
Y seleccionamos YES y quedaran enlazados  

![](/images/documentacion/host2host/conexion-paso-7b.png)  


**Paso 8**  

Comercio debe aceptar la conexión Bluetooth.  
*Se debe coordina la visita al comercio - Contraparte Comercio*

## Revisar habilitación SPP

**SPP: Serial Profile interfaz (interfaz que emula una conexión puerto serie) se debe configurar en caso que este deshabilitado**  


**Paso 1**  

En el Menú Principal del administrador de bluetooth.
Si se Requiere habilitar SSP (Serial Port Profile) **3> Config MENU CONEC. BLUETOOTH SPP \[Desactivado\]**  

![](/images/documentacion/host2host/habilitacion-spp-paso-1.png)

**Paso 2**  

Selecciona la opción **2> Servidor SPP**  

![](/images/documentacion/host2host/habilitacion-spp-paso-2.png)

**Paso 3**  

Aparecerá esta Pantalla y Volverá al menú Principal

![](/images/documentacion/host2host/habilitacion-spp-paso-3.png)

## Borrar equipos enlazados

**Paso 1**  

En el Menú del Administrador de
Bluetooth, seleccionar la opción
**2> Dispositivos Enlazados**

![](/images/documentacion/host2host/borrar-equipos-paso-1.png)

**Paso 2**  

En esta opción se mostrará un menú
con todos los dispositivos que están
enlazados con el pinpad e355, quedan
registrados los dispositivos
anteriormente enlazados.  

![](/images/documentacion/host2host/borrar-equipos-paso-2.png)  

**Paso 3**  

Si seleccionamos el dispositivo, aparecerá este
menú que nos mostrar 2 opciones.
Si seleccionamos **SI** eliminamos el enlace, y
volvemos al menú principal.

![](/images/documentacion/host2host/borrar-equipos-paso-3.png)  


## Utilización del SDK


Para la integración con aplicaciones propias del
comercio, es necesario utilizar en los desarrollos
propios, las librerías incluidas en el SDK del equipo
Verifone e355.  
Estas son:
1. libPtr
![](/images/documentacion/host2host/libPtr.png)  

2. libVmf  
![](/images/documentacion/host2host/libVmf.png)  

Para facilitar el proceso de integración se utiliza la
referencia al recurso “pinpad” que contiene
funciones para la comunicación entre Pinpad y la
App del Comercio.
La estructura y el código de ejemplo están
incluidos en el archivo **demo_e355.zip**


### Importación de librerías y recurso Pinpad

![](/images/documentacion/host2host/sdk-import-lib-pinpad.png)  

### Declaración del activity principal y PINPadTransport

![](/images/documentacion/host2host/sdk-pinpadtransport.png)  


### Función para la conexión y desconexión

![](/images/documentacion/host2host/sdk-conexion-desconexion.png)  

### Ejemplo para enviar el comando 0100 (lectura tarjeta al pinpad) al Pinpad

![](/images/documentacion/host2host/sdk-comando-0100.png)  

### Función para recibir la respuesta del Pinpad

![](/images/documentacion/host2host/sdk-respuesta-pinpad.png)  


## Instalación en Windows

Cuando se realiza el enlace se crean 2 puertos COM que se enlazan al Bluetooth.
Para poder revisarlo se debe ingresar a: **Dispositivos**

**Paso 1**

![](/images/documentacion/host2host/windows-1.png)  

**Paso 2**

![](/images/documentacion/host2host/windows-2.png)  

**Paso 3**

![](/images/documentacion/host2host/windows-3.png)  

**Paso 4**

![](/images/documentacion/host2host/windows-4.png)  

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
