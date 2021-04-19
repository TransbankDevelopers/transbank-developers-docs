# POS Autoservicio

La solución Autoservicio se integra al kiosco de tu comercio, con la particularidad de que no necesita la intervención
de un dependiente de tu establecimiento, sino que permite que los mismos clientes realicen la transacción de pago de
manera completa y autónoma. Ideal para negocios como estacionamientos, bencineras y cines, entre otros.

## Cómo funciona

El paso a paso para su implementación:

1. **Auditoría de factibilidad técnica**
Establecerá si tu comercio tiene las facultades para realizar el desarrollo tecnológico que requiere esta
integración. En caso de aprobación, se te entregará un Documento Técnico con las indicaciones a seguir.

2. **Desarrollo**
Tu comercio deberá realizar el desarrollo según las especificaciones que te entregará Transbank.

3. **Control de Calidad**
Este proceso verificará que el desarrollo de integración que hiciste,
cumple con los que te entregamos en las especificaciones, o si requiere alguna corrección o mejora.

4. **Etapa piloto**
En un lugar que acordemos juntos, llevamos a producción tu punto de venta autoservicio, donde con cliente reales haremos monitoreo en conjunto de su funcionamiento, por un periodo acotado de 2 semanas. Evaluamos los resultados, y acordamos masificación, también si se requiere algún ajuste.

5. **Masificación**
Construimos en conjunto un plan de instalación de los puntos de ventas.

<aside class="notice">
Para el desarrollo y pruebas del comercio Transbank entrega un set de pruebas que se deben ejecutar y un set de tarjetas físicas para estos casos, estas tarjetas solo funcionan en ambiente de desarrollo, certificación o integración.
</aside>

**Flujo de venta en POS autoservicio:**

1. El cliente realiza la selección del producto o servicio a comprar
2. El cliente selecciona la forma de pago que disponga el kiosko, efectivo o tarjeta crédito, tarjeta débito.
3. El kiosco invoca al POS pasándole el monto a cobrar.
4. El cliente opera tarjeta, selecciona cuotas si corresponde, e ingresa su pin.
5. El POS informa al kiosco el resultado de la venta (aprobada o rechazada).
6. El kiosko libera el producto e imprime los comprobantes.

## Equipos y conexiones disponibles

### Verifone ux100, ux400, ux300

POS autoservicio con conexión LAN y Serial
<img src="/images/documentacion/autoatencion/ux100completo.png" alt="Verifone ux100, ux300 y ux400">

## Funciones de pago disponibles en HOST

* Venta Crédito, con o sin cuotas
* Venta Débito

## Cómo empezar

Por el momento, hay un SDK para [.NET](https://github.com/TransbankDevelopers/transbank-pos-sdk-dotnet).

### SDK .NET

Para .NET lo puedes encontrar en [NuGet.org](https://www.nuget.org/packages/TransbankPosSDK/) para instalarlo puedes utilizar por ejemplo el package manager de VisualStudio.

```bash
PM> Install-Package TransbankPosSDK
```
### Integración Nativa

Es recomendable utilizar un SDK disponible a la hora de desarrollar la integración, lo que ahorra tiempo y te despreocupa de desarrollar las comunicaciones con el equipo POS Autoservicio, facilitando bastante la integración, pero en el caso que prefieras realizar la integración por tu cuenta y utilizar los comandos nativos, puedes revisarlos en la [Referencia](/referencia/pos-autoservicio).

## Primeros pasos

<div class="pos-title-nav">
  <div tbk-link='/referencia/pos-autoservicio?csharp#primeros-pasos' tbk-link-name='Referencia'></div>
</div>

Para usar el SDK es necesario incluir las siguientes referencias.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
using Transbank.Responses.CommonResponses;
using Transbank.Responses.AutoservicioResponse;
```

### Listar puertos disponibles

Si los respectivos drivers están instalados, entonces puedes usar la función `ListPorts()` para identificar los puertos que se encuentren disponibles y seleccionar el que
corresponda con el puerto donde conectaste el POS Autoservicio.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POSAutoservicio;
//...
List<string> ports = POSAutoservicio.Instance.ListPorts();
```

### Abrir un puerto Serial

Para abrir un puerto serial y comunicarte con el POS Autoservicio, necesitarás el nombre del puerto (el cual puedes identificar usando [la función mencionada en el apartado anterior](/documentacion/pos-autoservicio#listar-puertos-disponibles)). También necesitarás el _baudrate_ al cual esta configurado el puerto serial del POS Autoservicio (por defecto es **9600**).

Si el puerto no puede ser abierto, se lanzará una excepción `TransbankException` en .NET y Java.

<div class="language-simple" data-multiple-language></div>

```csharp
using Transbank.POS;
using Transbank.POS.Utils;
//...
string portName = "COM3";
POSAutoservicio.Instance.OpenPort(portName);
```



## Documentación disponible

A continuación, encontrarás la documentación en formato PDF:

* **Manual de integración POS Autoservicio UX100/300/400** - _20.1_ | [Descargar](/files/manual-integracion-pos-autoservicio-20-1.pdf)
_Este documento tiene por objetivo explicar las funcionalidades que debe implementar el
Cliente o su proveedor de caja para el desarrollo del módulo de autoservicio (en este caso los
ejemplos se efectuaran con el teclado Verifone UX100, el lector de tarjeta Verifone UX300 y el
lector de tarjetas Contactless Verifone UX400 (pago sin contacto)), permitiendo realizar
transacciones bancarias de Crédito o Débito (redcompra)_

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
