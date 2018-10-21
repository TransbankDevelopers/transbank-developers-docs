<script>$(function () {$('[data-toggle="popover"]').popover()});</script>

<div class="data-menu-side-right">
  <div class="btn-side-right"><span><img src="/images/navbar.png"></span></div>
  <div class="block-cantainer">
    <h4>Descarga nuestro plugin.</h4>
    {% for key, x in tool %}
      {% if x.fileActive %}
        <span class="btn-download" data-target='#modalDownloadPlugins' id="press-p-btn-{{x.ident}}"
              data-plugin="download?type_d=plugin_v&f={{x.fileActive}}&v={{x.ident}}&tool={{x.tool}}&type_t={{x.type_t}}&pr={{x.pr}}">
          <b class="td_btn-more sm">Versión {{x.version}}</b>
        </span>
        <h4>Compatibilidad</h4>
        <ul>
          <li>{{x.ecommerce}}</li>
          <li>{{x.lenguaje}}</li>
          <li><span data-container="body" data-toggle="popover" data-placement="top" data-content="{{x.md5}}">md5</span></li>
          <li><span data-container="body" data-toggle="popover" data-placement="top" data-content="{{x.sha1}}">sha1</span></li>
        </ul>
      {% endif %}
    {% endfor %}
    <span class="btn-download top-x2 bottom-x2" data-toggle="modal" data-target="#modalChangelogPlugins"><b>Changelog</b></span>
    <h4>Recuerda</h4>
    <ol>
      <li>Contar con tu llave privada y pública</li>
      <li>Tener correctamente configurado el Ecommerce</li>
      <li>Contar con un sitio 'https' seguro</li>
    </ol>
  </div>
</div>

<h1 class="toc-ignore">Webpay Virtuemart</h1>
<h1 style="display: none;">Webpay</h1>

## Requisitos
Tener instalado y configurado previamente tu E-commerce.

Habilitar los siguientes modulos para PHP:
+ Soap
+ OpenSSL 1.0.1 o superior
+ SimpleXML
+ DOM 2.7.8 o superior
+ Mcrypt
+ Sockets
+ Único protocolo de comunicación aceptado: TLS 1.2

## Instalación

1. Una vez descargado el plugin ingresa al panel de administración de Virtuemart y selecciona:
**Extensions → Manage**

<img src="/images/plug/virtue/webpay/01.png" class="rounded mx-auto d-block">

2. Al estar en el módulo de Manage, seleccionamos **install**

<img src="/images/plug/virtue/webpay/02.png" class="rounded mx-auto d-block">

3. Luego la opción **Seleccionar archivo** y selecciona el Plugin recién descargado

<img src="/images/plug/virtue/webpay/03.png" class="rounded mx-auto d-block">

4. Una vez seleccionado el Plugin, presionamos **Upload & Install**, si todo es correcto tendremos un mensaje satisfactorio:

<img src="/images/plug/virtue/webpay/04.png" class="rounded mx-auto d-block"/>

5. Luego en el menú izquierdo seleccionamos la opción **"Manage"** y habilitamos el plugin. Podemos introducir la palabra Webpay, para buscar mas fácilmente nuestro plugin lo checkeamos y presionamos el botón Enable:

<img src="/images/plug/virtue/webpay/05.png" class="rounded mx-auto d-block"/>

## Configuración

1. Una vez instalado el plugin de Webpay Plus es momento de configurarlo. Para debemos ingresar a **"VirtueMart → Payment Methods"**:

<img src="/images/plug/virtue/webpay/06.png" class="rounded mx-auto d-block"/>

2. Estando en el módulo de métodos de pago debemos crear un nuevo método, para lo cual presionamos el botón **"New"**:

<img src="/images/plug/virtue/webpay/07.png" class="rounded mx-auto d-block"/>

Asignamos un nombre al medio de pago (Payment Name), un alias, seleccionamos Yes en la opción de Published, para hacer público en nuestro e-commerce el medio de pago. Introducimos una descripción (Payment Description) y lo más importante en Payment Method seleccionamos nuestro plugin. Ahora presionamos el botón Save.

### Observaciones
<dl>
  <dt>Los ambientes que encontrarás en la configuración del plugin son:</dt>

  <dd>**Integración**: Modo de prueba, tanto código de comercio, llaves y certificados, vienen dadas por defecto en el plugin y te permitirá generar simulaciones de compra para verificar la conexión con webpay de Transbank.</dd>

  <dd>**Certificación**: Etapa en la que Transbank está validando tu e-commerce (proceso QA).</dd>

  <dd>**Producción**: Una vez que Transbank certifica tu e-commerce, estarás en condiciones de vender a través de tu sitio.</dd>
</dl>

Para acceder a los ambientes de Certificación y Producción, debes afiliar tu comercio a Transbank. Una vez realizado, se te entregará un código de comercio, con el que deberás crear la llave y el certificado público. El certificado público debes enviarlo a Transbank para validar tu comercio.

Para conocer los requisitos de afiliación.  <a href="https://portaltransbank.cl/afiliacion/" target="blank">https://portaltransbank.cl/afiliacion/</a>

### Ejemplo de Compra

A modo de ejemplo realizaremos la compra de un producto de una tienda y su interacción con el Plugin de Webpay Plus. Como vemos Webpay Plus se encuentra disponible como medio de pago dentro del eCommerce creado gracias al Plugin instalado.

<img src="/images/plug/virtue/webpay/08.png" class="rounded mx-auto d-block"/>

Al realizar el pedido, aparecerá un pequeño resumen, si todo nos parece bien presionamos el botón "Confirm Purchase".

El siguiente paso es la presentación del formulario de Webpay Plus, lo que inicia la interacción entre Transbank y tu eCommerce.

<img src="/images/plug/webpay_form/form_01.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/webpay_form/form_02.png" class="rounded mx-auto d-block"/>

Luego se presenta el formulario del emisor correspondiente a la tarjeta ingresada para validar las claves del usuario. Los datos a ingresar para finalizar la compra son:
+ Rut: 11.111.111-1
+ Clave: 123

Si lo haces con Tarjeta de Crédito, ingresa en el campo asignado los siguientes datos:
+ Nº tarjeta: 4051885600446623
+ Rut: 11.111.111-1
+ Clave: 123
+ Vencimiento: Cualquiera
+ Sin cuotas

<img src="/images/plug/webpay_form/form_03.png" class="rounded mx-auto d-block"/>

<img src="/images/plug/webpay_form/form_04.png" class="rounded mx-auto d-block"/>

Para presentar el voucher de Webpay Plus con el detalle de la transacción exitosa.

<img src="/images/plug/webpay_form/form_05.png" class="rounded mx-auto d-block"/>

Para finalizar con la presentación en el eCommerce del detalle de la transacción

<img src="/images/plug/virtue/webpay/12.png" class="rounded mx-auto d-block"/>
