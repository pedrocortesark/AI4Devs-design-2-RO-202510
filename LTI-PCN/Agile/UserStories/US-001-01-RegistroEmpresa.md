# US-001-01 — Registro de Empresa (Company)

## Narrativa

**Como** administrador de una nueva empresa que quiere usar LTI  
**Quiero** registrar mi empresa en la plataforma  
**Para** crear un tenant aislado donde mi equipo pueda gestionar procesos de reclutamiento

## Descripción

Esta historia permite que un usuario cree una cuenta de empresa (Company) en LTI, estableciendo el tenant principal bajo el cual se asociarán usuarios, ofertas y candidatos. El registro debe capturar información básica de la empresa (nombre, dominio) e integrar autenticación vía proveedor externo (Auth0/Clerk/Cognito).

Al completar el registro, se crea automáticamente el primer usuario con rol de Admin, quien podrá invitar a recruiters y hiring managers posteriormente.

## Criterios de Aceptación

```gherkin
Escenario 1: Registro exitoso de empresa
  Dado que un usuario accede a la página de registro de LTI
  Cuando completa el formulario con nombre de empresa "TechCorp" y dominio "techcorp.com"
  Y acepta términos y condiciones
  Y hace clic en "Crear cuenta"
  Entonces se crea un registro en la tabla Company con estado activo
  Y el usuario es redirigido a configurar su cuenta de administrador
  Y recibe un email de bienvenida con instrucciones iniciales

Escenario 2: Validación de dominio único
  Dado que existe una empresa con dominio "techcorp.com"
  Cuando un nuevo usuario intenta registrar otra empresa con el mismo dominio
  Entonces el sistema muestra error "Este dominio ya está registrado"
  Y no se crea ningún registro en la base de datos

Escenario 3: Integración con proveedor de autenticación
  Dado que un usuario inicia el registro de empresa
  Cuando completa el flujo de registro
  Entonces el sistema crea un registro en el proveedor de Auth (Auth0/Clerk)
  Y asocia el user_id del proveedor con el registro de User en LTI
  Y almacena tokens de sesión de forma segura
```

## Notas Técnicas

- **Modelo de datos**: Se crea registro en tabla `Company` con `id`, `name`, `domain`, `created_at`, `updated_at`.
- **Autenticación**: Integración con Auth0/Clerk/Cognito mediante OAuth 2.0/OIDC.
- **Multi-tenancy**: El `company_id` será el discriminador principal para todas las consultas posteriores.
- **Email**: Usar proveedor de email (SendGrid/Mailgun) para envío de bienvenida.

## Tareas

- [ ] Diseñar formulario de registro de empresa en Frontend (React)
- [ ] Implementar endpoint `POST /api/companies` en Backend
- [ ] Crear migración de BD para tabla `Company`
- [ ] Integrar flujo de registro con Auth0/Clerk (OAuth)
- [ ] Implementar validación de dominio único
- [ ] Crear plantilla de email de bienvenida
- [ ] Configurar envío de email con proveedor externo
- [ ] Escribir tests unitarios de validación de dominio
- [ ] Escribir tests de integración del flujo completo
- [ ] Documentar endpoint en OpenAPI/Swagger

## Estimación

**Story Points:** 5  
**Tiempo estimado:** 3-4 días

## Dependencias

- Configuración inicial de Auth0/Clerk/Cognito
- Configuración de proveedor de email (SendGrid/Mailgun)

## Prioridad

**Alta** — Historia fundacional, bloquea el resto del sistema.

---

## Plan de Implementación

Esta User Story ha sido descompuesta en 6 tickets técnicos ejecutables, cada uno con responsabilidades claras y trazabilidad completa:

### Tickets Backend/Infraestructura

1. **[TASK-001-01-01-MigracionCompany](../Tasks/TASK-001-01-01-MigracionCompany.md)** (2 SP)
   - **Rol:** Backend, DBA
   - **Objetivo:** Crear migración de Prisma para tabla `Company` con columnas, constraints e índices necesarios.
   - **Entregable:** Schema de BD validado y migración ejecutada.

2. **[TASK-001-01-02-IntegracionAuth0](../Tasks/TASK-001-01-02-IntegracionAuth0.md)** (3 SP)
   - **Rol:** Backend, DevOps
   - **Objetivo:** Configurar Auth0/Clerk para autenticación OAuth 2.0 + OIDC con custom claims de `company_id`.
   - **Entregable:** Middleware de autenticación funcional y tokens JWT con multi-tenancy.

3. **[TASK-001-01-03-EndpointPostCompanies](../Tasks/TASK-001-01-03-EndpointPostCompanies.md)** (3 SP)
   - **Rol:** Backend
   - **Objetivo:** Implementar endpoint `POST /api/companies` con validación de dominio único y arquitectura hexagonal.
   - **Entregable:** API RESTful documentada con Swagger, tests unitarios y de integración.

5. **[TASK-001-01-05-ConfiguracionEmail](../Tasks/TASK-001-01-05-ConfiguracionEmail.md)** (2 SP)
   - **Rol:** Backend, DevOps
   - **Objetivo:** Configurar SendGrid para envío de emails transaccionales con plantilla HTML de bienvenida.
   - **Entregable:** Email de bienvenida enviado tras registro con alta deliverability.

### Tickets Frontend

4. **[TASK-001-01-04-FormularioRegistro](../Tasks/TASK-001-01-04-FormularioRegistro.md)** (3 SP)
   - **Rol:** Frontend, UX/UI
   - **Objetivo:** Implementar formulario React con validación client-side (Zod) y consumo del endpoint de registro.
   - **Entregable:** UI responsive con feedback de errores y redirección post-registro.

### Tickets QA

6. **[TASK-001-01-06-TestsIntegracion](../Tasks/TASK-001-01-06-TestsIntegracion.md)** (2 SP)
   - **Rol:** QA, Backend, Frontend
   - **Objetivo:** Implementar tests de integración Backend (Supertest) y E2E Frontend (Playwright) cubriendo todos los escenarios.
   - **Entregable:** Coverage >80% Backend, >70% Frontend, tests integrados en CI/CD.

### Estimación Total y Distribución

**Story Points totales:** 15 SP (suma de tickets)  
**Story Points originales:** 5 SP (estimación de US-001-01)

**Nota:** La descomposición reveló mayor complejidad de la estimación inicial (3x). Esto es esperado en historias fundacionales que incluyen setup de infraestructura (Auth0, SendGrid, migraciones). Historias posteriores tendrán menor overhead.

**Distribución sugerida en Sprint:**
- **Sprint 1 (Semana 1):** TASK-001-01-01, TASK-001-01-02, TASK-001-01-03 (8 SP - Backend)
- **Sprint 1 (Semana 2):** TASK-001-01-04, TASK-001-01-05, TASK-001-01-06 (7 SP - Frontend + QA)

**Dependencias entre tickets:**
- TASK-001-01-01 debe completarse antes que TASK-001-01-03 (endpoint necesita tabla)
- TASK-001-01-02 debe completarse antes que TASK-001-01-04 (Frontend necesita Auth0 configurado)
- TASK-001-01-06 se ejecuta al final (requiere todos los tickets previos implementados)
