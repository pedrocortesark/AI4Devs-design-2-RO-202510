# US-001-03 — Invitación de Usuarios por Admin

## Narrativa

**Como** administrador de una empresa  
**Quiero** invitar a nuevos usuarios (Recruiters y Hiring Managers) a mi tenant  
**Para** que mi equipo pueda colaborar en los procesos de reclutamiento dentro de LTI

## Descripción

Esta historia permite que un usuario con rol de Admin invite a otros usuarios a unirse a su empresa en LTI. El flujo de invitación envía un email con link mágico/token temporal, que al ser usado crea la cuenta del nuevo usuario asociado a la empresa del Admin y con el rol especificado.

## Criterios de Aceptación

```gherkin
Escenario 1: Invitación exitosa de un Recruiter
  Dado que un Admin está autenticado en LTI
  Cuando accede a la sección "Gestión de Usuarios"
  Y hace clic en "Invitar usuario"
  Y completa el formulario con email "recruiter@techcorp.com", nombre "Ana López" y rol "Recruiter"
  Y hace clic en "Enviar invitación"
  Entonces el sistema crea un registro de invitación pendiente con token temporal
  Y envía un email a "recruiter@techcorp.com" con link de activación
  Y el Admin ve una confirmación "Invitación enviada correctamente"

Escenario 2: Usuario invitado completa registro
  Dado que un usuario recibe un email de invitación con token válido
  Cuando hace clic en el link de activación
  Entonces es redirigido a una página de configuración de cuenta
  Y puede establecer su contraseña (o usar login social)
  Y al completar el registro, su cuenta queda activa con rol asignado
  Y puede hacer login inmediatamente

Escenario 3: Validación de email único dentro del tenant
  Dado que existe un usuario con email "recruiter@techcorp.com" en la empresa TechCorp
  Cuando un Admin de TechCorp intenta invitar a otro usuario con el mismo email
  Entonces el sistema muestra error "Este email ya está registrado en tu empresa"
  Y no se envía ninguna invitación

Escenario 4: Token de invitación expirado
  Dado que un usuario recibe una invitación con token que expira en 48 horas
  Cuando intenta activar el link después de 49 horas
  Entonces el sistema muestra error "Este link de invitación ha expirado. Contacta al administrador"
  Y no se crea ninguna cuenta de usuario
```

## Notas Técnicas

- **Modelo de datos**: Tabla `UserInvitation` con `id`, `company_id`, `email`, `role_id`, `token`, `expires_at`, `status` (pending/accepted/expired).
- **Token temporal**: Generado con biblioteca segura (crypto), único por invitación, expira en 48 horas.
- **Email**: Plantilla de invitación con link formato `https://lti.app/invite?token={token}`.
- **Roles permitidos**: Admin puede invitar a Recruiter y Hiring Manager (pero no a otros Admins en MVP).
- **Seguridad**: Validar que el token pertenece a la empresa del usuario que lo activa.

## Tareas

- [ ] Diseñar pantalla de "Gestión de Usuarios" en Frontend
- [ ] Crear formulario de invitación con campos (email, nombre, rol)
- [ ] Implementar endpoint `POST /api/invitations` en Backend
- [ ] Crear migración de BD para tabla `UserInvitation`
- [ ] Implementar generación de token seguro y fecha de expiración
- [ ] Crear plantilla de email de invitación
- [ ] Configurar envío de email con link de activación
- [ ] Implementar endpoint `GET /api/invitations/:token` para validar token
- [ ] Implementar endpoint `POST /api/invitations/:token/accept` para activar cuenta
- [ ] Validar unicidad de email dentro del tenant
- [ ] Manejar expiración de tokens (job/cron que limpia tokens expirados)
- [ ] Crear tests unitarios de validación de token
- [ ] Crear tests de integración del flujo completo de invitación
- [ ] Documentar endpoints en OpenAPI/Swagger

## Estimación

**Story Points:** 5  
**Tiempo estimado:** 3-4 días

## Dependencias

- **US-001-01-RegistroEmpresa**: Debe existir una empresa.
- **US-001-02-LoginUsuario**: Admin debe estar autenticado.
- Proveedor de email configurado.

## Prioridad

**Alta** — Necesaria para que el tenant tenga más de un usuario y habilite colaboración.

---

## Actualización de Estimación

**Ajuste realizado:** 3 SP → 5 SP  
**Fecha:** 2025-11-24  
**Razón:** La estimación inicial consideraba esta historia como "quick win" simple, pero el análisis de US-001-01 reveló complejidades adicionales:
- Generación de tokens temporales con expiración (JWT o UUID con TTL en Redis)
- Plantilla de email de invitación con SendGrid (reutiliza infraestructura de US-001-01 pero requiere template nuevo)
- Validación de permisos (solo Admins pueden invitar) con middleware de autorización
- Flujo de aceptación de invitación con verificación de token y creación de User asociado a company_id correcto
- Manejo de casos edge (token expirado, email ya registrado, reinvitaciones)

La complejidad de SendGrid y gestión de tokens temporales justifica el ajuste de 3 a 5 SP.
