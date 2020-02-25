# POS Integrado

<aside class="success">
El SDK y la Librería en C, se encargan de este protocolo de comunicación con el POS y de manejar el puerto serial configurado.
</aside>

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

### Ejemplo de calculo LRC

Dado el siguiente comando:
`<STX>0200|123|<ETX>`

Que en notación hexadecimal seria:
`0x02 0x30 0x32 0x30 0x30 0x7C 0x31 0x32 0x33 0x7C 0x03`

Para calcular el `LRC` debemos omitir el inicio de texto `<STX>` o `0x02`.

La operación entonces seria:

`(((((((((0x30 XOR 0x32) XOR 0x30) XOR 0x30) XOR 0x7C) XOR 0x31) XOR 0x32) XOR 0x33) XOR 0x7C) XOR 0x03)`

El resultado entonces seria `0x31` en hexadecimal o `1` en ASCII, por lo tanto, el mensaje completo para enviar al POS Integrado es:

```bash
<STX>                   DATOS                           <ETX>   LRC
 0x02    0x30 0x32 0x30 0x30 0x7C 0x31 0x32 0x33 0x7C    0x03    0x31
```

## Primeros pasos

Para usar el SDK es necesario incluir las siguientes referencias.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Utils;
using Transbank.POS.Responses:
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
```

### Listar puertos disponibles

Si los respectivos drivers están instalados, entonces puedes usar la función `ListPorts()` del paquete
`Transbank.POS.Utils`para identificar los puertos que se encuentren disponibles y seleccionar el que
corresponda con el puerto donde conectaste el POS Integrado.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS.Utils;
//...
List<string> ports = Serial.ListPorts();
```

```c
#include "transbank_serial_utils.h"
//...
char *ports = list_ports();
```

### Abrir un puerto Serial

Para abrir un puerto serial y comunicarte con el POS Integrado, necesitarás el nombre del puerto (El cual puedes identificar usando [la función mencionada en el apartado anterior](referencia/posintegrado#listar-puertos-disponibles)). También necesitarás el baudrate al cual esta configurado el puerto serial del POS Integrado (Por defecto es 115200), y puedes obtener los distintos valores desde la clase `TbkBaudrates` del paquete `Transbank.POS.Utils`.

Si el puerto no puede ser abierto, se lanzará una exception `TransbankException`.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Utils;
//...
string portName = "COM3";
POS.Instance.OpenPort(portName, TbkBaudrate.TBK_115200);
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
char *portName = "COM4";
int retval = open_port(portName, 115200);
if ( retval == TBK_OK ){
    //...
}
```

### Cerrar un puerto Serial

Al finalizar el uso del POS, o si se desea desconectar de la Caja se debe liberar el puerto serial abierto anteriormente.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
//...
bool closed = POS.Instance.ClosePort();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
retval = close_port();
if(retval == SP_OK){
    //...
}
```

## Mensajes

### Mensaje de Venta

Este comando es enviado por la caja para solicitar la ejecución de una venta. Los siguientes parámetros deben ser enviados desde la caja:

- `Monto`: Monto en pesos informados al POS. Este parámetro es remitido a Transbank para realizar la autorización.
- `Número Ticket/Boleta`: Este número es impreso por el POS en el voucher que se genera luego de la venta.
- `Enviar Mensaje`: Este parámetro indica al POS si debe enviar mensajes intermedios a la caja mientras se realiza el proceso de venta.

<aside class="warning">
El SDK no soporta el envío de mensajes intermedios. Por esta razón el parámetro `Enviar Mensaje` sera siempre falso.
</aside>
  
- Los mensajes intermedios que envía el POS y que deben ser mostrados por la Caja, deben corresponder según los siguientes códigos:
  - `78`: Lectura de Tarjeta.
  - `79`: Confirmación de Monto.
  - `80`: Selección de Cuotas.
  - `81`: Ingreso de Pinpass.
  - `82`: Envío de transacción a Transbank.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
SaleResponse response = POS.Instance.Sale(ammount, ticket);
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
char* response = sale(ammount, ticket, false);
```

El resultado de la venta se entrega en la forma de un objeto `SaleResponse` o un `char*` en el caso de la librería C. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankSaleException`.

```json
"Function": 210
"Response": "Aprobado"
"Commerce Code": 550062700310
"Terminal Id": "ABC1234C"
"Ticket": "AB123"
"Autorization Code": "XZ123456"
"Ammount": 15000
"Shares Number": 3
"Shares Amount": 5000
"Last 4 Digits": 6677
"Operation Number": 60
"Card Type": CR
"Accounting Date":
"Account Number":
"Card Brand": AX
"Real Date": 28/10/2019 22:35:12
"Employee Id":
"Tip": 1500
```

<aside class="warning">
El SDK no soporta el envío de mensajes intermedios. Por esta razón el 3º parámetro de la función en C es siempre falso.
</aside>

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-venta.png" alt="Diagrama de Secuencia Venta">

1. La caja envía el requerimiento y espera como respuesta `<ACK>`/`<NAK>`, en caso de que llegue un `<NAK>`, debe reintentar el envío del requerimiento 2 veces. Si recibe un `<ACK>` debe esperar la respuesta de la transacción.
2. El POS solicita los datos al usuario, y envía el requerimiento al Autorizador, en caso de ser aprobada, se guarda en Batch y se envía respuesta a la caja. En caso de ser rechazada se envía respuesta a la caja indicando el error. ([Ver Tabla de Respuestas](/referencia/posintegrado#tabla-de-respuestas))
3. La caja al recibir la respuesta envía un `<ACK>` si el mensaje está correcto, o un `<NAK>` para el caso en que el `LRC` no corresponde.
4. El POS al recibir el `<ACK>` vuelve al inicio a esperar un nuevo comando, para el caso que recibe un `<NAK>` vuelve a enviar la respuesta 2 veces más.

#### Solicitud de Venta

DATO        | LARGO     | Comentario
------      | ------    | ------
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`   | 4         | <i>valor ASCII</i>: `0200` <br><i>valor hexadecimal</i>: `0x30 0x32 0x30 0x30`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Monto`     | 9         | Valor Numérico que debe ser convertido a hexadecimal.
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Ticket`    | 6         | Valor ASCII, Número de boleta o ticket, que debe ser convertido a hexadecimal
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Status`    | 1         | Indica al POS si debe enviar mensajes intermedios o de estado de la transacción <br><i>1</i>: Envía Mensajes<br><i>0</i>: No envía mensajes
`<ETX>`     | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`       | 1         | Resultado del calculo del `LRC` del mensaje

#### Respuesta de Venta

DATO                    | LARGO     | COMENTARIO
------                  | ------    | ------
`<STX>`                 |  1        | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`               |  4        | <i>Valor ASCII</i>:  `0510` <br><i>valor hexadecimal</i>: `0x30 0x35 0x31 0x30`
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

### Mensaje de Última Venta

Este comando es enviado por la caja para solicitar la re-impresión de la última venta.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
LastSaleResponse response = POS.Instance.LastSale();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
char* response = last_sale();
```

El resultado de la transacción última venta devuelve los mismos datos que una venta normal y se entrega en forma de un objeto `LastSaleResponse` o un `char*` en el caso de la librería C. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankLastSaleException`.

```json
"Function": 260
"Response": "Aprobado"
"Commerce Code": 550062700310
"Terminal Id": "ABC1234C"
"Ticket": "AB123"
"Autorization Code": "XZ123456"
"Ammount": 15000
"Shares Number": 3
"Shares Amount": 5000
"Last 4 Digits": 6677
"Operation Number": 60
"Card Type": CR
"Accounting Date":
"Account Number":
"Card Brand": AX
"Real Date": 28/10/2019 22:35:12
"Employee Id":
"Tip": 1500
```

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-ultima-venta.png" alt="Diagrama de Secuencia Última Venta">

1. La caja envía el requerimiento y espera como respuesta `<ACK>`/`<NAK>`, en caso de que llegue un `<NAK>`, debe reintentar el envío del requerimiento 2 veces. Si recibe un `<ACK>` debe esperar la respuesta de la transacción.
2. Una vez recibida la respuesta, la caja calcula el `<LRC>` del mensaje y lo compara con el recibido, en el caso de coincidir la caja envía un `<ACK>` al **POS** dando por finalizado el comando; en caso contrario envía `<NAK>` y vuelve a esperar la respuesta del **POS**.

#### Solicitud de Última Venta

DATO        | LARGO     | Comentario
------      | ------    | ------
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>Valor hexadecimal</i>: `0x02`
`Comando`   | 4         | <i>Valor ASCII</i>: `0250` <br><i>Valor hexadecimal</i>: `0x30 0x32 0x35 0x30`
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`<ETX>`     | 1         | Indica el fin de texto o comando <br><i>Valor hexadecimal</i>: `0x03`
`<LRC>`     | 1         | Resultado del cálculo (byte) `XOR` del mensaje

*Mensaje* en <i>ASCII</i>: `<STX>0250|<ETX>x`

#### Respuesta de Última Venta

DATO                    | LARGO     | COMENTARIO
------                  | ------    | ------
`<STX>`                 |  1        | Indica inicio de texto o comando <br><i>Valor hexadecimal</i>: `0x02`
`Comando`               |  4        | <i>Valor ASCII</i>:  `0260` <br><i>Valor hexadecimal</i>: `0x30 0x32 0x36 0x30`
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Código Respuesta`      |  2        | Valor Numérico
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Código de comercio`    | 12        | Valor Numérico
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Terminal ID`           |  8        | Valor Alfanumérico
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Ticket`                |  6        | Valor ASCII, Número de boleta o ticket
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Código de Autorización`|  6        | Valor ASCII
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Monto`                 |  9        | Valor Numérico <br><i>Largo máximo</i>: 9 <br><i>Largo mínimo</i>: 1
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Número Cuotas`         |  2        | Valor Numérico <br><i>Largo máximo</i>: 2 <br><i>Largo mínimo</i>: 1
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Monto Cuota`           |  9        | Valor Numérico **(Opcional)** <br><i>Largo máximo</i>: 9 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Últimos 4 Digitos`     |  4        | Valor Numérico **(Opcional)** <br><i>Largo máximo</i>: 4 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Número Operación`      |  6        | Valor Numérico, Correlativo de Transacción del POS **(Opcional)** <br><i>Largo máximo</i>: 6 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Tipo de Tarjeta`       |  2        | Valor ASCII <br><i>CR</i>: Crédito <br><i>DB</i>: Débito
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Fecha Contable`        |  6        | Valor ASCII. Se utiliza solo con ventas Débito **(Opcional)**
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Número de Cuenta`      | 19        | Valor ASCII. Se utiliza solo con ventas Débito **(Opcional)** <br><i>Largo máximo</i>: 19 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Abreviación Tarjeta`   |  2        | Valor ASCII **(Opcional)** <br>[Ver Tabla de abreviación de Tarjetas](/referencia/posintegrado#tabla-de-abreviacion-de-tarjetas)
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Fecha de Transacción`  |  2        | Valor ASCII **(Opcional)** <br><i>Formato</i>: `DDMMAAAA`
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Hora de Transacción`   |  6        | Valor ASCII **(Opcional)** <br><i> Formato</i>: `HHMMSS`
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Empleado`              |  4        | Valor Numérico **(Opcional)** <br><i>Largo máximo</i>: 4</i> <br><i>Largo mínimo</i>: 0
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Propina`               |  9        | Valor Numérico <br><i>Largo máximo</i>: 9 </i> <br><i>Largo mínimo</i>: 0
`Separador`             |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`<ETX>`                 |  1        | Indica el fin de texto o comando <br><i>Valor hexadecimal</i>: `0x03`
`<LRC>`                 |  1        | Resultado del cálculo (byte) `XOR` del mensaje

<br>

### Mensaje de Anulación

Esta transacción siempre será responsabilidad de la caja y es quien decide cuando realizar una anulación.

<aside class="warning">
Las anulaciones <strong>sólo</strong> pueden realizarse para transacciones con tarjeta de crédito y que aún se encuentren en la memoria del POS.
</aside>

El comando de anulación soporta los siguientes parámetros que pueden ser enviados desde la caja.

<aside class="notice">
<strong>Número de operación:</strong> es el correlativo impreso en el voucher de venta. Este número le indicará al POS la transacción en memoria que se desea anular.
</aside>

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
RefundResp response = POS.Instance.Refund(21);
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
RefundResponse response = refund(21);
```

Como respuesta el **POS** enviará un código de aprobación, acompañado de un código de autorización. En caso de rechazo el código de error está definido en la tabla de respuestas. [Ver tabla de respuestas](/referencia/posintegrado#tabla-de-respuestas)

```json
"FunctionCode": 1210
"ResponseCode": 0
"CommerceCode": 597029414300
"TerminalId": "ABCD1234"
"AuthorizationCode": "ABCD1234"
"OperationID": 123456
"ResponseMessage": "Aprobado"
"Success": true
```

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-anulacion.png" alt="Diagrama de Secuencia Anulación">

1. La caja envía el requerimiento y espera como respuesta `<ACK>`/`<NAK>`, en caso de que llegue un `<NAK>`, debe reintentar el envío del requerimiento 2 veces más. Si recibe un `<ACK>` debe esperar la respuesta de la transacción.
2. El **POS** envía el requerimiento al autorizador, en caso de ser aprobada se guarda en batch y se envía la respuesta a la caja. En el caso de ser rechazada se envía la respuesta a la caja indicando el error.
3. La caja al recibir la respuesta envía un `<ACK>` si el mensaje está correcto, o un `<NAK>` para el caso en que el `<LRC>` no corresponda.
4. El **POS** al recibir el `<ACK>` vuelve al inicio a la espera de un nuevo comando, para el caso que reciba un `<NAK>` o no reciba ninguna validación dentro de los próximos 10 segundos; vuelve a enviar la respuesta. Esto lo repetirá 2 veces más.

#### Solicitud de Anulación

DATO                      | LARGO     | Comentario
------                    | ------    | ------
`<STX>`                   | 1         | Indica el inicio de texto o comando <br><i>Valor hexadecimal</i>: `0x02`
`Comando`                 | 4         | <i>Valor ASCII</i>: `1200` <br><i>Valor hexadecimal</i>: `0x31 0x32 0x30 0x30`
`Separador`               | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Número de Operación`     | 6         | Valor Numérico, Correlativo de Transacción del POS<br><i>Largo máximo</i>: 6
`Separador`               | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`<ETX>`                   | 1         | Indica el fin de texto o comando <br><i>Valor hexadecimal</i>: `0x03`
`<LRC>`                   | 1         | Resultado del cálculo (byte) `XOR` del mensaje

*Mensaje* en <i>ASCII</i>: `<STX>1200|10|<ETX><LRC>`

*Mensaje* en <i>Hexadecimal</i>: `{0x02, 0x31, 0x32, 0x30, 0x30, 0x7c, 0x31, 0x30, 0x7c, 0x03, 0x01}`

#### Respuesta de Anulación

DATO                      | LARGO     | COMENTARIO
------                    | ------    | ------
`<STX>`                   |  1        | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`                 |  4        | <i>Valor ASCII</i>: `1210`<br><i>Valor hexadecimal</i>: `0x31 0x32 0x31 0x30`
`Separador`               |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Código de Respuesta`     |  2        | Valor Numérico
`Separador`               |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Código de comercio`      | 12        | Valor Numérico
`Separador`               |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Terminal ID`             |  8        | Valor Alfanumérico
`Separador`               |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Código de Autorización`  |  6        | Valor Alfanumérico<br><i>Largo máximo</i>: 6
`Separador`               |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Número de Operación`     |  6        | Valor Numérico, Correlativo de Transacción del POS<br><i>Largo máximo</i>: 6
`<ETX>`                   |  1        | Indica el fin de texto o comando <br><i>Valor hexadecimal</i>: `0x03`
`<LRC>`                   |  1        | Resultado del cálculo (byte) `XOR` del mensaje

### Mensaje de Cierre

Este comando es gatillado por la caja y no recibe parámetros. El POS ejecuta la transacción de cierre contra el Autorizador (no se contempla Batch Upload). Como respuesta el POS Integrado enviará un aprobado o rechazado. (Puedes ver la tabla de respuestas en este [link](/referencia/posintegrado#tabla-de-respuestas))

<aside class="success">
Esta transacción también realiza el cambió de llaves.
</aside>

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
CloseResponse response = POS.Instance.Close();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
BaseResponse response = register_close();
}
```

El resultado del cierre de caja se entrega en la forma de un objeto `CloseResponse`o una estructura `BaseResponse` en el caso de la librería C. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankCloseException`.

```json
"FunctionCode": 510
"ResponseMessage": "Aprobado"
"Success": true
"CommerceCode": 550062700310
"TerminalId": "ABC1234C"
```

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-cierre.png" alt="Diagrama de Secuencia Cierre">

1. La caja envía el requerimiento y espera como respuesta `<ACK>`/`<NAK>`, en caso de que llegue un `<NAK>`, debe reintentar el envío del requerimiento 2 veces. Si recibe un `<ACK>` debe esperar la respuesta de la transacción.
2. El POS envía requerimiento al Autorizador, en caso de ser aprobada, se borra Batch y se envía respuesta a la caja. En caso de ser rechazada se envía respuesta a la caja indicando el error.
3. La caja al recibir la respuesta envía un `<ACK>` si el mensaje está correcto, o un `<NAK>` para el caso en que el `LRC` no corresponde.
4. El POS al recibir el `<ACK>` vuelve al inicio a esperar un nuevo comando, para el caso que recibe un `<NAK>` vuelve a enviar la respuesta 2 veces más.

<aside class="notice">
Para el cierre no se solicitara tarjeta supervisora.
</aside>

#### Solicitud de Cierre

DATO        | LARGO     | Comentario
------      | ------    | ------
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`   | 4         | <i>valor ASCII</i>: `0500` <br><i>valor hexadecimal</i>: `0x30 0x35 0x30 0x30`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`<ETX>`     | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`       | 1         | Resultado del calculo del `LRC` del mensaje

*Mensaje* en <i>ASCII</i>: `<STX>0500||<ETX>6`

*Mensaje* en <i>Hexadecimal</i>: `{0x02, 0x30, 0x35, 0x30, 0x30, 0x7c, 0x7c, 0x03, 0x06}`

#### Respuesta de Cierre

DATO                    | LARGO     | COMENTARIO
------                  | ------    | ------
`<STX>`                 |  1        | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`               |  4        | <i>Valor ASCII</i>:  `0510` <br><i>valor hexadecimal</i>: `0x30 0x35 0x31 0x30`
`Separador`             |  1        | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Código Respuesta`      |  2        | Valor Numérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Código de comercio`    | 12        | Valor Numérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Terminal ID`           |  8        | Valor Alfanumérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`<ETX>`                 |  1        | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`                 |  1        | Resultado del calculo del `LRC` del mensaje

### Mensaje de Totales

Esta operación le permitirá a la caja obtener desde el _POS_ un resumen con el monto total y la cantidad de transacciones
que se han realizado hasta el minuto y que aún permanecen en la memoria del _POS_.

Además la caja podrá determinar si existen transacciones que no fueron informadas desde el _POS_,
haciendo una comparación de los totales entre la caja y el _POS_. La impresión del _Voucher_ con el resumen será realizada por el _POS_.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
TotalsResponse response = POS.Instance.Totals();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
TotalsCResponse response = get_totals();
}
```

El resultado de la transacción entrega en la forma de un objeto `TotalsResponse` o una estructura `TotalsCResponse` en el caso de la librería C. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankTotalsException`.

```json
"Function": 710
"Response": "Aprobado"
"TX Count": 3     // Cantidad de transacciones
"TX Total": 15000 // Suma total de los montos de cada transaccion
```

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-totales.png" alt="Diagrama de Solicitud de Totales">

#### Solicitud de Totales

DATO                         | LARGO     | COMENTARIO
------                       | ------    | ------
`<STX>`                      |  1        | Indica inicio de texto o comando <br><i>Valor hexadecimal</i>: `0x02`
`Comando`                    |  4        | <i>Valor ASCII</i>: `0700` <br><i>Valor hexadecimal</i>: `0x30 0x37 0x30 0x30`
`Separador`                  |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Separador`                  |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`<ETX>`                      |  1        | Indica el fin de texto o comando <br><i>Valor hexadecimal</i>: `0x03`
`<LRC>`                      |  1        | Resultado del cálculo (byte) del `XOR` del mensaje

*Mensaje* en <i>ASCII</i>: `<STX>0700||<ETX><EOT>`

*Mensaje* en <i>Hexadecimal</i>: `{0x02, 0x30, 0x37, 0x30, 0x30, 0x7c, 0x7c, 0x03, 0x04}`

#### Respuesta de Totales

DATO                         | LARGO     | COMENTARIO
------                       | ------    | ------
`<STX>`                      |  1        | Indica inicio de texto o comando <br><i>Valor hexadecimal</i>: `0x02`
`Comando`                    |  4        | <i>Valor ASCII</i>: `0710` <br><i>Valor hexadecimal</i>: `0x30 0x37 0x31 0x30`
`Separador`                  |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Código Respuesta`           |  2        | Valor Numérico
`Separador`                  |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Cantidad de transacciones`  |  3        | Valor Numérico
`Separador`                  |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Totales`                    |  9        | Valor Numérico
`<ETX>`                      |  1        | Indica el fin de texto o comando <br><i>Valor hexadecimal</i>: `0x03`
`<LRC>`                      |  1        | Resultado del cálculo (byte) del `XOR` del mensaje

*Mensaje* en <i>ASCII</i>: `<STX>0710|00|1|2000<ETX>J`

*Mensaje* en <i>Hexadecimal</i>: `{0x02, 0x30, 0x37, 0x31, 0x30, 0x7c, 0x30, 0x30, 0x7c, 0x31, 0x7c, 0x32, 0x30, 0x30, 0x30, 0x03, 0x04}`

### Mensaje Detalle de Ventas

Esta operación solicita al POS **todas** las transacciones que se han realizado y permanecen en la memoria del POS. El parámetro que recibe esta función es de tipo booleano e indica si se realiza la impresión del detalle en el POS. En el caso de que no se solicite la impresión, el POS envía **todas** las transacciones a la caja, una por una.

<aside class="warning">
Una transacción de cierre vacía la memoria del POS
</aside>

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
bool printOnPOS = false;
List<DetailResponse> Details(printOnPOS)
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
bool print_on_pos = false;
char *response = sales_detail(print_on_pos);
}
```

El resultado de la transacción entrega una lista de objetos  `DetailResponse` o un `char *` en el caso de la librería C. Si ocurre algún error al ejecutar la acción en el POS se lanzará una excepción del tipo `TransbankSalesDetailException`.

```json
[
  {
    "Function": 261,
    "Response": "Aprobado",
    "Commerce Code": 550062700310,
    "Terminal Id": "ABC1234C",
    "Ticket": "AB123",
    "Autorization Code": "XZ123456",
    "Ammount": 15000,
    "Shares Number": 3,
    "Shares Amount": 5000,
    "Last 4 Digits": 6677,
    "Operation Number": 60,
    "Card Type": CR,
    "Accounting Date": ,
    "Account Number": ,
    "Card Brand": AX,
    "Real Date": "28/10/2019 22:35:12",
    "Employee Id": ,
    "Tip": 1500,
  },
  {
    "Function": 261,
    "Response": "Aprobado",
    "Commerce Code": 550062700310,
    "Terminal Id": "ABC1234C",
    "Ticket": "AB123",
    "Autorization Code": "XZ123456",
    "Ammount": 15000,
    "Shares Number": 3,
    "Shares Amount": 5000,
    "Last 4 Digits": 6677,
    "Operation Number": 60,
    "Card Type": CR,
    "Accounting Date": ,
    "Account Number": ,
    "Card Brand": AX,
    "Real Date": "28/10/2019 22:35:12",
    "Employee Id": ,
    "Tip": 1500
  }
]
```

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-detalle-ventas.png" alt="Diagrama de Detalle de Ventas">

#### Solicitud de Detalle de Ventas

DATO                         | LARGO     | COMENTARIO
------                       | ------    | ------
`<STX>`                      |  1        | Indica inicio de texto o comando <br><i>Valor hexadecimal</i>: `0x02`
`Comando`                    |  4        | <i>Valor ASCII</i>: `0260` <br><i>Valor hexadecimal</i>: `0x30 0x32 0x36 0x30`
`Separador`                  |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`Imprimir en Caja`           |  1        | Valor Numérico: <br><i>Imprimir en POS:</i>: `0`<br><i>Enviar a Caja:</i>`1`
`Separador`                  |  1        | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c`
`<ETX>`                      |  1        | Indica el fin de texto o comando <br><i>Valor hexadecimal</i>: `0x03`
`<LRC>`                      |  1        | Resultado del cálculo (byte) del `XOR` del mensaje

*Mensaje* en <i>ASCII</i>: `<STX>0260|1|<ETX><LRC>`

*Mensaje* en <i>Hexadecimal</i>: `{0x02, 0x30, 0x32, 0x36, 0x30, 0x7c, 0x7c, 0x03, 0x07}`

#### Respuesta de Detalle de Ventas

DATO                    | LARGO     | COMENTARIO
------                  | ------    | ------
`<STX>`                 |  1        | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`               |  4        | <i>Valor ASCII</i>:  `0261` <br><i>valor hexadecimal</i>: `0x30 0x35 0x31 0x30`
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

### Mensaje de Carga de Llaves

Esta transacción permite al POS Integrado del comercio requerir cargar nuevas _Working Keys_ desde Transbank. Como respuesta el POS Integrado enviará un aprobado o rechazado. (Puedes ver la tabla de respuestas en este [link](/referencia/posintegrado#tabla-de-respuestas))

<aside class="success">
Las llaves se deben cambiar automáticamente todos los días. Puedes usar este método como parte de un procedimiento de inicialización que se ejecute en forma automática todos los días. Ten presente que la transacción de Cierre también realiza el cambió de llaves.
</aside>

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Responses;
//...
LoadKeysResponse response = POS.Instance.LoadKeys();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
BaseResponse response = load_keys();
}
```

El resultado de la carga de llaves se entrega en la forma de un objeto `LoadKeysResponse`o una estructura `BaseResponse` en el caso de la librería C. Si ocurre algún error al momento de ejecutar la acción en el POS, se lanzará una excepción del tipo `TransbankLoadKeysException`.

```json
"FunctionCode": 510
"ResponseMessage": "Aprobado"
"Success": true
"CommerceCode": 550062700310
"TerminalId": "ABC1234C"
```

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-carga-llaves.png" alt="Diagrama de Secuencia Carga de Llaves">

1. La caja envía el requerimiento y espera como respuesta `<ACK>`/`<NAK>`, en caso de que la respuesta sea negativa, se debe reintentar el envío del requerimiento 2 veces. Si recibe un `<ACK>` se debe esperar la respuesta de la transacción.
2. El POS envía el requerimiento al Autorizador, en caso de ser aprobado, se guarda la nueva llave y se envía la respuesta a la caja. En caso de ser rechazada se indica el error a la Caja.
3. Al recibir la respuesta por parte del POS, se debe enviar un `<ACK>` si el `LRC` del mensaje es correcto, en caso contrario se debe enviar un `<NAK>`.
4. Si el POS recibe un `<ACK>` vuelve al inicio y espera un nuevo comando, si recibe un `<NAK>` reintentara el envío de la respuesta 2 veces.

<aside class="warning">
El uso de esta transacción debe ser limitado a pruebas de comunicación o cuando el POS Integrado pierda las llaves.
</aside>

#### Solicitud de Carga de Llaves

DATO        | LARGO     | Comentario
------      | ------    | ------
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`   | 4         | <i>valor ASCII</i>: `0800` <br><i>valor hexadecimal</i>: `0x30 0x38 0x30 0x30`
`<ETX>`     | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`       | 1         | Resultado del calculo del `LRC` del mensaje

*Mensaje* en <i>ASCII</i>: `<STX>0800<ETX><VT>`

*Mensaje* en <i>Hexadecimal</i>: `{0x02, 0x30, 0x38, 0x30, 0x30, 0x03, 0x0B}`

#### Respuesta de Carga de Llaves

DATO                    | LARGO     | COMENTARIO
------                  | ------    | ------
`<STX>`                 |  1        | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`               |  4        | <i>Valor Alfanumérico</i>:  `0810` <br><i>valor hexadecimal</i>: `0x30 0x38 0x31 0x30`
`Separador`             |  1        | <i>valor Alfanumérico</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Código Respuesta`      |  2        | Valor Numérico
`Separador`             |  1        |  <i>valor Alfanumérico</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Código de comercio`    | 12        | Valor Numérico
`Separador`             |  1        |  <i>valor Alfanumérico</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`Terminal ID`           |  8        | Valor Alfanumérico
`Separador`             |  1        |  <i>valor Alfanumérico **(Opcional)**</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`<ETX>`                 |  1        | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`                   |  1        | Resultado del calculo del `LRC` del mensaje

### Mensaje de Poll

Esta mensaje es enviado por la caja para saber si el POS está conectado. En el SDK el resultado de esta operación es un `Booleano` o un `0` representado en la constante `TBK_OK` en el caso de la librería en C. Si ocurre algún error al momento de ejecutar la acción en el POS, se lanzará una excepción del tipo `TransbankException`.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
//...
bool connected = POS.Instance.Poll();
```

```c
#include "transbank.h"
#include "transbank_serial_utils.h"
//...
int retval = poll();
if (retval == TBK_OK){
    //...
}
```

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-poll.png" alt="Diagrama de Secuencia Poll">

1. La caja envía el requerimiento y espera como respuesta `<ACK>`, en caso de recibir `<ACK>`, esto indica que el POS se encuentra operativo y listo para recibir comandos. si no se recibe respuesta o es `<NAK>` se debe reintentar el envío del comando 2 veces.

#### Solicitud Poll

DATO        | LARGO     | Comentario
------      | ------    | ------
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`   | 4         | <i>valor Alfanumérico</i>: `0100` <br><i>valor hexadecimal</i>: `0x30 0x31 0x30 0x30`
`<ETX>`     | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`       | 1         | Resultado del calculo del LRC del mensaje

*Mensaje* en <i>ASCII</i>: `<STX>0100<ETX><STX>`

*Mensaje* en <i>Hexadecimal</i>: `{0x02, 0x30, 0x31, 0x30, 0x30, 0x03, 0x02}`

#### Respuesta Poll

DATO                    | LARGO         | COMENTARIO
------                  | ------        | ------
`<ACK>`                 |  1            | Indica correcta recepción del comando <br><i>valor hexadecimal</i>: `0x06`

### Mensaje de Cambio a POS Normal

Este comando le permitirá a la caja realizar el cambio de modalidad a través de un comando. El POS debe estar en modo integrado y al recibir el comando quedara en modo normal.  El resultado de esta operación es un `Booleano` en el caso del SDK o un `0` representado en la constante `TBK_OK` en el caso de la librería en C. Si ocurre algún error al momento de ejecutar la acción en el POS, se lanzará una excepción del tipo `TransbankException`.

<div class="language-simple" data-multiple-language></div>

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

<img class="td_img-night" src="/images/referencia/posintegrado/diagrama-cambio-pos-normal.png" alt="Diagrama de Secuencia Cambio a POS Normal">

1. La caja envía el requerimiento y espera como respuesta `<ACK>`, en caso de recibir `<ACK>`, esto indica que el POS cambio se realizó correctamente, si no se recibe respuesta o es `<NAK>` se debe reintentar el envío del comando 2 veces.

#### Solicitud Cambio a POS Normal

DATO        | LARGO     | Comentario
------      | ------    | ------
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`   | 4         | <i>valor ASCII</i>: `0300` <br><i>valor hexadecimal</i>: `0x30 0x33 0x30 0x30`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x7c`
`<ETX>`     | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`       | 1         | Resultado del calculo del LRC del mensaje

*Mensaje* en <i>ASCII</i>: `<STX>0300<ETX><NUL>`

*Mensaje* en <i>Hexadecimal</i>: `{0x02, 0x30, 0x33, 0x30, 0x30, 0x03, 0x00}`

#### Respuesta Cambio a POS Normal

DATO                    | LARGO         | COMENTARIO
------                  | ------        | ------
`<ACK>`                 |  1            | Indica correcta recepción del comando <br><i>valor hexadecimal</i>: `0x06`

<aside class="notice">
Si el POS Integrado se cambia a modo normal, debe ser configurado nuevamente en modo Integrado siguiendo estas instrucciones [Cambio a POS Integrado](referencia/posintegrado#cambio-modalidad-pos-integrado)
</aside>

## Operación y Configuración del POS

### Cambio Modalidad POS Integrado

1. Primero debes ingresar al menú `Comercio` en el POS seleccionando la opción correspondiente en la pantalla del POS.

2. Luego debes seleccionar la opción `Func. Comercio` e ingresar la **Clave Supervisora** confirmando con la tecla `Enter` (verde).
<img src="/images/referencia/posintegrado/cambio-pos-integrado-1.png" alt="Ingresar al menu Comercio">

3. A continuación  selecciona la opción `POS Integrado` desde la pantalla 2-2

4. Ingresa nuevamente la **Clave Supervisora** confirmando con la tecla `Enter`
<img src="/images/referencia/posintegrado/cambio-pos-integrado-2.png" alt="Pantalla POS Integrado">

5. Finalmente, debes seleccionar la opción `Conectar Caja`
<img src="/images/referencia/posintegrado/cambio-pos-integrado-3.png" alt="Pantalla POS Integrado">

<aside class="notice">
La <strong>Clave Supervisora</strong> por defecto es 123456
</aside>

<aside class="warning">
Este flujo es referencial y las opciones pueden depender de los equipos que el comercio tenga contratados.
</aside>

### Cambio Modalidad POS Normal

Si el POS se encuentra en modo Integrado, podrás ver una imagen de Transbank en la pantalla. Para volver al modo Normal de forma manual:

1. Presionar la tecla Asterisco (`*`) e ingresar la clave supervisora.
2. Seleccionar la opción `Desconectar Caja`.
3. Luego de este el equipo volverá a modo Normal, y veras el menu de venta nuevamente.

## Integración de Onepay para pagos con QR

Ahora es posible realizar pagos con Onepay dentro de tu sistema de caja. Para esto ponemos a tu disposición, dentro del SDK, las herramientas necesarias para generar un QR de Onepay y procesar el pago en tu sistema de caja.

Lo primero que debes hacer es importar `Transbank.POS` y `Transbank.POS.Model` en tu proyecto, crear un nuevo objeto `OnepayPayment` y suscribirte a los eventos que te notificaran del progreso del pago en el flujo de Onepay.

### Suscribirse a eventos y Crear una transacción

```csharp
using Transbank.POS;
using Transbank.POS.Model;
//...

int ticket = new Random().Next(1, 999999);
Pay = new OnepayPayment(ticket, total);

//Subscribe to events
Pay.OnConnect += (sender, e) => UpdateWSStatus("Web Socket Conectado");
Pay.OnDisconnect += WsDisconnected;
Pay.OnNewMessage += UpdateStatus;
Pay.OnSuccessfulPayment += ProcessPaymentResult;
```

### Obtener el QR para iniciar el pago

Luego de crear el objeto OnepayPayment debes iniciar el pago, lo cual te entregará la imagen del QR en base64 y el numero de `ott`, estos le permiten al usuario pagar escaneando la imagen o ingresando el `ott` en la aplicación de Onepay.

```csharp
using Transbank.POS;
using Transbank.POS.Model;
//...

OnepayCreateResponse response = Pay.StarPayment();

//draw the base64 image
byte[] imageBytes = Convert.FromBase64String(response.QrCodeAsBase64);
using (MemoryStream ms = new MemoryStream(imageBytes, 0, imageBytes.Length))
{
  img_qr.Image = Image.FromStream(ms);
}
txt_ott.Text = response.Ott;
```

### Esperar que el usuario termine el pago en la App de Onepay

El último paso es esperar a que el usuario termine el flujo en la app de Onepay. Cuando la transacción termine exitosamente en Onepay, se lanzara el evento `SuccessfulPaymentEventArgs`

```csharp
using Transbank.POS;
using Transbank.POS.Model;
//...
Pay.WatchPayment();
```

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
