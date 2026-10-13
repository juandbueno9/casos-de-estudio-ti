# 3. Identidad, SSO y envío de correo seguro

**Rol:** integración y configuración de identidad

## Problema
Cada aplicación interna tenía su propio login, y los correos automáticos se enviaban con usuario y contraseña SMTP guardados en archivos de configuración.

## Solución
- **SSO con Keycloak** (OpenID Connect) para las aplicaciones internas, con un login local de prueba activable por variable de entorno para desarrollar.
- **Registros de aplicación en Microsoft Entra ID**, uno por sistema, con permiso **`Mail.Send` de aplicación** y consentimiento del administrador. El envío se hace por **Microsoft Graph** con OAuth2 *client credentials* (MSAL), sin contraseñas de buzón.
- SMTP solo como alternativa de respaldo, configurable.
- Sistema de **bitácoras de TI** con auditoría: actas de entrega de equipos con PDF, revisión de alertas (antivirus, directorio activo, backups, cámaras, red), control de acceso al datacenter, compras y suministros, todo exportable a Excel.

## Resultado
- Login único para el personal y menos contraseñas que administrar.
- Correo automático sin credenciales de buzón expuestas, y cada app con solo el permiso que necesita.

**Tecnologías:** Keycloak · OpenID Connect · Microsoft Entra ID · Microsoft Graph · MSAL · OAuth2 · Node.js
