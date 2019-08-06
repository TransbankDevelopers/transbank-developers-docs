#

<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Compatibilidad</h4>
    <ul>
      <li>Magento >= 2.0</li>
      <li>PHP >= 5.6 y PHP <= 7.2</li>
    </ul>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu llave privada y pública</li>
      <li>Contar con Magento2 instalado en tu sitio</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Webpay Magento2</h1>
<h1 style="display: none;">Webpay</h1>

## Descripción

Este plugin oficial ha sido creado para que puedas integrar Webpay fácilmente en tu comercio, basado en Magento2.

## Requisitos

1. Debes tener instalado previamente Magento2 y asegurarte de tener [Composer](https://getcomposer.org) instalado.
2. Tus credenciales de Magento Market a mano. Si no sabes cuales son tus credenciales puedes revisar esta guía: [https://devdocs.magento.com/guides/v2.2/install-gde/prereq/connect-auth.html](https://devdocs.magento.com/guides/v2.2/install-gde/prereq/connect-auth.html)

Habilitar los siguientes módulos / extensiones para PHP:

- Soap
- OpenSSL 1.0.1 o superior
- SimpleXML
- DOM 2.7.8 o superior

## Instalación del Plugin

**Nota:** En este punto composer podría pedirte si lo requiere tus credenciales de magento2.

**Versiones disponibles** [Aquí](https://packagist.org/packages/transbank/webpay-magento2)

En tu directorio de Magento2, ejecutar el siguiente comando para instalar la última versión:

    composer require transbank/webpay-magento2

<img src="/images/plug/mage/webpay/paso7.png" class="rounded mx-auto d-block"/>

Cuando finalice, ejecutar el comando:

    magento module:enable Transbank_Webpay --clear-static-content

<img src="/images/plug/mage/webpay/paso8.png" class="rounded mx-auto d-block"/>

Cuando finalice, ejecutar el comando:

    magento setup:upgrade && magento setup:di:compile && magento setup:static-content:deploy

<img src="/images/plug/mage/webpay/paso9.png" class="rounded mx-auto d-block"/>

Una vez realizado el proceso anterior, Magento2 debe haber instalado el plugin Webpay. Cuando finalice, debes activar el plugin en el administrador de Magento2.

## Configuración

Este plugin posee un sitio de configuración que te permitirá ingresar credenciales que Transbank te otorgará y además podrás generar un documento de diagnóstico en caso que Transbank te lo pida.

Para acceder a la configuración, debes seguir los siguientes pasos:

1. Dirígete a la página de administración de Magento2 (usualmente en <http://misitio.com/admin>, <http://localhost/admin>) e ingresa usuario y clave.

<img src="/images/plug/mage/webpay/paso10.png" class="rounded mx-auto d-block"/>

2. Dentro del sitio de administración dirígete a (Stores / Configuration).

<img src="/images/plug/mage/webpay/paso11.png" class="rounded mx-auto d-block"/>

3. Luego a sección (Sales / Payments Methods).

<img src="/images/plug/mage/webpay/paso12.png" class="rounded mx-auto d-block"/>

4. Elegir el país Chile

<img src="/images/plug/mage/webpay/paso13.png" class="rounded mx-auto d-block"/>

5. Bajando al listado de métodos de pagos verás Webpay

<img src="/images/plug/mage/webpay/paso14.png" class="rounded mx-auto d-block"/>

6. ¡Ya está! Estás en la pantalla de configuración del plugin, debes ingresar la siguiente información:

    - **Enable**: Al activarlo, Webpay estará disponible como medio de pago. Ten la precaución de que se encuentre marcada esta opción cuando quieras que los usuarios paguen con Webpay.
    - **Ambiente**: Ambiente hacia donde se realiza la transacción.
    - **Código de comercio**: Es lo que te identifica como comercio. Si el código que posees es de 8 dígitos debes anteponer `5970`.
    - **Llave Privada**: Llave secreta que te autoriza y valida a hacer transacciones.
    - **Certificado Público**: Llave pública que te autoriza y valida a hacer transacciones.

  Las opciones disponibles para _Ambiente_ son: "Integración" para realizar pruebas y certificar la instalación con Transbank, y "Producción" para hacer transacciones reales una vez que Transbank ha aprobado el comercio.

<aside class="notice">
  Tip: Ya no es necesario configurar el **Certificado Transbank**, en versiones anteriores era requerido, actualmente este se reemplaza al seleccionar **ambiente**.
</aside>

### Credenciales de Prueba

Para el ambiente de Integración, puedes utilizar las siguientes credenciales para realizar pruebas:

- Código de comercio: `597020000540`
- Llave Privada: Se puede encontrar [aquí - private_key](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.key)
- Certificado Público: Se puede encontrar [aquí - public_cert](https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.crt)

<aside class="notice">
  Tip: Al momento de pasar al ambiente de producción será necesario generar tu propia llave privada, puedes ver las instrucciones para crearlas [aquí](https://www.transbankdevelopers.cl/documentacion/como_empezar#credenciales-en-webpay)
</aside>

7. Guardar los cambios presionando el botón [Save Config]

<img src="/images/plug/mage/webpay/paso15.png" class="rounded mx-auto d-block"/>

8. Además, puedes generar un documento de diagnóstico en caso que Transbank te lo pida. Para ello, haz click en "Parámetros Principales" botón "Información" ahí podrás descargar un pdf.

<img src="/images/plug/mage/webpay/paso17.png" class="rounded mx-auto d-block"/>

## Configuración de magento2 para Chile CLP

El plugin solamente funciona con moneda chilena CLP dado esto magento2 debe estar correctamente configurado para que que se pueda usar Webpay.

1. Ir a la sección de administración (Stores / General / Country Option) y elegir Chile tal como se muestra en la siguiente imagen, luego guardar los cambios.

<img src="/images/plug/mage/webpay/clp1.png" class="rounded mx-auto d-block"/>

2. Ir a la sección de administración (Stores / Currency Setup / Country Option) y elegir Chile tal como se muestra en la siguiente imagen, luego guardar los cambios.

<img src="/images/plug/mage/webpay/clp2.png" class="rounded mx-auto d-block"/>

3. Ir a la sección de administración (Stores / Currency) y verificar en las dos secciones (Currency Rates y Currency Symbols) que CLP se encuentre activo.

<img src="/images/plug/mage/webpay/clp3.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/mage/webpay/clp4.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/mage/webpay/clp5.png" class="rounded mx-auto d-block"/>

## Prueba de instalación con transacción

En ambiente de integración es posible realizar una prueba de transacción utilizando un emulador de pagos online.

- Ingresa al comercio

<img src="/images/plug/mage/webpay/demo1.png" class="rounded mx-auto d-block"/>

- Ya con la sesión iniciada, ingresa a cualquier sección para agregar productos

<img src="/images/plug/mage/webpay/demo2.png" class="rounded mx-auto d-block"/>

- Agrega al carro de compras un producto:

<img src="/images/plug/mage/webpay/demo3.png" class="rounded mx-auto d-block"/>

- Selecciona el carro de compras y luego presiona el botón [Proceed to Checkout]:

<img src="/images/plug/mage/webpay/demo4.png" class="rounded mx-auto d-block"/>

- Selecciona método de envío y presiona el botón [Next]

  Debes asegurarte que tu dirección de envío sea en Chile.

<img src="/images/plug/mage/webpay/demo5.png" class="rounded mx-auto d-block"/>

- Selecciona método de pago Transbank Webpay, luego presiona el botón [Place Order]

<img src="/images/plug/mage/webpay/demo6.png" class="rounded mx-auto d-block"/>

- Una vez presionado el botón para iniciar la compra, se mostrará la ventana de pago Webpay y deberás seguir el proceso de pago.

Para pruebas puedes usar los siguientes datos:

- Número de tarjeta: `4051885600446623`
- Rut: `11.111.111-1`
- Cvv: `123`

<img src="/images/plug/mage/webpay/demo7.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/mage/webpay/demo8.png" class="rounded mx-auto d-block"/>

Para pruebas puedes usar los siguientes datos:

- Rut: `11.111.111-1`
- Clave: `123`

<img src="/images/plug/mage/webpay/demo9.png" class="rounded mx-auto d-block"/>

Puedes aceptar o rechazar la transacción

<img src="/images/plug/mage/webpay/demo10.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/mage/webpay/demo11.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/mage/webpay/demo12.png" class="rounded mx-auto d-block"/>

- Serás redirigido a Magento2 y podrás comprobar que el pago ha sido exitoso.

<img src="/images/plug/mage/webpay/demo13.png" class="rounded mx-auto d-block"/>

- Además si accedes al sitio de administración sección (Sales / Orders) se podrá ver la orden creada y el detalle de los datos entregados por Webpay.

<img src="/images/plug/mage/webpay/order1.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/mage/webpay/order2.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/mage/webpay/order3.png" class="rounded mx-auto d-block"/>
