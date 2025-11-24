# EPIC-008 — Decisiones Finales

## Descripción

Esta épica implementa el registro explícito de decisiones finales sobre candidatos (`Decision`). Incluye flujo de decisión (Enviar oferta, Rechazar, Seguir en proceso), trazabilidad de quién decide y cuándo, y automatizaciones asociadas a cada tipo de decisión (emails a candidatos, notificaciones internas, cambios de estado).

El objetivo es formalizar las decisiones de contratación y asegurar que todas las partes (recruiter, manager, candidato) sean informadas correctamente.

## Valor de Negocio

- **Transparencia y trazabilidad**: Cada decisión queda registrada con responsable, fecha y motivo.
- **Automatización de comunicaciones**: Las decisiones disparan acciones automáticas (emails de rechazo, ofertas formales).
- **Reducción de errores**: Decisiones explícitas evitan candidatos "olvidados" o comunicaciones inconsistentes.
- **Métricas precisas**: Tasa de conversión a oferta, tiempo hasta decisión, motivos de rechazo.

## Funcionalidades Preliminares

- **Tipos de decisión**: `OFFER` (enviar oferta), `REJECT` (rechazar), `ADVANCE` (pasar a siguiente etapa), `HOLD` (pausar proceso).
- **Registro de decisión**: Formulario donde hiring manager (o recruiter con permisos) selecciona decisión, añade notas y confirma.
- **Automatizaciones por decisión**: Reglas que se disparan según tipo de decisión (email de oferta, email de rechazo, notificación a equipo).
- **Vista de decisiones**: Historial de decisiones por candidato y por oferta.
- **Aprobaciones multinivel (opcional MVP)**: En empresas que requieran aprobación de más de un manager antes de enviar oferta.
- **Integración con IA**: IA puede sugerir decisión basada en feedback y scoring (recomendación, no decisión automática).

## Dependencias

- **EPIC-001-GestionUsuarios**: Necesita usuarios autenticados y roles.
- **EPIC-003-GestionCandidatos**: Decisiones se asocian a candidaturas.
- **EPIC-004-PipelineVisual**: Decisiones afectan estado del pipeline.
- **EPIC-005-AutomatizacionBasica**: Automatizaciones disparadas por decisiones.
- **EPIC-006-IAOperativaInicial**: IA puede sugerir decisión.
- **EPIC-007-FeedbackColaborativo**: Decisión se toma tras revisar feedback.

## Estimación Preliminar

- **Complejidad**: Media
- **Esfuerzo estimado**: 2-3 semanas (incluyendo flujo de decisión, automatizaciones asociadas y vistas de historial).

## Criterios de Éxito

- Un hiring manager puede registrar una decisión final sobre un candidato.
- La decisión dispara automatizaciones configuradas (email al candidato, notificación al recruiter).
- El historial de decisiones es consultable y trazable.
- La IA puede sugerir una decisión que el manager puede aceptar o modificar.
