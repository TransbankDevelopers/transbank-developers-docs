# POS Integrado

## Cómo empezar

El SDK para POS Integrado, cuenta de 2 partes, la primera pieza es una librería/SDK escrito en C,
el cual es multi plataforma, y debe ser instalado en tu maquina para poder ser utilizado por la
segunda pieza, la cual mediante un Wrapper, accede a las funciones disponibles en la librería C.

Adicionalmente, necesitaras tener instalados los drivers correspondientes a tu tarjeta de
puerto serial o adaptador USB Serial.

<aside class="notice">
La comunicación con el POS Integrado se realiza mediante puerto serial RS232
</aside>

### Instalación del SDK

### Primeros pasos

Para usar el SDK es necesario incluir los respectivos paquetes a utilizar.

```csharp
using Transbank.POS;
using Transbank.POS.Utils;
using Transbank.POS.Responses:
```

### Listar puertos disponibles

Si los respectivos drivers están instalados, entonces puedes usar la función `ListPorts()` del paquete  
`Transbank.POS.Utils`para identificar los puertos que se encuentren disponibles y seleccionar el que 
corresponda con el puerto donde conectaste el POS Integrado.

```csharp
List<string> ports = Serial.ListPorts();
```

### Abrir un puerto Serial

Para abrir un puerto serial y comunicarte con el POS Integrado, necesitaras el nombre del puerto (El cual
puedes identificar usando la función mencionada en el apartado anterior #Listar puertos disponibles). Tambien
necesitaras el baudrate al cual esta configurado el puerto serial del POS Integrado (Por defecto es 115200),
y puedes obtener los distintos valores desde la clase `TbkBaudrates` del paquete `Transbank.POS.Utils`.

Si el puerto no puede ser abierto, se lanzara una exception `TransbankException`.

```csharp
using Transbank.POS;
using Transbank.POS.Utils;
//...
string portName = "COM3";
POS pos = new POS();
pos.OpenPort(portName, TbkBaudrate.TBK_115200);
```


### Cerrar un puerto Serial

Al finalizar el uso del POS, o si se desea desconectar de la Caja se debe liberar el puerto serial abierto anteriormente.

```csharp
using Transbank.POS;
//...
bool closed = pos.ClosePort();
```

## Transacciones Soportadas

### Transacción de Pooling

Esta transacción es enviada por la caja para saber si el POS está conectado. El resultado de esta operación es un `Booleano` en el caso del SDK o un `0` representado en la constante `TBK_OK` en el caso de la librería en C

```csharp
