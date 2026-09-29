Reto 11 –15
Reto 11 – Automatización de facturas
El Reto 11 automatiza el registro de facturas recibidas por correo electrónico. El flujo detecta un correo con una factura en PDF, extrae automáticamente la información importante mediante IA y registra los datos en Google Sheets.
Ejemplo real:
Una empresa recibe por Gmail una factura de un proveedor. n8n toma el PDF, identifica proveedor, NIT, número de factura, fechas, subtotal, IVA, total y moneda, y automáticamente crea una nueva fila en Google Sheets.
¿Para qué sirve?
Permite automatizar el control y registro de facturas, evitando digitar manualmente la información. Puede utilizarse en áreas de contabilidad, tesorería, compras y gestión administrativa para llevar un registro organizado de las facturas recibidas.

Resumen corto – Reto 12: Gobernanza de Entregables
El Reto 12 automatiza la recepción y validación de archivos en Google Drive. n8n revisa si el nombre del archivo cumple una convención establecida y, dependiendo del resultado, lo clasifica automáticamente como aceptado o rechazado, lo mueve a la carpeta correspondiente y registra la información en Google Sheets.
Caso ficticio: una dependencia entrega un informe llamado ACME_NEBULA_INFORME_20260927_V01.pdf. Si cumple el formato, n8n lo envía a ACEPTADOS y registra la entrega; si no cumple, lo envía a RECHAZADOS y registra el motivo.
¿Para qué sirve? Para controlar entregables, reducir errores de nomenclatura, organizar documentos y mantener trazabilidad automática de las entregas.

Reto 13 – Monitor corporativo de vencimientos
El Reto 13 automatiza el seguimiento de documentos o compromisos que tienen una fecha de vencimiento. El sistema consulta periódicamente una hoja de Google Sheets, identifica cuáles registros están próximos a vencer o ya vencieron y, si requieren alerta, envía automáticamente un correo electrónico con el reporte y actualiza la fecha de la última alerta enviada.
Caso ficticio:
Una Alcaldía tiene contratos, pólizas y certificados registrados en una hoja de cálculo. Cada día, el flujo revisa las fechas. Si una póliza vence en 7 días, por ejemplo, el sistema envía un correo al responsable indicando el documento, la fecha de vencimiento y los días restantes.
¿Para qué se puede usar?
Se puede aplicar para controlar contratos, pólizas, certificados, licencias, mantenimientos, renovaciones, vencimientos de documentos y cualquier obligación que tenga una fecha límite, evitando que se pasen fechas importantes y reduciendo el seguimiento manual.

Reto 14: recibir y registrar solicitudes de compra.
Recibe una solicitud enviada desde una intranet, valida sus datos, calcula el valor de cada producto y el total, asigna un identificador y registra la información en Google Sheets. Después la envía al Reto 15 y confirma su recepción sin esperar la aprobación.
Reto 15: aprobar y emitir la orden de compra.
Según el total, aprueba automáticamente o solicita autorización al jefe o al área financiera. Si se rechaza, informa al solicitante. Si se aprueba, genera la orden en Google Docs, la guarda en Drive, notifica a compras y al solicitante, y actualiza el estado a EMITIDA.
Ejemplo ficticio: Laura solicita tres monitores y tres docks por $3.900.000 COP. El Reto 14 calcula y registra la solicitud. El Reto 15 pide aprobación al jefe; cuando este acepta, genera la orden y la envía al área de compras para tramitarla.
¿Para qué sirve? Para automatizar las compras internas de una empresa, reducir cálculos y registros manuales, controlar las aprobaciones y consultar el estado de cada solicitud.



https://sift-almost-opossum.ngrok-free.dev/webhook-test/solicitudes-compra

$webhookUrl = "https://sift-almost-opossum.ngrok-free.dev/webhook-test/solicitudes-compra"
$correoPrueba = johnbayona01@gmail.com

.\pruebas-retos14-15.ps1 -Caso Automatica

.\pruebas-retos14-15.ps1 -Caso Jefe

.\pruebas-retos14-15.ps1 -Caso Financiera
.\pruebas-retos14-15.ps1 -Caso UnProducto
.\pruebas-retos14-15.ps1 -Caso TresProductos

8. Pruebas de solicitudes inválidas
Ejecuta cada comando por separado, dejando el Webhook escuchando antes de cada envío:
.\pruebas-retos14-15.ps1 -Caso SinCorreo
.\pruebas-retos14-15.ps1 -Caso SinSolicitante
.\pruebas-retos14-15.ps1 -Caso SinItems
.\pruebas-retos14-15.ps1 -Caso ItemsVacios
.\pruebas-retos14-15.ps1 -Caso CantidadCero
.\pruebas-retos14-15.ps1 -Caso ValorNegativo
En todos estos casos esperamos:
•	Respuesta HTTP 400.
•	Ninguna fila nueva en Sheets.
•	Ninguna ejecución nueva del Reto 15.
•	Ningún correo de aprobación ni documento.
Los casos CantidadCero y ValorNegativo alteran el segundo producto, para comprobar que la validación revise todo el array.
Si alguno devuelve HTTP 202, habrá que corregir la validación del Reto 14.
9. Pruebas exactas de los límites
Estas pruebas comprueban que no existan errores justo en los límites:
Comando	Ruta esperada
.\pruebas-retos14-15.ps1 -Caso Limite -Monto 1000000	Automática
.\pruebas-retos14-15.ps1 -Caso Limite -Monto 1000001	Jefe
.\pruebas-retos14-15.ps1 -Caso Limite -Monto 5000000	Jefe
.\pruebas-retos14-15.ps1 -Caso Limite -Monto 5000001	Financiera

