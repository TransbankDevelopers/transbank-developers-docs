# Host to Host


 <aside class="notice">
When you see things like text between symbols <strong> &lt; &gt; </strong> we are referring to ASCII characters that are not printable or visible as text, and when you see things with the ASCII representation of the characters a0e7843947c0xac in the form a0e7843947c06imal of the ASCII.
 </aside>

## Communication Protocol Box - POS

Communication is done through a serial port `RS232`, at speeds ranging from 1200bps to 115200bps` 8N1`
ie `8` data bits,` N`no parity bit, and `1` stop bit.

 <img class="td_img-night" src="/images/referencia/posintegrado/diagrama-comunicacion-caja-pos.png" alt="Diagrama de Comunicación Caja - POS">

 Finished    | Description
 -------    | -------
 `` <STX> ''    | Indicates the start of a message (text) <br> <i> Hex </i>: `0x02`
 DATA      | Corresponds to the command to send to the POS or its response
 `<ETX>`    | Indicates the end of a message (text) <br> <i> Hex </i>: `0x03`
 `LRC`      | It is a byte that is concatenated at the end of the `<ETX>` message and is calculated by performing the `XOR` operation byte by byte of` <DATOS> `+` <ETX> `
 `` <ACK> ``    | Represents the correct reception of the message sent <br> <i> Hex </i>: `0x06`
 `<NAK>`    | It represents the incorrect reception of the sent message, or that the `LRC` of the received message does not correspond to the one sent. <br> <i> Hex </i>: `0x15`
 Timeout1   | It is the time that the box waits to receive `<ACK>` / `<NAK>` from the Integrated POS before retrying sending the message.
 Timeout2   | It is the time that the POS waits to receive `<ACK>` / `<NAK>` from the Cashier before retrying sending the response message.

 <aside class="notice">
All commands that are sent to the POS must comply with the aforementioned flow.
 </aside>

 <aside class="notice">
All messages exchanged between the cashier and the Integrated POS comply with the format: <strong> &lt;STX&gt;DATOS&lt;ETX&gt;LRC a0f65c9z071f
 </aside>

### LRC calculation example

Given the following command:
`<STX> 0200 | 123 | <ETX> `

Which in hexadecimal notation would be:
`0x02 0x30 0x32 0x30 0x30 0x7C 0x31 0x32 0x33 0x7C 0x03`

To calculate the `LRC` we must omit the beginning of the text` <STX> `or` 0x02`.

The operation would then be:

`((((((((0x30 XOR 0x32) XOR 0x30) XOR 0x30) XOR 0x7C) XOR 0x31) XOR 0x32) XOR 0x33) XOR 0x7C) XOR 0x03)`

The result would then be `0x31` in hexadecimal or` 1` in ASCII, therefore, the complete message to send to the Integrated POS is:

bash
 <STX> DATA <ETX> LRC
 0x02 0x30 0x32 0x30 0x30 0x7C 0x31 0x32 0x33 0x7C 0x03 0x31
`` ''

 <aside class="warning">
The Java SDK and C do not support the sending of intermediate messages. For this reason the 3rd parameter of the function in C is always false.
 </aside>

 <img class="td_img-night" src="/images/referencia/posintegrado/diagrama-venta.png" alt="Diagrama de Secuencia Venta">

1. The box sends the request and waits for a response `<ACK>` / `<NAK>`, in case an `<NAK>` arrives, it must retry sending the request 2 times. If you receive an `<ACK>` you should wait for the transaction response.
2. The POS requests the data from the user, and sends the request to the Authorizer, if approved, it is saved in Batch and a response is sent to the box. In case of being rejected, a response is sent to the box indicating the error. ([See Response Table] (/ reference / postgraduate # response-table))
3. Upon receiving the response, the box sends an `<ACK>` if the message is correct, or an `<NAK>` for the case that the `LRC` does not correspond.
4. The POS upon receiving the `<ACK>` returns to the beginning to wait for a new command, in the case that it receives an `<NAK>` it sends the response again 2 more times.

 <strong> Sales Request </strong>

## Connection diagram

### Direct connection between pinpad and box

The commercial box and the pinpad are connected via a USB or SERIAL cable:

! [Connection between pinpad and box] (/ images / documentation / host2host / direct-connection-pinpad-box.png)

### Bluetooth pinpad connection and box

The retail box and the pinpad are connected via Bluetooth:

! [Bluetooth pinpad connection and box] (/ images / documentation / host2host / pinpad-bluetooth-box.png)

### Support software

There are several programs on the internet to send commands to the COM port, one that can help you is:
Docklight


## Messaging protocol according to type of communication

### TLS 1.2 security

SSL (Secure Sockets Layer) or Secure Sockets Layer. It is a protocol that makes use of certificates
to establish secure communications over the network. Since 2015 it has been replaced by TLS (Transport Layer Security) which is based on SSL and is fully compatible.
The communication between the pinpad and the cash register is not secure, but the merchant must implement internal security in its network.

### Security in the WI-FI Network of Commerce

In the event that the Commerce network requires the use of WI-FI connections, the network is required to be an encrypted network *** WPA2-PSK (AES). **

** Security recommendations for a WI-FI network: **
1. Change WI-FI network password regularly
By regularly changing the WI-FI network password, you prevent third parties from
make use of the trade network.
2. Configure the WI-FI network as "Not visible"
By hiding the WI-FI network, it is prevented that people outside the business can find and try to access the business network.
Now every time you try to connect a new device, it will be necessary to first enter the * SSID, and then enter the password.
3. Register and restrict connections by MAC
By having MAC connections enabled, it is specified which computers can make use of the WI-FI network, preventing any other computer from using the business network.
4. Restrict access to "Router Configuration" from WI-FI
By restricting access to the Router's configuration from WI-FI connections, it is prevented from WIFI devices from being able to access this configuration and from modifying its parameters.
5. Regularly monitor WI-FI connections
The current routers allow to know the devices that are connected to the WI-FI network.
It is advisable to monitor in order to avoid any device that is connected without
authorization.

 <aside class="notice">
* WPA2-PSK (AES): Protection system for WI-FI wireless networks. It is the latest WI-FI encryption standard and AES is the latest encryption algorithm.
 </aside>

 <aside class="notice">
* SSID: Broadcast a network SSID. An SSID is the public name of a wireless local area network (WLAN) that serves to differentiate it from other wireless networks in the area. SSID is the network name that is specified when setting up the Wi-Fi network.
 </aside>

### Communication protocol

The protocol that the PINPAD will use is VISA II, over which the request and response messages will be sent. Conceptually the following format is used:

! [Communication protocol] (/ images / documentation / host2host / communication-protocol.png)

Where:

FINISHED             | DESCRIPTION
-------             | -----------
COMMAND START      | Indicates the start of the message (STX).
END COMMAND         | Indicates the end of the message (ETX).
FIELD SEPARATOR     | Indicates the separator of each field within the request and response commands. ASCII value "& # 124;" (Hexa value 0x7c)
LRC                 | It is a byte that is concatenated after the `<FIN COMANDO>` and that is calculated by performing a byte by byte XOR of the messages, which consists of: `<DATA>` + `<FIN COMANDO>`.
ACK                 | It is sent by the PINPAD or the box as an OK reception notice for all commands (Hexa value 0x06).
NAK                 | It is sent by the PINPAD or the box when the calculated LRC does not correspond to the one sent for all commands (Hexa value 0x15).
Timeout1 ACK        | It is the waiting time for the ACK or NAK to retry sending the request through the box. 10 seconds
Timeout 2 Resp      | It is the time that the ACK or NAK waits to retry sending the response through the PINPAD. The time depends on each command.
Timeout 3 ACK       | It is the time that the ACK or NAK waits to retry sending the response through the PINPAD. 10 seconds
STX                 | Indicates a START of the message (Hexa value 0x02).
ETX                 | Indicates an END of the message (Hexa value 0x03).
DATA                | FIELD₀ & # 124; FIELD₁ & # 124; FIELD₂ & # 124;… & # 124; FIELD


### Generic script diagram

The diagram that is described corresponds to the generalization of the behavior of each of the commands or messages sent between the pinpad and the box.

! [Script between Box and PINPAD] (/ images / documentation / host2host / box-script-pinpad.png)

ACK: when a valid message was received, it will be processed and responded with the message or command
corresponding within the defined timeout.
NACK: when an invalid message was received and will not be processed.

Diagram shows the evaluation of the command by the pinpad and box, a command is received, its structure is evaluated, if it is ok, it responds ACK, it does not comply with the documentation, it responds NACK.

Then the response to the command code is validated, if 00 the command is processed, if it is different, it ends with the returned code.

! [Flow when receiving a command] (/ images / documentation / host2host / flow-when-receiving-command.png)

### Context ID Management

When starting a transaction the pinpad gives an ID that must be used by the cashier in the next commands, if the ID does not match the pinpad will reject the command, unless it is the start of another transaction.

Command / field where the box receives the ID from the pinpad:
** 0110 / Context indicator **

### Pinpad parameter update execution flow

Assuming the box has a connection to the pinpad, it prompts the pinpad for a pinpad parameter update

! [Pinpad parameter update command flow] (/ images / documentation / host2host / update-command-flow-param-pinpad.png)

Example:
`` ''
? 0610 | 00 | 0123456789ABCDEF | 0523 | 9.11S4HOST2HOST3DES1 171116113320AO60050000Q0123456789ABCDEFa0010050000 + 0000000000000000000000 + 0000000000 000000000000 + 000000000000000000d597044440001h0010050071r; 597044440001 = 9912 t74 0000000000000000326-478-322 15.30C S0000000000T0000000000W00700000009-A1EL20212223242526272829VI0 6MC0 67DC0 TM0 6AX012345OTTP06TR01TE0
TC12TD12TJ12TH12T812T90 -B01205240-C2100-P000000000000-I0-J0-K000-G33032131100000000 000000000000000000000000000000000000000000000000000000000000 |
m
`` ''

`` ''
0700 | 496 | 9.11S4HOST2HOST3DES1 171116113324AO60000007H9EAC32471E58DF4C996725D34FF96F6DI8A2A30A556EBC3E68B496CE2116A F50FM0000000000000000Q0123456789ABCDEFW0071111005g APROBADOh0010050071l0010050000 + 0000000000000000000000 + 0000000000000000000000 + 0000000 00000000000tS4HOST2HOST3DES19-A1EL20212223242526272829VI0 6MC0 67DC0 6AX012345OTTP06TR01TE0 TM0 TC12TD12TJ12TH12T812T90 -B11205243-C0000-G330321311000000000000000200000000000001200 000000000000000000000400000000000002400-P100000800000 |
`` ''
### Detailed sales flow

! [Detailed script of a sale] (/ images / documentation / host2host / detailed-script-sale-commands.png)

! [Detailed command flow diagram of a sale with problem response] (/ images / documentation / host2host / command-sale-response-with-problem.png)

1. In the case of a response from the pinpad that is outside of what is specified (wrong data type, wrong length, wrong or missing separator, etc.), the cash register must interpret that the transaction ended with a problem and then request a reversal according to the “flow of execution of reverse at the request of the box ”detailed below.
If you want to retry the sale, you must start a new sales flow.

Sale example:
`` ''
0100 | 00 | N | N | N | 12100 | CL | CR || 0 | 0 | 0110 | 00 | 2017111611350940 | 01 ||||| 5197 || MASTERCARD | MC | N | 4 0200 | 12100 | 0 || 0 | 0 | 2017111611350940 | 00 | 597044440001 | S4HOST2HOST3DES1 |||| 1711161136110 0000000000754 || 0123456789ABCDEF ||||| 5197 | v

0210 | 00 | 2017111611350940 | 0737 | 9.12S4HOST2HOST3DES1 171116113517FO00050000B000000000000012100P1Q0123456789ABCDEFa00000000000000000000000 000NU1711161136110000000000075400000000000000000CL0000000d597044440001e00h0010050081 G3F3308F4S0t74 0000000000000000326-478-322 15.30C 6-E051-I152-O0180152171116B8738FEAC1697887380000669B8C926200000080000015200000001210 00000000000000014A78003040000716800000000000000FF-P0100224403020002 E0F8C826478322RA00000000410109-A1EL20212223242526272829VI0 6MC0 67DC0
6AX012345OTTP06TR01TE0
TC12TD12TJ12TH12T812T90 -B41205240-C2100-P000000000000-I0-J0-K000W0161111005CR 0000- 4F552D8E65F6056E543A481CDD07D2525E2D7347C32D2CA5756BCB176482684949FD044044F176482684949FD0440DCB176482684949FD0440DC | (
TM0
0500 | 2017111611350940 | 513 | 9.12S4HOST2HOST3DES1 171116113521FO00000005B000000000000012100D4EF600979 BGA1B5296BH0D9C83C5A59B574F7AF35145C606D5D4ID5879B9816C5A304236AB90B081A60A0P1Q01234 56789ABCDEFS0 T0000000000W0161111005CRCMC0000a00000000000003000000000000NU171116113611000000000007 5400000000000000000CL0000000d597044440001e00g APROBADOh0010050081ptS4HOST2HOST3DES16-E051-I1529-A1EL20212223242526272829VI0 6MC0 67DC0 6AX012345OTTP06TR01TE0 TM0 TC12TD12TJ12TH12T812T90 -B11205243-C0000-P100000800000 |

0510 | 00 | 2017111611350940 | 597044440001 | S4HOST2HOST3DES1 | 0 | 0000 | 600979 B | 12100 || 00 || 5197 | 001005008 | CREDIT || ************ 5197 | MC | 171116 | 113521 | ||||||| 1 | 1 | 1 | 1 | 0 | 05 | CR | 0 | 0 | 0000 | NO FEES ||||||| 0000 ||| 0000 ||| 0000 ||| 005 | APPROVED | N || 0 | 1 || 001005008 | Y |||
`` ''

### Reverse Execution Flow at Cash's Demand

The box had no response from any command or had no response from an SPDH message in flight, therefore it could not finish a sale that it started, then it does not know if it is approved or rejected, in this case you must request a reversal to the pinpad.

! [Reverse requested by box] (/ images / documentation / host2host / reverse-requested-per-box.png)

For command 400, the following messages are considered as Successful Reverse:

Command             | Observation
-------             | -----------
510 & # 124; 89 & # 124;   | Deprecated since 18.2x
510 & # 124; 85 & # 124;   | Deprecated since 18.2x
510 & # 124; 00 & # 124; ... & # 124; Any Transbank Response Code & # 124; ... | Deprecated since 18.2x
510 & # 124; 00 & # 124; ... & # 124; Transbank response code <010 & # 124; ...     | Optimized response at 18.2x

Reverse example:

`` ''
0400 | 2019062813081650 |
0410 | 00 | 2019062813081650 | 0643 | 9.07S4CAJAHOST000010 190628130853FT00000000B000000000000650000P1Q0123456789ABCDEFa00000015000000000000000 000NU2019062813091100100100000100000000000000000CL0000000d597044440001e00h0010030051 G3F27A500S0t74 0000000000000000326-018-973 18.21P 6-E071-I152-O0180152190628236E485C1DC23FC81980067116EE3A5C0000008000001520000006500000000000000000110A04001220000000000000000000000FF-P0101221F03020002 00080826018973A00000000410109-A1EL20252627 VI123456MC123456DC AX123456OTTP06TR1 TE0 TM0 TC12TD12TJ12TH12T812T90 -B41205240-C2100-P000000000000-I0-J1-K000- M1W0161111005CR 0000 |
0500 | 2019062813081650 | 643 | 9.07S4CAJAHOST000010 190628130853FT00000000B000000000000650000P1Q0123456789ABCDEFa00000015000000000000000 000NU2019062813091100100100000100000000000000000CL0000000d597044440001e00h0010030051 G3F27A500S0t74 0000000000000000326-018-973 18.21P 6-E071-I152-O0180152190628236E485C1DC23FC81980067116EE3A5C00000080000015200000065000 00000000000000110A04001220000000000000000000000FF-P0101221F03020002 00080826018973A00000000410109-A1EL20252627 VI123456MC123456DC AX123456OTTP06TR1 TE0 TM0 TC12TD12TJ12TH12T812T90 -B41205240-C2100-P000000000000-I0-J1-K000-M1W0161111005CR 0000 |
0510 | 00 | 2019062813081650 | 597044440001 | S4CAJAHOST000010 ||| 265404 B | 650000 || 00 || 0003 | 001003005 | CREDIT || ************ 0003 | | 190628 | 130853 |||||||| 1 | 1 | 1 | 1 | 0 | 05 | CR | 0 | 0 | 0000 | NO FEES ||||||| 0000 ||| 0000 ||| 0000 ||| 000 | APPLIED REVERSE | N || 0 | 1 || 001003005 | Y ||| X
`` ''

With the 510 the pinpad displays “REVERSE APPLIED” on the screen.

## Command Description

This chapter details each command that can be sent to the PINPAD.
For serial communication it is necessary to indicate the start and end characters that are sent in
each command, which will be indicated by the following nomenclature:

CONTROL CHARACTER     | NOMENCLATURE
-------------------     | ------------
`` <STX> <DATA> <ETX> <LRC> '' | STXETX

In order to maintain the same language in TCP-IP communication, these STX, ETX and LRC characters are kept.

### Sales commands

The commands 0100, 0110, 0200, 0210, 0400, 0410, 0500, 0510 are the same as Retail Standard vx805 version 15.2 to leave them in a single document are attached here:

### 0100 - 0110 Read card command

At this point the box has already built the sale in its system, for which it already has the data
required to start the payment process with Transbank with the command 0100.
From this point, the transaction data must be recorded to be supplemented with the following commands, because in the event of a fall the data is protected to request a reversal or finally print the voucher with the approved transaction.

**Request**

FACT        | LENGTH     | COMMENTARY            | DEFAULT VALUE
------      | ------    | ------                | ------    
`` <STX> ''     | 1         | Indicates the start of text or command <br> <i> hex value </i>: `0x02`| STX
`Command`   | 4         |  <i> ASCII value </i>: `0100` <br> <i> hexadecimal value </i>:` 0x30 0x31 0x30 0x30` | 
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Local commerce OnUs` |2| ** Numerical Value ** <br /> 00 Merchant without own cards <br> 01-99 Onus merchants, TBK assigns a number for reading own cards |** 00 **
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`BIN delivery` | 1       | ** Alphanumeric value ** <br /> (Y: Yes) <br /> (N: No) <br /> It is used to know the bin of the card and be able to make a discount for sale by agreement with the bank |** N **
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Offline transaction`|1 | ** Alphanumeric value ** <br /> (Y: Yes) <br /> (N: No) <br /> Its use is no longer allowed | ** N **
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Self-service` | 1      | ** Alphanumeric value ** <br /> (Y: Yes) <br /> (N: No) <br />| ** N **
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount`     | 18        | ** Numeric value (maximum) ** <br /> Purchase Amount (without tip, no change) <br> Minimum amount $ 50.00 or US $ 1.00 <br /> Includes two decimal places.|
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Currency code` | 2  | ** Alphanumeric value ** <br /> & # 124; CL & # 124; Chilean pesos 152 <br> & # 124; US & # 124; US dollars 840 | ** CL **
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Card type`| 2    | ** Alphanumeric value ** <br /> Indicator of the type of menu for which the transaction was made. <br> & # 124; CR & # 124 ;: NO BANK <br> & # 124; DB & # 124 ;: DEBIT - PREPAYMENT <br> & # 124; NB & # 124 ;: NO BANK <br> card value type a19fda034bz0 Type of card value a19fda19bz0z sale made as debit can be authorized as prepaid *** | 
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`List of change amounts`| 60 | ** Numeric value (maximum) ** <br /> List of change amounts allowed, separated by ";" They include two decimal places, you must always send the 4 options defined by Transbank. <br> Parametric field by point of sale. <br> Only if "Card type = DB" | ** 500000; ** <br> ** 1000000; ** <br> ** 2000000; ** <br> ** 5000000; **
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount returned`| 18      | ** Numeric value (maximum) ** <br /> & # 124; 0 & # 124 ;: Does not show menu on pinpad <br> & # 124; & # 124; : ø Displays menu by consulting back <br> Yes & # 124; n & # 124; > 0 no menu is displayed. The value must correspond to one sent in the field "List of returned amounts" <br> Returned only exists in debit, send 0 in credit <br> Field c | **OR**
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Tip / donation amount`| 9 | ** Numeric value (maximum) ** <br /> Corresponds to the tip or donation amount of the sale or cancellation <br> (includes two decimals) <br/> <br/> ** Important: ** If you want to confirm it and ask the Card Holder to be tipped. You must send this field the empty value (Ø) <br /> ** For cancellations ** you must enter the amount of the tip of the sale to be canceled, in case of not having a tip ** enter a zero (0). ** | **OR**
`Separator` | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`     | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`     | 1         | Byte result of the message XOR operation | 


Maximum waiting time per command 110 of 35sec, since the PinPad waits 30sec for the customer to operate a card, therefore after 30 seconds if no card is operated, it returns a 110 & # 124; 99.

**Answer**


FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0110` <br> <i> hexadecimal value </i>:` 0x30 0x31 0x31 0x30` |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`PinPad response code`| 2  | ** Numerical Value ** <br> In case of rejection, it must be displayed at the point of sale: <br> PINPAD REJECTION - `<XX>`: `<GLOSA>` <br> response codes / command reference table [] to-host # response-table-pinpad) |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator` | 16| ** Alphanumeric value ** <br> Format yyyymmddhhmmssmm <br> It is just an ID, the date and time in the pinpad may be out of date. |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Type of capture` | 2       | ** Numeric value ** <br> This field will be used in futures <br> & # 124; 00 & # 124; : B - Band <br> & # 124; 01 & # 124; : E. EMV w / contact <br> & # 124; 02 & # 124; : C - Contacless <br> & # 124; 03 & # 124; : F - Fallback <br> & # 124; 04 & # 124; : D - Digitized |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`TRACK I`       | 80        | ** Alphanumeric value (maximum) ** <br> Filled with blanks (0x20) to the right <br> If “Local business OnUs ≠ 00” <br> 160 alphanumeric characters are delivered with encrypted bread, corresponding to 80 HEXA |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`TRACK II`      | 40        | ** Alphanumeric value (maximum) ** <br> Filled with blanks (0x20) to the right a <br> If “Local commerce OnUs ≠ 00” <br> With encrypted bread, 80 alphanumeric characters are delivered corresponding to 40 HEXA |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`PAN SHA-1`     | 40        | ** Alphanumeric value ** <br> PAN encrypted with SHA-1 algorithm <br> If "Offline transaction = Y" |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`BIN`           | 6         | ** Numerical Value ** <br> If "BIN delivery = Y" or "Offline transaction = Y" |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Last 4 digits` | 4     | **Numerical value** | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Cardholder name`| 26| ** Alphanumeric value (maximum) ** |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Card brand name`| 20 | ** Alphanumeric value (maximum) ** <br> According to [Flag table] (/ reference / host-to-host # flag-table) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Card abbreviation` | 2 | ** Alphanumeric value ** <br> According to [Flag table] (/ reference / host-to-host # flag-table) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag requests last 4 digits` | 1 |** Alphanumeric value ** <br> (Y: The point of sale must request the 4 units) <br> (N: The pinpad will request the entry of PIN) <br> In case of cancellation, no pin is entered, nor is entry of 4 units requested.    | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

### 0200 - 0210 Sales request / cancellation command

**Request**

FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0200` <br> <i> hexadecimal value </i>:` 0x30 0x32 0x30 0x30` | ** 0200 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount`         | 18        | ** Alphanumeric value (maximum) ** <br> Purchase Amount (without tip, without change) <br> Minimum amount $ 50.00 or US $ 1.00 <br> Includes two decimal places |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Ticket / Ballot Number`| 10 | ** Alphanumeric value ** <br> If the merchant does not use this field, send the field a zero | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Number of Quotas`| 2       | ** Numeric value ** <br> Mandatory if "Transaction type = 01" <br> If the original sale was without fees, the value 00 must be reported |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Print field`| 1        | ** Numeric value ** <br> Indicates if formatted voucher is delivered <br> 0: Does not send voucher (uses 500-510 commands) <br> 1: Sends voucher (uses 540-550 commands) | **one**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Send Messages`| 1        | ** Numeric value ** <br> Indicates whether the PINPAD should send transaction status messages <br> 0: Does not send messages (Default value) <br> 1: Sends messages | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`| 16 | ** Alphanumeric value ** <br> Id delivered by the pinpad for each transaction |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction type`| 2    | ** Numeric value ** <br> (00: Sale) <br> (01: Cancellation) |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Commercial code`| 12    | ** Numeric value ** <br> Merchant code delivered by TBK and configured at checkout. <br> EJ: 597012345678 |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Terminal ID`  | 16        | ** Alphanumeric value ** <br> DDLL or Logical address delivered by TBK and configured in the box in the parameter table. |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount of installments` | 9         | ** Numeric value ** <br> Mandatory if Transaction type = 01. If the original sale was without fees, the value "0" must be reported |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`` Product ``     | 1         | ** Numeric value ** <br> Mandatory if Transaction type = 01. Corresponds to the "Installment type" field of command 0510 of the original sale | **OR**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
~~ `Employee number` ~~| 6 | ** Alphanumeric value ** <br> Unused field always send empty | **OR**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Unique number`  | 26        | ** Numerical value ** <br> Cashier must send the unique number to print on the voucher for both sale and cancellation. <br> Each business can define a format for the unique number, TBK delivers the following <br> example: YYYYMMDDHHMMSSLLLCCCXXXXXX <br> Where LLL is a number of the premises <br> <br> counter CCC is the counter number XXXfz19 a0 CCC0 afz19XXX counter field afz02 CCC0 is the counter field number XXXfz  |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`` Clerk Code (Employee) ``| 4 | ** Numeric value ** <br> Employee field is optional <br> If the merchant does not use it, send the empty field | **OR**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Internal trade index`| 16  | ** Alphanumeric value (maximum) ** <br> Field that can be used by the merchant to add information that serves its internal processes, cash number, vendor, sales ID, etc. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Original transaction authorization code`| 8 | ** Alphanumeric value (maximum) ** <br> Authorization code of the original sale, mandatory if it is a cancellation "Transaction type = 01" | **OR**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Original transaction sequence number`| 9 | ** Alphanumeric value (maximum) ** <br> Sequence number of the original sale, mandatory if it is an annulment "Transaction type = 01" <br> Also known as transaction number | **OR**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Original transaction date`| 6 | ** Format YYMMDD ** <br> Date of the original sale, mandatory if it is an annulment "Transaction type = 01" | **OR**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Original transaction time` | 6 | ** HHMMSS format ** <br> Original sale time, mandatory if it is an annulment "Transaction type = 01" | **OR**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Last 4 digits`| 4      | ** Numerical value ** <br> If it is a cancellation "Transaction type = 01", you must enter the 4 units of the original sale, against this data the pinpad will compare with the card swiped to cancel. <br> If "Transaction type = 00" and "Flag requests last 4 digits = Y" of command 0110, the 4 units of the swiped card must be entered. | **OR**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

Maximum waiting time per command 210 of 125sec, since there are up to 4 interaction with the user.

**Answer**


FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0210` <br> <i> hexadecimal value </i>:` 0x30 0x32 0x31 0x30` | ** 0210 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`PinPad response code`| 2 | ** Numerical Value ** <br> In case of rejection, it must be displayed at the point of sale: <br> PINPAD REJECTION - `<XX>`: `<GLOSA>` <br> response codes / command reference table [] to-host # response-table-pinpad) |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`| 4  | ** Alphanumeric value ** <br> Id delivered by the pinpad for each transaction  |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Long message` | 4         | **Numerical value** |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`SPDH message`  | 2048      | ** Alphanumeric value (maximum) ** |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

### 0400 - 0410 Reverse request command

If a reversal is required, the cashier retrieves the context indicator it has saved from your last transaction (regardless of the level of completeness of the transaction) and prompts the pinpad for the reversal. The pinpad can discriminate if only the card was read, or only a quota inquiry was made, or if the sale was rejected or approved.
If the pinpad is not present, you must keep reverse pending, until the pinpad can respond.

**Request**


FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0400` <br> <i> hexadecimal value </i>:` 0x30 0x34 0x30 0x30` | ** 0400 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`| 16 | ** Alphanumeric value ** <br> Id of the transaction to be reversed  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

Maximum waiting timeout per command 410 of 20sec and does not require interaction with cardholder.

**Answer**



FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0410` <br> <i> hexadecimal value </i>:` 0x30 0x34 0x31 0x30` | ** 0410 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`PinPad response code`| 2 | ** Numerical value ** <br> In case of rejection it must be displayed at the point of sale: <br> PINPAD REJECTION - `<XX>`: `<GLOSA>` <br> (Table of response codes / command reference] to-host # response-table-pinpad) |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`| 16 | ** Alphanumeric value ** <br> Id delivered by the pinpad for each transaction   |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Long message` | 4         | **Numerical value** |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`SPDH message`  | 2048      | ** Alphanumeric value (maximum) ** |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

### 0500 - 0510 Command Validation / update request

In this command 0510 the box must validate if the final answer (Terminal flag) and if it is approved (Transbank answer code) and then print. If the transaction is not final (Flag terminal) you must forward the attached spdh message, it does not matter if the transaction is approved or not in this case.

**Request**


FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0500` <br> <i> hexadecimal value </i>:` 0x30 0x35 0x30 0x30` | ** 0500 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`| 16 | ** Alphanumeric value ** <br> Id delivered by the pinpad for each transaction  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Long message` | 4         | **Numerical value** |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`SPDH message`  | 2048      | ** Alphanumeric value (maximum) ** | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

Maximum waiting timeout per command 510 of 125sec, since there are up to 4 interaction with the user.

**Answer**

FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0510` <br> <i> hexadecimal value </i>:` 0x30 0x35 0x31 0x30` | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`PinPad response code`| 2 | ** Numerical value ** <br> In case of rejection it must be displayed at the point of sale: <br> PINPAD REJECTION - `<XX>`: `<GLOSA>` <br> (Table of response codes / command reference] to-host # response-table-pinpad) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`| 16 | ** Alphanumeric value ** <br> Id delivered by the pinpad for each transaction  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Commercial code`| 12    | ** Numeric value ** <br> Merchant code delivered by TBK and configured at checkout, printed on voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Terminal ID`  | 16        | ** Alphanumeric value ** <br> Logical address delivered by TBK and configured in the box, it is printed on the voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Ticket / Ticket Number`| 20  | ** Alphanumeric value ** <br> Optional field, if it comes it is printed in the voucher if it doesn't come the field is omitted | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Employee`      | 4         | ** Alphanumeric value ** <br> Optional field, if it comes it is printed in the voucher if it doesn't come the field is omitted | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Authorization Code`| 8    | ** Alphanumeric value (maximum) ** <br> Transaction authorization code sent by TBK example: & # 124; AB 12 C3 & # 124; <br> What comes in the voucher is printed | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount`         | 18        | ** Numeric value (maximum) ** <br /> Total authorized amount (includes the amount of the sale, tip, return and donation as the case may be) <br> It is printed on voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount returned`  | 18        | ** Numeric value (maximum) ** <br /> Return selected by customer, only applies in debit <br> It is printed in voucher| 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Number of Quotas`| 2       | ** Numeric value ** <br> Amount of transaction fees (for sales without fees, “00” is reported) <br> It is printed on the voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount fee`   | 14        | ** Numeric value ** <br> If the reported amount is empty & # 124; & # 124; or & # 124; 0 & # 124; box must omit the entire line on the voucher. <br> It is printed in the voucher if the field comes |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Last 4 Card Digits` | 4 | ** Numeric value ** <br> Does not print | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Operation Number`| 9       | ** Terminal transaction map ** <br> Also known as the sequence number, this field must be printed on the sales and cancellation voucher. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`` Card Type Gloss `| 7 | ** Alphanumeric value (maximum) ** <br> Gloss value in [Card type table] (/ reference / host-to-host # card-type-table) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`` Accounting Date ``| 6        | ** Alphanumeric value ** <br> It is used only if it is a Debit transaction <br> Cash must not be formatted (ex: DDAAMM), it simply must transfer the value to the voucher (XX / XX / XX) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Account Number`| 19      | ** Alphanumeric value ** <br> Masked card number to include in the voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Card abbreviation` | 2 | ** Alphanumeric value ** <br> Value to print on the voucher |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction Date`| 6      | ** YYMMDD format ** <br> Value to print in the voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction Time` | 6      | ** HHMMSS format ** <br> Value to print in the voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Print field`  | 8192   | ** Field depends if the box requires a formatted voucher (maximum) ** <br> The voucher is not sent in this command. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Winning transaction`| 1   | ** Numerical value ** <br> & # 124; 1 & # 124 ;: winning transaction <br> In this case, the cashier must print a PEL voucher in addition to the sales voucher <br> & # 124; & # 124 ;: transaction without a prize  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promotion type`| 1         | ** Numeric value ** <br> & # 124; 1 & # 124 ;: Delivery Point of Sale <br> & # 124; 2 & # 124 ;: Deferred Delivery <br> & # 124; 3 & # 124 ;: Return to Cardholder  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promo code`| 8       | ** Alphanumeric value ** <br> Value to print on the awarded voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promotion name`| 21      | ** Alphanumeric value ** <br> Value to print on the award voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
Glosa voucher prize| 62     | ** Alphanumeric value ** <br> Value to print on the award voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Prize voucher text`| 27     | ** Alphanumeric value ** <br> Value to print on the award voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag allows quotas`| 1    | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag of grace`| 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag C2C`      | 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag C3C`      | 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag NCuotas`  | 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Maximum quota flag`| 2  | ** Numeric value ** <br> Informational field of the trade configuration | ** 00 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Menu type`  | 2         | ** Alphanumeric value ** <br> Indicator of the type of menu for which the transaction was made <br> & # 124; CR & # 124; : CREDIT <br> & # 124; DB & # 124; : PREPAID DEBIT <br> & # 124; NB & # 124; : NON-BANK <br> Type value in [Card type table] (/ reference / host-to-host # card-type-table) <br> A sale made as debit can be authorized as prepaid | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Graceful transaction indicator`| 1 | ** Numeric value ** <br> Transaction mode indicator <br> & # 124; 0 & # 124; transaction without grace month <br> & # 124; 1 & # 124; grace month transaction | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Type of installments`   | 1         | ** Numeric value ** <br> & # 124; 0 & # 124; No fees <br> & # 124; 1 & # 124; Normal odds <br> & # 124; 3 & # 124; C3C or C2C <br> & # 124; 4 & # 124; CIC or N-installments | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Applied rate` | 4         | ** Numeric value ** <br> It is only printed in the voucher if "Flag print rate = 1" | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Fee-type gloss`| 30      | ** Alphanumeric value ** <br> Gloss to print on voucher <br> If the reported field is empty "& # 124; & # 124;" box should omit the line on the voucher. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Fee-type gloss 2`| 22    | ** Alphanumeric value ** <br> Gloss that is displayed in pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promotion gloss`| 10       | ** Alphanumeric value ** <br> Gloss that is displayed in pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Id promotion`  | 10        | ** Alphanumeric value ** <br> Gloss that is displayed in pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag print rate`| 1     | ** Numeric value ** <br> & # 124; & # 124; or & # 124; 0 & # 124; does not print applied rate <br> & # 124; 1 & # 124; print applied rate | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred period`  | 3     | ** Numeric value ** <br> Deferred period selected, value to print on voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 1 period`| 3     | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 1 rate value`| 4  | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 1 installment value`| 14| ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 2 period`| 3     | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 2 value rate`| 4  | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 2 installment value`| 14| ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 3 period`| 3     | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 3 value rate`| 4  | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 3 installment value`| 14| ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Original transaction sequence number` | 9 | ** Alphanumeric value (maximum) ** <br /> Also known as the original transaction number of the sale. This field is not being used, it is not printed | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transbank response code`| 3 | ** Numeric value ** <br /> Response code after the transaction is completed. It must be displayed at the point of sale. <br> EJ: TRANSBANK RESPONSE - `<XXX>`: `<GLOSA>` | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transbank answer gloss` | 48 | ** Alphanumeric value (maximum) ** <br /> Gloss that displays the pinpad once the transaction is completed. It must be displayed at the point of sale. <br> EJ: TRANSBANK RESPONSE - `<XXX>`: `<GLOSA>`| 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction flag with PIN`| 1 | ** Alphanumeric value ** <br /> Y: Transaction authenticated with PIN <br> N: Transaction authenticated by signature | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Cardholder name`| 26| ** Alphanumeric value ** <br /> Only print if "Flag type voucher = 1, 2 or 3" | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Voucher flag` | 1     | ** Numeric value ** <br /> Depending on the number received, a voucher must be printed with or without signature: <br> & # 124; 0 & # 124; = Unsigned <br> & # 124; 1 & # 124; or & # 124; 2 & # 124; or & # 124; 3 & # 124; = with signature <br> <br> ** Voucher headers: ** <br> For credit sales: “CREDIT SALE” <br> For debit sales (always without signature): “DEBIT SALE” with NOBANK sales a0342f BANK SALE: "<br> For prepaid sales (without signature):" PREPAID SALE "<br> For cancellations with credit (without signature):" CREDIT CANCELLATION "<br> For cancellations with no bank (without signature):" NON-BANK CANCELLATION " | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag quota mode`| 1  | ** Alphanumeric value ** <br /> 0: Mode 3.1 (Not used) <br> 1: Quota mode 4.0 | **one**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction gloss affects savings` | 40 | ** Alphanumeric value (maximum) ** <br /> It must be printed on the voucher when it is different from empty <br> Field 9, subfield D | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Sequence number` | 9   | ** Numeric value ** <br /> This field is not being used, it is not printed <br> Also known as operation number | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Terminal message flag` | 1 | ** Alphanumeric value ** <br /> Y: The message is terminal and the response SPDH message should NOT be sent <br> N: The response SPDH message should be sent | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Long message` | 4         | **Numerical value**  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`SPDH Sale / Reverse Message`| 2048 | ** Alphanumeric value (maximum) ** | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

### 0520 - 0530 Command Validation / update request

Only for standard retail wifi pinpad.

### 0540 - 0550 Command Validation / update request

In this command 0540 the box must validate if the final answer (Terminal flag) and if it is approved (Transbank answer code) and then print. If the transaction is not final (Flag terminal) you must forward the attached spdh message, it does not matter if the transaction is approved or not in this case.
Provides a voucher formatted for printing and also supports Prepaid cards.

**Request**



FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0540` <br> <i> hexadecimal value </i>:` 0x30 0x35 0x34 0x30` | ** 0540 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`| 16 | ** Alphanumeric value ** <br> Id delivered by the pinpad for each transaction | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Long message` | 4         | **Numerical value** | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`SPDH message`  | 2048      | ** Alphanumeric value (maximum) ** | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Trade Name`| 40       | ** Alphanumeric value ** <br> Boxed parametric field sent to pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Commerce Directorate`| 40    | ** Alphanumeric value ** <br> Boxed parametric field sent to pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Commerce Commune`| 40       | ** Alphanumeric value ** <br> Boxed parametric field sent to pinpad <br> May be commune or city | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

Maximum waiting time per command 540 of 125sec, since there are up to 4 interaction with the user.

**Answer**



FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0550` <br> <i> hexadecimal value </i>:` 0x30 0x35 0x31 0x30` | ** 0550 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`PinPad response code`| 2 | ** Numerical Value ** <br> In case of rejection, it must be displayed at the point of sale: <br> PINPAD REJECTION - `<XX>`: `<GLOSA>` <br> response codes / command reference table [] to-host # response-table-pinpad) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`| 16 | ** Alphanumeric value ** <br> Id delivered by the pinpad for each transaction  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Commercial code`| 12    | ** Numeric value ** <br> Merchant code delivered by TBK and configured at checkout, printed on voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Terminal ID`  | 16        | ** Alphanumeric value ** <br> Logical address delivered by TBK and configured in the box, it is printed on the voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Ticket / Ballot Number`| 20 | ** Alphanumeric value ** <br> Optional field, if it comes it is printed in the voucher if it doesn't come the field is omitted | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Employee`      | 4         | ** Alphanumeric value ** <br> Optional field, if it comes it is printed in the voucher if it doesn't come the field is omitted | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Authorization Code`| 8   | ** Alphanumeric value (maximum) ** <br> Transaction authorization code sent by TBK example: & # 124; AB 12 C3 & # 124; <br> What comes in the voucher is printed | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount`         | 18        | ** Numeric value (maximum) ** <br /> Total authorized amount (includes the amount of the sale, tip, return and donation as the case may be) <br> It is printed on voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount returned`  | 18        | ** Numeric value (maximum) ** <br /> Return selected by customer, only applies in debit <br> It is printed in voucher| 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Number of Quotas`| 2       | ** Numeric value ** <br> Amount of transaction fees (for sales without fees, “00” is reported) <br> It is printed on the voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount fee`   | 14        | ** Numeric value ** <br> If the reported amount is empty & # 124; & # 124; or & # 124; 0 & # 124; box must omit the entire line on the voucher. <br> It is printed in the voucher if the field comes |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Last 4 Card Digits` | 4 | ** Numeric value ** <br> Does not print | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Operation Number`| 9       | ** Terminal transaction map ** <br> Also known as the sequence number, this field must be printed on the sales and cancellation voucher. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`` Card Type Gloss `| 7 | ** Alphanumeric value (maximum) ** <br> Gloss value in [Card type table] (/ reference / host-to-host # card-type-table) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`` Accounting Date ``| 6        | ** Alphanumeric value ** <br> It is used only if it is a Debit transaction <br> Cash must not be formatted (ex: DDAAMM), it simply must transfer the value to the voucher (XX / XX / XX) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Account Number`| 19      | ** Alphanumeric value ** <br> Masked card number to include in the voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Card abbreviation` | 2 | ** Alphanumeric value ** <br> Value to print on the voucher |
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction Date`| 6      | ** YYMMDD format ** <br> Value to print in the voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction Time`| 6       | ** HHMMSS format ** <br> Value to print in the voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Print field`| 8192     | ** Field depends if the box requires formatted voucher (maximum) ** <br> Voucher is always sent | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Winning transaction`| 1   | ** Numerical value ** <br> & # 124; 1 & # 124 ;: winning transaction <br> In this case, the cashier must print a PEL voucher in addition to the sales voucher <br> & # 124; & # 124 ;: transaction without a prize  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promotion type`| 1         | ** Numeric value ** <br> & # 124; 1 & # 124 ;: Delivery Point of Sale <br> & # 124; 2 & # 124 ;: Deferred Delivery <br> & # 124; 3 & # 124 ;: Return to Cardholder  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promo code`| 8       | ** Alphanumeric value ** <br> Value to print on the awarded voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promotion name`| 21      | ** Alphanumeric value ** <br> Value to print on the award voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
Glosa voucher prize| 62     | ** Alphanumeric value ** <br> Value to print on the award voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Prize voucher text`| 27     | ** Alphanumeric value ** <br> Value to print on the award voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag allows quotas`| 1    | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag of grace`| 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag C2C`      | 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag C3C`      | 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag NCuotas`  | 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Maximum quota flag`| 2  | ** Numeric value ** <br> Informational field of the trade configuration | ** 00 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Menu type`  | 2         | ** Alphanumeric value ** <br> Indicator of the type of menu for which the transaction was made <br> & # 124; CR & # 124; : CREDIT <br> & # 124; DB & # 124; : PREPAID DEBIT <br> & # 124; NB & # 124; : NON-BANK <br> Type value in [Card type table] (/ reference / host-to-host # card-type-table) <br> A sale made as debit can be authorized as prepaid | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Graceful transaction indicator`| 1 | ** Numeric value ** <br> Transaction mode indicator <br> & # 124; 0 & # 124; transaction without grace month <br> & # 124; 1 & # 124; grace month transaction | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Type of installments`   | 1         | ** Numeric value ** <br> & # 124; 0 & # 124; No fees <br> & # 124; 1 & # 124; Normal odds <br> & # 124; 3 & # 124; C3C or C2C <br> & # 124; 4 & # 124; CIC or N-installments | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Applied rate` | 4         | ** Numeric value ** <br> It is only printed in the voucher if "Flag print rate = 1" | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Fee-type gloss`| 30      | ** Alphanumeric value ** <br> Gloss to print on voucher <br> If the reported field is empty "& # 124; & # 124;" box should omit the line on the voucher. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Fee-type gloss 2`| 22    | ** Alphanumeric value ** <br> Gloss that is displayed in pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promotion gloss`| 10       | ** Alphanumeric value ** <br> Gloss that is displayed in pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Id promotion`  | 10        | ** Alphanumeric value ** <br> Gloss that is displayed in pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag print rate`| 1     | ** Numeric value ** <br> & # 124; & # 124; or & # 124; 0 & # 124; does not print applied rate <br> & # 124; 1 & # 124; print applied rate | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred period`| 3       | ** Numeric value ** <br> Deferred period selected, value to print on voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 1 period`| 3     | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 1 rate value`| 4  | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 1 installment value`| 14| ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 2 period`| 3     | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 2 value rate`| 4  | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 2 installment value`| 14| ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 3 period`| 3     | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 3 value rate`| 4  | ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 3 installment value`| 14| ** Numeric value ** <br> Not used | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Original transaction sequence number` | 9 | ** Alphanumeric value (maximum) ** <br /> Also known as the original transaction number of the sale. This field is not being used, it is not printed | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transbank response code`| 3 | ** Numeric value ** <br /> Response code after the transaction is completed. It must be displayed at the point of sale. <br> EJ: TRANSBANK RESPONSE - `<XXX>`: `<GLOSA>` | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transbank answer gloss` | 48 | ** Alphanumeric value (maximum) ** <br /> Gloss that displays the pinpad once the transaction is completed. It must be displayed at the point of sale. <br> EJ: TRANSBANK RESPONSE - `<XXX>`: `<GLOSA>`| 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction flag with PIN`  | 1 | ** Alphanumeric value ** <br /> Y: Transaction authenticated with PIN <br> N: Transaction authenticated by signature | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Cardholder name` |26| ** Alphanumeric value ** <br /> Only print if "Flag type voucher = 1, 2 or 3" | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Voucher flag` | 1     | ** Numeric value ** <br /> Depending on the number received, a voucher must be printed with or without signature: <br> & # 124; 0 & # 124; = Unsigned <br> & # 124; 1 & # 124; or & # 124; 2 & # 124; or & # 124; 3 & # 124; = with signature <br> <br> ** Voucher headers: ** <br> For credit sales: “CREDIT SALE” <br> For debit sales (always without signature): “DEBIT SALE” with NOBANK sales a0342f BANK SALE: "<br> For prepaid sales (without signature):" PREPAID SALE "<br> For cancellations with credit (without signature):" CREDIT CANCELLATION "<br> For cancellations with no bank (without signature):" NON-BANK CANCELLATION " | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag quota mode` | 1 | ** Alphanumeric value ** <br /> 0: Mode 3.1 (Not used) <br> 1: Quota mode 4.0 | **one**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction gloss affects savings` | 40 | ** Alphanumeric value (maximum) ** <br /> It must be printed on the voucher when it is different from empty <br> Field 9, subfield D | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Sequence number` | 9   | ** Numeric value ** <br /> This field is not being used, it is not printed <br> Also known as operation number | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Terminal message flag` | 1 | ** Alphanumeric value ** <br /> Y: The message is terminal and the response SPDH message should NOT be sent <br> N: The response SPDH message should be sent | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Long message` | 4         | **Numerical value**  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`SPDH Sale / Reverse Message`| 2048 | ** Alphanumeric value (maximum) ** | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Tip`       | 18        | ** Numerical value ** <br> Tip or Donation Amount | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Rejection Voucher` | 1024 | ** Numeric value ** <br> When transaction is declined by EMV a special voucher must be printed. <br> If this field is empty it is not printed, if it comes with data it is printed <br> This voucher is printed alone, without sales voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`PEL Voucher`| 1024      | ** Alphanumeric value ** <br> PEL voucher, if the box comes, you must print it, only once together with the sales voucher <br> It must not be printed in duplicate <br> This voucher is printed together with the sales voucher | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`LABEL - EMV`   | 32        | ** Alphanumeric value ** <br> If the field comes with box data, it must be included in the voucher in the indicated position | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`RID - EMV`     | 32        | ** Alphanumeric value ** <br> If the field comes with box data, it must be included in the voucher in the indicated position | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Pinpad model` | 6         | ** Numeric value ** <br> Box must be included in the voucher example: VX805 <br> Mandatory field | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Pinpad version` | 6     | ** Numeric value ** <br> Box must include it in the voucher example: 12.34A <br> Mandatory field | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Prepaid Balance`| 40        | ** Alphanumeric value (maximum) ** <br> Indicates the balance of a prepaid card which must be printed on a voucher when it is a prepaid sale and when the balance comes. <br> Note: The Pinpad adds this gloss to the voucher as it comes in the message. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

### 0560 - 0570 Validation / update request command

Only for standard retail wifi pinpad


### 0580 - 0590 Command Request with Surcharge capability

In this command 0580 the box must validate if the final answer (Terminal flag) and if it is approved (Code
Transbank response) to later print. If the transaction is not final (Flag terminal) you must resend the
SPDH message attached, it does not matter if the transaction is approved or not in this case.
Submit a voucher formatted for printing. In addition, it supports Prepaid and Surcharge cards.

**Request**



FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0580` <br> <i> hexadecimal value </i>:` 0x30 0x35 0x38 0x30` | ** 0580 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`| 16 | ** Alphanumeric value ** <br> Id delivered by the pinpad for each transaction | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Long message` | 4         | **Numerical value** | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`SPDH message`  | 2048      | ** Alphanumeric value (maximum) ** | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Trade Name`| 40       | ** Alphanumeric value ** <br> Boxed parametric field sent to pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Commerce Directorate`| 40    | ** Alphanumeric value ** <br> Boxed parametric field sent to pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Commerce Commune`| 40       | ** Alphanumeric value ** <br> Boxed parametric field sent to pinpad <br> May be commune or city | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

Maximum waiting time per command 540 of 125sec, since there are up to 4 interaction with the user.

**Answer**



FACT                | LENGTH     | COMMENTARY        | DEFAULT VALUE
------              | ------    | ------            | ------
`` <STX> ''             | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`           | 4         |  <i> ASCII value </i>: `0590` <br> <i> hexadecimal value </i>:` 0x30 0x35 0x39 0x30` | ** 0590 **
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`PinPad response code`| 2 | ** Numerical Value ** <br> In case of rejection, it must be displayed at the point of sale: <br> PINPAD REJECTION - `<XX>`: `<GLOSA>` <br> response codes / command reference table [] to-host # response-table-pinpad) | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`| 16     | ** Alphanumeric value ** <br> Id delivered by the pinpad for each transaction  | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Commercial code`| 12        | ** Numeric value ** <br> Merchant code delivered by TBK and configured at checkout, printed on voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Terminal ID`      | 16        | ** Alphanumeric value ** <br> Logical address delivered by TBK and configured in the box, it is printed on the voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Ticket / Ballot Number`| 20     | ** Alphanumeric value ** <br> Optional field, if it comes it is printed in the voucher if it doesn't come the field is omitted | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Employee`          | 4         | ** Alphanumeric value ** <br> Optional field, if it comes it is printed in the voucher if it doesn't come the field is omitted | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Authorization Code`| 8        | ** Alphanumeric value (maximum) ** <br> Transaction authorization code sent by TBK example: & # 124; AB 12 C3 & # 124; <br> What comes in the voucher is printed | 
`Separator`          | 1        |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount`             | 18        | ** Numeric value (maximum) ** <br /> Total authorized amount (includes the amount of the sale, tip, return and donation as the case may be) <br> It is printed on voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount returned`      | 18        | ** Numeric value (maximum) ** <br /> Return selected by customer, only applies in debit <br> It is printed in voucher| 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Number of Quotas`  | 2         | ** Numeric value ** <br> Amount of transaction fees (for sales without fees, “00” is reported) <br> It is printed on the voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount fee`       | 14        | ** Numeric value ** <br> If the reported amount is empty & # 124; & # 124; or & # 124; 0 & # 124; box must omit the entire line on the voucher. <br> It is printed in the voucher if the field comes |
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Last 4 Card Digits`| 4  | ** Numeric value ** <br> Does not print | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Operation Number`  | 9         | ** Terminal transaction map ** <br> Also known as the sequence number, this field must be printed on the sales and cancellation voucher. | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Card Type Gloss`| 7      | ** Alphanumeric value (maximum) ** <br> Gloss value in [Card type table] (/ reference / host-to-host # card-type-table) | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`` Accounting Date ``   | 6         | ** Alphanumeric value ** <br> It is used only if it is a Debit transaction <br> Cash must not be formatted (ex: DDAAMM), it simply must transfer the value to the voucher (XX / XX / XX) | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Account Number`  | 19        | ** Alphanumeric value ** <br> Masked card number to include in the voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Card abbreviation`| 2  | ** Alphanumeric value ** <br> Value to print on the voucher |
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction Date` | 6         | ** YYMMDD format ** <br> Value to print in the voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction Time`  | 6         | ** HHMMSS format ** <br> Value to print in the voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Print field`   | 8192      | ** Field depends if the box requires formatted voucher (maximum) ** <br> Voucher is always sent | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Winning transaction`| 1       | ** Numerical value ** <br> & # 124; 1 & # 124 ;: winning transaction <br> In this case, the cashier must print a PEL voucher in addition to the sales voucher <br> & # 124; & # 124 ;: transaction without a prize  | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promotion type`    | 1         | ** Numeric value ** <br> & # 124; 1 & # 124 ;: Delivery Point of Sale <br> & # 124; 2 & # 124 ;: Deferred Delivery <br> & # 124; 3 & # 124 ;: Return to Cardholder  | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promo code`  | 8         | ** Alphanumeric value ** <br> Value to print on the awarded voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promotion name`  | 21        | ** Alphanumeric value ** <br> Value to print on the award voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
Glosa voucher prize | 62        | ** Alphanumeric value ** <br> Value to print on the award voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Prize voucher text` | 27        | ** Alphanumeric value ** <br> Value to print on the award voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag allows quotas`| 1        | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag of grace`    | 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag C2C`          | 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag C3C`          | 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag NCuotas`      | 1         | ** Numeric value ** <br> Informational field of the trade configuration | ** 0 **
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Maximum quota flag`| 2      | ** Numeric value ** <br> Informational field of the trade configuration | ** 00 **
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Menu type`      | 2         | ** Alphanumeric value ** <br> Indicator of the type of menu for which the transaction was made <br> & # 124; CR & # 124; : CREDIT <br> & # 124; DB & # 124; : PREPAID DEBIT <br> & # 124; NB & # 124; : NON-BANK <br> Type value in [Card type table] (/ reference / host-to-host # card-type-table) <br> A sale made as debit can be authorized as prepaid | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Graceful transaction indicator`| 1 | ** Numeric value ** <br> Transaction mode indicator <br> & # 124; 0 & # 124; transaction without grace month <br> & # 124; 1 & # 124; grace month transaction | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Type of installments`       | 1         | ** Numeric value ** <br> & # 124; 0 & # 124; No fees <br> & # 124; 1 & # 124; Normal odds <br> & # 124; 3 & # 124; C3C or C2C <br> & # 124; 4 & # 124; CIC or N-installments | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Applied rate`     | 4         | ** Numeric value ** <br> It is only printed in the voucher if "Flag print rate = 1" | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Fee-type gloss`  | 30        | ** Alphanumeric value ** <br> Gloss to print on voucher <br> If the reported field is empty "& # 124; & # 124;" box should omit the line on the voucher. | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Fee-type gloss 2`| 22        | ** Alphanumeric value ** <br> Gloss that is displayed in pinpad | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Promotion gloss`   | 10        | ** Alphanumeric value ** <br> Gloss that is displayed in pinpad | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Id promotion`      | 10        | ** Alphanumeric value ** <br> Gloss that is displayed in pinpad | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag print rate`| 1         | ** Numeric value ** <br> & # 124; & # 124; or & # 124; 0 & # 124; does not print applied rate <br> & # 124; 1 & # 124; print applied rate | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred period`  | 3         | ** Numeric value ** <br> Deferred period selected, value to print on voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 1 period`| 3         | ** Numeric value ** <br> Not used | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 1 rate value`| 4      | ** Numeric value ** <br> Not used | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 1 installment value`| 14    | ** Numeric value ** <br> Not used | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 2 period`| 3         | ** Numeric value ** <br> Not used | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 2 value rate`| 4      | ** Numeric value ** <br> Not used | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 2 installment value` | 14   | ** Numeric value ** <br> Not used | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 3 period`| 3         | ** Numeric value ** <br> Not used | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 3 value rate`| 4      | ** Numeric value ** <br> Not used | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Deferred 3 installment value`| 14    | ** Numeric value ** <br> Not used | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Original transaction sequence number` | 9 | ** Alphanumeric value (maximum) ** <br /> Also known as the original transaction number of the sale. This field is not being used, it is not printed | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transbank response code` | 3| ** Numeric value ** <br /> Response code after the transaction is completed. It must be displayed at the point of sale. <br> EJ: TRANSBANK RESPONSE - `<XXX>`: `<GLOSA>` | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transbank answer gloss` | 48| ** Alphanumeric value (maximum) ** <br /> Gloss that displays the pinpad once the transaction is completed. It must be displayed at the point of sale. <br> EJ: TRANSBANK RESPONSE - `<XXX>`: `<GLOSA>`| 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction flag with PIN` | 1  | ** Alphanumeric value ** <br /> Y: Transaction authenticated with PIN <br> N: Transaction authenticated by signature | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Cardholder name` | 26   | ** Alphanumeric value ** <br /> Only print if "Flag type voucher = 1, 2 or 3" | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Voucher flag` | 1         | ** Numeric value ** <br /> Depending on the number received, a voucher must be printed with or without signature: <br> & # 124; 0 & # 124; = Unsigned <br> & # 124; 1 & # 124; or & # 124; 2 & # 124; or & # 124; 3 & # 124; = with signature <br> <br> ** Voucher headers: ** <br> For credit sales: “CREDIT SALE” <br> For debit sales (always without signature): “DEBIT SALE” with NOBANK sales a0342f BANK SALE: "<br> For prepaid sales (without signature):" PREPAID SALE "<br> For cancellations with credit (without signature):" CREDIT CANCELLATION "<br> For cancellations with no bank (without signature):" NON-BANK CANCELLATION " | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag quota mode` | 1     | ** Alphanumeric value ** <br /> 0: Mode 3.1 (Not used) <br> 1: Quota mode 4.0 | **one**
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transaction gloss affects savings` | 40 | ** Alphanumeric value (maximum) ** <br /> It must be printed on the voucher when it is different from empty <br> Field 9, subfield D | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Sequence number` | 9       | ** Numeric value ** <br /> This field is not being used, it is not printed <br> Also known as operation number | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Terminal message flag` | 1     | ** Alphanumeric value ** <br /> Y: The message is terminal and the response SPDH message should NOT be sent <br> N: The response SPDH message should be sent | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Long message`     | 4         | **Numerical value**  | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`SPDH Sale / Reverse Message` | 2048 | ** Alphanumeric value (maximum) ** | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Tip`           | 18        | ** Numerical value ** <br> Tip or Donation Amount | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Rejection Voucher`| 1024      | ** Numeric value ** <br> When transaction is declined by EMV a special voucher must be printed. <br> If this field is empty it is not printed, if it comes with data it is printed <br> This voucher is printed alone, without sales voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`PEL Voucher`    | 1024      | ** Alphanumeric value ** <br> PEL voucher, if the box comes, you must print it, only once together with the sales voucher <br> It must not be printed in duplicate <br> This voucher is printed together with the sales voucher | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`LABEL - EMV`       | 32        | ** Alphanumeric value ** <br> If the field comes with box data, it must be included in the voucher in the indicated position | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`RID - EMV`         | 32        | ** Alphanumeric value ** <br> If the field comes with box data, it must be included in the voucher in the indicated position | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Pinpad model`     | 6         | ** Numeric value ** <br> Box must be included in the voucher example: VX805 <br> Mandatory field | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Pinpad version` | 6         | ** Numeric value ** <br> Box must include it in the voucher example: 12.34A <br> Mandatory field | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Prepaid Balance`     | 40        | ** Alphanumeric value (maximum) ** <br> Indicates the balance of a prepaid card which must be printed on a voucher when it is a prepaid sale and when the balance comes. <br> Note: The Pinpad adds this gloss to the voucher as it comes in the message. | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
Surcharge amount | 12        | ** Numeric value ** <br> Amount associated with Surcharge. Include 2 decimal places. <br> If the reported amount is empty & # 124; & # 124; or & # 124; 0 & # 124; box must omit the entire line on the voucher. <br> It is printed on the voucher if the field comes. <br> To make subsequent cancellations, this field must be added to the total amount of the sale. | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Surcharge Language`  | 2         | ** Alphanumeric value ** <br> The language in which the voucher will be issued is reported. <br> ES: Spanish <br> EN: English | 
`Separator`         | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`             | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`             | 1         | Byte result of the message XOR operation | 

## Commands update pinpad parameters (batch closure)

### 0600 - Pinpad parameter update command request

The batch closure is a transaction that is used to load parameters in the pinpad as configured
in Transbank for commerce (ex: service code)
This closing should be executed at the beginning of the day, ideally automatically. It can also be run on
manually, and this also serves to verify if there is communication with Transbank, the result must be
an approved transaction displayed on the pinpad screen and at checkout.

**Request**



FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0600` <br> <i> hexadecimal value </i>:` 0x30 0x36 0x30 0x30` | ** 0600 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Commercial code`  | 12  | ** Numeric value ** <br> Merchant code delivered by TBK and configured at checkout. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Terminal ID`    | 16       | ** Alphanumeric value ** <br> Logical address delivered by TBK and configured in the box. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Internal trade index`| 16 | ** Alphanumeric value (maximum) ** <br> Field that can be used by the merchant to add information that is useful to its internal processes. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

Maximum waiting timeout per command 610 of 20sec, does not require interaction with card holder.

**Answer**


FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0610` <br> <i> hexadecimal value </i>:` 0x30 0x36 0x31 0x30` | ** 0610 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Response code`  | 2  | ** Numerical Value ** <br> In case of rejection, it must be displayed at the point of sale: <br> PINPAD REJECTION - `<XX>`: `<GLOSA>` <br> response codes / command reference table [] to-host # response-table-pinpad) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Internal trade index`| 16 | ** Alphanumeric value (maximum) ** <br> Field that can be used by the merchant to add information that is useful to its internal processes. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Long message` | 4         | **Numerical value**  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Message SPDH Close batch` | 2048 | ** Alphanumeric value (maximum) ** | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 


### 0700 - Validation of pinpad parameter update command

**Request**


FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0700` <br> <i> hexadecimal value </i>:` 0x30 0x37 0x30 0x30` | ** 0700 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Long message` | 4         | **Numerical value**  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Message SPDH Close batch` | 2048 | ** Alphanumeric value (maximum) ** | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

Maximum waiting timeout per command 710 of 20sec, does not require interaction with card holder.

**Answer**


FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0710` <br> <i> hexadecimal value </i>:` 0x30 0x37 0x31 0x30` | ** 0710 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Response code`  | 2  | ** Numerical Value ** <br> In case of rejection, it must be displayed at the point of sale: <br> PINPAD REJECTION - `<XX>`: `<GLOSA>` <br> response codes / command reference table [] to-host # response-table-pinpad) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transbank response code`| 3 | ** Numeric Value ** <br> TBK response code. It must be displayed at the point of sale. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Transbank answer gloss` | 48 | ** Alphanumeric value (maximum) ** <br /> Gloss that displays the pinpad once the transaction is completed. It must be displayed at the point of sale. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Sales transaction quantity`| 4 | **Numerical value**  |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount of sale transactions` | 18 | **Numerical value**  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount of cancellation transactions` | 4 | **Numerical value**  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount of cancellation transactions` | 18 | **Numerical value**  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 



## ONUS Sales Commands

### 0800 - 0810 ONUS sale request

Only for retail with own cards

### 0900 - 0910 ONUS sale validation

Only for retail with own cards

## ADMN - Administrative Commands

These commands are currently only enabled for self-service terminals with equipment
Verifone ux300, ux100, ux400.
The administrative commands are described below, which have mandatory fields and others do not: <br> X = Mandatory <br> O = Optional <br> Z = deprecated

### Eco BOX - PINPAD

Command sent from the Box to the pinpad to verify that the pinpad is connected and
available, it must be shipping in a configurable time at the checkout for example every 5 minutes. East
command also serves to establish the connection between pinpad and box. The pinpad opens a socket
connection which is not closed usually, but before a new request closes the previous socket and opens another.

**Request**

FACT            | LENGTH     | COMMENTARY        | REQUIRED             | DEFAULT VALUE
------          | ------    | ------            | ------                | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | X | STX
`Command`       | 4         |  <i> Value </i>: `ADMN`                                                 | X | ADMN
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Command type`  | 2         | Numeric format <br> 01: pinpad connection echo <br> 02: reset pinpad <br> 03: update pinpad parameters | X | 01
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Date`         | 8         | Current date set in box (YYYYMMDD) 20171231                  | X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Hour`          | 6         | Time in 24hr format (HHMMSS) 246060                                  | X |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Box version` | 16      | Identify the version of the store's point of sale <br> EJ: NEWPOS 123.456 | X |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Box identifier` | 16| Identify the point of sale of the business <br> EJ: branch 123 box 456| X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte result of the message XOR operation                              | X | STX

**Answer**

FACT            | LENGTH     | COMMENTARY        | REQUIRED             | DEFAULT VALUE
------          | ------    | ------            | ------                | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02`       | X | STX
`Command`       | 4         |  <i> Value </i>: `ADMN`                                                       | X | ADMN
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Command response`  | 2    | Numeric Value <br> Command response code, according to [Command response code table] (/ reference / host-to-host # pinpad-response-table) |  X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`PinPad identifier`| 20  | Alphanumeric value Pinpad model brand <br> EJ: VERIFONE UX300    | X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`PinPad version`| 20       | Alphanumeric value <br> Version of the application loaded in pinpad <br> ex: 15.20A TBK20171231 | X |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`PinPad Serial Number` | 20  | Alphanumeric value <br> PINPAD serial number <br> EJ: 123-123-123-123     | X |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Box identifier`| 16 | Identify the point of sale of the business <br> EJ: branch 123 box 456| X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Date`         | 8         | Current date set in box (YYYYMMDD) 20171231                        | X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Hour`          | 6         | Time in 24hr format (HHMMSS) 246060                                        | X |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte result of the message XOR operation                              | X | STX

### Restart Pinpad (Box - Pinpad)

Command with which the box asks the pinpad to reboot, pinpad responds ok and reboots.
This command can be sent for example every day at 4:00 AM it should not be sent when starting the
box or point of sale of the store since at that moment the update of the parameters of the
pinpad or (close batch).

**Request**



FACT            | LENGTH     | COMMENTARY        | REQUIRED             | DEFAULT VALUE
------          | ------    | ------            | ------                | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | X | STX
`Command`       | 4         |  <i> Value </i>: `ADMN`                                                 | X | ADMN
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Command type`  | 2         | Numeric format <br> 01: pinpad connection echo <br> 02: reset pinpad <br> 03: update pinpad parameters | X | 02
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Date`         | 8         | Current date set in box (YYYYMMDD) 20171231                  | X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Hour`          | 6         | Time in 24hr format (HHMMSS) 246060                                  | X |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte result of the message XOR operation                              | X | STX


**Answer**

FACT            | LENGTH     | COMMENTARY        | REQUIRED             | DEFAULT VALUE
------          | ------    | ------            | ------                | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02`       | X | STX
`Command`       | 4         |  <i> Value </i>: `ADMN`                                                       | X | ADMN
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Command response`| 2      | Numeric Value <br> Command response code, according to [Command response code table] (/ reference / host-to-host # pinpad-response-table) |  X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Date`         | 8         | Current date set in box (YYYYMMDD) 20171231                        | X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Hour`          | 6         | Time in 24hr format (HHMMSS) 246060                                        | X |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte result of the message XOR operation                              | X | STX


### Pinpad parameters update (Pinpad - Box)

Command with which the pinpad requests the box to trigger a parameter update or batch closure,
Using the usual flow (600-700 commands), cash starts the standard flow 600, 610, 700, 710.

**Request**


FACT            | LENGTH     | COMMENTARY        | REQUIRED             | DEFAULT VALUE
------          | ------    | ------            | ------                | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | X | STX
`Command`       | 4         |  <i> Value </i>: `ADMN`                                                 | X | ADMN
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Command type`  | 2         | Numeric format <br> 01: pinpad connection echo <br> 02: reset pinpad <br> 03: update pinpad parameters | X | 03
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Date`         | 8         | Current date set in box (YYYYMMDD) 20171231                  | X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Hour`          | 6         | Time in 24hr format (HHMMSS) 246060                                  | X |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte result of the message XOR operation                              | X | STX

**Answer**

FACT            | LENGTH     | COMMENTARY        | REQUIRED             | DEFAULT VALUE
------          | ------    | ------            | ------                | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02`       | X | STX
`Command`       | 4         |  <i> Value </i>: `ADMN`                                                       | X | ADMN
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Command response`  | 2    | Numeric Value <br> Command response code, according to [Command response code table] (/ reference / host-to-host # pinpad-response-table) |  X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Date`         | 8         | Current date set in box (YYYYMMDD) 20171231                        | X | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`Hour`          | 6         | Time in 24hr format (HHMMSS) 246060                                        | X |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| X | & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`       | X | STX
`<LRC>`         | 1         | Byte result of the message XOR operation                              | X | STX


### 1600 - 1610 Read Bar Code Command

** Important: The barcodes supported by the e355 pinpad are: CODE39, CODE128, EAN and UPC. \ * Only for the model with a Verifone e355 barcode reader. **

Command to start the barcode capture from the Pinpad model E355.

**Request**

FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `1600` <br> <i> hexadecimal value </i>:` 0x31 0x36 0x30 0x30` | ** 1600 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Timeout Command`  | 2      | ** Numerical value ** <br> 01-99 Time in seconds, to keep the barcode reading on the pinpad. | **twenty**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Display line 1` | 21   | ** Alphaumeric value ** <br> First line of message to Display on the Pinpad to indicate the beginning of the Bar Code Reading. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Display line 2` | 21   | ** Alphaumeric value ** <br> Second line of message to Display on the Pinpad to indicate the beginning of the Bar Code Reading. | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | 

Maximum waiting time per command 1610 is the one configured in the request (1600), if this time is met, the Pinpad returns a 1610 & # 124; 99.

**Answer**

FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `1610` <br> <i> hexadecimal value </i>:` 0x31 0x36 0x31 0x30` | ** 1610 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Response code`  | 2  | ** Numerical Value ** <br> In case of rejection, it must be displayed at the point of sale: <br> PINPAD REJECTION - `<XX>`: `<GLOSA>` <br> response codes / command reference table [] to-host # response-table-pinpad) | ** 00 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Bar Code`  | 150    | ** 0 Variable long alphanumeric value ** <br> Bar code captured by the pinpad | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation |  



## Annexes

### Brand Table

NAME CARD BRAND         | CARD ABBREVIATION
------                          | -----------
VISA                            | SAW
MASTERCARD                      | MC
DINERS                          | DC
AMEX                            | AX
DISCOVER                        | DS
MAGNA                           | MG
PRESTO                          | TP
MAS (cencosud)                  | TM
CMR                             | TC
RIPLEY                          | TR
TEACHER                         | MT
ELECTRON                        | THE
DEBIT                          | DB
PREPAID                         | PP

### Card type table

CARD TYPE CODE          | GLOSS TYPE OF CARD
------                          | -----------
CR                              | CREDIT
DB                              | DEBIT - PREPAYMENT
NB                              | NO BANK
Null (empty)                    | Menu in PINPAD is displayed.

### Pinpad response code table to commands

These response codes to the commands sent to the pinpad, only in case of rejection (other than 00), are
should be displayed on the screen of the store's point of sale, in case of problems they help to identify the
cause.

RESPONSE CODE         | GLOSS
------                      | -----------
00                          | ** ANSWER OK **
-                          | TO BE DEFINED (CONSIDER AS IF IT WERE 99)
81                          | TIMEOUT FOR LESS THAN 30 SECONDS *
82                          | COMMAND INVALID
83                          | THERE IS NO MESSAGE CODE
84                          | CARD NOT SUPPORTED
85                          | REVERSE APPLIED
86                          | READ ERROR
87                          | PINPAD WITHOUT MASTER KEY
88                          | CARD DOES NOT ALLOW ONUS SALE
89                          | CHIP CARD DECLINED TRANSACTION
90                          | CARD NOT ALLOWED FOR THE SELECTED MODE
91                          | QUOTA AMOUNT ERROR
92                          | DOES NOT MATCH WITH THE FIRST “TAPEO” CARD
93                          | MINIMUM AMOUNT ERROR
94                          | VALIDATION ERROR RETURN AMOUNT
95                          | CONTEXT ID ERROR
96                          | THE LAST 4 DIGITS DO NOT MATCH
97                          | THE TRANSACTION DOES NOT ALLOW A REVERSE
98                          | MESSAGE FORMAT ERROR
99                          | CANCELLATION BY THE \ [CANCEL \] / TIMEOUT KEY

** Valid for the version of the application configured for Bencineras. *

### Some response codes from authorizers

These are just some of the response codes from the authorizers whether approved or rejected, there are many more but these are the most common.
When receiving a response to the transaction, whether it is approved or rejected, the 3 digits and the gloss delivered must be displayed on the store's point of sale screen, in case of problems they help to identify the cause, without having to search for the cash register. :

RESPONSE CODE         | GLOSS
------                      | -----------
000 - 009                   | PASSED
050                         | GENERAL REFUSAL OF THE BANK
056                         | PRODUCT NOT SUPPORTED
064                         | Retrying
066                         | PRODUCT NOT SUPPORTED
076                         | INSUFFICIENT FUNDS
082                         | EXCEED MAXIMUM
083                         | EXCEED MAXIMUM
085                         | INSUFFICIENT FUNDS
087                         | EXCEED MAXIMUM
095                         | EXCEED MAXIMUM
105                         | PRODUCT NOT SUPPORTED
106                         | EXCEED MAXIMUM
107                         | EXCEED MAXIMUM
109                         | EXCEED MAXIMUM
110                         | Retrying
112                         | EXCEED MAXIMUM
150                         | ERROR IN THE COMMERCE CODE OR IN THE BOX OR IN THE TBK ENVIRONMENT
201                         | INVALID KEY
202                         | EXCEED MAXIMUM
215                         | EXCEED MAXIMUM
217                         | Retrying
218                         | Retrying
219                         | INVALID MENU
251                         | PRODUCT NOT SUPPORTED
820                         | ERROR IN THE DDLL OR IN THE BOX OR IN THE TBK ENVIRONMENT
908                         | EXCEED MAXIMUM
950                         | BAD TRADE CONFIGURED IN TBK
964                         | Retrying
999                         | REJECTED

# ONUS ANNEX

## Messaging protocol according to type of communication

### Serial communication protocol (RS232 interface)
IDENTICAL TO STANDARD

### Generic script diagram
IDENTICAL TO STANDARD

### Detailed sales flow
DOES NOT APPLY

### Context management

! [Context handling] (/ images / documentation / host2host / context-handling-onus.png)

## Command Description
This chapter details each command that can be sent to the PINPAD.

For serial communication it is necessary to indicate the start and end characters that are sent in each command, which will be indicated by means of the following nomenclature:

CONTROL CHARACTER     | NOMENCLATURE
-------------------     | ------------
`` <STX> <DATA> <ETX> <LRC> '' | STXETX

In order to maintain the same language in TCP-IP communication, these
STX, ETX and LRC characters.

### ONUS command flow

! [Onus command flow] (/ images / documentation / host2host / onus-command-flow.png)

The merchant's cash register could use the 800 command more than once incorporating the context indicator, for example, in the first it only reads the card, in the second it asks to confirm the amount, in the third it requests a pin. Ideally, you should use a single 800 command to request all, read card, confirm amount, and request pin.

### 0800 - Sales / cancellation request command

With this command the cash register asks the pinpad to read a card, confirm the amount and ask for a pin. The box can make a request with all the requirements or several separate requests.

**Request**

FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0800` (Required) <br> <i> hexadecimal value </i>:` 0x30 0x38 0x30 0x30` | ** 0800 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Command context indicator`  | 16 | ** Alphanumeric value (optional) ** <br> Format yyyymmddhhmmssmm <br> It is only an ID, the date and time in the pinpad may be out of date <br> If this command comes from another and is part of the same transaction, the ID must be kept |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Local commerce OnUs`  | 2  | ** Numeric value (mandatory) ** <br> (00 -> 99) <br> Each local onus has its defined ID | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Working key track` | 32    | ** Alphanumeric value (optional) ** <br /> Contains the 3DES key used to encrypt the TRACK (ø: not encrypted) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Working key PIN` | 32     | ** Alphanumeric value (optional) ** <br /> Contains the 3DES key used to encrypt the TRACK (ø: not encrypted) | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount`         | 18        | ** Numeric value (required) ** <br> (includes two decimal places) <br> Variable length  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`List of change amounts` |60| ** Alphanumeric value (optional) ** <br> Field not used at the moment send empty <br> Variable length  | **OR**
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Amount returned`  | 18        | ** Numeric value (optional) ** <br> & # 124; 0 & # 124; Transaction without return <br> Field not used at the moment send zero <br> Variable length  | ** 0 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag requests confirm amount` | 1 | ** Alphanumeric value (required) ** <br> & # 124; Y & # 124; Request to confirm amount <br> & # 124; N & # 124; Do not ask to confirm amount  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Gloss 1 confirms amount`| 21| ** Alphanumeric value (optional) ** <br> Only if "Request confirms amount = Y" <br> Variable length  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Gloss 2 confirms amount`| 21| ** Alphanumeric value (optional) ** <br> Only if "Request confirms amount = Y" <br> Variable length  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Gloss 3 confirms amount`| 21| ** Alphanumeric value (optional) ** <br> Only if "Request confirms amount = Y" <br> Variable length  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Gloss 4 confirms amount`| 21| ** Alphanumeric value (optional) ** <br> Only if "Request confirms amount = Y" <br> Variable length  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Authentication request indicator` | 2 | ** Numeric value (required) ** <br> & # 124; 00 & # 124; Ask for PIN to all events <br> & # 124; 01 & # 124; It does not ask for a PIN unless the EMV card indicates it <br> & # 124; 02 & # 124; Request PIN according to service code <br> & # 124; 03 & # 124; Asks for PIN according to service code, but accepts null pin <br> & # 124; 04 & # 124; Never ask for PIN  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | LRC

### 0810 - Sales / cancellation request command

With this command, the pinpad delivers to the box the data obtained in the request for command 800, such as the tracks of the card (in clear or encrypted), the entered pin (which is always delivered encrypted) and the EMV data that must be send to the ONUS authorizer.

**Answer**

FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0810` (Required) <br> <i> hexadecimal value </i>:` 0x30 0x38 0x31 0x30` | ** 0810 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Response code`  | 2  | ** Numeric value ** <br> According to [Command response code table] (/ reference / host-to-host # onus-command-response-code-table) |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator` |16 | ** Alphanumeric value ** <br> Format yyyymmddhhmmssmm <br> It is just an ID, the date and time in the pinpad may be out of date | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`` Type of capture ` | 2      | ** Numeric value ** <br /> & # 124; 00 & # 124; : B - Band <br> & # 124; 01 & # 124; : E. EMV w / contact <br> & # 124; 02 & # 124; : C - Contacless <br> & # 124; 03 & # 124; : F - Fallback | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`TRACK I`       | 80        | ** Alphanumeric value ** <br /> This field is optional, therefore it can contain data or be empty. <br> If there is data and it is necessary, it is filled with blanks <br> (0x20) to the right <br> With encrypted bread, 160 alphanumeric characters are delivered corresponding to 80 HEXA <br> - If the empty pinpad cannot read a trackpad, this field will not be able to read a track. <br> - If the pinpad reads bad data on track1, this field will be empty. <br> - If the maximum length is exceeded, this field will be empty. |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`TRACK II`      | 40        | ** Alphanumeric value (maximum) ** <br> Track2 is a required value for any sale, therefore if it cannot be read or errors are read, a read error is delivered to the command. <br> If the data is obtained correctly from the card, it is filled with blanks (0x20) to the right if necessary and delivered. <br> With encrypted bread, 80 alphanumeric characters are delivered corresponding to 40 HEXA  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`BIN`           | 6         | ** Numeric value ** <br> First six digits of the card | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Last 4 digits`  | 4    | ** Numeric value ** <br> Last four digits of the card | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Cardholder name` |26| ** Alphanumeric value ** <br> This data is obtained from track1, therefore if track1 does not exist, the name of the cardholder is not delivered <br> Variable length  | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Card brand name`| 20 | ** Alphanumeric value ** <br> According to [Marks table] () | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Card abbreviation` | 2  | ** Alphanumeric value ** <br> According to [Marks table] () | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Pinblock`      | 16        | ** Alphanumeric value ** <br> PIN entered by the customer but encrypted with the keys provided by the box| 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Cryptogram`   | 999       | ** Alphanumeric value ** <br> ISO TLV format <br> Contains TAG and cryptogram required by the issuer to authorize transactions carried out with EMV technology and security <br> Variable length | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | LRC

### 0900 - Authorizing response validation command

With this command, the box delivers to the pinpad the result of the transaction answered by the onus authorizer, whether it is approved or rejected.
In addition, if the sale was made with a chip, the cryptogram generated by the onus authorizer must be delivered to the card that is still inserted, for evaluation.
As of pinpad version 15.2, with this command the pinpad, in addition to displaying “REMOVE CARD” on the screen, also emits an alert sound.

**Request**


FACT            | LENGTH     | COMMENTARY        | DEFAULT VALUE
------          | ------    | ------            | ------
`` <STX> ''         | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`       | 4         |  <i> ASCII value </i>: `0900` (Required) <br> <i> hexadecimal value </i>:` 0x30 0x39 0x30 0x30` | ** 0900 **
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Context indicator`  |16| ** Alphanumeric value ** <br> Format yyyymmddhhmmssmm <br> It is only an ID, the date and time in the pinpad may be out of date <br> If this command comes from another and is part of the same transaction, the ID must be kept | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Flag validates cryptogram` |1| ** Alphanumeric value ** <br> & # 124; Y & # 124; Validate cryptogram <br> & # 124; N & # 124; Do not validate cryptogram <br> Only applicable for transactions with a contact chip | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Glossy answer`       | 21| ** Alphanumeric value (maximum) ** <br /> Only if "Flag validates cryptogram = N" <br> Example: Approved / Rejected / Retry |  
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Cryptogram`   | 999       | ** Alphanumeric value ** <br> ISO TLV format <br> ISO TLV format <br> Only if “Flag validates cryptogram = Y” <br> This cryptogram is delivered by the pinpad to the card chip for validation. <br> It could result in a transaction declined by the card for which the cashier must request a reversal from the ONUS authorizer | 
`Separator`     | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`         | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`         | 1         | Byte result of the message XOR operation | LRC

### 0910 - Authorizing response validation command

First of all, this command is the ok (or nook) response to the 900 command request. The main objective of this command is for the Pinpad to deliver the result of the evaluation of the cryptogram sent by the authorizer, the result can be Approved or Declined by the card, so that the case of decline, the box reverses the approved sale.

**Answer**


FACT                   | LENGTH     | COMMENTARY        | DEFAULT VALUE
------                 | ------    | ------            | ------
`` <STX> ''                | 1         | Indicates start of text or command <br> <i> hex value </i>: `0x02` | STX
`Command`              | 4         |  <i> ASCII value </i>: `0910` (Required) <br> <i> Hexadecimal value </i>:` 0x30 0x39 0x31 0x30` | ** 0910 **
`Separator`            | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Response code`  | 2         | ** Numeric value ** <br> According to [Command response code table] (/ reference / host-to-host # onus-command-response-code-table) |  
`Separator`            | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`Glossy answer`      | 21        | ** Alphanumeric value ** <br> According to [Command response code table] (/ reference / host-to-host # onus-command-response-code-table). Only if "Flag validates cryptogram = Y" | 
`Separator`            | 1         |  <i> ASCII value </i>: <code> & # 124; </code> <br> <i> hex value </i>: `0x7c`| & # 124;
`<ETX>`                | 1         | Indicates the end of text or command <br> <i> hexadecimal value </i>: `0x03`     | ETX
`<LRC>`                | 1         | Byte result of the message XOR operation | LRC


## Annexes

### Brand table

IDENTICAL TO STANDARD

### Card type table

IDENTICAL TO STANDARD

### ONUS command response code table

Additional response codes for OnUs mode are detailed


RESPONSE CODE     | GLOSS
--------------------    | -----
00                      | ANSWER OK
-                      | To be defined (consider as if it were 99)
83                      | THERE IS NO MESSAGE CODE
84                      | CARD NOT SUPPORTED
85                      | REVERSE APPLIED
86                      | READ ERROR
87                      | PINPAD WITHOUT MASTER KEY
88                      | CARD DOES NOT ALLOW ONUS SALE
89                      | CHIP CARD DECLINED TRANSACTION
90                      | CARD NOT ALLOWED FOR THE SELECTED MODE
91                      | QUOTA AMOUNT ERROR
92                      | DOES NOT MATCH WITH THE FIRST “TAPEO” CARD
93                      | MINIMUM AMOUNT ERROR
94                      | VALIDATION ERROR RETURN AMOUNT
95                      | CONTEXT ID ERROR
96                      | THE LAST 4 DIGITS DO NOT MATCH
97                      | THE TRANSACTION DOES NOT ALLOW A REVERSE
98                      | MESSAGE FORMAT ERROR
99                      | CANCELLATION BY THE \ [CANCEL \] or TIMEOUT KEY

### Local code ONUS

To support the use of non-financial transactions that require card reading without still having the amount1, a 50 offset is defined to the local code associated with the OnUs merchant, for example:

01 -> 51

LOCAL CODE ONUS    | LOCAL CODE ONUS NON-FINANCIAL TRANSACTIONS
--------------------    | ---------------------------------------------
NN                      | 50 + NN
