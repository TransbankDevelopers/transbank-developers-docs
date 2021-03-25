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

### ** Sales flow on Pinpad Host: **

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

We are working on integrating the documentation in a more friendly format in this same section.
At the moment, we offer you the following documents in PDF format

### Available documents

* ** HOST command manual ** - _RS20.1_ | [Download] (/ files / manual-commands-3-0.pdf) <br />
_This document describes the way of operating, the functionality and the details of the messaging between the TRANSBANK PINPAD and your cash system using commands via USB or SERIAL port.
* ** Host RS20.1 Technical Specifications Manual ** - _v2.2_ | [Download] (/ files / manual-specifications-host-40-22.pdf) <br />
_This document contains the technical specifications for using the Host to Host product. already with a look of what is required by the box and by the host server of the trade.
* ** ONUS HOST command manual ** - _2018.06.01 v1.6_ | [Download] (/ files / manual-commands-pinpad-annex-onus-1-6.pdf) <br />
_This document describes the way of operating, the functionality and the detail of the messaging of a PINPAD TRANSBANK working under the OnUs mode ._
* ** HOST Wifi command manual ** - _18.2 v2.4_ | [Download] (/ files / manual-commands-pinpad-host-wifi.pdf) <br />
_This document describes how to operate, the functionality and the details of the messaging of a PINPAD TRANSBANK using the TCP / IP protocol.
* ** HOST voucher manual ** - _RS20.1_ | [Download] (/ files / manual-voucher-con-surcharge.pdf) <br />
_The pinpad will deliver the voucher ready to print, but it will also deliver all the necessary data for the same box to build the voucher itself._
* ** Manual of technical specifications of special outputs ** - _20201112_ | [Download] (/ files / manual-technical-specifications-of-special-outputs-20201112.pdf) <br />
_This annex describes the quadrature (reconciliation) and settlement files that are delivered to the Host client.
Quadrature (reconciliation) files are flat files that contain transaction records
(of sale and cancellation) _
* ** Manual to link Pinpad Bluetooth ** - _v3.1_ | [Download] (/ files / manual-link-pinpad-bluetooth-3-1.pdf) <br />
_Bluetooth Pinpad connection and connection procedure e355_


 <div class="container slate">
   <div class='slate-after-footer'>
     <div class='row d-flex align-items-stretch'>
       <div class='col-12 col-lg-6'>
         <h3 class='toc-ignore fo-size-22 text-center'> Do you have any integration questions? </h3>
         <a href='https://join-transbankdevelopers-slack.herokuapp.com/' target='_blank'>
           <div class='td_block_gray'>
             <img src="https://p9.zdassets.com/hc/theme_assets/138842/200037786/logo.png" alt="" >
             <div class='td_pa-txt'>
              Join the community of integrators in Slack and share good tips by helping new ones or looking for alternative solutions. Our team is there to help you.
             </div>
           </div>
         </a>
       </div>
       <div class='mt-3 mt-lg-0 col-12 col-lg-6'>
         <h3 class='toc-ignore fo-size-22 text-center'> If you still have questions send us a message </h3>
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
