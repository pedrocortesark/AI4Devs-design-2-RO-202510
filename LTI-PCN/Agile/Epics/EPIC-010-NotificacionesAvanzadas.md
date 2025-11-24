# EPIC-010 — Notificaciones Avanzadas

## Descripción

Esta épica implementa integraciones nativas con Slack y Microsoft Teams para enviar notificaciones en tiempo real y permitir acciones rápidas sin abandonar estas plataformas. Incluye resúmenes de candidatos, alertas de decisiones pendientes y la posibilidad de aprobar/rechazar candidatos desde Slack/Teams.

El objetivo es convertir LTI en parte del flujo de trabajo real de managers y recruiters, que pasan su día en Slack/Teams, no en el ATS.

## Valor de Negocio

- **Reducción de fricción**: Managers pueden revisar candidatos y dejar feedback sin cambiar de herramienta.
- **Velocidad de respuesta**: Notificaciones en tiempo real aceleran decisiones.
- **Adopción de producto**: Integraciones con herramientas diarias aumentan engagement y sticky de LTI.
- **Diferenciación competitiva**: Pocos ATS ofrecen integraciones profundas con Slack/Teams (más allá de notificaciones básicas).

## Funcionalidades Preliminares

- **Integración con Slack**: App de Slack que envía notificaciones y permite acciones (aprobar candidato, solicitar más info, ver perfil).
- **Integración con Teams**: Connector de Teams con funcionalidades equivalentes.
- **Tipos de notificación**:
  - Nuevo candidato en etapa de revisión.
  - Decisión pendiente de manager.
  - Cambio de etapa crítico.
  - Recordatorio de feedback pendiente.
- **Acciones rápidas desde Slack/Teams**: Botones para mover candidato de etapa, aprobar/rechazar, ver perfil completo en LTI.
- **Resúmenes diarios/semanales**: Digest de candidatos pendientes, decisiones tomadas, métricas clave.
- **Configuración de preferencias**: Usuarios eligen qué notificaciones reciben y en qué canal (Slack, Teams, email, in-app).

## Dependencias

- **EPIC-001-GestionUsuarios**: Necesita usuarios autenticados.
- **EPIC-004-PipelineVisual**: Notificaciones basadas en cambios de pipeline.
- **EPIC-005-AutomatizacionBasica**: Reglas de automatización disparan notificaciones.
- **EPIC-007-FeedbackColaborativo**: Recordatorios de feedback.
- **EPIC-008-DecisionesFinales**: Notificaciones de decisiones pendientes.

## Estimación Preliminar

- **Complejidad**: Media-Alta
- **Esfuerzo estimado**: 3-4 semanas (incluyendo OAuth de Slack/Teams, manejo de webhooks, acciones interactivas y configuración de preferencias).

## Criterios de Éxito

- Un hiring manager recibe notificación en Slack cuando un candidato entra en su etapa de revisión.
- Puede hacer clic en "Ver perfil" y se abre una vista resumida del candidato.
- Puede hacer clic en "Aprobar" o "Rechazar" y la acción se registra en LTI.
- Los usuarios pueden configurar sus preferencias de notificación.
- Las integraciones son estables y manejan errores de forma elegante (si Slack está caído, LTI no falla).
