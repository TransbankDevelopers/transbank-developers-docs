
 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Download our plugin. </h4>
     <a class="td_btn-more" target="_blank"  href="https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/releases/latest"> Download Plugin </a>
     <br>
     <h4> Compatibility </h4>
     <ul>
       <li> WooCommerce> = 3.2 </li>
       <li> PHP> = 7.0 </li>
     </ul>
     <h4> Remember </h4>
     <ol>
       <li> Have your own trade code and secret key </li>
       <li> Have WooCommerce installed on your site </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

___

 <h1 class="toc-ignore"> WooCommerce | Webpay Plus Plugin </h1>
 <h1 style="display: none;"> Webpay REST </h1>

## Description

This official plugin has been created so that you can easily integrate Webpay into your business, based on WooCommerce.

You are looking at the new REST documentation for this plugin. If you are still using the older SOAP version (the one that asks for certificates and a private key), check out this video to update yourself.

 <div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube.com/embed/9--NHgh07Fw" >
   <div class="container-embed">
     <div class="data-info-url">
       <b> How to switch from SOAP to REST </b>
     </div>
     <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
   </div>
 </div>


## Requirements

You must have previously installed [WooCommerce] (https://woocommerce.com/).
Make sure you have the following modules / extensions enabled for PHP:

* PHP JSON extension (ext-json)
* OpenSSL 1.0.1 or higher
* DOM 2.7.8 or higher
* PHP 7.0 or higher

When installing the plugin, you will be able to check if all these requirements are met, through the diagnostic screen that is included.

_This diagnostic screen is found in the configuration section of the plugin (where you configure your trading code and your ** secret key **). There you will see a tab "Diagnosis" ._

## Installation

As you can see in the video, the steps to install the plugin are as follows:
1. Go to the administration panel of your Wordpress> Plugins> Add New
2. Find the "Transbank Webpay Plus REST" plugin, install it and activate it.

 <div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube.com/embed/IeKb8RreF08" >
   <div class="container-embed">
     <div class="data-info-url">
       <b> How to install the WooCommerce plugin </b>
     </div>
     <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
   </div>
 </div>

### Manual installation (obsolete)
If you cannot install it automatically, you can also download the plugin .zip file and upload it manually to your Wordpress.

1. [Download the plugin .zip file](https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/releases/latest)
2. Upload the zip file in the Plugin> Upload new plugin section in your Wordpress administrator
Detailed installation instructions can be found at the [following link] (https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/blob/master/docs/INSTALLATION.md).

## How to use

Once installed, a new payment method will be added to the checkout process. If it is not activated correctly, check that the plugin is active and that your store is configured in Chilean Pesos.

### Testing environment

Once the plugin is installed, it is configured in the Transbank ** Integration ** environment, so you can perform all the payment tests you need, since real money is not used.
In this environment, only the test credit and debit cards work that you can [find here] (/ documentation / how to start # integration-environment).

### Cancellations

Within the detail of an order, you can make a cancellation or reverse of a payment.
You can see the details of a reversal or cancellation ("refund") [in this link] (https://transbankdevelopers.cl/documentacion/webpay-plus#reversar-o-anular-una-transaccion)

Debit cards only support Reversals (refunds made within the first 30 minutes after the transaction is approved).
Reversals, cancellations and partial cancellations are allowed on credit cards.

In the following video we show you how to carry out the process:
 <div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/ogw2yHcaZ4M" >
   <div class="container-embed">
     <div class="data-info-url">
       <b> Tutorial of "refunds" (41s) </b>
     </div>
     <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
   </div>
 </div>

### Check status

Also in the detail of an order, you can review the current status of a transaction (up to 7 days after the transaction).
You can check if a transaction is authorized, voided, partially voided, or reversed.

In the following video we leave you the instructions to carry out this process
 <div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/AB9eh7BTJUE" >
   <div class="container-embed">
     <div class="data-info-url">
       <b> Transaction query tutorial (02:07) </b>
     </div>
     <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
   </div>
 </div>

## Obtain your secret key (validation process)

To use the plugin in the production environment (where real money is used), you need to have your ** secret key **, which is a special code that is associated with your trading code.
To obtain it you need to pass a validation process, which is [explained here] (/ documentation / how to start # the-validation-process).

This process aims to verify that the merchant is transacting safely and smoothly. This validation is a requirement to leave the merchant in production and a merchant will not be allowed to use the Webpay service productively without having this validation.
At this stage, you must send the evidence to [soporte@transbank.cl] (mailto: soporte@transbank.cl).

Validation form for official plugins: [Download] (https://transbankdevelopers.cl/files/evidencia-integracion-webpay-plugins-rest.docx)

Support will validate the submitted form and, if everything is correct, you will be notified of the approval to go to production, receiving your ** production secret key ** (_Api Key Secret_) and some instructions.

Note: This ** secret key ** is a password for your trading code, so it should not be shared.

## Put into production

 <div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube.com/embed/B9sb7SyROVk" >
   <div class="container-embed">
     <div class="data-info-url">
       <b> How to go to production </b>
     </div>
     <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
   </div>
 </div>

If you already have your production trade code and secret key, you just have to go to the configuration of your plugin ([instructions in this link] (https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/ blob / master / docs / INSTALLATION.md # configuration% C3% B3n)) and place:

* Environment: Production
* Commercial code: your production commercial code
* Api Key: Your secret key

When saving, the plugin will work immediately in a production environment and you will be able to operate with real cards and transactions. You will be asked to make a real transaction in this production environment for $ 50 to finish your process.

## Problems, Doubts, Suggestions

If you have any problems, questions or suggestions, you can contact us in our Slack community, which you can [join here] (https://join-transbankdevelopers-slack.herokuapp.com/)

Additionally, you can check if more businesses have presented any errors or similar doubts in the [_issues_ of the github repository] (https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/issues). If nobody has commented something similar, you can [create a new _issue_] (https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay-rest/issues/new) with your suggestion, bug, problem, etc.
