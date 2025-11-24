# US-003-01 — Registro Manual de Candidato

## Narrativa

**Como** recruiter  
**Quiero** registrar manualmente un candidato en el sistema  
**Para** crear su perfil y vincularlo a ofertas relevantes

## Descripción

Esta historia permite a recruiters crear perfiles de candidatos manualmente, capturando información básica (nombre, email, teléfono, ubicación), cargando CV a S3 y registrando la fuente del candidato (referral, networking, sourcing directo). El sistema debe validar unicidad de email para evitar duplicados.

## Criterios de Aceptación

```gherkin
Escenario 1: Crear candidato con datos básicos
  Dado que un recruiter está autenticado en LTI
  Cuando accede a "Candidatos" y hace clic en "Añadir candidato"
  Y completa el formulario con:
    | Campo     | Valor                        |
    | Nombre    | Laura Martínez               |
    | Email     | laura.martinez@example.com   |
    | Teléfono  | +34 600 123 456              |
    | Ubicación | Barcelona, España            |
    | Fuente    | Referral                     |
  Y carga un archivo CV (PDF, max 5MB)
  Y hace clic en "Guardar"
  Entonces se crea un registro en Candidate
  Y el CV se sube a S3 y la URL se guarda en resume_url
  Y el recruiter ve confirmación "Candidato creado correctamente"

Escenario 2: Validación de email único
  Dado que existe un candidato con email "laura.martinez@example.com"
  Cuando un recruiter intenta crear otro candidato con el mismo email
  Entonces el sistema muestra error "Ya existe un candidato con este email"
  Y sugiere ver el perfil existente
  Y no se crea ningún registro duplicado

Escenario 3: Upload de CV a S3
  Dado que un recruiter está creando un candidato
  Cuando sube un CV en formato PDF de 2MB
  Entonces el archivo se sube a S3 con ruta "candidates/{candidate_id}/cv.pdf"
  Y se genera una URL firmada temporal para visualización
  Y la URL se almacena en el campo resume_url

Escenario 4: Validación de formato de CV
  Dado que un recruiter intenta subir un archivo de 8MB
  Cuando hace clic en "Guardar"
  Entonces el sistema muestra error "El archivo excede el tamaño máximo de 5MB"
  Y no se completa el registro del candidato
```

## Notas Técnicas

- **Modelo de datos**: Registro en `Candidate` con `id`, `full_name`, `email`, `phone`, `location`, `resume_url`, `source`, `created_at`.
- **Upload S3**: Usar SDK de AWS S3 con bucket configurado, generar URLs firmadas con expiración de 1 hora.
- **Validación unicidad**: Query a BD para verificar que no existe otro candidato con el mismo `email` en la misma `company_id`.
- **Formatos CV**: Soportar PDF, DOCX (validar MIME type).

## Tareas

- [ ] Diseñar formulario de creación de candidato en Frontend
- [ ] Implementar endpoint `POST /api/candidates` en Backend
- [ ] Crear migración de BD para tabla `Candidate`
- [ ] Implementar upload de archivo a S3 con SDK de AWS
- [ ] Generar URLs firmadas para visualización de CVs
- [ ] Implementar validación de email único por company_id
- [ ] Validar formato y tamaño de archivo (PDF/DOCX, max 5MB)
- [ ] Crear dropdown de fuentes de candidato (Referral, Job Board, Direct, Agency)
- [ ] Implementar manejo de errores de upload (S3 no disponible, timeout)
- [ ] Crear tests unitarios de validación de unicidad
- [ ] Crear tests de integración con S3 (usar localstack o mocks)
- [ ] Documentar endpoint en OpenAPI/Swagger

## Estimación

**Story Points:** 5  
**Valor de Negocio:** 5 (Crítico — sin candidatos no hay proceso)  
**Urgencia:** 5 (Bloqueante para MVP)

## Dependencias

- **US-001-02-LoginUsuario**: Recruiter debe estar autenticado
- Configuración de bucket S3 y credenciales AWS

## Prioridad

**Alta** — Dominio core del ATS

---

## Actualización de Estimación

**Ajuste realizado:** 5 SP → 8 SP  
**Fecha:** 2025-11-24  
**Razón:** La estimación inicial no contemplaba la complejidad acumulada de infraestructura multi-tenant y servicios externos descubierta en US-001-01. Factores que justifican el ajuste:
- Migración de Prisma para tabla Candidate con FK a Company (multi-tenancy estricto) y índices en email + company_id (unicidad compuesta)
- Integración completa con AWS S3 para upload de CVs (configuración de bucket, IAM roles, pre-signed URLs, validación de tipos MIME)
- Sistema de permisos multi-tenant (solo usuarios del company_id pueden registrar candidatos)
- Validación de duplicados por email dentro del mismo company_id
- Arquitectura hexagonal con use case + repository + S3 service
- Manejo de errores de S3 (timeouts, cuotas excedidas, archivos corruptos)
- Tests de integración con mocking de S3 + tests de validación de archivos

La complejidad de S3 + migraciones multi-tenant + permisos justifica el ajuste de 5 a 8 SP (similar a US-001-01 y US-001-02).
