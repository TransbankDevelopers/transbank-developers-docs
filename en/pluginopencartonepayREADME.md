 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Download our plugin. </h4>
     <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-opencart-onepay/releases/latest"> Download Plugin </a>
     <br>
     <h4> Compatibility </h4>
     <ul>
       <li> OpenCart> = 3.x </li>
       <li> PHP> = 5.6 and <= 7.0 </li>
     </ul>
     <h4> Remember </h4>
     <ol>
       <li> Have your Api Key and Shared Secret </li>
       <li> Have OpenCart installed on your site </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

 <h1 class="toc-ignore"> Onepay OpenCart </h1>
 <h1 style="display: none;"> Onepay </h1>

## Description

This official plugin has been created so that you can easily integrate Onepay into your business, based on OpenCart 3.x.

## Requirements

1. You must have previously installed a version of OpenCart 3

## Plugin installation

1. Go to [https://github.com/TransbankDevelopers/transbank-plugin-opencart-onepay/releasesiexpress(https://github.com/TransbankDevelopers/transbank-plugin-opencart-onepay/releases) and download the latest version available of the plugin.

    Once the plugin is downloaded, go to the OpenCart administration page (usually at [http://mysite.com/admin ](http://mysite.com/admin), [http: // localhost / admin] ( http: // localhost / admin)) and go to Extensions / Installer, indicated below:

    ! [Step 1] (/ images / plug / open / onepay / step1.png)

2. Click on the "Upload" button and select the file that you downloaded in the previous step. Upon completion it will appear that it was installed successfully.

  ! [Step 2] (/ images / plug / open / onepay / step2.png)

## Setting

This plugin has a configuration site that will allow you to enter credentials that Transbank will grant you and you will also be able to generate a diagnostic document in case Transbank requests it.

To access the configuration, you must follow the following steps:

1. Go to the OpenCart administration page (usually at [http://mysite.com/admin] (http://mysite.com/admin), [http: // localhost / admin] (http: // localhost / admin)) and enter username and password.

2. Within the administration site go to (Extensions / Extensions) and filter by "Payments".

    ! [Step 3] (/ images / plug / open / onepay / step3.png)

3. Look down for the "Transbank Onepay" plugin.

    ! [Step 4] (/ images / plug / open / onepay / step4.png)

4. Press the green "+" button to install the plugin.

    ! [Step 5] (/ images / plug / open / onepay / step5.png)

5. It will turn red.

    ! [Step 6] (/ images / plug / open / onepay / step6.png)

6. Press the button to edit the plugin

7. That's it! You are in the plugin configuration screen, you must enter the following information:
   * ** Status **: When activated, Onepay will be available as a means of payment. Be careful that this option is selected when you want users to pay with Onepay.
   * ** Endpoint **: Environment where the transaction is carried out.
   * ** APIKey **: It is what identifies you as a business.
   * ** Shared Secret **: Secret key that authorizes and validates you to make transactions.

  The options available for _Endpoint_ are: "Integration" to carry out tests and certify the installation with Transbank, and "Production" to carry out real transactions once Transbank has approved the trade. Depending on which Endpoint has been selected, the plugin will use one of the two sets of APIKey and Shared Secret as appropriate.

### Test Credentials

For the Integration environment, you can use the following credentials for testing:

* APIKey: `dKVhq1WGt_XapIYirTXNyUKoWTDFfxaEV63-O5jcsdw`
* Shared Secret: `? XW # WOLG ## FBAGEAYSNQ5APD # JF @ $ AYZ`

1. Save the changes by pressing the button! [Save] (/ images / plug / open / onepay / save.png)

   ! [step7] (/ images / plug / open / onepay / step7.png)

2. In addition, you can generate a diagnostic document in case Transbank requests it. To do this, click on "Generate Diagnostic PDF" and this document will automatically download.

  ! [Step 8] (/ images / plug / open / onepay / step8.png)

### Refresh the OpenCart mods system

1. Go to (Extensions / Modifications) and select the "Transbank Onepay" plugin indicated below:

    ! [Step 9] (/ images / plug / open / onepay / step9.png)

2. With the "Transbank Onepay" plugin selected press the "Refresh" button! [Save] (/ images / plug / open / onepay / mod_refresh.png) in the upper right corner.

OpenCart will indicate that the modifications have been successful on the plugin:

  ! [Step 10] (/ images / plug / open / onepay / step10.png)

## Installation test with transaction

In an integration environment it is possible to perform a transaction test using an online payment emulator.

* Enter the trade

  ! [demo 1] (/ images / plug / open / onepay / demo1.png)

* Once logged in, enter any section to add products

  ! [demo 2] (/ images / plug / open / onepay / demo2.png)

* Add a product to the shopping cart:

  ! [demo 3] (/ images / plug / open / onepay / demo3.png)

* Select the shopping cart and then press the [Checkout] button:

  ! [demo 4] (/ images / plug / open / onepay / demo4.png)

* Enter the requested information such as address, shipping method and then select Transbank Onepay payment method, then press the [Continue] button

  ! [demo 5] (/ images / plug / open / onepay / demo5.png)

* Then press the [Confirm Order] button

  ! [demo 6] (/ images / plug / open / onepay / demo6.png)

* Once the button is pressed to start the purchase, the Onepay payment window will be displayed, as seen in the image. Take note of the number that appears as "Purchase code", as you will need it to emulate the payment in the next step (In this example 8660 - 7579):

  ! [demo 7] (/ images / plug / open / onepay / demo7.png)

* In another browser window, enter the payment emulator from [https://onepay.ionix.cl/mobile-payment-emulator/)(https://onepay.ionix.cl/mobile-payment-emulator/), use test@onepay.cl as an email, and the purchase code obtained from the previous screen. Once you have entered the requested data, press the "Start Payment" button:

  ! [demo 8] (/ images / plug / open / onepay / demo8.png)

* If all goes well, the emulator will show options to simulate different situations. To simulate a successful payment, press the `PRE_AUTHORIZED` button. In case you want to simulate a failed payment, press the `REJECTED` button. We will simulate a successful payment by pressing the `PRE_AUTHORIZED` button.

  ! [demo 9] (/ images / plug / open / onepay / demo9.png)

* Go back to the browser window where OpenCart is located and you will be able to check that the payment has been successful.

 ! [demo 10] (/ images / plug / open / onepay / demo10.png)

 ! [demo 11] (/ images / plug / open / onepay / demo11.png)

* Also if you access the administration site section (Sales / Orders) you will be able to see the order created and the details of the data delivered by Onepay.

 ! [demo 12] (/ images / plug / open / onepay / demo12.png)

 ! [demo 13] (/ images / plug / open / onepay / demo13.png)
