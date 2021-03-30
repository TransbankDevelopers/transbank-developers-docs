# PatPass

PatPass is the Transbank service that allows automatic payment of accounts by credit cards. It is the ideal solution for paying bills, educational centers, contributions to foundations, and other businesses and institutions.

PatPass ** operates exclusively with credit cards (national and international) **. Redcompra debit cards are not accepted. The use of quotas is not contemplated.

PatPass interacts with merchants and issuers through the following platforms:

* ** Transdata ** is the platform that Transbank makes available to merchants so that they can review the result of the charges generated from their customers subscribed to PatPass, also allowing the administration of their universe of subscriptions.

* ** Issuer PatPass ** is the platform that allows the cardholder to manage the automatic payments on their card from their issuing bank's website.

Complementary to these platforms, ** PatPass by Webpay ** allows merchants to make customer registrations available directly to the cardholder, taking advantage of the infrastructure and security offered by the Webpay gateway.

## PatPass by Webpay {data-submenuhidden = true}

 <div class="pos-title-nav">
   <div tbk-link='/documentacion/patpass' tbk-link-name='Documentación'> </div>
   <div tbk-link='/referencia/patpass' tbk-link-name='Referencia Api'> </div>
 </div>

Through PatPass by Webpay, the cardholder generates an automatic payment with a Credit Card for a product and / or service for a fixed amount, making ** the first payment online through Webpay Plus and then making the monthly payment automatically ** via the PatPass engine, with a merchant-defined end date.

Regarding currencies, PatPass by Webpay allows you to charge in Peso, UF and Dollar. For payments in pesos and UF PatPass will assign the same commercial code. For Dollar (in case the merchant has signed the * Dollar Contract Annex *) a different merchant code will be assigned. Collections in UF will be valued on the same day the collection is made. In the event that the charge is on a non-business day, it will be charged the next business day.

PatPass by Webpay is a channel used only to register automatic payments. If the business requires canceling an automatic payment, you must use the other existing channels (such as Transdata).

** The merchant must store the information of all the transactions carried out through the PatPass by Webpay product **, and must maintain the respective records for at least one year from the date of each operation, as a backup of the sales made and services provided. This information must be made available to Transbank each time and as soon as it is required. The minimum information that the Commercial Establishment must store for each of the transactions is the following:

* Commercial Code assigned by Transbank.
* Order Number.
* Date and time of the transaction.
* Amount and currency of the operation.
* Authorization Code number delivered by Transbank.
* Service Identification.
* Description of goods sold or services provided.
* Name of the cardholder making the purchase.
* Issuer authentication field.
* Subscription expiration date.
* Cardholder contact email.

### Flow of a PatPass by Webpay transaction

The flow of a PatPass by Webpay transaction can be summarized in the following steps:

1. The cardholder navigates within the merchant's page.
2. The cardholder selects the PatPass by Webpay payment option and completes the information required by the merchant (RUT, name, email, telephone)
3. After completing the information, the cardholder is redirected to Webpay where they are informed of the amount and the end date of their monthly subscription. In the Webpay form, the cardholder enters their card information.
4. The card issuer authenticates the cardholder before making the financial transaction, in order to validate that the card is being used by the cardholder.
5. If the authentication is successful, PatPass by Webpay checks the customer information sent by the issuer with the information provided by the merchant to ensure they match. If so, we proceed with the authorization and capture of the amount charged.
6. If said authorization and capture is successful, Transbank proceeds automatically and transparently (both for the merchant and for the cardholder) to register the recurrence in the PatPass Platform.
7. Finally, an email will be sent to the merchant and another email to the cardholder with the information of the automatic payment activated.

Once the operation is completed, the merchant will be able to view the details of the transactions and consult the recurring charges through Transdata. By the cardholder, you can view the registration of this monthly charge through the Issuer PatPass (HomeBanking) in their respective Bank. There you can see the channel of origin that corresponds to "Monthly Webpay" and the end date of this recurring charge.

In the event that a collection is not successful or the automatic payment is canceled, the cardholder will also receive an email alert (as received in the registration described above in point 7).

## PatPass Trade

 <div class="pos-title-nav">
   <div tbk-link='/documentacion/patpass' tbk-link-name='Documentación'> </div>
   <div tbk-link='referencia/patpasscomercio' tbk-link-name='Referencia Api'> </div>
 </div>

Through PatPass Comercio, the cardholder generates an automatic payment subscription with a Credit Card for a product or service for a fixed amount, making ** the first payment online through the PatPass Platform and then automatically charging monthly ** to Through the same PatPass engine, with an end date defined by the merchant.

Regarding currencies, PatPass Commerce only allows you to use Chilean Peso. For charges in other currencies, the merchant has to take care of the conversion with the fees that this entails.

PatPass Commerce is a channel used only to register automatic payments. If the business requires canceling an automatic payment, you must use the other existing channels (such as Transdata).

** The merchant must store the information of all the transactions carried out through the PatPass Commerce product **, and must maintain the respective records for at least one year from the date of each operation, as a backup of the sales. carried out and the services provided. This information must be made available to Transbank each time and as soon as it is required. The minimum information that the Commercial Establishment must store for each of the transactions is the following:

* Commercial Code assigned by Transbank.
* Order Number.
* Date and time of the transaction.
* Amount and currency of the operation.
* Authorization Code number delivered by Transbank.
* Service Identification.
* Description of goods sold or services provided.
* Name of the cardholder making the purchase.
* Issuer authentication field.
* Subscription expiration date.
* Cardholder contact email.

### Flow of a PatPass Commerce transaction

The ** PatPass Commerce ** transaction flow can be summarized in the following steps:

1. The cardholder navigates within the merchant's page.
2. The cardholder selects the PatPass Comercio payment option and completes the information required by the merchant (RUT, full name, email, address, mobile and landline phones).
3. Once the information has been sent, the client is redirected to Webpay where they are informed of both the amount and the end date of the monthly subscription, in this form they must enter their card information.
4. The card issuer authenticates the cardholder before making the financial transaction, in order to validate that the card is being used by the cardholder.
5. If authentication is successful, PatPass Commerce checks the customer information submitted by the issuer with the information provided by the merchant to ensure they match. If so, we proceed with the authorization and capture of the amount charged.
6. If said authorization and capture is successful, Transbank proceeds automatically and transparently (both for the merchant and for the cardholder) to register the recurrence in the PatPass Platform.
7. Finally, an email will be sent to the merchant and another email to the cardholder with the information of the automatic payment activated.

Once the entire operation is completed, the merchant will be able to view the details of the transactions and inquire about recurring charges through Transadata.
On the part of the cardholder, they will be able to view the registration of this monthly charge through the Issuer PatPass (HomeBanking) in their respective Bank. There you can see the channel of origin that corresponds to "Monthly Webpay" and the end date of this recurring charge.

### PatPass overrides

An Cancellation corresponds to the revocation of a sale already authorized and captured by Transbank.

Sales cancellations may be requested by:

* Entering the Transbank web portal.
* Calling Customer Service.

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
