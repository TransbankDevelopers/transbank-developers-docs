# POS Integrado

<aside class="success">
El SDK y la Librería en C, se encargan de este protocolo de comunicación con el POS y de manejar el puerto serial configurado.
</aside>

## Protocolo de Comunicación Caja - POS

La comunicación se realiza a través de un puerto serial `RS232`, a velocidades que van entre los 1200bps hasta 115200bps `8N1`
es decir `8` bits de datos, `N`ingún bit de paridad y `1` bit de parada.

![Diagrama de Comunicación Caja - POS](/images/referencia/posintegrado/diagrama-comunicacion-caja-pos.png)

 Termino    | Descripción
 -------    | -------
 STX        | Indica el inicio de un mensaje (texto) <br><i>Hex</i>: `0x02`
 DATOS      | Corresponde al commando a enviar al POS o la respuesta de este
 ETX        | Indica el fin de un mensaje (texto) <br><i>Hex</i>: `0x03`
 LRC        | Es un byte que se concatena al final del mensaje `<ETX>` y se calcula realizando la operación `XOR` byte a byte de `<DATOS>` + `<ETX>`
 ACK        | Representa la recepción correcta del mensaje enviado <br><i>Hex</i>: `0x06`
 NACK       | Representa la incorrecta recepción del mensaje enviado, o que el `LRC` del mensaje recibido no corresponde con el enviado. <br><i>Hex</i>: `0x15`
 Timeout1   | Es el tiempo de espera de la caja para recibir ACK/NACK por parte del POS Integrado antes de reintentar el envío del mensaje.
 Timeout2   | Es el tiempo de espera de el POS para recibir ACK/NACK por parte de la Caja antes de reintentar el envío del mensaje de respuesta.

<aside class="notice">
Todos los comandos que se envían al POS deben cumplir con el flujo antes mencionado.
</aside>

<aside class="notice">
Todos los mensajes intercambiados entre la caja y el POS Integrado cumplen con el formato: <strong>&lt;STX&gt;DATOS&lt;ETX&gt;LRC</strong>
</aside>

### Ejemplo de calculo LRC

Dado el siguiente comando:

```html
<STX>0200|123|<ETX>
```

Que en notación hexadecimal seria:

```bash
0x02 0x30 0x32 0x30 0x30 0x7C 0x31 0x32 0x33 0x7C 0x03
```

Para calcular el `LRC` debemos omitir el inicio de texto `<STX>` o `0x02`.

La operación entonces seria:

```c
(((((((((0x30 XOR 0x32) XOR 0x30) XOR 0x30) XOR 0x7C) XOR 0x31) XOR 0x32) XOR 0x33) XOR 0x7C) XOR 0x03)
```

El resultado entonces seria `0x31` en hexadecimal o `1` en ASCII, por lo tanto, el mensaje completo para enviar al POS Integrado es:

```bash
<STX>                   DATOS                           <ETX>   LRC
 0x02    0x30 0x32 0x30 0x30 0x7C 0x31 0x32 0x33 0x7C    0x03    0x31
```

## Mensajes

### Mensaje de Cierre

Este comando es gatillado por la caja y no recibe parámetros. El POS ejecuta la transacción de cierre contra el Autorizador (no se contempla Batch Upload). Como respuesta el POS Integrado enviará un aprobado o rechazado. (Puedes ver la tabla de respuestas en este [link](/referencias/posintegrado#tabla-de-respuestas))

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
RegisterCloseResponse responce = POS.Instance.RegisterClose();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
LoadKeyCloseResponse responce = register_close();
}
```

El resultado del cierre de caja se entrega en la forma de un objeto `RegisterCloseResponse`o una estructura `BaseResponse` en el caso de la librería C.

```json
"FunctionCode": 510
"ResponseMessage": "Aprobado"
"Success": true
"CommerceCode": 550062700310
"TerminalId": "ABC1234C"
```

![Diagrama de Secuencia Cierre](/images/referencia/posintegrado/diagrama-cierre.png)

1. La caja envía el requerimiento y espera como respuesta ACK/NACK, en caso de que llegue un NACK, debe reintentar el envío del requerimiento 2 veces. Si recibe un ACK debe esperar la respuesta de la transacción.
2. El POS envía requerimiento al Autorizador, en caso de ser aprobada, se borra Batch y se envía respuesta a la caja. En caso de ser rechazada se envía respuesta a la caja indicando el error.
3. La caja al recibir la respuesta envía un ACK si el mensaje está correcto, o un NACK para el caso en que el LRC no corresponde.
4. El POS al recibir el ACK vuelve al inicio a esperar un nuevo comando, para el caso que recibe un NACK vuelve a enviar la respuesta 2 veces más.

<aside class="notice">
Para el cierre no se solicitara tarjeta supervisora.
</aside>

#### Solicitud de Cierre

DATO        | LARGO     | Comentario
------      | ------    | ------
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`   | 4         | <i>valor ASCII</i>: `0500` <br><i>valor hexadecimal</i>: `0x30 0x35 0x30 0x30`
`Separador` | 1         | <i>valor ASCII</i>: `|` <br><i>valor hexadecimal</i>: `0x07`
`Separador` | 1         | <i>valor ASCII</i>: `|` <br><i>valor hexadecimal</i>: `0x07`
`<ETX>`     | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`<LRC>`     | 1         | Resultado del calculo del LRC del mensaje.

*Mensaje* en <i>ASCII</i>: `<STX>0500||<ETX>6`

*Mensaje* en <i>Hexadecimal</i>: `{0x02, 0x30, 0x35, 0x30, 0x30, 0x07, 0x07, 0x03, 0x06}`

#### Respuesta de Cierre

DATO                    | LARGO     | COMENTARIO
------                  | ------    | ------
`<STX>`                 |  1        | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`               |  4        | <i>Valor ASCII</i>:  `0510` <br><i>valor hexadecimal</i>: `0x30 0x35 0x31 0x30`
`Separador`             |  1        | <i>valor ASCII</i>: `|` <br><i>valor hexadecimal</i>: `0x07`
`Código Respuesta`      |  2        | Valor Numérico
`Separador`             |  1        |  <i>valor ASCII</i>: `|` <br><i>valor hexadecimal</i>: `0x07`
`Código de comercio`    | 12        | Valor Numérico
`Separador`             |  1        |  <i>valor ASCII</i>: `|` <br><i>valor hexadecimal</i>: `0x07`
`Terminal ID`           |  8        | Valor Alfanumérico
`Separador`             |  1        |  <i>valor ASCII</i>: `|` <br><i>valor hexadecimal</i>: `0x07`
`<ETX>`                 |  1        | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`<LRC>`                 |  1        | Resultado del calculo del LRC del mensaje.

### Mensaje de Carga de Llaves

Esta transacción permite al POS Integrado del comercio requerir cargar nuevas _Working Keys_ desde Transbank. Como respuesta el POS Integrado enviará un aprobado o rechazado. (Puedes ver la tabla de respuestas en este [link](/referencia/posintegrado#tabla-de-respuestas))

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
LoadKeysResponse responce = POS.Instance.LoadKeys();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
LoadKeyCloseResponse responce = load_keys();
}
```

<aside class="warning">
El uso de esta transacción debe ser limitado a pruebas de comunicación o cuando el POS Integrado pierda las llaves.
</aside>

### Mensaje de Pooling

Esta mensaje es enviado por la caja para saber si el POS está conectado. En el SDK el resultado de esta operación es un `Booleano` o un `0` representado en la constante `TBK_OK` en el caso de la librería en C.

```csharp
using Transbank.POS;
//...
bool connected = POS.Instance.Polling();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
int retval = polling();
if (retval == TBK_OK){
    //...
}
```

### Mensaje de Cambio a POS Normal

Este comando le permitirá a la caja realizar el cambio de modalidad a través de un comando. El POS debe
estar en modo integrado y al recibir el comando quedara en modo normal.  El resultado de esta operación es un `Booleano` en el caso del SDK o un `0` representado en la constante `TBK_OK` en el caso de la librería en C.

```csharp
using Transbank.POS;
//...
bool connected = POS.Instance.SetNormalMode();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
int retval = set_normal_mode();
if (retval == TBK_OK){
    //...
}
```

<aside class="notice">
Si el POS Integrado se cambia a modo normal, debe ser configurado nuevamente en modo Integrado siguiendo las instrucciones disponibles descritas en []()
</aside>

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
No puede Anular Transacción Debito                      | 08
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
