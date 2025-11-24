# US-005-02 — Motor de Ejecución de Reglas de Automatización

## Narrativa

**Como** sistema LTI  
**Quiero** evaluar y ejecutar reglas de automatización cuando ocurren eventos de dominio  
**Para** automatizar acciones configuradas sin intervención manual

## Descripción

Esta historia implementa el motor de automatización que escucha eventos de dominio (ApplicationStageChanged, JobPostingPublished, etc.), evalúa reglas activas y ejecuta acciones configuradas (enviar emails, notificaciones). Incluye logging de ejecuciones y manejo de errores.

## Criterios de Aceptación

```gherkin
Escenario 1: Ejecución automática de regla al cambiar etapa
  Dado que existe una regla activa:
    - Disparador: "Candidato entra en Screening"
    - Acción: "Enviar email al Hiring Manager"
  Cuando un candidato es movido a etapa "Screening"
  Entonces el motor de automatización:
    1. Recibe evento "ApplicationStageChanged"
    2. Consulta reglas activas con trigger_type="ON_STAGE_CHANGE"
    3. Evalúa condiciones de cada regla
    4. Ejecuta acción "Enviar email" de la regla que aplica
    5. Registra evento en AutomationEvent con status=SUCCESS

Escenario 2: Evaluación de condiciones complejas
  Dado que una regla tiene condición "Si fuente es Referral"
  Cuando se evalúa para un candidato con source=JOB_BOARD
  Entonces la condición no se cumple
  Y no se ejecuta ninguna acción
  Y se registra en AutomationEvent con status=SKIPPED

Escenario 3: Manejo de errores en ejecución
  Dado que una regla debe enviar email al manager
  Cuando el servicio de email está caído (error 500)
  Entonces el motor de automatización:
    1. Captura la excepción
    2. Registra en AutomationEvent con status=FAILED y error_message
    3. Reintenta la acción hasta 3 veces con backoff exponencial
    4. Si falla definitivamente, envía alerta a Admins

Escenario 4: Procesamiento de múltiples reglas en paralelo
  Dado que 3 reglas aplican al mismo evento
  Cuando se dispara el evento
  Entonces el motor ejecuta las 3 acciones en paralelo
  Y registra 3 eventos en AutomationEvent (uno por regla)
  Y el sistema no se bloquea esperando respuestas
```

## Notas Técnicas

- **Arquitectura**: Módulo suscrito al bus de eventos interno (Redis/in-memory).
- **Evaluación**: Motor evalúa reglas en orden de prioridad/creación.
- **Ejecución paralela**: Usar workers/promises para ejecutar acciones sin bloquear.
- **Reintentos**: Implementar retry con backoff exponencial (1s, 2s, 4s).
- **Logging**: Todos los eventos registrados en `AutomationEvent` para auditoría.

## Tareas

- [ ] Implementar módulo de motor de automatización en Backend
- [ ] Suscribir motor al bus de eventos interno
- [ ] Implementar lógica de consulta de reglas activas por tipo de evento
- [ ] Implementar evaluador de condiciones (parsear trigger_config JSON)
- [ ] Implementar ejecutor de acciones (dispatcher según action.type)
- [ ] Crear implementación de acción SEND_EMAIL (integración con proveedor)
- [ ] Crear implementación de acción SEND_NOTIFICATION (notificación in-app)
- [ ] Implementar registro en AutomationEvent (SUCCESS/FAILED/SKIPPED)
- [ ] Implementar retry logic con backoff exponencial
- [ ] Implementar alertas a Admins en caso de fallos persistentes
- [ ] Configurar workers paralelos para ejecución de acciones
- [ ] Crear tests unitarios de evaluación de condiciones
- [ ] Crear tests de integración: evento → evaluación → ejecución → logging
- [ ] Crear tests de manejo de errores y reintentos

## Estimación

**Story Points:** 13  
**Valor de Negocio:** 5 (Crítico — sin motor no hay automatización)  
**Urgencia:** 5 (Bloqueante para MVP)

## Dependencias

- **US-005-01-BuilderAutomatizaciones**: Reglas deben estar configuradas
- **US-002-03-PublicarOfertaAutomatizaciones**: Bus de eventos debe existir
- Proveedor de email configurado

## Prioridad

**Crítica** — Sin motor, el builder es inútil
