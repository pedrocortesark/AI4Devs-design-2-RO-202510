# US-003-03 — Vista de Perfil de Candidato

## Narrativa

**Como** recruiter o hiring manager  
**Quiero** visualizar el perfil completo de un candidato  
**Para** revisar su información, CV, historial de candidaturas y tomar decisiones informadas

## Descripción

Esta historia implementa la vista detallada del perfil de candidato, mostrando información personal, CV descargable, histórico de todas sus aplicaciones y su estado actual en cada proceso. Sirve como hub de información para evaluar candidatos en múltiples contextos.

## Criterios de Aceptación

```gherkin
Escenario 1: Ver perfil completo de candidato
  Dado que existe un candidato "Laura Martínez" con aplicaciones a 2 ofertas
  Cuando un recruiter hace clic en su nombre desde cualquier vista
  Entonces se abre la página de perfil mostrando:
    - Nombre, email, teléfono, ubicación
    - Fuente de origen (Referral)
    - Link de descarga del CV (URL firmada de S3)
    - LinkedIn URL (si existe)
    - Historial de aplicaciones (ofertas, etapa actual, fecha de aplicación)

Escenario 2: Descarga de CV
  Dado que el candidato tiene un CV almacenado en S3
  Cuando el usuario hace clic en "Descargar CV"
  Entonces se genera una URL firmada temporal (válida 1 hora)
  Y se descarga el archivo CV en el navegador

Escenario 3: Historial de aplicaciones visible
  Dado que Laura tiene aplicaciones a "Senior Backend" (etapa Interview) y "Tech Lead" (etapa Applied)
  Cuando se visualiza su perfil
  Entonces se muestra una tabla con:
    | Oferta          | Etapa Actual | Fecha Aplicación | Estado  |
    | Senior Backend  | Interview    | 2025-11-10       | ACTIVE  |
    | Tech Lead       | Applied      | 2025-11-20       | ACTIVE  |
  Y cada fila enlaza al pipeline correspondiente

Escenario 4: Permisos de visualización
  Dado que un Hiring Manager solo tiene permisos sobre sus ofertas asignadas
  Cuando visualiza el perfil de un candidato
  Entonces solo ve aplicaciones a ofertas donde es hiring_manager
  Y no ve aplicaciones a otras ofertas de la empresa
```

## Notas Técnicas

- **Permisos**: Recruiters y Admins ven todo; Hiring Managers solo ven candidatos de sus ofertas.
- **URLs firmadas**: Regenerar URL de S3 cada vez que se accede al perfil (expiración 1 hora).
- **Histórico**: Query a `Application` con join a `JobPosting` y `PipelineStage` para mostrar estado actual.

## Tareas

- [ ] Diseñar página de perfil de candidato en Frontend
- [ ] Implementar endpoint `GET /api/candidates/:id` en Backend
- [ ] Implementar lógica de permisos (Recruiter ve todo, Hiring Manager solo sus ofertas)
- [ ] Implementar generación de URL firmada de S3 para CV
- [ ] Consultar historial de aplicaciones con joins a JobPosting y PipelineStage
- [ ] Crear componente de tabla de historial de aplicaciones
- [ ] Implementar botón de descarga de CV con URL firmada
- [ ] Añadir enlaces a perfiles de LinkedIn (si existen)
- [ ] Crear tests unitarios de lógica de permisos
- [ ] Crear tests de integración de carga de perfil completo
- [ ] Documentar endpoint en OpenAPI/Swagger

## Estimación

**Story Points:** 3  
**Valor de Negocio:** 4 (Importante para evaluación)  
**Urgencia:** 4 (Necesario para flujo completo)

## Dependencias

- **US-003-01-RegistroCandidato**: Debe existir candidato
- **US-003-02-AplicacionCandidato**: Para mostrar historial de aplicaciones

## Prioridad

**Media-Alta** — Mejora la experiencia de evaluación
