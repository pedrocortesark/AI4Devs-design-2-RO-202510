# TASK-001-01-02 — Integración con Auth0/Clerk para Registro de Usuario

---

## Metadatos del Ticket

| Campo                  | Valor                                      |
|------------------------|--------------------------------------------|
| **ID**                 | TASK-001-01-02                             |
| **User Story Padre**   | US-001-01-RegistroEmpresa                  |
| **Roles Implicados**   | Backend, DevOps                            |
| **Tipo**               | Infra                                      |
| **Categoría/Tags**     | Auth, OAuth, Security                      |
| **Estimación Técnica** | 3 SP (Fibonacci)                           |
| **Prioridad**          | Crítica (bloqueante para autenticación)    |

---

## Contexto (Product Owner)

Para cumplir el criterio de aceptación **"Escenario 3: Integración con proveedor de autenticación"**, debemos delegar la gestión de credenciales y sesiones a un proveedor externo (Auth0 o Clerk), asegurando que:
- Los usuarios se registren de forma segura.
- Obtenemos un `user_id` único del proveedor.
- Tokens JWT se emitan con claims de `company_id` para multi-tenancy.

---

## Especificación Técnica (Tech Lead)

### Objetivo
Configurar y conectar Auth0 (opción recomendada) con LTI para flujo de registro OAuth 2.0 + OIDC.

### Configuración de Auth0

**1. Crear aplicación en Auth0 Dashboard:**
- Tipo: **Regular Web Application**
- Allowed Callback URLs: `http://localhost:3000/api/auth/callback`
- Allowed Logout URLs: `http://localhost:3000`
- JWT Expiration: 7200 segundos (2 horas)

**2. Variables de entorno requeridas:**

```env
# .env
AUTH0_DOMAIN=lti-platform.us.auth0.com
AUTH0_CLIENT_ID=your_client_id_here
AUTH0_CLIENT_SECRET=your_client_secret_here
AUTH0_AUDIENCE=https://api.lti-platform.com
AUTH0_CALLBACK_URL=http://localhost:3000/api/auth/callback
```

**3. Implementación Backend (Node.js + Express):**

**Archivo:** `src/infrastructure/auth/auth0.service.ts`

```typescript
import { auth } from 'express-openid-connect';

export const auth0Config = {
  authRequired: false,
  auth0Logout: true,
  secret: process.env.AUTH0_CLIENT_SECRET,
  baseURL: process.env.BASE_URL || 'http://localhost:3000',
  clientID: process.env.AUTH0_CLIENT_ID,
  issuerBaseURL: `https://${process.env.AUTH0_DOMAIN}`,
  audience: process.env.AUTH0_AUDIENCE,
};

// Middleware de autenticación
export const authMiddleware = auth(auth0Config);
```

**4. Endpoint de callback para capturar user_id:**

**Archivo:** `src/api/routes/auth.routes.ts`

```typescript
import { Router } from 'express';
import { authMiddleware } from '../../infrastructure/auth/auth0.service';

const router = Router();

// Auth0 gestiona el callback automáticamente
router.use(authMiddleware);

// Endpoint para obtener usuario autenticado
router.get('/me', (req, res) => {
  if (!req.oidc.isAuthenticated()) {
    return res.status(401).json({ error: 'Not authenticated' });
  }
  
  const auth0User = req.oidc.user;
  res.json({
    sub: auth0User.sub, // Este es el user_id de Auth0
    email: auth0User.email,
    name: auth0User.name,
  });
});

export default router;
```

### Flujo de Registro

1. Usuario hace clic en "Crear cuenta" → redirige a Auth0 Hosted Login
2. Usuario completa registro en Auth0 (email + password)
3. Auth0 redirige a `/api/auth/callback` con authorization code
4. Backend intercambia code por tokens (access_token + id_token)
5. Backend obtiene `user.sub` (user_id de Auth0) y lo asocia con User en LTI

---

## Validación (Arquitecto de Software)

### Consideraciones de Seguridad

✅ **OAuth 2.0 + OIDC estándar**: Auth0 gestiona credenciales, LTI nunca ve passwords.

✅ **JWT con firma RS256**: Tokens firmados con clave privada de Auth0, verificables con clave pública.

✅ **Claims personalizados**: Agregar `company_id` al token JWT para multi-tenancy.

**Configuración de Custom Claims en Auth0:**

**Auth0 Action (post-login):**

```javascript
exports.onExecutePostLogin = async (event, api) => {
  const namespace = 'https://lti-platform.com';
  
  // Obtener company_id del metadata del usuario
  const companyId = event.user.app_metadata?.company_id;
  
  if (companyId) {
    api.idToken.setCustomClaim(`${namespace}/company_id`, companyId);
    api.accessToken.setCustomClaim(`${namespace}/company_id`, companyId);
  }
};
```

### Consideraciones de Performance

⚠️ **Token refresh**: Implementar refresh token rotation para evitar re-logins frecuentes.

✅ **Cache de public keys**: Cachear JWKS (JSON Web Key Set) de Auth0 para evitar llamadas en cada request.

### Deuda Técnica a Evitar

❌ **No implementar autenticación custom**: Usar Auth0/Clerk evita vulnerabilidades comunes (XSS, CSRF).

❌ **No almacenar passwords en LTI**: Delegar completamente a Auth0.

---

## Definición de Hecho (DoD)

- [ ] Cuenta de Auth0 creada y aplicación configurada
- [ ] Variables de entorno configuradas en `.env`
- [ ] Middleware de Auth0 integrado en Backend
- [ ] Endpoint `/api/auth/me` devuelve `user.sub` correctamente
- [ ] Flujo de registro probado manualmente (registro → callback → token)
- [ ] JWT incluye custom claim `company_id`
- [ ] Documentación de configuración de Auth0 en README.md
- [ ] Tests de integración verifican flujo OAuth completo

---

## Notas Adicionales

**Alternativa: Clerk**

Si se elige Clerk en lugar de Auth0, usar SDK `@clerk/clerk-sdk-node`:

```typescript
import { ClerkExpressRequireAuth } from '@clerk/clerk-sdk-node';

router.get('/protected', ClerkExpressRequireAuth(), (req, res) => {
  res.json({ userId: req.auth.userId });
});
```

**Dependencias de package.json:**

```json
{
  "dependencies": {
    "express-openid-connect": "^2.17.0",
    "jsonwebtoken": "^9.0.2",
    "jwks-rsa": "^3.1.0"
  }
}
```
