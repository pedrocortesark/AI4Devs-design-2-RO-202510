# EPIC-014 — Movilidad Interna

## Descripción

Esta épica implementa la integración con HRIS (sistemas de gestión de empleados) para identificar talento interno apto para nuevas vacantes y facilitar la movilidad interna. Incluye matching de empleados actuales con ofertas abiertas, flujos de aplicación interna y visibilidad para managers de talento disponible en su organización.

El objetivo es completar el concepto de "Talent Orchestration Platform", unificando talento externo e interno.

## Valor de Negocio

- **Retención de talento**: Facilitar movilidad interna reduce churn y mejora employee engagement.
- **Reducción de costes**: Contratar internamente es mucho más barato que contratar externamente.
- **Velocidad de contratación**: Empleados internos conocen la empresa, reduciendo time-to-productivity.
- **Diferenciación competitiva**: La integración profunda entre ATS y HRIS es rara; LTI la ofrece desde MVP extendido.

## Funcionalidades Preliminares

- **Integración con HRIS**: Conectores a BambooHR, Personio, Workday, etc. para sincronizar datos de empleados (skills, roles, ubicación, performance).
- **Matching interno-externo**: IA sugiere empleados actuales que matchean con ofertas abiertas.
- **Aplicación interna**: Empleados pueden aplicar internamente a ofertas (con aprobación de su manager actual si aplica).
- **Vista de talento interno**: Recruiters y hiring managers ven candidatos internos junto a externos en el pipeline.
- **Flujo de aprobación**: Si un empleado aplica a otra posición, su manager actual es notificado y puede aprobar o bloquear (según política de empresa).
- **Métricas de movilidad interna**: % de vacantes cubiertas internamente, tiempo de movilidad, retención post-movimiento.

## Dependencias

- **EPIC-001-GestionUsuarios**: Empleados son también usuarios de LTI.
- **EPIC-002-GestionOfertas**: Ofertas pueden ser internas o externas.
- **EPIC-003-GestionCandidatos**: Empleados internos aparecen como candidatos.
- **EPIC-006-IAOperativaInicial**: IA para matching de talento interno.
- **EPIC-012-ReportingEsencial**: Métricas de movilidad interna.

## Estimación Preliminar

- **Complejidad**: Alta
- **Esfuerzo estimado**: 5-6 semanas (incluyendo conectores HRIS, lógica de matching, flujos de aprobación y métricas).

## Criterios de Éxito

- LTI sincroniza datos de empleados desde HRIS.
- Al abrir una oferta, el recruiter ve sugerencias de empleados internos que matchean.
- Un empleado puede aplicar internamente y su manager actual es notificado.
- Las métricas de movilidad interna son visibles en dashboards.
- La integración es configurable (empresas pueden habilitar/deshabilitar movilidad interna).
