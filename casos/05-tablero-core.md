# 5. Tablero del core financiero en solo lectura

**Rol:** diseño y desarrollo

## Problema
La gerencia necesitaba indicadores diarios (colocación, captaciones, recaudo, cartera por agencia), pero consultar el core financiero en producción podía bloquear tablas y afectar la operación.

## Solución
- API en Node.js conectada al core **IBM Db2** con un **usuario de solo lectura**, y todas las consultas con `WITH UR` (lectura sin bloqueos).
- Tablero en React con varias vistas: resumen, colocación por día, línea, agencia y asesor, y comparativo de agencias.
- **Modo demo** con datos enmascarados para cuando la base no responde o para hacer demostraciones sin exponer información real.
- Script de prueba de conexión y de cada consulta antes de desplegar.

## Resultado
Indicadores al día sin riesgo para el core y sin depender de reportes manuales.

**Tecnologías:** IBM Db2 · Node.js · Express · React · Vite
