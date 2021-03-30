
 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Download our plugin. </h4>
     <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-onepay/releases/latest"> Download Plugin </a>
     <br>
     <h4> Compatibility </h4>
     <ul>
       <li> Woocommerce> = 3.2 </li>
       <li> PHP> = 5.6 and PHP <= 7.2 </li>
     </ul>
     <h4> Remember </h4>
     <ol>
       <li> Have your Api Key and Shared Secret </li>
       <li> Have WooCommerce installed on your site </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

 <h1 class="toc-ignore"> Onepay WooCommerce </h1>
 <h1 style="display: none;"> Onepay </h1>

## Description

This official plugin has been created so that you can easily integrate Onepay into your business, based on WooCommerce.

## Requirements

You must have previously installed Wordpress, and WooCommerce.

## Plugin Installation

1. Go to [https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-onepay/releasesiexpress(https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-onepay/releases), and download the latest version available from the plugin.

   Once the plugin is downloaded, go to the Wordpress administration page (usually at _mysite.com_ / wp-admin), and go to Plugins, Add new, indicated below:

   ! [Step 1] (/ images / plug / woo / onepay / step1.png)

2. Click on the "Upload plugin" button, and select the file that you downloaded in the previous step. After that, hit the "Install Now" button.

   ! [Step 2] (/ images / plug / woo / onepay / step2.png)

3. Once the previous step is done, Wordpress will install the Onepay plugin. When finished, you must activate the plugin by clicking the "Activate plugin" button.

  ! [Step 3] (/ images / plug / woo / onepay / step3.png)

## Setting

This plugin has a configuration site that will allow you to enter credentials that Transbank will grant you, and you will also be able to generate a diagnostic document in case Transbank requests it.

To access the configuration, you must follow the following steps:

1. Head over to the Wordpress administration page (usually at _missite.com_ / wp-admin), and then go to WooCommerce, Settings.

    ! [Step 1] (/ images / plug / woo / onepay / step4.png)

2. You will arrive at the WooCommerce configuration page, now you must click on "Payments".

   ! [Step 2] (/ images / plug / woo / onepay / step5.png)

3. On this screen you will see all the available payment methods. Look for the "Onepay" plugin, and click on the title.

    ! [Step 3] (/ images / plug / woo / onepay / step6.png)

4. That's it! You are in the plugin configuration screen, you must enter the following information:
     * ** Activate Onepay **: When activated, Onepay will be available as a means of payment. Take care that this option is checked when you want users to pay with Onepay.
     * ** APIKey **: It is what identifies you as a business.
     * ** Shared Secret **: Secret key that authorizes and validates you to make transactions.
     * ** Endpoint **: Environment where the transaction is carried out.

  The options available for _Endpoint_ are: "Integration" to carry out tests and certify the installation with Transbank, and "Production" to carry out real transactions once Transbank has approved the trade.

  In addition, you can generate a diagnostic document in case Transbank requests it. To do this, click on "Generate Diagnostic PDF", and this document will automatically download.

  ! [Step 4] (/ images / plug / woo / onepay / step7.png)

## Test Credentials

For the Integration environment, you can use the following credentials for testing:

* APIKey: `dKVhq1WGt_XapIYirTXNyUKoWTDFfxaEV63-O5jcsdw`
* Shared Secret: `? XW # WOLG ## FBAGEAYSNQ5APD # JF @ $ AYZ`

## Installation test with transaction

In an integration environment it is possible to carry out a transaction test using an online payment emulator.

* To do this, add a product to the shopping cart:
  ! [Step 1] (/ images / plug / woo / onepay / emu1.png)

* Then go to checkout to start the payment flow:
  ! [Step 2] (/ images / plug / woo / onepay / emu2.png)

* On the checkout page, first verify that Onepay is available as a means of payment. Select it, and then place the purchase order to initiate the payment.
  ! [Step 3] (/ images / plug / woo / onepay / emu3.png)

* Once the button is pressed to start the purchase, the Onepay payment window will be displayed, as seen in the image. Take note of the number that appears as "Purchase Code", as you will need it to emulate the payment in the next step:
  ! [Step 4] (/ images / plug / woo / onepay / emu4.png)

* In another browser window, enter the payment emulator from [https://onepay.ionix.cl/mobile-payment-emulator/)(https://onepay.ionix.cl/mobile-payment-emulator/), use test@onepay.cl as an email, and the purchase code obtained from the previous screen. Once you have entered the requested data, press the "Start Payment" button:
  ! [Step 5] (/ images / plug / woo / onepay / emu5.png)

* If all goes well, the emulator will show options to simulate different situations. To simulate a successful payment, press the `PRE_AUTHORIZED` button. In case you want to simulate a failed payment, press the `REJECTED` button. We will simulate a successful payment by pressing the `PRE_AUTHORIZED` button.
  ! [Step 6] (/ images / plug / woo / onepay / emu6.png)

* Go back to the browser window where WooCommerce is located, and you can check that the payment has been successful.
 ! [Step 6] (/ images / plug / woo / onepay / emu7.png)

* You will automatically be redirected to WooCommerce, with the final verification of the payment.
 ! [Step 7] (/ images / plug / woo / onepay / emu8.png)
