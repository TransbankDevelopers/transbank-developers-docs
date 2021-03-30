# transbank-developers-docs

Docs in markdown of what is published in transbankdevelopers.cl

The directory structure follows the same URLs as the portal
 <http://transbankdevelopers.cl>. For example the content of
 <https://transbankdevelopers.cl/referencia/onepay> comes from the file
README.md inside [./reference/onepay)(./reference/onepay).

## Restrictions

* Only README.md files are supported. Any file with another name will not
will be taken to the portal in the Cumbre repository.

* All links must be made absolutely on the root and
pointing to the * directory * and not the README file.

  * ✅ Correct link: `/ product / webpay`
  * ❗ Incorrect link: `../ product / webpay`
  * ❗ Incorrect link: `/ product / webpay / README.md`

  (In the case of ** images ** they must also be linked absolutely,
  but in this case ** pointing to the file and not the directory **)

### Build

Via <https://github.com/continuum/transbankdevelopers-web> the simple markdown of
this repository is transformed and leads to the repositories required by Cumbre
to be published on the site transbankdevelopers.cl.

[! [Build
Status] (https://semaphoreci.com/api/v1/continuum/transbankdevelopers-web/branches/master/badge.svg)] (https://semaphoreci.com/continuum/transbankdevelopers-web)
