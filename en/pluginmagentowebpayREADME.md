
 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Download our plugin. </h4>
     <a class="td_btn-more" target="_blank"  href="https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/releases/latest"> Download Plugin </a>
     <br>
     <h4> Compatibility </h4>
     <ul>
       <li> Magento2 2.2.X and 2.3.X </li>
       <li> PHP> = 5.6 </li>
     </ul>
     <h4> Remember </h4>
     <ol>
       <li> Have your own trade code and secret key </li>
       <li> Have Magento2 installed on your site </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

___

 <aside class="notice">
You are looking at the <strong> new REST documentation </strong> for this plugin. If you want to see reference of the previous version
(SOAP) make [click here] (/ plugin / magento / webpay-soap)
 </aside>

 <h1 class="toc-ignore"> Webpay REST Magento2 </h1>
 <h1 style="display: none;"> Webpay REST </h1>

## Description

This official plugin has been created so that you can easily integrate Webpay into your business, based on Magento2.
You can find the Open Source repository for this plugin [here] (https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest).

## Requirements

You must have previously installed [Magento2] (https://magento.com/).
Make sure you have the following modules / extensions enabled for PHP:

* Soap
* OpenSSL 1.0.1 or higher
* SimpleXML
* DOM 2.7.8 or higher
* PHP 5.6 or higher

When installing the plugin, you will be able to check if all these requirements are met, through the diagnostic screen that is included.

_This diagnostic screen is found in the configuration section of the plugin (where you configure your trading code and your ** secret key **). There you must press the button "System Information" _

## Installation

### A) With composer (recommended)

This is the recommended option to install the package in Magento.

First, enter the Magento root directory with the terminal and install the package using [Composer] (https://getcomposer.org/).

bash
composer require transbank / webpay-magento2-rest
`` ''

When done, run the command:

bash
magento module: enable Transbank_Webpay --clear-static-content
`` ''

and finally:

bash
magento setup: upgrade && magento setup: di: compile && magento setup: static-content: deploy
`` ''

You can find detailed installation instructions at the [following link] (https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/blob/master/docs/INSTALLATION.md) of the repository.

### B) As file

1. [Download the plugin .zip file](https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/releases/latest)
2. Unzip the file in the folder app / code / Transbank / WebpayRest
3. Edit the app / etc / config.php file and add `Transbank_Webpay_Rest => 1` to the list of modules.
4. Enter the magento admin and delete the cache. (System> Cache Management)
5. The module should be installed.

## Testing environment

Once the plugin is installed, it is configured in the Transbank ** Integration ** environment, so you can perform all the payment tests you need, since real money is not used.
In this environment, only the test credit and debit cards work that you can [find here] (/ documentation / how to start # integration-environment).

## Obtain your secret key (validation process)

To use the plugin in the production environment (where real money is used), you need to have your ** secret key **, which is a special code that is associated with your trading code.
To obtain it you need to pass a validation process, which is [explained here] (https://transbankdevelopers.cl/documentacion/como_empezar#puesta-en-produccion).

At the end of this validation process, you will get your ** secret key **.
Note: This ** secret key ** is like the password of your trading code, so you should not share it. It is used to identify that your business is who is actually performing each operation (transaction, cancellation of a payment, etc).

## Put into production

 <div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube.com/embed/B9sb7SyROVk" >
   <div class="container-embed">
     <div class="data-info-url">
       <b> How to go to production </b>
     </div>
     <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
   </div>
 </div>

If you already have your production trade code and secret key, you just have to go to the configuration of your plugin ([instructions in this link] (https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/ blob / master / docs / INSTALLATION.md # configuration% C3% B3n)) and place:

* Environment: Production
* Commercial code: your production commercial code
* Api Key: Your secret key

When saving, the plugin will work immediately in a production environment and you will be able to operate with real cards and transactions.

## Problems, Doubts, Suggestions

If you have any problems, questions or suggestions, you can contact us in our Slack community, which you can [join here] (https://join-transbankdevelopers-slack.herokuapp.com/)
Additionally, you can check if more businesses have presented any similar errors / doubts in the [_issues_ of the github repository] (https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/issues). If nobody has commented something similar, you can [create a new _issue_] (https://github.com/TransbankDevelopers/transbank-plugin-magento2-webpay-rest/issues/new) with your suggestion, bug, problem, etc.
