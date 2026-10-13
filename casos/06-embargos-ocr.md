# 6. Lectura automática de oficios de embargo

**Rol:** diseño y desarrollo

## Problema
Los oficios de embargo (de juzgados, la DIAN, etc.) llegan en PDF o Word, muchas veces escaneados. Había que buscar a mano cada cédula o NIT en el core para saber si la persona era asociada.

## Solución
- Extracción de texto de PDF, Word y **OCR** para documentos escaneados.
- Detección automática de cédulas y NIT en el texto.
- Consulta directa al core Db2 **desde el servidor** (los navegadores no pueden abrir conexiones a bases de datos), con credenciales solo en el equipo que la ejecuta.
- Si el driver de Db2 no está disponible, la app genera la consulta SQL lista para ejecutar manualmente.

## Resultado
Lo que antes se buscaba cédula por cédula ahora se revisa en segundos por oficio.

**Tecnologías:** Next.js · TypeScript · Tesseract.js · pdf.js · IBM Db2
