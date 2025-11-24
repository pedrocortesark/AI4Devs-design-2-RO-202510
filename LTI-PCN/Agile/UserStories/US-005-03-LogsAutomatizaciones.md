# US-005-03 — Logs y Monitoreo de Automatizaciones

## Narrativa

**Como** recruiter o admin  
**Quiero** ver logs de ejecución de reglas de automatización  
**Para** verificar que funcionan correctamente, auditar acciones y diagnosticar errores

## Descripción

Esta historia implementa la vista de logs de automatizaciones, mostrando historial de ejecuciones de reglas (cuándo se dispararon, qué acciones ejecutaron, resultados). Permite filtrar por regla, estado (SUCCESS/FAILED) y rango de fechas. Es crítico para debugging y confianza del usuario en el sistema.

## Criterios de Aceptación

```gherkin
Escenario 1: Ver logs de todas las automatizaciones
  Dado que el sistema ha ejecutado 50 reglas en los últimos 7 días
  Cuando el recruiter accede a "Automatizaciones" → "Logs"
  Entonces se muestra una tabla con:
    | Fecha/Hora      | Regla                    | Evento Disparador      | Estado   | Acción Ejecutada       |
    | 2025-11-24 10:30| Email a Manager          | Candidato → Screening  | SUCCESS  | Email enviado          |
    | 2025-11-24 10:25| Confirmación candidato   | Candidato aplicado     | SUCCESS  | Email enviado          |
    | 2025-11-24 10:20| Notificar equipo         | Oferta publicada       | FAILED   | Error: SMTP timeout    |
  Y se muestra paginación (20 registros por página)

Escenario 2: Filtrar logs por estado
  Dado que el recruiter está en la vista de logs
  Cuando filtra por "FAILED"
  Entonces solo se muestran ejecuciones con errores
  Y se resaltan en rojo
  Y se muestra mensaje de error detallado al expandir

Escenario 3: Ver detalles de ejecución específica
  Dado que el recruiter selecciona un log de ejecución
  Cuando hace clic en "Ver detalles"
  Entonces se muestra modal con:
    - Regla ejecutada (nombre, configuración)
    - Evento que la disparó (payload completo)
    - Condiciones evaluadas (resultado de cada condición)
    - Acciones ejecutadas (timestamp, payload enviado, respuesta)
    - Errores (si aplica): stack trace, código de error

Escenario 4: Reintentar ejecución fallida manualmente
  Dado que una ejecución falló por timeout del servicio de email
  Cuando el admin hace clic en "Reintentar"
  Entonces el sistema reintenta ejecutar la misma acción
  Y registra nuevo evento en AutomationEvent
  Y actualiza el log mostrando resultado del reintento
```

## Notas Técnicas

- **Consulta BD**: Query a `AutomationEvent` con joins a `AutomationRule`, `AutomationAction`.
- **Payload storage**: Guardar payload de evento y respuesta en `AutomationEvent.metadata` (JSON).
- **Retención**: Logs se guardan 90 días (configurable), después se archivan o eliminan.
- **Performance**: Índices en `executed_at`, `status` para optimizar queries con filtros.

## Tareas

- [ ] Diseñar vista de logs de automatizaciones en Frontend
- [ ] Implementar endpoint `GET /api/automation-events` en Backend con filtros
- [ ] Consultar AutomationEvent con joins a AutomationRule y AutomationAction
- [ ] Implementar filtros por regla, estado, rango de fechas
- [ ] Crear vista detallada de ejecución (modal con payload completo)
- [ ] Implementar funcionalidad de "Reintentar" ejecución fallida
- [ ] Implementar endpoint `POST /api/automation-events/:id/retry` en Backend
- [ ] Añadir resaltado visual de errores (rojo) y éxitos (verde)
- [ ] Implementar paginación de logs (20 por página)
- [ ] Crear job de limpieza de logs antiguos (>90 días)
- [ ] Optimizar queries con índices en BD
- [ ] Crear tests unitarios de lógica de filtros
- [ ] Crear tests de integración de carga de logs
- [ ] Documentar endpoints en OpenAPI/Swagger

## Estimación

**Story Points:** 5  
**Valor de Negocio:** 4 (Importante para confianza y debugging)  
**Urgencia:** 4 (Necesario para soporte y troubleshooting)

## Dependencias

- **US-005-02-MotorEjecucionAutomatizaciones**: Logs se generan al ejecutar reglas

## Prioridad

**Alta** — Sin logs, las automatizaciones son una "caja negra"
