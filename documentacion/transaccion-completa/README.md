# Transacción Completa



## Webpay Transacción Completa %<span class='tbk-tagTitleDesc'>REST</span>%

<div class="pos-title-nav">
  <div tbk-link='/referencia/transaccion-completa' tbk-link-name='Referencia API'></div>
</div>

Una transacción completa permite al comercio presentar al tarjetahabiente un
formulario propio para almacenar los datos de la tarjeta, fecha de vencimiento
y cvv (no necesario para comercios con la opción `sin cvv` habilitada) . 

<aside class="warning">
Este producto tiene requerimientos comerciales más estrictos que el resto de los productos.  
No inicies la integración si aún no completan la afiliación comercial.
</aside>


### Crear una transacción

Antes de realizar cualquier transacción es necesario instanciar el producto.

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;

Transaction.setCommerceCode = "Codigo de Comercio";
Transaction.setApiKey = "Apikey entregado por Transabank";
Transaction.setIntegrationType = "TEST/LIVE"; //ambiente de integracion;
```

```php
use Transbank\TransaccionCompleta;

TransaccionCompleta::setCommerceCode = "Codigo de Comercio";
TransaccionCompleta::setApiKey = "Apikey entregado por Transabank";
TransaccionCompleta::setIntegrationType = "TEST/LIVE"; //ambiente de integracion;


```

```csharp
...
using Transbank.Webpay.Common;
using Transbank.Webpay.TransaccionCompleta;
...

TransaccionCompleta.CommerceCode = "Codigo de Comercio";
TransaccionCompleta.ApiKey = "Apikey entregado por Transabank";
TransaccionCompleta.IntegrationType = "TEST/LIVE"; //ambiente de integracion;

```

```ruby

```

```python


```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```
Es recomendado encapsular la asignacion para utilizarla sin problemas en los demas metodos.

Ahora se pueden realizar transacciones sin problemas

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;

String buyOrder = "Orden de compra de la transaccion";
String sessionId = "Identificador del servicio unico de transacción";
double amount = 10000; // mongo en pesos
String cardNumber= "Numero de Tarjeta";
String cardExpirationDate= "Fecha de expiracion en formato AA/MM";
short cvv = 123; // CVV de la tarjeta.

FullTransactionCreateResponse response = FullTransaction.Transaction.create(buyOrder, sessionId, amount, cardNumber, cardExpirationDate, cvv);

```

```php
use Transbank\TransaccionCompleta\Transaction;

$buyOrder = "Orden de compra de la transaccion";
$sessionId = "Identificador del servicio unico de transacción";
$amount = 10000; // mongo en pesos
$cardNumber= "Numero de Tarjeta";
$cardExpirationDate= "Fecha de expiracion en formato AA/MM";
$cvv = 123; // CVV de la tarjeta.

$response = Transaction::create($buyOrder, $sessionId, $amount, $cardNumber, $cardExpirationDate, $cvv);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

var buy_order = "Orden de compra de la transaccion";
var session_id = "Identificador del servicio unico de transacción";
var amount = 10000; // monto en pesos
var card_number = "Numero de Tarjeta";
var card_expiration_date = "Fecha de expiracion en formato AA/MM";
var cvv = 123; // CVV de la tarjeta.

var response = FullTransaction.Create(
                buyOrder: buy_order,
                sessionId: session_id,
                amount: amount,
                cvv: cvv,
                cardNumber: card_number,
                cardExpirationDate: card_expiration_date);
```

```ruby
buy_order = "Orden de compra de la transaccion"
session_id = "Identificador del servicio unico de transacción"
amount = 1000 /* monto en pesos */
card_number = "Numero de Tarjeta"
card_expiration_date = "Fecha de expiracion en formato AA/MM"
cvv = 123 /* CVV de la tarjeta. */


response = Transbank::TransaccionCompleta::Transaction::create(
                                                            buy_order: buy_order,
                                                            session_id: session_id,
                                                            amount: amount,
                                                            card_number: card_number,
                                                            cvv: cvv,
                                                            card_expiration_date: card_expiration_date
                                                            )

```

```python

from transbank.transaccion_completa.transaction import Transaction

    # leyendo variables desde un formulario
    buy_order = 'Orden de compra de la transaccion '
    session_id = 'Identificador del servicio unico de transacción'
    amount = 10000; #monto en pesos
    card_number = 'Numero de Tarjeta'
    cvv = 123 #CVV de la tarjeta.
    card_expiration_date = 'Fecha de expiracion en formato AA/MM'

    resp = Transaction.create(
        buy_order=buy_order, session_id=session_id, amount=amount,
        card_number=card_number, cvv=cvv, card_expiration_date=card_expiration_date
    )

```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```

### Consulta de Cuotas

Antes de confirmar una transaccion es necesario confirmar la cantidad de cuotas y entregar el valor de estas.

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;


String token = "token obtenido como respuesta de la creacion de transaccion";
int installmentsNumber = 10; // numero de cuotas;

FullTransactionInstallmentResponse response = FullTransaction.Transaction.installment(
  token,
  installmentsNumber
);
```

```php
use Transbank\TransaccionCompleta\Transaction;

$token = "token obtenido como respuesta de la creacion de transaccion";
$installmentsNumber = 10; // numero de cuotas;

$response = Transaction::installments(
    token,
    installmentsNumber
);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

var token = "token obtenido como respuesta de la creacion de transaccion";
var installments_number = 10; // numero de cuotas;

var response = FullTransaction.Installments(
  token: token,
  installmentsNumber: installments_number
  );
```

```ruby
token = "token obtenido como respuesta de la creacion de transaccion"
installments_number = 10 /* numero de cuotas */

response = Transbank::TransaccionCompleta::Transaction::installments( token: token,
                                                                      installments_number: installments_number )
```

```python
from transbank.transaccion_completa.transaction import Transaction

#obtener form desde el request
    req = request.form
    token = request.form.get('token') #token obtenido al iniciar la transaccion
    installments_number = 10 #numero de cuotas
    resp = Transaction.installments(token=token, installments_number=installments_number)

```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```

### Confirmar Transaccion

Una vez obtenido la respuesta de la consulta de cuotas, con los datos de esta se puede realizar commit de la transaccion:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;


String token = "token obtenido como respuesta de la creacion de transaccion";
int idQueryInstallments = 12345679; // numero identificador de las cuotas.
byte deferredPeriodIndex= 1;
Boolean gracePeriod = false;

FullTransactionCommitResponse response = FullTransaction.Transaction.commit(
  token,
  idQueryInstallments,
  deferredPeriodIndex,
  gracePeriod
);

```

```php
use Transbank\TransaccionCompleta\Transaction;

$token = "token obtenido como respuesta de la creacion de transaccion";
$idQueryInstallments = 12345679; // numero identificador de las cuotas.
$deferredPeriodIndex= 1;
$gracePeriod = false;


$response = Transaction::commit(
    $token,
    $idQueryInstallments,
    $deferredPeriodIndex,
    $gracePeriod,
);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

var token = "token obtenido como respuesta de la creacion de transaccion";
var idQueryInstallments = 12345679; // numero identificador de las cuotas.
var deferredPeriodsIndex = 1;
var gracePeriods = false;

var result = FullTransaction.Commit(
  token:token,
  idQueryInstallments: idQueryInstallments,
  deferredPeriodsIndex: deferredPeriodsIndex,
  gracePeriods: gracePeriods
);
```

```ruby
token = var token = "token obtenido como respuesta de la creacion de transaccion"
id_query_installments = 12345679 /* numero identificador de las cuotas. */
deferred_period_index = 1
grace_period = false

response = Transbank::TransaccionCompleta::Transaction::commit( token: token,
                                                            id_query_installments: id_query_installments,
                                                            deferred_period_index:deferred_period_index,
                                                            grace_period: grace_period )
```

```python
    from transbank.transaccion_completa.transaction import Transaction
    
    #token obtenido como respuesta de la creacion de transaccion
    token = request.form.get('token')
    id_query_installments = 12345679 # numero identificador de las cuotas.
    deferred_period_index = 1
    grace_period = 'false'

    response = Transaction.commit(token=token,
                              id_query_installments=id_query_installments,
                              deferred_period_index=deferred_period_index,
                              grace_period=grace_period)

```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```


### Estado Transaccion

Para obtener el resultado de la transaccion exite el siguiente metodo:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;


String token = "token obtenido como respuesta de la creacion de transaccion";

FullTransactionCommitResponse response = FullTransaction.Transaction.status(
  token,
);

```

```php
use Transbank\TransaccionCompleta\Transaction;

$token = "token obtenido como respuesta de la creacion de transaccion";

$response = Transaction::status(
    $token
);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

var token = "token obtenido como respuesta de la creacion de transaccion";

var response = FullTransaction.Status(
  token:token
);
```

```ruby
token = "token obtenido como respuesta de la creacion de transaccion"

response = Transbank::TransaccionCompleta::Transaction::status(token: token)
```

```python
from transbank.transaccion_completa.transaction import Transaction

    response = Transaction.status(token=token) #token obtenido como respuesta de la creacion de transaccion

```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```

### Reembolso Transaccion

Para procesar un reembolso de la transaccion exite el siguiente metodo:

<div class="language-simple" data-multiple-language></div>

```java
import cl.transbank.transaccioncompleta.FullTransaction;
  String tokenWs = //token obtenido al crear transaccion
  double amount = 1000;

  final FullTransactionRefundResponse response = FullTransaction.Transaction.refund(tokenWs,amount);

```

```php
use Transbank\TransaccionCompleta\Transaction;

$token = "token obtenido como respuesta de la creacion de transaccion";
$amount = 1000; // monto a reembolsar

$response = Transaction::refund(
    $token,
    $amount
);
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

var token = "token obtenido como respuesta de la creacion de transaccion";
var amount = 1000; // monto a reembolsar

var response = FullTransaction.Refund(
  token: token,
  amount: amount
);
```

```ruby
token = "token obtenido como respuesta de la creacion de transaccion"
amount = 1000 /* monto a reembolsar */

response =  Transbank::TransaccionCompleta::Transaction::refund(token: token, amount: amount)
```

```python
from transbank.transaccion_completa.transaction import Transaction

   #obtener form desde el request
    req = request.form
    token = req.get('token') #token obtenido al crear la transaccion
    amount = '1000' #monto a reembolsar
    response = Transaction.refund(token=token, amount=amount)

```
```javascript
// No está implementado en el SDK. De momento puedes usar la referencia del API o usar una librería externa. 
```

## Credenciales y Ambiente

### Ambiente de integración
En el ambiente de integración existen códigos de comercio previamente creados para todos los productos (Webpay Plus, 
OneClick, etc), para cada una de sus variaciones (Captura Diferida, Mall, Mall Captura Diferida, etc) y dependiendo de 
la moneda que acepten (USD o CLP).

Esto permite que puedas operar en el ambiente de pruebas con un código de comercio que tenga la misma configuración contratada
en tu código de comercio productivo. (Si contrataste OneClick Mall Captura Diferida, debes usar ese código de comercio 
en integración para realizar las pruebas) 

Puedes revisar los códigos de comercio del ambiente de integración de todos nuestros productos y variaciones 
[en este link](/documentacion/como_empezar#ambiente-de-integracion).

### OneClick: Configuración SDK 
Los SDK vienen preconfigurados para operar con OneClick Mall captura simultanea. Si necesitas operar con otra modalidad, 
como captura diferida, debes configurar explícitamente el [código de comercio que usarás](/documentacion/como_empezar#ambiente-de-integracion).
No es necesario definir el Api Key Secret (llave secreta) ya que en este ambiente, todos los productos usan la misma y 
ya viene preconfigurada. 

```java
// OneclickMall Live Config
FullTransaction.Transaction.setCommerceCode("pon-tu-codigo-de-comercio-aca");
``` 

```php
\Transbank\TransaccionCompleta::setCommerceCode("{commerce-code}");
```

```csharp
using Transbank.Webpay.TransaccionCompleta;

TransaccionCompleta.Webpay.CommerceCode = "5970TuCodigo";
```

```ruby
# OneClick
Transbank::Webpay::TransaccionCompleta::Base.commerce_code = "commercecode"
```

```python
from transbank import transaccion_completa as BaseTransaccionCompleta

BaseTransaccionCompleta.commerce_code = "commercecode"
```

### Apuntar a producción

Antes de operar en el ambiente de producción, debes pasar por un [proceso de validación](/documentacion/como_empezar#el-proceso-de-validacion), luego del cual te entregaremos 
tu Api Key Secret (**llave secreta**).  

Si ya tienes tu llave secreta, puedes revisar como configurar el SDK para usar este ambiente de producción en [esta sección](/documentacion/como_empezar#configuracion-de-produccion)

## Ejemplos de integración

Ponemos a tu disposición una serie de repositorios en nuestro Github para ayudarte a entender la integración de mejor forma.
Puedes encontrar una lista de [proyectos de ejemplo acá](/documentacion/como_empezar#ejemplos). 


<div class="container slate">
  <div class='slate-after-footer'>
    <div class='row d-flex align-items-stretch'>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22 text-center'>¿Tienes alguna duda de integración?</h3>
        <a href='https://join-transbankdevelopers-slack.herokuapp.com/' target='_blank'>
          <div class='td_block_gray'>
            <img src="https://p9.zdassets.com/hc/theme_assets/138842/200037786/logo.png" alt="" >
            <div class='td_pa-txt'>
              Únete a la comunidad de integradores en Slack y comparte buenos tips ayudando a los nuevos o buscando soluciones alternativas. Nuestro equipo está ahí para ayudarte.
            </div>
          </div>
        </a>
      </div>
      <div class='mt-3 mt-lg-0 col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22 text-center'>Si aún tienes dudas envíanos un mensaje</h3>
        <a class="pointer magenta" data-toggle='modal' data-target='#modalContactForm'>
          <div class='td_block_gray'>
            <div class="fo-size-20 text-center sub-title_bloq"><i class="fas fa-envelope"></i> Envíanos un mensaje.</div>
            <div class='td_pa-txt'>
              Si necesitas resolver algún tipo de incidencia en el portal o si existe algún problema con tu integración y  que no has podido resolver, contáctanos a través de nuestro formulario.
            </div>
          </div>
        </a>
      </div>
    </div>
  </div>
</div>
