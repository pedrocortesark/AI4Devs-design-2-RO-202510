# TASK-001-01-03 — Endpoint POST /api/companies (Backend)

---

## Metadatos del Ticket

| Campo                  | Valor                                      |
|------------------------|--------------------------------------------|
| **ID**                 | TASK-001-01-03                             |
| **User Story Padre**   | US-001-01-RegistroEmpresa                  |
| **Roles Implicados**   | Backend                                    |
| **Tipo**               | Feature                                    |
| **Categoría/Tags**     | API, Business Logic, Validation            |
| **Estimación Técnica** | 3 SP (Fibonacci)                           |
| **Prioridad**          | Alta                                       |

---

## Contexto (Product Owner)

Para cumplir los criterios de aceptación **"Escenario 1: Registro exitoso de empresa"** y **"Escenario 2: Validación de dominio único"**, necesitamos un endpoint que:
1. Reciba datos de la empresa (nombre, dominio).
2. Valide que el dominio no esté ya registrado.
3. Cree el registro en la tabla `Company`.
4. Retorne el `company_id` para asociarlo con el usuario administrador.

---

## Especificación Técnica (Tech Lead)

### Objetivo
Implementar endpoint RESTful para creación de empresas con validación de dominio único.

### Firma del Endpoint

```
POST /api/companies
Content-Type: application/json
Authorization: Bearer <access_token> (opcional en registro inicial)
```

**Request Body:**

```json
{
  "name": "TechCorp",
  "domain": "techcorp.com"
}
```

**Response (201 Created):**

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "TechCorp",
  "domain": "techcorp.com",
  "status": "ACTIVE",
  "created_at": "2025-11-24T10:30:00Z"
}
```

**Response (409 Conflict):**

```json
{
  "error": "DomainAlreadyExists",
  "message": "Este dominio ya está registrado"
}
```

### Implementación Backend (Node.js + Express + Prisma)

**Archivo:** `src/api/routes/companies.routes.ts`

```typescript
import { Router } from 'express';
import { createCompanyController } from '../controllers/companies.controller';
import { validateRequest } from '../middleware/validation.middleware';
import { createCompanySchema } from '../schemas/company.schema';

const router = Router();

router.post(
  '/',
  validateRequest(createCompanySchema),
  createCompanyController
);

export default router;
```

**Archivo:** `src/api/schemas/company.schema.ts`

```typescript
import Joi from 'joi';

export const createCompanySchema = Joi.object({
  name: Joi.string().min(2).max(255).required(),
  domain: Joi.string().domain().required(),
});
```

**Archivo:** `src/api/controllers/companies.controller.ts`

```typescript
import { Request, Response } from 'express';
import { CreateCompanyUseCase } from '../../application/useCases/CreateCompanyUseCase';
import { PrismaCompanyRepository } from '../../infrastructure/repositories/PrismaCompanyRepository';

export async function createCompanyController(req: Request, res: Response) {
  try {
    const repository = new PrismaCompanyRepository();
    const useCase = new CreateCompanyUseCase(repository);
    
    const company = await useCase.execute({
      name: req.body.name,
      domain: req.body.domain,
    });
    
    return res.status(201).json(company);
  } catch (error: any) {
    if (error.code === 'DOMAIN_ALREADY_EXISTS') {
      return res.status(409).json({
        error: 'DomainAlreadyExists',
        message: 'Este dominio ya está registrado',
      });
    }
    
    console.error('Error creating company:', error);
    return res.status(500).json({ error: 'InternalServerError' });
  }
}
```

**Archivo:** `src/application/useCases/CreateCompanyUseCase.ts`

```typescript
import { ICompanyRepository } from '../../domain/repositories/ICompanyRepository';
import { Company } from '../../domain/entities/Company';

interface CreateCompanyDTO {
  name: string;
  domain: string;
}

export class CreateCompanyUseCase {
  constructor(private companyRepository: ICompanyRepository) {}
  
  async execute(data: CreateCompanyDTO): Promise<Company> {
    // Validar dominio único
    const existingCompany = await this.companyRepository.findByDomain(data.domain);
    
    if (existingCompany) {
      throw { code: 'DOMAIN_ALREADY_EXISTS' };
    }
    
    // Crear empresa
    const company = await this.companyRepository.create({
      name: data.name,
      domain: data.domain,
      status: 'ACTIVE',
    });
    
    return company;
  }
}
```

**Archivo:** `src/infrastructure/repositories/PrismaCompanyRepository.ts`

```typescript
import { PrismaClient } from '@prisma/client';
import { ICompanyRepository } from '../../domain/repositories/ICompanyRepository';
import { Company } from '../../domain/entities/Company';

const prisma = new PrismaClient();

export class PrismaCompanyRepository implements ICompanyRepository {
  async create(data: Partial<Company>): Promise<Company> {
    return await prisma.company.create({
      data: {
        name: data.name!,
        domain: data.domain!,
        status: data.status || 'ACTIVE',
      },
    });
  }
  
  async findByDomain(domain: string): Promise<Company | null> {
    return await prisma.company.findUnique({
      where: { domain },
    });
  }
}
```

---

## Validación (Arquitecto de Software)

### Consideraciones de Seguridad

✅ **Validación de dominio**: Schema Joi valida formato de dominio (previene inyección).

✅ **Sanitización de inputs**: Prisma usa prepared statements (previene SQL injection).

⚠️ **Rate limiting**: Agregar middleware para prevenir spam de registros.

**Recomendación:**

```typescript
import rateLimit from 'express-rate-limit';

const registerLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 5, // 5 intentos por IP
  message: 'Demasiados intentos de registro, intenta más tarde',
});

router.post('/', registerLimiter, validateRequest(createCompanySchema), createCompanyController);
```

### Consideraciones de Performance

✅ **Query con índice**: `findUnique({ where: { domain } })` usa índice creado en TASK-001-01-01.

✅ **Transacción implícita**: Prisma `create()` es atómica.

### Consideraciones de Arquitectura

✅ **Hexagonal Architecture**: Separación clara entre controlador → use case → repository.

✅ **Dependency Injection**: Use case recibe repository como dependencia (testable).

### Deuda Técnica a Evitar

❌ **No mezclar lógica de negocio en controller**: Use cases encapsulan reglas.

❌ **No acceder directamente a Prisma desde controller**: Usar repository pattern.

---

## Definición de Hecho (DoD)

- [ ] Endpoint `POST /api/companies` implementado
- [ ] Validación de schema con Joi funciona correctamente
- [ ] Validación de dominio único retorna 409 si ya existe
- [ ] Creación exitosa retorna 201 con datos de la empresa
- [ ] Arquitectura hexagonal respetada (controller → use case → repository)
- [ ] Rate limiting configurado (5 intentos / 15 min)
- [ ] Tests unitarios de `CreateCompanyUseCase` (cobertura >80%)
- [ ] Tests de integración de endpoint (happy path + error cases)
- [ ] Documentación en OpenAPI/Swagger generada

---

## Notas Adicionales

**Dependencias de package.json:**

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "joi": "^17.11.0",
    "express-rate-limit": "^7.1.5",
    "@prisma/client": "^5.7.0"
  },
  "devDependencies": {
    "jest": "^29.7.0",
    "supertest": "^6.3.3"
  }
}
```

**Tests de ejemplo:**

```typescript
describe('POST /api/companies', () => {
  it('should create company successfully', async () => {
    const response = await request(app)
      .post('/api/companies')
      .send({ name: 'TechCorp', domain: 'techcorp.com' });
    
    expect(response.status).toBe(201);
    expect(response.body.domain).toBe('techcorp.com');
  });
  
  it('should return 409 if domain exists', async () => {
    await prisma.company.create({ data: { name: 'Existing', domain: 'techcorp.com' } });
    
    const response = await request(app)
      .post('/api/companies')
      .send({ name: 'TechCorp', domain: 'techcorp.com' });
    
    expect(response.status).toBe(409);
    expect(response.body.error).toBe('DomainAlreadyExists');
  });
});
```
