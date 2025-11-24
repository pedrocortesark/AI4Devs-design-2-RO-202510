# EPIC-006 — IA Operativa Inicial

## Descripción

Esta épica implementa la integración de IA operativa en el flujo de trabajo de LTI desde el MVP. Incluye generación de mejoras de descripciones de ofertas, resúmenes de CVs, scoring básico de candidatos por etapa y sugerencias de próximos pasos.

El objetivo es demostrar que la IA en LTI no es un adorno, sino un copiloto que asiste en decisiones operativas diarias.

## Valor de Negocio

- **Diferenciación competitiva**: La IA embebida en el flujo (no como feature aislada) es un diferenciador clave frente a ATS tradicionales.
- **Reducción de carga cognitiva**: Recruiters reciben resúmenes y recomendaciones que aceleran screening y priorización.
- **Mejora de calidad**: Ofertas mejor redactadas y candidatos mejor evaluados aumentan tasas de conversión.
- **Base para evolución**: La infraestructura de IA (`AIInsight`) soportará análisis más sofisticados en fases posteriores (predicción de riesgos, detección de sesgos).

## Funcionalidades Preliminares

- **Mejora de descripciones de ofertas**: IA reescribe y mejora textos de ofertas a partir de inputs básicos del recruiter.
- **Resumen de CVs**: IA extrae puntos clave de un CV (experiencia relevante, skills, formación) y los presenta en formato estructurado.
- **Scoring básico de candidatos**: IA asigna un score (1-100) a cada candidato por etapa según match con requisitos de la oferta.
- **Sugerencias de próximos pasos**: IA propone acciones ("Mover a entrevista técnica", "Solicitar más información", "Rechazar").
- **Almacenamiento de insights**: Resultados de IA se guardan en `AIInsight` para auditoría y mejora continua.
- **Configuración de proveedores**: Integración con OpenAI (GPT-4) o Claude; selección de modelo según caso de uso.
- **Control de costes**: Límites de uso de IA por empresa y alertas cuando se alcancen umbrales.

## Dependencias

- **EPIC-001-GestionUsuarios**: Necesita usuarios autenticados.
- **EPIC-002-GestionOfertas**: IA mejora descripciones de ofertas.
- **EPIC-003-GestionCandidatos**: IA analiza CVs y candidaturas.
- **EPIC-004-PipelineVisual**: Scoring de IA se visualiza en el pipeline.

## Estimación Preliminar

- **Complejidad**: Alta
- **Esfuerzo estimado**: 4-5 semanas (incluyendo integración con proveedores de IA, parsing de CVs, persistencia de insights, control de costes y pruebas de calidad de outputs).

## Criterios de Éxito

- Un recruiter puede solicitar mejora de una descripción de oferta y recibir una versión mejorada en segundos.
- Un recruiter ve resúmenes de CVs generados por IA al abrir el perfil de un candidato.
- El pipeline muestra scoring de IA para cada candidato y permite ordenar por score.
- Los insights de IA quedan registrados en `AIInsight` y son auditables.
- El sistema controla el uso de IA y alerta cuando se alcancen límites configurados.
