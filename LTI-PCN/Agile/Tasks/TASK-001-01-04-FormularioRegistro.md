# TASK-001-01-04 — Formulario de Registro de Empresa (Frontend)

---

## Metadatos del Ticket

| Campo                  | Valor                                      |
|------------------------|--------------------------------------------|
| **ID**                 | TASK-001-01-04                             |
| **User Story Padre**   | US-001-01-RegistroEmpresa                  |
| **Roles Implicados**   | Frontend, UX/UI                            |
| **Tipo**               | Feature                                    |
| **Categoría/Tags**     | UI/UX, Forms, Validation                   |
| **Estimación Técnica** | 3 SP (Fibonacci)                           |
| **Prioridad**          | Alta                                       |

---

## Contexto (Product Owner)

Para cumplir el criterio de aceptación **"Escenario 1: Registro exitoso de empresa"**, necesitamos una interfaz de usuario intuitiva donde:
1. El usuario ingrese nombre y dominio de su empresa.
2. Se valide el formato de los campos en tiempo real.
3. Se muestre feedback claro en caso de éxito o error.
4. Tras registro exitoso, se redirija a configuración de cuenta de administrador.

---

## Especificación Técnica (Tech Lead)

### Objetivo
Implementar componente React con formulario controlado, validación client-side y consumo del endpoint `POST /api/companies`.

### Implementación Frontend (React + TypeScript + React Hook Form)

**Archivo:** `src/pages/auth/RegisterCompany.tsx`

```typescript
import React from 'react';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { useNavigate } from 'react-router-dom';
import { registerCompany } from '../../services/api/companies.service';
import { Button } from '../../components/ui/Button';
import { Input } from '../../components/ui/Input';
import { Alert } from '../../components/ui/Alert';

const registerSchema = z.object({
  name: z.string().min(2, 'El nombre debe tener al menos 2 caracteres'),
  domain: z.string()
    .regex(/^[a-z0-9]+([-.][a-z0-9]+)*\.[a-z]{2,}$/, 'Formato de dominio inválido'),
  acceptTerms: z.boolean().refine(val => val === true, 'Debes aceptar los términos'),
});

type RegisterFormData = z.infer<typeof registerSchema>;

export function RegisterCompany() {
  const navigate = useNavigate();
  const [error, setError] = React.useState<string | null>(null);
  const [isLoading, setIsLoading] = React.useState(false);

  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<RegisterFormData>({
    resolver: zodResolver(registerSchema),
  });

  const onSubmit = async (data: RegisterFormData) => {
    setIsLoading(true);
    setError(null);

    try {
      const response = await registerCompany({
        name: data.name,
        domain: data.domain,
      });

      // Guardar company_id en localStorage (temporal, mejorar con context)
      localStorage.setItem('company_id', response.id);

      // Redirigir a configuración de admin
      navigate('/auth/setup-admin');
    } catch (err: any) {
      if (err.response?.status === 409) {
        setError('Este dominio ya está registrado. ¿Intentas acceder a una cuenta existente?');
      } else {
        setError('Error al registrar la empresa. Intenta nuevamente.');
      }
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-50">
      <div className="max-w-md w-full bg-white p-8 rounded-lg shadow-lg">
        <h1 className="text-2xl font-bold text-gray-900 mb-2">
          Crea tu cuenta en LTI
        </h1>
        <p className="text-gray-600 mb-6">
          Comienza a optimizar tu reclutamiento con IA y automatización
        </p>

        {error && (
          <Alert variant="error" className="mb-4">
            {error}
          </Alert>
        )}

        <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
          <div>
            <label htmlFor="name" className="block text-sm font-medium text-gray-700">
              Nombre de la empresa
            </label>
            <Input
              id="name"
              type="text"
              placeholder="TechCorp"
              {...register('name')}
              error={errors.name?.message}
            />
          </div>

          <div>
            <label htmlFor="domain" className="block text-sm font-medium text-gray-700">
              Dominio de la empresa
            </label>
            <Input
              id="domain"
              type="text"
              placeholder="techcorp.com"
              {...register('domain')}
              error={errors.domain?.message}
            />
            <p className="text-xs text-gray-500 mt-1">
              Será usado para identificar tu cuenta de forma única
            </p>
          </div>

          <div className="flex items-start">
            <input
              id="acceptTerms"
              type="checkbox"
              {...register('acceptTerms')}
              className="mt-1 h-4 w-4 text-blue-600 border-gray-300 rounded"
            />
            <label htmlFor="acceptTerms" className="ml-2 text-sm text-gray-700">
              Acepto los{' '}
              <a href="/terms" className="text-blue-600 hover:underline">
                términos y condiciones
              </a>{' '}
              de LTI
            </label>
          </div>
          {errors.acceptTerms && (
            <p className="text-sm text-red-600">{errors.acceptTerms.message}</p>
          )}

          <Button
            type="submit"
            className="w-full"
            disabled={isLoading}
          >
            {isLoading ? 'Creando cuenta...' : 'Crear cuenta'}
          </Button>
        </form>

        <p className="mt-4 text-center text-sm text-gray-600">
          ¿Ya tienes cuenta?{' '}
          <a href="/login" className="text-blue-600 hover:underline">
            Inicia sesión
          </a>
        </p>
      </div>
    </div>
  );
}
```

**Archivo:** `src/services/api/companies.service.ts`

```typescript
import axios from 'axios';

const API_BASE_URL = import.meta.env.VITE_API_BASE_URL || 'http://localhost:3000/api';

interface RegisterCompanyDTO {
  name: string;
  domain: string;
}

interface Company {
  id: string;
  name: string;
  domain: string;
  status: string;
  created_at: string;
}

export async function registerCompany(data: RegisterCompanyDTO): Promise<Company> {
  const response = await axios.post<Company>(`${API_BASE_URL}/companies`, data);
  return response.data;
}
```

---

## Validación (Arquitecto de Software)

### Consideraciones de Seguridad

✅ **Validación client-side con Zod**: Previene envío de datos malformados.

✅ **Sanitización de dominio**: Regex valida formato estricto de dominio.

⚠️ **CSRF protection**: Asegurar que Backend use tokens CSRF o validar origin headers.

### Consideraciones de UX

✅ **Feedback inmediato**: Errores de validación se muestran en tiempo real.

✅ **Estados de carga**: Botón deshabilitado y texto "Creando cuenta..." durante submit.

✅ **Manejo de errores específicos**: Mensaje diferenciado para 409 (dominio duplicado).

### Consideraciones de Accesibilidad

✅ **Labels asociados**: Todos los inputs tienen `<label>` con `htmlFor`.

✅ **Errores semánticos**: Mensajes de error asociados a campos con ARIA.

⚠️ **Contraste de colores**: Verificar que texto de error tenga ratio WCAG AA (4.5:1).

### Deuda Técnica a Evitar

❌ **No usar localStorage para company_id en producción**: Migrar a Context API o Redux.

❌ **No omitir loading states**: Usuarios deben ver feedback de progreso.

---

## Definición de Hecho (DoD)

- [ ] Componente `RegisterCompany` implementado en React
- [ ] Validación client-side con Zod funciona correctamente
- [ ] Formulario consume endpoint `POST /api/companies`
- [ ] Manejo de errores 409 muestra mensaje específico
- [ ] Estados de carga implementados (botón deshabilitado, spinner)
- [ ] Redirección a `/auth/setup-admin` tras registro exitoso
- [ ] Responsive design funciona en mobile y desktop
- [ ] Tests de componente con React Testing Library (cobertura >70%)
- [ ] Accesibilidad validada con Lighthouse (score >90)

---

## Notas Adicionales

**Dependencias de package.json:**

```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-hook-form": "^7.49.2",
    "zod": "^3.22.4",
    "@hookform/resolvers": "^3.3.3",
    "axios": "^1.6.2",
    "react-router-dom": "^6.20.1"
  },
  "devDependencies": {
    "@testing-library/react": "^14.1.2",
    "@testing-library/jest-dom": "^6.1.5",
    "vitest": "^1.0.4"
  }
}
```

**Tests de ejemplo:**

```typescript
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { RegisterCompany } from './RegisterCompany';
import { registerCompany } from '../../services/api/companies.service';

jest.mock('../../services/api/companies.service');

describe('RegisterCompany', () => {
  it('should show validation error for invalid domain', async () => {
    render(<RegisterCompany />);
    
    const domainInput = screen.getByLabelText(/Dominio de la empresa/i);
    fireEvent.change(domainInput, { target: { value: 'invalid-domain' } });
    fireEvent.blur(domainInput);
    
    await waitFor(() => {
      expect(screen.getByText(/Formato de dominio inválido/i)).toBeInTheDocument();
    });
  });
  
  it('should submit form successfully', async () => {
    (registerCompany as jest.Mock).mockResolvedValue({ id: '123', name: 'TechCorp' });
    
    render(<RegisterCompany />);
    
    fireEvent.change(screen.getByLabelText(/Nombre/i), { target: { value: 'TechCorp' } });
    fireEvent.change(screen.getByLabelText(/Dominio/i), { target: { value: 'techcorp.com' } });
    fireEvent.click(screen.getByLabelText(/Acepto/i));
    fireEvent.click(screen.getByText(/Crear cuenta/i));
    
    await waitFor(() => {
      expect(registerCompany).toHaveBeenCalledWith({
        name: 'TechCorp',
        domain: 'techcorp.com',
      });
    });
  });
});
```

**Diseño UI (referencia Figma/Tailwind):**

- Fondo: `bg-gray-50`
- Card: `bg-white shadow-lg rounded-lg p-8`
- Inputs: Tailwind `Input` component con border-gray-300
- Botón primario: `bg-blue-600 hover:bg-blue-700 text-white`
- Errores: `text-red-600 text-sm`
