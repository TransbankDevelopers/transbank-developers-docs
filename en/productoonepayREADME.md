# Onepay

 <div class="pos-title-nav">

   <div class="video" data-toggle="modal" data-src="/public/resourse/mooc/onepay/menu/index.html" data-target="#ModalCenterData"> Introduction Elearning <i class="op-link"> </i> </div>

   <div tbk-link='/documentacion/onepay' tbk-link-name='Documentación'> </div>
   <div tbk-link='/referencia/onepay' tbk-link-name='Referencia Api'> </div>
   <div tbk-link='/plugin/onepay' tbk-link-name='Plugins'> </div>
 </div>

Onepay is a digital wallet that allows you to make purchases in e-commerce stores with a Credit Card, without having to enter the card information for each purchase. The wallet works on * Android or iOS smartphones *. This type of payment facilitates the sale, reduces the time of the transaction and reduces the risks of erroneous entry of the data of the means of payment.

The product is safe, since the cardholder data is not stored on the mobile device. And the business is protected against fraud or ignorance of purchase.

Onepay operates with local credit card issuers and operates with all types of fees available.

For the cardholder, two payment flows can be identified depending on whether the purchase is made on the same device where their Onepay wallet resides (Mobile Payment) or if the purchase is made on another device (Desktop Payment). However, for the Onepay web integration the difference is totally transparent for the trade.

## Onepay Pago Mobile

Onepay is the ideal product for your customers to pay on their mobile device. When the cardholder has their virtual wallet installed, the payment flow with Onepay consists of the following steps:

1. The cardholder decides to purchase a product or service within the merchant's website or app.
2. The cardholder selects the option to pay with Onepay.
3. The cardholder is automatically redirected to the Onepay app.
4. By selecting one of their already registered cards and entering their wallet PIN, the cardholder confirms the payment.
5. The cardholder is redirected back to the merchant's website or app, who receives the authorized payment information.
6. The merchant contacts Transbank to confirm the transaction.

If the cardholder does not have the Onepay application installed, the instructions to install and configure the wallet are indicated at checkout.

## Onepay Pago Desktop

In the event that the cardholder is operating on a laptop or desktop computer, the payment flow with the Onepay wallet is as follows:

1. The cardholder decides to purchase a product or service on the merchant's website.
2. The cardholder selects the option to pay with Onepay.
3. The store's website presents the cardholder with a QR code that must be scanned using the Onepay wallet with their phone's camera.
4. The cardholder scans the code, selects one of their already registered cards, and enters their wallet PIN to confirm payment.
5. The merchant's website (through the Onepay integration) automatically detects that the payment has been authorized in the wallet.
6. The merchant contacts Transbank to confirm the transaction.

In this way, the payment is made with minimal friction for the cardholder and allows a greater conversion in the merchant's sales.

## Onepay payment physical store

Onepay can be used as a Sharpener or Mobile Cash in your physical stores; allowing you to make sales more efficient and reducing the payment lines and waiting times of your customers, thanks to the implementation of a mobile application (on a tablet or phone) that your store staff will have, or directly on a self-service totem that allow closing sales by integrating with Onepay.

The flow in this case works like this:

1. The cardholder has the possibility of making their purchase at a self-service totem or with the assistance of a seller who has a mobile device with the integrated application to start the payment process with Onepay.
 <img alt="onepay_step_1" src="/images/producto/onepay/onepay_venta_fisica_paso_1.png" class="rounded mx-auto d-block"/>

2. The device or totem communicates with the merchant's backend to generate a Onepay transaction with a "mobile" type channel, receiving the information of the transaction created and displaying a QR on the device screen.
 <img alt="onepay_step_2" src="/images/producto/onepay/onepay_venta_fisica_paso_2.png" class="rounded mx-auto d-block"/>

3. The customer scans the QR code displayed on the merchant's device and pays confirming the purchase with their Onepay PIN.
 <img alt="onepay_step_3" src="/images/producto/onepay/onepay_venta_fisica_paso_3.png"  class="rounded mx-auto d-block"/>

4. To finalize the transaction, the customer sees the proof of purchase on his phone. When generating this receipt, the merchant's backend notifies the seller's device or the self-service totem that the transaction has been completed successfully.
 <img alt="onepay_step_4" src="/images/producto/onepay/onepay_venta_fisica_paso_4.png"  class="rounded mx-auto d-block"/>

This Onepay integration is a simple alternative to implement through SDKs, you just have to be attentive to the updates that Transbank carries out periodically.

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
