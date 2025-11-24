# US-003-02 — Aplicación de Candidato a Oferta

## Narrativa

**Como** recruiter  
**Quiero** vincular un candidato existente a una oferta publicada  
**Para** iniciar su proceso de selección en el pipeline

## Descripción

Esta historia permite asociar candidatos a ofertas creando registros de `Application`. Un candidato puede aplicar a múltiples ofertas, y cada aplicación tiene su propio estado y progreso en el pipeline. Al crear la aplicación, se asigna automáticamente a la primera etapa del pipeline (`Applied`).

## Criterios de Aceptación

```gherkin
Escenario 1: Aplicar candidato existente a oferta
  Dado que existe un candidato "Laura Martínez" y una oferta "Senior Backend Engineer" publicada
  Cuando el recruiter abre el perfil del candidato
  Y hace clic en "Aplicar a oferta"
  Y selecciona "Senior Backend Engineer" del dropdown
  Y hace clic en "Confirmar"
  Entonces se crea un registro en Application con:
    - candidate_id = ID de Laura
    - job_posting_id = ID de la oferta
    - current_stage_id = ID de etapa "Applied"
    - status = ACTIVE
    - source = (fuente del candidato)
    - applied_at = timestamp actual
  Y el candidato aparece en el pipeline de la oferta

Escenario 2: Evitar aplicación duplicada
  Dado que Laura Martínez ya tiene una aplicación activa en "Senior Backend Engineer"
  Cuando el recruiter intenta aplicarla nuevamente a la misma oferta
  Entonces el sistema muestra mensaje "Este candidato ya tiene una aplicación activa en esta oferta"
  Y no se crea ningún registro duplicado

Escenario 3: Aplicación a múltiples ofertas
  Dado que Laura Martínez está en pipeline de "Senior Backend Engineer"
  Cuando el recruiter la aplica también a "Tech Lead"
  Entonces se crea una segunda Application independiente
  Y ambas aplicaciones tienen su propio estado y progreso en pipeline

Escenario 4: Registro en historial de etapas
  Dado que se crea una nueva Application
  Cuando el sistema asigna la etapa inicial "Applied"
  Entonces se crea un registro en ApplicationStageHistory con:
    - application_id
    - from_stage_id = NULL (primera etapa)
    - to_stage_id = ID de "Applied"
    - changed_at = timestamp actual
    - changed_by_user_id = recruiter actual
```

## Notas Técnicas

- **Modelo de datos**: Registro en `Application` con foreign keys a `Candidate`, `JobPosting` y `PipelineStage`.
- **Validación unicidad**: No permitir múltiples aplicaciones activas del mismo candidato a la misma oferta (unique constraint en BD).
- **Etapa inicial**: Obtener la etapa con `position=1` del pipeline de la oferta.
- **Historial automático**: Crear entrada en `ApplicationStageHistory` al crear Application.

## Tareas

- [ ] Diseñar modal de "Aplicar a oferta" en perfil de candidato
- [ ] Implementar endpoint `POST /api/applications` en Backend
- [ ] Crear migración de BD para tabla `Application`
- [ ] Implementar validación de unicidad (candidato + oferta + status=ACTIVE)
- [ ] Obtener etapa inicial del pipeline (query a `PipelineStage` con position=1)
- [ ] Crear registro automático en `ApplicationStageHistory`
- [ ] Implementar dropdown de ofertas publicadas (solo de la empresa del recruiter)
- [ ] Manejar error si la oferta no tiene pipeline configurado
- [ ] Crear tests unitarios de validación de unicidad
- [ ] Crear tests de integración: Application → ApplicationStageHistory
- [ ] Documentar endpoint en OpenAPI/Swagger

## Estimación

**Story Points:** 3  
**Valor de Negocio:** 5 (Crítico — conecta candidatos con ofertas)  
**Urgencia:** 5 (Bloqueante para MVP)

## Dependencias

- **US-002-03-PublicarOfertaAutomatizaciones**: Debe existir al menos una oferta publicada con pipeline
- **US-003-01-RegistroCandidato**: Debe existir al menos un candidato

## Prioridad

**Alta** — Sin aplicaciones no hay pipeline
