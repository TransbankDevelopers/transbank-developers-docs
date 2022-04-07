# Webpay Modal

Webpay Modal permite a los comercios integrar un formulario de pago directamente dentro de su sitio web, sin una redirección 
del navegador web a una plataforma de pago externa, permitiendo a sus clientes realizar una solicitud de autorización 
financiera de un pago con tarjetas de crédito, débito o prepago. 
Usada para un pago puntual en una tienda simple. Se generará un único cobro para todos los productos o servicios adquiridos por el tarjetahabiente.

<aside class="success">
Este producto está disponible desde la versión 1.1 del API REST. 
</aside>

### Flujo en caso de éxito

De cara al tarjetahabiente, el flujo de pago es el siguiente: 

1. Una vez seleccionado los bienes o servicios, el tarjetahabiente decide pagar a
   través de Webpay Modal.
2. El comercio inicia una transacción en Webpay Modal en su servidor. 
3. Webpay procesa el requerimiento y entrega como resultado de la operación el
   token de la transacción.
4. El comercio muestra, dentro de su mismo sitio, un formulario de pago Webpay Modal, utilizando el token de la
   transacción recibido en el punto anterior. Para esto se utiliza el SDK Javascript.  
5. El usuario completa el formulario de pago, sin pasar por un proceso de autorización bancaria (no es redirigido a su 
   banco para autorizar el pago).  
6. Una vez completado el pago, Webpay Modal le indicará al comercio que el pago ha finalizado (mediante 
   Javascript, a través de un callback).
7. El comercio recibe la notificación y realiza la confirmación (commit) de la transacción en su servidor, para revisar si la transacción salió aprobada o rechazada. Acá es donde el comercio realiza sus procesos internos tras la confirmación del pago.  
8. Si la transacción ha sido aprobada (código de respuesta es igual a 0), el comercio muestra el voucher con los datos de la transacción y/o una página de gracias. 

### Crear una transacción

Esta operación te permite crear una transacción.  Webpay Modal procesa el requerimiento y entrega
como resultado de la operación el token de la transacción.

<aside class="notice">
Es importante considerar que una vez invocado este método, el token que es
entregado tiene un periodo reducido de vida de 5 minutos, posterior a esto el
token es caducado y no podrá ser utilizado en un pago.
</aside>

Este método recibe tres parámetros: 
- El monto de la transacción
- La orden de compra, que es un identificador interno que te permita identificar la transacción
- Un sessionId que te permita identificar la sesión del usuario. Similar a buyOrder, pero este es opcional. 

<div class="language-simple" data-multiple-language></div>

```php
use Transbank\Webpay\Webpay\Modal\Transaction;

$response = Transaction::create($amount, $buyOrder, $sessionId);
```

<strong>Respuesta crear una transacción</strong>

La respuesta de Webpay Modal a la creación es el token de la transacción.
Te recomendamos guardar este token en tu base de datos, asociándolo a tu orden de compra, carro de compra, pedido o lo que ocupes 
para identificar tu transacción.

<div class="language-simple" data-multiple-language></div>

```php
$response->getToken();
```

### Mostrar formulario de pago
Una vez que ya tienes el token, debes desplegar el formulario de pago de Webpay Modal. 
Para eso, debes incluir el siguiente `<script>` en tu web. 

```html 
<!-- A) Incluir este para el ambiente de integración -->
<script type="text/javascript" src="https://webpay3gint.transbank.cl/webpayserver/webpay.js"></script>

<!-- B) Incluir este para el ambiente de producción -->
<script type="text/javascript" src="https://webpay3g.transbank.cl/webpayserver/webpay.js"></script>
```
Para el ambiente de **integración**, usar dominio `https://webpay3gint.transbank.cl` y para **producción* usar el dominio `https://webpay3g.transbank.cl`

Luego, es necesario tener un `<div>` en donde se montará el formulario de pago: 
```html 
    <div id="webpay_div" style="margin: 0 auto; height:637px !important">
```

Posteriormente, se debe ejecutar la función `Webpay.show(...)` para desplegar el formulario. Esta función se debe ejecutar después de que el div haya sido creado en el DOM (por ejemplo, dentro de `window.onload`, en un tag script casi al cerrar el tag `body` o algo similar).
```html
<div id="webpay_div" style="margin: 0 auto; height:637px !important">
<script>
    Webpay.show("webpay_div", "EL_TOKEN_DE_LA_TRANSACCION_CREADA_ACA", success_callback, error_callback);
    
    function success_callback(token) {
        // Llamar a una página del backend por ajax para realizar la confirmación (commit) enviando el token de la transacción.
        // Una vez que se realiza el commit, y el respondeCode de la transacción sea 0, entonces mostrar mensaje de éxito. 
    }

    function error_callback(token, errorCode, errorDetail) {
        // Opcional: Llamar a una página del backend para "informar" que la transacción fue rechazada (enviando el token de la transacción)
        alert(errorDetail);
    }
</script>
```

Esta función recibe los siguientes parámetros: 
- **Container**: ID del elemento HTML en donde se incluirá dinámicamente el formulario de pago
- **Token**: Token de la transacción, recibido al crear la transacción. 
- **Success Callback**: Esta es una función Javascript que se ejecutará en caso de **éxito**. Recibe como parámetro el 
  token de la transacción, que debe ser utilizado para realizar la confirmación de pago (commit). 
- **Error Callback**: Esta es una función Javascript que se ejecutará cuando haya ocurrido algún error. Recibe el token, un código de
  error y una descripción. El comercio debe tomar estos errores como errores fatales
  terminales, los cuales no le permitirán continuar con esta transacción (token se
  invalida), por lo que, en el caso de querer persistir su flujo de compra, debes
  reiniciar el pago con una nueva transacción.

El ancho de este div es responsivo, y muestra la vista móvil para viewports menores o iguales
a 659px, por el contrario, mostrará desde 660px la versión desktop. El alto es fijo y para el
móvil es de 637px, mientras que para desktop es de 435px. Es de responsabilidad del
comercio verificar el viewport de su layout, particularmente que esta versión permite el uso
de divs con ancho porcentual. Por lo que si se despliega la versión móvil o de escritorio
dependerá de los componentes que puedan estar rodeando al iframe en el sitio del comercio.

<aside class="notice">
Tip: En el ambiente de integración puedes usar la tarjeta VISA 4051885600446623
para hacer pruebas simples de éxito. El CVV es 123 y la fecha de vencimiento
cualquiera superior a la fecha actual.Para pruebas exhaustivas <a href="https://www.transbankdevelopers.cl/documentacion/como_empezar#ambientes">consulta todas las
tarjetas de prueba en la sección de Ambientes</a>.
</aside>

### Esperar que el usuario termine el flujo de pago


Una vez que el tarjetahabiente ha pagado, Webpay Plus retornará
el control al comercio mediante Javascript, ejecutando la función de éxito ("Success Callback"). 
En ese momento, el comercio debe realizar la confirmación de la transacción (commit) para validar el estado del pago.

Manteniendo el ejemplo de código anterior, donde usamos el nombre `success_callback` para nuestra función de éxito, sería algo como: 
```html
<script>
function success_callback(token) {
    console.log('Confirmando transacción...');
    jQuery.post('http://mitiendadeejemplo.cl/modal/commit', {token: token}, function(ajaxResponse) {
      if (ajaxResponse.responseCode === 0) {
        alert('Compra aprobada');
        // Mostrar voucher con los datos de la transacción directamente en la página, redirigir a una página de éxito o lo que se estime conveniente. 
        window.location = 'http://mitiendadeejemplo.cl/transacción/modal/voucher/' + token;
      } else {
          alert('La transacción ha sido rechazada. Intenta nuevamente')
      }
    })
}
</script>
```
Lo importante del código anterior es entender que una vez que se ejecuta el success callback, no significa necesariamente que la 
transacción haya sido aprobada, sino que el flujo de pago finalizó correctamente. 
Es en ese momento en el que debemos confirmar en la transacción (en nuestro servidor, en el backend) y en este código de ejemplo
se envía el token mediante AJAX a una API dentro de nuestro sitio. Es en ese endpoint en donde se realiza la confirmación (también puede ser una redirección a otra página o algo así). 

### Confirmar una transacción

Este método recibe un parámetro: El token de la transacción.  

```php
use Transbank\Webpay\Webpay\Modal\Transaction;

$response = Transaction::commit($token);
```


<div class="language-simple" data-multiple-language></div>

```php
use Transbank\Webpay\Webpay\Modal\Transaction;

$response = Transaction::commit($token);
```


<strong>Respuesta de confirmar una transacción</strong>

<div class="language-simple" data-multiple-language></div>


```php
$response->getVci();
$response->getAmount();
$response->getStatus();
$response->getBuyOrder();
$response->getSessionId();
$response->getCardDetail();
$response->getCardNumber();
$response->getAccountingDate();
$response->getTransactionDate();
$response->getAuthorizationCode();
$response->getPaymentTypeCode();
$response->getResponseCode();
$response->getInstallmentsAmount();
$response->getInstallmentsNumber();
```
 
Si `responseCode`  es igual a `0`, entonces la transacción fue aprobada satisfactoriamente. Cualquier otro valor indicará que el pago fue rechazado.
En este momento, dependiendo netamente del responseCode, puedes verificar el estado de la transacción y realizar las acciones que necesites, como aprobar el pedido en tu base de datos, rebajar stock, etc.

<strong>Mostrar el voucher</strong>
Siguiendo con el ejemplo, podemos utilizar la respuesta de la confirmación podrás mostrar un comprobante o página de éxito a tu usuario en caso de que el responseCode sea igual a cero.
Si es diferente de cero, puedes mostrar un mensaje indicando que la transacción fue rechazada para que pueda reintentar el pago (se debe comenzar el proceso desde cero, creando una nueva transacción y obteniendo un nuevo token)

### Obtener estado de una transacción


Esta operación permite obtener el estado de la transacción en los siguientes 7 días desde su creación.
En condiciones normales es probable que no se requiera ejecutar, pero en caso de ocurrir un error
inesperado permite conocer el estado y tomar las acciones que correspondan.

Debes enviar el `token` dela transacción de la cual desees obtener el estado.

<div class="language-simple" data-multiple-language></div>

```php
use Transbank\Webpay\Webpay\Modal\Transaction;

$response = Transaction::getStatus($token);  
```

<strong>Respuesta estado de una transacción</strong>

Para obtener la información contenida en la respuesta puedes hacerlo de la siguiente manera.

<div class="language-simple" data-multiple-language></div>


```php
$response->getAmount();
$response->getStatus();
$response->getBuyOrder();
$response->getSessionId();
$response->getCardDetail();
$response->getCardNumber();
$response->getAccountingDate();
$response->getTransactionDate();
$response->getAuthorizationCode();
$response->getPaymentTypeCode();
$response->getResponseCode();
$response->getInstallmentsAmount();
$response->getInstallmentsNumber();
$response->getBalance();
```

### Reversar o Anular una transacción
Esta operación permite a todo comercio habilitado, reversar o anular una
transacción que fue generada en Webpay Plus.

Puedes realizar un reembolso invocando al método refund(), dependiendo de algunas condiciones correspondera a una **Reversa** o **Anulación**.  

Puedes [leer más sobre la anulación en la información del
producto Webpay](/producto/webpay#anulaciones) para conocer
más detalles y restricciones.

<aside class="notice">
El método `Transaction.refund()` debe ser invocado siempre indicando el código del comercio que realizó la transacción.
</aside>

<div class="language-simple" data-multiple-language></div>


```php
use Transbank\Webpay\Webpay\Modal\Transaction;

$response = Transaction::refund($token, $amount);
```

<strong>Respuesta Reversa o Anulación</strong>

Para obtener la información contenida en la respuesta puedes hacerlo de la siguiente manera.

<div class="language-simple" data-multiple-language></div>


```php
$response->getAuthorizationCode();
$response->getAuthorizationDate();
$response->getBalance();
$response->getNullifiedAmount();
$response->getResponseCode();
$response->getType();
```

## Credenciales y Ambiente

Para Webpay Modal, las credenciales del comercio (código de comercio y API Key) vienen pre configurados en los SDK. 

Si no usas un SDK, estos es el código de comercio y Api Key del ambiente de integración:

La **llave secreta (Api Key Secret)** es `579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C`

Producto | Código de Comercio |
-------- | ------------ |
Webpay Modal | `597055555584`

Si quieres apuntar a producción tienes dos opciones: puedes re-configurar el SDK para que apunte a producción utilizando el código de comercio y API Key o puedes configurar cada llamada a las operaciones siguientes para pasar un objeto `Options` en el cual configuras donde quieres apuntar.

Ejemplo:
```php
$apiKey = 'MiApiKeySEcret_Llave_secreta';
$commerceCode = '5970MICODIGO';
$environment = 'LIVE'; //Ambiente de producción

$options = new Transbank\Webpay\Options($apiKey, $commerceCode, $environment);
Transaction::create($amount, $buyOrder, $sessionId, $options);
```
A todos los métodos se les puede entregar un objeto Options en el último parámetro, que permite especificar un código de comercio, api key y ambiente. 

Puede encontrar más información al respecto [en este link](/documentacion/como_empezar#b-utilizando-los-sdk)

## Conciliación de Transacciones

Una vez hayas realizado transacciones en producción quedará un historial de
transacciones que puedes revisar entrando a
[www.transbank.cl](https://www.transbank.cl/). Si lo deseas  puedes realizar una
conciliación entre tu sistema y el reporte que entrega el portal.

## Ejemplos de integración

Ponemos a tu disposición una serie de repositorios en nuestro Github para ayudarte a entender la integración de mejor forma.

[Puedes verlos acá](/documentacion/como_empezar#ejemplos)

<div class="container slate">
  <div class='slate-after-footer'>
    <div class='row d-flex align-items-stretch'>
      <div class='col-12 col-lg-6'>
        <h3 class='toc-ignore fo-size-22 text-center'>¿Tienes alguna duda de integración?</h3>
        <a href='https://transbank.continuumhq.dev/slack_community' target='_blank'>
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
