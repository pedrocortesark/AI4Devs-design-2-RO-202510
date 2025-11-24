# EPIC-005 — Automatización Básica

## Descripción

Esta épica implementa el **builder no-code inicial** para configurar reglas de automatización simples dentro de LTI. Los recruiters pueden definir disparadores (eventos de dominio como cambios de etapa o publicación de ofertas) y acciones (enviar email, notificar a un usuario, registrar evento) sin necesidad de programar.

El objetivo es demostrar que LTI no es solo un ATS pasivo, sino una plataforma de orquestación que automatiza tareas repetitivas.

## Valor de Negocio

- **Diferenciación inmediata**: La automatización no-code es el principal diferenciador de LTI frente a ATS tradicionales.
- **Reducción drástica de trabajo manual**: Emails de confirmación, recordatorios a managers, actualizaciones de estado se ejecutan automáticamente.
- **Accesibilidad para no técnicos**: Recruiters pueden construir y mantener flujos sin depender de IT.
- **Base para automatizaciones avanzadas**: La infraestructura de reglas (`AutomationRule`, `AutomationAction`, `AutomationEvent`) soportará flujos más complejos en fases posteriores.

## Funcionalidades Preliminares

- **Builder visual de reglas**: Interfaz drag & drop para configurar:
  - **Disparadores**: `ON_STAGE_CHANGE`, `ON_APPLICATION_CREATED`, `ON_JOB_POSTING_PUBLISHED`.
  - **Condiciones**: Filtros por etapa origen/destino, fuente de candidato, etc.
  - **Acciones**: `SEND_EMAIL`, `SEND_NOTIFICATION`, `UPDATE_STAGE` (limitado).
- **Plantillas de email**: Editor de plantillas con variables dinámicas (nombre candidato, título oferta, etc.).
- **Gestión de reglas**: Activar/desactivar reglas, duplicar, eliminar.
- **Logs de ejecución**: Vista de `AutomationEvent` para auditar qué reglas se ejecutaron, cuándo y con qué resultado (SUCCESS/FAILED).
- **Reglas predefinidas**: Plantillas de reglas comunes (confirmación de recepción de candidatura, notificación a hiring manager cuando candidato entra en etapa de entrevista).

## Dependencias

- **EPIC-001-GestionUsuarios**: Necesita usuarios y permisos.
- **EPIC-002-GestionOfertas**: Disparadores basados en publicación de ofertas.
- **EPIC-003-GestionCandidatos**: Disparadores basados en creación de candidaturas.
- **EPIC-004-PipelineVisual**: Disparadores basados en cambios de etapa.

## Estimación Preliminar

- **Complejidad**: Alta
- **Esfuerzo estimado**: 4-5 semanas (incluyendo motor de reglas, builder UI, plantillas de email, logs de ejecución y pruebas end-to-end).

## Criterios de Éxito

- Un recruiter puede crear una regla que envíe un email de confirmación a candidatos cuando aplican a una oferta.
- Una regla puede notificar a un hiring manager cuando un candidato entra en etapa de entrevista.
- Las reglas se ejecutan correctamente tras los eventos correspondientes y los logs de ejecución son visibles.
- El builder es usable por usuarios no técnicos (validado con testers internos o early adopters).
