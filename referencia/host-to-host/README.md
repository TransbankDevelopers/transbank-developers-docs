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

<aside class="warning">
El SDK Java y C no soportan el envío de mensajes intermedios. Por esta razón el 3º parámetro de la función en C es siempre falso.
</aside>

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-venta.png" alt="Diagrama de Secuencia Venta">

1. La caja envía el requerimiento y espera como respuesta `<ACK>`/`<NAK>`, en caso de que llegue un `<NAK>`, debe reintentar el envío del requerimiento 2 veces. Si recibe un `<ACK>` debe esperar la respuesta de la transacción.
2. El POS solicita los datos al usuario, y envía el requerimiento al Autorizador, en caso de ser aprobada, se guarda en Batch y se envía respuesta a la caja. En caso de ser rechazada se envía respuesta a la caja indicando el error. ([Ver Tabla de Respuestas](/referencia/posintegrado#tabla-de-respuestas))
3. La caja al recibir la respuesta envía un `<ACK>` si el mensaje está correcto, o un `<NAK>` para el caso en que el `LRC` no corresponde.
4. El POS al recibir el `<ACK>` vuelve al inicio a esperar un nuevo comando, para el caso que recibe un `<NAK>` vuelve a enviar la respuesta 2 veces más.

<strong>Solicitud de Venta</strong>

## Diagrama de conexión 

### Conexión directa entre pinpad y caja

La caja del comercio y el pinpad se conectan a través de un cable de USB o SERIAL:

<img class="" src="">

### Conexión pinpad bluetooth y caja

La caja del comercio y el pinpad se conectan a través de Bluetooth:

<img class="" src="">

### Software de apoyo

Hay varios programas en internet para enviar comandos al puerto COM, uno que puede ayudarlo es:  
Docklight


## Protocolo de mensajería según tipo de comunicación

### Seguridad TLS 1.2

SSL (Secure Sockets Layer) o Capa de Conexiones Seguras. Es un protocolo que hace uso de certificados
digitales para establecer comunicaciones seguras a través de red. Desde el 2015 ha sido sustituido por TLS (Transport Layer Security) el cual está basado en SSL y son totalmente compatibles.  
La comunicación entre el pinpad y la caja no tiene seguridad, pero el comercio debe implementar seguridad interna en su red.

### Seguridad en la Red WI-FI del Comercio

En el caso de que la red del Comercio requiera usar conexiones por WI-FI se exige que la red sea una red cifrada ***WPA2-PSK (AES).**  

**Recomendaciones se seguridad para una red WI-FI:**
1. Cambiar regularmente la contraseña de red WI-FI  
Al cambiar regularmente la contraseña de red WI-FI evita que terceras personas puedan
hacer uso de la red del comercio.
2. Configurar la red WI-FI como “No visible”  
Al ocultar la red WI-FI se evita que personas externas al comercio puedan encontrar e intentar acceder a la red del comercio.  
Ahora cada vez que se intente conectar un nuevo dispositivo, será necesario colocar primero la *SSID, para posteriormente ingresar la contraseña.
3. Registrar y restringir las conexiones por MAC  
Al tener habilitadas las conexiones por MAC, se especifica que equipos pueden hacer uso de la red WI-FI, evitando que cualquier otro equipo haga uso de la red del comercio.
4. Restringir el acceso a la “Configuración del Router” desde WI-FI  
Al restringir el acceso a la configuración del Router desde conexiones WI-FI se evita que desde dispositivos WIFI se pueda acceder a esta configuración y se modifiquen sus parámetros.
5. Monitorear regularmente las conexiones WI-FI  
Los Router actuales permiten conocer los dispositivos que están conectados a la red WI-FI.  
Es recomendable monitorear para poder evitar algún dispositivo que esté conectado sin
autorización.

>>*WPA2-PSK (AES): Sistema de protección para redes inalámbricas WI-FI. Es el último estándar de encriptación WI-FI y AES es el más reciente algoritmo de cifrado.

>>*SSID: Difusión de un SSID de red. Un SSID es el nombre público de una red de área local inalámbrica (WLAN) que sirve para diferenciarla de otras redes inalámbricas en la zona. SSID es el nombre de la red que se especifica al configurar la red WI-Fi.  

### Protocolo de comunicación

El protocolo que usará el PINPAD es VISA II, sobre el que se enviarán los mensajes de requerimiento y respuesta. Conceptualmente se utiliza el siguiente formato:  

<img class="" src="">

<img class="" src="">

Donde:
TERMINO             | DESCRIPCIÓN
-------             | -----------
INICIO COMANDO      | Indica el inicio del mensaje (STX).
FIN COMANDO         | Indica el fin del mensaje (ETX).
SEPARADOR CAMPO     | Indica el separador de cada campo dentro de los comandos de requerimientos y respuestas. Valor ASCII “&#124;” (valor Hexa 0x7c)
LRC                 | Es un byte que se concatena luego del `<FIN COMANDO>` y que se calcula realizando un XOR byte a byte del mensajes, el cual consta de: `<DATA>` + `<FIN COMANDO>`.
ACK                 | Lo envía el PINPAD o la caja como aviso de recepción OK para todos los comandos (valor Hexa 0x06).
NAK                 | Lo envía el PINPAD o la caja cuando el LRC calculado no corresponde al enviado para todos los comandos (valor Hexa 0x15). 
Timeout1 ACK        | Es el tiempo de espera del ACK o NAK para reintentar el envío del requerimiento por la caja. 10 segundos
Timeout 2 Resp      | Es el tiempo de espera del ACK o NAK para reintentar el envío de la respuesta por el PINPAD. El tiempo depende de cada comando.
Timeout 3 ACK       | Es el tiempo de espera del ACK o NAK para reintentar el envío de la respuesta por el PINPAD. 10 segundos
STX                 | Indica un INICIO del mensaje (valor Hexa 0x02).
ETX                 | Indica un FIN del mensaje (valor Hexa 0x03).
DATA                | CAMPO0&#124;CAMPO1&#124;CAMPO2&#124;…&#124;CAMPON

### Diagrama genérico de secuencia de comandos 

El diagrama que se describe corresponde a la generalización del comportamiento de cada uno de los comandos o mensajes enviados entre pinpad y caja.  

<img class="" src="">

ACK: cuando se recibió un mensaje válido, se va a procesar y responder con el mensaje o comando
correspondiente dentro del timeout definido.  
NACK: cuando se recibió un mensaje no válido y no será procesado.  

Diagrama muestra la evaluación del comando por parte del pinpad y caja, se recibe un comando se evalúa su estructura, si está ok, se responde ACK, no está conforme a la documentación se responde NACK.  

Luego se valida la respuesta al código del comando, si 00 se procesa el comando si es distinto se termina por el código retornado.

<img class="" src="">

### Administración ID de Contexto

Al iniciar una transacción el pinpad entrega un ID que debe ser utilizado por la caja en los próximos comandos, si el ID no coincide el pinpad rechazará el comando, a menos que sea el inicio de otra transacción.  

Comando/campo donde la caja recibe el ID por parte del pinpad:  
**0110/Indicador de contexto**

### Flujo de ejecución de actualización de parámetro pinpad

Asumiendo que la caja tiene conexión con el pinpad, solicita al pinpad una actualización de parámetros de pinpad 

<img class="" src="">

TABLA CON CODIGOS   |
-----------------   |

### Flujo de venta detallado

<img class="" src="">

<img class="" src="">

1. Para el caso de una respuesta del pinpad que esté fuera de lo especificado (tipo de dato erróneo, largo incorrecto, separador incorrecto o faltante, etc.) la caja debe interpretar que la transacción terminó con problema y luego solicitar reversa según el “flujo de ejecución de reversa a solicitud de la caja” detallado a continuación.  
Si se desea reintentar la venta se debe iniciar un nuevo flujo de venta.

TABLA CON CODIGOS   |
-----------------   |

### Flujo de ejecución de reversa a solicitud de la caja

La caja no tuvo respuesta de algún comando o no tuvo respuesta de un mensaje SPDH en vuelo, por lo tanto no pudo terminar una venta que inicio, luego no sabe si está aprobada o rechazada, en este caso debe solicitar una reversa al pinpad.  

<img class="" src="">

Para el comando 400 se consideran como Reversa Exitosa los siguientes mensajes:

Comando             | Observación
-------             | -----------
510&#124;89&#124;   | Deprecado desde la 18.2x
510&#124;85&#124;   | Deprecado desde la 18.2x
510&#124;00&#124;...&#124;Cualquier Código respuesta Transbank&#124;… | Deprecado desde la 18.2x
510&#124;00&#124;...&#124;Código respuesta Transbank < 010&#124;…     | Respuesta optimizada en la 18.2x

Ejemplo de reversa:

TABLA CON CODIGO |
---------------  |

Con el 510 el pinpad muestra por pantalla “REVERSA APLICADA”.

## Descripción de comandos

En este capítulo se detalla cada comando que se puede enviar al PINPAD.
Para la comunicación serial es necesario indicar los caracteres de inicio y fin que se envían en
cada comando, los que serán indicados por medio de la siguiente nomenclatura:

CARÁCTER DE CONTROL     | NOMENCLATURA
-------------------     | ------------
`<STX><DATA><ETX><LRC>` | STXETX

A fin de mantener el mismo lenguaje en comunicación TCP-IP se mantienen estos caracteres STX, ETX y LRC.

### Comandos ventas

Los comandos 0100, 0110, 0200, 0210, 0400, 0410, 0500, 0510 son Igual al Retail Estándar vx805 versión 15.2 para dejarlos en un solo documento se anexan acá:

### 0100 – 0110 Comando Lectura de tarjeta

En este punto la caja ya tiene construida la venta en su sistema, por lo cual ya cuenta con los datos
requeridos para iniciar el proceso de pago con Transbank con el comando 0100.  
Desde este punto se debe registrar los datos de la transacción para ir complementando con los siguientes comandos, pues frente a alguna caída los datos están resguardados para solicitar reversa o finalmente imprimir el voucher con la transacción aprobada.

DATO        | LARGO     | COMENTARIO            | VALOR POR DEFECTO
------      | ------    | ------                | ------    
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`| STX
`Comando`   | 4         | <i>valor ASCII</i>: `0100` <br><i>valor hexadecimal</i>: `0x30 0x31 0x30 0x30`| 
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Local comercio OnUs` |2| **Valor Numérico** <br /> 00 Comercio sin tarjetas propias <br>01-99 Comercios onus, TBK asigna un numero para lectura de tarjetas propias |**00**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Entrega BIN` | 1       | **Valor alfanumérico** <br />(Y: Si)<br />(N: No) <br />Sirve para conocer el bin de la tarjeta y poder realizar algún descuento a la venta por convenio con el banco |**N**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Transacción offline`|1 | **Valor alfanumérico** <br />(Y: Si)<br />(N: No) <br />Ya no está permitido su uso | **N**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Autoservicio` | 1      | **Valor alfanumérico** <br />(Y: Si)<br />(N: No) <br />| **N**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto`     | 18        | **Valor numérico (máximo)** <br />Monto de Compra (sin propina, sin vuelto) <br>Monto mínimo $50,00 o US$1,00 <br />Incluye dos decimales.|
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de moneda` | 2  | **Valor alfanumérico** <br />&#124;CL&#124; Pesos chilenos 152<br>&#124;US&#124; Dólares estadounidenses 840 | **CL**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo de tarjeta`| 2    | **Valor alfanumérico** <br />Indicador del tipo de menú por el cual se realizó la transacción.<br>&#124;CR&#124;: CRÉDITO<br>&#124;DB&#124;: DÉBITO - PREPAGO<br>&#124;NB&#124;: NO BANCARIA<br>Valor de tipo en Tabla tipo de tarjeta<br>***Una venta hecha como debito puede ser autorizada como prepago*** | 
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Lista montos de vuelto`| 60 | **Valor numérico (máximo)** <br />Lista de montos de vuelto permitidos, separados por “;” Incluyen dos decimales, siempre se debe enviar las 4 opciones definidas por Transbank.<br>Campo paramétrico por punto de venta.<br>Sólo si “Tipo de tarjeta = DB” | **500000;**<br>**1000000;**<br>**2000000;**<br>**5000000;**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto vuelto`| 18      | **Valor numérico (máximo)** <br /> &#124;0&#124;: No muestra menú en pinpad<br> &#124; &#124; : ø Muestra menú consultando por vuelto<br>Si &#124;n&#124; > 0 no se muestra menú. El valor debe corresponder a alguno enviado en el campo “Lista de montos de vuelto”<br>Vuelto solo existe en débito, enviar 0 en crédito<br>Campo c | **Ø**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto propina/donación`| 9 | **Valor numérico (máximo)** <br />Corresponde al monto propina o donación de la venta o anulación<br>(incluye dos decimales)<br/><br/>**Importante:** Si se desea pedir propina al Tarjeta Habiente y que este la confirme, se debe enviar este campo el valor en vacío (Ø )<br />**Para las anulaciones** se debe colocar el monto de la propina de la venta a anular, en caso de no tener propina **colocar un cero (0).** | **Ø**
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`     | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`     | 1         | Byte resultado de la operación XOR del mensaje | 


Timeout máximo de espera por comando 110 de 35seg, ya que el PinPad espera 30seg a que el cliente opere tarjeta, por lo tanto a los 30 segundas si no se opera tarjeta, devuelve un 110&#124;99.


DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0110` <br><i>valor hexadecimal</i>: `0x30 0x31 0x31 0x30` |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta PinPad`| 2  | **Valor Numérico**<br>En caso de rechazo se debe desplegar en el punto de venta:<br>RECHAZO PINPAD - `<XX>` : `<GLOSA>`<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto` | 16| **Valor alfanumérico** <br> Formato aaaammddhhmmssmm <br>Es solo un ID, la fecha y hora en el pinpad puede estar desactualizada. |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo de captura` | 2       | **Valor numérico**<br>Este campo será utilizado en el futuros<br>&#124;00&#124; : B - Banda<br>&#124;01&#124; : E . EMV c/contactoa<br>&#124;02&#124; : C - Contacless<br>&#124;03&#124; : F - Fallback<br>&#124;04&#124; : D - Digitada |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`TRACK I`       | 80        | **Valor alfanumérico (máximo)** <br>Rellenados con blancos (0x20) a la derecha<br>Si “Local comercio OnUs ≠ 00”<br>Con pan encriptado se entrega 160 caracteres alfanuméricos que corresponde a 80 HEXA |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`TRACK II`      | 40        | **Valor alfanumérico (máximo)** <br>Rellenados con blancos (0x20) a la derechaa<br>Si “Local comercio OnUs ≠ 00”<br>Con pan encriptado se entrega 80 caracteres alfanuméricos que corresponde a 40 HEXA |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`PAN SHA-1`     | 40        | **Valor alfanumérico** <br>PAN encriptado con algoritmo SHA-1<br>Si “Transacción offline = Y” |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`BIN`           | 6         | **Valor Numérico** <br>Si “Entrega BIN = Y” o “Transacción offline = Y” |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`4 últimos dígitos` | 4     | **Valor Numérico** | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre tarjetahabiente`| 26| **Valor alfanumérico (máximo)** |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre marca de la tarjeta`| 20 | **Valor alfanumérico (máximo)** <br>De acuerdo a [Tabla de marcas](/referencia/host-to-host#tabla-de-marcas) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Abreviación de la tarjeta` | 2 | **Valor alfanumérico** <br>De acuerdo a [Tabla de marcas](/referencia/host-to-host#tabla-de-marcas) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag solicita 4 últimos dígitos` | 1 |**Valor alfanumérico** <br> (Y: El punto de venta debe solicitar los 4ud)<br>(N: El pinpad solicitará el ingreso de PIN)<br>En caso de anulación no se ingresa pin, tampoco se solicita ingreso de 4ud.    | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

### 0200 – 0210 Comando Requerimiento de venta/anulación


DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0200` <br><i>valor hexadecimal</i>: `0x30 0x32 0x30 0x30` | **0200**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto`         | 18        | **Valor alfanumérico (máximo)** <br>Monto de Compra (sin propina, sin vuelto)<br>Monto mínimo $50,00 o US$1,00<br>Incluye dos decimales |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número Ticket/Boleta `| 10 | **Valor alfanumérico**<br>Si comercio no utiliza este campo enviar el campo un cero | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Numero de Cuotas`| 2       | **Valor numérico**<br>Obligatorio si “Tipo de transacción = 01”<br>Si la venta original fue sin cuotas se debe informar el valor 00 |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Campo Impresión`| 1        | **Valor numérico**<br>Indica si entrega voucher formateado <br>0: No envía voucher (utiliza comandos 500-510)<br>1: Envía voucher (utiliza comandos 540-550) | **1**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Enviar Mensajes`| 1        | **Valor numérico**<br>Indica si el PINPAD debe enviar mensajes de estatus de la transacción<br>0: No envía mensajes (Valor por defecto)<br>1: Envía mensajes | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`| 16 | **Valor alfanumérico**<br>Id entregado por el pinpad por cada transacción |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo de transacción`| 2    | **Valor numérico**<br>(00: Venta)<br>(01: Anulación) |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de comercio`| 12    | **Valor numérico**<br>Código del comercio entregado por TBK y configurado en la caja.<br>EJ: 597012345678 |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Terminal ID `  | 16        | **Valor alfanumérico**<br>DDLL o Dirección lógica entregada por TBK y configurada en la caja en tabla de parametros. |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto cuotas ` | 9         | **Valor numérico**<br>Obligatorio si Tipo de transacción = 01. Si la venta original fue sin cuotas se debe informar el valor “0” |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Producto `     | 1         | **Valor numérico**<br>Obligatorio si Tipo de transacción = 01. Corresponde al campo “Tipo cuotas” del comando 0510 de la venta original | **Ø**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
~~`Número de empleado`~~| 6 | **Valor alfanumérico**<br>Campo sin uso enviar vacío siempre | **Ø**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número único`  | 26        | **Valor numérico**<br>Caja debe enviar el número único para imprimir en el voucher tanto en venta como anulación.<br>Cada comercio puede definir un formato para el numero único, TBK entrega el siguiente<br>ejemplo: AAAAMMDDHHMMSSLLLCCCXXXXXX<br>Donde LLL es un número del local <br>CCC es el número de la caja o punto de venta<br>XXXXXX es un contador<br>Campo a  |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de dependiente (Empleado)`| 4 | **Valor numérico**<br>Campo de Empleado es opcional<br>Si comercio no lo utiliza enviar el campo vacío | **Ø**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Índice interno del comercio`| 16  | **Valor alfanumérico (máximo)**<br>Campo que puede ser utilizado por el comercio para agregar información que le sirva a sus procesos internos, numero caja, vendedor, ID venta, etc. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de autorización transacción original`| 8 | **Valor alfanumérico (máximo)**<br>Código de autorización de la venta original, obligatorio si es una anulación “Tipo de transacción= 01” | **Ø**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número de secuencia transacción original`| 9 | **Valor alfanumérico (máximo)**<br>Número de secuencia de la venta original, obligatorio si es una anulación “Tipo de transacción= 01”<br>También conocido como número de operación | **Ø**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Fecha transacción original`| 6 | **Formato AAMMDD**<br>Fecha de la venta original, obligatorio si es una anulación “Tipo de transacción = 01” | **Ø**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Hora transacción original` | 6 | **Formato HHMMSS**<br>Hora de la venta original, obligatorio si es una anulación “Tipo de transacción = 01” | **Ø**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`4 últimos dígitos`| 4      | **Valor numérico**<br>Si es una anulación “Tipo de transacción = 01”, se debe ingresar los 4ud de la venta original, contra este dato el pinpad comparará con la tarjeta deslizada para anular.<br>Si “Tipo de transacción = 00” y “Flag solicita 4 últimos dígitos = Y” del comando 0110, se debe ingresar los 4ud de la tarjeta deslizada. | **Ø**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

Timeout máximo de espera por comando 210 de 125seg, ya que hay hasta 4 interacción con el usuario.


DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0210` <br><i>valor hexadecimal</i>: `0x30 0x32 0x31 0x30` | **0210**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta PinPad`| 2 | **Valor Numérico**<br>En caso de rechazo se debe desplegar en el punto de venta:<br>RECHAZO PINPAD - `<XX>` : `<GLOSA>`<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`| 4  | **Valor alfanumérico**<br>Id entregado por el pinpad por cada transacción  |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Largo mensaje` | 4         | **Valor numérico** |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Mensaje SPDH`  | 2048      | **Valor alfanumérico (máximo)** |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

### 0400 – 0410 Comando Requerimiento de reversa

Si se requiere una reversa, la caja rescata el indicador de contexto que ha guardado de su última transacción (independiente del nivel de completitud de la transacción) y solicita al pinpad la reversa. El pinpad puede discriminar si solo se hizo lectura de tarjeta, o solo se hizo una consulta de cuotas, o si la venta está rechazada o aprobada.  
Si el pinpad no está presente, debe mantener la reversa pendiente, hasta que el pinpad pueda responder.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0400` <br><i>valor hexadecimal</i>: `0x30 0x34 0x30 0x30` | **0400**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`| 16 | **Valor alfanumérico**<br>Id de la transacción que se quiere reversar  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

Timeout máximo de espera por comando 410 de 20seg y no requiere interacción con tarjetabiente.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0410` <br><i>valor hexadecimal</i>: `0x30 0x34 0x31 0x30` | **0410**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta PinPad`| 2 | **Valor numérico**<br>En caso de rechazo se debe desplegar en el punto de venta:<br>RECHAZO PINPAD - `<XX>` : `<GLOSA>`<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`| 16 | **Valor alfanumérico**<br>Id entregado por el pinpad por cada transacción   |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Largo mensaje` | 4         | **Valor numérico** |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Mensaje SPDH`  | 2048      | **Valor alfanumérico (máximo)** |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

### 0500 – 0510 Comando Requerimiento de validación/actualización

En este comando 0510 la caja debe validar si la respuesta final (Flag terminal) y si está aprobada (Código respuesta Transbank) para luego imprimir. Si la transacción no es final (Flag terminal) debe renviar el mensaje spdh adjunto, no importa si la transacción está o no aprobada en este caso.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0500` <br><i>valor hexadecimal</i>: `0x30 0x35 0x30 0x30` | **0500**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`| 16 | **Valor alfanumérico**<br>Id entregado por el pinpad por cada transacción  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Largo mensaje` | 4         | **Valor numérico** |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Mensaje SPDH`  | 2048      | **Valor alfanumérico (máximo)** | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

Timeout máximo de espera por comando 510 de 125seg, ya que hay hasta 4 interacción con el usuario.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0510` <br><i>valor hexadecimal</i>: `0x30 0x35 0x31 0x30` | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta PinPad`| 2 | **Valor numérico**<br>En caso de rechazo se debe desplegar en el punto de venta:<br>RECHAZO PINPAD - `<XX>` : `<GLOSA>`<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`| 16 | **Valor alfanumérico**<br>Id entregado por el pinpad por cada transacción  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de comercio`| 12    | **Valor numérico**<br>Código del comercio entregado por TBK y configurado en la caja, se imprime en voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Terminal ID `  | 16        | **Valor alfanumérico**<br>Dirección lógica entregada por TBK y configurada en la caja, se imprime en voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número Ticket/Boleta`| 20  | **Valor alfanumérico**<br>Campo opcional, si viene se imprime en voucher si no viene se omite el campo | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Empleado`      | 4         | **Valor alfanumérico**<br>Campo opcional, si viene se imprime en voucher si no viene se omite el campo | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código Autorización`| 8    | **Valor alfanumérico (máximo)**<br>Código de autorización de la transacción enviado por TBK ejemplo: &#124;AB 12 C3&#124;<br> Se imprime lo que viene en el voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto`         | 18        | **Valor numérico (máximo)** <br />Monto total autorizado (incluye el monto de la venta, propina, vuelto y donación según sea el caso)<br>Se imprime en voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto vuelto`  | 18        | **Valor numérico (máximo)** <br />Vuelto seleccionado por cliente, solo aplica en debito<br> Se imprime en voucher| 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Numero de Cuotas`| 2       | **Valor numérico**<br>Cantidad de cuotas de la transacción (para ventas sin cuotas se informa “00”)<br>Se imprime en voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto cuota`   | 14        | **Valor numérico**<br>Si el monto informado es vacío &#124;&#124; o &#124;0&#124; caja debe omitir la línea completa en el voucher.<br>Se imprime en voucher si viene el campo |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Últimos 4 Dígitos Tarjeta` | 4 | **Valor numérico**<br>No se imprime | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número Operación`| 9       | **Correlativo de transacción del terminal**<br>También conocido como número de secuencia este campo se debe imprimir en voucher de venta y anulación. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa Tipo de Tarjeta `| 7 | **Valor alfanumérico (máximo)**<br>Valor de glosa en [Tabla tipo de tarjeta](/referencia/host-to-host#tabla-de-tipo-de-tarjeta) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Fecha Contable `| 6        | **Valor alfanumérico**<br>Se utiliza sólo si es transacción de Debito<br>Caja no debe formatear (ej: DDAAMM), simplemente debe transferir el valor al voucher (XX/XX/XX) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número de Cuenta`| 19      | **Valor alfanumérico**<br>Número de tarjeta enmascarado para incluir en el voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Abreviación de la tarjeta` | 2 | **Valor alfanumérico** <br>Valor a imprimir en el voucher |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Fecha Transacción`| 6      | **Formato AAMMDD**<br>Valor a imprimir en el voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Hora Transacción` | 6      | **Formato HHMMSS**<br>Valor a imprimir en el voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Campo Impresión`  | 8192   | **Campo depende si la caja requiere voucher formateado (máximo)**<br>En este comando no se envía el voucher. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Transacción premiada`| 1   | **Valor numérico**<br>&#124;1&#124;: transacción premiada<br>En este caso caja debe imprimir voucher PEL además del de venta<br>&#124;&#124;: transacción sin premio  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo promoción`| 1         | **Valor numérico**<br>&#124;1&#124;: Entrega Pto. de Venta<br>&#124;2&#124;: Entrega Diferida<br>&#124;3&#124;: Devolución al Tarjeta Habiente  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código promoción`| 8       | **Valor alfanumérico**<br>Valor a imprimir en el voucher premiado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre promoción`| 21      | **Valor alfanumérico**<br>Valor a imprimir en el voucher de premio | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa vale premio`| 62     | **Valor alfanumérico**<br>Valor a imprimir en el voucher de premio | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Texto vale premio`| 27     | **Valor alfanumérico**<br>Valor a imprimir en el voucher de premio | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag permite cuotas`| 1    | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag de gracia`| 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag C2C`      | 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag C3C`      | 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag NCuotas`  | 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag máximo de cuotas`| 2  | **Valor numérico**<br>Campo informativo de la configuración del comercio | **00**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo de menú`  | 2         | **Valor alfanumérico**<br>Indicador del tipo de menú por el cual se realizó la transacción<br>&#124;CR&#124; : CRÉDITO<br>&#124;DB&#124; : DÉBITO PREPAGO<br>&#124;NB&#124; : NO BANCARIA<br>Valor de tipo en [Tabla tipo de tarjeta](/referencia/host-to-host#tabla-de-tipo-de-tarjeta)<br>Una venta hecha como debito puede ser autorizada como prepago | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador transacción con gracia`| 1 | **Valor numérico**<br>Indicador de modalidad de la transacción<br>&#124;0&#124; transacción sin mes gracia<br>&#124;1&#124; transacción con mes gracia | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo cuotas`   | 1         | **Valor numérico**<br>&#124;0&#124; Sin cuotas<br>&#124;1&#124; Cuotas normales<br>&#124;3&#124; C3C o C2C<br>&#124;4&#124; CIC o N-cuotas | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tasa aplicada` | 4         | **Valor numérico**<br>Solo se imprime en voucher si “Flag imprimir tasa = 1” | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa tipo cuota`| 30      | **Valor alfanumérico**<br>Glosa a imprimir en voucher<br>Si el campo informado viene vacío “&#124;&#124;” caja debe omitir la línea en el voucher. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa tipo cuota 2`| 22    | **Valor alfanumérico**<br>Glosa que se despliega en pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa promoción`| 10       | **Valor alfanumérico**<br>Glosa que se despliega en pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Id promoción`  | 10        | **Valor alfanumérico**<br>Glosa que se despliega en pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag imprimir tasa`| 1     | **Valor numérico**<br>&#124;&#124; o &#124;0&#124; no imprime tasa aplicada<br>&#124;1&#124; imprime tasa aplicada | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Periodo diferido`  | 3     | **Valor numérico**<br>Periodo diferido seleccionado, valor a imprimir en voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 1 periodo`| 3     | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 1 valor tasa`| 4  | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 1 valor cuota`| 14| **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 2 periodo`| 3     | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 2 valor tasa`| 4  | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 2 valor cuota`| 14| **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 3 periodo`| 3     | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 3 valor tasa`| 4  | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 3 valor cuota`| 14| **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número de secuencia transacción original` | 9 | **Valor alfanumérico (máximo)** <br />También conocido como número de operación original de la venta. No se está usando este campo, no se imprime | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código respuesta Transbank`| 3 | **Valor numérico** <br />Código de respuesta una vez finalizada la transacción. Se debe desplegar en el punto de venta.<br>EJ: RESPUESTA TRANSBANK - `<XXX>` : `<GLOSA>` | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa respuesta Transbank` | 48 | **Valor alfanumérico (máximo)** <br />Glosa que despliega el pinpad una vez finalizada la transacción. Se debe desplegar en el punto de venta.<br>EJ: RESPUESTA TRANSBANK - `<XXX>` : `<GLOSA>`| 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag transacción con PIN`| 1 | **Valor alfanumérico** <br />Y: Transacción autentificada con PIN<br>N: Transacción autentificada por firma | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre tarjetahabiente`| 26| **Valor alfanumérico** <br />Sólo imprimir si “Flag tipo voucher = 1, 2 ó 3” | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag tipo voucher` | 1     | **Valor numérico** <br />Según el número recibido se debe imprimir voucher con o sin firma:<br>&#124;0&#124; = Sin firma<br>&#124;1&#124; o &#124;2&#124; o &#124;3&#124; = con firma<br><br>**Cabeceras de los voucher:**<br>Para ventas con crédito: “VENTA CREDITO”<br>Para ventas con débito (siempre sin firma): “VENTA DEBITO”<br>Para ventas con no bancaria: “VENTA NO BANCARIA”<br>Para ventas con prepago (sin firma): “VENTA PREPAGO”<br>Para anulaciones con crédito (sin firma):“ANULACION CREDITO”<br>Para anulaciones con no bancaria (sin firma): “ANULACION NO BANCARIA” | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag modalidad cuotas`| 1  | **Valor alfanumérico** <br />0: Modalidad 3.1 (No utilizado)<br>1: Modalidad cuotas 4.0 | **1**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa transacción afecta a ahorro` | 40 | **Valor alfanumérico (máximo)** <br />Se debe imprimir en el voucher cuando sea distinta de vacío<br>Campo 9, subcampo D | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número de secuencia` | 9   | **Valor numérico** <br />No se está usando este campo, este no se imprime<br>También conocido como número de operación | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag mensaje terminal` | 1 | **Valor alfanumérico** <br />Y: El mensaje es terminal y NO se debe enviar el mensaje SPDH de respuesta<br>N: Se debe enviar el mensaje SPDH de respuesta | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Largo mensaje` | 4         | **Valor numérico**  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Mensaje SPDH Venta/Reversa`| 2048 | **Valor alfanumérico (máximo)** | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

### 0520 – 0530 Comando Requerimiento de validación/actualización

Solo para pinpad wifi retail estándar. 

### 0540 – 0550 Comando Requerimiento de validación/actualización

En este comando 0540 la caja debe validar si la respuesta final (Flag terminal) y si está aprobada (Código respuesta Transbank) para luego imprimir. Si la transacción no es final (Flag terminal) debe renviar el mensaje spdh adjunto, no importa si la transacción está o no aprobada en este caso.  
Entrega un voucher formateado para impresión y además soporta tarjetas de Prepago.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0540` <br><i>valor hexadecimal</i>: `0x30 0x35 0x34 0x30` | **0540**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`| 16 | **Valor alfanumérico**<br>Id entregado por el pinpad por cada transacción | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Largo mensaje` | 4         | **Valor Numérico** | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Mensaje SPDH`  | 2048      | **Valor alfanumérico (máximo)** | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre Comercio`| 40       | **Valor alfanumérico**<br>Campo paramétrico en caja enviado al pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Dirección Comercio`| 40    | **Valor alfanumérico**<br>Campo paramétrico en caja enviado al pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Comuna Comercio`| 40       | **Valor alfanumérico**<br>Campo paramétrico en caja enviado al pinpad<br>Puede ser comuna o ciudad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

Timeout maximo de espera por comando 540 de 125seg, ya que hay hasta 4 interacción con el usuario.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0550` <br><i>valor hexadecimal</i>: `0x30 0x35 0x31 0x30` | **0550**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta PinPad`| 2 | **Valor Numérico**<br>En caso de rechazo se debe desplegar en el punto de venta:<br>RECHAZO PINPAD - `<XX>` : `<GLOSA>`<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`| 16 | **Valor alfanumérico**<br>Id entregado por el pinpad por cada transacción  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de comercio`| 12    | **Valor numérico**<br>Código del comercio entregado por TBK y configurado en la caja, se imprime en voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Terminal ID `  | 16        | **Valor alfanumérico**<br>Dirección lógica entregada por TBK y configurada en la caja, se imprime en voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número Ticket/Boleta `| 20 | **Valor alfanumérico**<br>Campo opcional, si viene se imprime en voucher si no viene se omite el campo | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Empleado`      | 4         | **Valor alfanumérico**<br>Campo opcional, si viene se imprime en voucher si no viene se omite el campo | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código Autorización `| 8   | **Valor alfanumérico (máximo)**<br>Código de autorización de la transacción enviado por TBK ejemplo: &#124;AB 12 C3&#124;<br> Se imprime lo que viene en el voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto`         | 18        | **Valor numérico (máximo)** <br />Monto total autorizado (incluye el monto de la venta, propina, vuelto y donación según sea el caso)<br>Se imprime en voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto vuelto`  | 18        | **Valor numérico (máximo)** <br />Vuelto seleccionado por cliente, solo aplica en debito<br> Se imprime en voucher| 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Numero de Cuotas`| 2       | **Valor numérico**<br>Cantidad de cuotas de la transacción (para ventas sin cuotas se informa “00”)<br>Se imprime en voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto cuota`   | 14        | **Valor numérico**<br>Si el monto informado es vacío &#124;&#124; o &#124;0&#124; caja debe omitir la línea completa en el voucher.<br>Se imprime en voucher si viene el campo |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Últimos 4 Dígitos Tarjeta` | 4 | **Valor numérico**<br>No se imprime | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número Operación`| 9       | **Correlativo de transacción del terminal**<br>También conocido como número de secuencia este campo se debe imprimir en voucher de venta y anulación. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa Tipo de Tarjeta `| 7 | **Valor alfanumérico (máximo)**<br>Valor de glosa en [Tabla tipo de tarjeta](/referencia/host-to-host#tabla-de-tipo-de-tarjeta) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Fecha Contable `| 6        | **Valor alfanumérico**<br>Se utiliza sólo si es transacción de Debito<br>Caja no debe formatear (ej: DDAAMM), simplemente debe transferir el valor al voucher (XX/XX/XX) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número de Cuenta`| 19      | **Valor alfanumérico**<br>Número de tarjeta enmascarado para incluir en el voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Abreviación de la tarjeta` | 2 | **Valor alfanumérico** <br>Valor a imprimir en el voucher |
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Fecha Transacción`| 6      | **Formato AAMMDD**<br>Valor a imprimir en el voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Hora Transacción`| 6       | **Formato HHMMSS**<br>Valor a imprimir en el voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Campo Impresión`| 8192     | **Campo depende si la caja requiere voucher formateado (máximo)**<br>Se envía voucher siempre | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Transacción premiada`| 1   | **Valor numérico**<br>&#124;1&#124;: transacción premiada<br>En este caso caja debe imprimir voucher PEL además del de venta<br>&#124;&#124;: transacción sin premio  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo promoción`| 1         | **Valor numérico**<br>&#124;1&#124;: Entrega Pto. de Venta<br>&#124;2&#124;: Entrega Diferida<br>&#124;3&#124;: Devolución al Tarjeta Habiente  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código promoción`| 8       | **Valor alfanumérico**<br>Valor a imprimir en el voucher premiado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre promoción`| 21      | **Valor alfanumérico**<br>Valor a imprimir en el voucher de premio | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa vale premio`| 62     | **Valor alfanumérico**<br>Valor a imprimir en el voucher de premio | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Texto vale premio`| 27     | **Valor alfanumérico**<br>Valor a imprimir en el voucher de premio | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag permite cuotas`| 1    | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag de gracia`| 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag C2C`      | 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag C3C`      | 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag NCuotas`  | 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag máximo de cuotas`| 2  | **Valor numérico**<br>Campo informativo de la configuración del comercio | **00**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo de menú`  | 2         | **Valor alfanumérico**<br>Indicador del tipo de menú por el cual se realizó la transacción<br>&#124;CR&#124; : CRÉDITO<br>&#124;DB&#124; : DÉBITO PREPAGO<br>&#124;NB&#124; : NO BANCARIA<br>Valor de tipo en [Tabla tipo de tarjeta](/referencia/host-to-host#tabla-de-tipo-de-tarjeta)<br>Una venta hecha como debito puede ser autorizada como prepago | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador transacción con gracia`| 1 | **Valor numérico**<br>Indicador de modalidad de la transacción<br>&#124;0&#124; transacción sin mes gracia<br>&#124;1&#124; transacción con mes gracia | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo cuotas`   | 1         | **Valor numérico**<br>&#124;0&#124; Sin cuotas<br>&#124;1&#124; Cuotas normales<br>&#124;3&#124; C3C o C2C<br>&#124;4&#124; CIC o N-cuotas | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tasa aplicada` | 4         | **Valor numérico**<br>Solo se imprime en voucher si “Flag imprimir tasa = 1” | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa tipo cuota`| 30      | **Valor alfanumérico**<br>Glosa a imprimir en voucher<br>Si el campo informado viene vacío “&#124;&#124;” caja debe omitir la línea en el voucher. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa tipo cuota 2`| 22    | **Valor alfanumérico**<br>Glosa que se despliega en pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa promoción`| 10       | **Valor alfanumérico**<br>Glosa que se despliega en pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Id promoción`  | 10        | **Valor alfanumérico**<br>Glosa que se despliega en pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag imprimir tasa`| 1     | **Valor numérico**<br>&#124;&#124; o &#124;0&#124; no imprime tasa aplicada<br>&#124;1&#124; imprime tasa aplicada | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Periodo diferido`| 3       | **Valor numérico**<br>Periodo diferido seleccionado, valor a imprimir en voucher | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 1 periodo`| 3     | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 1 valor tasa`| 4  | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 1 valor cuota`| 14| **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 2 periodo`| 3     | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 2 valor tasa`| 4  | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 2 valor cuota`| 14| **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 3 periodo`| 3     | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 3 valor tasa`| 4  | **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 3 valor cuota`| 14| **Valor numérico**<br>No utilizado | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número de secuencia transacción original` | 9 | **Valor alfanumérico (máximo)** <br />También conocido como número de operación original de la venta. No se está usando este campo, no se imprime | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código respuesta Transbank`| 3 | **Valor numérico** <br />Código de respuesta una vez finalizada la transacción. Se debe desplegar en el punto de venta.<br>EJ: RESPUESTA TRANSBANK - `<XXX>` : `<GLOSA>` | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa respuesta Transbank` | 48 | **Valor alfanumérico (máximo)** <br />Glosa que despliega el pinpad una vez finalizada la transacción. Se debe desplegar en el punto de venta.<br>EJ: RESPUESTA TRANSBANK - `<XXX>` : `<GLOSA>`| 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag transacción con PIN`  | 1 | **Valor alfanumérico** <br />Y: Transacción autentificada con PIN<br>N: Transacción autentificada por firma | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre tarjetahabiente` |26| **Valor alfanumérico** <br />Sólo imprimir si “Flag tipo voucher = 1, 2 ó 3” | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag tipo voucher` | 1     | **Valor numérico** <br />Según el número recibido se debe imprimir voucher con o sin firma:<br>&#124;0&#124; = Sin firma<br>&#124;1&#124; o &#124;2&#124; o &#124;3&#124; = con firma<br><br>**Cabeceras de los voucher:**<br>Para ventas con crédito: “VENTA CREDITO”<br>Para ventas con débito (siempre sin firma): “VENTA DEBITO”<br>Para ventas con no bancaria: “VENTA NO BANCARIA”<br>Para ventas con prepago (sin firma): “VENTA PREPAGO”<br>Para anulaciones con crédito (sin firma):“ANULACION CREDITO”<br>Para anulaciones con no bancaria (sin firma): “ANULACION NO BANCARIA” | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag modalidad cuotas` | 1 | **Valor alfanumérico** <br />0: Modalidad 3.1 (No utilizado)<br>1: Modalidad cuotas 4.0 | **1**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa transacción afecta a ahorro` | 40 | **Valor alfanumérico (máximo)** <br />Se debe imprimir en el voucher cuando sea distinta de vacío<br>Campo 9, subcampo D | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número de secuencia` | 9   | **Valor numérico** <br />No se está usando este campo, este no se imprime<br>También conocido como número de operación | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag mensaje terminal` | 1 | **Valor alfanumérico** <br />Y: El mensaje es terminal y NO se debe enviar el mensaje SPDH de respuesta<br>N: Se debe enviar el mensaje SPDH de respuesta | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Largo mensaje` | 4         | **Valor numérico**  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Mensaje SPDH Venta/Reversa`| 2048 | **Valor alfanumérico (máximo)** | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Propina`       | 18        | **Valor numérico**<br>Monto Propina o Donación | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Voucher de Rechazo` | 1024 | **Valor numérico**<br>Cuando transacción es declinada por EMV se debe imprimir un voucher especial.<br>Si este campo viene vacío no se imprime, si viene con dato se imprime<br>Este voucher se imprime solo, sin voucher de venta | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Voucher de PEL`| 1024      | **Valor alfanumérico**<br>Voucher de PEL si viene la caja debe imprimirlo, solo una vez junto al voucher de venta<br>No se debe imprimir en duplicado<br>Este voucher se imprime junto al de venta | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`LABEL - EMV`   | 32        | **Valor alfanumérico**<br>Si el campo viene con datos caja debe incluirlo en el voucher en la posición indicada | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`RID - EMV`     | 32        | **Valor alfanumérico**<br>Si el campo viene con datos caja debe incluirlo en el voucher en la posición indicada | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Modelo pinpad` | 6         | **Valor numérico**<br>Caja debe incluirlo en el voucher ejemplo: VX805<br>Campo obligatorio | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Versión de pinpad` | 6     | **Valor numérico**<br>Caja debe incluirlo en el voucher ejemplo: 12.34A<br>Campo obligatorio | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Saldo Prepago `| 40        | **Valor alfanumérico (máximo)**<br>Indica el saldo de una tarjeta de prepago la cual se debe imprimir en voucher cuando es venta de prepago y cuando viene el saldo.<br>Nota: El Pinpad agrega esa glosa al voucher tal como viene en la mensajería. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

### 0560 – 0570 Comando Requerimiento de validación/actualización

Solo para pinpad wifi retail estándar


### 0580 – 0590 Comando Requerimiento con capacidad Surcharge

En este comando 0580 la caja debe validar si la respuesta final (Flag terminal) y si está aprobada (Código
respuesta Transbank) para luego imprimir. Si la transacción no es final (Flag terminal) debe renviar el
mensaje SPDH adjunto, no importa si la transacción está o no aprobada en este caso.
Entrega un voucher formateado para impresión. Además, soporta tarjetas de Prepago y Surcharge.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0580` <br><i>valor hexadecimal</i>: `0x30 0x35 0x38 0x30` | **0580**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`| 16 | **Valor alfanumérico**<br>Id entregado por el pinpad por cada transacción | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Largo mensaje` | 4         | **Valor Numérico** | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Mensaje SPDH`  | 2048      | **Valor alfanumérico (máximo)** | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre Comercio`| 40       | **Valor alfanumérico**<br>Campo paramétrico en caja enviado al pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Dirección Comercio`| 40    | **Valor alfanumérico**<br>Campo paramétrico en caja enviado al pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Comuna Comercio`| 40       | **Valor alfanumérico**<br>Campo paramétrico en caja enviado al pinpad<br>Puede ser comuna o ciudad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

Timeout máximo de espera por comando 540 de 125seg, ya que hay hasta 4 interacción con el usuario.


DATO                | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------              | ------    | ------            | ------
`<STX>`             | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`           | 4         | <i>Valor ASCII</i>:  `0590` <br><i>valor hexadecimal</i>: `0x30 0x35 0x39 0x30` | **0590**
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta PinPad`| 2 | **Valor Numérico**<br>En caso de rechazo se debe desplegar en el punto de venta:<br>RECHAZO PINPAD - `<XX>` : `<GLOSA>`<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`| 16     | **Valor alfanumérico**<br>Id entregado por el pinpad por cada transacción  | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de comercio`| 12        | **Valor numérico**<br>Código del comercio entregado por TBK y configurado en la caja, se imprime en voucher | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Terminal ID `      | 16        | **Valor alfanumérico**<br>Dirección lógica entregada por TBK y configurada en la caja, se imprime en voucher | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número Ticket/Boleta `| 20     | **Valor alfanumérico**<br>Campo opcional, si viene se imprime en voucher si no viene se omite el campo | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Empleado`          | 4         | **Valor alfanumérico**<br>Campo opcional, si viene se imprime en voucher si no viene se omite el campo | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código Autorización`| 8        | **Valor alfanumérico (máximo)**<br>Código de autorización de la transacción enviado por TBK ejemplo: &#124;AB 12 C3&#124;<br> Se imprime lo que viene en el voucher | 
`Separador`          | 1        | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto`             | 18        | **Valor numérico (máximo)** <br />Monto total autorizado (incluye el monto de la venta, propina, vuelto y donación según sea el caso)<br>Se imprime en voucher | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto vuelto`      | 18        | **Valor numérico (máximo)** <br />Vuelto seleccionado por cliente, solo aplica en debito<br> Se imprime en voucher| 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Numero de Cuotas`  | 2         | **Valor numérico**<br>Cantidad de cuotas de la transacción (para ventas sin cuotas se informa “00”)<br>Se imprime en voucher | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto cuota`       | 14        | **Valor numérico**<br>Si el monto informado es vacío &#124;&#124; o &#124;0&#124; caja debe omitir la línea completa en el voucher.<br>Se imprime en voucher si viene el campo |
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Últimos 4 Dígitos Tarjeta`| 4  | **Valor numérico**<br>No se imprime | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número Operación`  | 9         | **Correlativo de transacción del terminal**<br>También conocido como número de secuencia este campo se debe imprimir en voucher de venta y anulación. | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa Tipo de Tarjeta`| 7      | **Valor alfanumérico (máximo)**<br>Valor de glosa en [Tabla tipo de tarjeta](/referencia/host-to-host#tabla-de-tipo-de-tarjeta) | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Fecha Contable `   | 6         | **Valor alfanumérico**<br>Se utiliza sólo si es transacción de Debito<br>Caja no debe formatear (ej: DDAAMM), simplemente debe transferir el valor al voucher (XX/XX/XX) | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número de Cuenta`  | 19        | **Valor alfanumérico**<br>Número de tarjeta enmascarado para incluir en el voucher | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Abreviación de la tarjeta`| 2  | **Valor alfanumérico** <br>Valor a imprimir en el voucher |
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Fecha Transacción` | 6         | **Formato AAMMDD**<br>Valor a imprimir en el voucher | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Hora Transacción`  | 6         | **Formato HHMMSS**<br>Valor a imprimir en el voucher | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Campo Impresión`   | 8192      | **Campo depende si la caja requiere voucher formateado (máximo)**<br>Se envía voucher siempre | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Transacción premiada`| 1       | **Valor numérico**<br>&#124;1&#124;: transacción premiada<br>En este caso caja debe imprimir voucher PEL además del de venta<br>&#124;&#124;: transacción sin premio  | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo promoción`    | 1         | **Valor numérico**<br>&#124;1&#124;: Entrega Pto. de Venta<br>&#124;2&#124;: Entrega Diferida<br>&#124;3&#124;: Devolución al Tarjeta Habiente  | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código promoción`  | 8         | **Valor alfanumérico**<br>Valor a imprimir en el voucher premiado | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre promoción`  | 21        | **Valor alfanumérico**<br>Valor a imprimir en el voucher de premio | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa vale premio` | 62        | **Valor alfanumérico**<br>Valor a imprimir en el voucher de premio | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Texto vale premio` | 27        | **Valor alfanumérico**<br>Valor a imprimir en el voucher de premio | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag permite cuotas`| 1        | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag de gracia`    | 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag C2C`          | 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag C3C`          | 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag NCuotas`      | 1         | **Valor numérico**<br>Campo informativo de la configuración del comercio | **0**
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag máximo de cuotas`| 2      | **Valor numérico**<br>Campo informativo de la configuración del comercio | **00**
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo de menú`      | 2         | **Valor alfanumérico**<br>Indicador del tipo de menú por el cual se realizó la transacción<br>&#124;CR&#124; : CRÉDITO<br>&#124;DB&#124; : DÉBITO PREPAGO<br>&#124;NB&#124; : NO BANCARIA<br>Valor de tipo en [Tabla tipo de tarjeta](/referencia/host-to-host#tabla-de-tipo-de-tarjeta)<br>Una venta hecha como debito puede ser autorizada como prepago | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador transacción con gracia`| 1 | **Valor numérico**<br>Indicador de modalidad de la transacción<br>&#124;0&#124; transacción sin mes gracia<br>&#124;1&#124; transacción con mes gracia | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo cuotas`       | 1         | **Valor numérico**<br>&#124;0&#124; Sin cuotas<br>&#124;1&#124; Cuotas normales<br>&#124;3&#124; C3C o C2C<br>&#124;4&#124; CIC o N-cuotas | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tasa aplicada`     | 4         | **Valor numérico**<br>Solo se imprime en voucher si “Flag imprimir tasa = 1” | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa tipo cuota`  | 30        | **Valor alfanumérico**<br>Glosa a imprimir en voucher<br>Si el campo informado viene vacío “&#124;&#124;” caja debe omitir la línea en el voucher. | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa tipo cuota 2`| 22        | **Valor alfanumérico**<br>Glosa que se despliega en pinpad | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa promoción`   | 10        | **Valor alfanumérico**<br>Glosa que se despliega en pinpad | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Id promoción`      | 10        | **Valor alfanumérico**<br>Glosa que se despliega en pinpad | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag imprimir tasa`| 1         | **Valor numérico**<br>&#124;&#124; o &#124;0&#124; no imprime tasa aplicada<br>&#124;1&#124; imprime tasa aplicada | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Periodo diferido`  | 3         | **Valor numérico**<br>Periodo diferido seleccionado, valor a imprimir en voucher | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 1 periodo`| 3         | **Valor numérico**<br>No utilizado | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 1 valor tasa`| 4      | **Valor numérico**<br>No utilizado | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 1 valor cuota`| 14    | **Valor numérico**<br>No utilizado | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 2 periodo`| 3         | **Valor numérico**<br>No utilizado | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 2 valor tasa`| 4      | **Valor numérico**<br>No utilizado | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 2 valor cuota` | 14   | **Valor numérico**<br>No utilizado | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 3 periodo`| 3         | **Valor numérico**<br>No utilizado | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 3 valor tasa`| 4      | **Valor numérico**<br>No utilizado | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Diferido 3 valor cuota`| 14    | **Valor numérico**<br>No utilizado | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número de secuencia transacción original` | 9 | **Valor alfanumérico (máximo)** <br />También conocido como número de operación original de la venta. No se está usando este campo, no se imprime | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código respuesta Transbank` | 3| **Valor numérico** <br />Código de respuesta una vez finalizada la transacción. Se debe desplegar en el punto de venta.<br>EJ: RESPUESTA TRANSBANK - `<XXX>` : `<GLOSA>` | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa respuesta Transbank` | 48| **Valor alfanumérico (máximo)** <br />Glosa que despliega el pinpad una vez finalizada la transacción. Se debe desplegar en el punto de venta.<br>EJ: RESPUESTA TRANSBANK - `<XXX>` : `<GLOSA>`| 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag transacción con PIN` | 1  | **Valor alfanumérico** <br />Y: Transacción autentificada con PIN<br>N: Transacción autentificada por firma | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre tarjetahabiente` | 26   | **Valor alfanumérico** <br />Sólo imprimir si “Flag tipo voucher = 1, 2 ó 3” | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag tipo voucher` | 1         | **Valor numérico** <br />Según el número recibido se debe imprimir voucher con o sin firma:<br>&#124;0&#124; = Sin firma<br>&#124;1&#124; o &#124;2&#124; o &#124;3&#124; = con firma<br><br>**Cabeceras de los voucher:**<br>Para ventas con crédito: “VENTA CREDITO”<br>Para ventas con débito (siempre sin firma): “VENTA DEBITO”<br>Para ventas con no bancaria: “VENTA NO BANCARIA”<br>Para ventas con prepago (sin firma): “VENTA PREPAGO”<br>Para anulaciones con crédito (sin firma):“ANULACION CREDITO”<br>Para anulaciones con no bancaria (sin firma): “ANULACION NO BANCARIA” | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag modalidad cuotas` | 1     | **Valor alfanumérico** <br />0: Modalidad 3.1 (No utilizado)<br>1: Modalidad cuotas 4.0 | **1**
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa transacción afecta a ahorro` | 40 | **Valor alfanumérico (máximo)** <br />Se debe imprimir en el voucher cuando sea distinta de vacío<br>Campo 9, subcampo D | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Número de secuencia` | 9       | **Valor numérico** <br />No se está usando este campo, este no se imprime<br>También conocido como número de operación | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag mensaje terminal` | 1     | **Valor alfanumérico** <br />Y: El mensaje es terminal y NO se debe enviar el mensaje SPDH de respuesta<br>N: Se debe enviar el mensaje SPDH de respuesta | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Largo mensaje`     | 4         | **Valor numérico**  | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Mensaje SPDH Venta/Reversa` | 2048 | **Valor alfanumérico (máximo)** | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Propina`           | 18        | **Valor numérico**<br>Monto Propina o Donación | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Voucher de Rechazo`| 1024      | **Valor numérico**<br>Cuando transacción es declinada por EMV se debe imprimir un voucher especial.<br>Si este campo viene vacío no se imprime, si viene con dato se imprime<br>Este voucher se imprime solo, sin voucher de venta | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Voucher de PEL`    | 1024      | **Valor alfanumérico**<br>Voucher de PEL si viene la caja debe imprimirlo, solo una vez junto al voucher de venta<br>No se debe imprimir en duplicado<br>Este voucher se imprime junto al de venta | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`LABEL - EMV`       | 32        | **Valor alfanumérico**<br>Si el campo viene con datos caja debe incluirlo en el voucher en la posición indicada | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`RID - EMV`         | 32        | **Valor alfanumérico**<br>Si el campo viene con datos caja debe incluirlo en el voucher en la posición indicada | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Modelo pinpad`     | 6         | **Valor numérico**<br>Caja debe incluirlo en el voucher ejemplo: VX805<br>Campo obligatorio | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Versión de pinpad` | 6         | **Valor numérico**<br>Caja debe incluirlo en el voucher ejemplo: 12.34A<br>Campo obligatorio | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Saldo Prepago`     | 40        | **Valor alfanumérico (máximo)**<br>Indica el saldo de una tarjeta de prepago la cual se debe imprimir en voucher cuando es venta de prepago y cuando viene el saldo.<br>Nota: El Pinpad agrega esa glosa al voucher tal como viene en la mensajería. | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto Surcharge  ` | 12        | **Valor numérico**<br>Monto asociado a Surcharge. Incluye 2 decimales.<br>Si el monto informado es vacío &#124;&#124; o &#124;0&#124; caja debe omitir la línea completa en el voucher.<br>Se imprime en voucher si viene el campo. <br>Para realizar posteriores anulaciones, este campo se debe sumar al monto total de la venta. | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Surcharge Idioma`  | 2         | **Valor alfanumérico**<br>Se informa el idioma en que será emitido el voucher.<br>ES: Español<br>EN: Ingles | 
`Separador`         | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`             | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`             | 1         | Byte resultado de la operación XOR del mensaje | 

## Comandos actualización parámetros pinpad (cierre batch)

### 0600 - Solicitud comando actualización parámetros pinpad 

El cierre batch es una transacción que sirve para cargar parámetros en el pinpad conforme a lo configurado
en Transbank para el comercio (ej: código de servicio)
Este cierre se debe ejecutar al iniciar el día, idealmente en forma automática. También puede ejecutarse en
forma manual, y esto además sirve para verificar si hay comunicación con Transbank, el resultado debe ser
una transacción aprobada que figura en pantalla de pinpad y en caja.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0600` <br><i>valor hexadecimal</i>: `0x30 0x36 0x30 0x30` | **0600**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de comercio`  | 12  | **Valor numérico**<br>Código del comercio entregado por TBK y configurado en la caja. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Terminal ID`    | 16       | **Valor alfanumérico**<br>Dirección lógica entregada por TBK y configurada en la caja. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Índice interno del comercio`| 16 | **Valor alfanumérico (máximo)**<br>Campo que puede ser utilizado por el comercio para agregar información que le sirva a sus procesos internos. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

Timeout máximo de espera por comando 610 de 20seg, no requiere interacción con tarjetabiente.

```
0610|00|0123456789ABCDEF|0523|9.11S4HOST2HOST3DES1
171116113320AO60050000Q0123456789ABCDEFa0010050000+0000000000000000000000+0000000000
000000000000+000000000000000000d597044440001h0010050071r;597044440001=9912?t74
0000000000000000326-478-322 15.30C
S0000000000T0000000000W00700000009-A1EL20212223242526272829VI0 6MC0 67DC0
6AX012345OTTP06TR01TE0 TM0
TC12TD12TJ12TH12T812T90 -B01205240-C2100-P000000000000-I0-J0-K000-G33032131100000000
0000000000000000000000000000000000000000000000000000000000000000|
 m

```

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0610` <br><i>valor hexadecimal</i>: `0x30 0x36 0x31 0x30` | **0610**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta`  | 2  | **Valor Numérico**<br>En caso de rechazo se debe desplegar en el punto de venta:<br>RECHAZO PINPAD - `<XX>` : `<GLOSA>`<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Índice interno del comercio`| 16 | **Valor alfanumérico (máximo)**<br>Campo que puede ser utilizado por el comercio para agregar información que le sirva a sus procesos internos. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Largo mensaje` | 4         | **Valor numérico**  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Mensaje SPDH Cierre batch` | 2048 | **Valor alfanumérico (máximo)** | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 


### 0700 - Validación comando actualización parámetros pinpad

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0700` <br><i>valor hexadecimal</i>: `0x30 0x37 0x30 0x30` | **0700**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Largo mensaje` | 4         | **Valor numérico**  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Mensaje SPDH Cierre batch` | 2048 | **Valor alfanumérico (máximo)** | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

Timeout máximo de espera por comando 710 de 20seg, no requiere interacción con tarjetabiente.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0710` <br><i>valor hexadecimal</i>: `0x30 0x37 0x31 0x30` | **0710**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta`  | 2  | **Valor Numérico**<br>En caso de rechazo se debe desplegar en el punto de venta:<br>RECHAZO PINPAD - `<XX>` : `<GLOSA>`<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código respuesta Transbank`| 3 | **Valor Numérico**<br>Código de respuesta de TBK. Se debe desplegar en el punto de venta. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa respuesta Transbank` | 48 | **Valor alfanumérico (máximo)** <br />Glosa que despliega el pinpad una vez finalizada la transacción. Se debe desplegar en el punto de venta. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Cantidad transacciones venta`| 4 | **Valor numérico**  |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto transacciones venta` | 18 | **Valor numérico**  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Cantidad transacciones anulación` | 4 | **Valor numérico**  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto transacciones anulación` | 18 | **Valor numérico**  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

## Comandos de Ventas ONUS

### 0800 – 0810 Solicitud venta ONUS

Solo para retail con tarjetas propias

### 0900 – 0910 Validación venta ONUS

Solo para retail con tarjetas propias

## ADMN - Comandos administrativos 

Estos comandos por el momento solo están habilitados para los terminales autoservicio con equipos
Verifone ux300, ux100, ux400.
A continuación se describen los comandos administrativos, los cuales tienen campos mandatorios y otros no:<br>X = Obligatorio <br>O = Opcional <br>Z = en desuso

### Eco CAJA -> PINPAD

Comando enviado desde la Caja hacia el pinpad para verificar que el pinpad se encuentra conectado y
disponible, debe ser enviando en un tiempo configurable en la caja por ejemplo cada 5 minutos. Este
comando también sirve para establecer la conexión entre pinpad y caja. El pinpad abre un socket de
conexión el cual no se cierra habitualmente, pero ante una nueva solicitud cierra el socket anterior y abre
otro.


DATO            | LARGO     | COMENTARIO        | REQUERIDO             | VALOR POR DEFECTO
------          | ------    | ------            | ------                | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | X | STX
`Comando`       | 4         | <i>Valor</i>:  `ADMN`                                                 | X | ADMN
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Tipo comando`  | 2         | Formato numérico<br>01: eco conexión pinpad<br>02: reiniciar pinpad<br>03: actualización de parámetros pinpad | X | 01
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Fecha`         | 8         | Fecha actual configurada en caja (AAAAMMDD) 20171231                  | X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Hora`          | 6         | Hora en formato 24hr (HHMMSS) 246060                                  | X |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Version de caja` | 16      | Identifica la versión del punto de venta del comercio<br>EJ: NEWPOS 123.456 | X |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Identificador de caja` | 16| Identifica al punto de venta del comercio<br>EJ: sucursal 123 caja 456| X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje                              | X | STX


DATO            | LARGO     | COMENTARIO        | REQUERIDO             | VALOR POR DEFECTO
------          | ------    | ------            | ------                | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`       | X | STX
`Comando`       | 4         | <i>Valor</i>:  `ADMN`                                                       | X | ADMN
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Respuesta comando`  | 2    | Valor Numérico<br>Código de respuesta al comando, de acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) |  X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Identificador PinPad`| 20  | Valor alfanumérico Marca modelo pinpad<br>EJ: VERIFONE UX300    | X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Versión PinPad `| 20       | Valor alfanumérico<br>Versión de aplicativo cargado en pinpad<br>ej: 15.20A TBK20171231 | X |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Número Serie PinPad` | 20  | Valor alfanumérico<br>Número de serie del PINPAD<br>EJ: 123-123-123-123     | X |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Identificador de caja`| 16 | Identifica al punto de venta del comercio<br>EJ: sucursal 123 caja 456| X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Fecha`         | 8         | Fecha actual configurada en caja (AAAAMMDD) 20171231                        | X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Hora`          | 6         | Hora en formato 24hr (HHMMSS) 246060                                        | X |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje                              | X | STX

### Reiniciar Pinpad (Caja -> Pinpad)

Comando con el cual la caja solicita al pinpad que se reinicie, pinpad responde ok y reinicia.
Este comando puede ser enviado por ejemplo todos los días a las 4:00AM no debe ser enviado al iniciar la
caja o punto de venta del comercio ya que en ese instante se debe enviar la actualización de parámetros del
pinpad o (cierre batch)

DATO            | LARGO     | COMENTARIO        | REQUERIDO             | VALOR POR DEFECTO
------          | ------    | ------            | ------                | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | X | STX
`Comando`       | 4         | <i>Valor</i>:  `ADMN`                                                 | X | ADMN
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Tipo comando`  | 2         | Formato numérico<br>01: eco conexión pinpad<br>02: reiniciar pinpad<br>03: actualización de parámetros pinpad | X | 02
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Fecha`         | 8         | Fecha actual configurada en caja (AAAAMMDD) 20171231                  | X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Hora`          | 6         | Hora en formato 24hr (HHMMSS) 246060                                  | X |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje                              | X | STX


DATO            | LARGO     | COMENTARIO        | REQUERIDO             | VALOR POR DEFECTO
------          | ------    | ------            | ------                | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`       | X | STX
`Comando`       | 4         | <i>Valor</i>:  `ADMN`                                                       | X | ADMN
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Respuesta comando`| 2      | Valor Numérico<br>Código de respuesta al comando, de acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) |  X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Fecha`         | 8         | Fecha actual configurada en caja (AAAAMMDD) 20171231                        | X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Hora`          | 6         | Hora en formato 24hr (HHMMSS) 246060                                        | X |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje                              | X | STX


### Actualización parámetros de Pinpad (Pinpad -> Caja)

Comando con el cual el pinpad solicita a la caja gatillar una actualización de parámetros o cierre batch,
mediante el flujo habitual (comandos 600-700), caja inicia el flujo estándar 600, 610, 700, 710.

DATO            | LARGO     | COMENTARIO        | REQUERIDO             | VALOR POR DEFECTO
------          | ------    | ------            | ------                | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | X | STX
`Comando`       | 4         | <i>Valor</i>:  `ADMN`                                                 | X | ADMN
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Tipo comando`  | 2         | Formato numérico<br>01: eco conexión pinpad<br>02: reiniciar pinpad<br>03: actualización de parámetros pinpad | X | 03
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Fecha`         | 8         | Fecha actual configurada en caja (AAAAMMDD) 20171231                  | X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Hora`          | 6         | Hora en formato 24hr (HHMMSS) 246060                                  | X |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje                              | X | STX

DATO            | LARGO     | COMENTARIO        | REQUERIDO             | VALOR POR DEFECTO
------          | ------    | ------            | ------                | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`       | X | STX
`Comando`       | 4         | <i>Valor</i>:  `ADMN`                                                       | X | ADMN
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Respuesta comando`  | 2    | Valor Numérico<br>Código de respuesta al comando, de acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) |  X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Fecha`         | 8         | Fecha actual configurada en caja (AAAAMMDD) 20171231                        | X | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`Hora`          | 6         | Hora en formato 24hr (HHMMSS) 246060                                        | X |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| X | &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje                              | X | STX


### 1600 – 1610 Comando Lectura de Código de Barras*

**Importante: Los códigos de barra soportados por el pinpad e355 son: CODE39, CODE128, EAN y UPC. \*Solo para el modelo que tiene lector de código de barras Verifone e355**  

Comando para iniciar la captura de código de barra desde el Pinpad modelo E355.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `1600` <br><i>valor hexadecimal</i>: `0x31 0x36 0x30 0x30` | **1600**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Timeout Comando`  | 2      | **Valor numérico**<br>01-99 Tiempo en segundos, para mantener la lectura de código de barra en el pinpad. | **20**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Línea de Display 1` | 21   | **Valor alfaumérico**<br>Primera línea de mensaje a Desplegar en el Pinpad para indicar el inicio de Lectura de Código de Barra. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Línea de Display 2` | 21   | **Valor alfaumérico**<br>Segunda línea de mensaje a Desplegar en el Pinpad para indicar el inicio de Lectura de Código de Barra. | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

Timeout máximo de espera por comando 1610 es el configurado en la solicitud (1600), si se cumple este tiempo el Pinpad devuelve un 1610&#124;99.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `1610` <br><i>valor hexadecimal</i>: `0x31 0x36 0x31 0x30` | **1610**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta`  | 2  | **Valor Numérico**<br>En caso de rechazo se debe desplegar en el punto de venta:<br>RECHAZO PINPAD - `<XX>` : `<GLOSA>`<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-respuesta-pinpad) | **00**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de Barra`  | 150    | **0 Valor alfanumérico largo variable**<br>Código de barra capturado por el pinpad | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | 

## Anexos

### Tabla de Marcas

NOMBRE MARCA DE TARJETA         | ABREVIACIÓN DE TARJETA
------                          | -----------
VISA                            | VI
MASTERCARD                      | MC
DINERS                          | DC
AMEX                            | AX
DISCOVER                        | DS
MAGNA                           | MG
PRESTO                          | TP
MAS (cencosud)                  | TM
CMR                             | TC
RIPLEY                          | TR
MAESTRO                         | MT
ELECTRON                        | EL
DEBITO                          | DB
PREPAGO                         | PP

### Tabla de tipo de tarjeta

CÓDIGO TIPO DE TARJETA          | GLOSA TIPO DE TARJETA
------                          | -----------
CR                              | CREDITO
DB                              | DEBITO - PREPAGO
NB                              | NO BANCARIA
Null (vacío)                    | Se despliega Menú en PINPAD.

### Tabla de códigos de respuesta de pinpad a los comandos

Estos códigos de respuesta a los comandos enviado al pinpad, solo en caso de rechazo (distinto a 00), se
deben desplegar en pantalla del punto de venta del comercio, en caso de problemas ayudan a identificar la
causa.

CÓDIGO DE RESPUESTA         | GLOSA
------                      | -----------
00                          | **RESPUESTA OK**
--                          | POR DEFINIR (CONSIDERAR COMO SI FUERA 99)
81                          | TIMEOUT POR MENOS DE 30 SEGUNDOS*
82                          | COMANDO NO VÁLIDO
83                          | NO EXISTE CÓDIGO DE MENSAJE
84                          | TARJETA NO SOPORTADA
85                          | REVERSA APLICADA
86                          | ERROR DE LECTURA
87                          | PINPAD SIN MASTER KEY
88                          | TARJETA NO PERMITE VENTA ONUS
89                          | TRANSACCIÓN DECLINADA POR LA TARJETA CHIP
90                          | TARJETA NO PERMITIDA PARA EL MODO SELECCIONADO
91                          | ERROR CANTIDAD DE CUOTAS
92                          | NO COINCIDE CON TARJETA DE PRIMER “TAPEO”
93                          | ERROR DE MONTO MÍNIMO
94                          | ERROR DE VALIDACIÓN MONTO VUELTO
95                          | ERROR ID DE CONTEXTO
96                          | NO COINCIDE LOS 4 ULTIMOS DIGITOS
97                          | LA TRANSACCIÓN NO PERMITE REVERSA
98                          | ERROR DE FORMATO DEL MENSAJE
99                          | CANCELACIÓN POR LA TECLA \[CANCEL\] / TIMEOUT

**Valido para la versión del aplicativo configurado para Bencineras.*

### Algunos códigos de respuesta de los autorizadores

Estos son solo algunos de los códigos de respuestas de los autorizadores sean aprobadas o rechazadas, hay muchos más pero estos son los más comunes.  
Al recibir respuesta de la transacción sea aprobada o rechazada, se debe desplegar los 3 dígitos y la glosa entregada en la pantalla del punto de venta del comercio, en caso de problemas ayudan a identificar la causa, sin tener que estar buscando log de la caja:

CÓDIGO DE RESPUESTA         | GLOSA
------                      | -----------
000 - 009                   | APROBADO
050                         | RECHAZO GENERAL DEL BANCO
056                         | PRODUCTO NO SOPORTADO
064                         | REINTENTE
066                         | PRODUCTO NO SOPORTADO
076                         | FONDOS INSUFICIENTES
082                         | EXCEDE MAXIMO
083                         | EXCEDE MAXIMO
085                         | FONDOS INSUFICIENTES
087                         | EXCEDE MAXIMO
095                         | EXCEDE MAXIMO
105                         | PRODUCTO NO SOPORTADO
106                         | EXCEDE MAXIMO
107                         | EXCEDE MAXIMO
109                         | EXCEDE MAXIMO
110                         | REINTENTE
112                         | EXCEDE MAXIMO
150                         | ERROR EN EL CÓDIGO DE COMERCIO O EN LA CAJA O EN EL AMBIENTE TBK
201                         | CLAVE INVALIDA
202                         | EXCEDE MAXIMO
215                         | EXCEDE MAXIMO
217                         | REINTENTE
218                         | REINTENTE
219                         | MENU INVALIDO
251                         | PRODUCTO NO SOPORTADO
820                         | ERROR EN LA DDLL O EN LA CAJA O EN AMBIENTE TBK
908                         | EXCEDE MAXIMO
950                         | COMERCIO MAL CONFIGURADO EN TBK
964                         | REINTENTE
999                         | RECHAZADO

# ANEXO ONUS

## Descripción de comandos 
En este capítulo se detalla cada comando que se puede enviar al PINPAD.  

Para la comunicación serial es necesario indicar los caracteres de inicio y fin que se envían en cada comando, los que serán indicados por medio de la siguiente nomenclatura:  

CARÁCTER DE CONTROL     | NOMENCLATURA
-------------------     | ------------
`<STX><DATA><ETX><LRC>` | STXETX

A fin de mantener el mismo lenguaje en comunicación TCP-IP se mantienen estos
caracteres STX, ETX y LRC.

### Flujo comandos ONUS

![]()

La caja del comercio podría usar el comando 800 más de una vez incorporando el indicado de contexto, por ejemplo en la primera solo lee tarjeta, en la segunda solicita confirmar monto, en la tercera solicita pin. Lo ideal es que utilice un solo comando 800 para solicitar todo, leer tarjeta, confirmar monto y pedir pin.

### 0800 - Comando requerimiento venta/anulación

Con este comando la caja solicita al pinpad leer una tarjeta, confirmar monto y pedir pin. La caja pude hacer una solicitud con todos los requerimientos o varias solicitudes por separado.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0800` (Obligatorio) <br><i>valor hexadecimal</i>: `0x30 0x38 0x30 0x30` | **0800**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto comando`  | 16 | **Valor alfanumérico (opcional)**<br>Formato aaaammddhhmmssmm<br>Es solo un ID, la fecha y hora en el pinpad puede estar desactualizada<br>Si este comando proviene de otro y es parte de la misa transacción se debe mantener el ID |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Local comercio OnUs`  | 2  | **Valor numérico (obligatorio)**<br>(00 -> 99)<br>Cada local onus tiene su ID definido | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Working key track` | 32    | **Valor alfanumérico (opcional)** <br />Contiene la llave 3DES utilizada para encriptar el TRACK (ø: no encripta) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Working key PIN ` | 32     | **Valor alfanumérico (opcional)** <br />Contiene la llave 3DES utilizada para encriptar el TRACK (ø: no encripta) | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto`         | 18        | **Valor numérico (obligatorio)**<br>(incluye dos decimales)<br>Largo variable  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Lista montos de vuelto` |60| **Valor alfanumérico (opcional)**<br>Campo no utilizado por el momento enviar vacío<br>Largo variable  | **Ø**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Monto vuelto`  | 18        | **Valor numérico (opcional)**<br>&#124;0&#124; Transacción sin vuelto<br>Campo no utilizado por el momento enviar cero<br>Largo variable  | **0**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag solicita confirma monto` | 1 | **Valor alfanumérico (obligatorio)**<br>&#124;Y&#124; Solicitar confirmar monto<br>&#124;N&#124; No solicitar confirmar monto  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa 1 confirma monto`| 21| **Valor alfanumérico (opcional)**<br>Solo si “Solicita confirma monto = Y”<br>Largo variable  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa 2 confirma monto`| 21| **Valor alfanumérico (opcional)**<br>Solo si “Solicita confirma monto = Y”<br>Largo variable  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa 3 confirma monto`| 21| **Valor alfanumérico (opcional)**<br>Solo si “Solicita confirma monto = Y”<br>Largo variable  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa 4 confirma monto`| 21| **Valor alfanumérico (opcional)**<br>Solo si “Solicita confirma monto = Y”<br>Largo variable  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador solicitud de autentificación` | 2 | **Valor numérico (obligatorio)**<br>&#124;00&#124; Pide PIN a todo evento<br>&#124;01&#124; No pide PIN a menos que la tarjeta EMV lo indique<br>&#124;02&#124; Pide PIN según código de servicio<br>&#124;03&#124; Pide PIN según código de servicio, pero acepta pin nulo<br>&#124;04&#124; No pide PIN nunca  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | LRC

### 0810 - Comando requerimiento venta/anulación

Con este comando el pinpad entrega a la caja los datos obtenidos en la solicitud del comando 800, como los tracks de la tarjeta (en claro o encriptado), el pin ingresado (que siempre se entrega encriptado) y los datos de EMV que se deben enviar al autorizador ONUS.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0810` (Obligatorio) <br><i>valor hexadecimal</i>: `0x30 0x38 0x31 0x30` | **0810**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta`  | 2  | **Valor numérico**<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-codigos-de-respuesta-de-comandos-onus) |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto` |16 | **Valor alfanumérico**<br>Formato aaaammddhhmmssmm<br>Es solo un ID, la fecha y hora en el pinpad puede estar desactualizada | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Tipo de captura ` | 2      | **Valor numérico**<br />&#124;00&#124; : B - Banda<br>&#124;01&#124; : E . EMV c/contacto<br>&#124;02&#124; : C - Contacless<br>&#124;03&#124; : F - Fallback | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`TRACK I`       | 80        | **Valor alfanumérico** <br />Este campo es opcional, por lo tanto puede contener datos o estar vacío.<br>Si existen datos y es necesario se rellena con blancos<br>(0x20) a la derecha<br>Con pan encriptado se entrega 160 caracteres alfanuméricos que corresponde a 80 HEXA<br>- Si el pinpad no logra leer un track1, este campo irá vacío.<br>- Si el pinpad lee datos erróneos en el track1, este campo irá vacío.<br>- Si se excede el largo máximo, este campo irá vacío. |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`TRACK II`      | 40        | **Valor alfanumérico (máximo)**<br>El track2 es un valor requerido para cualquier venta, por lo tanto si no se logra leer o se lee errores, se entrega un error de lectura al comando.<br> Si se obtiene correctamente el dato desde la tarjeta, se rellena con blancos (0x20) a la derecha si es necesario y se entrega.<br> Con pan encriptado se entrega 80 caracteres alfanuméricos que corresponde a 40 HEXA  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`BIN`           | 6         | **Valor numérico**<br>Seis primeros dígitos de la tarjeta | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`4 últimos dígitos`  | 4    | **Valor numérico**<br>Cuatro últimos dígitos de la tarjeta | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre tarjetahabiente` |26| **Valor alfanumérico**<br>Este dato se obtiene desde el track1, por lo tanto si no existe el track1, no se entrega el nombre del tarjetahabiente<br>Largo variable  | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Nombre marca de la tarjeta`| 20 | **Valor alfanumérico** <br>De acuerdo a [Tabla de marcas]() | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Abreviación de la tarjeta` | 2  | **Valor alfanumérico** <br>De acuerdo a [Tabla de marcas]() | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Pinblock`      | 16        | **Valor alfanumérico**<br>PIN ingresado por el cliente pero encriptado con las llaves proporcionadas por la caja| 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Criptograma`   | 999       | **Valor alfanumérico**<br>Formato TLV ISO<br>Contiene TAG y criptograma requerido por el emisor para autorizar transacciones realizadas con tecnología y seguridad EMV<br>Largo variable | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | LRC

### 0900 - Comando validación de respuesta autorizador

Con este comando la caja entrega al pinpad el resultado de la transacción respondida por el autorizador onus, sea aprobada o rechazada.  
Además si la venta se hizo con chip se debe entregar el criptograma generado por el autorizador onus a la tarjeta que aún está insertada, para su evaluación.  
A partir de la versión de pinpad 15.2 con este comando el pinpad además de mostrar en pantalla “RETIRE TARJETA” también emite un sonido de alerta.

DATO            | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------          | ------    | ------            | ------
`<STX>`         | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`       | 4         | <i>Valor ASCII</i>:  `0900` (Obligatorio) <br><i>valor hexadecimal</i>: `0x30 0x39 0x30 0x30` | **0900**
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Indicador de contexto`  |16| **Valor alfanumérico**<br>Formato aaaammddhhmmssmm<br>Es solo un ID, la fecha y hora en el pinpad puede estar desactualizada<br> Si este comando proviene de otro y es parte de la misa transacción se debe mantener el ID | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Flag valida criptograma` |1| **Valor alfanumérico**<br>&#124;Y&#124;  Validar criptograma<br>&#124;N&#124; No validar criptograma<br>Solo aplica para transacciones con chip con contacto | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa respuesta`       | 21| **Valor alfanumérico (máximo)** <br />Sólo si “Flag valida criptograma = N”<br>Ejemplo: Aprobado / Rechazado / Reintente |  
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Criptograma`   | 999       | **Valor alfanumérico**<br>Formato TLV ISO<br>Formato TLV ISO<br>Sólo si “Flag valida criptograma = Y”<br>Este criptograma el pinpad entrega al chip de la tarjeta para validación.<br>Podría resultar en una transacción declinada por la tarjeta para lo cual la caja debe solicitar reversa al autorizador ONUS | 
`Separador`     | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`         | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte resultado de la operación XOR del mensaje | LRC

### 0910 - Comando validación de respuesta autorizador

Primero que nada este comando es la respuesta ok (o nook) al requerimiento del comando 900. El objetivo principal de este comando es que el Pinpad entregue el resultado de la evaluación del criptograma enviado por el autorizador, el resultado puede ser Aprobado o Declinado por la tarjeta, para que el caso de declinación, la caja reverse la venta aprobada.

DATO                   | LARGO     | COMENTARIO        | VALOR POR DEFECTO
------                 | ------    | ------            | ------
`<STX>`                | 1         | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02` | STX
`Comando`              | 4         | <i>Valor ASCII</i>:  `0910` (Obligatorio) <br><i>valor hexadecimal</i>: `0x30 0x39 0x31 0x30` | **0910**
`Separador`            | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Código de respuesta`  | 2         | **Valor numérico**<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-codigos-de-respuesta-de-comandos-onus) |  
`Separador`            | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`Glosa respuesta`      | 21        | **Valor alfanumérico**<br>De acuerdo a [Tabla de códigos de respuesta de comandos](/referencia/host-to-host#tabla-de-codigos-de-respuesta-de-comandos-onus). Sólo si “Flag valida criptograma = Y” | 
`Separador`            | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`| &#124;
`<ETX>`                | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`     | ETX
`<LRC>`                | 1         | Byte resultado de la operación XOR del mensaje | LRC


## Anexos

### Tabla de marcas

IDÉNTICO AL ESTÁNDAR

### Tabla de tipo de tarjeta

IDÉNTICO AL ESTÁNDAR

### Tabla de códigos de respuesta de comandos ONUS

Se detalla los códigos de respuesta adicionales para la modalidad OnUs


CÓDIGO DE RESPUESTA     | GLOSA
--------------------    | -----
00                      | RESPUESTA OK
--                      | Por definir (considerar como si fuera 99)
83                      | NO EXISTE CODIGO DE MENSAJE
84                      | TARJETA NO SOPORTADA
85                      | REVERSA APLICADA
86                      | ERROR DE LECTURA
87                      | PINPAD SIN MASTER KEY
88                      | TARJETA NO PERMITE VENTA ONUS
89                      | TRANSACCIÓN DECLINADA POR LA TARJETA CHIP
90                      | TARJETA NO PERMITIDA PARA EL MODO SELECCIONADO
91                      | ERROR CANTIDAD DE CUOTAS
92                      | NO COINCIDE CON TARJETA DE PRIMER “TAPEO”
93                      | ERROR DE MONTO MÍNIMO
94                      | ERROR DE VALIDACIÓN MONTO VUELTO
95                      | ERROR ID DE CONTEXTO
96                      | NO COINCIDE LOS 4 ULTIMOS DIGITOS
97                      | LA TRANSACCIÓN NO PERMITE REVERSA
98                      | ERROR DE FORMATO DEL MENSAJE
99                      | CANCELACIÓN POR LA TECLA \[CANCEL\] o TIMEOUT

### Código de local OnUs

Para soportar la utilización de transacciones no financieras que necesitan la lectura de tarjeta sin tener aún el monto1 se define un desplazamiento de 50 al código de local asociado al comercio OnUs, por ejemplo:  

01 -> 51

CÓDIGO DE LOCAL ONUS    | CODIGO LOCAL ONUS TRANSACIONES NO FINANCIERAS
--------------------    | ---------------------------------------------
NN                      | 50+NN