# Integrated POS

 <div class="pos-title-nav">
   <div tbk-link='/documentacion/posintegrado?csharp' tbk-link-name='Documentación'> </div>
   <div tbk-link='/referencia/posintegrado?csharp' tbk-link-name='Referencia'> </div>
 </div>

The integrated POS allows you to carry out transactions with a Credit / Debit / Prepaid card with Transbank using the serial port of a PC or Cashier. Communication with Transbank and the logic of processing a financial transaction is carried out by the POS team, thus facilitating integration with a cash system.

## Supported Transactions

* Sale.
* Closing.
* Annulment.
* Last sale.
* Sales detail.
* Totals.
* Loading keys.
* Poll (POS communication test - box).
* Change to normal POS mode.
* Multi-code sale.
* Last multi-code sale
* Multi-code sales detail.

## Requirements

### Local Requirements

* The store must have its own cash system, with the following desirable characteristics:
  * Product Reading System (scanner / code).
  * Stock system.
  * Quadrature System.
  * System Support Provider.
  * Remote access.
* The box must have at least one `RS232` port or, failing that, an exclusive USB input for the POS Device.
* The store must have in each location with exclusive points to connect to the Internet via Lan `RJ45` the POS.
* It is recommended to segment the bandwidth with exclusive `300 kb` for the use of the POS, since when there are batch processes, security cameras, use of desktop applications, etc. under the same segment can affect a transaction if the contracted bandwidth is not high enough.
* The premises must have an exclusive 220v power point to plug in the POS.

### Trade Requirements

* The store must have one or more areas in charge of the following points:
  * Contingency Procedures in the event of Internet signal drops and electricity.
  * Backup and Recovery Procedure for operations carried out in Transbank's Integrated POS.
  * Homologation in box system versions.
  * Homologation in Cash Operating System.
  * Management of Supervisor Keys and Operating System users.
  * Security Procedures and responsibility in the use of the information provided by Transbank and Clients.
  * Training and dissemination of the use of new systems.
  * Systems Manuals.
  * Network Support.

 <aside class="notice">
It is recommended that the Establishment have the same version of the Operating System and Cashier Software in all its boxes. In case of having different versions, it will be necessary to carry out an Integration and Certification process with Transbank for each of them.
 </aside>

 <aside class="notice">
In the event of any change made by the Establishment, whether in the Operating System or Cashier Software, there could be impacts on the Integration carried out with the Integrated POS, so it is suggested to test the correct operation with the Integrated POS before making the change in the Productive environment. .
 </aside>

## Security

### Confidentiality of information

In accordance with current regulations, Credit and Debit Card transactions include the following security elements in the system:

* The information read at the point of sale is NOT stored in any system.
* For purposes of quadrature and identification of transactions, the OPERATION NUMBER will be used.

### Treatment of Card Tracks

The information recorded in Track I and Track II is read only by security devices (POS). These devices encrypt the content of Track I and Track II.

### Sensitive data processing

To ensure the confidentiality of the information, the transaction messages or at least the sensitive data (in addition to the PIN) travel encrypted in the different sections of the connection, both in the request and in the response. The following are considered sensitive data: card number, expiration date, account number and transaction amount (data validated in Message Authentication or MAC).

### The Master / Session KEY model

The current method of key management is the so-called Master / Session Key, in which the PEDs (Pin Entry Device) are loaded in a secure environment with a Master Key and the Working Key or Session Key is loaded remotely.

The current procedure to encrypt a PinBlock on the Pin Pads is as follows:

* The Working Key is decrypted using the Master Key that the PED has loaded.
* With the Working Key, the PinBlock is encrypted and sent to the server.

The Working Key is changed periodically (at least at each closure), to prevent it from being discovered by
third parties.

This key management model is the one used for MAC keys.

### The DUKPT model - PIN encryption

The new PIN key management method that Transbank will use is the so-called ―Unique Key derived by transaction or DUKPT by its initials in English.

Under this method the PEDs are initialized in a secure environment, with the identification data of each PED (Identifier of the bypass key, unique PED identifier and a transaction counter started at zero), plus an initial key that is calculated using the data of each PED and the base bypass key. With this initial key the next encryption key for PIN is generated. This process is carried out with an asymmetric function (DUKPT of the PinPad), that is, a one-way function, so that the PED is not able to generate any key prior to the current one.

### MAC calculation

To ensure the integrity of the information that travels to and from the Trade Authorizer, a message authentication code (MAC) is entered which is sent in the request message and validated by the Transbank Authorizer upon receipt. In turn, the Transbank Authorizer sends a MAC code for the response message, which must be validated by the bank. If the validation that the MAC code box does is negative, it should generate a reverse. The reverse transaction must be equal to the response received but with the RESPONSE CODE field set to 989 and the MESSAGE SUBTYPE field set to R. When the Transbank Authorizer detects an invalid MAC in the request message, it sends a response message with rejection code 898 (invalid MAC).

### MAC (Message Authentication Code) key handling

The cryptographic keys for the generation of MAC (MAC working key) is handled according to the following:

* The working keys are generated by the Transbank system and transmitted online for each of the terminal IDs defined in the client business.
* To load and / or change the MAC working keys, the CLOSE BATCH and LOAD KEY transactions are used (See Administrative Transactions).

MAC working keys are updated in each new transaction handled by Transbank. So the cashier must register this new key for use in the next transaction.
Keys should be changed automatically every day. This implies that there must be a mandatory initialization or closing procedure in each box (terminal ID) that is executed automatically every day and that as part of this procedure a CLOSE BATCH or KEY LOAD transaction is sent to Transbank for each box (terminal ID).

The working keys (MAC) are transmitted encrypted using the DES algorithm (the data to be encrypted is the working key) with an encryption key called master key, defined by Transbank. Transbank defines a master key for PIN and another master key for MAC.

Transbank initially loads the master keys in each POS, an operation that is carried out prior to their installation in the cashiers.

To load the PIN and MAC master keys, the POS model used must have a key loader device that will be managed by Transbank and that allows:

Enter the master keys in the device, which cannot be modified, violated or tampered with. Load the master keys connecting one by one the POS to the device

### Technical Key Management

To access the Options Menu for Technician, you must be accredited with the RUT and the Code that corresponds to this routine. This key is dynamically generated, with a maximum expiration of 31 days.

### Supervisory Password Management

In the oldest versions, each store had a supervisory card that allowed them to authenticate themselves to carry out closings, cancellations and other operations. From 2011 onwards, during the self-installation process, the supervisor password will be requested, and it will be stored until the merchant wishes to change it, this being the responsibility of the same. If the trade forgets this key, there is a master trade key that allows a new trade key to be entered. To obtain it, you must call Customer Service, from cell phones 600 638 6380 and from landlines +56 2 2661 2700.

### Trade Master Key Management Activation

The request for this password is made to Customer Service, from cell phones 600 638 6380 and from landlines +56 2 2661 2700.

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
