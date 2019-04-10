# POS Integrado

## A

## Tabla de respuestas

Respuesta                                               | Código
------                                                  | -----------
Aprobado                                                | 00
Rechazado                                               | 01
Host no Responde                                        | 02
Conexión Fallo                                          | 03
Transacción ya Fue Anulada                              | 04
No existe Transacción para Anular                       | 05
Tarjeta no Soportada                                    | 06
Transacción Cancelada desde el POS                      | 07
No puede Anular Transacción Debito                      | 08
Error Lectura Tarjeta                                   | 09
Monto menor al mínimo permitido                         | 10
No existe venta                                         | 11
Transacción No Soportada                                | 12
Debe ejecutar cierre                                    | 13
No hay Tono                                             | 14
Archivo BITMAP.DAT no encontrado. Favor cargue          | 15
Error Formato Respuesta del HOST                        | 16
Error en los 4 últimos dígitos.                         | 17
Menú invalido                                           | 18
ERROR_TARJ_DIST                                         | 19
Tarjeta Invalida                                        | 20
Anulación. No Permitida                                 | 21
TIMEOUT                                                 | 22
Impresora Sin Papel                                     | 24
Fecha Invalida                                          | 25
Debe Cargar Llaves                                      | 26
Debe Actualizar                                         | 27
Error en Número de Cuotas                               | 60
Error en Armado de Solicitud                            | 61
Problema con el Pinpad interno                          | 62
Error al Procesar la Respuesta del Host                 | 65
Superó Número Máximo de Ventas, Debe Ejecutar Cierre    | 67
Error Genérico, Falla al Ingresar Montos                | 68
Error de formato Campo de Boleta MAX 6                  | 70
Error de Largo Campo de Impresión                       | 71
Error de Monto Venta, Debe ser Mayor que 0              | 72
Terminal ID no configurado                              | 73
Debe Ejecutar CIERRE                                    | 74
Comercio no tiene Tarjetas Configuradas                 | 75
Supero Número Máximo de Ventas, Debe Ejecutar CIERRE    | 76
Debe Ejecutar Cierre                                    | 77
Esperando Leer Tarjeta                                  | 78
Solicitando Confirmar Monto                             | 79
Solicitando Ingreso de Clave                            | 81
Enviando transacción al Host                            | 82
Error Cantidad Cuotas                                   | 88
Declinada                                               | 93
Error al Procesar Respuesta                             | 94
Error al Imprimir TASA                                  | 95

<aside class="warning">
Toda transacción cuyo código de respuesta en el POS Integrado sea distinto de `0` será considerada como un rechazo. Por secreto bancario el detalle de la causa del rechazo no será entregado al comercio.
</aside>

## Tabla de Abreviación de Tarjetas

Tarjeta         | Abreviación
VISA            | VI
MASTERCARD      | MC
CABAL           | CA
CREDENCIAL      | CR
AMEX            | AX
CERRADA         | CE
DINNERS         | DC
PRESTO          | TP
MAGNA           | MG
MAS (CENCOSUD)  | TM
RIPLEY          | RP
EXTRA           | EX
CMR             | TC
REDCOMPRA       | DB