# Host to Host

Do you manage a business with a high number of transactions and a large number of premises, which require managing
centralized internal squares? Transbank has a good alternative for these requirements.
Host to Host, solution that implies a development in the application of the boxes of your business to integrate it to the
equipment provided by Transbank, connecting via dedicated link to our servers, which will deliver robustness and
great speed to the payment process and information flow.

## How does it work

Host to Host requires a high level of integration, which consists of the following stages:

1. **Development**
Your business must carry out the development according to the specifications that Transbank will give you.

2. **QA**
This process will verify that the integration development you did,
complies with those that we give you in the specifications, or if it requires any correction or improvement.

3. ** Pilot stage **
In a place that we agree on together, we take your pinpad host point of sale to production, where with real clients we will monitor its operation together, for a limited period of 2 weeks. We evaluate the results, and agree to overcrowding, also if any adjustments are required.

4. ** Massification **
We jointly build an installation plan for the points of sale.

 <aside class="notice">
For the development and testing of the business, Transbank delivers a set of tests to be executed and a set of physical cards for these cases, these cards only work in a development, certification or integration environment.
 </aside>

### Sales flow on Pinpad Host:

1. The customer delivers the products or services they want to buy to the seller.
2. The seller makes the sale in his cash system.
3. The seller asks the customer how they want to pay, cash, credit card, debit card and selects it at the checkout.
4. The box invokes the pinpad, passing it the amount of the sale and the credit or debit payment method
5. The client operates a card, selects fees if applicable, and enters their pin
6. The pinpad assembles the sales request message and delivers it to the checkout counter.
7. The box takes the message and delivers it to the trade host and this in turn to the Transbank host (hence its host to host name)
8. The response message goes the reverse way until it reaches the pinpad
9. The pinpad verifies the approved or rejected response and the integrity of the message and delivers the result to the box
10. The cashier prints the sales receipts.

## Available equipment and connections

### Verifone vx805

Fixed USB or Serial Pinpad host
 <img src="/images/documentacion/host2host/vx805.png" alt="Verifone vx805">

 <strong> Driver </strong>

These devices work with both RS232 and USB serial ports (generally plug and play), for which you may need to install a [verifone driver] (/ files / verifone.zip).

This driver is compatible with the following operating systems, reported by Verifone:
* 32/64-bit Windows 10
* Windows 8 / 8.1 32/64 bit
* 32/64-bit Windows 7

By PCI standards, businesses must not use an operating system that is obsolete. It is also very important to keep your Operating System with the latest patches installed, this mainly for a security issue.

### Verifone vx680

Mobile equipment wifi Pinpad host
 <img src="/images/documentacion/host2host/vx680.png" alt="Verifone vx680">

### Verifone e355

Mobile Device Bluetooth Pinpad host
 <img src="/images/documentacion/host2host/e355.png" alt="Verifone e355">

## Payment features available on HOST

* Sale Credit, with or without fees, tip or donation
* Cancellation of sale Credit.
* Sale Debit, debit with change, tip or donation
* Sales in ONUS mode (business cards)

## How to start
Next, you will see the documentation associated with how to print the vouchers.
You can check the [Reference] (/ reference / host-to-host) to see the information associated with the communication and the commands.

## Voucher
The pinpad will deliver the voucher ready to print, but will also deliver all the data
necessary for the same box to build the voucher by itself.
If it is required to obtain the duplicate voucher, the box must build the voucher, without the PEL data and
adding duplicate gloss.

### Considerations
** Type of transactions **

These are the glosses that should be displayed in the header of the voucher:

Card Type         | Transaction Type
------                  | -----------
CR                      | SALE CREDIT
CR                      | CREDIT CANCELLATION
DB                      | DEBIT SALE
NB                      | NON-BANK SALE
NB                      | NON-BANK CANCELLATION
DB                      | PREPAID SALE
CR                      | RESERVATION
CR                      | CANCELLATION RESERVATION
CR                      | NO SHOW
CR                      | CANCELLATION NO SHOW
CR                      | CHECK IN
CR                      | CANCELLATION CHECK IN
CR                      | RE-AUTHORIZATION
CR                      | CANCELLATION RE-AUTHORIZATION
CR                      | CHECK OUT
CR                      | CANCELLATION CHECK OUT
CR                      | DELAYED CHARGE
CR                      | DELAYED CHARGE CANCELLATION
DB                      | CASH DEBIT WITHDRAWAL
CR                      | CASH CREDIT ADVANCE
CR                      | CANCELLATION OF EFFECTIVE CREDIT ADVANCE

** Card types **

Consider that the cards issued by commercial houses under a brand such as VISA or Master
they are considered bank and not non-bank cards.
Prepaid cards will be considered debit cards.

Code    | Gloss
------    | -----------
CR        | CREDIT
DB        | DEBIT
NB        | NO BANK
PR        | PREPAID

** Card brands **

Name     | Abbreviation
------     | -----------
VISA       | SAW
MASTERCARD | MC
AMEX       | AX
DINERS     | DC
DISCOVER   | DC
MAGNA      | MG
PRESTO     | TP
MORE        | TM
RIPLEY     | TR
RIPLEY     | RP
CMR        | TC
DEBIT     | DB
ELECTRON   | THE
TEACHER    | MT
PREPAID    | PR


### Print order
The idea of this voucher is that it be the only outlet for any type of Transbank voucher, at
be a unique voucher, regardless of the type of transaction they are always made the same
validations, to see if each field is printed or not, be it credit, debit or cancellation voucher
of credit.

Two copies of the voucher should always be printed, one for the merchant and one for the customer.
In the case of a voucher with signature, if the merchant so wishes, it may not print on the copy of the
store, the signature lines, customer name and rut, in the store voucher must be
signed if applicable.

The print order should be:
1. TBK award voucher
2. TBK Customer Voucher
3. TBK merchant voucher

Any variation to the order or format of the voucher must be authorized by Transbank

### Other considerations
Some considerations when printing Transbank vouchers
1. Vouchers must contain only the information requested in this manual. ** Any exception must be authorized and formalized by Transbank. **
2. Customer name can be printed only on signed voucher
3. Return lines should not be printed if they are not being used.
4. Donation or tip lines should not be printed if they are not being used.
5. ** Award vouchers should NOT be printed on self-copy paper **. If
   the customer only has printers that use carbonless paper, ** must be notified to
   Transbank of this situation, and it is the latter who must authorize the acceptance
   this type of printing, the functionality of promotions may be left out in
   line**.
6. Only ** one ** copy of an award voucher should be generated, against any condition of
   edge if the voucher could not be printed, the prize is lost, in the duplicate you should not
   figure nothing about it.
7. ** It is NOT possible to reprint the award voucher ** or make award allusions in the
   Transbank vouchers in case of printing a duplicate.
8. The award code and the glosses are delivered by the PINPAD after validating the
   response of the request.
9. The voucher printing ** MUST ** be done automatically, upon approval of the
   transaction, not optional or manual. It should not depend on a boxed enter to release the
   Print
10. In cases where the cardholder's signature is required, the ** MUST ** print
    suppose an adequate space for said operation.
11. The start of a new transaction ** MUST ** take place ** ONLY ** at the time of completion
    printing of the voucher of the previous transaction, ** NEVER ** during printing or
    reprint, since the transaction is considered finalized at the time of printing of the
    voucher or its reprint.
12. The separation between the vouchers (transaction (original / copy), award) can be
    physical (cut) or delimited by dotted lines.
13. The last thing to be printed is the Transbank voucher
14. The voucher in fiscal printer goes between the glosses. "START COMMENT" and "END
    COMMENTARY". Given the number of possible lines to print on this type of printer,
    the elimination of blank lines is allowed so that the voucher can be printed
    correctly. Each voucher could be separated by a beginning and end of comment, that is
    the pel voucher, the customer voucher and the merchant voucher.

### Sales voucher or cancellations
! [] (/ images / documentation / host2host / voucher-sale.png)

### Sales voucher in English with surcharge
! [] (/ images / documentation / host2host / voucher-sale-surcharge.png)

### Pel voucher and reprint
It is only allowed to print 1 time the original voucher and the PEL voucher, if there is a problem with
the printing of the original voucher must be aborted and allowed to obtain a duplicate.
Original:

1. PEL voucher
2. Voucher 1 business / client
3. Voucher 2 merchant / client

Duplicates:
1. Voucher 1 business / customer "duplicate without pel gloss"
2. Voucher 2 merchant / customer "duplicate without pel gloss"

### Voucher pel 1 - Online Awards
This voucher should be printed only if:
** COMMAND ** H505 ** FIELD ** TYPE OF VOUCHER = Delivery Point of Sale

! [] (/ images / documentation / host2host / voucher-pel-1.png)

### Voucher pel 2 - Online Awards
This voucher should be printed only if:
** COMMAND ** H505 ** FIELD ** VOUCHER TYPE = | 2 | Deferred Delivery

! [] (/ images / documentation / host2host / voucher-pel-2.png)

### Voucher pel 3 - Online Awards
This voucher should be printed only if:
** COMMAND ** H505 ** FIELD ** VOUCHER TYPE = | 3 | Return to Cardholder

! [] (/ images / documentation / host2host / voucher-pel-3.png)

# ONUS ANNEX

### goals

This documentation describes the way of operating, the functionality and the detail of the messaging of a PINPAD TRANSBANK working under the OnUs mode.
The PINPAD application assumes the existence of an intelligent ECR (for example, a cash register) that will send the requirements to the PINPAD, so that it can process them and deliver the results when appropriate.

### Audience
To fully understand this documentation, it is necessary to have transactional knowledge and to know the functions commonly implemented in PINPADs used in banking transactions.
In particular, this manual is intended exclusively for ONUS merchants that have their own cards and use the Transabank pinpad to read their cards, but the transactions are carried out and authorized by themselves.

### Scope
Applies to Verifone vx805 equipment (Serial or USB connection)
Applies to Verifone e355 equipment (Bluetooth connection)

You can check the [Reference] (/ reference / host-to-host # annex-onus) to see the information associated with the communication and the commands.

# Technical specifications and special outputs

## Squaring and settlement
This section describes the quadrature and settlement files that are delivered to the
client who uses the Special Departures service.
Quadrature (reconciliation) files are flat files that contain the records
of the transactions (of sale and cancellation) carried out in a business within a
determined period.
Settlement files are flat files that contain credit records.
and withholdings made on the merchant account within a specified period.

### Distribution
Quadrature and settlement files are periodically placed in a box
SFTP created exclusively for the respective business, for it to download
to your own system.
The following table indicates the frequency and hours of placement of quadrature files
and settlement in the box:

Archive     | Period covered | Placement in box | File Copy Schedule
------      | -----------      | -----------           | ----------- 
Quadrature  | 00ºº hrs day - 00ºº hrs next day | Every day | 12:00 Tuesday 14:00
Settlement  | 00ºº hrs day - 00ºº hrs next day | Business day  | 12:00 Tuesday 14:00

### Box access
Merchants must use an application of the SFTP client type available in the
market for box access, some of these can be Filezilla, WinSCP, PuTTY,
MobaxTerm

The name of the files of the Special Outputs (SSEE) will be generated dynamically from the
data entered in the enrollment plus the process date

`` ''
 <Formato de Salida> _ <Periodo> _ <Agrupación Archivo de Salidas> _ <RutComercio ó CódigoComercio> _ <Tipo Conexion> _ <Agrupación de Transacciones> _ <Número Agrupación>
`` ''

**Output format**
* LDN: Debt settlement
* LCN: Credit settlement
* CDN: Square debit
* CCN: Credit squaring

**Period**
* Daily: A daily file is generated based on the process date
* Monthly: One file is generated per month. Date associated with the penultimate business day
of the month

** Grouping File of Outputs **
* RE: File with unique information associated with the RUT enrolled (Generates a file)
* CC: File with information separated by trade code of the transaction
(generates a different file for each trade code)

** Connection Type **
* A single file is generated. File information is not separated by type of
connection (when it is a single file it is not part of the name)
* When a file is generated for face-to-face connection type transactions
and not in person
o NP: Not in person
o PR: face-to-face

** Grouping of transactions **
* When it is grouped by RUT it is not included in the file name
* RB: Item

** Group Number **
* It will correspond to the number of the Enrolled Item.

### Quadrature Credit File (L3)
The following describes in detail the structure of the records of the data files.
squaring credit. Each file has a header record ("header"), then
a record for each transaction carried out in the period covered and at the end a record
standing (“footer”).
The credit quadrature file contains the detailed record of transactions
financial transactions made with a credit card. Contains both transactions
processed in person and not in person.

** Header **

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Dktt-Hr-Reg | "HR" | Alphanumeric | 2 | 2
Dktt-Hr-Date-Proc | Process Date <br > Format "YYMMDD" The day the file is processed is the business day immediately following the period covered. | Numeric | 6 | 8
Dktt-Hr-Hour-Proc | Process Time Format "HHMMSS" | Numeric | 6 | 14
Dktt-Hr-Glosa | Merchant Name <br > Merchant Fantasy Name | Alphanumeric | 25 | 39
Filler | Available |  Alphanumeric | 282 | 321

** Footer **

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Dktt-Tr-Reg | Fixed value "TR" | Alphanumeric | 2 | 2
Dktt-Tr-Fech-Proc | Process Date Format "YYMMDD" | Numeric | 6 | 8
Dktt-Tr-Hour-Proc | Process Time Format "HHMMSS" | Numeric | 6 | 14
Dktt-Tr-Cant-Reg | Total records | Numeric | 7 | 21
Dktt-Tr-Acum-Amount | Total. 11 integers 2 decimals | Numeric | 13 | 34
Dktt-Tr-Date-From | Txs minor date <br> Format "YYMMDD". Among the transactions recorded in the file, indicate the date on which the earliest was made.  | Numeric | 6 | 40
Dktt-Tr-Hour-From | Txs minor hour <br> Format "HHMMSS". Among the transactions registered in the file, it indicates the time in which the earliest was made.  | Numeric | 6 | 46
Dktt-Tr-Date-Until | Date greater than Txs. <br> Format "YYMMDD". Among the transactions recorded in the file, indicate the date on which the latest was made. | Numeric | 6 | 52
Dktt-Tr-Hour-Until | Txs major hour. <br> Format "HHMMSS". Among the transactions registered in the file, it indicates the time in which the latest was carried out. | Numeric | 6 | 58
Filler | Available | Alphanumeric | 263 | 321

**Detail**
The following table describes each of its fields:

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
DSK-DT-REG | Record Type Fixed value "DT" | Alphanumeric | 2 | 2
DSK-TYP | Transaction Type 0210: online sale 0420: reverse | Numeric | 4 | 6
DSK-TC | Transaction Code4 "10": Purchase "18": Purchase with change "30": Withholding | Numeric | 2 | 8
DSK-TRAN-DAT | Transaction Date <br> Format “YYMMDD”. Corresponds to the date on which Transbank issued a response to the transaction | Numeric | 6 | 14
DSK-TRAN-TIM |  Transaction Time <br> Format “HHMMSS”. Corresponds to the time in which Transbank issued a response to the transaction | Numeric | 6 | 20
DSK-ID-RETAILER | Provider Commerce Code | Numeric | 12 | 32
DSK-NAME-RETAILER | Merchant name <br> Merchant fantasy name | Alphanumeric | 20 | 52
DSK-CARD | Card Number <br> Only the last 4 digits appear, the others are masked with * | Alphanumeric | 19 | 71
DSK-AMT-1 | Purchase amount. 11 integers 2 decimals | Numeric | 13 | 84
DSK-AMT-2 | Return amount. 11 integers 2 decimals | Numeric | 13 | 97
DSK-AMT-TIP | Tip Amount. 7 integers 2 decimals | Numeric | 9 | 106
DSK-RESP-CDE | Response Code <br> Issued by Base-24. Values between 000 and 009 indicate an approved transaction. |  Alphanumeric |  |3 109
DSK-APPRV-CDE | Approval Code <br> "Or Authorization". Delivered by the Issuer. | Alphanumeric | 8 | 117
DSK-TERM-NAME | Terminal code POS Terminal ID | Alphanumeric | 16 | 133
DSK-ID-BOX | Box Identifier | Alphanumeric | 16 | 149
DSK-NUM-BALLOT | Ballot Number | Alphanumeric | 10 | 159
DSK-DATE-PAYMENT | Payment Date <br> 1 business day after the File Process date | Numeric | 6 | 165
DSK-IDENT | Host identifier <br> Corresponds to the "Merchant Identifier" that appears at the beginning of the name of the file name | Alphanumeric | 2 | 167
DSK-ID-RETAILER | Code of Responsible Commerce <br> Code of intermediary commerce (if any) between the Merchant and the cardholder | Numeric | 8 | 175
DSK-ID-COD-SERVI | Service Code | Alphanumeric | 20 | 195
DSK-ID-NRO-UNICO | Unique number <br> Assigned by the merchant | Alphanumeric | 26 | 221
DSK-PREPAYMENT | Prepaid <br> Value "P" for records that come from prepaid. Blank if it is a transaction for another product. | Alphanumeric | 1 | 222
FILLER | Available | Alphanumeric | 18 | 240

### Square debit file (L3)
The debit quadrature file contains the detailed record of the transactions
financial transactions made with a debit card. Contains both transactions
processed in person and not in person.

** Header **

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Dktt-Hr-Reg | Type of register | Alphanumeric | 2 | 2
Dktt-Hr-Date-Proc | Date of process | Numeric | 6 | 8
Dktt-Hr-Hour-Proc | Processing Time | Numeric | 6 | 14
Dktt-Hr-Glosa | Merchant Name <br > Merchant Fantasy Name | Alphanumeric | 25 | 39
Filler | Available |  Alphanumeric | 201 | 240

** Footer **

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
DSK-TR-REG | Type of register. Value "TR" | Alphanumeric | 2 | 2
DSK-TR-DATE-PROC | Date of process | Numeric | 6 | 8
DSK-TR-TIME-PROC | Processing time | Numeric | 6 | 14
DSK-TR-TOT-REG | Total Records | Numeric | 7 | 21
DSK-TR-MONTO | Total amount. 11 integers 2 decimals | Numeric | 13 | 34
DSK-TR-MONTO-COM | Total Commissions Amount. 11 integers 2 | Numeric | 13 | 47
FILLER | | Alphanumeric | 193 | 240

**Detail**

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
DSK-DT-REG | Record Type Fixed value "DT" | Alphanumeric | 2 | 2
DSK-TYP | Transaction Type 0210: online sale 0420: reverse | Numeric | 4 | 6
DSK-TC | Transaction Code4 "10": Purchase "18": Purchase with change "30": Withholding | Numeric | 2 | 8
DSK-TRAN-DAT | Transaction Date <br> Format “YYMMDD”. Corresponds to the date on which Transbank issued a response to the transaction | Numeric | 6 | 14
DSK-TRAN-TIM |  Transaction Time <br> Format “HHMMSS”. Corresponds to the time in which Transbank issued a response to the transaction | Numeric | 6 | 20
DSK-ID-RETAILER | Provider Commerce Code | Numeric | 12 | 32
DSK-NAME-RETAILER | Merchant name <br> Merchant fantasy name | Alphanumeric | 20 | 52
DSK-CARD | Card Number <br> Only the last 4 digits appear, the others are masked with * | Alphanumeric | 19 | 71
DSK-AMT-1 | Purchase amount. 11 integers 2 decimals | Numeric | 13 | 84
DSK-AMT-2 | Return amount. 11 integers 2 decimals | Numeric | 13 | 97
DSK-AMT-TIP | Tip Amount. 7 integers 2 decimals | Numeric | 9 | 106
DSK-RESP-CDE | Response Code <br> Issued by Base-24. Values between 000 and 009 indicate an approved transaction. |  Alphanumeric |  |3 109
DSK-APPRV-CDE | Approval Code <br> "Or Authorization". Delivered by the Issuer. | Alphanumeric | 8 | 117
DSK-TERM-NAME | Terminal code POS Terminal ID | Alphanumeric | 16 | 133
DSK-ID-BOX | Box Identifier | Alphanumeric | 16 | 149
DSK-NUM-BALLOT | Ballot Number | Alphanumeric | 10 | 159
DSK-DATE-PAYMENT | Payment Date <br> 1 business day after the File Process date | Numeric | 6 | 165
DSK-IDENT | Host identifier <br> Corresponds to the "Merchant Identifier" that appears at the beginning of the name of the file name | Alphanumeric | 2 | 167
DSK-ID-RETAILER | Code of Responsible Commerce <br> Code of intermediary commerce (if any) between the Merchant and the cardholder | Numeric | 8 | 175
DSK-ID-COD-SERVI | Service Code | Alphanumeric | 20 | 195
DSK-ID-NRO-UNICO | Unique number <br> Assigned by the merchant | Alphanumeric | 26 | 221
DSK-PREPAYMENT | Prepaid <br> Value "P" for records that come from prepaid. Blank if it is a transaction for another product. | Alphanumeric | 1 | 222
FILLER | Available | Alphanumeric | 18 | 240

### Credit settlement file (L5)
The credit settlement file contains the detailed record of the credits and
withholdings on the merchant account within a period determined by
credit card transactions.

** Header **

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Pass - From | Payment period start date ddmmyyyy | Numeric | 8 | 8
Filler | Stuffed | Alphanumeric | 1 | 9
Subscription - Until | End date of payment period ddmmyyyy | Numeric | 8 | 17
Filler | Stuffed | Alphanumeric | 1 | 18
Process - Date | Process date, in ddmmyyyy format | Numeric | 8 | 26
Filler | Stuffed | Alphanumeric | 1 | 27
Payment - Date | Transaction payment date, in ddmmayyy format | Numeric | 8 | 35

IF length of client code is less than 4

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Client code |  Internal customer number | Numeric | 3 | 38
Customer name | Trade name | Alphanumeric | 20 | 58
Filler | Stuffed | Alphanumeric | 76 | 134
Filler | Contains "HEADER" | Alphanumeric | 6 | 140
Filler | Stuffed | Alphanumeric | 89 | 229

IF length of client code is greater than 3

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Client code |  Internal customer number | Numeric | 5 | 40
Customer name | Trade name | Alphanumeric | 20 | 60
Filler | Stuffed | Alphanumeric | 74 | 134
Filler | Contains "HEADER" | Alphanumeric | 6 | 140
Filler | Stuffed | Alphanumeric | 89 | 229

** Footer **

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Salt - Counter | Number of transactions to be paid | Numeric | 10 | 10
Filler | Stuffed  | Alphanumeric | 1 | 11
Salt - Amount | Total amount of sales to be paid (11 integers, 2 decimals) | Numeric | 13 | 24
Filler | Stuffed  | Alphanumeric | 1 | 25
Salt - Rret. | Number of withholdings | Numeric | 10 | 35
Filler | Stuffed  | Alphanumeric | 1 | 36
Salt - Ret. | Total amount of withholdings (11 integers, 2 decimals) | Numeric | 13 | 49
Filler | Stuffed  | Alphanumeric | 1 | 50
Salt - Ceic | Total amount of commission plus VAT of the commission (Sale) (11 integers, 2 decimals) | Numeric | 13 | 63
Salt - caeica | Total amount of additional commission plus additional VAT of the commission (11 integers, 2 decimals) | Numeric | 13 | 76
Salt - dceic | Total amount of commission plus VAT of the commission (Withholdings) (11 integers, 2 decimals) | Numeric | 13 | 89
Salt - dcaeica | Total amount of additional commission plus additional VAT of the commission (Withholdings) (11 integers, 2 decimals). | Numeric | 13 | 102
Filler | Stuffed  | Alphanumeric | 32 | 134
Filler | Contains "FOOTER" | Alphanumeric | 6 | 140
Filler | Stuffed  | Alphanumeric | 89 | 229

**Detail**

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Liq-Numc | Commercial Code | Numeric | 8 | 8
Liq-Fproc | Process Date, in ddmmyyyy format | Numeric | 8 | 16
Liq-Fcom | Date of sale or retention, in ddmmyyyy format | Numeric | 8 | 24
Liq-Micr | Microfilm Number | Alphanumeric | 8 | 32
Liq-Numta | Card Number IF OrderNumber is null | Alphanumeric | 19 | 51
Liq-Brand | Card Type. The values are, VI: Visa; MC: Master; DI: Diners; AX: Amex | Alphanumeric | 2 | 53
Liq-Amount | Amount of sale or withholding (11 integers, 2 decimals) | Numeric | 11 | 64
Liq-Currency | Type of currency. 0: weights; 1 dollar. | Numeric | 1 | 65
Liq-Txs | Transaction Type. If it is sale: "VA" Sale to be paid "VP" Paired sale with withholding ("RP"). If it is withholding: <br> “RP” Withholding paired with sale (“VP”) <br> “RA” Total withholding applied <br> “RC” Withholding balance partially applied. <br> "RE" Pending Hold | Alphanumeric | 2 | 67
Liq-Rete | Attribute of the transaction. If it is sale to pay or paired it goes "0000". If it is a 3CSI sale, go to “C3C *” where * is the number of the installment. If it is sale Quotas Comercio, it goes "I &&&", where &&& is the amount of quotas, aligned to the right. If it is NCuotas sale, it goes “nnSI”, where nn indicates the number of installments (02 to 24) and SI indicates: installments without interest. If it is withholding, the withholding code goes. | Alphanumeric | 4 | 71
Liq-Cprin | Commercial headquarters code. If LiqNumc is the matrix, it bears “99999999”. | Numeric | 8 | 79
Liq-Fpayment | Payment date, in ddmmyyyy format | Numeric | 8 | 87
Liq-orpedi | Order Number or Bar Code | Alphanumeric | 26 | 113
Liq-codaut | Transaction authorization code | Alphanumeric | 6 | 119
Liq-fees | Number of installment that is being paid and that operates for sales made with the products Ncuotas or Sales in installments without interest. | Numeric | 2 | 121
Liq-vci | Transaction authentication value (Sale) | Numeric | 4 | 125
Liq-ceic | Commission value plus commission VAT (Sale) | Numeric | 11 | 136
Liq-caeica | Value of the additional commission plus additional VAT of the commission | Numeric | 11 | 147
Liq-dceic | Commission value plus commission VAT (Withholdings) | Numeric | 11 | 158
Liq-dcaeica | Value of the additional commission plus additional VAT of the commission (Withholdings) | Numeric | 11 | 169
Liq-ntc | total number of installments of the original sale and that operates for sales made with the products NCuotas or Sales in installments without interest. | Numeric | 2 | 171
Liq_Bank_name | Bank name will indicate the name of the bank | Alphabetical | 35 | 206
Liq_Tipo_cuenta_banco | Type of the bank account associated with the credit | Alphabetical | 2 | 208
Liq_Number_bank_account | Bank account number associated with the credit | Alphabetical | 18 | 226
Liq_Moneda_cuenta_banco | Currency of the selected subscription account | Alphabetical | 3 | 229

### Debt Settlement File (L5)
The debit settlement file contains the detailed record of the credits and
withholdings on the merchant account within a period determined by
debit card transactions.

** Header **

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Subscription-From | Subscription period start date Format ‘ddmmyy’ | Numeric | 6 | 6
Filler | Available | Alphanumeric | 1 | 7
Subscription-Until | End date of subscription period Format ‘ddmmyy’ | Numeric | 6 | 13
Filler | Available | Alphanumeric | 4 | 17
Liqu-Date | Settlement date Format ‘ddmmyy’ | Numeric | 6 | 23
Filler | Available | Alphanumeric | 1 | 24
Name-Fan | Trade fancy name | Alphanumeric | 25 | 49
Header | “HEADER” record indicator | Alphanumeric | 6 | 55
Filler | Available | Alphanumeric | 160 | 215

** Footer **

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Liq-Ncom | Sum of detail records For detail records with Liq-Ttra = "00" | Numeric | 10 | 10
Filler | Available | Numeric | 1 | 11
Liq-Tcom | Liq-Amt1 field summation. 11 integers 2 decimals <br> For detail records with Liq-Ttra = "00" | Numeric | 13 | 24
Filler | Available | Numeric | 1 | 25
Liq-Nret | Summation of withholding detail records | Numeric | 10 | 35
Filler | Available | Alphanumeric | 1 | 36
Liq-Mret | Liq-Amt1 field summation, for detail records with Liq-Ttra = ”03”. 11 integers 2 decimals | Numeric | 13 | 49
Liq-Vret | Fixed values of "000000000" | Numeric | 9 | 58
Liq-Tret | Fixed values of "0000000000" | Numeric | 10 | 68
Footer | “FOOTER” record type indicator | Alphanumeric | 6 | 74
Liq-Tot-Com-comiv | Sum of the commission plus VAT of the commission (Sale). 11 integers 2 decimals | Numeric | 13 | 87
Filler | Available | Alphanumeric | 1 | 88
Liq-Tot-decom-ivcom | Sum of the commission plus VAT of the commission (Withholdings). 11 integers 2 decimals | Numeric | 13 | 101
Liq-Fill  | | Alphanumeric | 114 | 215

**Detail**

Name | Description | Format | Length | Total
------ | ----------- | ------- | ----- | -----
Liq-Ccre | Transbank's Internal Commercial Code It is the code assigned to the merchant by Transbank. It does not include the prefix 5970. | Numeric | 8 | 8
Liq-Fpro | Process date of the file Format ‘ddmmyy’. Corresponds to the processing date of the respective transaction. | Numeric | 6 | 14
Liq-Fcom | Purchase date Format ‘ddmmyy’. Corresponds to the date on which the transaction was made | Numeric | 6 | 20
Liq-Appr | Authorization Code Delivered by the authorizer | Alphanumeric | 6 | 26
Liq-Pan | Card number Last four digits only, other characters with asterisks | Alphanumeric | 19 | 45
Liq-Amt1 | Amount of the transaction. 11 integers 2 decimals | Numeric | 13 | 58
Liq-Ttra | Transaction type = 0: purchase paid <br> greater than 0: hold | Numeric | 2 | 60
Liq-Cpri | Fixed value "99999999" | Numeric | 8 | 68
Liq-Marc | Withholdings Value “RE”: for withholdings. Blank if it is a paid transaction. | Alphanumeric | 2 | 70
Liq-Fedi | Settlement Date Format ‘dd / mm / yy’ Corresponds to the date of payment of the respective transaction | Alphanumeric | 8 | 78
Liq-nro-unique | Unique number | Alphanumeric | 26 | 104
Liq-com-comiv | Commission value plus VAT of the commission (Sale). 11 integers 2 decimals | Numeric | 13 | 117
Liq-cad-cadiva | N / A The length is filled with "0". 11 integers 2 decimals | Numeric | 13 | 130
Liq-decom-ivcom | Commission value plus VAT of the commission (Withholdings). 11 integers 2 decimals | Numeric | 13  |143
Liq-dcoad-ivcom | N / A The length is filled with "0". 11 integers 2 decimals | Numeric | 13 | 156
Liq_prepago | Prepaid Value "P" for records that come from prepaid. Blank if it is a transaction for another product. | Alphanumeric | 1 | 157
Liq_Bank_name | Bank name the bank name will be indicated | Alphabetical | 35 | 192
Liq_Tipo_cuenta_banco | Type of the bank account associated with the credit | Alphabetical | 2 | 194
Liq_Number_bank_account | Bank account number associated with the credit | Alphabetical | 18 | 212
Liq_Moneda_cuenta_banco | Currency of the selected subscription account | Alphabetical | 3 | 215


### Grades

* All the "Process Date" fields explained in this document apply to the
File Process Date
* The following table indicates which transactions can occur both through Host
as contingency POS, and which only through one of both ways:

 <table>
 <tr>
 <td> </td> <td colspan="3"> Transaction type </td>
 </tr>
 <tr>
     <th> Transaction Code </th>
     <td> 210 </td>
     <td> 220 </td>
     <td> 420 </td>
 </tr>
 <tr>
     <th> 10 </th>
     <td> Host - POS </td>
     <td> Host - POS </td>
     <td> Host - POS </td>
 </tr>
 <tr>
     <th> 13 </th>
     <td> Webpay </td>
     <td> </td>
     <td> Webpay </td>
 </tr>
 <tr>
     <th> 14 </th>
     <td> POS - Webpay </td>
     <td> Host </td>
     <td> Host - POS - Webpay </td>
 </tr>
 <tr>
     <th> 18 </th>
     <td> Host - POS </td>
     <td> </td>
     <td> Host - POS </td>
 </tr>
 </table>


* For applications with SPDH 3.1 messaging, override transactions
made through the Host are considered as FS04 (offline), which
in the file it would be represented as a 220-14
* For the purposes of matching transactions, only records with
transaction code “10” (Purchases), “13” (WebPay) and “18” (Purchase with
turned).
* For the records corresponding to held transactions (Code of
Transaction “30”), only the following fields will be reported:
o Transaction Date (Dktt-Dt-Tran-Dat)
o Commercial code (Dktt-Dt-Id-Retailer)
o Unique Number (DSK-ID-NUM-UNICO)
o Transaction amount (Dktt-Dt-Amt-1)
* In the case of Settlement, the field (liq-vci) may contain the TSY values
(correctly authenticated), since you show only national ok authorizations.
In the case of an international transaction, you will be able to view TSY and A (attempt
authentication)
* In quadrature file cases, the field (Dktt-Dt-VCI) in addition to the
previous cases, there will also be rejections with TO (TimeOut) and TSN
(Unauthenticated transaction).

### Balances file
It considers the accounting balances pending payment for the Credit products ($ and US $)
and Debit. Contains all transactions that are processed until closing
accounting (last day of process until 2:00 p.m.) and that are pending
credit (sales) or charge (withholdings).

** Material available in the box **
* Detail of Accounting Balances Pending payment; File containing all
pending credit transactions that support the report available on the portal
* Detail transactions at the end of the month; File that presents the detail of
transactions at the end of the month, which have not been processed within the month and
that do not make up the report of the balance pending payment.
For each customer box a folder called CARTOLA_SALDOS will be created, inside
from it will be all the files where its name will be identified with PREFIX_DDMMYYY_XXXXXXXXX (DET_DDMMYYY_XXXXXXXXX).

DET_31102018_761349465. * (.Txt)
COL_31122018_761349465. * (.Txt)

** Text file structure **

Field | Data Type | Length
--- | ---- | ---
PROCESS DATE | DATE | 
TRADE RUT | STRING | 9
COMMERCE CODE | STRING | 8
TYPE OF CONTRACT | STRING | 2
CONTRACT TYPE DESCRIPTION | STRING | 13
FLOW TYPE | STRING | 4
FLOW TYPE DESCRIPTION | STRING | 2
SALE DATE | DATE |
DATE OF PAYMENT | DATE | 
CARD | STRING | 19
NRO FEE | NUMBER | 2
FEE AMOUNT | NUMBER | 17.2
LNIN SEC | NUMBER | 23
PROCESS DATE TXS | DATE | 
SALE AMOUNT | NUMBER | 17.2
AUTHORIZATION CODE | STRING | 6
ORDER ORDER | STRING | 26
SERVICE ID | STRING | 20
STOPPED | STRING | 2



### Structure file names balances
The file names will have a new structure, according to the following detail:
`` ''
 <Formato de Salida> _ <Periodo> _ <Agrupación Archivo de Salidas> _ <Agrupación de Transacciones> _ <RutEnrolado ó CódigoComercio> _ <Tipo Conexion> <Modo Agrupación>
`` ''

** `<Formato de Salida>` ** corresponds to the output format:
LDN = Debt settlement
LCN = Credit settlement
CDN = Square debit
CCN = Credit squaring

** `<Periodo>` ** corresponds to the date in DDMMYYYY format, depending on the frequency
configured:
* Daily: A daily file is generated based on the process date; this is the
  default frequency
* Monthly: One file is generated per month. Date associated with the penultimate business day of the
  month

** `<Agrupación Archivo de Salidas>` ** corresponds to the grouping nature:
* RE: File with unique information associated with the RUT enrolled (Generates a file)
* CC: File with information separated by trade code of the transaction
  (generates a different file for each trade code)

** `<Tipo Conexión>` ** corresponds to the type of connection of the commercial codes:
* A: A single file is generated. File information is not separated by type
  connection (when it is a single file it is not part of the name)
* SE: A file is generated for transactions with a face-to-face connection type and not
  face-to-face
* NP: Not in person
* PR: face-to-face

** `<Modo Agrupación>` ** corresponds to the grouping mode of the commercial codes
SC: Main Branch (when grouped by RUT it is not included in the name
from file)
RB: Item


# Procedure link and connection Pinpad Bluetooth e355

## Bluetooth Pinpad link flow

! [] (/ images / documentation / host2host / stream-pp-blue-e355.png)

## E355 Bluetooth Pinpad connection


### Power on equipment

**Step 1**

When you turn on the equipment, it enters the main screen of the application.

**Step 2**

The keys “* 0” must be entered sequentially to enter the “Bluetooth Connection Menu.
! [] (/ images / documentation / host2host / connection-step-2.png)


### Link devices

**Step 3**

In the bluetooth connection menu.
The option ** 1> Link Device is selected **

**Step 4**

If a search was previously carried out, it will ask if you want to ** Search again **, if you press
** 2> NO ** will show the devices found in the previous search.

! [] (/ images / documentation / host2host / connection-step-4a.png)

* The ** BT Name **, which is the name that will be visible to other Bluetooth devices *

! [] (/ images / documentation / host2host / connection-step-4b.png)

** Step 5 **

At the end of the search, it shows the devices found

! [] (/ images / documentation / host2host / connection-step-5.png)

** Step 6 **

Selecting a device will display this
window to confirm the link.

! [] (/ images / documentation / host2host / connection-step-6.png)

** Step 7 **

When selecting the device, this window will appear to
confirm the link.

! [] (/ images / documentation / host2host / connection-step-7a.png)


A similar message will appear on the other device, the
code that appears should be the same in both
devices;
In this case ** 648005 **
And we select YES and they will be linked

! [] (/ images / documentation / host2host / connection-step-7b.png)


** Step 8 **

Commerce must accept the Bluetooth connection.
* The visit to the store must be coordinated - Counterpart Commerce *

## Review SPP enablement

** SPP: Serial Profile interface (interface that emulates a serial port connection) must be configured in case it is disabled **


**Step 1**

In the Main Menu of the bluetooth manager.
If required enable SSP (Serial Port Profile) ** 3> Config MENU CONNECT. BLUETOOTH SPP \ [Off \] **

! [] (/ images / documentation / host2host / enable-spp-step-1.png)

**Step 2**

Select option ** 2> SPP Server **

! [] (/ images / documentation / host2host / enable-spp-step-2.png)

**Step 3**

This screen will appear and you will return to the Main menu

! [] (/ images / documentation / host2host / enable-spp-step-3.png)

## Delete linked teams

**Step 1**

In the Administrator Menu
Bluetooth, select option
** 2> Linked Devices **

! [] (/ images / documentation / host2host / delete-machines-step-1.png)

**Step 2**

In this option a menu will be displayed
with all devices that are
paired with the e355 pinpad, they are
registered devices
previously linked.

! [] (/ images / documentation / host2host / delete-machines-step-2.png)

**Step 3**

If we select the device, this will appear
menu that show us 2 options.
If we select ** YES ** we remove the link, and
we return to the main menu.

! [] (/ images / documentation / host2host / delete-machines-step-3.png)


## Using the SDK


For integration with applications of the
trade, it is necessary to use in the developments
own, the libraries included in the team's SDK
Verifone e355.
These are:
1. libPtr
! [] (/ images / documentation / host2host / libPtr.png)

2. libVmf
! [] (/ images / documentation / host2host / libVmf.png)

To facilitate the integration process, the
reference to the resource "pinpad" that contains
functions for communication between Pinpad and the
Commerce App.
The structure and sample code are
included in the ** demo_e355.zip ** file


### Import of libraries and Pinpad resource

! [] (/ images / documentation / host2host / sdk-import-lib-pinpad.png)

### Declaration of the main activity and PINPadTransport

! [] (/ images / documentation / host2host / sdk-pinpadtransport.png)


### Function for connection and disconnection

! [] (/ images / documentation / host2host / sdk-connection-disconnection.png)

### Example to send command 0100 (read card to pinpad) to Pinpad

! [] (/ images / documentation / host2host / sdk-command-0100.png)

### Function to receive the response from the Pinpad

! [] (/ images / documentation / host2host / sdk-answer-pinpad.png)


## Installation on Windows

When the link is made, 2 COM ports are created that link to Bluetooth.
In order to review it, you must enter: ** Devices **

**Step 1**

! [] (/ images / documentation / host2host / windows-1.png)

**Step 2**

! [] (/ images / documentation / host2host / windows-2.png)

**Step 3**

! [] (/ images / documentation / host2host / windows-3.png)

**Step 4**

! [] (/ images / documentation / host2host / windows-4.png)


 <div class="container slate">
   <div class='slate-after-footer'>
     <div class='row d-flex align-items-stretch'>
       <div class='col-12 col-lg-6'>
         <h3 class='toc-ignore fo-size-22 text-center'> Do you have any integration questions? </h3>
         <a href='https://join-transbankdevelopers-slack.herokuapp.com/' target='_blank'>
           <div class='td_block_gray'>
             <img src="https://p9.zdassets.com/hc/theme_assets/138842/200037786/logo.png" alt="" >
             <div class='td_pa-txt'>
              Join the community of integrators on Slack and share good tips by helping new ones or looking for alternative solutions. Our team is there to help you.
             </div>
           </div>
         </a>
       </div>
       <div class='mt-3 mt-lg-0 col-12 col-lg-6'>
         <h3 class='toc-ignore fo-size-22 text-center'> If you still have questions, send us a message </h3>
         <a class="pointer magenta" data-toggle='modal' data-target='#modalContactForm'>
           <div class='td_block_gray'>
             <div class="fo-size-20 text-center sub-title_bloq"> <i class="fas fa-envelope"> </i> Send us a message. </div>
             <div class='td_pa-txt'>
              If you need to solve any type of incident on the portal or if there is a problem with your integration that you have not been able to solve, contact us through our form.
             </div>
           </div>
         </a>
       </div>
     </div>
   </div>
 </div>
