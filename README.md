# transbank-developers-docs

Docs en markdown de lo que se publica en transbankdevelopers.cl

La estructura de directorios sigue las mismas URLs del portal
<http://transbankdevelopers.cl>. Por ejemplo el contenido de
<https://transbankdevelopers.cl/referencia/onepay> proviene del archivo
README.md dentro de [./referencia/onepay](./referencia/onepay).

## Restricciones

* Sólo se soportan archivos README.md. Cualquier archivo con otro nombre no
será llevado al portal en el repositorio de Cumbre.

* Todos los links deben ser realizados de manera absoluta sobre la raíz y
apuntando al *directorio* y no al archivo README.

  * ✅ Link correcto: `/producto/webpay`
  * ❗ Link incorrecto: `../producto/webpay`
  * ❗ Link incorrecto: `/producto/webpay/README.md`

  (En el caso de las **imágenes** deben linkearse también de manera absoluta,
  pero en este caso **apuntando al archivo y no al directorio**)

### Build

Via <https://github.com/continuum/transbankdevelopers-web> el markdown simple de
este repositorio se transforma y lleva a los repositorios requeridos por Cumbre
para ser publicado en el sitio transbankdevelopers.cl.

[![Build
Status](https://semaphoreci.com/api/v1/continuum/transbankdevelopers-web/branches/master/badge.svg)](https://semaphoreci.com/continuum/transbankdevelopers-web)

## Versionamiento de API

Al momento de crear una versión relacionada a **referencia** o **documentación**, se desplegará un selector de versiones en transbankdevelopers.cl, el cual mostrará por defecto la última versión, dejando de visualizar el archivo README.md que se encuentra en la raíz.

_En caso que no exista una **carpeta con el número de versión**, se cargará por defecto el archivo README.md existente en la raíz._

> Ejemplo: al crear la siguiente versión `/referencia-o-documentacion/webpay/1.0.0/README.md`, se dejará de mostrar la información que contiene el siguiente archivo `/referencia-o-documentacion/webpay/README.md`

### Nueva versión

Para nuevas versiones relacionadas a **referencia** o **documentación** respectivamente, se debe **crear una carpeta** con el número de versión y en la siguiente estructura `/referencia-o-documentacion/{{nombre producto}}/{{número versión}}/README.md` que se quiere publicar.

> Ejemplo: referencia/webpay/1.0.0/README.md

**Los archivos README.md siguen el mismo formato que se utiliza para las publicaciones actuales de la documentación.**
