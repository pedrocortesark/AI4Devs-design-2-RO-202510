# EPIC-013 — Reutilización de Talento

## Descripción

Esta épica implementa el sistema de recomendación de candidatos previos para nuevas ofertas. Incluye identificación de "silver medalists" (candidatos que llegaron lejos en procesos anteriores pero no fueron contratados), talent pools y sugerencias automáticas basadas en skills y experiencia.

El objetivo es activar el concepto de LTI como "Talent Orchestration Platform", no solo como ATS de procesos aislados.

## Valor de Negocio

- **Reducción de costes de sourcing**: Reutilizar candidatos previos es mucho más barato que reclutar desde cero.
- **Velocidad de contratación**: Candidatos conocidos pueden entrar en etapas avanzadas de pipeline directamente.
- **Mejora de candidate experience**: Candidatos aprecian ser recordados y considerados para nuevas oportunidades.
- **Diferenciación competitiva**: Pocos ATS ofrecen reutilización de talento de forma inteligente; LTI lo hace con IA.

## Funcionalidades Preliminares

- **Identificación de silver medalists**: IA identifica candidatos que llegaron a etapas finales en procesos anteriores y quedaron en "hold" o "no fit en esta ocasión".
- **Talent pools**: Recruiters pueden crear pools de candidatos por skills, ubicación, etc.
- **Sugerencias automáticas**: Al crear una nueva oferta, LTI sugiere candidatos previos que matchean con requisitos.
- **Campañas de reactivación**: Automatización que envía mensajes personalizados a candidatos previos cuando surge oferta relevante.
- **Scoring de reutilización**: IA evalúa probabilidad de que candidato anterior acepte nueva oferta (basado en feedback previo, tiempo transcurrido, match con nueva oferta).
- **Vista de candidatos reutilizables**: Filtro en pipeline que muestra candidatos internos/previos para esa oferta.

## Dependencias

- **EPIC-003-GestionCandidatos**: Base de candidatos históricos.
- **EPIC-006-IAOperativaInicial**: IA para scoring y matching.
- **EPIC-008-DecisionesFinales**: Histórico de decisiones.
- **EPIC-011-AutomatizacionAvanzada**: Campañas de reactivación automatizadas.

## Estimación Preliminar

- **Complejidad**: Media-Alta
- **Esfuerzo estimado**: 4-5 semanas (incluyendo identificación de silver medalists, talent pools, sugerencias automáticas, scoring y campañas).

## Criterios de Éxito

- Al crear una nueva oferta, el recruiter ve sugerencias de candidatos previos relevantes.
- Puede crear talent pools y añadir candidatos manualmente o mediante filtros automáticos.
- Las campañas de reactivación envían mensajes personalizados a candidatos previos cuando surge oferta relevante.
- El scoring de reutilización es preciso (validado con early adopters).
- Los candidatos reutilizados entran en pipeline más rápido que candidatos nuevos.
