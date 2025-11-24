# EPIC-001 — Gestión de Usuarios

## Descripción

Esta épica cubre la implementación del sistema de autenticación, autorización y gestión de usuarios multi-tenant de LTI. Incluye registro, login, gestión de roles (Recruiter, Hiring Manager, Admin) y permisos, así como la integración con un proveedor externo de autenticación (Auth0, Clerk o Cognito).

El objetivo es establecer la infraestructura de seguridad y multitenancy que soportará todas las funcionalidades posteriores de la plataforma.

## Valor de Negocio

- **Seguridad y confianza**: Garantizar que solo usuarios autorizados accedan al sistema y que cada empresa (tenant) vea únicamente sus propios datos.
- **Escalabilidad multi-tenant**: Permitir que múltiples empresas usen LTI de forma aislada sin configuraciones complejas.
- **Base para colaboración**: Los roles (Recruiter, Hiring Manager) habilitan flujos colaborativos diferenciados entre perfiles.
- **Reducción de fricción de onboarding**: Integración con proveedores externos de Auth facilita login social (Google, Microsoft) y mejora UX inicial.

## Funcionalidades Preliminares

- **Registro de empresas (Companies)**: Crear tenants con dominio y configuración inicial.
- **Registro y login de usuarios**: Integración con proveedor de Auth (SSO, social login, recuperación de contraseña).
- **Gestión de roles y permisos**: Definir roles (Recruiter, Hiring Manager, Admin) y asociarlos a usuarios.
- **Multi-tenancy lógico**: Asegurar que todas las consultas y operaciones filtren por `company_id` de forma transparente.
- **Invitaciones de usuario**: Permitir a Admins invitar a nuevos usuarios vía email.
- **Gestión básica de perfiles**: Actualizar nombre, email, foto de perfil.
- **Auditoría inicial**: Registrar eventos de login/logout en `AuditLog`.

## Dependencias

- Ninguna (épica fundacional).

## Estimación Preliminar

- **Complejidad**: Media
- **Esfuerzo estimado**: 3-4 semanas (incluyendo integración con proveedor de Auth, setup multi-tenant y pruebas de seguridad).

## Criterios de Éxito

- Un usuario puede registrarse, hacer login y ser asignado a una `Company` y un `Role`.
- Usuarios de diferentes empresas no pueden ver datos entre sí.
- Admins pueden invitar a recruiters y hiring managers a su empresa.
- El sistema soporta login social (Google, Microsoft) y recuperación de contraseña.
