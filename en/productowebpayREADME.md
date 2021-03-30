# Webpay

 <div class="pos-title-nav">
   <div class="video" data-toggle="modal" data-src="/public/resourse/mooc/webpay/menu/index.html" data-target="#ModalCenterData"> Introduction E-learning <i class="op-link"> </i> </div>
 </div>

Webpay is Transbank's payment gateway to carry out transactions from the Internet with Redcompra credit and debit cards in an efficient and secure way.

A payment flow in Webpay generally has the following steps:

1. The cardholder selects the products or services to pay on the merchant's site.

2. The cardholder chooses to pay with Webpay where, depending on the products contracted by the merchant, the payment alternatives are displayed: Credit card, Redcompra debit and / or prepaid.

3. During the payment process, the card issuer authenticates the cardholder before making the financial transaction, in order to validate that the card is being used by the cardholder.

4. Once the authentication is resolved, the payment is authorized. Webpay delivers the authorization result to the merchant system.

## Webpay Products

The products offered on the Webpay web services are described below.

### Webpay Plus

 <div class="pos-title-nav">
   <div tbk-link='/documentacion/webpay-plus#webpay-plus' tbk-link-name='Documentación'> </div>
   <div tbk-link='/referencia/webpay#webpay-plus-normal' tbk-link-name='Referencia Api'> </div>
   <div tbk-link='/plugin/webpay' tbk-link-name='Plugins'> </div>
 </div>

Webpay Plus allows you to make a request for financial authorization ** of a payment ** with credit, Redcompra or prepaid debit cards where whoever makes the payment enters the merchant's site, selects products or services, and indicates the data associated with the card of the previously selected payment method, this action is carried out safely in Webpay. The merchant that receives payments through Webpay Plus is identified by a merchant code.

It is the most common type of transaction, used for a punctual payment in a simple store. A single charge will be generated for
all products or services purchased by the cardholder.

### Webpay Plus Mall

 <div class="pos-title-nav">
   <div tbk-link='/referencia/webpay#webpay-plus-mall' tbk-link-name='Referencia Api'> </div>
 </div>

Webpay Plus Mall allows you to make a financial authorization request for ** a set of payments ** with credit, debit or prepaid cards, through a single unified transaction. Whoever makes the payment, enters the merchant's site, selects several products or services (belonging to different merchant codes), and selects Webpay Plus as a means of payment, making the transaction securely in Webpay for all payments. ** Each payment will have its own result, authorized or rejected. **
The concept of "mall" groups together multiple stores. It is these stores that can generate transactions. Both the mall
as each of the associated stores are identified by a different trade code.

The Mall transaction type is useful for technology service providers (PSTs) that can perform a single
integration with Transbank and make payments on behalf of the clients of said technological services. For example, a
SaaS platform can be a mall and the client companies of the platform can be the stores of said mall. Of that
In this way, the collection made by the platform will go directly to the client on whose behalf each payment was made.

### Oneclick Mall

 <div class="pos-title-nav">
   <div tbk-link='/referencia/oneclick' tbk-link-name='Referencia Api'> </div>
 </div>

Oneclick Mall allows you to group payments in a single Oneclick transaction to multiple merchant codes
(similar to a Mall transaction). In a transaction of this type, as in Oneclick, the cardholder may
make payments without the need to enter your credit card information in each of them. This type of
payment facilitates the sale, centralizes payments, reduces transaction time and reduces income risks
erroneous data of the means of payment.

** Oneclick Mall operates with Redcompra credit, prepaid and debit cards. ** The payment model includes the same enrollment process as the Oneclick transaction.

With regard to commerce, Oneclick Mall combines two groups of benefits:

* It allows a cardholder to register their card only once and pay frequently at any of the stores in the mall.

* It allows a technology service provider (PST) to perform a single integration and make payments on behalf of multiple client stores.

By not having a bank authentication system in the charges that are made after authorization, the merchant will be responsible for assuming the risk of fraud or ignorance of the purchase made by a cardholder.

### Webpay Complete Transaction {data-submenuhidden = true}

 <div class="pos-title-nav">
   <div tbk-link='/referencia/webpay#webpay-transaccion-completa' tbk-link-name='Referencia Api'> </div>
 </div>

This product allows you to carry out credit card transactions without using the standard Webpay form. The biggest difference with Webpay Plus Traditional is that the credit card data is entered within the merchant's website itself, which allows customizing, at its discretion, the way in which it requests and validates the customer's data. .

* The Full Transaction mode only works with credit cards.

* The merchant can customize the form in which it requests cardholder data to process a transaction.
The merchant that wishes to start operating with the Full Transaction modality must have the certification of PCI DSS Standards (Payment Card Industry-Data Security Standard) and renew them annually, due to the handling of sensitive data that they can process and that will be used exclusively in its relationship with Transbank.

Complete Transaction, offers the merchant to choose one or both of the following functionalities:

* Online authorization and simultaneous capture, in which the cardholder is charged immediately.
* Online authorization and deferred capture, in which a credit reservation is made on an estimated value of the product and / or service to be acquired by the cardholder, subsequently the merchant defines the amount of the transaction which will be less than or equal to the authorized one, in case of being higher the transaction will be rejected.

For this reason this product is not included in Transbank's public offer.

## Payment types

The types of payment currently available through Webpay depend on the type of card used by the cardholder and those that the merchant has activated.

For credit card, it can be the following types of payment (with abbreviations in parentheses):

* Normal Sale (VN): Payment in 1 installment.
* 2 Installments without interest (S2): The merchant receives the payment in 2 equal installments without interest.
* 3 Installments without interest (SI): The merchant receives the payment in 3 equal installments without interest.
* N Interest-free installments (NC): The merchant receives the payment in a number of equal, interest-free installments that the cardholder can choose from a range of 2 and N (the value N is defined by the merchant and cannot be higher than 12)
* Normal installments (VC): The issuer offers the cardholder between 2 and 48 installments. The issuer defines whether they are interest-free (if they have established a range of shares in promotion) or interest-free. The issuer can also offer 1 to 3 months of deferred payment. All this without impact for the commerce that in this modality of quotas always receives the payment in 48 working hours.

For Redcompra debit card, the type of payment always corresponds to:

* Redcompra debit sale (VD): Payment with a Redcompra debit card.

* Prepaid Sale (VP): Payment with a Redcompra debit card.

## Authorization response codes

When a transaction is rejected, Transbank sends a response code to the merchant indicating that it could not be carried out correctly.
Currently, merchants have level 1 configured, which is the standard for Webpay and provides general rejection information,
 grouping in the same different types of rejection.

You can see the detail of level 1 below:

Answers to trade <br> | Description
------   | -----------
-1 | Rejection - Possible error in the transaction data entry
-2 | Rejection - There was a failure to process the transaction, this rejection message is related to parameters of the card and / or its associated account
-3 | Rejection - Internal Transbank
-4 | Rejection - Rejected by the issuer
-5 | Rejection - Transaction with risk of possible fraud

As of March 1, 2021, the <b> level 2 </b> of authorization response codes is available which
they deliver rejection information more specifically.

To activate level 2, you must send an email to support@transbank.cl indicating the trade code to which
you want to perform this activation.

You will be informed in a timely manner the day on which the activation will take place.

You can see the detail of level 2 below:

Answers to trade <br> | Description
------   | -----------
-1 | Invalid card
-2 | Connection error
-3 | Exceeds maximum amount
-4 | Invalid expiration date
-5 | Authentication problem
-6 | General rejection
-7 | Locked card
-8 | Expired card
-9 | Transaction not supported
-10 | Transaction problem


## Authorization and Capture

 <div class="pos-title-nav">
   <div tbk-link='/documentacion/webpay-plus#capturar-una-transaccion' tbk-link-name='Documentación'> </div>
 </div>

Webpay transactions have 2 phases: authorization and capture. The ** authorization ** is in charge of validating whether it is possible to charge the account associated with the credit card by reserving the transaction amount in the same act. The ** capture ** makes the reservation made previously effective or debits the credit account associated with the cardholder's card.

For Redcompra and prepaid debit cards, authorization and capture is always simultaneous. For credit cards, authorization and capture can be simultaneous or separate. In the latter case, it is a ** delayed capture **.

From the point of view of the transaction, what happens is the following:

* ** Authorization and simultaneous capture: ** The transaction is validated online by Transbank. The payment charge is made simultaneously on the customer's Redcompra credit or debit card.

* ** Authorization and deferred capture (Only valid for credit cards): ** The value of the purchase is retained from the customer's credit card balance, reserving space but without making the transaction definitively until the merchant confirms the purchase ( via deferred capture) and communicate it to Transbank. There is a ** maximum time of 7 calendar days ** to capture. After that time the retention of the credit card will be reversed and the quota will be released. Capture can be done through the [Transbank portal] (https://www.transbank.cl/web/login) or through [the deferred capture web service] (/ reference / webpay-soap # capture-deferred-webpay- plus).

## Cancellations

 <div class="pos-title-nav">
   <div tbk-link='/documentacion/webpay-plus#reversar-o-anular-una-transaccion-mall' tbk-link-name='Documentación'> </div>
 </div>

Webpay ** transactions made with a credit card ** can be canceled through web services. This functionality does not apply to Redcompra debit cards.

Cancellations can be total or partial. ** Partial cancellations are only allowed in the Normal Sale (NV) mode **. In the event that the transaction has been paid to the merchant, the cancellation will generate a withholding on subsequent payments for the previously authorized amount.

The merchant has a period of 90 days from the date of sale to cancel transactions via web services.

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
