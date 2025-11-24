# TASK-001-01-06 — Tests de Integración End-to-End de Registro

---

## Metadatos del Ticket

| Campo                  | Valor                                      |
|------------------------|--------------------------------------------|
| **ID**                 | TASK-001-01-06                             |
| **User Story Padre**   | US-001-01-RegistroEmpresa                  |
| **Roles Implicados**   | QA, Backend, Frontend                      |
| **Tipo**               | Testing                                    |
| **Categoría/Tags**     | E2E, Integration, QA                       |
| **Estimación Técnica** | 2 SP (Fibonacci)                           |
| **Prioridad**          | Alta (no se puede desplegar sin tests)     |

---

## Contexto (Product Owner)

Para garantizar que los criterios de aceptación se cumplen en todos los escenarios (happy path, validaciones, errores), necesitamos tests de integración automatizados que verifiquen:
1. Flujo completo de registro exitoso (Frontend → Backend → DB → Email).
2. Validación de dominio duplicado.
3. Manejo de errores de integración (Auth0 caído, SendGrid falla).

---

## Especificación Técnica (Tech Lead)

### Objetivo
Implementar tests E2E con Playwright y tests de integración Backend con Supertest que cubran todos los escenarios de la US-001-01.

### Tests Backend (Integration Tests con Supertest + Jest)

**Archivo:** `tests/integration/companies.test.ts`

```typescript
import request from 'supertest';
import { app } from '../../src/app';
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

describe('POST /api/companies', () => {
  beforeEach(async () => {
    // Limpiar BD antes de cada test
    await prisma.company.deleteMany();
  });

  afterAll(async () => {
    await prisma.$disconnect();
  });

  describe('Happy Path', () => {
    it('should create company successfully with valid data', async () => {
      const response = await request(app)
        .post('/api/companies')
        .send({
          name: 'TechCorp',
          domain: 'techcorp.com',
        })
        .expect(201);

      expect(response.body).toMatchObject({
        name: 'TechCorp',
        domain: 'techcorp.com',
        status: 'ACTIVE',
      });
      expect(response.body.id).toBeDefined();
      expect(response.body.created_at).toBeDefined();

      // Verificar que se creó en BD
      const companyInDB = await prisma.company.findUnique({
        where: { domain: 'techcorp.com' },
      });
      expect(companyInDB).toBeTruthy();
      expect(companyInDB!.name).toBe('TechCorp');
    });
  });

  describe('Validation Errors', () => {
    it('should return 400 for missing name', async () => {
      const response = await request(app)
        .post('/api/companies')
        .send({ domain: 'techcorp.com' })
        .expect(400);

      expect(response.body.error).toContain('name');
    });

    it('should return 400 for invalid domain format', async () => {
      const response = await request(app)
        .post('/api/companies')
        .send({ name: 'TechCorp', domain: 'invalid-domain' })
        .expect(400);

      expect(response.body.error).toContain('domain');
    });
  });

  describe('Business Logic Errors', () => {
    it('should return 409 if domain already exists', async () => {
      // Crear empresa inicial
      await prisma.company.create({
        data: { name: 'Existing Corp', domain: 'techcorp.com' },
      });

      // Intentar crear otra con mismo dominio
      const response = await request(app)
        .post('/api/companies')
        .send({ name: 'TechCorp', domain: 'techcorp.com' })
        .expect(409);

      expect(response.body.error).toBe('DomainAlreadyExists');
      expect(response.body.message).toContain('ya está registrado');
    });
  });

  describe('Email Sending', () => {
    it('should send welcome email after company creation', async () => {
      // Mock de sendEmail
      const sendEmailMock = jest.spyOn(require('../../src/infrastructure/email/sendgrid.service'), 'sendEmail');
      sendEmailMock.mockResolvedValue(undefined);

      await request(app)
        .post('/api/companies')
        .send({
          name: 'TechCorp',
          domain: 'techcorp.com',
          adminEmail: 'admin@techcorp.com',
        })
        .expect(201);

      // Verificar que se llamó sendEmail
      expect(sendEmailMock).toHaveBeenCalledWith(
        expect.objectContaining({
          to: 'admin@techcorp.com',
          subject: expect.stringContaining('Bienvenido'),
        })
      );

      sendEmailMock.mockRestore();
    });

    it('should complete registration even if email fails', async () => {
      // Mock de sendEmail que falla
      const sendEmailMock = jest.spyOn(require('../../src/infrastructure/email/sendgrid.service'), 'sendEmail');
      sendEmailMock.mockRejectedValue(new Error('SendGrid down'));

      const response = await request(app)
        .post('/api/companies')
        .send({
          name: 'TechCorp',
          domain: 'techcorp.com',
          adminEmail: 'admin@techcorp.com',
        })
        .expect(201); // Debe retornar 201 aunque falle email

      expect(response.body.domain).toBe('techcorp.com');

      sendEmailMock.mockRestore();
    });
  });
});
```

### Tests Frontend (E2E con Playwright)

**Archivo:** `e2e/register-company.spec.ts`

```typescript
import { test, expect } from '@playwright/test';

test.describe('Company Registration Flow', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/auth/register');
  });

  test('should complete registration successfully', async ({ page }) => {
    // Llenar formulario
    await page.fill('input[name="name"]', 'TechCorp');
    await page.fill('input[name="domain"]', 'techcorp-test.com');
    await page.check('input[name="acceptTerms"]');

    // Interceptar llamada a API
    await page.route('**/api/companies', async (route) => {
      await route.fulfill({
        status: 201,
        body: JSON.stringify({
          id: '123',
          name: 'TechCorp',
          domain: 'techcorp-test.com',
          status: 'ACTIVE',
        }),
      });
    });

    // Submit
    await page.click('button[type="submit"]');

    // Verificar redirección
    await expect(page).toHaveURL('/auth/setup-admin');
  });

  test('should show error for duplicate domain', async ({ page }) => {
    await page.fill('input[name="name"]', 'TechCorp');
    await page.fill('input[name="domain"]', 'existing-domain.com');
    await page.check('input[name="acceptTerms"]');

    // Interceptar API con error 409
    await page.route('**/api/companies', async (route) => {
      await route.fulfill({
        status: 409,
        body: JSON.stringify({
          error: 'DomainAlreadyExists',
          message: 'Este dominio ya está registrado',
        }),
      });
    });

    await page.click('button[type="submit"]');

    // Verificar mensaje de error
    await expect(page.locator('text=Este dominio ya está registrado')).toBeVisible();
  });

  test('should validate domain format', async ({ page }) => {
    await page.fill('input[name="name"]', 'TechCorp');
    await page.fill('input[name="domain"]', 'invalid-domain');
    await page.blur('input[name="domain"]');

    // Verificar error de validación client-side
    await expect(page.locator('text=Formato de dominio inválido')).toBeVisible();
  });

  test('should disable submit button until terms accepted', async ({ page }) => {
    await page.fill('input[name="name"]', 'TechCorp');
    await page.fill('input[name="domain"]', 'techcorp.com');

    // Botón debe estar deshabilitado
    const submitButton = page.locator('button[type="submit"]');
    await expect(submitButton).toBeDisabled();

    // Aceptar términos
    await page.check('input[name="acceptTerms"]');

    // Botón debe estar habilitado
    await expect(submitButton).toBeEnabled();
  });
});
```

---

## Validación (Arquitecto de Software)

### Consideraciones de Testing

✅ **Tests aislados**: Cada test limpia BD antes de ejecutar (`beforeEach`).

✅ **Mocking de servicios externos**: SendGrid no se llama en tests (evita costos y flakiness).

✅ **Coverage objetivo**: >80% en Backend, >70% en Frontend.

### Consideraciones de CI/CD

✅ **Tests en pipeline**: Deben ejecutarse antes de merge a main.

⚠️ **BD de test**: Usar PostgreSQL separado o in-memory (SQLite) para tests.

**Configuración de BD de test:**

```env
# .env.test
DATABASE_URL=postgresql://user:password@localhost:5433/lti_test
```

### Deuda Técnica a Evitar

❌ **No compartir estado entre tests**: Cada test debe ser independiente.

❌ **No omitir tests de errores**: Edge cases son críticos (409, 500, timeouts).

---

## Definición de Hecho (DoD)

- [ ] Tests de integración Backend implementados (happy path + errores)
- [ ] Tests E2E Frontend implementados con Playwright
- [ ] Coverage de Backend >80% en `CreateCompanyUseCase`
- [ ] Coverage de Frontend >70% en `RegisterCompany` component
- [ ] Todos los tests pasan en local
- [ ] Tests integrados en CI/CD (GitHub Actions o similar)
- [ ] BD de test configurada y documentada
- [ ] Mocks de servicios externos (SendGrid, Auth0) implementados
- [ ] Documentación de cómo ejecutar tests en README.md

---

## Notas Adicionales

**Comandos para ejecutar tests:**

```bash
# Backend (Integration tests)
npm run test:integration

# Frontend (E2E tests)
npm run test:e2e

# Coverage report
npm run test:coverage
```

**GitHub Actions CI (ejemplo):**

```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:14
        env:
          POSTGRES_PASSWORD: password
          POSTGRES_DB: lti_test
        ports:
          - 5433:5432

    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run migrations
        run: npx prisma migrate deploy
        env:
          DATABASE_URL: postgresql://postgres:password@localhost:5433/lti_test
      
      - name: Run tests
        run: npm run test:integration
```

**Dependencias adicionales:**

```json
{
  "devDependencies": {
    "@playwright/test": "^1.40.1",
    "jest": "^29.7.0",
    "supertest": "^6.3.3",
    "@types/supertest": "^6.0.2"
  }
}
```
