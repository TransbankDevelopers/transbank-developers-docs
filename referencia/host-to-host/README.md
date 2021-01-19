# Host to Host


<aside class="notice">
Cuando veas cosas como texto entre símbolos <strong>&lt; &gt;</strong> nos estamos refiriendo a caracteres ASCII que no son imprimibles o visible como texto, y cuando veas cosas con la forma <strong>0x00</strong> nos referimos a la representación hexadecimal de los caracteres ASCII.
</aside>

## Protocolo de Comunicación Caja - POS

La comunicación se realiza a través de un puerto serial `RS232`, a velocidades que van entre los 1200bps hasta 115200bps `8N1`
es decir `8` bits de datos, `N`ingún bit de paridad y `1` bit de parada.

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-comunicacion-caja-pos.png" alt="Diagrama de Comunicación Caja - POS">

 Termino    | Descripción
 -------    | -------
 `<STX>`    | Indica el inicio de un mensaje (texto) <br><i>Hex</i>: `0x02`
 DATOS      | Corresponde al commando a enviar al POS o la respuesta de este
 `<ETX>`    | Indica el fin de un mensaje (texto) <br><i>Hex</i>: `0x03`
 `LRC`      | Es un byte que se concatena al final del mensaje `<ETX>` y se calcula realizando la operación `XOR` byte a byte de `<DATOS>` + `<ETX>`
 `<ACK>`    | Representa la recepción correcta del mensaje enviado <br><i>Hex</i>: `0x06`
 `<NAK>`    | Representa la incorrecta recepción del mensaje enviado, o que el `LRC` del mensaje recibido no corresponde con el enviado. <br><i>Hex</i>: `0x15`
 Timeout1   | Es el tiempo de espera de la caja para recibir `<ACK>`/`<NAK>` por parte del POS Integrado antes de reintentar el envío del mensaje.
 Timeout2   | Es el tiempo de espera de el POS para recibir `<ACK>`/`<NAK>` por parte de la Caja antes de reintentar el envío del mensaje de respuesta.

<aside class="notice">
Todos los comandos que se envían al POS deben cumplir con el flujo antes mencionado.
</aside>

<aside class="notice">
Todos los mensajes intercambiados entre la caja y el POS Integrado cumplen con el formato: <strong>&lt;STX&gt;DATOS&lt;ETX&gt;LRC</strong>
</aside>

### Ejemplo de cálculo LRC

Dado el siguiente comando:
`<STX>0200|123|<ETX>`

Que en notación hexadecimal seria:
`0x02 0x30 0x32 0x30 0x30 0x7C 0x31 0x32 0x33 0x7C 0x03`

Para calcular el `LRC` debemos omitir el inicio de texto `<STX>` o `0x02`.

La operación entonces sería:

`(((((((((0x30 XOR 0x32) XOR 0x30) XOR 0x30) XOR 0x7C) XOR 0x31) XOR 0x32) XOR 0x33) XOR 0x7C) XOR 0x03)`

El resultado entonces seria `0x31` en hexadecimal o `1` en ASCII, por lo tanto, el mensaje completo para enviar al POS Integrado es:

```bash
<STX>                   DATOS                           <ETX>   LRC
 0x02    0x30 0x32 0x30 0x30 0x7C 0x31 0x32 0x33 0x7C    0x03    0x31
```

## Mensajes

### Comando lectura de tarjeta

En este punto la caja ya tiene construida la venta en su sistema, por lo cual ya cuenta con los datos
requeridos para iniciar el proceso de pago con Transbank con el comando 0100.
Desde este punto se debe registrar los datos de la transacción para ir complementando con los siguientes
comandos, pues frente a alguna caída los datos están resguardados para solicitar reversa o finalmente
imprimir el voucher con la transacción aprobada.

<aside class="warning">
El SDK Java y C no soportan el envío de mensajes intermedios. Por esta razón el 3º parámetro de la función en C es siempre falso.
</aside>

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-venta.png" alt="Diagrama de Secuencia Venta">

1. La caja envía el requerimiento y espera como respuesta `<ACK>`/`<NAK>`, en caso de que llegue un `<NAK>`, debe reintentar el envío del requerimiento 2 veces. Si recibe un `<ACK>` debe esperar la respuesta de la transacción.
2. El POS solicita los datos al usuario, y envía el requerimiento al Autorizador, en caso de ser aprobada, se guarda en Batch y se envía respuesta a la caja. En caso de ser rechazada se envía respuesta a la caja indicando el error. ([Ver Tabla de Respuestas](/referencia/posintegrado#tabla-de-respuestas))
3. La caja al recibir la respuesta envía un `<ACK>` si el mensaje está correcto, o un `<NAK>` para el caso en que el `LRC` no corresponde.
4. El POS al recibir el `<ACK>` vuelve al inicio a esperar un nuevo comando, para el caso que recibe un `<NAK>` vuelve a enviar la respuesta 2 veces más.

<strong>Solicitud de Venta</strong>

DATO        | LARGO     | COMENTARIO DATO       | VALOR POR DEFECTO
------      | ------    | ------                | ------    
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`| STX
`Comando`   | 4         | <i>valor ASCII</i>: `0100` <br><i>valor hexadecimal</i>: `0x30 0x31 0x30 0x30`| 
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`Local comercio OnUs`     | 2         | **Valor Numérico** <br /> 00 Comercio sin tarjetas propias 01-99 Comercios onus, TBK asigna un numero para lectura de tarjetas propias |**00**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`Entrega BIN`    | 1         | **Valor alfanumérico** <br />(Y: Si)<br />(N: No) <br />Sirve para conocer el bin de la tarjeta y poder realizar algún descuento a la venta por convenio con el banco |**N**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`Transacción offline`    | 1         | **Valor alfanumérico** <br />(Y: Si)<br />(N: No) <br />Ya no está permitido su uso | **N**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`Autoservicio` | 1         | **Valor alfanumérico** <br />(Y: Si)<br />(N: No) <br />| **N**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`Monto` | 18         | **Valor numérico (máximo)** <br />Monto de Compra (sin propina, sin vuelto) <br>Monto mínimo $50,00 o US$1,00 <br />Incluye dos decimales.|
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`Código de moneda`    | 2         | **Valor alfanumérico** <br />\|CL\| Pesos chilenos 152<br>\|US\| Dólares estadounidenses 840 | **CL**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`Tipo de tarjeta`       | 2         | **Valor alfanumérico** <br />Indicador del tipo de menú por el cual se realizó la transacción.<br>\|CR\| : CRÉDITO<br>\|DB\| : DÉBITO - PREPAGO<br>\|NB\| : NO BANCARIA<br>Valor de tipo en Tabla tipo de tarjeta<br>***Una venta hecha como debito puede ser autorizada como prepago*** | 
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`Lista montos de vuelto`       | 60         | **Valor numérico (máximo)** <br />Lista de montos de vuelto permitidos, separados por “;” Incluyen dos decimales, siempre se debe enviar las 4 opciones definidas por Transbank.<br>Campo paramétrico por punto de venta.<br>Sólo si “Tipo de tarjeta = DB” | **500000;**<br>**1000000;**<br>**2000000;**<br>**5000000;**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`Monto vuelto`       | 18         | **Valor numérico (máximo)** <br />\|0\| : No muestra menú en pinpad<br>\|\| : ø Muestra menú consultando por vuelto<br>Si \|n\|> 0 no se muestra menú. El valor debe corresponder a alguno enviado en el campo “Lista de montos de vuelto”<br>Vuelto solo existe en débito, enviar 0 en crédito<br>Campo c | **Ø**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`Monto propina/donación`       | 9         | **Valor numérico (máximo)** <br />Corresponde al monto propina o donación de la venta o anulación<br>(incluye dos decimales)<br/><br/>**Importante:** Si se desea pedir propina al Tarjeta Habiente y que este la confirme, se debe enviar este campo el valor en vacío (Ø )<br />**Para las anulaciones** se debe colocar el monto de la propina de la venta a anular, en caso de no tener propina **colocar un cero (0).** | **Ø**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| \||
`<ETX>`                 |  1        | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03` | ETX
`<LRC>`                   |  1        | Resultado del calculo del `LRC` del mensaje | 


Timeout máximo de espera por comando 110 de 35seg, ya que el PinPad espera 30seg a que el cliente opere
tarjeta, por lo tanto a los 30 segundas si no se opera tarjeta, devuelve un 110\|99.



DATO                    | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------                  | ------    | ------            | ------
`<STX>`                 |  1        | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`               |  4        | <i>Valor ASCII</i>:  `0110` <br><i>valor hexadecimal</i>: `0x30 0x31 0x31 0x30`
`Separador`             |  1        | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Código Respuesta`      |  2        | Valor Numérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Código de comercio`    | 12        | Valor Numérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Terminal ID`           |  8        | Valor Alfanumérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Ticket`                |  6        | Valor ASCII, Número de boleta o ticket
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Codigo de Autorizacion`|  6        | Valor ASCII
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Monto`                 |  9        | Valor Numérico <br><i>Largo máximo</i>: 9 <br><i>Largo mínimo</i>: 1
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Número Cuotas`         |  2        | Valor Numérico <br><i>Largo máximo</i>: 2 <br><i>Largo mínimo</i>: 1
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Monto Cuota`           |  9        | Valor Numérico **(Opcional)** <br><i>Largo máximo</i>: 9 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Últimos 4 Digitos`     |  4        | Valor Numérico **(Opcional)** <br><i>Largo máximo</i>: 4 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Número Operación`      |  6        | Valor Numérico, Correlativo de Transacción del POS **(Opcional)** <br><i>Largo máximo</i>: 6 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Tipo de Tarjeta`       |  2        | Valor ASCII <br><i>CR</i>: Crédito <br><i>DB</i>: Débito
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Fecha Contable`        |  6        | Valor ASCII. Se utiliza solo con ventas Débito **(Opcional)**
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Número de Cuenta`      | 19        | Valor ASCII. Se utiliza solo con ventas Débito **(Opcional)** <br><i>Largo máximo</i>: 19 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Abreviación Tarjeta`   |  2        | Valor ASCII **(Opcional)** <br>[Ver Tabla de abreviación de Tarjetas](/referencia/posintegrado#tabla-de-abreviacion-de-tarjetas)
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Fecha de Transacción`  |  2        | Valor ASCII **(Opcional)** <br><i>Formato</i>: `DDMMAAAA`
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Hora de Transacción`   |  6        | Valor ASCII **(Opcional)** <br><i> Formato</i>: `HHMMSS`
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Empleado`              |  4        | Valor Numérico **(Opcional)** <br><i>Largo máximo</i>: 4</i> <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Propina`               |  9        | Valor Numérico <br><i>Largo máximo</i>: 9 </i> <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`<ETX>`                 |  1        | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`                   |  1        | Resultado del calculo del `LRC` del mensaje


## Tabla de respuestas

Respuesta                                               | Código
------                                                  | -----------
Aprobado                                                | 00
Rechazado                                               | 01
Host no Responde                                        | 02
Conexión Fallo                                          | 03
Transacción ya Fue Anulada                              | 04
No existe Transacción para Anular                       | 05
Tarjeta no Soportada                                    | 06
Transacción Cancelada desde el POS                      | 07
No puede Anular Transacción Débito                      | 08
Error Lectura Tarjeta                                   | 09
Monto menor al mínimo permitido                         | 10
No existe venta                                         | 11
Transacción No Soportada                                | 12
Debe ejecutar cierre                                    | 13
No hay Tono                                             | 14
Archivo BITMAP.DAT no encontrado. Favor cargue          | 15
Error Formato Respuesta del HOST                        | 16
Error en los 4 últimos dígitos.                         | 17
Menú invalido                                           | 18
ERROR_TARJ_DIST                                         | 19
Tarjeta Invalida                                        | 20
Anulación. No Permitida                                 | 21
TIMEOUT                                                 | 22
Impresora Sin Papel                                     | 24
Fecha Invalida                                          | 25
Debe Cargar Llaves                                      | 26
Debe Actualizar                                         | 27
Error en Número de Cuotas                               | 60
Error en Armado de Solicitud                            | 61
Problema con el Pinpad interno                          | 62
Error al Procesar la Respuesta del Host                 | 65
Superó Número Máximo de Ventas, Debe Ejecutar Cierre    | 67
Error Genérico, Falla al Ingresar Montos                | 68
Error de formato Campo de Boleta MAX 6                  | 70
Error de Largo Campo de Impresión                       | 71
Error de Monto Venta, Debe ser Mayor que 0              | 72
Terminal ID no configurado                              | 73
Debe Ejecutar CIERRE                                    | 74
Comercio no tiene Tarjetas Configuradas                 | 75
Supero Número Máximo de Ventas, Debe Ejecutar CIERRE    | 76
Debe Ejecutar Cierre                                    | 77
Esperando Leer Tarjeta                                  | 78
Solicitando Confirmar Monto                             | 79
Solicitando Ingreso de Clave                            | 81
Enviando transacción al Host                            | 82
Error Cantidad Cuotas                                   | 88
Declinada                                               | 93
Error al Procesar Respuesta                             | 94
Error al Imprimir TASA                                  | 95

<aside class="warning">
Toda transacción cuyo código de respuesta en el POS Integrado sea distinto de `0` será considerada como un rechazo. Por secreto bancario el detalle de la causa del rechazo no será entregado al comercio.
</aside>

## Tabla de Abreviación de Tarjetas

Tarjeta         | Abreviación
------          | -----------
VISA            | VI
MASTERCARD      | MC
CABAL           | CA
CREDENCIAL      | CR
AMEX            | AX
CERRADA         | CE
DINNERS         | DC
PRESTO          | TP
MAGNA           | MG
MAS (CENCOSUD)  | TM
RIPLEY          | RP
EXTRA           | EX
CMR             | TC
REDCOMPRA       | DB
