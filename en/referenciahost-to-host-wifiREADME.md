# Host to Host Pinpad Wifi

### Messaging protocol

** TLS 1.2 security **

SSL (Secure Sockets Layer) or Secure Sockets Layer. It is a protocol that makes use of certificates
to establish secure communications over the network. Since 2015 it has been replaced by
TLS (Transport Layer Security) which is based on SSL and is fully supported.

** Security in the WI-FI Network of Commerce **

In the event that the Commerce network requires the use of WI-FI connections, it is required that the network be a network
encrypted * WPA2-PSK (AES).

** Security recommendations for a WI-FI network: **
Change WI-FI network password regularly

1. By regularly changing the WI-FI network password, you prevent third parties from using the business network.
2. Configure the WI-FI network as "Not visible"
   By hiding the WI-FI network, it is prevented that people outside the business can find and
   try to access the commerce network.
   Now every time you try to connect a new device, it will be necessary to place
   first the * SSID, then enter the password
3. Register and restrict connections by MAC
   By having MAC connections enabled, it is specified which equipment can do
   use of the WI-FI network, preventing any other equipment from using the business network.
4. Restrict access to "Router Configuration" from WI-FI
   By restricting access to the Router's configuration from WI-FI connections, it is avoided that
   from WIFI devices you can access this configuration and modify its
   parameters.
5. Regularly monitor WI-FI connections
   The current routers allow to know the devices that are connected to the WIFI network. It is advisable to monitor in order to avoid any device that is connected
   without authorization.

* WPA2-PSK (AES): Protection system for WI-FI wireless networks. It is the latest WI-FI encryption standard and AES is the
Latest encryption algorithm.

* SSID: Broadcast a network SSID. An SSID is the public name of a wireless local area network (WLAN) that is used to
differentiate it from other wireless networks in the area. SSID is the network name that is specified when setting up the Wi-Fi network.

### TLS implementation file
The implementation of TLS carried out is that of two-way mutual authentication, that is, the PinPad validates the
box and vice versa.
To implement TLS between the Pinpad vx680 and the retail box, the following are required
records:

** Pinpad TLS certificates: **
1. Client certificate: "client.PEM": Certificate with which it is presented to the Server.
2. Client certificate validation key: "client.key": Necessary to validate that the uploaded certificate is the correct one.
3. Certificate to validate the server: "CA.PEM": To validate the certificate of the Server.

** TLS Server Certificates (Caja del Comercio): **
1. Server certificate: "server.PEM": Certificate with which it is presented to the client.
2. Server certificate validation key: "server.key": Private key used to sign.
3. Certificate to validate the client: "CAClient.PEM": To validate the client's certificate.

### TLS implementation
The communication channel must be encrypted with the latest version of the TLS communication protocol.
Double authentication (Two-way TLS) should be used to give greater security to the process.

! [TLS implementation] (/ images / documentation / host2host / implementation-tls.png)

The client has the CA of the server's certificate in its truststore.
During the TLS handshake, the client compares the certified CA in its truststore with the certificate being sent from the server to verify the identity of the server.
The server has the CA of the client's certificate in its truststore.
During the TLS handshake, the server compares the CA in its truststore with sending the client's certificate to verify the client's identity.

Depending on the language or platform, the location of the "Certificate Stores" (KeyStore and TrustStore) must be specified.
Depending on the language or platform, a KeyStore is created for the certificate and the private key of the Server and a TrustStore for the "CA Certificate" of the Pinpad.

java
// We specify the KeyStore that contains the Server certificate and the private key
KeyStore keyStore = KeyStore.getInstance ("JKS");
keyStore.load (new FileInputStream ("server / serverKeyStore.jks"), "servpass" .toCharArray ());

KeyManagerFactory kmf = KeyManagerFactory.getInstance (KeyManagerFactory.getDefaultAlgorithm ());
kmf.init (keyStore, "servpass" .toCharArray ());

// We specify the TrustStore that contains the client's CA Certificate
KeyStore trustedStore = KeyStore.getInstance ("JKS");
trustedStore.load (new FileInputStream ("server / serverTrustedCerts.jks"), "servpass" .toCharArray ());

TrustManagerFactory tmf = TrustManagerFactory.getInstance (TrustManagerFactory.getDefaultAlgorithm ());
tmf.init (trustedStore);

// We create the TLS instance
SSLContext sc = SSLContext.getInstance ("TLSv1.2");
TrustManager [] trustManagers = tmf.getTrustManagers ();
KeyManager [] keyManagers = kmf.getKeyManagers ();
sc.init (keyManagers, trustManagers, null);

// the socket that will be used for communication is created
SSLServerSocketFactory ssf = sc.getServerSocketFactory ();
serverSocket = (SSLServerSocket) ssf.createServerSocket (port);

try {
   // We wait for the PINPAD to connect
   Socket aClient = serverSocket.accept ();
   System.out.println ("client accepted");
   aClient.setSoLinger (true, 1000);
   aClient.setSoTimeout (10 * 1000);

   // We read what was sent from the PINPAD
   BufferedReader input = new BufferedReader (new InputStreamReader (aClient.getInputStream ()));
   String received = input.readLine ();
   System.out.println ("Received" + received);

   // We send ACK to the Pinpad
   PrintWriter output = new PrintWriter (aClient.getOutputStream ());
   output.println ("0004ACK");
   output.flush ();

   // We send Command to Pinpad
   output.println ("0025CONN | 00 | 01 | Text line 1 |");
   output.flush ();

} catch (Exception e) {
   e.printStackTrace ();
}
`` ''

### TCP / IP communication protocol
This protocol allows communication between an ECR (Electronic Cash Register) and a
PINPAD over a network that supports the TCP / IP protocol. It is important to keep in
Consideration of error handling over TCP / IP functions to ensure that each
one of the messages reaches the recipient and manage retries when
necessary. The following diagram exemplifies the construction of the commands in the protocol
TCP / IP.

! [TCP / IP protocol] (/ images / documentation / host2host / protocol-tcp.png)

Where:
* LONG_DATA: 4 bytes indicating the length of the message, not including these 4 bytes.
* DATA: It is the message that you want to send.

** Generic script diagram **

The diagram described corresponds to the generalization of the behavior of each
one of the commands.

! [TCP / IP Protocol Box to Pinpad] (/ images / documentation / host2host / stream-stream-wifi.png)

! [TCP / IP PinPad to Box Protocol] (/ images / documentation / host2host / stream-stream-wifi-pinpad-box.png)

** Detailed sales flow **

! [Detailed sale sequence] (/ images / documentation / host2host / detailed-sequence-sale-wifi.png)

**Diagram**

! [Detailed sale diagram] (/ images / documentation / host2host / detailed-sale-flow-diagram.png)


### Sale example:
`` ''
0100 | 00 | N | N | N | 12100 | CL | CR || 0 | 0 |
0110 | 00 | 2017111611350940 | 01 ||||| 5197 || MASTERCARD | MC | N | 4
0200 | 12100 | 0 || 0 | 0 | 2017111611350940 | 00 | 597044440001 | S4HOST2HOST3DES1 |||| 17111611361100
000000000754 || 0123456789ABCDEF ||||| 5197 | v
0210 | 00 | 2017111611350940 | 0737 | 9.12S4HOST2HOST3DES1
171116113517FO00050000B000000000000012100P1Q0123456789ABCDEFa000000000000000000000000
00NU1711161136110000000000075400000000000000000CL0000000d597044440001e00h0010050081G3
F3308F4S0t74 0000000000000000326-478-322 15.30C
6-E051-I152-O0180152171116B8738FEAC1697887380000669B8C9262000000800000152000000012100
0000000000000014A78003040000716800000000000000FF-P0100224403020002
E0F8C826478322RA00000000410109-A1EL20212223242526272829VI0 6MC0 67DC0
6AX012345OTTP06TR01TE0 TM0 TC12TD12TJ12TH12T812T90
-B41205240-C2100-P000000000000-I0-J0-K000W0161111005CR -
4F552D8E65F6056E543A481CDD07D2525E2D7347C32D2CA5756F176482684949FD0443BCB1235018CC0CD
DC7C0EA41BF | (
0500 | 2017111611350940 | 513 | 9.12S4HOST2HOST3DES1
171116113521FO00000005B000000000000012100D4EF600979
BGA1B5296BH0D9C83C5A59B574F7AF35145C606D5D4ID5879B9816C5A304236AB90B081A60A0P1Q012345
6789ABCDEFS0
T0000000000W0161111005CRCMC0000a00000000000003000000000000NU1711161136110000000000075
400000000000000000CL0000000d597044440001e00g
APPROVEDh0010050081ptS4HOST2HOST3DES16-E051-I1529-A1EL20212223242526272829VI0 6MC0
67DC0 6AX012345OTTP06TR01TE0 TM0 TC12TD12TJ12TH12T812T90
-B11205243-C0000-P100000800000 |
0510 | 00 | 2017111611350940 | 597044440001 | S4HOST2HOST3DES1 | 0 | 0000 | 600979
B | 12100 || 00 || 5197 | 001005008 | CREDIT || ************ 5197 | MC | 171116 | 113521 |||||||| 1 | 1 | 1 | 1
| 0 | 05 | CR | 0 | 0 | 0000 | NO FEES ||||||| 0000 ||| 0000 ||| 0000 ||| 005 |
APPROVED | N || 0 | 1 || 001005008 | Y |||
`` ''

** NOTE: ** If for some reason the pinpad responds to a command in a format that does not
corresponds (wrong data type, wrong length, wrong or missing separator,
etc.) the cash should request a reversal according to the “reverse execution flow to
box request ”detailed below. Subsequently, a
new sales flow.

### Reverse execution flow at cash request
The box had no response from any command or had no response from an SPDH message in
flight, therefore he could not finish a sale that he started, then he does not know if it is approved or
rejected, in this case you must request a reverse to the pinpad.

! [Reverse flow] (/ images / documentation / host2host / reverse-box.png)

** Example of reverse: **
`` ''
0400 | 2019062813081650 |
0410 | 00 | 2019062813081650 | 0643 | 9.07S4CAJAHOST000010
190628130853FT00000000B000000000000650000P1Q0123456789ABCDEFa000000150000000000000000
00NU2019062813091100100100000100000000000000000CL0000000d597044440001e00h0010030051G3
F27A500S0t74 0000000000000000326-018-973 18.21P
6-E071-I152-O0180152190628236E485C1DC23FC81980067116EE3A5C000000800000152000000650000
0000000000000110A04001220000000000000000000000FF-P0101221F03020002
00080826018973A00000000410109-A1EL20252627 VI123456MC123456DC
AX123456OTTP06TR1 TE0 TM0 TC12TD12TJ12TH12T812T90
-B41205240-C2100-P000000000000-I0-J1-K000-M1W0161111005CR 0000 |
0500 | 2019062813081650 | 643 | 9.07S4CAJAHOST000010
190628130853FT00000000B000000000000650000P1Q0123456789ABCDEFa000000150000000000000000
00NU2019062813091100100100000100000000000000000CL0000000d597044440001e00h0010030051G3
F27A500S0t74 0000000000000000326-018-973 18.21P
6-E071-I152-O0180152190628236E485C1DC23FC81980067116EE3A5C000000800000152000000650000
0000000000000110A04001220000000000000000000000FF-P0101221F03020002
00080826018973A00000000410109-A1EL20252627 AX123456OTTP06TR1 TE0 TM0 TC12TD12TJ12TH12T812T90
-B41205240-C2100-P000000000000-I0-J1-K000-M1W0161111005CR 0000 |
0510 | 00 | 2019062813081650 | 597044440001 | S4CAJAHOST000010 ||| 265404
B | 650000 || 00 || 0003 | 001003005 | CREDIT || ************ 0003 |
| 190628 | 130853 |||||||| 1 | 1 | 1 | 1 | 0 | 05 | CR | 0 | 0 | 0000 | SIN
FEES ||||||| 0000 ||| 0000 ||| 0000 ||| 000 | REVERSE APPLIED | N || 0 | 1 || 001003005 | Y |||
`` ''

** Initialization diagram and connection to PINPAD **

! [Initialization and connection diagram] (/ images / documentation / host2host / initialization-and-connection-diagram.png)

** Command execution flow to PINPAD **

! [Command execution flow] (/ images / documentation / host2host / command-flow-pinpad-wifi.png)

** Context management **

! [Command execution flow] (/ images / documentation / host2host / context-handling.png)

## Command Description

### 0100 - Read card command
Same as Retail vx805 standard

### 0200 - Sales request / cancellation command
Same as Retail vx805 standard

###  0400 - Reverse request command
Same as Retail vx805 standard

###  0500 - Validation / update request command
Same as Retail vx805 standard

### 0520 - Transaction response validation request command
This command is a counterpart to 500, where box delivers the spdh message to the pinpad, but
the pinpad when the transaction is approved hides the message on the screen, if the
transaction is rejected behaves the same as command 510.
At the end of the transaction, cash must send command 1100 to the pinpad with the message that is
Requires display on screen (Approved).

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | 
Command | 4 | Value 0520 | 
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` | 
Context indicator | 16  | Alphanumeric value Format yyyymmddhhmmssmm |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Long message | 4 | Numerical value |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
SPDH message | 2048 | Alphanumeric value (maximum) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

Note: It does not display a gloss on the PINPAD screen or update parameters

**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes
Command | 4 | Value 0530 |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Response Code | 2 | Numeric value According to Table of command response codes  |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Context indicator | 16 | Alphanumeric value Format yyyymmddhhmmssmm |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Commercial Code | 12 | Alphanumeric value |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Terminal ID | 16 | Alphanumeric value <br > Field "t" message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Ticket / Ballot Number | 20 | Alphanumeric value <br> Field "S" message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Employee | 4 | Alphanumeric value <br> Field "T" message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Authorization Code | 8 | Alphanumeric value (maximum) <br> Field "F" message SPDH
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Amount | 18 | Numeric value (maximum) <br> Field "B" message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Amount returned | 18 | Numeric value (maximum) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Number of Quotas | 2 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Amount Fee | 14 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Last 4 Card Digits | 4 | Numerical value |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Operation Number | 6 | Terminal transaction correlation (maximum)
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Glosa Card Type | 7 | Alphanumeric value (maximum) <br> Gloss value in Table type of agreement card "Type of card" of field "W" message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Accounting date | 6 | Alphanumeric value <br> It is used only if it is a Debit transaction From field “to” SPDH message |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Account number | 19 | Alphanumeric value <br> Masked card number to include in the voucher Field "E" message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Card abbreviation | 2 | Alphanumeric value <br> From field "W" message SPDH
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Transaction Date | 6 | Format YYMMDD of the SPDH message header |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Transaction Time | 6 | HHMMSS format of the SPDH message header |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Print field | 8192 | Field depends if the box requires formatted voucher (maximum) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Award-winning transaction | 1 | Numeric value <br> Position 1, field “f” message SPDH <br> (1: Delivery Point of Sale) <br> (2: Deferred Delivery) | (3: Return to Cardholder)
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Promotion type | 1 | Numeric value <br> Position 2, field “f” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Promo code | 8 | Alphanumeric value <br> Position 3-10, field “f” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Promotion name | 21 | Alphanumeric value <br> Position 11-31, field “f” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Glosa voucher award | 62 | Alphanumeric value <br> Position 32-93, field “f” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Prize voucher text | 27 | Alphanumeric value <br> Position 94-120, field “f” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag allows quotas | 1 | Numeric value <br> From field “W” message SPDH | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Grace flag | 1 | Numeric value <br> From field “W” message SPDH | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag C2C | 1 | Numeric value <br> From field “W” message SPDH | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag C3C | 1 | Numeric value <br> From field “W” message SPDH | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag NCuotas | 1 | Numeric value <br> From field “W” message SPDH | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Maximum quota flag | 2 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Menu type | 2 | Alphanumeric value <br> From field "W" message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Graceful transaction indicator | 1 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Type of installments | 1 | Numeric value <br> From field “W” message SPDH <br> (1: Normal) <br> (3: C3C or C2C) <br> (4: Cyc or N-installments) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Applied rate | 4 | Numeric value <br> From field "W" message SPDH <br> It is only printed in voucher if "Flag print rate = 1" |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Fee-type gloss | 30 | Alphanumeric value <br> From field "W" message SPDH <br> (Gloss to be printed on voucher) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Gloss type fee 2 | 22 | Alphanumeric value <br> From field “W” message SPDH <br> (Gloss that is displayed in PINPAD) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Promotion gloss | 10 | Alphanumeric value <br> From field "W" message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Id promotion | 8 | Alphanumeric value <br> From field "W" message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag print rate | 1 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred period | 3 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 1 period | 3 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 1 rate value | 4 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 1 installment value | 14 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 2 period | 3 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred rate 2 value | 4 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 2 installment value | 14 | "Numeric value <br> From field" W "SPDH message" |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 3 period | 3 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 3 value rate | 4 | Numeric value <br> From field “W” message SPDH|
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 3 installment value | 14 | Numeric value <br> From field “W” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Original transaction sequence number | 9 | Alphanumeric value (maximum) <br> Field “i”. Only for cancellations |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Transbank response code | 3 | Numerical value |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Transbank response gloss | 48 | Alphanumeric value (maximum) <br> Field “g” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag transaction with PIN | 1 | Alphanumeric value <br> (Y: Transaction authenticated with PIN) <br> (N: Transaction authenticated by signature) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Cardholder name | 26 | Alphanumeric value <br> Only if "Transaction flag with PIN = N" |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Voucher type flag | 1 | Numeric value <br> (0: Sale with PIN, without signature, without gloss) <br> (1: Regular sale, with signature, with gloss) <br> (2: Sale with PIN, with signature, without gloss) <br> (3: normal sale, with signature, without gloss) <br> Position 8, subfield “B” field “9” message SPDH |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Quota mode flag | 1 | Alphanumeric value <br> (0: Mode 3.1) <br> (1: New quotas mode) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Glossy transaction affects savings | 40 | Alphanumeric value (maximum) <br> (Must be printed on the voucher when applicable) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Sequence number | 9 | Numerical value |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Terminal message flag | 1 | Alphanumeric value <br> (Y: The message is terminal and the response SPDH message must NOT be sent) <br> (N: The response SPDH message must be sent) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Long message | 4 | Numerical value |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
SPDH Sell / Reverse Message | 2048 | Alphanumeric value (maximum) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

### 0560 - Transaction response validation request command
This command is a counterpart to the 540, where the box delivers the spdh message to the pinpad.
The difference is that the behavior defined for the pinpad with this command is to ignore the approved gloss on the screen.

The gloss that indicates the approval of the transaction will only be shown on the screen when the pinpad receives the command 1100 from the box, this command must include the code of the message that is required to be displayed, in this case Approved.

When the box requires to finalize the approval of the transaction, once it has received the 570 command with the terminal flag set to Y, it must send the 1100 command with the message code to be displayed on the pinpad.

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
`` <STX> '' | 1 | Indicates command start Hexa Value 0x02 | STX
Command | 4 | Value 0560 | 560
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Context indicator | 16 | Alphanumeric value <br> Id delivered by the pinpad for each transaction |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Long message | 4 | Numerical value |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
SPDH message | 2048 | Alphanumeric value (maximum) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Trade Name | 40 | Alphanumeric value <br> Boxed parametric field sent to pinpad |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Trade Directorate | 40 | Alphanumeric value <br> Boxed parametric field sent to pinpad |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Commune Commerce | 40 | Alphanumeric value <br> Boxed parametric field sent to pinpad <br> It can be commune or city |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
`<ETX>` | 1 | Indicates End of command Hexa Value 0x03 | ETX
`<LRC>` | 1 | Byte result of the message XOR operation |

Timeout per command 560 of 125sec, since there are up to 4 interaction with the user

**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
 <STX> | 1 | Indicates command start Hexa Value 0x02 | STX
Command | 4 | Value 0570 | 570
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
PinPad Response Code | 2 | "Numeric value <br> In case of rejection, it must be displayed at the point of sale: <br /> PINPAD REJECTION - <XX>: <GLOSA> <br /> According to the command code table" |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Context indicator | 16 | Alphanumeric value <br /> Id delivered by the pinpad for each transaction |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Commercial Code | 12 | Numerical value <br /> Trade code delivered by TBK and configured in the box, it is printed on the voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Terminal ID | 16 | Alphanumeric Value <br /> Logical address delivered by TBK and configured in the box, it is printed on the voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Ticket / Ballot Number | 20 | Alphanumeric value <br /> Optional field, if it comes it is printed on the voucher if it does not come the field is omitted |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Employee | 4 | Alphanumeric value <br /> Optional field, if it comes it is printed on the voucher if it does not come the field is omitted |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Authorization Code | 8 | Alphanumeric value (maximum) <br /> Transaction authorization code sent by TBK example: `[AB 12 C3]` <br /> What comes in the voucher is printed |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Amount | 18 | Numeric value (maximum) <br /> Total authorized amount (includes the amount of the sale, tip, return and donation as the case may be) <br /> It is printed on voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Amount returned | 18 | Numeric value (maximum) <br /> Return selected by customer, only applies in debit <br /> It is printed in voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Number of Quotas | 2 | Numeric value <br /> Amount of transaction fees (for sales without fees, “00” is reported) <br /> It is printed on the voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Amount Fee | 14 | Numeric value <br /> If the reported amount is empty `` or `0` box, you must omit the entire line in the voucher. <br /> It is printed on the voucher if the field comes |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Last 4 Card Digits | 4 | Numeric Value <br /> Does not print |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Operation Number | 9 | Terminal transaction correlative <br /> Also known as a sequence number, this field must be printed on the sales and cancellation voucher. |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Glosa Card Type | 7 | Alphanumeric value (maximum) <br /> Gloss value in Card type table |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Accounting date | 6 | Alphanumeric value <br /> It is used only if it is a Debit transaction <br /> Cash must not be formatted (ex: DDAAMM), it simply must transfer the value to the voucher (XX / XX / XX) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Account number | 19 | Alphanumeric value <br /> Masked card number to include in the voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Card abbreviation | 2 | Alphanumeric value <br /> Value to print on the voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Transaction Date | 6 | Format YYMMDD <br /> Value to print on the voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Transaction Time | 6 | Format HHMMSS <br /> Value to print in the voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Print field | 8192 | Field depends on whether the box requires formatted voucher (maximum) <br /> In command 510 the voucher is not sent <br /> In command 550 voucher is always sent |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Award-winning transaction | 1 | Numerical value <br /> `1`: winning transaction <br /> In this case cash register must print PEL voucher in addition to the sales voucher <br />` [empty] `: transaction without prize |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Promotion type | 1 | Numerical value <br /> `1`: Delivery Point of Sale <br />` 2`: Deferred Delivery <br /> `3`: Return to Cardholder |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Promo code | 8 | Alphanumeric value <br /> Value to print on the awarded voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Promotion name | 21 | Alphanumeric value <br /> Value to print on the award voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Glosa voucher award | 62 | Alphanumeric value <br /> Value to print on the award voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Prize voucher text | 27 | Alphanumeric value <br /> Value to print on the award voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag allows quotas | 1 | Numeric value <br /> Informational field of the merchant configuration | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Grace flag | 1 | Numeric value <br /> Informational field of the merchant configuration | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag C2C | 1 | Numeric value <br /> Informational field of the merchant configuration | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag C3C | 1 | Numeric value <br /> Informational field of the merchant configuration | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag NCuotas | 1 | Numeric value <br /> Informational field of the merchant configuration | 0 
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Maximum quota flag | 2 | Numeric value <br /> Informational field of the merchant configuration | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Menu type | 2 | Alphanumeric value <br /> Indicator of the type of menu for which the transaction was carried out <br /> `CR`: CREDIT <br />` DB`: PREPAID DEBIT <br /> A05e44 debit card value to type05ac BAN05ac0AN05e44 BE05e44Be05e44Be05e44Be05e44e05e44 sale can be authorized as prepaid |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Graceful transaction indicator | 1 | Numeric value <br /> Transaction mode indicator <br /> `0` transaction without grace month <br />` 1` transaction with grace month |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Type of installments | 1 | Numeric value <br /> `0` No quotas <br />` 1` Normal quotas <br /> `3` C3C or C2C <br />` 4` CIC or N-quotas |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Applied rate | 4 | Numerical value <br /> It is only printed in the voucher if "Flag print rate = 1" |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Fee-type gloss | 30 | Alphanumeric value <br /> Gloss to print on voucher <br /> If the reported field is empty `` box must omit the line in the voucher. |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Gloss type fee 2 | 22 | Alphanumeric value <br /> Gloss that is displayed on pinpad |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Promotion gloss | 10 | Alphanumeric value <br /> Gloss that is displayed on pinpad |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Id promotion | 10 | Alphanumeric value <br /> Gloss that is displayed on pinpad |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag print rate | 1 | Numeric value <br /> `[empty]` or `0` does not print applied rate <br />` 1` prints applied rate | 0
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred period | 3 | Numeric value <br /> Selected deferred period, value to print on voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 1 period | 3 | Numeric value <br /> Not used |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 1 rate value | 4 | Numeric value <br /> Not used |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 1 installment value | 14 | Numeric value <br /> Not used |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 2 period | 3 | Numeric value <br /> Not used |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred rate 2 value | 4 | Numeric value <br /> Not used |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 2 installment value | 14 | Numeric value <br /> Not used |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 3 period | 3 | Numeric value <br /> Not used |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 3 value rate | 4 | Numeric value <br /> Not used |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Deferred 3 installment value | 14 | Numeric value <br /> Not used |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Original transaction sequence number | 9 | Alphanumeric value (maximum) <br /> Also known as the original transaction number of the sale, This field is not being used, it is not printed |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Transbank response code | 3 | Numeric value <br /> Response code after the transaction is completed. It must be displayed at the point of sale. <br /> EJ: TRANSBANK RESPONSE - <XXX>: <GLOSA> |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Transbank response gloss | 48 | Alphanumeric value (maximum) <br /> Gloss that displays the pinpad once the transaction is completed. It must be displayed at the point of sale. <br /> EJ: TRANSBANK RESPONSE - <XXX>: <GLOSA> |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Flag transaction with PIN | 1 | Alphanumeric value <br /> Y: Transaction authenticated with PIN <br /> N: Transaction authenticated by signature |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Cardholder name | 26 | Alphanumeric value <br /> Only print if "Flag type voucher = 1, 2 or 3" |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Voucher type flag | 1 | Numerical value <br /> Depending on the number received, a voucher must be printed with or without signature: <br /> `0` = Without signature <br />` 1` or `2` or` 3` = with signature <br />e0544e00a0e voucher with credit v0544e00a06 : <br /> "SALE CREDIT" <br /> For sales with debit cards (always unsigned): <br /> "SALE dEBIT" <br /> For sales with non-bank: <br /> "SALE nonbank" <br /> For sales prepaid (unsigned): <br /> "SALE pREPAID" <br /> For cancellations with credit (without signature): <br /> “CANCELLATION CREDIT” <br /> For cancellations with non-bank (without signature): <br /> “NON-BANK CANCELLATION” |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Quota mode flag | 1 | Alphanumeric value <br /> `0`: Mode 3.1 (Not used) <br /> 1: Quota mode 4.0 | 1
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Glossy transaction affects savings | 40 | Alphanumeric value (maximum) <br /> It must be printed on the voucher when it is different from empty <br /> Field 9, subfield D |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Sequence number | 9 | Numeric value <br /> This field is not being used, it is not printed <br /> Also known as operation number |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Terminal message flag | 1 | Alphanumeric value <br /> Y: The message is terminal and the response SPDH message should NOT be sent <br /> N: The response SPDH message should be sent |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Long message | 4 | Numerical value |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
SPDH Sell / Reverse Message | 2048 | Alphanumeric value (maximum) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Tip | 18 | Numerical value <br /> Tip or Donation Amount |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Rejection Voucher | 1024 | Numeric value <br /> When transaction is declined by EMV a special voucher must be printed. <br /> If this field is empty it is not printed, if it comes with data it is printed <br /> This voucher is printed alone, without a sales voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Voucher PEL | 1024 | Alphanumeric value <br /> PEL voucher if it comes with the box, you must print it, only once together with the sales voucher <br /> It must not be printed in duplicate <br /> This voucher is printed together with the sales voucher |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
LABEL - EMV | 32 | Alphanumeric value <br /> If the field comes with box data, it must be included in the voucher in the indicated position |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
RID - EMV | 32 | Alphanumeric value <br /> If the field comes with box data, it must be included in the voucher in the indicated position |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Pinpad model | 6 | Numeric value <br /> Box must be included in the voucher example: <br /> VX805 <br /> Mandatory field |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Pinpad version | 6 | Numeric value <br /> Box must be included in the voucher example: <br /> 12.34A <br /> Mandatory field |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Prepaid Balance | 40 | Alphanumeric value (maximum) <br /> Indicates the balance of a prepaid card which must be printed on a voucher when it is a prepaid sale and when the balance comes. <br /> Note: The Pinpad adds this gloss to the voucher as it comes in the message. |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
`<ETX>` | 1 | Indicates End of command Hexa Value 0x03 | ETX |
`<LRC>` | 1 | Byte result of the message XOR operation | 

### 0600 - Close batch command request
Same as Retail vx805 standard

### 0700 - Close batch command validation

Same as Retail vx805 standard

### 1100 - Command display message

With this command the box can show some predefined messages on the pinpad screen.

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) <br> Indicating the length of the message, not including these 4 bytes |
Command | 4 | Value 1100 |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Message code | 4 | Numeric Value Check: Message Code Table |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Message timeout | 2 | Numerical Value (00 -> 09) |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` | | 

**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes |
Command | 4 | Value 1110 |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Response code | 2 | Numeric value According to Table of command response codes |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |


### CONN - PINPAD Connection Command - Box
Command sent from the PINPAD to the Box in order to establish the connection. The box must open a connection port which must not be closed at any time.

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes |
Command | 4 | Value ‘CONN’ |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Serial Number | 15 | Alphanumeric value PINPAD serial number |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Data | 20 | Alphanumeric value (TRANSBANK VER. 4.01A) Identifier of the application plus the version of the application | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | Value ‘CONN’ | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Return Code | 2 | Numeric value (00: Start Prompt) (01: Reject Gloss) | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Number of lines | 2 | Numeric value (00 -> 99) Number of gloss lines to display. | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Gloss | 16 | Alphanumeric value Glossy error description. | Optional | 
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |


### ECHO - Echo Box Command - PINPAD
Command that allows the Cashier to verify that the PINPAD is connected, to identify it by its serial number and application version.

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | 'ECHO' value | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |


**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | 'ECHO' value | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Return Code | 2 | Numeric value (00: OK, Start Prompt) (01: NOK, Reject Gloss) | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Serial Number | 15 | Alphanumeric value PINPAD serial number | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Data | 20 | Alphanumeric value (TRANSBANK VER. 4.01A) Identifier of the application plus the version of the application | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

### ISES - Command Login Box- PINPAD
This command allows opening a session with the PINPAD from the Cashier.

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | 'ISES' value | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |


**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | 'ISES' value | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Return Code | 2 | Numeric value (00: OK) (Other: Error) | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Drums | 3 | Numerical value (000 -> 100) Battery charge percentage. | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |


### FSES - Box Session End Command - PINPAD
This command allows you to close a session with the PINPAD from the Cashier.

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | Value ‘FSES’ | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |


**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | Value ‘FSES’ | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` | 
Return Code | 2 | Numeric value (00: OK) (Other: Error) | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

### VOUC - Print Request Command Voucher Box - PINPAD

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory |
Command | 4 | Value ‘VOUC’ | Mandatory |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Time out | 5 | Numeric value (00000 -> 99999) Timeout of the command in milliseconds. | Mandatory |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Gloss 1 | 16 | Alphanumeric value Gloss to display first line on screen. | Mandatory |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Gloss 2 | 16 | Alphanumeric value Gloss to display second line on screen. | Mandatory |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Voucher to Print | 4000 | Alphanumeric value Voucher separated by "\ n" for new line, "\ c" for voucher cut | Mandatory |
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | Value ‘VOUC’ | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` | 
Return Code | 2 | Numeric value (00: Print OK) (01: Print error) | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |



### REIM - Reprint Request Command Last PINPAD Voucher - Box
Command sent from the PINPAD to the Cashier so that the cashier sends the Voucher of the last sale to the PINPAD. To send this command you must press on the PINPAD `[ENTER] + [5]`

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | ‘REIM’ value | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |



**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | ‘REIM’ value | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` | 
Return Code | 2 | Numeric value (00: Received OK) (01: No print available) | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` | 
Time out | 5 | Numeric value (00000 -> 99999) Timeout of the command in milliseconds. | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` | 
Gloss 1 | 16 | Alphanumeric value Gloss to display first line on screen. | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` | 
Gloss 2 | 16 | Alphanumeric value Gloss to display second line on screen. | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` | 
Voucher to Print | 4000 | Voucher alphanumeric value separated by `\ n` for new line,` \ c` for voucher cut | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |


### REST - Command Reset Socket Box - PINPAD
Command allows the Box to reset the PINPAD socket.

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | ‘REST’ value | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | ‘REST’ value | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Return Code | 2 | Numeric value (00: Received OK) (01: Error Message) | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

### LKEY - PINPAD Key Load Command - Box
Command executed through the PINPAD menu, which allows the cashier to request a 'Load of keys' to the Cashier. The key loading transaction must be constructed as defined in the "Technical Specifications" manual.

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | Value 'LKEY' | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | Value 'LKEY' | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Return Code | 2 | Numeric value (00: Received OK) (01: Error loading keys) | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

### 0000 - TEST PINPAD command - Box
Command sent from the PINPAD to the Box to keep the socket open.

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
Command | 4 | Value '0000' | Mandatory

**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
Command | 4 | Value '0000' | Mandatory

### CLSB - PINPAD Batch Close Command - Box
Command executed through the PINPAD menu, which allows to indicate to the cashier the sending of a 'Close batch' request.

**Request**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | Value 'CLSB' | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

**Answer**

FACT | LENGTH | COMMENTARY | DEFAULT VALUE
---- | ----- | ---------- | ------------------
LONG_DATA | 4 | Numeric value (0000 -> 9999) Indicating the length of the message, not including these 4 bytes | Mandatory
Command | 4 | Value 'CLSB' | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |
Return Code | 2 | Numeric value (00: Received OK) (Other: Error) | Mandatory
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> Hexadecimal value </i>: `0x7c` |

## Other data

### Card brand table

Name | Abbreviation
---- | -----
VISA | SAW
MASTERCARD | MC
DINERS | DC
AMEX | AX
MAGNA | MG
PRESTO | TP
MORE | TM
CMR | TC
RIPLEY | TR
TEACHER | MT
ELECTRON | THE
DEBIT | DB

### Card type table

Code | Gloss
---- | -----
CR | CREDIT
DB | DEBIT
NB | NO BANK
Null (empty) | Menu in PINPAD is displayed.

### Command response code table

Response code | Gloss
---- | -----
00 | ANSWER OK
84 | THERE IS NO MESSAGE CODE
86 | READ ERROR.
87 | PINPAD WITHOUT MASTER KEY
89 | CHIP CARD DECLINED TRANSACTION
90 | CARD NOT ALLOWED FOR THE SELECTED MODE
91 | QUOTA AMOUNT ERROR
92 | DOES NOT MATCH WITH THE FIRST “TAPEO” CARD
93 | MINIMUM AMOUNT ERROR
94 | VALIDATION ERROR RETURN AMOUNT
95 | CONTEXT ID ERROR
96 | THE LAST 4 DIGITS DO NOT MATCH
97 | THE TRANSACTION DOES NOT ALLOW A REVERSE
98 | MESSAGE FORMAT ERROR
99 | COMMAND CANCELLATION THROUGH THE `[CANCEL]` / TIMEOUT KEY

### PINPAD display message code table

Response code | Gloss
---- | -----
0000 | PASSED
0001 | INTERNAL MESSAGING ERROR
0002 | INTERNAL MESSAGING ERROR
0003 | LOW BATTERY LEVEL
0004 | OPERATION CANCELLATION
0005 | ISSUER NOT AVAILABLE
0006 | TERMINAL NOT AVAILABLE
0007 | NO CONFIRMATION
0008 | ERROR IN POS OPERATION
0009 | MAXIMUM TRIES EXCEEDED SWITCH SERVER CONNECTION
0010 | OPERATION CANCELLATION
0011 | MEANS OF PAYMENT NOT AVAILABLE
0014 | INTERNAL SYSTEM ERROR
0015 | INTERNAL SYSTEM ERROR
0016 | INTERNAL MESSAGING ERROR
0017 | INTERNAL MESSAGING ERROR
0018 | NON-EXISTING SALE TRANSACTION
0019 | ORIGINAL TRANSACTION IS NOT A SALE
0020 | ORIGINAL TRANSACTION NOT FINISHED
0021 | TRANSACTION PENDING ISSUER
0022 | AMOUNT TO BE CANCELED GREATER THAN THE SALE AMOUNT
0023 | AMOUNT TO BE CANCELED OTHER THAN THE SALE AMOUNT
0024 | CARD NOT ALLOWED
0025 | TERMINAL RESPONSE ERROR
0026 | TERMINAL RESPONSE ERROR
