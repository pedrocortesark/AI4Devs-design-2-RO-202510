# EPIC-016 — Integraciones Extendidas

## Descripción

Esta épica implementa conectores adicionales con job boards (LinkedIn, Indeed, etc.), herramientas de evaluación técnica (HackerRank, Codility), CRMs de reclutamiento (Gem, Ashby) y APIs abiertas para clientes enterprise que necesitan integraciones custom. Incluye marketplace de integraciones y documentación de APIs.

El objetivo es convertir LTI en una plataforma abierta y flexible que se integra con el stack completo de herramientas de reclutamiento.

## Valor de Negocio

- **Reducción de trabajo manual**: Sincronización automática de candidatos desde job boards, resultados de pruebas técnicas, etc.
- **Apertura a enterprise**: Clientes enterprise necesitan integraciones custom; APIs abiertas habilitan este segmento.
- **Ecosistema y partners**: Marketplace de integraciones atrae partners y amplifica value proposition de LTI.
- **Diferenciación competitiva**: Flexibilidad de integración es crítica para empresas con stacks complejos.

## Funcionalidades Preliminares

- **Conectores con job boards**:
  - LinkedIn Jobs: Publicar ofertas, sincronizar candidatos.
  - Indeed: Publicar ofertas, sincronizar candidatos.
  - Otros (configurables según mercado).
- **Conectores con herramientas de evaluación técnica**:
  - HackerRank, Codility, CodeSignal: Enviar invitaciones, sincronizar resultados.
- **Conectores con CRMs de reclutamiento**:
  - Gem, Ashby: Sincronizar candidatos, secuencias de outreach.
- **APIs abiertas para clientes**:
  - REST APIs documentadas (OpenAPI/Swagger).
  - Webhooks para eventos de dominio.
  - SDKs en lenguajes populares (JavaScript, Python).
- **Marketplace de integraciones**: Catálogo de conectores disponibles, instalación con un clic, configuración guiada.
- **Gestión de credenciales**: OAuth flows seguros para cada integración.

## Dependencias

- **EPIC-002-GestionOfertas**: Publicación de ofertas en job boards.
- **EPIC-003-GestionCandidatos**: Sincronización de candidatos.
- **EPIC-005-AutomatizacionBasica**: Automatizaciones basadas en eventos de integraciones.
- **EPIC-011-AutomatizacionAvanzada**: Conectores externos usados en flujos avanzados.

## Estimación Preliminar

- **Complejidad**: Alta
- **Esfuerzo estimado**: 6-8 semanas (incluyendo múltiples conectores, APIs abiertas, marketplace, documentación y pruebas de integraciones).

## Criterios de Éxito

- LTI puede publicar ofertas en LinkedIn y Indeed con un clic.
- Los candidatos que aplican en LinkedIn se sincronizan automáticamente en LTI.
- Las pruebas técnicas de HackerRank se envían desde LTI y los resultados se sincronizan.
- Las APIs abiertas están documentadas (OpenAPI) y funcionales.
- Clientes enterprise pueden construir integraciones custom usando APIs de LTI.
- El marketplace de integraciones es usable y los conectores se instalan/configuran sin fricción.
