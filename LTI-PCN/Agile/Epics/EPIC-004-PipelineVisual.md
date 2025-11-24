# EPIC-004 — Pipeline Visual

## Descripción

Esta épica implementa la vista kanban del pipeline de candidatos por oferta. Permite a recruiters y hiring managers visualizar candidaturas organizadas por etapas (`PipelineStage`), mover candidatos entre etapas mediante drag & drop, y acceder al historial de movimientos (`ApplicationStageHistory`).

El objetivo es ofrecer una experiencia visual intuitiva que convierta a LTI en el hub operativo diario del equipo de reclutamiento.

## Valor de Negocio

- **UX diferenciadora**: La vista kanban es el elemento más visible de LTI y mejora radicalmente la usabilidad frente a listas y tablas tradicionales.
- **Reducción de fricción operativa**: Mover candidatos de etapa es rápido y visual, sin formularios complejos.
- **Transparencia y trazabilidad**: El historial de etapas permite auditar decisiones y entender el flujo de cada candidato.
- **Preparación para automatización**: Los cambios de etapa son el disparador principal de reglas no-code.

## Funcionalidades Preliminares

- **Vista kanban por oferta**: Mostrar columnas por `PipelineStage` (Applied, Screening, Interview, Offer, Rejected, etc.).
- **Tarjetas de candidato**: Cada candidatura aparece como tarjeta con nombre, foto, datos clave y scoring de IA (si existe).
- **Drag & drop**: Mover candidatos entre etapas arrastrando las tarjetas.
- **Actualización en tiempo casi real**: Usar WebSocket/SSE para reflejar cambios cuando otro usuario modifica el pipeline.
- **Historial de etapas**: Ver todos los cambios de etapa de una candidatura (`ApplicationStageHistory`).
- **Configuración de etapas**: Permitir a Admins personalizar etapas del pipeline por oferta (nombre, orden, etapas finales).
- **Filtros y búsqueda**: Filtrar candidatos por etapa, fuente, scoring, fecha de aplicación.

## Dependencias

- **EPIC-001-GestionUsuarios**: Necesita usuarios autenticados.
- **EPIC-002-GestionOfertas**: El pipeline se visualiza por oferta.
- **EPIC-003-GestionCandidatos**: Las candidaturas son los elementos del pipeline.

## Estimación Preliminar

- **Complejidad**: Media-Alta
- **Esfuerzo estimado**: 3-4 semanas (incluyendo drag & drop, WebSocket/SSE, histórico y configuración de etapas).

## Criterios de Éxito

- Un recruiter puede visualizar el pipeline de una oferta en formato kanban.
- Puede mover candidatos entre etapas mediante drag & drop y los cambios se persisten correctamente.
- Múltiples usuarios pueden ver actualizaciones en tiempo casi real.
- El historial de etapas de cada candidatura es consultable y preciso.
