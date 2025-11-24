# EPIC-012 — Reporting Esencial

## Descripción

Esta épica implementa dashboards de métricas clave orientados a acción, que permiten a recruiters, hiring managers y líderes de People/HR entender el estado del reclutamiento, identificar cuellos de botella y priorizar esfuerzos. Incluye métricas estándar de la industria (time-to-hire, conversión por etapa, source-of-hire) y vistas personalizables.

El objetivo es convertir LTI en una herramienta de decisión estratégica, no solo operativa.

## Valor de Negocio

- **Visibilidad estratégica**: C-level y Heads of People pueden justificar ROI de reclutamiento y optimizar inversión en canales de sourcing.
- **Detección de problemas**: Identificar rápidamente procesos atascados, etapas que tardan demasiado o fuentes de candidatos poco efectivas.
- **Benchmark interno**: Comparar performance entre equipos, ofertas o periodos.
- **Diferenciación competitiva**: Reporting accionable (no solo descriptivo) es raro en ATS de la competencia.

## Funcionalidades Preliminares

- **Métricas clave**:
  - Time-to-hire (tiempo medio desde publicación de oferta hasta decisión de contratación).
  - Time-in-stage (tiempo medio en cada etapa del pipeline).
  - Tasa de conversión por etapa.
  - Source-of-hire (efectividad de canales de sourcing).
  - Volumen de candidatos por oferta, por etapa, por periodo.
  - Tasa de rechazo por etapa.
- **Dashboards por rol**:
  - Recruiter: Métricas por oferta y por pipeline.
  - Hiring Manager: Resumen de sus ofertas activas y decisiones pendientes.
  - Admin/Head of People: Métricas agregadas de toda la empresa.
- **Filtros y segmentación**: Por oferta, por equipo, por periodo, por fuente.
- **Exportación de datos**: CSV/Excel para análisis externos.
- **Alertas automáticas**: Notificar cuando métricas críticas se desvían de umbrales (ej.: time-to-hire > X días).

## Dependencias

- **EPIC-001-GestionUsuarios**: Necesita roles para vistas diferenciadas.
- **EPIC-002-GestionOfertas**: Métricas basadas en ofertas.
- **EPIC-003-GestionCandidatos**: Métricas de candidaturas.
- **EPIC-004-PipelineVisual**: Métricas de tiempo en etapas.
- **EPIC-008-DecisionesFinales**: Métricas de decisiones y conversión.

## Estimación Preliminar

- **Complejidad**: Media-Alta
- **Esfuerzo estimado**: 4-5 semanas (incluyendo cálculo de métricas, dashboards, filtros, exportación y alertas).

## Criterios de Éxito

- Un recruiter puede ver time-to-hire y conversión por etapa de una oferta específica.
- Un Head of People puede ver métricas agregadas de todas las ofertas de la empresa.
- Las métricas se actualizan en tiempo casi real (con cache de minutos, no días).
- Los filtros permiten análisis granular (por periodo, por equipo, por fuente).
- Las exportaciones funcionan correctamente y los datos son precisos.
