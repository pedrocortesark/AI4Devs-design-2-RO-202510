# EPIC-009 — Candidate Experience

## Descripción

Esta épica implementa el portal para candidatos y las comunicaciones personalizadas que mejoran la transparencia y experiencia de los candidatos durante el proceso de selección. Incluye actualizaciones de estado automáticas, mensajes personalizados generados por IA y feedback post-rechazo.

El objetivo es convertir el "agujero negro" del proceso de selección en una experiencia transparente y respetuosa que mejore el employer branding.

## Valor de Negocio

- **Employer branding**: Candidatos hablan bien de empresas que comunican y respetan su tiempo, incluso si son rechazados.
- **Reducción de consultas**: Candidatos que ven su estado en tiempo real no envían emails preguntando "¿qué pasó con mi candidatura?".
- **Diferenciación competitiva**: Pocos ATS ofrecen candidate experience realmente buena; LTI puede destacar aquí.
- **Preparación para futuro**: Portal de candidatos puede evolucionar a talent community y nurturing de candidatos.

## Funcionalidades Preliminares

- **Portal de candidato**: Página donde candidatos ven estado de su candidatura (etapa actual, fecha de última actualización).
- **Notificaciones automáticas**: Email al candidato cuando:
  - Se recibe su candidatura.
  - Cambia de etapa.
  - Se toma decisión final (oferta o rechazo).
- **Mensajes personalizados con IA**: IA genera emails más humanos y personalizados según etapa y contexto.
- **Feedback post-rechazo**: Opción de enviar feedback constructivo a candidatos rechazados (opcional, configurable por empresa).
- **Self-service**: Candidatos pueden actualizar su perfil, adjuntar documentos adicionales o retirar su candidatura.
- **Encuesta de experiencia**: Solicitar feedback de candidatos sobre el proceso (NPS de candidate experience).

## Dependencias

- **EPIC-001-GestionUsuarios**: Candidatos tienen acceso limitado (sin login, vía magic link o token).
- **EPIC-003-GestionCandidatos**: Portal muestra datos de candidatura.
- **EPIC-004-PipelineVisual**: Cambios de etapa disparan notificaciones.
- **EPIC-005-AutomatizacionBasica**: Automatizaciones de envío de emails.
- **EPIC-006-IAOperativaInicial**: IA personaliza mensajes.
- **EPIC-008-DecisionesFinales**: Decisiones disparan notificaciones finales.

## Estimación Preliminar

- **Complejidad**: Media
- **Esfuerzo estimado**: 3-4 semanas (incluyendo portal, generación de mensajes con IA, notificaciones y encuestas).

## Criterios de Éxito

- Un candidato recibe un email de confirmación tras aplicar.
- Puede acceder a un portal (vía link) donde ve el estado de su candidatura.
- Recibe notificaciones automáticas cuando cambia de etapa o se toma decisión final.
- Los mensajes son personalizados y suenan humanos (validado por testers).
- Candidatos rechazados reciben feedback constructivo (si la empresa lo habilita).
