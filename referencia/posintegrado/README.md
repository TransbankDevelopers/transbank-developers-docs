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

![Diagrama de Comunicación Caja - POS](/images/referencia/posintegrado/diagrama-comunicacion-caja-pos.png)

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

## Primeros pasos

Para usar el SDK es necesario incluir las siguientes referencias.

<div class="language-simple" data-multiple-language></div>

```java
próximamente
```

```php
próximamente
```

```ruby
próximamente
```

```python
próximamente
```

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

```java
próximamente
```

```php
próximamente
```

```ruby
próximamente
```

```python
próximamente
```

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

```java
próximamente
```

```php
próximamente
```

```ruby
próximamente
```

```python
próximamente
```

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

## Mensajes

### Mensaje de Venta

Este comando es enviado por la caja para solicitar la ejecución de una venta. Los siguientes parámetros deben ser enviados desde la caja:

- `Monto`: Monto en pesos informados al POS. Este parámetro es remitido a Transbank para realizar la autorización.
- `Número Ticket/Boleta`: Este número es impreso por el POS en el voucher que se genera luego de la venta.
- `Enviar Mensaje`: Este parámetro indica al POS si debe enviar mensajes intermedios a la caja mientras se realiza el proceso de venta. Los mensajes intermedios que envía el POS y que deben ser mostrados por la Caja, deben corresponder según los siguientes códigos:
  - `78`: Lectura de Tarjeta.
  - `79`: Confirmación de Monto.
  - `80`: Selección de Cuotas.
  - `81`: Ingreso de Pinpass.
  - `82`: Envío de transacción a Transbank.

<div class="language-simple" data-multiple-language></div>

```java
próximamente
```

```php
próximamente
```

```ruby
próximamente
```

```python
próximamente
```

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
Actualmente no están soportados los mensajes intermedios. Por esta razón el 3º parámetro de la función en C debe ser siempre falso.
</aside>

![Diagrama de Secuencia Venta](/images/referencia/posintegrado/diagrama-venta.png)

1. La caja envía el requerimiento y espera como respuesta `<ACK>`/`<NAK>`, en caso de que llegue un `<NAK>`, debe reintentar el envío del requerimiento 2 veces. Si recibe un `<ACK>` debe esperar la respuesta de la transacción.
2. El POS solicita los datos al usuario, y envía el requerimiento al Autorizador, en caso de ser aprobada, se guarda en Batch y se envía respuesta a la caja. En caso de ser rechazada se envía respuesta a la caja indicando el error. ([Ver Tabla de Respuestas](/referencia/posintegrado#tabla-de-respuestas))
3. La caja al recibir la respuesta envía un `<ACK>` si el mensaje está correcto, o un `<NAK>` para el caso en que el `LRC` no corresponde.
4. El POS al recibir el `<ACK>` vuelve al inicio a esperar un nuevo comando, para el caso que recibe un `<NAK>` vuelve a enviar la respuesta 2 veces más.

#### Solicitud de Venta

DATO        | LARGO     | Comentario
------      | ------    | ------
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`   | 4         | <i>valor ASCII</i>: `0200` <br><i>valor hexadecimal</i>: `0x30 0x32 0x30 0x30`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Monto`     | 9         | Valor Numérico que debe ser convertido a hexadecimal.
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Ticket`    | 6         | Valor ASCII, Número de boleta o ticket, que debe ser convertido a hexadecimal
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Status`    | 1         | Indica al POS si debe enviar mensajes intermedios o de estado de la transacción <br><i>1</i>: Envía Mensajes<br><i>0</i>: No envía mensajes
`<ETX>`     | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`       | 1         | Resultado del calculo del `LRC` del mensaje

#### Respuesta de Venta

DATO                    | LARGO     | COMENTARIO
------                  | ------    | ------
`<STX>`                 |  1        | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`               |  4        | <i>Valor ASCII</i>:  `0510` <br><i>valor hexadecimal</i>: `0x30 0x35 0x31 0x30`
`Separador`             |  1        | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Código Respuesta`      |  2        | Valor Numérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Código de comercio`    | 12        | Valor Numérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Terminal ID`           |  8        | Valor Alfanumérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Ticket`                |  6        | Valor ASCII, Número de boleta o ticket
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Codigo de Autorizacion`|  6        | Valor ASCII
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Monto`                 |  9        | Valor Numérico <br><i>Largo máximo</i>: 9 <br><i>Largo mínimo</i>: 1
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Número Cuotas`         |  2        | Valor Numérico <br><i>Largo máximo</i>: 2 <br><i>Largo mínimo</i>: 1
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Monto Cuota`           |  9        | Valor Numérico **(Opcional)** <br><i>Largo máximo</i>: 9 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Últimos 4 Digitos`     |  4        | Valor Numérico **(Opcional)** <br><i>Largo máximo</i>: 4 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Número Operación`      |  6        | Valor Numérico, Correlativo de Transacción del POS **(Opcional)** <br><i>Largo máximo</i>: 6 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Tipo de Tarjeta`       |  2        | Valor ASCII <br><i>CR</i>: Crédito <br><i>DB</i>: Debito
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Fecha Contable`        |  6        | Valor ASCII. Se utiliza solo con ventas Debito **(Opcional)**
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Número de Cuenta`      | 19        | Valor ASCII. Se utiliza solo con ventas Debito **(Opcional)** <br><i>Largo máximo</i>: 19 <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Abreviación Tarjeta`   |  2        | Valor ASCII **(Opcional)** <br>[Ver Tabla de abreviación de Tarjetas](/referencia/posintegrado#tabla-de-abreviacion-de-tarjetas)
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Fecha de Transacción`  |  2        | Valor ASCII **(Opcional)** <br><i>Formato</i>: `DDMMAAAA`
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Hora de Transacción`   |  6        | Valor ASCII **(Opcional)** <br><i> Formato</i>: `HHMMSS`
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Empleado`              |  4        | Valor Numérico **(Opcional)** <br><i>Largo máximo</i>: 4</i> <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Propina`               |  9        | Valor Numérico <br><i>Largo máximo</i>: 9 </i> <br><i>Largo mínimo</i>: 0
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`<ETX>`                 |  1        | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`                   |  1        | Resultado del calculo del `LRC` del mensaje

### Mensaje de Cierre

Este comando es gatillado por la caja y no recibe parámetros. El POS ejecuta la transacción de cierre contra el Autorizador (no se contempla Batch Upload). Como respuesta el POS Integrado enviará un aprobado o rechazado. (Puedes ver la tabla de respuestas en este [link](/referencia/posintegrado#tabla-de-respuestas))

<div class="language-simple" data-multiple-language></div>

```java
próximamente
```

```php
próximamente
```

```ruby
próximamente
```

```python
próximamente
```

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

![Diagrama de Secuencia Cierre](/images/referencia/posintegrado/diagrama-cierre.png)

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
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`<ETX>`     | 1         | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`       | 1         | Resultado del calculo del `LRC` del mensaje

*Mensaje* en <i>ASCII</i>: `<STX>0500||<ETX>6`

*Mensaje* en <i>Hexadecimal</i>: `{0x02, 0x30, 0x35, 0x30, 0x30, 0x07, 0x07, 0x03, 0x06}`

#### Respuesta de Cierre

DATO                    | LARGO     | COMENTARIO
------                  | ------    | ------
`<STX>`                 |  1        | Indica inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`               |  4        | <i>Valor ASCII</i>:  `0510` <br><i>valor hexadecimal</i>: `0x30 0x35 0x31 0x30`
`Separador`             |  1        | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Código Respuesta`      |  2        | Valor Numérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Código de comercio`    | 12        | Valor Numérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Terminal ID`           |  8        | Valor Alfanumérico
`Separador`             |  1        |  <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`<ETX>`                 |  1        | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`                 |  1        | Resultado del calculo del `LRC` del mensaje

### Mensaje de Carga de Llaves

Esta transacción permite al POS Integrado del comercio requerir cargar nuevas _Working Keys_ desde Transbank. Como respuesta el POS Integrado enviará un aprobado o rechazado. (Puedes ver la tabla de respuestas en este [link](/referencia/posintegrado#tabla-de-respuestas))

<div class="language-simple" data-multiple-language></div>

```java
próximamente
```

```php
próximamente
```

```ruby
próximamente
```

```python
próximamente
```

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

![Diagrama de Secuencia Carga de Llaves](/images/referencia/posintegrado/diagrama-carga-llaves.png)

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
`Separador`             |  1        | <i>valor Alfanumérico</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Código Respuesta`      |  2        | Valor Numérico
`Separador`             |  1        |  <i>valor Alfanumérico</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Código de comercio`    | 12        | Valor Numérico
`Separador`             |  1        |  <i>valor Alfanumérico</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`Terminal ID`           |  8        | Valor Alfanumérico
`Separador`             |  1        |  <i>valor Alfanumérico **(Opcional)**</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
`<ETX>`                 |  1        | Indica el fin de texto o comando <br><i>valor hexadecimal</i>: `0x03`
`LRC`                   |  1        | Resultado del calculo del `LRC` del mensaje

### Mensaje de Poll

Esta mensaje es enviado por la caja para saber si el POS está conectado. En el SDK el resultado de esta operación es un `Booleano` o un `0` representado en la constante `TBK_OK` en el caso de la librería en C. Si ocurre algún error al momento de ejecutar la acción en el POS, se lanzará una excepción del tipo `TransbankException`.

<div class="language-simple" data-multiple-language></div>

```java
próximamente
```

```php
próximamente
```

```ruby
próximamente
```

```python
próximamente
```

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

![Diagrama de Secuencia Poll](/images/referencia/posintegrado/diagrama-poll.png)

1. La caja envía el requerimiento y espera como respuesta `<ACK>`, en caso de recibir `<ACK>`, esto indica que el POS se encuentra operativo y listo para recibir comandos. si no se recibe respuesta o no es `<ACK>` se debe reintentar el envío del comando 2 veces.

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

```java
próximamente
```

```php
próximamente
```

```ruby
próximamente
```

```python
próximamente
```

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

![Diagrama de Secuencia Cambio a POS Normal](/images/referencia/posintegrado/diagrama-cambio-pos-normal.png)

1. La caja envía el requerimiento y espera como respuesta `<ACK>`, en caso de recibir `<ACK>`, esto indica que el POS cambio se realizó correctamente, si no se recibe respuesta o no es `<ACK>` se debe reintentar el envío del comando 2 veces.

#### Solicitud Cambio a POS Normal

DATO        | LARGO     | Comentario
------      | ------    | ------
`<STX>`     | 1         | Indica el inicio de texto o comando <br><i>valor hexadecimal</i>: `0x02`
`Comando`   | 4         | <i>valor ASCII</i>: `0300` <br><i>valor hexadecimal</i>: `0x30 0x33 0x30 0x30`
`Separador` | 1         | <i>valor ASCII</i>: <code>&#124;</code> <br><i>valor hexadecimal</i>: `0x07`
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
![Ingresar al menu Comercio](/images/referencia/posintegrado/cambio-pos-integrado-1.png)

3. A continuación  selecciona la opción `POS Integrado` desde la pantalla 2-2

4. Ingresa nuevamente la **Clave Supervisora** confirmando con la tecla `Enter`
![Pantalla POS Integrado](/images/referencia/posintegrado/cambio-pos-integrado-2.png)

5. Finalmente, debes seleccionar la opción `Conectar Caja`
![Pantalla POS Integrado](/images/referencia/posintegrado/cambio-pos-integrado-3.png)

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
