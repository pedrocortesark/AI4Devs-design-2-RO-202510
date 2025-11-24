# US-002-01 — Crear Oferta de Trabajo con Asistencia de IA

## Narrativa

**Como** recruiter  
**Quiero** crear una nueva oferta de trabajo con ayuda de IA para mejorar la descripción  
**Para** publicar ofertas de alta calidad que atraigan candidatos cualificados

## Descripción

Esta historia implementa el flujo de creación de ofertas (`JobPosting`) desde el formulario inicial hasta el guardado como borrador. Incluye integración con IA para mejorar automáticamente la descripción del rol y generar un resumen atractivo. El recruiter puede aceptar sugerencias de IA, editarlas o mantener su texto original antes de guardar.

## Criterios de Aceptación

```gherkin
Escenario 1: Crear oferta con datos básicos
  Dado que un recruiter está autenticado en LTI
  Cuando accede al panel de ofertas y hace clic en "Crear nueva oferta"
  Y completa el formulario con:
    | Campo            | Valor                                  |
    | Título           | Senior Backend Engineer                |
    | Ubicación        | Madrid, España (Remoto)                |
    | Tipo de contrato | Full-time                              |
    | Descripción      | Buscamos ingeniero con 5+ años exp...  |
  Y hace clic en "Guardar como borrador"
  Entonces se crea un registro en JobPosting con estado "DRAFT"
  Y el recruiter ve confirmación "Oferta guardada como borrador"

Escenario 2: Solicitar mejora de descripción con IA
  Dado que un recruiter está creando una oferta con descripción básica
  Cuando hace clic en "Mejorar con IA"
  Entonces el sistema envía la descripción al servicio de IA
  Y en menos de 5 segundos muestra una versión mejorada con:
    - Descripción del rol más clara y atractiva
    - Lista de responsabilidades estructuradas
    - Requisitos organizados en técnicos y blandos
    - Tono profesional pero humano
  Y el recruiter puede aceptar sugerencias, editarlas o descartarlas

Escenario 3: Asignar Hiring Manager a la oferta
  Dado que un recruiter está creando una oferta
  Cuando selecciona del dropdown "Hiring Manager" al usuario "Carlos Gómez"
  Y guarda la oferta
  Entonces la oferta queda asociada a Carlos Gómez como hiring_manager_id
  Y Carlos puede ver la oferta en su panel de ofertas asignadas

Escenario 4: Validación de campos obligatorios
  Dado que un recruiter intenta guardar una oferta
  Cuando no ha completado el campo "Título"
  Entonces el sistema muestra error "El título de la oferta es obligatorio"
  Y no se crea ningún registro en la base de datos
```

## Notas Técnicas

- **Modelo de datos**: Registro en tabla `JobPosting` con campos: `id`, `company_id`, `title`, `description`, `location`, `employment_type`, `status` (DRAFT), `hiring_manager_id`, `created_by_id`, `created_at`.
- **IA**: Llamada al servicio de IA (OpenAI GPT-4 o Claude) con prompt estructurado para mejora de ofertas.
- **Timeout**: Si IA no responde en 10 segundos, mostrar mensaje de error y permitir continuar sin sugerencias.
- **Permisos**: Solo Recruiters y Admins pueden crear ofertas.

## Tareas

- [ ] Diseñar formulario de creación de oferta en Frontend (React)
- [ ] Implementar endpoint `POST /api/job-postings` en Backend
- [ ] Crear migración de BD para tabla `JobPosting`
- [ ] Implementar validación de campos obligatorios (título, description)
- [ ] Crear servicio de integración con IA para mejora de descripciones
- [ ] Diseñar prompt de IA para optimización de ofertas
- [ ] Implementar llamada al servicio de IA con timeout de 10s
- [ ] Implementar dropdown de selección de Hiring Manager (consulta a tabla User con role=HIRING_MANAGER)
- [ ] Implementar botón "Mejorar con IA" con loading state
- [ ] Crear vista previa de sugerencias de IA (diff entre texto original y mejorado)
- [ ] Guardar registro en estado DRAFT con created_by_id = usuario actual
- [ ] Crear tests unitarios de validación de campos
- [ ] Crear tests de integración del flujo completo (sin llamada real a IA, usando mock)
- [ ] Documentar endpoint en OpenAPI/Swagger

## Estimación

**Story Points:** 8  
**Tiempo estimado:** 5-6 días

## Dependencias

- **US-001-02-LoginUsuario**: Recruiter debe estar autenticado.
- **US-001-03-InvitacionUsuarios**: Debe existir al menos un Hiring Manager para asignar.
- Servicio de IA configurado (OpenAI API key o Claude API key).

## Prioridad

**Alta** — Historia core del MVP, sin ofertas no hay proceso de reclutamiento.

---

## Actualización de Estimación

**Ajuste realizado:** 8 SP → 13 SP  
**Fecha:** 2025-11-24  
**Razón:** El análisis técnico de US-001-01 reveló que las historias fundacionales con migraciones multi-tenant, integraciones externas y arquitectura hexagonal tienen overhead significativo. Para US-002-01 se identificaron:
- Migración de Prisma para tabla JobPosting con relaciones a Company (FK multi-tenant), User (hiring_manager_id), PipelineStage (cascade)
- Índices compuestos para queries filtradas por company_id + status
- Integración con OpenAI/Claude para mejora de descripciones (configuración de API, manejo de errores, retry logic, tracking de costos)
- Sistema de permisos por rol (Recruiter vs Hiring Manager) con middleware de autorización
- Validación de ownership (solo miembros del company_id pueden crear ofertas)
- Arquitectura hexagonal (controller → use case → repository + service de IA)
- Tests unitarios de use case + tests de integración de endpoint + tests de manejo de errores de IA

La suma de migración compleja + IA + permisos multi-tenant justifica el ajuste de 8 a 13 SP (similar a EPIC-005 builder de automatizaciones).
