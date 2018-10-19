# transbank-developers-docs

Docs en markdown de lo que se publica en transbankdevelopers.cl

La estructura de directorios sigue las mismas URLs del portal 
http://transbankdevelopers.cl. Por ejemplo el contenido de 
https://transbankdevelopers.cl/referencia/onepay proviene del archivo
README.md dentro de [./referencia/onepay](./referencia/onepay).

### Restricciones

- Sólo se soportan archivos README.md. Cualquier archivo con otro nombre no 
será llevado al portal en el repositorio de Cumbre. 

- Todos los links deben ser realizados de manera absoluta sobre la raíz y 
apuntando al *directorio* y no al archivo README. 

  - ✅ Link correcto: `/producto/webpay`
  - ❗ Link incorrecto: `../producto/webpay`
  - ❗ Link incorrecto: `/producto/webpay/README.md`

  (En el caso de las **imágenes** deben linkearse también de manera absoluta, 
  pero en este caso **apuntando al archivo y no al directorio**)
