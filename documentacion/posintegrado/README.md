# POS Integrado

## Cómo empezar

El SDK para POS Integrado, cuenta de 2 partes, la primera pieza es una librería/SDK escrito en C,
el cual es multi plataforma, y debe ser instalado en tu maquina para poder ser utilizado por la
segunda pieza, la cual mediante un Wrapper, accede a las funciones disponibles en la librería C.

Adicionalmente, necesitaras tener instalados los drivers correspondientes a tu tarjeta de
puerto serial o adaptador USB Serial.

<aside class="notice">
La comunicación con el POS Integrado se realiza mediante puerto serial RS232.
</aside>

<aside class="notice">
Tu eres el responsable de instalar el driver correcto para tu tarjeta o adaptador serial.
</aside>

### LibSerialPort

El SDK depende de [libSerialPort](https://sigrok.org/wiki/Libserialport) para la comunicación serial.

Puedes obtener el código desde el repositorio usando git:

```bash
git clone git://sigrok.org/libserialport
```

Para compilar en windows necesitaras lo siguiente:

- [msys2 - mingw-w64](http://www.msys2.org/) Puedes descargarlo siguiendo el link y las instrucciones provistas en su sitio web.
  - Adicionalmente, necesitaras el toolchain para tu arquitectura:
    - x86: `pacman -S mingw-w64-i686-toolchain`
    - x64: `pacman -S mingw-w64-x86_64-toolchain`

<aside class="notice">
Procura seguir todos los pasos descritos en el sitio de msys2
</aside>

### Primeros pasos

Para usar el SDK es necesario incluir las siguientes referencias.

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

```csharp
using Transbank.POS.Utils;
//...
List<string> ports = Serial.ListPorts();
```

```c
#include "transbank_serial_utils.h"
//...
char * ports = list_ports();
```

### Abrir un puerto Serial

Para abrir un puerto serial y comunicarte con el POS Integrado, necesitaras el nombre del puerto (El cual
puedes identificar usando la función mencionada en el apartado anterior #Listar puertos disponibles). También
necesitaras el baudrate al cual esta configurado el puerto serial del POS Integrado (Por defecto es 115200),
y puedes obtener los distintos valores desde la clase `TbkBaudrates` del paquete `Transbank.POS.Utils`.

Si el puerto no puede ser abierto, se lanzara una exception `TransbankException`.

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
char* portName = "COM4";
int retval = open_port(portName, 115200);
if ( retval == TBK_OK ){
    //...
}
```

### Cerrar un puerto Serial

Al finalizar el uso del POS, o si se desea desconectar de la Caja se debe liberar el puerto serial abierto anteriormente.

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

### Transacción de Cierre

Este comando es gatillado por la caja y no recibe parámetros. El POS ejecuta la transacción de cierre contra el Autorizador (no se contempla Batch Upload). Como respuesta el POS Integrado enviará un aprobado o rechazado. (Puedes ver la tabla de respuestas en este [link](/referencias/posintegrado#tabla-de-respuestas))

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

<aside class="notice">
Para el cierre no se solicitara tarjeta supervisora.
</aside>

### Transacción de Carga de Llaves

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

### Transacción de Pooling

Esta transacción es enviada por la caja para saber si el POS está conectado. El resultado de esta operación es un `Booleano` en el caso del SDK o un `0` representado en la constante `TBK_OK` en el caso de la librería en C.

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

### Transacción de Cambio a POS Normal

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
