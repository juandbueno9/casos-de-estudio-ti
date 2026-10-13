# 1. Centro de observabilidad de servidores

**Rol:** diseño, implementación y operación · **Entorno:** servidores Ubuntu en AWS + perímetro con FortiGate

## Problema
La entidad tenía varios servidores en AWS sin una vista centralizada. Las caídas y los certificados SSL vencidos se detectaban cuando un usuario se quejaba, y no se sabía con certeza quién entraba por SSH a cada servidor, porque todos usaban el mismo usuario del sistema.

## Solución
Un stack de observabilidad completo, más un panel propio adaptado a las necesidades del equipo.

```
 Servidores (Ubuntu / AWS)              Servidor de monitoreo
┌──────────────────────────┐        ┌──────────────────────────────────┐
│ node_exporter            │◄───────│ Prometheus ──► Alertmanager ──► correo
│ agente propio (bash):    │        │     │                            │
│  servicios, puertos,     │        │     ├──► Grafana                 │
│  contenedores, sesiones  │        │     └──► Panel propio (React)    │
│  SSH, parches, info AWS  │        │ Blackbox: HTTP, SSL, puertos TCP │
│ Promtail ────────────────┼───────►│ Loki (logs, retención 90 días)   │
└──────────────────────────┘        └──────────────────────────────────┘
```

- **Agente en Bash** que instala node_exporter y publica métricas propias: servicios activos, puertos abiertos con el proceso que los usa, contenedores Docker, sesiones y accesos SSH, parches pendientes y metadatos de AWS.
- **Auditoría SSH por huella de llave:** aunque todos entran con el mismo usuario, el agente identifica a cada persona por la huella de su llave pública. Si entra una llave no registrada, se dispara una alerta.
- **Blackbox exporter:** disponibilidad y latencia de los sitios web, días que faltan para que venza cada certificado SSL, y una prueba de **exposición de puertos sensibles** hacia internet, incluyendo la IP WAN del firewall.
- **Loki + Promtail + Alertmanager:** logs centralizados con retención de 90 días, reglas de alerta y notificación por correo.
- **Usuarios de solo lectura** en MySQL y PostgreSQL para monitorear las bases de datos sin riesgo.
- **Panel web propio** (React + Node.js) con vistas de flota, alertas, seguridad, sitios/SSL y auditoría. Las acciones del equipo, como silenciar alertas o exportar datos, quedan registradas en logs de solo agregar.

## Decisiones técnicas
- Los servicios nuevos van en un `docker-compose` **separado** del original, con respaldo, validación y reversión automática, para no poner en riesgo lo que ya funcionaba.
- Prometheus no se expone a internet; para desarrollar se usa un **túnel SSH** a `localhost`.
- Producción detrás de **Nginx** con servicios de **systemd**.

## Resultado
- Se detectan caídas, certificados por vencer y puertos expuestos **antes** de que afecten a los usuarios.
- Se sabe quién accedió a cada servidor y cuándo.
- Una vista única del estado de toda la infraestructura.

**Tecnologías:** Prometheus · node_exporter · Blackbox exporter · Grafana · Loki · Promtail · Alertmanager · Docker Compose · Nginx · systemd · Bash · Node.js · React · AWS EC2
