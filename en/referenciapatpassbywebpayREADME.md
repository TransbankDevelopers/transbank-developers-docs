# PatPass Rest

## Environments and Credentials

The PatPass by Webpay REST API is protected to ensure that only merchants authorized by Transbank make use of the available operations. Security is implemented through the following mechanisms:

* Secure channel through TLSv1.2 for customer communication with Webpay.
* Authentication and authorization through the exchange of Tbk-Api-Key-Id and Tbk-Api-Key-Secret headers.

### Production environment

Production endpoint URLs are hosted within
 <https://webpay3g.transbank.cl/>.

java
// This SDK is not yet available
`` ''

`` `php
// This SDK is not yet available
`` ''

csharp
// This SDK is not yet available
`` ''

ruby
# This SDK is not yet available
`` ''

python
# This SDK is not yet available
`` ''

http
Host: https://webpay3g.transbank.cl
`` ''

### Integration Environment

java
// This SDK is not yet available
`` ''

`` `php
// This SDK is not yet available
`` ''

csharp
// This SDK is not yet available
`` ''

ruby
# This SDK is not yet available
`` ''

python
# This SDK is not yet available
`` ''

http
Host: https://webpay3gint.transbank.cl
`` ''

Integration endpoint URLs are hosted within
 <https://webpay3gint.transbank.cl/>.

See [documentation for test cards that work on
the integration environment] (/ documentation / how to start # environments).

### Trade Credentials

java
// This SDK is not yet available
`` ''

`` `php
// This SDK is not yet available
`` ''

csharp
// This SDK is not yet available
`` ''

ruby
# This SDK is not yet available
`` ''

python
# This SDK is not yet available
`` ''

http
Tbk-Api-Key-Id: Coming soon ...
Tbk-Api-Key-Secret: Coming soon ...
Content-Type: application / json
`` ''

All the requests you make must include the commercial code and the key
secret issued by Transbank, both acting as the credentials that authorize
different operations.

 <aside class="notice">
Please note that your trade code (s) in production environment are not
equal to those delivered for the integration environment.
 </aside>

In [the GitHub repository
`transbank-webpay-credenciales`] (https://github.com/TransbankDevelopers/transbank-webpay-credenciales/)
you can find [store codes and secret keys to try
in integration even if you don't have your own code yet
commerce] (https://github.com/TransbankDevelopers/transbank-webpay-credenciales/tree/master/integracion).

> The SDKs already include those trade codes and secret keys
which work in the integration environment, so you can get
> quickly a configuration ready to do your first tests in said
> environment:

java
// This SDK is not yet available
`` ''

`` `php
// This SDK is not yet available
`` ''

csharp
// This SDK is not yet available
`` ''

ruby
# This SDK is not yet available
`` ''

python
# This SDK is not yet available
`` ''

http

`` ''

## PatPass by Webpay Normal

java
// This SDK is not yet available
`` ''

`` `php
// This SDK is not yet available
`` ''

csharp
// This SDK is not yet available
`` ''

ruby
# This SDK is not yet available
`` ''

python
# This SDK is not yet available
`` ''

http

`` ''

A PatPass by WebPay authorization transaction corresponds to an enrollment request for a recurring credit card payment, where the first payment is resolved instantly, and subsequent payments are scheduled to be executed month by month. PatPass by WebPay has an expiration or expiration date, which must be provided along with other information for this transaction. The transaction can be made in Dollars and Pesos, for the latter case it is possible to send the amount in UF and WebPay will convert to pesos at the time of charging the cardholder.

The payment flow in PatPass by WebPay starts from the merchant, where the Cardholder selects the product or service. Once this is done, choose to pay with PatPass by WebPay, where the payment form is displayed and the required data is completed, giving way to the Cardholder authentication process in order to validate that the card is being used by the holder. Once the authentication is resolved, the payment is authorized and if it is approved, an electronic payment receipt is issued and the system proceeds to generate the inscription in charge of the monthly recurrence.

Among the most relevant attributes we can mention:

* Allows secure and online transactions over the Internet.
* In transactions with PatPass by Webpay, the cardholder is requested to authenticate with their issuer, thus protecting the merchant from possible fraud or unknown purchases.
* Security is reinforced through the use of secure servers, protected with TLS 1.2

### Flow in case of success

For the cardholder, the page flow for the transaction is as follows:

! [PatPass by Webpay Normal page flow] (/ images / page-flow-webpay.png)

From a technical point of view, the sequence is as follows:

! [PatPass by Webpay Normal sequence diagram] (/ images / webpay-sequence-diagram.png)

1. Once the goods or services have been selected, the cardholder decides to pay through PatPass by WebPay.
2. The merchant initiates a transaction in Webpay, invoking the `Transaction.create ()` method.
3. Webpay processes the request and delivers, as a result of the operation, the transaction token and redirection URL to which the cardholder must be redirected.
4. Commerce redirects the cardholder to Webpay, with the transaction token to the URL indicated in point 3. The redirection is carried out by sending the token in variable `token_ws` by POST method.
5. The cardholder's Web browser makes an HTTPS request to Webpay, based on the redirection generated by the merchant in point 4.
6. Webpay responds to the request by displaying the Webpay payment form. From this point the communication is between Webpay and the cardholder, without interfering with the trade. The PatPass by Webpay payment form displays, among other things, the transaction amount, subscription termination date, merchant information such as name and logo, and the option to pay through credit.
7. Cardholder enters card details, clicks pay.
8. WebPay processes the authorization request (first bank authentication and then the authorization of the transaction), and if everything is successful, it goes through the registration process of PatPass by WebPay.
9. Once the authorization is resolved, WebPay returns control to the merchant, performing an HTTPS redirection to the merchant's transition page, where the transaction token is sent by POST method in the `token_ ws` variable. The merchant must implement the reception of this variable.
10. The cardholder's Web browser makes an HTTPS request to the merchant's site, based on the redirection generated by WebPay in point 9.
11. The merchant site receives the `token_ws` variable and invokes the second one
    Web method to confirm and get the enrollment result. The result of the authorization may be consulted later with the
    previously mentioned variable.
12. Commerce receives the confirmation result.

    <aside class="warning">
    En la versión anterior de WebPay, había que invocar `acknowledgeTransaction()`
    para informar a WebPay que se había recibido el resultado la transacción sin
    problemas. Ahora no es necesario, ya que ésto se realiza de forma automática
    una vez que se confirma la transacción.  Además ya no se debe mostrar el voucher
    de Transbank, solo debe mostrarse desde el sitio del comercio.
    </aside>

13. Commerce site displays voucher with registration data.

### Flow if user aborts payment

! [Sequence diagram if user aborts
payment] (/ images / diagram-sequence-webpay-abort.png)

If the cardholder cancels the transaction in the Webpay payment form, the flow changes and the steps are as follows:

1. Once the goods or services have been selected, the cardholder decides to pay through PatPass by WebPay.
2. The merchant initiates a transaction in Webpay, invoking the `Transaction.create ()` method.
3. Webpay processes the request and delivers, as a result of the operation, the transaction token and redirection URL to which the cardholder must be redirected.
4. Commerce redirects the cardholder to Webpay, with the transaction token to the URL indicated in point 3. The redirection is carried out by sending the token in variable `token_ws` by POST method.
5. The cardholder's Web browser makes an HTTPS request to Webpay, based on the redirection generated by the merchant in point 4.
6. Webpay responds to the request by displaying the Webpay payment form. From this point the communication is between Webpay and the cardholder, without interfering with the trade. The PatPass by Webpay payment form displays, among other things, the transaction amount, subscription termination date, merchant information such as name and logo, and the option to pay through credit.
7. Cardholder clicks void in PatPass by WebPay form.
8. Webpay returns control to the merchant, performing an HTTPS redirection to the ** merchant return page **, where the transaction token is sent by POST method in the variable `TBK_TOKEN` in addition to the variables` TBK_ORDEN_COMPRA` and ` TBK_ID_SESION`.

    <aside class="warning">
    Nota que el nombre de las variables recibidas es diferente. En lugar de `token_ws` acá el token viene en la variable `TBK_TOKEN`.
    </aside>

9. Trading with the variable `TBK_TOKEN` should invoke the method
   `Transaction.commit ()`, to get the authorization result. In
   this case should get an exception, as the payment was aborted.
10. The merchant must inform the cardholder that their payment was not completed.

### Create a PatPass by Webpay Normal transaction

To create a transaction, just call the `Transaction.create ()` method.

 <strong> Transaction.create () </strong>

Allows you to initialize a transaction in PatPass by Webpay. In response to the invocation, a token is generated that uniquely represents a transaction.

 <aside class="notice">
It is important to consider that once this method is invoked, the token that is delivered has a reduced life span of 5 minutes, after which the token is expired and cannot be used in a payment.
 </aside>

java
// This SDK is not yet available
`` ''

`` `php
// This SDK is not yet available
`` ''

csharp
// This SDK is not yet available
`` ''

ruby
# This SDK is not yet available
`` ''

python
# This SDK is not yet available
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions

{
  "buy_order": "OrdenCompra12345678",
  "session_id": "sesion1234557545",
  "amount": 10000,
  "return_url": "http://www.comercio.cl/webpay/retorno",
  "wpm_detail": {
    "service_id": "123345567",
    "card_holder_id": "12345",
    "card_holder_name": "Juan",
    "card_holder_last_name1": "Perez",
    "card_holder_last_name2": "Gonzalez",
    "card_holder_mail": "juan.perez@gmail.com",
    "cellphone_number": "9912345678",
    "expiration_date": "2019-03-20T20: 18: 20Z",
    "commerce_mail": "contacto@comercio.cl",
    "uf_flag": false
  }
}
`` ''

 <strong> Parameters Transaction.create </strong>

Name <br> <i> type </i> | Description
------   | -----------
buyOrder <br> <i> String </i> | Store purchase order. This number must be unique for each transaction. Maximum length: 26. The purchase order can have: Numbers, letters, uppercase and lowercase, and the signs <code> & # 124; _ = &%., ~: /? [+! @ ()> - </code>. Maximum length: 26
sessionId <br> <i> String </i> | (Optional) Session identifier, internal trade use, this value is returned at the end of the transaction. Maximum length: 61
amount <br> <i> Number </i> | Amount of the transaction. Maximum 2 decimal places for USD and UF. Maximum length: 17
returnURL <br> <i> String </i> | URL of the merchant, to which Webpay will redirect after the authorization process. Maximum length: 255
wpmDetail <br> <i> wpmDetail </i> | Object containing data associated with the PatPass by WebPay enrollment.
wpmDetail.serviceId <br> <i> String </i> | Corresponds to the service identifier with which the merchant identifies its customer. Maximum length: 255
wpmDetail.cardHolderId <br> <i> String </i> | RUT of the cardholder. Maximum length: 255. Format: NNNNNNNNA
wpmDetail.cardHolderName <br> <i> String </i> | Cardholder name. Maximum length: 255
wpmDetail.cardHolderLastName1 <br> <i> String </i> | Cardholder paternal last name. Maximum length: 255
wpmDetail.cardHolderLastName2 <br> <i> String </i> | Cardholder maternal last name. Maximum length: 255
wpmDetail.cardHolderMail <br> <i> String </i> | Cardholder email. Maximum length: 255
wpmDetail.cellPhoneNumber <br> <i> String </i> | Cardholder cell phone number. Maximum length: 255
wpmDetail.expirationDate <br> <i> DateTime </i> | PatPass by WebPay expiration date, corresponds to the last payment. Length: 10. Format YYYY-MM-DD
wpmDetail.commerceMail <br> <i> String </i> | E-mail commerce. Maximum length: 50. The SDKs automatically take care of this parameter from the merchant email entered in the configuration used to initiate the transaction
wpmDetail.ufFlag <br> <i> Boolean </i> | Value in true indicates that the amount sent is expressed in UF, value in false indicates that value is expressed in pesos or dollar as appropriate

 <strong> Transaction.create response </strong>

java
// This SDK is not yet available
`` ''

`` `php
// This SDK is not yet available
`` ''

csharp
// This SDK is not yet available
`` ''

ruby
# This SDK is not yet available
`` ''

python
# This SDK is not yet available
`` ''

http
200 OK
Content-Type: application / json

{
 "token": "e9d555262db0f989e49d724b4db0b0af367cc415cde41f500a776550fc5fddd3",
 "url": "https://webpay3gint.transbank.cl/webpayserver/initTransaction"
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> |Token of the transaction. Length: 64.
url <br> <i> String </i> | PatPass by Webpay payment form URL. Maximum length: 256.

### Confirm a PatPass by Webpay Normal transaction

When the merchant takes back control via `returnURL` you can commit a transaction using the` Transaction.commit () `method.

 <strong> Transaction.commit () </strong>

It allows you to obtain the result of the transaction once Webpay has resolved your financial authorization.

java
// This SDK is not yet available
`` ''

`` `php
// This SDK is not yet available
`` ''

csharp
// This SDK is not yet available
`` ''

ruby
# This SDK is not yet available
`` ''

python
# This SDK is not yet available
`` ''

http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/[token}
Tbk-Api-Key-Id: Coming soon ...
Tbk-Api-Key-Secret: Coming soon ...
Content-Type: application / json
`` ''

 <strong> Transaction.commit parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> |Token of the transaction. Length: 64.

 <strong> Transaction.commit response </strong>

java
// This SDK is not yet available
`` ''

`` `php
// This SDK is not yet available
`` ''

csharp
// This SDK is not yet available
`` ''

ruby
# This SDK is not yet available
`` ''

python
# This SDK is not yet available
`` ''

http
200 OK
Content-Type: application / json

{
  "vci": "TSY",
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "OrdenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
      "card_number": "6623"
  },
  "accounting_date": "0522",
  "transaction_date": "2019-05-22T16: 41: 21.063Z",
  "authorization_code": "1213",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
VCI <br> <i> String </i> | Result of cardholder authentication. It can take the value TSY (Authentication successful), TSN (Authentication failed), TO (Maximum time exceeded for authentication), ABO (Aborted authentication by cardholder), U3 (Internal authentication error), NP (Does not participate, probably because a foreign card that does not participate in the 3DSecure program), ACS2 (Foreign Authentication Failed). It can be empty if the transaction was not authenticated. Maximum length: 3. This field is supplementary additional information to the `responseCode` but the merchant ** does not ** have to validate this field. Because new authentication mechanisms are constantly being added that translate into new values for this field that are not necessarily documented. (In the case of international cards that do not provide 3D-Secure, the merchant's decision to accept them or not is made at the merchant's configuration level in Transbank and must be discussed with the merchant's executive). Maximum length: 64
amount <br> <i> Number </i> | Amount of the transaction. Integer format for transactions in weight and decimal for transactions in dollars and UF maximum 2 decimal places. Maximum length: 17
status <br> <i> String </i> | Transaction status (AUTHORIZED, FAILED). Maximum length: 64
buyOrder <br> <i> String </i> | Purchase order from the store indicated in `Transaction.create ()`. Maximum length: 26
sessionId <br> <i> String </i> |Session identifier, the same one originally sent by the merchant in `Transaction.create ()`. Maximum length: 61.
cardDetails <br> <i> carddetails </i> | Object that represents the cardholder's credit card data.
cardDetails.cardNumber <br> <i> String </i> |Last 4 numbers of the cardholder's credit card. Only for businesses authorized by Transbank is the full number sent. Maximum length: 19.
accountingDate <br> <i> String </i> | Authorization date. Length: 4, MMDD format
transactionDate <br> <i> String </i> | Date and time of authorization. Length: 6, format: MMDDHHmm
authorizationCode <br> <i> String </i> | Transaction authorization code Maximum length: 6
paymentTypeCode <br> <i> String </i> | [Payment type] (/ product / webpay # payment-types) of the transaction. <br> VD = Sale Debit. <br> VN = Normal Sale. <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Installments without interest <br> VP = Prepaid Sale.
responseCode <br> <i> String </i> | Authorization response code. Possible values: <br> 0 = Transaction approved. <br> -1 = Transaction rejection. <br> -2 = Transaction must be retried. <br> -3 = Transaction error. <br> -4 = Transaction rejection. <br> -5 = Rejection due to rate error. <br> -6 = Exceeds maximum monthly quota. <br> -7 = Exceeds daily limit per transaction. <br> -8 = Item not authorized. <br> -100 Rejection due to enrollment of PatPass by Webpay.
installmentsNumber <br> <i> Number </i> | Amount of fees. Maximum length: 2
