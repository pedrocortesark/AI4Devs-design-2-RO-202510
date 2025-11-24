# US-002-02 — Revisión y Aprobación de Oferta por Hiring Manager

## Narrativa

**Como** recruiter  
**Quiero** enviar una oferta a revisión del hiring manager  
**Para** asegurar alineación con los requisitos del equipo antes de publicarla

## Descripción

Esta historia implementa el flujo de revisión de ofertas entre recruiter y hiring manager. Una vez creada la oferta como borrador, el recruiter la envía a revisión, lo que genera una notificación al hiring manager. El manager puede aprobar la oferta o solicitar cambios, devolviendo feedback al recruiter. Solo después de aprobación la oferta puede publicarse.

## Criterios de Aceptación

```gherkin
Escenario 1: Enviar oferta a revisión
  Dado que un recruiter ha creado una oferta en estado DRAFT
  Cuando hace clic en "Enviar a revisión"
  Entonces el estado de la oferta cambia a "UNDER_REVIEW"
  Y se envía una notificación por email al hiring manager asignado
  Y el recruiter ve confirmación "Oferta enviada a revisión de [Nombre del Manager]"

Escenario 2: Hiring Manager aprueba oferta
  Dado que un hiring manager recibe notificación de oferta en revisión
  Cuando accede a LTI y abre la oferta
  Y revisa el contenido (título, descripción, requisitos)
  Y hace clic en "Aprobar oferta"
  Entonces el estado de la oferta cambia a "APPROVED"
  Y se envía notificación al recruiter "Tu oferta [Título] ha sido aprobada"
  Y el recruiter puede proceder a publicarla

Escenario 3: Hiring Manager solicita cambios
  Dado que un hiring manager está revisando una oferta
  Cuando hace clic en "Solicitar cambios"
  Y añade comentarios "Falta especificar conocimientos en Kubernetes"
  Y hace clic en "Enviar feedback"
  Entonces el estado de la oferta cambia a "DRAFT" (vuelve a borrador)
  Y se envía notificación al recruiter con los comentarios del manager
  Y el recruiter puede editar la oferta y reenviar a revisión

Escenario 4: Notificación incluye link directo a la oferta
  Dado que se envía una notificación por email al hiring manager
  Cuando el manager hace clic en el link del email
  Entonces es redirigido directamente a la página de revisión de la oferta en LTI
  Y puede aprobar o solicitar cambios sin navegar por el sistema
```

## Notas Técnicas

- **Estados de JobPosting**: `DRAFT` → `UNDER_REVIEW` → `APPROVED` (o vuelta a `DRAFT` si solicita cambios).
- **Modelo de datos**: Añadir campo `feedback` (texto) en `JobPosting` o tabla separada `JobPostingFeedback` si se requiere historial completo.
- **Notificaciones**: Email con template personalizado + link directo formato `https://lti.app/job-postings/{id}/review`.
- **Permisos**: Solo el hiring manager asignado a la oferta puede aprobar/rechazar.
- **Auditoría**: Registrar evento en `AuditLog` cada cambio de estado (quién, cuándo, acción).

## Tareas

- [ ] Añadir botón "Enviar a revisión" en UI de oferta (solo visible si estado=DRAFT)
- [ ] Implementar endpoint `POST /api/job-postings/:id/submit-for-review` en Backend
- [ ] Actualizar estado de oferta a UNDER_REVIEW en BD
- [ ] Crear plantilla de email de notificación para hiring manager
- [ ] Implementar envío de email con link directo a oferta
- [ ] Crear página de revisión de oferta para hiring manager (Frontend)
- [ ] Implementar endpoint `POST /api/job-postings/:id/approve` en Backend
- [ ] Implementar endpoint `POST /api/job-postings/:id/request-changes` con body `{ feedback: string }` en Backend
- [ ] Validar permisos: solo hiring_manager_id asignado puede aprobar/rechazar
- [ ] Actualizar estado a APPROVED o vuelta a DRAFT según acción
- [ ] Crear plantilla de email de notificación al recruiter (aprobada o cambios solicitados)
- [ ] Registrar eventos en tabla `AuditLog` para cada cambio de estado
- [ ] Crear tests unitarios de validación de permisos
- [ ] Crear tests de integración del flujo completo de revisión
- [ ] Documentar endpoints en OpenAPI/Swagger

## Estimación

**Story Points:** 5  
**Tiempo estimado:** 3-4 días

## Dependencias

- **US-002-01-CrearOfertaConIA**: Debe existir una oferta en estado DRAFT.
- **US-001-03-InvitacionUsuarios**: Debe existir un Hiring Manager asignado.
- Proveedor de email configurado.

## Prioridad

**Alta** — Colaboración recruiter-manager es diferenciador clave de LTI.
