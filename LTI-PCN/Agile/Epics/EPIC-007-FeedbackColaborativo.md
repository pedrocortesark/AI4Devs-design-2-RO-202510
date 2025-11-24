# EPIC-007 — Feedback Colaborativo

## Descripción

Esta épica implementa el sistema de feedback estructurado entre recruiters y hiring managers sobre candidatos. Incluye scorecards personalizables, comentarios por entrevista, y la capacidad de agregar feedback de múltiples evaluadores antes de tomar decisiones finales.

El objetivo es facilitar la colaboración entre recruiters y managers, asegurando que las evaluaciones sean consistentes, trazables y útiles para decisiones.

## Valor de Negocio

- **Mejora de calidad de decisiones**: Feedback estructurado reduce subjetividad y sesgo.
- **Colaboración efectiva**: Managers pueden dejar feedback sin abandonar su flujo de trabajo (integración con Slack/email).
- **Trazabilidad**: Todo el feedback queda registrado para auditorías, métricas y aprendizaje organizacional.
- **Reducción de fricción**: Interfaces simples y recordatorios automáticos aceleran la recolección de feedback.

## Funcionalidades Preliminares

- **Scorecards personalizables**: Admins pueden definir criterios de evaluación por oferta (skills técnicas, fit cultural, experiencia).
- **Feedback por entrevista**: Registrar comentarios y puntuaciones tras cada entrevista.
- **Agregación de feedback**: Vista consolidada de todo el feedback de un candidato.
- **Recordatorios automáticos**: Notificar a managers cuando deben dejar feedback (vía regla de automatización).
- **Feedback privado y compartido**: Permitir comentarios visibles solo para el recruiter o compartidos con todo el equipo.
- **Historial de feedback**: Ver evolución de feedback a lo largo del proceso.

## Dependencias

- **EPIC-001-GestionUsuarios**: Necesita usuarios autenticados y roles.
- **EPIC-003-GestionCandidatos**: Feedback se asocia a candidaturas.
- **EPIC-004-PipelineVisual**: Feedback se visualiza en el pipeline.
- **EPIC-005-AutomatizacionBasica**: Recordatorios automáticos de feedback.

## Estimación Preliminar

- **Complejidad**: Media-Alta
- **Esfuerzo estimado**: 3-4 semanas (incluyendo scorecards, UI de feedback, recordatorios y agregación).

## Criterios de Éxito

- Un hiring manager puede dejar feedback estructurado tras una entrevista.
- El recruiter ve todo el feedback agregado de un candidato en una vista consolidada.
- Los recordatorios automáticos se disparan cuando un manager no ha dejado feedback en X días.
- Los scorecards son configurables por oferta y los evaluadores pueden rellenarlos fácilmente.
