# 2. Despliegue de aplicaciones y migración a PostgreSQL autohospedado

**Rol:** despliegue, administración de servidor y base de datos

## Problema
Dos aplicaciones internas, una de gestión de convenios y otra de caja menor, dependían de Supabase Cloud para la base de datos, la autenticación y los archivos. Por ser una entidad financiera se necesitaba que **los datos quedaran en infraestructura propia**, y además una de las apps debía conectarse al core legado (IBM Db2), algo que no se puede hacer desde funciones serverless.

## Solución
- **Migración a PostgreSQL autohospedado**: esquema completo en SQL plano, con las reglas de negocio como triggers en la base de datos (consecutivos, `updated_at`).
- **Capa de compatibilidad** que imita la API de supabase-js sobre el driver `pg`, así se migró **sin reescribir** el código de la aplicación.
- **Archivos en disco local** con enlaces temporales firmados, en lugar de Supabase Storage.
- **Estructura estándar en el servidor** para todos los proyectos:

```
/opt/enterprise-projects/<app>/
├── backups/   # pg_dump diario comprimido (cron)
├── config/    # .env de producción (fuera del código)
├── files/     # archivos subidos por usuarios
├── logs/      # salida de pm2
└── source/    # código desplegado
```

- **Ejecución persistente** con pm2/systemd, **Nginx** como reverse proxy y script de despliegue repetible.
- **Backups**: `pg_dump | gzip` programado con cron, más respaldo de los archivos.

## Resultado
- Datos 100 % en infraestructura propia, cumpliendo los requisitos de la entidad.
- Un procedimiento de despliegue documentado que se reutiliza en los demás proyectos.

**Tecnologías:** Ubuntu Server · PostgreSQL · Nginx · pm2 · systemd · cron · Node.js · Next.js · IBM Db2
