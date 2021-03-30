# Oneclick

___

 <aside class="notice">
You are looking at the <strong> new REST reference </strong>. If you want to see the previous reference (deprecated)
(SOAP) make [click here] (/ reference / webpay-soap)
 </aside>

## Environments and Credentials

The Webpay REST API is protected to guarantee that only merchants authorized by Transbank make use of the available operations. Security is implemented through the following mechanisms:

* Secure channel through TLSv1.2 for customer communication with Webpay.
* Authentication and authorization through the exchange of headers `Tbk-Api-Key-Id` (commercial code) and` Tbk-Api-Key-Secret` (secret key).

### Production environment

Production endpoint URLs are hosted within
 <https://webpay3g.transbank.cl/>.

java
// Host: https://webpay3g.transbank.cl
`` ''

`` `php
// Host: https://webpay3g.transbank.cl
`` ''

csharp
// Host: https://webpay3g.transbank.cl
`` ''

ruby
# Host: https://webpay3g.transbank.cl
`` ''

python
# Host: https://webpay3g.transbank.cl
`` ''

http
Host: https://webpay3g.transbank.cl
`` ''

### Integration Environment

Integration endpoint URLs are hosted within
 <https://webpay3gint.transbank.cl/>.

java
// Host: https://webpay3gint.transbank.cl
`` ''

`` `php
// Host: https://webpay3gint.transbank.cl
`` ''

csharp
// Host: https://webpay3gint.transbank.cl
`` ''

ruby
# Host: https://webpay3gint.transbank.cl
`` ''

python
# Host: https://webpay3g.transbank.cl
`` ''

http
Host: https://webpay3gint.transbank.cl
`` ''

### Cards and test users

See [documentation for test cards that work on
the integration environment] (/ documentation / how to start # environments).

### Trade credentials

java
// Tbk-Api-Key-Id: Trade code
// Tbk-Api-Key-Secret: Secret key
// Content-Type: application / json
`` ''

`` `php
// Tbk-Api-Key-Id: Trade code
// Tbk-Api-Key-Secret: Secret key
// Content-Type: application / json
`` ''

csharp
// Tbk-Api-Key-Id: Trade code
// Tbk-Api-Key-Secret: Secret key
// Content-Type: application / json
`` ''

ruby
# Tbk-Api-Key-Id: Trade Code
# Tbk-Api-Key-Secret: Secret key
# Content-Type: application / json
`` ''

python
# Tbk-Api-Key-Id: Trade Code
# Tbk-Api-Key-Secret: Secret key
# Content-Type: application / json
`` ''

http
Tbk-Api-Key-Id: Commercial Code
Tbk-Api-Key-Secret: Secret key
Content-Type: application / json
`` ''

All the requests you make must include the commercial code and the key
secret issued by Transbank, both acting as the credentials that authorize
different operations.

 <aside class="notice">
Please note that your trade code (s) in production environment are not
equal to those delivered for the integration environment.
 </aside>

### Trade Codes

In the documentation you can check [all commercial codes] (/ documentation / como_empezar # commercial-codes) of the integration environment

> The SDKs already include those trade codes and secret keys
which work in the integration environment, so you can get
> quickly a configuration ready to do your first tests in said
> environment:

## Oneclick Mall

Check out the [Oneclick Mall documentation] (/ documentation / oneclick) for more information on how it works
the product and have more details on how to make your integration.

### Create an Enrollment
You can check more details of this operation in [its documentation] (/ documentation / oneclick # create-an-enrollment)

Lets get started with the enrollment process.

java
OneclickMallInscriptionStartResponse response = OneclickMall.Inscription.start (userName, email, responseUrl);
`` ''

`` `php
use Transbank \ Webpay \ Oneclick;

$ response = MallInscription :: start ($ userName, $ email, $ responseUrl);
`` ''

csharp
using Transbank.Webpay.Oneclick;

var response = Inscription.Start (userName, email, returnUrl);
`` ''

ruby
response = Transbank :: Webpay :: Oneclick :: MallInscription :: start (
  user_name: user_name,
  email: email,
  response_url: response_url
)
`` ''

python
MallInscription.start (
        user_name = user_name,
        email = email,
        response_url = response_url)
`` ''

javascript
const Oneclick = require ("transbank-sdk"). Oneclick; // CommonJS
import {Oneclick,} from 'transbank-sdk'; // ES6 Modules

const response = await Oneclick.MallInscription.start (
  userName, email, responseUrl
);
`` ''

http
POST /rswebpaytransaction/api/oneclick/v1.0/inscriptions

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
 "username": "juanperez",
 "email": "juan.perez@gmail.com",
 "response_url": "http://www.comercio.cl/return_inscription"
}
`` ''

 <strong> Parameters Create an entry </strong>

Name <br> <i> type </i> | Description
------   | -----------
username <br> <i> String </i> | User identifier registered in the store. Maximum length: 40.
email <br> <i> String </i> | Email of the user registered in the store. Maximum length: 100.
response_url <br> <i> String </i> | URL of the merchant to which Webpay will redirect after the registration process. Maximum length: 255.

 <strong> Response Create an entry </strong>

java
response.getToken ();
response.getUrlWebpay ();
`` ''

`` `php
$ response-> getToken ();
$ response-> getUrlWebpay ();
`` ''

csharp
response.Token;
response.UrlWebpay
`` ''

ruby
response.token
response.url_webpay
`` ''

python
response.token
response.url_webpay
`` ''

javascript
response.token
response.url_webpay
`` ''

http
200 OK
Content-Type: application / json

{
  "token": "e128a9c24c0a3cbc09223973327b97c8c474f6b74be509196cce4caf162a016a",
  "url_webpay": "https://webpay3g.transbank.cl/webpayserver/bp_inscription.cgi"
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> |Unique identifier of the registration process. Length: 64.
url_webpay <br> <i> String </i> | Webpay URL to start enrollment. Length: 255.

 <aside class="notice">
Once this webservice is called, the user must be redirected via
POST to `urlInscriptionForm` with parameter` TBK_TOKEN` equal to token.
 </aside>

### Confirm an enrollment

It allows to finish the registration process by obtaining the user tbk.
More information in [the documentation] (/ documentation / oneclick).

java
final OneclickMallInscriptionFinishResponse response = OneclickMall.Inscription.finish (token);
`` ''

`` `php
use Transbank \ Webpay \ Oneclick;

$ response = MallInscription :: finish ($ token);
`` ''

csharp
using Transbank.Webpay.Oneclick;

var response = Inscription.Finish (token);

`` ''

ruby
response = Transbank :: Webpay :: Oneclick :: MallInscription :: finish (token: token)
`` ''

python
response = MallInscription.finish (token = token)
`` ''

javascript
const Oneclick = require ('transbank-sdk'). Oneclick; // CommonJS
import {Oneclick,} from 'transbank-sdk'; // ES6 Modules

const response = await Oneclick.MallInscription.finish (token);
`` ''

http
PUT /rswebpaytransaction/api/oneclick/v1.0/inscriptions/{token}

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json
`` ''

 <strong> Parameters Confirm an enrollment </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Enrollment process identifier. It is delivered by Webpay in the response of the `Inscription.start ()` method. (See send in URL, not body)

 <strong> Response Confirm an enrollment </strong>

java
response.getAuthorizationCode ();
response.getCardType ();
response.getCardNumber ();
response.getResponseCode ();
response.getTbkUser ();
`` ''

`` `php
$ response-> getAuthorizationCode ();
$ response-> getCardType ();
$ response-> getCardNumber ();
$ response-> getResponseCode ();
$ response-> getTbkUser ();
`` ''

csharp
response.ResponseCode;
response.TransbankUser;
response.AuthorizationCode;
response.CardType;
response.CardNumber;
`` ''

ruby
response.response_code
response.transbank_user
response.authorization_code
response.card_type
response.card_number
`` ''

python
response.response_code
response.transbank_user
response.authorization_code
response.card_type
response.card_number
`` ''

javascript
response.response_code
response.transbank_user
response.authorization_code
response.card_type
response.card_number
`` ''

http
200 OK
Content-Type: application / json

{
  "response_code": 0,
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "authorization_code": "123456",
  "card_type": "Visa",
  "card_number": "XXXXXXXXXXXX6623"
}

`` ''

Name <br> <i> type </i> | Description
------   | -----------
response_code <br> <i> Number </i> | Authorization response code. <br> Length: 2. <br> Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # response-codes-de-authorization) a01934bzfda
tbk_user <br> <i> String </i> | Unique identifier of the customer's registration in Oneclick, which must be used to make payments or delete the registration. <br> Length: 40.
authorization_code <br> <i> String </i> | Code that identifies the authorization of the registration. <br> Length: 6.
card_type <br> <i> cardType </i> | Indicates the type of card registered by the customer (Visa, AmericanExpress, MasterCard, Diners, Magna, Redcompra). <br> Length: 10.
card_number <br> <i> String </i> | Last 4 digits of the registered card: <br> Length: 4.

### Delete an entry
You can check more details of this operation in [its documentation] (/ documentation / oneclick # delete-an-entry)

Once the registration process is finished, it is possible to delete it if necessary. For this you must use the method called `Inscription.remove ()`.

 <strong> Inscription.remove () </strong>

Allows you to delete a user enrolled in Oneclick Mall.

java
Oneclick.MallInscription.delete (tbkUser, userName)
`` ''

`` `php
use Transbank \ Webpay \ Oneclick;

MallInscription :: delete ($ tbkUser, $ userName);
`` ''

csharp
using Transbank.Webpay.Oneclick;

Inscription.Delete (userName, tbkUser);
`` ''

ruby
MallInscription :: delete (user_name: user_name, tbk_user: tbk_user)
`` ''

python
MallInscription.delete (tbk_user, user_name)
`` ''

javascript
const Oneclick = require ('transbank-sdk'). Oneclick; // CommonJS
import {Oneclick,} from 'transbank-sdk'; // ES6 Modules

const response = await Oneclick.MallInscription.delete (tbkUser, userName);
`` ''

http
DELETE /rswebpaytransaction/api/oneclick/v1.0/inscriptions

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json


{
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "username": "juanperez",
}
`` ''

 <strong> Parameters Delete an entry </strong>

Name <br> <i> type </i> | Description
------   | -----------
tbk_user <br> <i> String </i> | Unique identifier of the customer's enrollment (returned by `Inscription.finish ()`). Length: 40.
username <br> <i> String </i> | User identifier in the merchant's systems (the same one indicated in `Inscription.start ()`). Maximum length: 40.

 <strong> Response Delete an entry </strong>

This request does not have a response body, it only returns a 204 when it is done correctly

java
// 204 OK
`` ''

`` `php
// 204 OK
`` ''

csharp
// 204 OK
`` ''

ruby
# 204 OK
`` ''

python
# 204 OK
`` ''

javascript
// 204 OK
`` ''

http
204 OK
Content-Type: application / json
`` ''

### Authorize a transaction
You can review more details of this operation in [its documentation] (/ documentation / oneclick # authorize-a-transaction)

After registration, the merchant can use the received `tbkUser`
to carry out transactions. For that you must use the `Transaction.authorize ()` method.

 <strong> Transaction.authorize () </strong>

Allows you to authorize a payment.

java
MallTransactionCreateDetails transactionDetails = MallTransactionCreateDetails.build ()
  .add (amountMallOne, commerceCodeMallOne, buyOrderMallOne, installmentsNumberMallOne)
  .add (amountMallTwo, commerceCodeMallTwo, buyOrderMallTwo, installmentsNumberMallTwo);

end OneclickMallTransactionAuthorizeResponse response = OneclickMall.Transaction.authorize (username, tbkUser, buyOrder, transactionDetails);
`` ''

`` `php
use Transbank \ Webpay \ Oneclick;

$ details = [
    [
        "commerce_code" => "597055555542",
        "buy_order" => "orderCompra123445",
        "amount" => 1000,
        "installments_number" => 5
    ]
];


$ response = MallTransaction :: authorize ($ userName, $ tbkUser, $ parentBuyOrder, $ details);
`` ''

csharp
using Transbank.Webpay.Oneclick;

List <PaymentRequest> details = new List <PaymentRequest> ();
details.Add (new PaymentRequest (
  childCommerceCodeOne, buyOrderMallOne, amountMallOne, installmentsNumber
));
details.Add (new PaymentRequest (
  childCommerceCodeTwo, buyOrderMallTwo, amountMallTwo, installmentsNumber
));

var result = MallTransaction.Authorize (userName, tbkUser, buyOrder, details);
`` ''

ruby
details = [
  {
    commerce_code: amountMallOne,
    buy_order: buyOrderMallOne,
    amount: amountMallOne,
    installments_number: installmentsNumberMallOne
  },
  {
    commerce_code: amountMallTwo,
    buy_order: buyOrderMallTwo,
    amount: amountMallTwo,
    installments_number: installmentsNumberMallTwo
  }
]

Transbank :: Webpay :: Oneclick :: MallTransaction :: authorize (username: username,
                                                       tbk_user: tbk_user,
                                                       parent_buy_order: buy_order,
                                                       details: details)
`` ''

python
details = MallTransactionAuthorizeDetails (
  commerce_code, buy_order_child, installments_number, amount
) .add (
  commerce_code2, buy_order_child2, installments_number2, amount2
)

MallTransaction.authorize (
  user_name = user_name,
  tbk_user = tbk_user,
  buy_order = buy_order,
  details = details
)
`` ''

javascript
const Oneclick = require ('transbank-sdk'). Oneclick; // CommonJS
const TransactionDetail = require ('transbank-sdk'). TransactionDetail; // CommonJS
import {Oneclick, TransactionDetail} from 'transbank-sdk'; // ES6 Modules

const details = [
  new TransactionDetail (amount, commerceCode, childBuyOrder),
  new TransactionDetail (amount2, commerceCode2, childBuyOrder2)
];

const response = await Oneclick.MallTransaction.authorize (
  userName, tbkUser, buyOrder, details
);
`` ''

http
POST /rswebpaytransaction/api/oneclick/v1.0/transactions

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "username": "juanperez",
  "tbk_user": "b6bd6ba3-e718-4107-9386-d2b099a8dd42",
  "buy_order": "OrdenCompra123456789",
  "details": [
    {
      "commerce_code": "597055555542",
      "buy_order": "OrdenCompra123445",
      "amount": 1000,
      "installments_number": 5
  }]
}
`` ''

 <strong> Parameters Authorize a payment </strong>

Name <br> <i> type </i> | Description
------   | -----------
username <br> <i> String </i> | User identifier in the merchant's systems (the same one indicated in `Inscription.start ()`). Maximum length: 40.
tbk_user <br> <i> String </i> | Unique identifier of the customer's enrollment (returned by `Inscription.finish ()`). Length: 40.
buy_order <br> <i> Number </i> | Unique identifier of the purchase generated by the merchant. Maximum length: 26.
details <br> <i> Array </i> | List of objects, one for each different store in the mall that participates in the transaction.
details [] .commerce_code <br> <i> String </i> | Trade code assigned by Transbank for the store belonging to the mall to which this transaction corresponds. Length: 12.
details [] .buy_order <br> <i> String </i> | Unique identifier of the purchase generated by the child business (store). Maximum length: 26.
details [] .amount <br> <i> Decimal </i> | Amount of the payment transaction. Maximum length: 17.
details [] .installments_number <br> <i> Number </i> | Amount of installments of the payment transaction. Long 2. Not mandatory.

 <strong> Response Authorize a payment </strong>

java
response.getAccountingDate ();
response.getBuyOrder ();
response.getTransactionDate ();
final CardDetail cardDetail = response.getCardDetail ();
cardDetail.getCardNumber ();
final List <Detail> detailsResp = response.getDetails ();
for (Detail detail: detailsResp) {
    detail.getAmount ();
    detail.getAuthorizationCode ();
    detail.getBuyOrder ();
    detail.getCommerceCode ();
    detail.getInstallmentsNumber ();
    detail.getPaymentTypeCode ();
    detail.getStatus ();
}
`` ''

`` `php
$ response-> getAccountingDate ();
$ response-> getBuyOrder ();
$ card_detail = response-> getCardDetail ();
$ card_detail-> getCardNumber ();
$ response-> getTransactionDate ();
$ response-> getVci ();
$ details = response-> getDetails ();
foreach ($ details as $ detail) {
    detail-> getAmount ();
    detail-> getAuthorizationCode ();
    detail-> getBuyOrder ();
    detail-> getCommerceCode ();
    detail-> getInstallmentsNumber ();
    detail-> getPaymentTypeCode ();
    detail-> getResponseCode ();
    detail-> getStatus ();
}
`` ''

csharp
response.AccountingDate;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.SessionId;
response.TransactionDate;
response.Vci;
var details = response.Details;
foreach (var detail in details) {
    detail.Amount;
    detail.AuthorizationCode;
    detail.BuyOrder;
    detail.CommerceCode;
    detail.InstallmentsNumber;
    detail.PaymentTypeCode;
    detail.ResponseCode;
    detail.Status;
}
`` ''

ruby
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.each do | detail |
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
end
`` ''

python
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for detail in details:
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
`` ''

javascript
response.accounting_date
response.buy_order
cardDetail = response.card_detail
cardDetail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for (let detail of details) {
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
}
`` ''

http
200 OK
Content-Type: application / json

{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15: 43: 48.523Z",
  "details": [
    {
      "amount": 500,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555542",
      "buy_order": "505479072"
  }]
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | Purchase order generated by the parent business.
card_detail <br> <i> cardDetail </i> | Object that contains information about the card used by the cardholder.
card_detail.card_number <br> <i> String </i> | The last 4 digits of the card used in the transaction.
accounting_date <br> <i> String </i> | Accounting date of payment authorization.
transaction_date <br> <i> DateTime </i> | Full date (timestamp) of the payment authorization. ISO 8601
details <br> <i> Array </i> | List with the result of each transaction of the child stores.
details [] .amount <br> <i> Decimal </i> | Amount of the payment transaction.
details [] .status <br> <i> String </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED).
details [] .authorization_code <br> <i> String </i> | Authorization code of the payment transaction.
details [] .payment_type_code <br> <i> String </i> | [Type] (/ product / webpay # types-of-payment) of payment of the transaction. <br> VD = Sale Debit. <br> VP = Prepaid Sale <br> VN = Normal Sale. <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Interest-free installments <br>
details [] .response_code <br> <i> Number </i> | Payment result code, where: 0 (zero) is approved. Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # authorization-response-codes) <br> Some specific codes for Oneclick are: a0342fccfda19 : `tbk_user` does not exist <br> -97: Oneclick limits, maximum daily payment amount exceeded. <br> -98: Oneclick limits, maximum payment amount exceeded <br> -99: Oneclick limits, maximum amount of daily payments exceeded.
details [] .installments_number <br> <i> Number </i> | Amount of installments of the payment transaction.
details [] .commerce_code <br> <i> Number </i> | Commercial code of the child trade (store).
details [] .buy_order <br> <i> String </i> | Purchase order generated by the child merchant for the payment transaction.

 <aside class="warning">
Any non-number value in `installmentsNumber` (including letters,
non-existence of the field or null) will be assumed as zero, that is, "No quotas".
 </aside>

### Get status of a transaction

Allows you to check the status of payment made through Oneclick.
Returns the authorization result.

You can check more details of this operation in [its documentation] (/ documentation / oneclick # get-status-of-a-transaction)

java
final OneclickMallTransactionStatusResponse response =
  OneclickMall.Transaction.status (buyOrder);
`` ''

`` `php
use Transbank \ Webpay \ Oneclick;

$ response = MallTransaction :: getStatus ($ buyOrder);
`` ''

csharp
using Transbank.Webpay.Oneclick;

var result = MallTransaction.Status (buyOrder);
`` ''

ruby
response = Transbank :: Webpay :: Oneclick :: MallTransaction :: status (buy_order: buy_order)
`` ''

python
var response = MallTransaction.status (buy_order)
`` ''

javascript
const Oneclick = require ('transbank-sdk'). Oneclick; // CommonJS
import {Oneclick,} from 'transbank-sdk'; // ES6 Modules

const response = await Oneclick.MallTransaction.status (token);
`` ''

http
GET /rswebpaytransaction/api/oneclick/v1.0/transactions/[buyOrder}

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json
`` ''

 <strong> Parameters Consult a payment made </strong>

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | Purchase order of the transaction to consult (it is sent in the URL, not in the body).

 <strong> Response Check a payment made </strong>

java
response.getAccountingDate ();
response.getBuyOrder ();
response.getTransactionDate ();
final CardDetail cardDetail = response.getCardDetail ();
cardDetail.getCardNumber ();
final List <Detail> detailsResp = response.getDetails ();
for (Detail detail: detailsResp) {
    detail.getAmount ();
    detail.getAuthorizationCode ();
    detail.getBuyOrder ();
    detail.getCommerceCode ();
    detail.getInstallmentsNumber ();
    detail.getPaymentTypeCode ();
    detail.getStatus ();
}
`` ''

`` `php
$ response-> getAccountingDate ();
$ response-> getBuyOrder ();
$ card_detail = $ response-> getCardDetail ();
$ card_detail-> getCardNumber ();
$ response-> getTransactionDate ();
$ response-> getVci ();
$ details = $ response-> getDetails ();
foreach ($ details as $ detail) {
    detail-> getAmount ();
    detail-> getAuthorizationCode ();
    detail-> getBuyOrder ();
    detail-> getCommerceCode ();
    detail-> getInstallmentsNumber ();
    detail-> getPaymentTypeCode ();
    detail-> getResponseCode ();
    detail-> getStatus ();
}
`` ''

csharp
response.AccountingDate;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.SessionId;
response.TransactionDate;
response.Vci;
var details = response.Details;
foreach (var detail in details) {
    detail.Amount;
    detail.AuthorizationCode;
    detail.BuyOrder;
    detail.CommerceCode;
    detail.InstallmentsNumber;
    detail.PaymentTypeCode;
    detail.ResponseCode;
    detail.Status;
}
`` ''

ruby
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.each do | detail |
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
end
`` ''

python
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for detail in details:
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
`` ''

javascript
response.accounting_date
response.buy_order
cardDetail = response.card_detail
cardDetail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for (detail on details) {
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
}
`` ''

http
200 OK
Content-Type: application / json

{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15: 43: 48.523Z",
  "details": [
    {
      "amount": 500,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555542",
      "buy_order": "505479072"
  }]
}

`` ''

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | Purchase order generated by the parent business.
card_detail <br> <i> cardDetail </i> | Object that contains information about the card used by the cardholder.
card_detail.card_number <br> <i> String </i> | The last 4 digits of the card used in the transaction.
accounting_date <br> <i> String </i> | Accounting date of payment authorization.
transaction_date <br> <i> DateTime </i> | Full date (timestamp) of the payment authorization.
details <br> <i> Array </i> | List with the result of each transaction of the child stores.
details [] .amount <br> <i> Decimal </i> | Amount of the payment sub-transaction.
details [] .status <br> <i> String </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED).
details [] .authorization_code <br> <i> String </i> | Authorization code of the payment sub-transaction.
details [] .payment_type_code <br> <i> String </i> | [Type] (/ product / webpay # types-of-payment) of payment of the transaction. <br> VD = Sale Debit. <br> VP = Prepaid Sale <br> VN = Normal Sale. <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Interest-free installments <br>
details [] .response_code <br> <i> Number </i> | Return code of the payment process, where: <br> 0 (zero) is approved. Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # authorization-response-codes) <br> Some specific codes for Oneclick are: a0342fccfda19 : `tbk_user` does not exist <br> -97: Oneclick limits, maximum daily payment amount exceeded. <br> -98: Oneclick limits, maximum payment amount exceeded <br> -99: Oneclick limits, maximum amount of daily payments exceeded.
details [] .installments_number <br> <i> Number </i> | Amount of installments of the payment sub-transaction.
details [] .commerce_code <br> <i> Number </i> | Commercial code of the child trade (store).
details [] .buy_order <br> <i> String </i> | Purchase order generated by the child merchant for the payment sub-transaction.
status <br> <i> Text </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Maximum length: 64
balance <br> <i> Decimal </i> | Remaining amount of original payment sub-transaction: initial amount - voided amount. Maximum length: 17

### Reverse or void a transaction

You can check more details of this operation in [its documentation] (/ documentation / oneclick # revert-or-cancel-a-transaction)

java
final OneclickMallTransactionRefundResponse response =
  OneclickMall.Transaction.refund (buyOrder, childCommerceCode, childBuyOrder, amount);
`` ''

`` `php
use Transbank \ Webpay \ Oneclick;

$ response = MallTransaction :: refund ($ buyOrder, $ childCommerceCode, $ childBuyOrder, $ amount);
`` ''

csharp
using Transbank.Webpay.Oneclick;

var response = MallTransaction.Refund (buyOrder, childCommerceCode, childBuyOrder, amount);
`` ''

ruby
response = Transbank :: Webpay :: Oneclick :: MallTransaction :: refund (
  buy_order: buy_order,
  child_commerce_code: child_commerce_code,
  child_buy_order: child_buy_order,
  amount: amount
)
`` ''

python
var response = MallTransaction.refund (buy_order, child_commerce_code, child_buy_order, amount)
`` ''

javascript
const Oneclick = require ('transbank-sdk'). Oneclick; // CommonJS
import {Oneclick,} from 'transbank-sdk'; // ES6 Modules

const response = await Oneclick.MallTransaction.refund (buyOrder, childCommerceCode, childBuyOrder, amount);
`` ''

http
POST /rswebpaytransaction/api/oneclick/v1.0/transactions/[buyOrder-lex.europa.eu/refunds

Tbk-Api-Key-Id: 597055555541
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "commerce_code": "597055555542",
  "detail_buy_order": "OrdenCompra12345",
  "amount": 1000
}
`` ''

 <strong> Reverse or Cancel Parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | Purchase order of the transaction to be reversed or canceled. It is sent in the URL, not the body. Maximum length: 26.
commerce_code <br> <i> String </i> | Child commercial code. Maximum length: 12.
detail_buy_order <br> <i> String </i> | Purchase order child of the transaction to be reversed or canceled. Maximum length: 26.
amount <br> <i> Integer format for weight transactions. Only in case of dollar it accepts two decimal places. </i> | Amount to be canceled or reversed from the transaction. Maximum length: 17

 <strong> Reverse or Abort Response </strong>

java
response.getAuthorizationCode ();
response.getAuthorizationDate ();
response.getBalance ();
response.getNullifiedAmount ();
response.getResponseCode ();
response.getType ();
`` ''

`` `php
response-> getAuthorizationCode;
response-> getAuthorizationDate;
response-> getBalance;
response-> getNullifiedAmount;
response-> getResponseCode;
response-> getType;
`` ''

csharp
response.AuthorizationCode;
response.AuthorizationDate;
response.Balance;
response.NullifiedAmount;
response.ResponseCode;
response.Type;
`` ''

ruby
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
`` ''

python
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
`` ''

javascript
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
`` ''

http
200 OK
Content-Type: application / json

{
  "type": "NULLIFIED",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20: 18: 20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}

In case of a reversal it does not return more information
{
  "type": "REVERSED",
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
type <br> <i> String </i> | Type of refund, REVERSED or NULLIFIED, if it is REVERSED, no transaction data will be returned (authorization code, etc). Maximum length: 10
authorization_code <br> <i> Boolean </i> | (Only if it is NULLIFIED) Authorization code. Maximum length: 6
authorization_date <br> <i> ISO8601 </i> | (Only if it is NULLIFIED) Date of the authorization of the transaction.
nullified_amount <br> <i> Decimal </i> | (Only if it is NULLIFIED) Amount canceled. Maximum length: 17
balance <br> <i> Decimal </i> | (Only if NULLIFIED) Remaining amount of original payment transaction: initial amount - voided amount. Maximum length: 17
response_code <br> <i> Number </i> | (Only if it is NULLIFIED) Payment result code, where: 0 (zero) is approved. Maximum length: 2
buy_order <br> <i> String </i> | (Only if NULLIFIED) Purchase order generated by the child merchant for the payment transaction. Maximum length: 26.

### Deferred capture of a transaction

Check more details about this modality in [the documentation] (/ documentation / oneclick # capture-a-transaction)

java
final OneclickMallTransactionCaptureResponse response = Oneclick.MallDeferredTransaction.capture (
  childCommerceCode, childBuyOrder, amount, authorizationCode
);
`` ''

`` `php
$ response = MallTransaction :: capture ($ commerce_code, $ buy_order, $ authorization_code, $ amount);
`` ''

csharp
// This function is not yet available in the SDK
`` ''

ruby
response = Transbank :: Webpay :: Oneclick :: MallDeferredTransaction :: capture (
  child_commerce_code: @commerce_code, child_buy_order: @buy_order,
  amount: @capture_amount, authorization_code: @authorization_code
)
`` ''

python
# This function is not yet available in the SDK
`` ''

javascript
const Oneclick = require ('transbank-sdk'). Oneclick; // CommonJS
import {Oneclick,} from 'transbank-sdk'; // ES6 Modules

const response = Oneclick.MallTransaction.capture (
  childCommerceCode, childBuyOrder, amount, authorizationCode
);
`` ''

http
PUT / rswebpaytransaction / api / oneclick / mall / v1_0 / transactions / capture
Tbk-Api-Key-Id: 597055555547
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json
{
    "commerce_code": 597055555548,
    "buy_order": "OCDT12345678",
    "capture_amount": 50,
    "authorization_code": "1213"
}
`` ''

 <strong> Deferred Capture Parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
commerce_code <br> <i> Number </i> | Daughter store that carried out the transaction. Length: 6.
buy_order <br> <i> String </i> | Purchase order of the transaction that is required to be captured. Maximum length: 26.
capture_amount <br> <i> Decimal </i> |Amount to be captured. Maximum length: 17.
authorization_code <br> <i> String </i> |Authorization code of the transaction that is required to capture Maximum length: 6.

 <aside class="notice">
The `capture ()` method must be invoked always indicating the code of the
specific virtual store trade.
 </aside>

 <strong> Deferred Capture Response </strong>

java
response.getAuthorizationCode ();
response.getAuthorizationDate ();
response.getCapturedAmount ();
response.getResponseCode ();
`` ''

`` `php
$ response-> getAuthorizationCode ();
$ response-> getAuthorizationDate ();
$ response-> getCapturedAmount ();
$ response-> getResponseCode ();
`` ''

csharp
// This function is not yet available in the SDK
`` ''

ruby
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
`` ''

python
# This function is not yet available in the SDK
`` ''

javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
`` ''

http
200 OK
Content-Type: application / json
{
    "authorization_code": "152759",
    "authorization_date": "2020-04-03T01: 49: 50.181Z",
    "captured_amount": 50,
    "response_code": 0
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
authorization_code <br> <i> String </i> | Authorization code for deferred capture. Maximum length: 6
authorization_date <br> <i> String </i> | Date and time of authorization.
captured_amount <br> <i> Decimal </i> | Amount captured. Maximum length: 6
response_code <br> <i> Number </i> | Capture result code. If it is successful it is 0, otherwise the capture was not carried out. Maximum length: 2

 <aside class="notice">
In case of error, the same exclusive codes of the `capture ()` method will appear.
for simultaneous capture.
 </aside>


## Error codes and messages

When making any request to the REST API, in addition to the response data, one of the following HTTP response status codes will be included depending on the result obtained:

### Successful application

When the requested operation is executed correctly, these HTTP statuses can be received:

HTTP status code | Description
------ | -----------
200 | The operation has been executed successfully
204 | DELETE operation has been executed successfully

 <strong> Error codes </strong>

All errors reported by the Webpay REST API display a JSON message with a description of the error.

json
{
  "error_message": "token is required"
}
`` ''

HTTP status code | Description
------ | -----------
400 | The JSON message is invalid. It may be that it does not correspond to a well structured message or that it contains an unexpected field.
401 | Not authorized. Invalid API Key and / or API Secret
404 | The transaction has not been found.
405 | Method not allowed.
406 | It was not possible to process the response in the format that the client indicates.
415 | Message type not allowed.
422 | The request could not be processed either by data validations or by business logic.
500 | An unexpected error has occurred.

## Put into Production

1. Once the merchant determines that its integration has completed, a [validation process] (/ reference / webpay # validation-process) must be carried out.

2. Once Transbank confirms that the integration form is correct (it does not apply to plugins), the confirmation will be sent to the merchant and its ** shared secret ** will be generated, which together with the commerce code, allow operating in production .

3. When you receive the email, it will be necessary to [change the e-commerce configuration to work in production] (# configuration-for-production-using-the-sdk)

4. With the configuration of the production environment ready, it will be necessary to make a purchase of $ 10 to validate the correct operation.

### Validation process

During the validation of the integration, it is intended to verify that the merchant transacts safely and without problems, for which a series of tests will be requested and subsequent sending of evidence to validate the integration. This validation is a requirement for the business to be able to operate in the production environment (banks and real money) and a business will not be allowed to use the service productively without having a validation.

Transbank will only validate the integrations of those businesses that have a productive commerce code. To obtain it, follow the instructions to become a client on the portal [http://www.transbank.cl] (http://www.transbank.cl) or contact your commercial executive.

At this stage, the merchant sends the evidence to [soporte@transbank.cl] (mailto: soporte@transbank.cl) in ** PDF format ** using the form corresponding to the integrated product, clearly indicating the purchase orders, date and time of transactions. For Webpay integrations that use an [official plugin] (https://transbankdevelopers.cl/plugin) there is a special form.

[Download the evidence form](https://transbankdevelopers.cl/files/evidencia-integracion-webpay-rest.docx)

Support will validate that the test cases are consistent with those registered in the Webpay systems and, if everything is correct, the merchant will be notified of the compliance to go to production, receiving the instructions for it. If the tests are not consistent, the merchant will be contacted regarding its integration, so that you can make the corresponding corrections and send the evidence again once said corrections have been completed.

In the hiring process, you received your trade code, and together with the ** shared secret ** that was given to you after the certification, you can complete your credentials, which ** you must safeguard and prevent them from being in the hands of third parties ** since they allow you to make (or cancel) transactions on behalf of your business.

* Trading code (* API Key *)
* Shared Secret (* Shared Secret *)

After the validation process of your integration is finished, you must make the configuration so that your site is in production.

### Configuration for production using the SDKs

If you are using an official Transbank SDK, then you must follow the following steps.

 <aside class="warning">
Never leave your trade code and shared secret directly in your code, we recommend using environment variables or another method that allows you to keep your credentials safe.
 </aside>

1. Assign the productive trade code, delivered by Transbank at the time of contracting the product.

java
// For Oneclick
OneclickMall.setCommerceCode ('YOUR_TRADE_CODE');
`` ''

`` `php
// This function is not yet available in the SDK
`` ''

csharp
// This function is not yet available in the SDK
`` ''

ruby
# This function is not yet available in the SDK

`` ''

python
# This function is not yet available in the SDK
`` ''

javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
`` ''

http
200 OK
Content-Type: application / json
{
    "authorization_code": "152759",
    "authorization_date": "2020-04-03T01: 49: 50.181Z",
    "captured_amount": 50,
    "response_code": 0
}
`` ''

`` `php
// This function is not yet available in the SDK
`` ''

csharp
// This function is not yet available in the SDK
`` ''

ruby
# This function is not yet available in the SDK

`` ''

python
# This function is not yet available in the SDK
`` ''

javascript
const Oneclick = require ('transbank-sdk'). Oneclick; // CommonJS
import {Oneclick,} from 'transbank-sdk'; // ES6 Modules

Oneclick.commerceCode = 'YOUR_TRADE_CODE';
`` ''

2. Shared secret configuration.

java
// For Oneclick
OneclickMall.setApiKey ('TU_API_KEY');
`` ''

`` `php
// This function is not yet available in the SDK
`` ''

csharp
// This function is not yet available in the SDK
`` ''

ruby
# This function is not yet available in the SDK

`` ''

python
# This function is not yet available in the SDK
`` ''

javascript
const Oneclick = require ('transbank-sdk'). Oneclick; // CommonJS
import {Oneclick,} from 'transbank-sdk'; // ES6 Modules

Oneclick.apiKey = 'TU_API_KEY';
`` ''

3. Selection of the productive environment.

java
// For Oneclick
OneclickMall.setIntegrationType (IntegrationType.LIVE);
`` ''

`` `php
// This function is not yet available in the SDK
`` ''

csharp
// This function is not yet available in the SDK
`` ''

ruby
# This function is not yet available in the SDK

`` ''

python
# This function is not yet available in the SDK
`` ''

javascript
const Oneclick = require ('transbank-sdk'). Oneclick; // CommonJS
const Environment = require ('transbank-sdk'). Environment // CommonJS
import {Oneclick, Environment} from 'transbank-sdk'; // ES6 Modules

Oneclick.environment = Environment.Production;
`` ''

Alternatively some SDKs offer a method to configure directly to production
java
// This function is not yet available in the SDK
`` ''

`` `php
// This function is not yet available in the SDK
`` ''

csharp
// This function is not yet available in the SDK
`` ''

ruby
# This function is not yet available in the SDK

`` ''

python
# This function is not yet available in the SDK
`` ''

javascript
const Oneclick = require ('transbank-sdk'). Oneclick; // CommonJS
import {Oneclick,} from 'transbank-sdk'; // ES6 Modules

Oneclick.configureForProduction ('YOUR_TRADE_CODE', 'YOUR_API_KEY');
`` ''
