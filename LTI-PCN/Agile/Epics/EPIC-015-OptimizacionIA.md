# EPIC-015 — Optimización de IA

## Descripción

Esta épica mejora las capacidades de IA operativa de LTI, añadiendo priorización predictiva de candidatos, detección de riesgos de caída en pipeline, sugerencias de próximos pasos más sofisticadas y análisis de sesgos en evaluación. Incluye feedback loop para entrenar modelos con datos históricos de la empresa.

El objetivo es convertir la IA de LTI en un verdadero copiloto predictivo, no solo descriptivo.

## Valor de Negocio

- **Mejora de calidad de contratación**: Priorización predictiva aumenta probabilidad de contratar candidatos que tendrán éxito.
- **Reducción de caída de candidatos**: Detección temprana de riesgos permite intervención antes de perder candidatos valiosos.
- **Fairness y compliance**: Análisis de sesgos ayuda a empresas a cumplir con políticas de DEI (Diversity, Equity, Inclusion).
- **Diferenciación competitiva sostenible**: IA predictiva y personalizada por empresa es difícil de replicar.

## Funcionalidades Preliminares

- **Priorización predictiva**: IA predice probabilidad de éxito de candidatos (probabilidad de avanzar en pipeline, fit cultural, performance futura).
- **Detección de riesgos**: IA alerta cuando candidato está en riesgo de abandonar proceso (falta de comunicación, tiempo excesivo en etapa).
- **Sugerencias de próximos pasos avanzadas**: IA propone acciones específicas ("Programa entrevista técnica antes de viernes", "Solicita referencias", "Envía oferta competitiva").
- **Análisis de sesgos**: IA detecta patrones de sesgo en evaluación (ej.: candidatos de ciertos perfiles sistemáticamente mejor/peor valorados sin justificación objetiva).
- **Feedback loop**: Modelos de IA aprenden de decisiones históricas de la empresa (qué candidatos fueron contratados, cuál fue su performance).
- **Personalización por empresa**: Modelos de IA se ajustan a criterios específicos de cada empresa (no solo genéricos).

## Dependencias

- **EPIC-006-IAOperativaInicial**: Infraestructura base de IA.
- **EPIC-007-FeedbackColaborativo**: Feedback histórico alimenta modelos.
- **EPIC-008-DecisionesFinales**: Decisiones históricas alimentan modelos.
- **EPIC-012-ReportingEsencial**: Métricas de precisión de IA.

## Estimación Preliminar

- **Complejidad**: Alta
- **Esfuerzo estimado**: 5-6 semanas (incluyendo desarrollo de modelos predictivos, feedback loop, análisis de sesgos y validación de precisión).

## Criterios de Éxito

- La priorización predictiva aumenta tasa de conversión a contratación en X% (validado con A/B tests).
- La detección de riesgos reduce caída de candidatos en Y% (validado con early adopters).
- Las sugerencias de próximos pasos son útiles y accionables (validado con recruiters).
- El análisis de sesgos identifica patrones reales (validado con auditorías internas de empresas).
- Los modelos mejoran con el tiempo (métricas de precisión aumentan en iteraciones posteriores).
