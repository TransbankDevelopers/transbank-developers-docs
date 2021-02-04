# Host to Host Pinpad Wifi

### Protocolo de mensajería

**Seguridad TLS 1.2**

SSL (Secure Sockets Layer) o Capa de Conexiones Seguras. Es un protocolo que hace uso de certificados
digitales para establecer comunicaciones seguras a través de red. Desde el 2015 ha sido sustituido por
TLS (Transport Layer Security) el cual está basado en SSL y son totalmente compatibles. 

**Seguridad en la Red WI-FI del Comercio**

En el caso de que la red del Comercio requiera usar conexiones por WI-FI se exige que la red sea una red
cifrada *WPA2-PSK (AES). 

**Recomendaciones de seguridad para una red WI-FI:**
Cambiar regularmente la contraseña de red WI-FI
    
1. Al cambiar regularmente la contraseña de red WI-FI evita que terceras personas puedan hacer uso de la red del comercio.
2. Configurar la red WI-FI como “No visible”
   Al ocultar la red WI-FI se evita que personas externas al comercio puedan encontrar e
   intentar acceder a la red del comercio.
   Ahora cada vez que se intente conectar un nuevo dispositivo, será necesario colocar
   primero la *SSID, para posteriormente ingresar la contraseña
3. Registrar y restringir las conexiones por MAC
   Al tener habilitadas las conexiones por MAC, se especifica que equipos pueden hacer
   uso de la red WI-FI, evitando que cualquier otro equipo haga uso de la red del comercio.
4. Restringir el acceso a la “Configuración del Router” desde WI-FI
   Al restringir el acceso a la configuración del Router desde conexiones WI-FI se evita que
   desde dispositivos WIFI se pueda acceder a esta configuración y se modifiquen sus
   parámetros.
5. Monitorear regularmente las conexiones WI-FI
   Los Router actuales permiten conocer los dispositivos que están conectados a la red WIFI. Es recomendable monitorear para poder evitar algún dispositivo que esté conectado
   sin autorización. 

*WPA2-PSK (AES): Sistema de protección para redes inalámbricas WI-FI. Es el último estándar de encriptación WI-FI y AES es el
más reciente algoritmo de cifrado.

*SSID: Difusión de un SSID de red. Un SSID es el nombre público de una red de área local inalámbrica (WLAN) que sirve para
diferenciarla de otras redes inalámbricas en la zona. SSID es el nombre de la red que se especifica al configurar la red WI-Fi.

### Archivo de implementación TLS
La implementación de TLS realizada es la de autenticación mutua two-way, es decir el PinPad valida a la
caja y a la inversa.
Para implementar TLS entre el Pinpad vx680 y la caja del comercio se requieren de los siguientes
archivos: 

**Certificados TLS del Pinpad:**
1. Certificado cliente: "client.PEM": Certificado con el que se presenta al Servidor.
2. Llave de validación del certificado de cliente: "client.key": Necesario para validar que el certificado cargado es el que corresponde.
3. Certificado para validar el servidor: "CA.PEM": Para validar el certificado del Servidor.

**Certificados TLS del Servidor (Caja del Comercio):**
1. Certificado servidor: "server.PEM": Certificado con el que se presenta al cliente.
2. Llave de validación del certificado de servidor: "server.key": Llave privada usada para firmar.
3. Certificado para validar el cliente: "CAClient.PEM": Para validar el certificado del cliente.

### Implementación TLS
Se debe cifrar el canal de comunicación con la versión más reciente del protocolo de comunicación TLS.
Se debe usar la autenticación doble (Two-way TLS) para darle mayor seguridad a proceso.

![Implementación TLS](/images/documentacion/host2host/implementacion-tls.png)

El cliente tiene la CA del certificado del servidor en su almacén de confianza.
Durante el protocolo de enlace TLS, el cliente compara la CA certificado en su almacén de confianza con el envío del certificado desde el servidor para verificar la identidad del servidor.
El servidor tiene la CA del certificado del cliente en su almacén de confianza.
Durante el protocolo de enlace TLS, el servidor compara la CA en su almacén de confianza con el envío del certificado del cliente para verificar la identidad del cliente.

Dependiendo del lenguaje o plataforma, se debe especificar la ubicación de los “Almacenes de Certificados” (KeyStore y TrustStore).
Dependiendo del lenguaje o plataforma se crea un KeyStore para el certificado y la llave privada del Servidor y un TrustStore para el “Certificado CA” del Pinpad.

```java
//Especificamos el KeyStore que contiene el certificado del Servidor y la llave privada
KeyStore keyStore = KeyStore.getInstance("JKS");
keyStore.load(new FileInputStream("server/serverKeyStore.jks"),"servpass".toCharArray());

KeyManagerFactory kmf = KeyManagerFactory.getInstance(KeyManagerFactory.getDefaultAlgorithm());
kmf.init(keyStore, "servpass".toCharArray());

//Especificamos el TrustStore que contiene el Certificado CA del cliente
KeyStore trustedStore = KeyStore.getInstance("JKS");
trustedStore.load(new FileInputStream("server/serverTrustedCerts.jks"),"servpass".toCharArray());

TrustManagerFactory tmf = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
tmf.init(trustedStore);

//Creamos la instancia TLS
SSLContext sc = SSLContext.getInstance("TLSv1.2");
TrustManager[] trustManagers = tmf.getTrustManagers();
KeyManager[] keyManagers = kmf.getKeyManagers();
sc.init(keyManagers, trustManagers, null);

//se crea el socket que se usara para la comunicación
SSLServerSocketFactory ssf = sc.getServerSocketFactory();
serverSocket = (SSLServerSocket) ssf.createServerSocket(port);

try {
   //Esperamos que el PINPAD se conecte
   Socket aClient = serverSocket.accept();
   System.out.println("client accepted");
   aClient.setSoLinger(true, 1000);
   aClient.setSoTimeout(10 * 1000);
   
   //Leemos lo enviado desde el PINPAD
   BufferedReader input = new BufferedReader(new InputStreamReader(aClient.getInputStream()));
   String recibido = input.readLine();
   System.out.println("Recibido " + recibido);
   
   //Enviamos ACK al Pinpad
   PrintWriter output = new PrintWriter(aClient.getOutputStream());
   output.println("0004ACK ");
   output.flush();
   
   //Enviamos Comando al Pinpad
   output.println("0025CONN|00|01|Texto línea 1|");
   output.flush();

} catch (Exception e) {
   e.printStackTrace();
} 
```

### Protocolo de comunicación TCP/IP 
Este protocolo permite la comunicación entre un ECR (Electronic Cash Register) y un
PINPAD a través de una red que soporte el protocolo TCP/IP. Es importante tener en
consideración el control de errores sobre las funciones de TCP/IP para asegurar que cada
uno de los mensajes llegue al destinatario y hacer gestión sobre los reintentos cuando sea
necesario. El siguiente diagrama ejemplifica la construcción de los comandos en el protocolo
TCP/IP.

![Protocolo TCP/IP](/images/documentacion/host2host/protocolo-tcp.png)

Donde:
* LARGO_DATA: 4 bytes indicando el largo del mensaje, sin incluir estos 4 bytes.
* DATA: Es el mensaje que se quiere enviar.

**Diagrama genérico de secuencia de comandos**

El diagrama que se describe corresponde a la generalización del comportamiento de cada
uno de los comandos.
  
![Protocolo TCP/IP Caja a Pinpad](/images/documentacion/host2host/flujo-secuencia-wifi.png)

![Protocolo TCP/IP PinPad a Caja](/images/documentacion/host2host/flujo-secuencia-wifi-pinpad-caja.png)

**Flujo de venta detallado**

![Secuencia detallada venta](/images/documentacion/host2host/secuencia-detallada-venta-wifi.png)

**Diagrama**

![Diagrama detallada venta](/images/documentacion/host2host/diagrama-flujo-detallado-venta.png)


### Ejemplo de venta:
```
0100|00|N|N|N|12100|CL|CR||0|0|
0110|00|2017111611350940|01|||||5197||MASTERCARD|MC|N| 4
0200|12100|0||0|0|2017111611350940|00|597044440001|S4HOST2HOST3DES1||||17111611361100
000000000754||0123456789ABCDEF|||||5197| v
0210|00|2017111611350940|0737|9.12S4HOST2HOST3DES1
171116113517FO00050000B000000000000012100P1Q0123456789ABCDEFa000000000000000000000000
00NU1711161136110000000000075400000000000000000CL0000000d597044440001e00h0010050081G3
F3308F4S0t74 0000000000000000326-478-322 15.30C
6-E051-I152-O0180152171116B8738FEAC1697887380000669B8C9262000000800000152000000012100
0000000000000014A78003040000716800000000000000FF-P0100224403020002
E0F8C826478322RA00000000410109-A1EL20212223242526272829VI0 6MC0 67DC0
6AX012345OTTP06TR01TE0 TM0 TC12TD12TJ12TH12T812T90
-B41205240-C2100-P000000000000-I0-J0-K000W0161111005CR 0000-
4F552D8E65F6056E543A481CDD07D2525E2D7347C32D2CA5756F176482684949FD0443BCB1235018CC0CD
DC7C0EA41BF| (
0500|2017111611350940|513|9.12S4HOST2HOST3DES1 
171116113521FO00000005B000000000000012100D4EF600979
BGA1B5296BH0D9C83C5A59B574F7AF35145C606D5D4ID5879B9816C5A304236AB90B081A60A0P1Q012345
6789ABCDEFS0
T0000000000W0161111005CRCMC0000a00000000000003000000000000NU1711161136110000000000075
400000000000000000CL0000000d597044440001e00g
APROBADOh0010050081ptS4HOST2HOST3DES16-E051-I1529-A1EL20212223242526272829VI0 6MC0
67DC0 6AX012345OTTP06TR01TE0 TM0 TC12TD12TJ12TH12T812T90
-B11205243-C0000-P100000800000|
0510|00|2017111611350940|597044440001|S4HOST2HOST3DES1|0 |0000|600979
B|12100||00||5197|001005008|CREDITO||************5197|MC|171116|113521||||||||1|1|1|1
|0|05|CR|0|0|0000|SIN CUOTAS|||||||0000|||0000|||0000|||005|
APROBADO|N||0|1||001005008|Y||| 
```

**NOTA:** Si por algún motivo el pinpad responde un comando en un formato que no
corresponde (tipo de dato erróneo, largo incorrecto, separador incorrecto o faltante,
etcétera) la caja debe solicitar reversa según el “flujo de ejecución de reversa a
solicitud de la caja” detallado a continuación. Posteriormente se debe iniciar un
nuevo flujo de venta. 

### Flujo ejecución de reversa a solicitud de la caja
La caja no tuvo respuesta de algún comando o no tuvo respuesta de un mensaje SPDH en
vuelo, por lo tanto no pudo terminar una venta que inicio, luego no sabe si está aprobada o
rechazada, en este caso debe solicitar una reversa al pinpad. 

![Flujo reversa](/images/documentacion/host2host/reversa-caja.png)

**Ejemplo de reversa:**
```
0400|2019062813081650|
0410|00|2019062813081650|0643|9.07S4CAJAHOST000010
190628130853FT00000000B000000000000650000P1Q0123456789ABCDEFa000000150000000000000000
00NU2019062813091100100100000100000000000000000CL0000000d597044440001e00h0010030051G3
F27A500S0t74 0000000000000000326-018-973 18.21P
6-E071-I152-O0180152190628236E485C1DC23FC81980067116EE3A5C000000800000152000000650000
0000000000000110A04001220000000000000000000000FF-P0101221F03020002
00080826018973A00000000410109-A1EL20252627 VI123456MC123456DC
AX123456OTTP06TR1 TE0 TM0 TC12TD12TJ12TH12T812T90
-B41205240-C2100-P000000000000-I0-J1-K000-M1W0161111005CR 0000|
0500|2019062813081650|643|9.07S4CAJAHOST000010
190628130853FT00000000B000000000000650000P1Q0123456789ABCDEFa000000150000000000000000
00NU2019062813091100100100000100000000000000000CL0000000d597044440001e00h0010030051G3
F27A500S0t74 0000000000000000326-018-973 18.21P
6-E071-I152-O0180152190628236E485C1DC23FC81980067116EE3A5C000000800000152000000650000
0000000000000110A04001220000000000000000000000FF-P0101221F03020002
00080826018973A00000000410109-A1EL20252627 AX123456OTTP06TR1 TE0 TM0 TC12TD12TJ12TH12T812T90
-B41205240-C2100-P000000000000-I0-J1-K000-M1W0161111005CR 0000|
0510|00|2019062813081650|597044440001|S4CAJAHOST000010|||265404
B|650000||00||0003|001003005|CREDITO||************0003|
|190628|130853||||||||1|1|1|1|0|05|CR|0|0|0000|SIN
CUOTAS|||||||0000|||0000|||0000|||000|REVERSA APLICADA|N||0|1||001003005|Y|||
```

**Diagrama Inicialización y conexión al PINPAD**

![Diagrama inicialización y conexión](/images/documentacion/host2host/diagrama-inicializacion-y-conexion.png)

**Flujo de ejecución de comandos al PINPAD**

![Flujo de ejecución de comandos](/images/documentacion/host2host/flujo-comandos-pinpad-wifi.png)

**Manejo de contexto**

![Flujo de ejecución de comandos](/images/documentacion/host2host/manejo-de-contexto.png)

## Descripción de comandos 

### 0100 - Comando Lectura de tarjeta
Igual al Retail estándar vx805

### 0200 - Comando Requerimiento de venta/anulación
Igual al Retail estándar vx805

###  0400 - Comando Requerimiento de reversa
Igual al Retail estándar vx805

###  0500 - Comando Requerimiento de validación/actualización
Igual al Retail estándar vx805

### 0520 - Comando Requerimiento de validación respuesta de transacción
Este comando es un homologo al 500, donde caja entrega el mensaje spdh al pinpad, pero
el pinpad cuando la transacción está aprobada oculta el mensaje en pantalla, si la
transacción es rechazada se comporta igual al comando 510.
Al finalizar la transacción caja debe enviar comando 1100 al pinpad con el mensaje que se
requiere desplegar en pantalla (Aprobado). 

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes |
Comando | 4 | Valor 0520 | 
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Indicador de contexto | 16  | Valor alfanumérico Formato aaaammddhhmmssmm
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Largo mensaje | 4 | Valor Numérico |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Mensaje SPDH | 2048 | Valor alfanumérico (máximo) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

Nota: No despliega glosa en pantalla del PINPAD ni actualiza parámetros

**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes
Comando | 4 | Valor 0530 |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código Respuesta | 2 | Valor numérico De acuerdo a Tabla de códigos de respuesta decomandos  |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Indicador de contexto | 16 | Valor alfanumérico Formato aaaammddhhmmssmm |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código de comercio | 12 | Valor alfanumérico |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Terminal ID | 16 | Valor Alfanumérico <br >Campo “t” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número Ticket/Boleta | 20 | Valor alfanumérico<br>Campo “S” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Empleado | 4 | Valor alfanumérico <br> Campo “T” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código Autorización | 8 | Valor Alfanumérico (máximo) <br> Campo “F” mensaje SPDH
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Monto | 18 | Valor numérico (máximo) <br> Campo “B” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Monto vuelto | 18 | Valor numérico (máximo) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Numero de Cuotas | 2 | Valor numérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Monto Cuota | 14 | Valor numérico <br>Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Últimos 4 Dígitos Tarjeta | 4 | Valor Numérico |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número Operación | 6 | Correlativo de transacción del terminal (máximo)
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa Tipo de Tarjeta | 7 | Valor alfanumérico (máximo) <br> Valor de glosa en Tabla tipo de tarjeta de acuerdo “Tipo de tarjeta” del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Fecha Contable | 6 | Valor alfanumérico <br> Se utiliza sólo si es transacción de Debito Del campo “a” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número de Cuenta | 19 | Valor alfanumérico <br> Número de tarjeta enmascarado para incluir en el voucher Campo “E” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Abreviación de la tarjeta | 2 | Valor alfanumérico <br> Del campo “W” mensaje SPDH
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Fecha Transacción | 6 | Formato AAMMDD Del encabezado mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Hora Transacción | 6 | Formato HHMMSS Del encabezado mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Campo Impresión | 8192 | Campo depende si la caja requiere voucher formateado (máximo) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Transacción premiada | 1 | Valor numérico <br> Posición 1, campo “f” mensaje SPDH <br> (1: Entrega Pto. de Venta)<br> (2: Entrega Diferida) | (3: Devolución al Tarjeta Habiente)
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Tipo promoción | 1 | Valor numérico <br> Posición 2, campo “f” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código promoción | 8 | Valor alfanumérico <br> Posición 3-10, campo “f” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Nombre promoción | 21 | Valor alfanumérico <br>  Posición 11-31, campo “f” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa vale premio | 62 | Valor alfanumérico <br>  Posición 32-93, campo “f” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Texto vale premio | 27 | Valor alfanumérico <br>  Posición 94-120, campo “f” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag permite cuotas | 1 | Valor numérico <br> Del campo “W” mensaje SPDH | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag de gracia | 1 | Valor numérico <br>  Del campo “W” mensaje SPDH | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag C2C | 1 | Valor numérico <br> Del campo “W” mensaje SPDH | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag C3C | 1 | Valor numérico <br> Del campo “W” mensaje SPDH | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag NCuotas | 1 | Valor numérico <br> Del campo “W” mensaje SPDH | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag máximo de cuotas | 2 | Valor numérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Tipo de menú | 2 | Valor alfanumérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Indicador transacción con gracia | 1 | Valor numérico <br>Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Tipo cuotas | 1 | Valor numérico <br>Del campo “W” mensaje SPDH <br>(1 : Normal) <br>(3 : C3C o C2C) <br>(4 : Cic o N-cuotas) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Tasa aplicada | 4 | Valor numérico <br> Del campo “W” mensaje SPDH <br> Solo se imprime en voucher si “Flag imprimir tasa = 1” |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa tipo cuota | 30 | Valor alfanumérico <br> Del campo “W” mensaje SPDH <br> (Glosa a imprimir en voucher) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa tipo cuota 2 | 22 | Valor alfanumérico <br> Del campo “W” mensaje SPDH <br> (Glosa que se despliega en PINPAD) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa promoción | 10 | Valor alfanumérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Id promoción | 8 | Valor alfanumérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag imprimir tasa | 1 | Valor numérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Periodo diferido | 3 | Valor numérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 1 periodo | 3 | Valor numérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 1 valor tasa | 4 | Valor numérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 1 valor cuota | 14 | Valor numérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 2 periodo | 3 | Valor numérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 2 valor tasa | 4 | Valor numérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 2 valor cuota | 14 | "Valor numérico  <br> Del campo “W” mensaje SPDH" |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 3 periodo | 3 | Valor numérico  <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 3 valor tasa | 4 | Valor numérico <br> Del campo “W” mensaje SPDH|
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 3 valor cuota | 14 | Valor numérico <br> Del campo “W” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número de secuencia transacción original | 9 | Valor alfanumérico (máximo)  <br> Campo “i”. Sólo para anulaciones |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código respuesta Transbank | 3 | Valor Numérico |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa respuesta Transbank | 48 | Valor alfanumérico (máximo) <br> Campo “g” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag transacción con PIN | 1 | Valor alfanumérico  <br> (Y: Transacción autentificada con PIN) <br> (N: Transacción autentificada por firma) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Nombre tarjetahabiente | 26 | Valor alfanumérico <br> Sólo si “Flag transacción con PIN = N” |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag tipo voucher | 1 | Valor numérico  <br> (0: Venta con PIN, sin firma, sin glosa) <br> (1: Venta normal, con firma, con glosa) <br> (2: Venta con PIN, con firma, sin glosa) <br> (3: Venta normal, con firma, sin glosa) <br> Posición 8, subcampo “B” campo “9” mensaje SPDH |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag modalidad cuotas | 1 | Valor alfanumérico  <br> (0: Modalidad 3.1) <br> (1: Modalidad nuevas cuotas) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa transacción afecta a ahorro | 40 | Valor alfanumérico (máximo)  <br> (Se debe imprimir en el voucher cuando corresponda) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número de secuencia | 9 | Valor numérico |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag mensaje terminal | 1 | Valor alfanumérico  <br> (Y: El mensaje es terminal y NO se debe enviar el mensaje SPDH de respuesta) <br> (N: Se debe enviar el mensaje SPDH de respuesta) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Largo mensaje | 4 | Valor numérico |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Mensaje SPDH Venta/Reversa | 2048 | Valor alfanumérico (máximo) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

### 0560 - Comando Requerimiento de validación respuesta de transacción
Este comando es un homologo al 540, donde la caja entrega el mensaje spdh al pinpad.
La diferencia es que el comportamiento definido para el pinpad con este comando es de omitir la glosa de aprobado en pantalla.

La glosa que indica la aprobación de la transacción solo será mostrada en pantalla cuando el pinpad reciba el comando 1100 desde la caja, este comando debe incluir el código del mensaje que se requiere desplegar, para este caso Aprobado.

Cuando la caja requiera finalizar la aprobación de la transacción, una vez haya recibido el comando 570 con el flag terminal en Y, debe enviar el comando 1100 con el código de mensaje a mostrar en el pinpad.

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
`<STX>` | 1 | Indica inicio de comando Valor Hexa 0x02 | STX
Comando | 4 | Valor 0560 | 560
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Indicador de contexto | 16 | Valor alfanumérico <br> Id entregado por el pinpad por cada transacción |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Largo mensaje | 4 | Valor Numérico |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Mensaje SPDH | 2048 | Valor alfanumérico (máximo) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Nombre Comercio | 40 | Valor Alfanumérico <br> Campo paramétrico en caja enviado al pinpad |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Dirección Comercio | 40 | Valor Alfanumérico <br> Campo paramétrico en caja enviado al pinpad |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Comuna Comercio | 40 | Valor Alfanumérico <br> Campo paramétrico en caja enviado al pinpad  <br> Puede ser comuna o ciudad |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
`<ETX>` | 1 | Indica Fin de comando Valor Hexa 0x03 | ETX
`<LRC>` | 1 | Byte resultado de la operación XOR del mensaje |

Timeout de espera por comando 560 de 125seg, ya que hay hasta 4 interacción con el usuario

**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
<STX> | 1 | Indica inicio de comando Valor Hexa 0x02 | STX
Comando | 4 | Valor 0570 | 570
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código Respuesta PinPad | 2 | "Valor numérico <br> En caso de rechazo se debe desplegar en el punto de venta: <br /> RECHAZO PINPAD - <XX> : <GLOSA> <br /> De acuerdo a Tabla de códigos de respuesta de comandos" |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Indicador de contexto | 16 | Valor alfanumérico <br /> Id entregado por el pinpad por cada transacción |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código de comercio | 12 | Valor numérico  <br /> Código del comercio entregado por TBK y configurado en la caja, se imprime en voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Terminal ID | 16 | Valor Alfanumérico <br /> Dirección lógica entregada por TBK y configurada en la caja, se imprime en voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número Ticket/Boleta | 20 | Valor alfanumérico <br /> Campo opcional, si viene se imprime en voucher si no viene se omite el campo |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Empleado | 4 | Valor alfanumérico <br /> Campo opcional, si viene se imprime en voucher si no viene se omite el campo |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código Autorización | 8 | Valor Alfanumérico (máximo) <br /> Código de autorización de la transacción enviado por TBK ejemplo:  `[AB 12 C3]` <br /> Se imprime lo que viene en el voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Monto | 18 | Valor numérico (máximo) <br /> Monto total autorizado (incluye el monto de la venta, propina, vuelto y donación según sea el caso) <br /> Se imprime en voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Monto vuelto | 18 | Valor numérico (máximo) <br /> Vuelto seleccionado por cliente, solo aplica en débito <br /> Se imprime en voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Numero de Cuotas | 2 | Valor numérico <br /> Cantidad de cuotas de la transacción (para ventas sin cuotas se informa “00”) <br /> Se imprime en voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Monto Cuota | 14 | Valor numérico <br /> Si el monto informado es vacío `` o `0` caja debe omitir la línea completa en el voucher. <br /> Se imprime en voucher si viene el campo |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Últimos 4 Dígitos Tarjeta | 4 | Valor Numérico <br /> No se imprime |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número Operación | 9 | Correlativo de transacción del terminal <br /> También conocido como número de secuencia este campo se debe imprimir en voucher de venta y anulación. |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa Tipo de Tarjeta | 7 | Valor alfanumérico (máximo) <br /> Valor de glosa en Tabla tipo de tarjeta |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Fecha Contable | 6 | Valor alfanumérico <br /> Se utiliza sólo si es transacción de Debito <br /> Caja no debe formatear (ej: DDAAMM), simplemente debe transferir el valor al voucher (XX/XX/XX) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número de Cuenta | 19 | Valor alfanumérico <br /> Número de tarjeta enmascarado para incluir en el voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Abreviación de la tarjeta | 2 | Valor alfanumérico <br /> Valor a imprimir en el voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Fecha Transacción | 6 | Formato AAMMDD <br /> Valor a imprimir en el voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Hora Transacción | 6 | Formato HHMMSS <br /> Valor a imprimir en el voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Campo Impresión | 8192 | Campo depende si la caja requiere voucher formateado (máximo) <br /> En el comando 510 no se envía el voucher <br /> En el comando 550 se envía voucher siempre |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Transacción premiada | 1 | Valor numérico <br /> `1`: transacción premiada <br /> En este caso caja debe imprimir voucher PEL además del de venta <br /> `[vacío]`  : transacción sin premio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Tipo promoción | 1 | Valor numérico <br /> `1`: Entrega Pto. de Venta <br /> `2`: Entrega Diferida <br /> `3`: Devolución al Tarjeta Habiente |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código promoción | 8 | Valor alfanumérico <br /> Valor a imprimir en el voucher premiado |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Nombre promoción | 21 | Valor alfanumérico <br /> Valor a imprimir en el voucher de premio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa vale premio | 62 | Valor alfanumérico <br /> Valor a imprimir en el voucher de premio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Texto vale premio | 27 | Valor alfanumérico <br /> Valor a imprimir en el voucher de premio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag permite cuotas | 1 | Valor numérico <br /> Campo informativo de la configuración del comercio | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag de gracia | 1 | Valor numérico <br /> Campo informativo de la configuración del comercio | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag C2C | 1 | Valor numérico <br /> Campo informativo de la configuración del comercio | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag C3C | 1 | Valor numérico <br /> Campo informativo de la configuración del comercio | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag NCuotas | 1 | Valor numérico <br /> Campo informativo de la configuración del comercio | 0 
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag máximo de cuotas | 2 | Valor numérico <br /> Campo informativo de la configuración del comercio | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Tipo de menú | 2 | Valor alfanumérico <br /> Indicador del tipo de menú por el cual se realizó la transacción <br /> `CR` : CRÉDITO <br /> `DB` : DÉBITO PREPAGO <br /> `NB` : NO BANCARIA <br /> Valor de tipo en Tabla tipo de tarjeta <br /> Una venta hecha como débito puede ser autorizada como prepago |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Indicador transacción con gracia | 1 | Valor numérico <br /> Indicador de modalidad de la transacción <br /> `0` transacción sin mes gracia <br /> `1` transacción con mes gracia |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Tipo cuotas | 1 | Valor numérico <br /> `0` Sin cuotas <br /> `1` Cuotas normales <br /> `3` C3C o C2C <br /> `4` CIC o N-cuotas |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Tasa aplicada | 4 | Valor numérico <br /> Solo se imprime en voucher si “Flag imprimir tasa = 1” |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa tipo cuota | 30 | Valor alfanumérico <br /> Glosa a imprimir en voucher <br /> Si el campo informado viene vacío ` `  caja debe omitir la línea en el voucher. |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa tipo cuota 2 | 22 | Valor alfanumérico <br /> Glosa que se despliega en pinpad |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa promoción | 10 | Valor alfanumérico <br /> Glosa que se despliega en pinpad |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Id promoción | 10 | Valor alfanumérico <br /> Glosa que se despliega en pinpad |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag imprimir tasa | 1 | Valor numérico <br /> `[vacío]` o `0` no imprime tasa aplicada <br /> `1` imprime tasa aplicada | 0
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Periodo diferido | 3 | Valor numérico <br /> Periodo diferido seleccionado, valor a imprimir en voucher |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 1 periodo | 3 | Valor numérico <br /> No utilizado |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 1 valor tasa | 4 | Valor numérico <br /> No utilizado |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 1 valor cuota | 14 | Valor numérico <br /> No utilizado |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 2 periodo | 3 | Valor numérico <br /> No utilizado |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 2 valor tasa | 4 | Valor numérico <br /> No utilizado |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 2 valor cuota | 14 | Valor numérico <br /> No utilizado |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 3 periodo | 3 | Valor numérico <br /> No utilizado |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 3 valor tasa | 4 | Valor numérico <br /> No utilizado |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Diferido 3 valor cuota | 14 | Valor numérico <br /> No utilizado |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número de secuencia transacción original | 9 | Valor alfanumérico (máximo) <br /> También conocido como número de operación original de la venta, No se está usando este campo, no se imprime |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código respuesta Transbank | 3 | Valor numérico <br /> Código de respuesta una vez finalizada la transacción. Se debe desplegar en el punto de venta. <br /> EJ: RESPUESTA TRANSBANK - <XXX> : <GLOSA> |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa respuesta Transbank | 48 | Valor alfanumérico (máximo) <br /> Glosa que despliega el pinpad una vez finalizada la transacción. Se debe desplegar en el punto de venta. <br /> EJ: RESPUESTA TRANSBANK - <XXX> : <GLOSA> |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag transacción con PIN | 1 | Valor alfanumérico <br /> Y: Transacción autentificada con PIN <br /> N: Transacción autentificada por firma |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Nombre tarjetahabiente | 26 | Valor alfanumérico <br /> Sólo imprimir si “Flag tipo voucher = 1, 2 ó 3” |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag tipo voucher | 1 | Valor numérico <br /> Según el número recibido se debe imprimir voucher con o sin firma: <br /> `0` = Sin firma <br /> `1` o `2` o `3` = con firma <br />  <br /> Cabeceras de los voucher: <br /> Para ventas con crédito: <br /> “VENTA CREDITO” <br /> Para ventas con débito (siempre sin firma): <br /> “VENTA DEBITO” <br /> Para ventas con no bancaria: <br /> “VENTA NO BANCARIA” <br /> Para ventas con prepago (sin firma): <br /> “VENTA PREPAGO” <br /> Para anulaciones con crédito (sin firma): <br /> “ANULACION CREDITO” <br /> Para anulaciones con no bancaria (sin firma): <br /> “ANULACION NO BANCARIA” |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag modalidad cuotas | 1 | Valor alfanumérico <br /> `0`: Modalidad 3.1 (No utilizado) <br /> 1: Modalidad cuotas 4.0 | 1
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa transacción afecta a ahorro | 40 | Valor alfanumérico (máximo) <br /> Se debe imprimir en el voucher cuando sea distinta de vacío <br /> Campo 9, subcampo D |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número de secuencia | 9 | Valor numérico <br /> No se está usando este campo, este no se imprime <br /> También conocido como número de operación |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Flag mensaje terminal | 1 | Valor alfanumérico <br /> Y: El mensaje es terminal y NO se debe enviar el mensaje SPDH de respuesta <br /> N: Se debe enviar el mensaje SPDH de respuesta |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Largo mensaje | 4 | Valor numérico |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Mensaje SPDH Venta/Reversa | 2048 | Valor alfanumérico (máximo) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Propina | 18 | Valor numérico <br /> Monto Propina o Donación |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Voucher de Rechazo | 1024 | Valor numérico <br /> Cuando transacción es declinada por EMV se debe imprimir un voucher especial. <br /> Si este campo viene vacío no se imprime, si viene con dato se imprime <br /> Este voucher se imprime solo, sin voucher de venta |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Voucher PEL | 1024 | Valor alfanumérico <br /> Voucher de PEL si viene la caja debe imprimirlo, solo una vez junto al voucher de venta <br /> No se debe imprimir en duplicado <br /> Este voucher se imprime junto al de venta |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
LABEL - EMV | 32 | Valor alfanumérico <br /> Si el campo viene con datos caja debe incluirlo en el voucher en la posición indicada |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
RID - EMV | 32 | Valor alfanumérico <br /> Si el campo viene con datos caja debe incluirlo en el voucher en la posición indicada |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Modelo pinpad | 6 | Valor numérico <br /> Caja debe incluirlo en el voucher ejemplo: <br /> VX805 <br /> Campo obligatorio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Versión de pinpad | 6 | Valor numérico <br /> Caja debe incluirlo en el voucher ejemplo: <br /> 12.34A <br /> Campo obligatorio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Saldo Prepago | 40 | Valor alfanumérico (máximo) <br /> Indica el saldo de una tarjeta de prepago la cual se debe imprimir en voucher cuando es venta de prepago y cuando viene el saldo. <br /> Nota: El Pinpad agrega esa glosa al voucher tal como viene en la mensajería. |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
`<ETX>` | 1 | Indica Fin de comando Valor Hexa 0x03 | ETX |
`<LRC>` | 1 | Byte resultado de la operación XOR del mensaje | 

### 0600 - Solicitud comando Cierre batch
Igual al Retail estándar vx805

### 0700 - Validación comando Cierre batch

Igual al Retail estándar vx805

### 1100 - Comando despliegue mensaje

Con este comando la caja puede mostrar en pantalla del pinpad algunos mensajes predefinidos.

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) <br> Indicando el largo del mensaje, sin incluir estos 4 bytes |
Comando | 4 | Valor 1100 |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código mensaje | 4 | Valor Numérico Revisar: Tabla de código de mensajes |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Timeout mensaje | 2 | Valor Numérico (00 -> 09) |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` | | 

**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes |
Comando | 4 | Valor 1110 |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código respuesta | 2 | Valor numérico  De acuerdo a Tabla de códigos de respuesta de comandos |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |


### CONN - Comando Conexión PINPAD – Caja
Comando enviado desde el PINPAD a la Caja de modo de establecer la conexión. La caja debe abrir un puerto de conexión el cual no debe ser cerrado en ningún momento.w

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes |
Comando | 4 | Valor ‘CONN’ |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número Serial | 15 | Valor alfanumérico Número serial del PINPAD |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Data | 20 | Valor alfanumérico (TRANSBANK VER. 4.01A) Identificador de la aplicación más la versión de la aplicación | Obligatorio 
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘CONN’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código Retorno | 2 | Valor numérico (00: Prompt de inicio) (01: Glosa de rechazo) | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número de Líneas | 2 | Valor numérico (00 -> 99) Cantidad de líneas de glosas a desplegar. | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa | 16 | Valor alfanumérico Glosa descripción de error. | Opcional | 
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |


### ECHO - Comando Echo Caja – PINPAD
Comando que permite a la Caja verificar que el PINPAD se encuentra conectado, identificarlo por su número de serie y versión de aplicación.

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘ECHO’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |


**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘ECHO’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código Retorno | 2 | Valor numérico (00: OK, Prompt de inicio) (01: NOK, Glosa de rechazo) | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Número Serial | 15 | Valor alfanumérico Número serial del PINPAD | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Data | 20 | Valor alfanumérico (TRANSBANK VER. 4.01A) Identificador de la aplicación más la versión de la aplicación | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

### ISES - Comando Inicio de Sesión Caja- PINPAD
Este comando permite abrir una sesión con el PINPAD desde la Caja.

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘ISES’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |


**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘ISES’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código Retorno | 2 | Valor numérico (00: OK) (Otro: Error) | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Batería | 3 | Valor numérico (000 -> 100) Porcentaje de carga de batería. | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |


### FSES - Comando Fin de Sesión Caja – PINPAD
Este comando permite cerrar una sesión con el PINPAD desde la Caja.

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘FSES’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |


**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘FSES’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` | 
Código Retorno | 2 | Valor numérico (00: OK) (Otro: Error) | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

### VOUC - Comando Solicitud Impresión Voucher Caja – PINPAD

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio |
Comando | 4 | Valor ‘VOUC’ | Obligatorio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Time Out | 5 | Valor numérico (00000 -> 99999) Timeout del comando en milisegundos. | Obligatorio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa 1 | 16 | Valor alfanumérico Glosa a desplegar primera línea en pantalla. | Obligatorio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Glosa 2 | 16 | Valor alfanumérico Glosa a desplegar segunda línea en pantalla. | Obligatorio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Voucher a Imprimir | 4000 | Valor alfanumérico Voucher separado por “\n” para nueva línea, “\c” para corte de voucher | Obligatorio |
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘VOUC’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` | 
Código Retorno | 2 | Valor numérico (00: Impresión OK) (01: Error en la impresión) | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |



### REIM - Comando Solicitud Reimpresión Último Voucher PINPAD – Caja
Comando enviado desde el PINPAD a la Caja de modo de que la caja envíe al PINPAD el Voucher de la última venta. Para enviar este comando se debe presionar en el PINPAD ` [ENTER] + [5]`

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘REIM’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |



**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘REIM’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` | 
Código Retorno | 2 | Valor numérico (00: Recibido OK) (01: No existe impresión disponible) | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` | 
Time Out | 5 | Valor numérico (00000 -> 99999) Timeout del comando en milisegundos. | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` | 
Glosa 1 | 16 | Valor alfanumérico Glosa a desplegar primera línea en pantalla. | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` | 
Glosa 2 | 16 | Valor alfanumérico Glosa a desplegar segunda línea en pantalla. | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` | 
Voucher a Imprimir | 4000 | Valor alfanumérico Voucher separado por `\n` para nueva línea, `\c` para corte de voucher | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |


### REST - Comando Reset Socket Caja – PINPAD
Comando permite a la Caja realizar un reset al socket del PINPAD.

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘REST’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘REST’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código Retorno | 2 | Valor numérico (00: Recibido OK) (01: Error Mensaje) | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

### LKEY - Comando Carga de Llave PINPAD – Caja
Comando ejecutado a través del menú del PINPAD, el cual permite solicitar a la caja una ‘Carga de llaves’ a la Caja. La transacción de carga de llaves debe ser construida tal como lo define el manual de “Especificaciones Técnicas”.

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘LKEY’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘LKEY’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código Retorno | 2 | Valor numérico (00: Recibido OK) (01: Error en carga de llaves) | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

### 0000 - Comando TEST PINPAD – Caja
Comando enviado desde el PINPAD hacia la Caja para mantener el socket abierto.

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
Comando | 4 | Valor ‘0000’ | Obligatorio

**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
Comando | 4 | Valor ‘0000’ | Obligatorio

### CLSB - Comando Cierre Batch PINPAD – Caja
Comando ejecutado a través del menú del PINPAD, el cual permite indicar a la caja el envío de una solicitud de ‘Cierre batch’.

**Requerimiento**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘CLSB’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

**Respuesta**

DATO | LARGO | COMENTARIO | VALOR POR DEFECTO
---- | ----- | ---------- | ------------------
LARGO_DATA | 4 | Valor numérico (0000 -> 9999) Indicando el largo del mensaje, sin incluir estos 4 bytes | Obligatorio
Comando | 4 | Valor ‘CLSB’ | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |
Código Retorno | 2 | Valor numérico (00: Recibido OK) (Otro: Error) | Obligatorio
`Separador` | 1         | <i>Valor ASCII</i>: <code>&#124;</code> <br><i>Valor hexadecimal</i>: `0x7c` |

## Otros datos

### Tabla de marcas de tajetas

Nombre | Abreviación
---- | -----
VISA | VI
MASTERCARD | MC
DINERS | DC
AMEX | AX
MAGNA | MG
PRESTO | TP
MAS | TM
CMR | TC
RIPLEY | TR
MAESTRO | MT
ELECTRON | EL
DEBITO | DB

### Tabla de tipo de tarjeta

Código | Glosa
---- | -----
CR | CREDITO
DB | DEBITO
NB | NO BANCARIA
Null (vacío) | Se despliega Menú en PINPAD.

### Tabla de códigos de respuesta de comandos

Código de respuesta | Glosa
---- | -----
00 | RESPUESTA OK
84 | NO EXISTE CODIGO DE MENSAJE
86 | ERROR DE LECTURA.
87 | PINPAD SIN MASTER KEY
89 | TRANSACCIÓN DECLINADA POR LA TARJETA CHIP
90 | TARJETA NO PERMITIDA PARA EL MODO SELECCIONADO
91 | ERROR CANTIDAD DE CUOTAS
92 | NO COINCIDE CON TARJETA DE PRIMER “TAPEO”
93 | ERROR DE MONTO MÍNIMO
94 | ERROR DE VALIDACIÓN MONTO VUELTO
95 | ERROR ID DE CONTEXTO
96 | NO COINCIDE LOS 4 ULTIMOS DIGITOS
97 | LA TRANSACCIÓN NO PERMITE REVERSA
98 | ERROR DE FORMATO DEL MENSAJE
99 | CANCELACIÓN DEL COMANDO A TRAVÉS DE LA TECLA `[CANCEL]` / TIMEOUT

### Tabla de códigos de mensajes de despliegue en PINPAD

Código de respuesta | Glosa
---- | -----
0000 | APROBADO
0001 | ERROR INTERNO DE MENSAJERIA
0002 | ERROR INTERNO DE MENSAJERIA
0003 | NIVEL DE BATERIA BAJO
0004 | CANCELACION DE OPERACIÓN
0005 | EMISOR NO DISPONIBLE
0006 | TERMINAL NO DISPONIBLE
0007 | NO HAY CONFIRMACION
0008 | ERROR EN OPERACIÓN CON POS
0009 | MAXIMO INTENTOS SUPERADOS CONEXIÓN SWITCH SERVER
0010 | CANCELACION DE OPERACIÓN
0011 | MEDIOS DE PAGO NO DISPONIBLES
0014 | ERROR INTERNO DE SISTEMA
0015 | ERROR INTERNO DE SISTEMA
0016 | ERROR INTERNO DE MENSAJERIA
0017 | ERROR INTERNO DE MENSAJERIA
0018 | TRANSACCION DE VENTA INEXISTENTE
0019 | TRANSACCION ORIGINAL NO ES UNA VENTA
0020 | TRANSACCION ORIGINAL NO TERMINADA
0021 | TRANSACCION PENDIENTE EMISOR
0022 | MONTO A ANULAR MAYOR AL MONTO DE VENTA
0023 | MONTO A ANULAR DISTINTO AL MONTO DE VENTA 
0024 | TARJETA NO PERMITIDA
0025 | ERROR RESPUESTA TERMINAL
0026 | ERROR RESPUESTA TERMINAL 
