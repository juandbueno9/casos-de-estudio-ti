# Casos de estudio — Infraestructura y Automatización TI

Proyectos que diseñé e implementé en el área de sistemas de una **cooperativa financiera en Colombia**.

> **Nota:** el código fuente y la configuración pertenecen a mi empleador y contienen información confidencial del sector financiero. Aquí describo el problema, la solución, la arquitectura y los resultados, **sin código, credenciales ni datos de la entidad**. Con gusto los amplío en una entrevista.

## Índice

| # | Caso | Área | Tecnologías |
|---|---|---|---|
| 1 | [Centro de observabilidad de servidores](casos/01-observabilidad.md) | Monitoreo / Seguridad | Prometheus, Grafana, Loki, Alertmanager, Blackbox, Bash, AWS |
| 2 | [Despliegue y migración a PostgreSQL autohospedado](casos/02-despliegue-postgres.md) | Despliegue / Bases de datos | Ubuntu, Nginx, pm2, systemd, PostgreSQL, cron |
| 3 | [Identidad, SSO y correo seguro](casos/03-identidad-sso.md) | Identidad / Seguridad | Keycloak, Microsoft Entra ID, Microsoft Graph, OAuth2 |
| 4 | [Automatizaciones programadas en servidor](casos/04-automatizaciones.md) | Automatización | Python, Bash, cron, Playwright, Db2 |
| 5 | [Tablero del core financiero en solo lectura](casos/05-tablero-core.md) | Datos / Seguridad | IBM Db2, Node.js, React |
| 6 | [Lectura automática de oficios de embargo](casos/06-embargos-ocr.md) | Automatización / OCR | Next.js, Tesseract, Db2 |

## Prácticas que aplico en todos

- **Credenciales fuera del código**: variables de entorno, plantillas `.env.example` y archivos sensibles fuera de git.
- **Mínimo privilegio**: usuarios de base de datos de solo lectura y permisos de API solo de aplicación.
- **Cambios reversibles**: cada instalación en servidor tiene respaldo, validación y un procedimiento documentado para revertirla.
- **Auditoría**: quién hizo qué y cuándo, en logs de solo agregar.
- **Documentación**: cada proyecto tiene README con instalación, operación y solución de problemas.
