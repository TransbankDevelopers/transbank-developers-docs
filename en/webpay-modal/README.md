# Webpay Modal

Webpay Modal allows merchants to integrate a payment form directly within their website, without a redirect
from the web browser to an external payment platform, allowing your customers to make an authorization request
financial of a payment with credit, debit or prepaid cards.
Used for a timely payment in a simple store. A single charge will be generated for all products or services purchased by the cardholder.

 <aside class="success">
This product is available since version 1.1 of the REST API.
 </aside>

### Flow in case of success

For the cardholder, the payment flow is as follows:

1. Once the goods or services have been selected, the cardholder decides to pay
   through Webpay Modal.
2. The merchant initiates a transaction in Webpay Modal on its server.
3. Webpay processes the request and delivers as a result of the operation on
   token of the transaction.
4. The merchant shows, within its same site, a Webpay Modal payment form, using the token of the
   transaction received in the previous point. For this the Javascript SDK is used.
5. The user completes the payment form, without going through a bank authorization process (he is not redirected to his
   bank to authorize payment).
6. Once the payment is completed, Webpay Modal will indicate to the merchant that the payment has finished (by
   Javascript, through a callback).
7. The merchant receives the notification and performs the confirmation (commit) of the transaction on its server, to check if the transaction was approved or rejected. This is where the merchant carries out its internal processes after payment confirmation.
8. If the transaction has been approved (response code equals 0), the merchant displays the voucher with the transaction details and / or a thank you page.

### Create a transaction

This operation allows you to create a transaction. Webpay Modal processes the request and delivery
as a result of the operation the transaction token.

 <aside class="notice">
It is important to consider that once this method is invoked, the token that is
delivered has a reduced life span of 5 minutes, after this the
token is expired and cannot be used in a payment.
 </aside>

This method receives three parameters:
- The amount of the transaction
- The purchase order, which is an internal identifier that allows you to identify the transaction
- A sessionId that allows you to identify the user's session. Similar to buyOrder, but this is optional.

 <div class="language-simple" data-multiple-language> </div>

`` `php
use Transbank \ Webpay \ Webpay \ Modal \ Transaction;

$ response = Transaction :: create ($ amount, $ buyOrder, $ sessionId);
`` ''

 <strong> Response create a transaction </strong>

Webpay Modal's response to creation is the transaction token.
We recommend you save this token in your database, associating it with your purchase order, shopping cart, order or whatever you occupy.
to identify your transaction.

 <div class="language-simple" data-multiple-language> </div>

`` `php
$ response-> getToken ();
`` ''

### Show payment form
Once you have the token, you must display the Webpay Modal payment form.
For that, you must include the following `<script>` on your website.

html
 <!-- A) Incluir este para el ambiente de integración -->
 <script type="text/javascript" src="https://webpay3gint.transbank.cl/webpayserver/webpay.js"> </script>

 <!-- B) Incluir este para el ambiente de producción -->
 <script type="text/javascript" src="https://webpay3g.transbank.cl/webpayserver/webpay.js"> </script>
`` ''
For ** integration ** environment, use domain `https: // webpay3gint.transbank.cl` and for ** production * use domain` https: // webpay3g.transbank.cl`

Then, it is necessary to have an `<div>` where the payment form will be mounted:
html
     <div id="webpay_div" style="margin: 0 auto; height:637px !important">
`` ''

Subsequently, the `Webpay.show (...)` function must be executed to display the form. This function should be executed after the div has been created in the DOM (for example, inside `window.onload`, in a script tag almost when closing the` body` tag or something similar).
html
 <div id="webpay_div" style="margin: 0 auto; height:637px !important">
 <script>
    Webpay.show ("webpay_div", "EL_TOKEN_DE_LA_TRANSACCION_CREADA_ACA", success_callback, error_callback);

    function success_callback (token) {
        // Call a backend page by ajax to perform the commit by sending the transaction token.
        // Once the commit is done, and the responderCode of the transaction is 0, then show success message.
    }

    function error_callback (token, errorCode, errorDetail) {
        // Optional: Call a backend page to "inform" that the transaction was rejected (sending the transaction token)
        alert (errorDetail);
    }
 </script>
`` ''

This function receives the following parameters:
- ** Container **: ID of the HTML element where the payment form will be dynamically included
- ** Token **: Token of the transaction, received when creating the transaction.
- ** Success Callback **: This is a Javascript function that will be executed in case of ** success **. It receives as a parameter the
  token of the transaction, which must be used to make the payment confirmation (commit).
- ** Error Callback **: This is a Javascript function that will be executed when an error has occurred. Receive the token, a code of
  error and a description. The trade must take these mistakes as fatal mistakes
  terminals, which will not allow you to continue with this transaction (token is
  invalid), so, in the case of wanting to persist your purchase flow, you must
  restart payment with a new transaction.

The width of this div is responsive, and it shows the mobile view for less or equal viewports
at 659px, on the contrary, it will show the desktop version from 660px. The height is fixed and for the
mobile is 637px, while for desktop it is 435px. It is the responsibility of the
trade check the viewport of your layout, particularly that this version allows the use
of divs with percentage width. So if the mobile or desktop version is deployed
it will depend on the components that may be surrounding the iframe on the trade site.

 <aside class="notice">
Tip: In the integration environment you can use the VISA card 4051885600446623
to do simple tests of success. The CVV is 123 and the expiration date
any higher than the current date For exhaustive tests <a href="https://www.transbankdevelopers.cl/documentacion/como_empezar#ambientes"> see all
test cards in the Environment section </a>.
 </aside>

### Wait for the user to finish the payment flow


Once the cardholder has paid, Webpay Plus will return
the control of the trade by means of Javascript, executing the success function ("Success Callback").
At that time, the merchant must perform the confirmation of the transaction (commit) to validate the status of the payment.

Keeping the code example above, where we use the name `success_callback` for our success function, it would be something like:
html
 <script>
function success_callback (token) {
    console.log ('Committing transaction ...');
    jQuery.post ('http://myexample.cl/modal/commit', {token: token}, function (ajaxResponse) {
      if (ajaxResponse.responseCode === 0) {
        alert ('Purchase approved');
        // Show voucher with transaction data directly on the page, redirect to a success page or whatever is deemed convenient.
        window.location = 'http://myexampleshop.cl/transaction/modal/voucher/' + token;
      } else {
          alert ('The transaction has been rejected. Please try again')
      }
    })
}
 </script>
`` ''
The important thing in the above code is to understand that once the success callback is executed, it does not necessarily mean that the
transaction has been approved, but the payment flow completed successfully.
It is at that moment that we must confirm in the transaction (on our server, in the backend) and in this example code
the token is sent via AJAX to an API within our site. It is in that endpoint where the confirmation is made (it can also be a redirection to another page or something like that).

### Confirm a transaction

This method receives a parameter: The token of the transaction.

`` `php
use Transbank \ Webpay \ Webpay \ Modal \ Transaction;

$ response = Transaction :: commit ($ token);
`` ''


 <div class="language-simple" data-multiple-language> </div>

`` `php
use Transbank \ Webpay \ Webpay \ Modal \ Transaction;

$ response = Transaction :: commit ($ token);
`` ''


 <strong> Response of committing a transaction </strong>

 <div class="language-simple" data-multiple-language> </div>


`` `php
$ response-> getAmount ();
$ response-> getStatus ();
$ response-> getBuyOrder ();
$ response-> getSessionId ();
$ response-> getCardDetail ();
$ response-> getCardNumber ();
$ response-> getAccountingDate ();
$ response-> getTransactionDate ();
$ response-> getAuthorizationCode ();
$ response-> getPaymentTypeCode ();
$ response-> getResponseCode ();
$ response-> getInstallmentsAmount ();
$ response-> getInstallmentsNumber ();
`` ''

If `responseCode` equals` 0`, then the transaction was approved successfully. Any other value will indicate that the payment was declined.
At this time, depending clearly on the responseCode, you can check the status of the transaction and perform the actions you need, such as approving the order in your database, reducing stock, etc.

 <strong> Show voucher </strong>
Continuing with the example, we can use the confirmation response, you can show a receipt or success page to your user in case the responseCode is equal to zero.
If it is different from zero, you can show a message indicating that the transaction was rejected so that you can retry the payment (you must start the process from scratch, creating a new transaction and obtaining a new token)

### Get status of a transaction


This operation allows you to obtain the status of the transaction in the next 7 days from its creation.
Under normal conditions it is probably not required to execute, but in case of an error
Unexpected allows you to know the status and take the corresponding actions.

You must send the `token` of the transaction for which you want to obtain the status.

 <div class="language-simple" data-multiple-language> </div>

`` `php
use Transbank \ Webpay \ Webpay \ Modal \ Transaction;

$ response = Transaction :: getStatus ($ token);
`` ''

 <strong> Status response of a transaction </strong>

To obtain the information contained in the answer you can do it in the following way.

 <div class="language-simple" data-multiple-language> </div>


`` `php
$ response-> getAmount ();
$ response-> getStatus ();
$ response-> getBuyOrder ();
$ response-> getSessionId ();
$ response-> getCardDetail ();
$ response-> getCardNumber ();
$ response-> getAccountingDate ();
$ response-> getTransactionDate ();
$ response-> getAuthorizationCode ();
$ response-> getPaymentTypeCode ();
$ response-> getResponseCode ();
$ response-> getInstallmentsAmount ();
$ response-> getInstallmentsNumber ();
$ response-> getBalance ();
`` ''

### Reverse or Void a transaction


This operation allows any authorized merchant to refund or cancel a
transaction that was generated in Webpay Plus.
You can generate a refund of all or part of the amount of a transaction, depending on the
following business logic invoking this operation will generate a reverse or abort:

* If the amount sent is less than the total amount then a partial cancellation will be executed.
* If the amount sent is equal to the total, then an annulment or reversal will be evaluated. It will be reversed if the time to execute it has not expired, otherwise an annulment will be executed.

Cancellation can be made a maximum of 90 days after the date of the
original transaction.

You can [read more about the cancellation in the information of the
Webpay product] (/ product / webpay # cancellations) to know
more details and restrictions.

 <aside class="notice">
The `Transaction.refund ()` method must always be invoked indicating the code of the merchant that carried out the transaction.
 </aside>

 <div class="language-simple" data-multiple-language> </div>


`` `php
use Transbank \ Webpay \ Webpay \ Modal \ Transaction;

$ response = Transaction :: refund ($ token, $ amount);
`` ''

 <strong> Reverse Response or Cancel </strong>

To obtain the information contained in the answer you can do it in the following way.

 <div class="language-simple" data-multiple-language> </div>


`` `php
$ response-> getAuthorizationCode ();
$ response-> getAuthorizationDate ();
$ response-> getBalance ();
$ response-> getNullifiedAmount ();
$ response-> getResponseCode ();
$ response-> getType ();
`` ''

## Credentials and Environment

For Webpay Modal, the merchant credentials (merchant code and API Key) are pre-configured in the SDKs.

If you do not use an SDK, these is the commercial code and Api Key of the integration environment:

The ** secret key (Api Key Secret) ** is `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`

Product | Commercial Code |
-------- | ------------ |
Webpay Modal | `597055555584`

If you want to point to production you have two options: you can reconfigure the SDK to point to production using the commercial code and API Key or you can configure each call to the following operations to pass an `Options` object in which you configure where you want point.

Example:
`` `php
$ apiKey = 'MyApiKeySEcret_Secret_Key';
$ commerceCode = '5970MICODIGO';
$ environment = 'LIVE'; //Production environment

$ options = new Transbank \ Webpay \ Options ($ apiKey, $ commerceCode, $ environment);
Transaction :: create ($ amount, $ buyOrder, $ sessionId, $ options);
`` ''
All methods can be given an Options object in the last parameter, which allows specifying a trading code, api key and environment.

You can find more information about it [in this link] (/ documentation / how to start # b-using-the-sdk)

## Reconciliation of Transactions

Once you have made transactions in production, there will be a history of
transactions that you can review by entering
[www.transbank.cl] (https://www.transbank.cl/). If you wish you can make a
reconciliation between your system and the report delivered by the portal.

## Integration examples

We put at your disposal a series of repositories on our Github to help you understand the integration in a better way.

[You can see them here](/documentacion/como_empezar#ejemplos)

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
