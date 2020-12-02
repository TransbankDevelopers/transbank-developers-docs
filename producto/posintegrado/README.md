# POS Integrado

<div class="pos-title-nav">
  <div tbk-link='/documentacion/posintegrado?csharp' tbk-link-name='Documentación'></div>
  <div tbk-link='/referencia/posintegrado?csharp' tbk-link-name='Referencia'></div>
</div>

El POS integrado permite realizar transacciones con tarjeta de Crédito/Débito/Prepago con Transbank utilizando el puerto serial de un PC o Caja. La comunicación con Transbank y la lógica del procesamiento de una transacción financiera es realizada por el equipo POS, facilitando así la integración con un sistema de caja.

## Transacciones Soportadas

* Venta.
* Cierre.
* Anulación.
* Última venta.
* Detalle de ventas.
* Totales.
* Carga de llaves.
* Poll (prueba de comunicación POS - caja).
* Cambio a modalidad POS normal.
* Venta multicódigo.
* Ultima venta multicódigo
* Detalle de ventas multicódigo.

## Requerimientos

### Requisitos en Local

* El comercio debe contar con sistema propio de caja, con las siguientes características deseables:
  * Sistema de Lectura de producto (scanner / código).
  * Sistema de Stock.
  * Sistema de Cuadratura.
  * Proveedor de Soporte de Sistema.
  * Acceso Remoto.
* La caja debe contar con a lo menos un puerto `RS232` o en su defecto con entrada USB exclusiva para el Dispositivo POS.
* El comercio debe disponer en cada local con puntos exclusivos para conectar a Internet vía Lan `RJ45` los POS.
* Se recomienda segmentar el ancho de banda con `300 kb` exclusivos para el uso del POS, dado que cuando existen procesos batch, cámaras de seguridad, uso de aplicaciones de escritorio, etc. bajo la mismo segmento pueden afectar una transacción si la banda ancha contratada no es suficientemente alta.
* El local debe disponer con un punto de corriente exclusivo de `220v` para enchufar el POS.

### Requisitos del Comercio

* El comercio debe contar con una o más áreas encargadas de los siguientes puntos:
  * Procedimientos de Contingencia ante caídas de señal Internet y energía eléctrica.
  * Procedimiento de Respaldo y Recuperación de operaciones efectuadas en POS Integrado de Transbank.
  * Homologación en versiones sistemas de Caja.
  * Homologación en Sistema Operativos de Caja.
  * Manejo de Claves Supervisora y de usuarios de Sistema Operativo.
  * Procedimientos de Seguridad y responsabilidad en el uso de la información proporcionada por Transbank y Clientes.
  * Capacitación y divulgación de uso de nuevos sistemas.
  * Manuales de Sistemas.
  * Soporte de Redes.

<aside class="notice">
Se recomienda que el Establecimiento posea la misma versión de Sistema Operativo y Software de Caja en todas sus cajas. En caso de poseer distintas versiones, será necesario efectuar un proceso de Integración y Certificación con Transbank por cada uno de ellos.
</aside>

<aside class="notice">
Ante cualquier cambio que efectúe el Establecimiento, ya sea de Sistema Operativo o Software de Caja, podrían existir impactos en la Integración efectuada con el POS Integrado, por lo que se sugiere probar la correcta operación con POS Integrado antes de efectuar el cambio en ambiente Productivo.
</aside>

## Seguridad

### Confidencialidad de la Información

De acuerdo a las normativas vigentes, las transacciones con Tarjeta de Crédito y Débito incorporan los siguientes elementos de seguridad en el sistema:

* La información leída en el punto de venta NO es almacenada en ningún sistema.
* Para efectos de cuadratura e identificación de transacciones, se utilizar el NÚMERO DE OPERACIÓN.

### Tratamiento de los Tracks de la Tarjeta

La información grabada en el Track I y Track II es leída sólo por los dispositivos de seguridad (POS). Estos dispositivos encriptan el contenido del Track I y Track II.

### Tratamiento de datos sensibles

Para asegurar la confidencialidad de la información, los mensajes de las transacciones o al menos los datos sensibles (además del PIN) viajan encriptados en los distintos tramos de la conexión, tanto en el requerimiento como en la respuesta. Como datos sensibles se consideran: número de tarjeta, fecha expiración, número de cuenta y monto de la transacción (dato validado en Autenticación de Mensajes o MAC).

### El modelo Master/Session KEY

El método actual de administración de llaves es el llamado Máster/Session Key, en el cual los PED (Pin Entry Device) son cargados en un ambiente seguro con una Master Key y en forma remota se carga la Working Key o Session Key.

El procedimiento actual para cifrar en los Pin Pads un PinBlock es el siguiente:

* Se descifra la Working Key usando la Master Key que tiene cargado el PED.
* Con la Working Key, se cifra el PinBlock y se envía al servidor.

La Working Key se cambia en forma periódica (al menos en cada cierre), para evitar que sea descubierta por
terceros.

Este modelo de administración de llaves es el que se usa para las llaves MAC.

### El modelo DUKPT – Encriptación de PIN

El nuevo método de administración de llaves para PIN que usará Transbank es el denominado ―Llave Única derivada por transacción o DUKPT por sus iniciales en inglés.

Bajo este método los PED son inicializados en un ambiente seguro, con datos de identificación propios de cada PED (Identificador de la llave de derivación, Identificador de PED único y un contador de transacciones iniciado en cero), más una llave inicial que se calcula usando los datos propios de cada PED y la llave de derivación base. Con esta llave inicial se genera la próxima llave de cifrado para PIN. Este proceso se realiza con una función asimétrica (DUKPT del PinPad), es decir, una función de un solo sentido, de forma que el PED no sea capaz de generar ninguna llave anterior a la actual.

### Cálculo de MAC

Para asegurar la integridad de la información que viaja desde y hacia el Autorizador de comercio, se introduce un código de autenticación de mensajes (MAC) el cual es enviado en el mensaje de requerimiento y validado por el Autorizador de Transbank al recibirlo. A su vez, el Autorizador de Transbank envía un código de MAC para el mensaje de respuesta, el cual debe ser validado por la caja. Si la validación que hace la caja del código de MAC es negativa debe generar una reversa. La transacción de reversa debe ser igual a la respuesta recibida pero con el campo RESPONSE CODE con el valor 989 y el campo MESSAGE SUBTYPE en R. Cuando el Autorizador de Transbank detecta un MAC inválido en el mensaje de requerimiento, envía un mensaje de respuesta con código de rechazo 898 (MAC inválido).

### Manejo de llaves MAC (Message Authentication Code)

Las llaves criptográficas para la generación de MAC (working key de MAC) se maneja de acuerdo a lo siguiente:

* Las working key son generadas por el sistema de Transbank y transmitidas en línea para cada uno de los terminal ID definidos en el comercio cliente.
* Para la carga y / o cambio de la working keys de MAC se utilizan las transacciones de CIERRE BATCH y CARGA DE LLAVE (Ver Transacciones Administrativas).

Las llaves working keys de MAC se actualizan en cada nueva transacción atendida por Transbank. Por lo que la caja debe registrar esta nueva llave para su uso en la siguiente transacción.
Las llaves se deben cambiar automáticamente todos los días. Esto implica que debe existir un procedimiento de inicialización o cierre obligatorio en cada caja (terminal ID) que se ejecuta en forma automática todos los días y que como parte de este procedimiento se envía a Transbank una transacción de CIERRE BATCH o CARGA DE LLAVE por cada caja (terminal ID).

Las working keys (MAC) se transmiten encriptadas utilizando el algoritmo DES (dato a encriptar es la working key) con una llave de encriptación denominada master key, definida por Transbank. Transbank define una master key para PIN y otra master key para MAC.

Transbank carga inicialmente las master keys en cada POS, operación que se realiza previamente a la instalación de éstos en las cajas.

Para la carga de las master keys de PIN y MAC, el modelo de POS utilizado debe contar con un dispositivo cargador de llaves que será administrado por Transbank y que permite:

Ingresar las master keys en el dispositivo, la que no podrá ser modificada, violada o adulterada. Cargar las master keys conectando uno por uno los POS al dispositivo

### Manejo de Clave Técnico

Para acceder al Menú de opciones para Técnico, deberá acreditarse con el RUT y la Clave que corresponde a este rut. Esta clave es de generación dinámica, con caducidad máxima en 31 días.

### Manejo de Clave Supervisora

En las versiones más antiguas cada comercio tenía una tarjeta supervisora que le permitía autenticarse para realizar cierres, anulaciones y otras operaciones. A partir del año 2011 en adelante, durante el proceso de auto instalación se solicitará la digitación de la clave supervisora, quedando esta almacenada hasta que el comercio desee cambiarla, siendo esta responsabilidad del mismo. Si el comercio olvida esta clave, existe una clave maestra de comercio que permite la digitación de una nueva clave de comercio. Para obtenerla, debe llamar a Servicio al Cliente, desde celulares 600 638 6380 y desde teléfonos fijos +56 2 2661 2700.

### Manejo de Clave Maestra de Comercio Activación

La solicitud de esta clave se realiza a Servicio al Cliente, desde celulares 600 638 6380 y desde teléfonos fijos +56 2 2661 2700.

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
