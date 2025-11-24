# TASK-001-01-01 — Migración de Base de Datos para Tabla Company

---

## Metadatos del Ticket

| Campo                  | Valor                                      |
|------------------------|--------------------------------------------|
| **ID**                 | TASK-001-01-01                             |
| **User Story Padre**   | US-001-01-RegistroEmpresa                  |
| **Roles Implicados**   | Backend, DBA                               |
| **Tipo**               | DB                                         |
| **Categoría/Tags**     | Database, Migration, Schema                |
| **Estimación Técnica** | 2 SP (Fibonacci)                           |
| **Prioridad**          | Crítica (bloqueante para Backend)          |

---

## Contexto (Product Owner)

Para cumplir el criterio de aceptación **"Escenario 1: Registro exitoso de empresa"**, necesitamos almacenar los datos básicos de la empresa (nombre, dominio, estado) en una tabla dedicada que servirá como base del modelo multi-tenant de LTI.

Esta tabla `Company` será el punto de entrada del sistema y todas las entidades posteriores (User, JobPosting, Candidate) tendrán una foreign key hacia ella (`company_id`).

---

## Especificación Técnica (Tech Lead)

### Objetivo
Crear migración de Prisma para la tabla `Company` con columnas y restricciones necesarias.

### Definición del Schema

**Archivo:** `prisma/schema.prisma`

```prisma
model Company {
  id         String   @id @default(uuid())
  name       String
  domain     String   @unique
  status     CompanyStatus @default(ACTIVE)
  created_at DateTime @default(now())
  updated_at DateTime @updatedAt
  
  users      User[]
  jobPostings JobPosting[]
  candidates  Candidate[]

  @@index([domain])
  @@map("companies")
}

enum CompanyStatus {
  ACTIVE
  SUSPENDED
  TRIAL
}
```

### Comandos de Ejecución

```bash
# Generar migración
npx prisma migrate dev --name create_company_table

# Verificar migración
npx prisma migrate status

# Aplicar en producción (futuro)
npx prisma migrate deploy
```

### Estructura SQL Esperada (PostgreSQL)

```sql
CREATE TABLE companies (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  domain VARCHAR(255) NOT NULL UNIQUE,
  status VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_companies_domain ON companies(domain);
```

---

## Validación (Arquitecto de Software)

### Consideraciones de Seguridad
- ✅ **Unicidad de dominio**: El constraint `@unique` en `domain` previene registros duplicados.
- ✅ **Identificador UUID**: Usar UUID en lugar de auto-increment previene enumeración de tenants.
- ✅ **Índice en domain**: Necesario para búsquedas rápidas en validación de registro.

### Consideraciones de Performance
- ✅ **Índice en domain**: Optimiza queries de validación (`WHERE domain = ?`).
- ⚠️ **Sin índice en name**: No es búsqueda frecuente, evitar overhead innecesario.

### Consideraciones de Escalabilidad
- ✅ **Status como enum**: Permite estados futuros (TRIAL, SUSPENDED) sin cambios de schema.
- ✅ **Timestamps automáticos**: `@default(now())` y `@updatedAt` para auditoría.

### Deuda Técnica a Evitar
- ❌ **No usar INT como PK**: UUIDs son mejores para multi-tenancy y seguridad.
- ❌ **No omitir updated_at**: Crítico para sincronización y cache invalidation.

---

## Definición de Hecho (DoD)

- [ ] Migración ejecutada correctamente en entorno local
- [ ] Schema validado con `npx prisma validate`
- [ ] Tabla creada en PostgreSQL con columnas y constraints esperados
- [ ] Índice en `domain` verificado con `\d companies` en psql
- [ ] Migración documentada en archivo SQL generado por Prisma
- [ ] Tests de integración verifican creación de Company

---

## Notas Adicionales

**Dependencias externas:**
- PostgreSQL 14+ instalado y configurado
- Prisma CLI instalado (`npm install -D prisma`)

**Relaciones futuras:**
Esta tabla será referenciada por `User`, `JobPosting`, `Candidate`, `Application` mediante FK `company_id`.
