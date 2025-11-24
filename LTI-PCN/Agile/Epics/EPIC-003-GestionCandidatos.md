# EPIC-003 — Gestión de Candidatos

## Descripción

Esta épica implementa la gestión de candidatos (`Candidate`) y candidaturas (`Application`) dentro de LTI. Incluye el registro de candidatos (manual o vía aplicación externa), la vinculación de candidaturas a ofertas, la gestión de perfiles de candidato y la trazabilidad de fuentes de candidatos (job boards, referencias, aplicación directa).

El objetivo es construir el dominio central del ATS: candidatos y sus aplicaciones a ofertas.

## Valor de Negocio

- **Base del pipeline de reclutamiento**: Sin candidatos y candidaturas no hay proceso de selección.
- **Reutilización de talento**: Candidatos pueden aplicar a múltiples ofertas; su perfil queda registrado para futuras oportunidades.
- **Trazabilidad de fuentes**: Saber de dónde vienen los candidatos permite optimizar estrategias de sourcing.
- **Preparación para IA**: Los perfiles de candidato (CV, LinkedIn, datos de contacto) alimentan análisis de IA.

## Funcionalidades Preliminares

- **Registro de candidato**: Crear perfil con nombre, email, teléfono, ubicación, CV (upload a S3), URL de LinkedIn.
- **Aplicación a oferta**: Vincular un candidato a una oferta creando una `Application`.
- **Fuentes de candidatos**: Registrar fuente (job board, referral, aplicación directa, agencia).
- **Vista de perfil de candidato**: Mostrar datos del candidato, historial de candidaturas y entrevistas (preliminar).
- **Búsqueda de candidatos**: Filtrar por nombre, email, fuente, estado de candidatura.
- **Detección de duplicados**: Evitar registrar el mismo candidato múltiples veces (matching por email).
- **Estados de candidatura**: ACTIVE, WITHDRAWN, HIRED, REJECTED (vinculado a `Application.status`).

## Dependencias

- **EPIC-001-GestionUsuarios**: Necesita usuarios autenticados.
- **EPIC-002-GestionOfertas**: Las candidaturas se vinculan a ofertas publicadas.

## Estimación Preliminar

- **Complejidad**: Media
- **Esfuerzo estimado**: 2-3 semanas (incluyendo upload de CVs a S3 y detección de duplicados).

## Criterios de Éxito

- Un candidato puede ser registrado (manual o automáticamente) y aplicar a una oferta.
- El perfil del candidato muestra su CV, datos de contacto y fuente.
- No se crean candidatos duplicados para el mismo email.
- Las candidaturas quedan correctamente vinculadas a ofertas y listas para gestionarse en el pipeline.
