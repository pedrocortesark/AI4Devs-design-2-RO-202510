# US-001-02 — Login de Usuario con Autenticación Multi-Tenant

## Narrativa

**Como** usuario registrado (Admin, Recruiter o Hiring Manager)  
**Quiero** iniciar sesión en LTI de forma segura  
**Para** acceder a las funcionalidades de la plataforma correspondientes a mi empresa y rol

## Descripción

Esta historia implementa el flujo de login para usuarios ya registrados, garantizando que cada usuario acceda únicamente a los datos de su empresa (tenant) y con los permisos correspondientes a su rol. El login debe integrarse con el proveedor de autenticación externo y validar claims de empresa y rol en el token JWT/OIDC.

## Criterios de Aceptación

```gherkin
Escenario 1: Login exitoso con credenciales válidas
  Dado que un usuario con email "recruiter@techcorp.com" y contraseña válida existe en el sistema
  Cuando accede a la página de login e ingresa sus credenciales
  Y hace clic en "Iniciar sesión"
  Entonces el sistema valida las credenciales con el proveedor de Auth
  Y obtiene un token JWT con claims de company_id y role_id
  Y el usuario es redirigido al dashboard correspondiente a su rol
  Y la sesión queda activa durante 8 horas (o hasta logout explícito)

Escenario 2: Login con credenciales incorrectas
  Dado que un usuario intenta hacer login con email "recruiter@techcorp.com"
  Cuando ingresa una contraseña incorrecta
  Entonces el sistema muestra error "Credenciales incorrectas"
  Y no se genera ningún token de sesión
  Y el usuario permanece en la página de login

Escenario 3: Login con cuenta desactivada
  Dado que existe un usuario con email "disabled@techcorp.com" con estado is_active=false
  Cuando intenta hacer login con credenciales válidas
  Entonces el sistema muestra error "Tu cuenta está desactivada. Contacta al administrador"
  Y no se genera ningún token de sesión

Escenario 4: Multi-tenancy en login
  Dado que un usuario pertenece a la empresa con company_id=123
  Cuando hace login exitosamente
  Entonces el token JWT incluye el claim "company_id": 123
  Y todas las consultas posteriores filtran datos por company_id=123
  Y el usuario no puede ver datos de otras empresas
```

## Notas Técnicas

- **Autenticación**: OAuth 2.0/OIDC con Auth0/Clerk/Cognito.
- **Token JWT**: Claims obligatorios: `user_id`, `company_id`, `role_id`, `email`, `exp` (expiración).
- **Validación de sesión**: Middleware en Backend valida token en cada request.
- **Multi-tenancy**: El `company_id` del token se usa para filtrar todas las consultas a BD.
- **Seguridad**: Tokens almacenados en cookies HttpOnly y SameSite para prevenir XSS/CSRF.

## Tareas

- [ ] Diseñar página de login en Frontend (React)
- [ ] Implementar integración con Auth0/Clerk para flujo de login (OAuth)
- [ ] Crear endpoint `POST /api/auth/login` que valida token con proveedor externo
- [ ] Implementar middleware de validación de token JWT en Backend
- [ ] Configurar cookies HttpOnly para almacenar tokens
- [ ] Implementar lógica de validación de cuenta activa (`is_active=true`)
- [ ] Configurar expiración de sesión (8 horas)
- [ ] Implementar endpoint `POST /api/auth/logout` para invalidar sesión
- [ ] Crear tests unitarios de validación de token
- [ ] Crear tests de integración del flujo completo de login
- [ ] Documentar flujo de autenticación en README técnico

## Estimación

**Story Points:** 5  
**Tiempo estimado:** 3-4 días

## Dependencias

- **US-001-01-RegistroEmpresa**: Debe existir al menos una empresa y un usuario registrado.
- Configuración completa de Auth0/Clerk/Cognito con callbacks y secrets.

## Prioridad

**Alta** — Sin login no hay acceso a la plataforma.

---

## Actualización de Estimación

**Ajuste realizado:** 5 SP → 8 SP  
**Fecha:** 2025-11-24  
**Razón:** La estimación original no contemplaba la complejidad heredada de la infraestructura de Auth0 configurada en US-001-01. Factores identificados:
- Middleware de autenticación con verificación de JWT y extracción de custom claims (company_id)
- Lógica de contexto multi-tenant en cada request (filtrado automático por company_id)
- Manejo de refresh tokens y rotación de sesiones
- Tests de integración con mocking de Auth0 y validación de claims
- Configuración de CORS y cookies seguras (HttpOnly, SameSite)

Esta historia hereda la complejidad de autenticación multi-tenant descubierta en el análisis técnico de US-001-01.
