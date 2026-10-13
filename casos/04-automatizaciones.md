# 4. Automatizaciones programadas en servidor

**Rol:** desarrollo e instalación en servidor Linux

Varias tareas manuales que convertí en procesos automáticos en un servidor Ubuntu.

| Automatización | Qué hace | Cómo |
|---|---|---|
| **Alerta de tasas** | Consulta las tasas publicadas por el Banco de la República cada hora en horario laboral y envía correo solo si alguna cambió (la primera corrida del día siempre notifica). | Python + Playwright (Chromium headless), cron, script de instalación idempotente con `set -euo pipefail` |
| **Monitor de normatividad** | Detecta nuevas publicaciones normativas de una alcaldía y envía un resumen HTML con enlaces a los PDF. | Python, consumo de la API JSON interna del portal en vez de scraping frágil, Microsoft Graph. Solo marca como notificado si el correo salió bien. |
| **Correo de bienvenida** | Detecta asociados nuevos en el core y les envía un correo de bienvenida, sin enviarlo dos veces. | Python, consulta de solo lectura a Db2, Microsoft Graph, modo de prueba sin tocar la base |
| **Extractos de cartera** | Genera un PDF de estado de cuenta por asociado, cifrado con contraseña. | Python, pandas, ReportLab, pypdf |

## Buenas prácticas aplicadas
- Scripts de instalación que validan sus requisitos antes de ejecutarse y fallan de forma explícita.
- Entornos virtuales de Python, dependencias fijadas y credenciales en `.env` fuera de git.
- Modos `--test` / `--inspeccionar` para validar sin afectar producción.

**Tecnologías:** Python · Bash · cron · Playwright · IBM Db2 · Microsoft Graph · ReportLab
