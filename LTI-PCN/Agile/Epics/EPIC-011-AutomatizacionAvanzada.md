# EPIC-011 — Automatización Avanzada

## Descripción

Esta épica amplía el motor de automatización no-code con condiciones complejas, acciones múltiples encadenadas, y conectores a herramientas externas (calendarios, videoconferencia, evaluaciones técnicas, CRMs). Incluye templates de flujos predefinidos y la posibilidad de programar acciones con delays y condiciones lógicas (AND, OR).

El objetivo es convertir el motor de automatización de LTI en una herramienta potente comparable a Zapier/Make, pero embebida en el ATS.

## Valor de Negocio

- **Reducción masiva de trabajo manual**: Flujos complejos (recordatorios escalonados, coordinación de entrevistas, envío de pruebas técnicas) se automatizan completamente.
- **Flexibilidad extrema**: Empresas pueden adaptar LTI a sus procesos únicos sin necesidad de desarrollo custom.
- **Ventaja competitiva sostenible**: La profundidad de automatización de LTI será difícil de replicar por competidores legacy.
- **Monetización**: Automatizaciones avanzadas pueden ser parte de tiers superiores de pricing.

## Funcionalidades Preliminares

- **Condiciones lógicas avanzadas**: AND, OR, NOT en reglas de automatización.
- **Acciones múltiples encadenadas**: Una regla puede ejecutar varias acciones en secuencia.
- **Delays programados**: "Esperar X días antes de ejecutar acción Y".
- **Conectores externos**:
  - Google Calendar / Outlook Calendar (crear eventos de entrevista).
  - Zoom / Google Meet (generar links de videoconferencia).
  - Herramientas de evaluación técnica (HackerRank, Codility, etc.).
  - CRMs (Salesforce, HubSpot) para sincronizar candidatos/contactos.
- **Templates de flujos**: Biblioteca de flujos predefinidos (onboarding de candidato, seguimiento de silver medalists, campañas de reactivación).
- **Testing de reglas**: Modo "dry run" para probar reglas sin ejecutar acciones reales.
- **Versioning de reglas**: Historial de cambios en reglas para auditoría y rollback.

## Dependencias

- **EPIC-005-AutomatizacionBasica**: Infraestructura base de automatización.
- **EPIC-008-DecisionesFinales**: Automatizaciones disparadas por decisiones.
- **EPIC-010-NotificacionesAvanzadas**: Acciones de notificación avanzadas.

## Estimación Preliminar

- **Complejidad**: Alta
- **Esfuerzo estimado**: 5-6 semanas (incluyendo conectores externos, lógica avanzada, templates, testing y versioning).

## Criterios de Éxito

- Un recruiter puede configurar un flujo que:
  1. Cuando un candidato entra en etapa de entrevista técnica.
  2. Espera 1 día.
  3. Envía email con link de prueba técnica.
  4. Crea evento en Google Calendar para revisión de resultados.
  5. Notifica a hiring manager en Slack cuando la prueba esté completada.
- Los conectores externos funcionan de forma confiable (autenticación OAuth, manejo de errores).
- Los templates de flujos son usables por usuarios no técnicos (validado con early adopters).
- El modo "dry run" permite probar reglas sin efectos reales.
