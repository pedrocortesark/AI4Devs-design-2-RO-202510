# US-004-01 — Vista Kanban del Pipeline

## Narrativa

**Como** recruiter  
**Quiero** visualizar el pipeline de candidatos de una oferta en formato kanban  
**Para** tener una vista clara del estado de cada candidato y del proceso global

## Descripción

Esta historia implementa la vista kanban principal del pipeline, mostrando columnas por etapa y tarjetas por candidato. Es la interfaz operativa principal de LTI donde recruiters pasan la mayor parte de su tiempo gestionando procesos de selección.

## Criterios de Aceptación

```gherkin
Escenario 1: Visualizar pipeline en formato kanban
  Dado que una oferta "Senior Backend Engineer" tiene 10 candidatos en diferentes etapas
  Cuando el recruiter abre el pipeline de la oferta
  Entonces se muestra una vista kanban con columnas:
    | Nombre Columna | Cantidad Candidatos |
    | Applied        | 5                   |
    | Screening      | 3                   |
    | Interview      | 2                   |
    | Offer          | 0                   |
    | Hired          | 0                   |
    | Rejected       | 0                   |
  Y cada candidato aparece como tarjeta en su columna correspondiente

Escenario 2: Información visible en tarjeta de candidato
  Dado que se visualiza el pipeline
  Cuando un candidato aparece como tarjeta
  Entonces la tarjeta muestra:
    - Nombre del candidato
    - Foto de perfil (si existe) o avatar con iniciales
    - Fuente (Referral, Job Board, etc.)
    - Scoring de IA (si existe) con código de color
    - Fecha de última actividad
    - Iconos de acciones rápidas (ver perfil, mover)

Escenario 3: Orden de tarjetas en columna
  Dado que hay múltiples candidatos en la misma etapa
  Cuando se visualiza la columna
  Entonces las tarjetas están ordenadas por:
    1. Scoring de IA (descendente)
    2. Fecha de última actividad (más reciente primero)
  Y el usuario puede reordenar manualmente si lo desea

Escenario 4: Vista responsive
  Dado que el recruiter accede desde tablet o desktop
  Cuando visualiza el pipeline
  Entonces las columnas se ajustan al ancho de pantalla
  Y mantienen scroll horizontal si hay más columnas que espacio visible
  Y las tarjetas mantienen legibilidad
```

## Notas Técnicas

- **Componente UI**: Usar librería de kanban (react-beautiful-dnd o similar) para drag & drop.
- **Consultas**: Query a `Application` con joins a `Candidate`, `PipelineStage` y `AIInsight` (scoring).
- **Performance**: Implementar paginación/lazy loading si hay >50 candidatos por etapa.
- **Permisos**: Recruiters ven todos los candidatos; Hiring Managers solo ven su oferta.

## Tareas

- [ ] Diseñar componente Kanban en Frontend (React)
- [ ] Implementar endpoint `GET /api/job-postings/:id/pipeline` en Backend
- [ ] Consultar Applications con joins a Candidate, PipelineStage, AIInsight
- [ ] Ordenar candidatos por scoring IA + fecha actividad
- [ ] Implementar componente de tarjeta de candidato con datos relevantes
- [ ] Configurar react-beautiful-dnd para drag & drop (se implementará en US-004-02)
- [ ] Implementar scroll horizontal responsive
- [ ] Añadir iconos de acciones rápidas (ver perfil, mover)
- [ ] Implementar loading states y skeletons
- [ ] Crear tests unitarios de lógica de ordenación
- [ ] Crear tests de integración de carga de pipeline
- [ ] Documentar endpoint en OpenAPI/Swagger

## Estimación

**Story Points:** 8  
**Valor de Negocio:** 5 (Crítico — UI principal de LTI)  
**Urgencia:** 5 (Bloqueante para demo/MVP)

## Dependencias

- **US-002-03-PublicarOfertaAutomatizaciones**: Pipeline debe existir
- **US-003-02-AplicacionCandidato**: Candidatos deben estar aplicados

## Prioridad

**Crítica** — Diferenciador UX más visible de LTI
