# US-002-03 — Publicar Oferta y Activar Automatizaciones Iniciales

## Narrativa

**Como** recruiter  
**Quiero** publicar una oferta aprobada  
**Para** que esté visible para recibir candidaturas y se activen las automatizaciones configuradas

## Descripción

Esta historia implementa el flujo de publicación de ofertas que han sido aprobadas por el hiring manager. Al publicar, la oferta cambia a estado `PUBLISHED`, se registra la fecha de publicación y se notifica al motor de automatización para que evalúe y ejecute las reglas no-code asociadas (ej: pipeline inicial, notificaciones internas, emails de confirmación configurados).

## Criterios de Aceptación

```gherkin
Escenario 1: Publicar oferta aprobada
  Dado que una oferta está en estado "APPROVED"
  Cuando el recruiter hace clic en "Publicar oferta"
  Entonces el estado de la oferta cambia a "PUBLISHED"
  Y se registra la fecha de publicación en el campo published_at
  Y la oferta queda visible para recibir candidaturas
  Y el recruiter ve confirmación "Oferta publicada correctamente"

Escenario 2: Activación de automatizaciones iniciales
  Dado que se publica una oferta con ID 123
  Cuando el backend actualiza el estado a PUBLISHED
  Entonces se emite evento de dominio "JobPostingPublished" con payload { job_posting_id: 123 }
  Y el motor de automatización recibe el evento
  Y evalúa las reglas no-code activas para evento "JobPostingPublished"
  Y ejecuta las acciones configuradas (ej: crear pipeline inicial, notificar a equipo, registrar métrica)
  Y registra eventos de ejecución en tabla AutomationEvent

Escenario 3: Pipeline inicial creado automáticamente
  Dado que se publica una oferta
  Y existe una regla de automatización que crea etapas de pipeline inicial
  Cuando la oferta cambia a estado PUBLISHED
  Entonces se crean automáticamente registros en tabla PipelineStage:
    | Nombre           | Posición | is_final |
    | Applied          | 1        | false    |
    | Screening        | 2        | false    |
    | Interview        | 3        | false    |
    | Offer            | 4        | false    |
    | Hired            | 5        | true     |
    | Rejected         | 6        | true     |
  Y el pipeline queda listo para recibir candidatos

Escenario 4: No se puede publicar oferta no aprobada
  Dado que una oferta está en estado "DRAFT" o "UNDER_REVIEW"
  Cuando el recruiter intenta hacer clic en "Publicar oferta"
  Entonces el botón está deshabilitado
  Y muestra tooltip "La oferta debe ser aprobada antes de publicarse"
```

## Notas Técnicas

- **Estados de JobPosting**: Solo ofertas en estado `APPROVED` pueden publicarse.
- **Evento de dominio**: `JobPostingPublished` emitido al bus interno de eventos (Redis/in-memory).
- **Motor de automatización**: Escucha evento `JobPostingPublished` y ejecuta reglas activas asociadas.
- **Pipeline inicial**: Configuración por defecto de etapas (puede ser customizable por empresa en el futuro).
- **Permisos**: Solo Recruiters y Admins pueden publicar ofertas.
- **Auditoría**: Registrar evento en `AuditLog` (quién publicó, cuándo).

## Tareas

- [ ] Añadir botón "Publicar oferta" en UI de oferta (solo visible si estado=APPROVED)
- [ ] Implementar endpoint `POST /api/job-postings/:id/publish` en Backend
- [ ] Actualizar estado de oferta a PUBLISHED y campo published_at en BD
- [ ] Emitir evento de dominio `JobPostingPublished` al bus de eventos interno
- [ ] Configurar suscripción del motor de automatización al evento `JobPostingPublished`
- [ ] Implementar lógica de creación de pipeline inicial (etapas por defecto)
- [ ] Crear registros en tabla `PipelineStage` asociados a la oferta
- [ ] Validar permisos: solo Recruiters y Admins pueden publicar
- [ ] Implementar UI de tooltip/mensaje si oferta no está aprobada
- [ ] Registrar evento en tabla `AuditLog` para publicación
- [ ] Crear tests unitarios de validación de estado (solo APPROVED puede publicarse)
- [ ] Crear tests de integración: publicación → emisión de evento → creación de pipeline
- [ ] Crear tests del motor de automatización con evento mock
- [ ] Documentar endpoint en OpenAPI/Swagger

## Estimación

**Story Points:** 8  
**Tiempo estimado:** 5-6 días

## Dependencias

- **US-002-02-RevisionAprobacionOferta**: Oferta debe estar en estado APPROVED.
- Motor de automatización básico implementado (infraestructura de eventos).
- Bus de eventos interno configurado (Redis o in-memory).

## Prioridad

**Alta** — Sin publicación no hay candidaturas; automatizaciones son diferenciador clave de LTI.
