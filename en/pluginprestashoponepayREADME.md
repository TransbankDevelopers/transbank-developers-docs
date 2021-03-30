
 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Download our plugin. </h4>
     <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-prestashop-onepay/releases/latest"> Download Plugin </a>
     <br>
     <h4> Compatibility </h4>
     <ul>
       <li> Prestashop> = 1.7 </li>
       <li> PHP> = 5.6 and PHP <= 7.2 </li>
     </ul>
     <h4> Remember </h4>
     <ol>
       <li> Have your Api Key and Shared Secret </li>
       <li> Have Prestashop installed on your site </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

 <h1 class="toc-ignore"> Onepay Prestashop </h1>
 <h1 style="display: none;"> Onepay </h1>

## Description

This official plugin has been created so that you can easily integrate Onepay into your business, based on Prestashop.

## Requirements

You must have Prestashop previously installed.

## Plugin Installation

1. Go to [https://github.com/TransbankDevelopers/transbank-plugin-prestashop-onepay/releasesiexpress(https://github.com/TransbankDevelopers/transbank-plugin-prestashop-onepay/releases), and download the latest version available from the plugin.

    Once the plugin is downloaded, enter the Prestashop administration page (usually at _misite.com_ / admin), and go to Modules, Modules and Services, indicated below:

    ! [Step 1] (/ images / plug / prestashop / onepay / step1.png)

2. Click on the button "Upload a module":

    ! [Step 2] (/ images / plug / prestashop / onepay / step2.png)

3. A box will open to upload the previously downloaded module. Proceed to drag the file, or click on "select the file" and select it from your computer:

    ! [Step 3] (/ images / plug / prestashop / onepay / step3.png)

4. Prestashop will proceed to install the module. Once finished, you will be told that the module was installed, and when this happens you must click on the "Configure" button:

    ! [Step 4] (/ images / plug / prestashop / onepay / step4.png)

5. You will be redirected to the list of available modules. Search for "Onepay", and press the "Configure" button:

    ! [Step 5] (/ images / plug / prestashop / onepay / step5.png)

## Setting

This plugin has a configuration site that will allow you to enter credentials that Transbank will grant you, and you will also be able to generate a diagnostic document in case Transbank requests it.

To access the configuration, you must follow the following steps:

1. Go to the Prestashop administration page (usually at _mysite.com_ / admin), and then go to Modules, Modules and Services.

    ! [Step 1] (/ images / plug / prestashop / onepay / step1.png)

2. Search for "Onepay", and press the "Configure" button:

    ! [Step 2] (/ images / plug / prestashop / onepay / step5.png)

3. That's it! You are in the plugin configuration screen, you must enter the following information:
      * ** Activate Onepay **: When activated, Onepay will be available as a means of payment. Take care that this option is checked when you want users to pay with Onepay.
      * ** APIKey **: It is what identifies you as a business.
      * ** Shared Secret **: Secret key that authorizes and validates you to make transactions.
      * ** Endpoint **: Environment where the transaction is carried out.

    The options available for _Endpoint_ are: "Integration" to carry out tests and certify the installation with Transbank, and "Production" to carry out real transactions once Transbank has approved the trade.

    ! [Step 3] (/ images / plug / prestashop / onepay / step6.png)

4. In addition, you can generate a diagnostic document in case Transbank requests it. To do this, click on "Generate Diagnostic PDF", and this document will automatically download.

  ! [Step 4] (/ images / plug / prestashop / onepay / step7.png)

## Test Credentials

For the Integration environment, you can use the following credentials for testing:

* APIKey: `dKVhq1WGt_XapIYirTXNyUKoWTDFfxaEV63-O5jcsdw`
* Shared Secret: `? XW # WOLG ## FBAGEAYSNQ5APD # JF @ $ AYZ`

## Installation test with transaction

In an integration environment it is possible to carry out a transaction test using an online payment emulator.

* To do this, add a product to the shopping cart:
  ! [Step 1] (/ images / plug / prestashop / onepay / emu1.png)

* Then go to the checkout to start the payment flow, clicking on "Checkout":
  ! [Step 2] (/ images / plug / prestashop / onepay / emu2.png)
  ! [Step 3] (/ images / plug / prestashop / onepay / emu3.png)

* On the checkout page, enter the data required to make the purchase:
  ! [Step 4] (/ images / plug / prestashop / onepay / emu4.png)

* Select the option "Pay with Onepay", and press the "Order with payment obligation" button.
  ! [Step 5] (/ images / plug / prestashop / onepay / emu5.png)

* Once the button is pressed to start the purchase, the Onepay payment window will be displayed, as seen in the image. Take note of the number that appears as "Purchase Code", as you will need it to emulate the payment in the next step:
  ! [Step 6] (/ images / plug / prestashop / onepay / emu6.png)

* In another browser window, enter the payment emulator from [https://onepay.ionix.cl/mobile-payment-emulator/)(https://onepay.ionix.cl/mobile-payment-emulator/), use test@onepay.cl as an email, and the purchase code obtained from the previous screen. Once you have entered the requested data, press the "Start Payment" button:
  ! [Step 7] (/ images / plug / prestashop / onepay / emu7.png)

* If all goes well, the emulator will show options to simulate different situations. To simulate a successful payment, press the `PRE_AUTHORIZED` button. In case you want to simulate a failed payment, press the `REJECTED` button. We will simulate a successful payment by pressing the `PRE_AUTHORIZED` button.
  ! [Step 8] (/ images / plug / prestashop / onepay / emu8.png)

* Go back to the browser window where Prestashop is located, and you will be able to check that the payment has been successful.
 ! [Step 9] (/ images / plug / prestashop / onepay / emu9.png)

* You will automatically be redirected to Prestashop, with the final verification of the payment.
 ! [Step 10] (/ images / plug / prestashop / onepay / emu10.png)
