# TASK-001-01-05 — Configuración de Proveedor de Email para Bienvenida

---

## Metadatos del Ticket

| Campo                  | Valor                                      |
|------------------------|--------------------------------------------|
| **ID**                 | TASK-001-01-05                             |
| **User Story Padre**   | US-001-01-RegistroEmpresa                  |
| **Roles Implicados**   | Backend, DevOps                            |
| **Tipo**               | Infra                                      |
| **Categoría/Tags**     | Email, Integration, Templates              |
| **Estimación Técnica** | 2 SP (Fibonacci)                           |
| **Prioridad**          | Media                                      |

---

## Contexto (Product Owner)

Para cumplir el criterio de aceptación **"Escenario 1: El usuario recibe un email de bienvenida con instrucciones iniciales"**, necesitamos integrar un proveedor de email transaccional (SendGrid, Mailgun, Resend) que:
1. Envíe emails de forma confiable (alta deliverability).
2. Permita plantillas HTML personalizadas.
3. Provea tracking de entregas y aperturas.

---

## Especificación Técnica (Tech Lead)

### Objetivo
Configurar SendGrid (opción recomendada) para envío de emails transaccionales desde LTI.

### Configuración de SendGrid

**1. Crear cuenta en SendGrid:**
- Plan Free: 100 emails/día (suficiente para MVP)
- Plan Essentials: 40,000 emails/mes si se escala

**2. Generar API Key:**
- Dashboard → Settings → API Keys → Create API Key
- Permisos: **Mail Send** (Full Access)
- Guardar API Key de forma segura

**3. Variables de entorno:**

```env
# .env
SENDGRID_API_KEY=SG.xxxxxxxxxxxxxxxxxxxxx
SENDGRID_FROM_EMAIL=no-reply@lti-platform.com
SENDGRID_FROM_NAME=LTI Platform
```

**4. Verificar dominio (Sender Authentication):**
- Dashboard → Settings → Sender Authentication → Verify Single Sender
- Verificar email `no-reply@lti-platform.com` (o dominio completo con DNS records)

### Implementación Backend (Node.js + SendGrid SDK)

**Archivo:** `src/infrastructure/email/sendgrid.service.ts`

```typescript
import sgMail from '@sendgrid/mail';

sgMail.setApiKey(process.env.SENDGRID_API_KEY!);

interface SendEmailParams {
  to: string;
  subject: string;
  html: string;
}

export async function sendEmail({ to, subject, html }: SendEmailParams): Promise<void> {
  const msg = {
    to,
    from: {
      email: process.env.SENDGRID_FROM_EMAIL!,
      name: process.env.SENDGRID_FROM_NAME!,
    },
    subject,
    html,
  };

  try {
    await sgMail.send(msg);
    console.log(`Email sent successfully to ${to}`);
  } catch (error: any) {
    console.error('Error sending email:', error.response?.body || error.message);
    throw new Error('Failed to send email');
  }
}
```

**Archivo:** `src/infrastructure/email/templates/welcome.template.ts`

```typescript
interface WelcomeEmailParams {
  companyName: string;
  adminName: string;
  loginUrl: string;
}

export function getWelcomeEmailHTML(params: WelcomeEmailParams): string {
  return `
    <!DOCTYPE html>
    <html lang="es">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Bienvenido a LTI</title>
      <style>
        body { font-family: Arial, sans-serif; line-height: 1.6; color: #333; }
        .container { max-width: 600px; margin: 0 auto; padding: 20px; }
        .header { background-color: #2563eb; color: white; padding: 20px; text-align: center; }
        .content { background-color: #f9fafb; padding: 30px; }
        .button { 
          display: inline-block; 
          background-color: #2563eb; 
          color: white; 
          padding: 12px 24px; 
          text-decoration: none; 
          border-radius: 6px; 
          margin: 20px 0;
        }
        .footer { text-align: center; color: #6b7280; font-size: 12px; margin-top: 20px; }
      </style>
    </head>
    <body>
      <div class="container">
        <div class="header">
          <h1>¡Bienvenido a LTI!</h1>
        </div>
        <div class="content">
          <p>Hola <strong>${params.adminName}</strong>,</p>
          <p>
            ¡Felicitaciones! Has creado exitosamente la cuenta de <strong>${params.companyName}</strong> 
            en LTI, la plataforma de orquestación de talento con IA y automatización no-code.
          </p>
          <p><strong>Próximos pasos:</strong></p>
          <ol>
            <li>Completa la configuración de tu perfil de administrador</li>
            <li>Invita a tu equipo de reclutamiento</li>
            <li>Crea tu primera oferta de trabajo</li>
            <li>Configura automatizaciones básicas para ahorrar tiempo</li>
          </ol>
          <p style="text-align: center;">
            <a href="${params.loginUrl}" class="button">Acceder a mi cuenta</a>
          </p>
          <p>
            Si tienes preguntas, no dudes en contactarnos en 
            <a href="mailto:support@lti-platform.com">support@lti-platform.com</a>
          </p>
          <p>¡Estamos emocionados de ayudarte a optimizar tu reclutamiento!</p>
          <p>El equipo de LTI</p>
        </div>
        <div class="footer">
          <p>LTI Platform © 2025 | <a href="https://lti-platform.com">lti-platform.com</a></p>
          <p>Este es un email automático, por favor no respondas directamente.</p>
        </div>
      </div>
    </body>
    </html>
  `;
}
```

**Integración en Use Case:**

**Archivo:** `src/application/useCases/CreateCompanyUseCase.ts` (modificado)

```typescript
import { sendEmail } from '../../infrastructure/email/sendgrid.service';
import { getWelcomeEmailHTML } from '../../infrastructure/email/templates/welcome.template';

export class CreateCompanyUseCase {
  // ... código existente ...
  
  async execute(data: CreateCompanyDTO): Promise<Company> {
    // Crear empresa (código existente)
    const company = await this.companyRepository.create({
      name: data.name,
      domain: data.domain,
      status: 'ACTIVE',
    });
    
    // Enviar email de bienvenida (asíncrono, no bloquear response)
    this.sendWelcomeEmail(company, data.adminEmail);
    
    return company;
  }
  
  private async sendWelcomeEmail(company: Company, adminEmail: string): Promise<void> {
    try {
      const html = getWelcomeEmailHTML({
        companyName: company.name,
        adminName: adminEmail.split('@')[0], // Temporal, mejorar con nombre real
        loginUrl: `${process.env.APP_URL}/auth/setup-admin`,
      });
      
      await sendEmail({
        to: adminEmail,
        subject: `¡Bienvenido a LTI, ${company.name}!`,
        html,
      });
    } catch (error) {
      console.error('Failed to send welcome email:', error);
      // No lanzar excepción: el registro de la empresa debe completarse aunque falle el email
    }
  }
}
```

---

## Validación (Arquitecto de Software)

### Consideraciones de Seguridad

✅ **API Key en variable de entorno**: Nunca hardcodear en código.

✅ **Sender verification**: SendGrid requiere verificación de dominio para evitar spoofing.

⚠️ **Rate limiting**: SendGrid Free plan tiene límite de 100 emails/día, monitorear uso.

### Consideraciones de Performance

✅ **Email asíncrono**: No bloquear el response del endpoint mientras se envía email.

⚠️ **Job queue recomendado**: En producción, usar Bull/BullMQ para emails:

```typescript
import { Queue } from 'bull';

const emailQueue = new Queue('emails', { redis: { host: 'localhost', port: 6379 } });

emailQueue.process(async (job) => {
  await sendEmail(job.data);
});

// En use case:
await emailQueue.add({ to: adminEmail, subject: '...', html: '...' });
```

### Consideraciones de Observabilidad

✅ **Logging de errores**: Console.error captura fallos de envío.

⚠️ **Tracking de entregas**: SendGrid Event Webhook para monitorear bounces, opens, clicks.

### Deuda Técnica a Evitar

❌ **No usar SMTP directamente**: SendGrid API es más confiable que SMTP.

❌ **No enviar emails en proceso síncrono**: Usar queue para evitar timeouts.

---

## Definición de Hecho (DoD)

- [ ] Cuenta de SendGrid creada y API Key generada
- [ ] Dominio verificado en SendGrid (Sender Authentication)
- [ ] Variables de entorno configuradas en `.env`
- [ ] Servicio `sendgrid.service.ts` implementado
- [ ] Plantilla HTML de email de bienvenida creada
- [ ] Integración en `CreateCompanyUseCase` implementada
- [ ] Email enviado correctamente tras registro de empresa (test manual)
- [ ] Email aparece en inbox (no spam) con formato correcto
- [ ] Logs de errores de email configurados
- [ ] Documentación de configuración de SendGrid en README.md

---

## Notas Adicionales

**Dependencias de package.json:**

```json
{
  "dependencies": {
    "@sendgrid/mail": "^8.1.0"
  }
}
```

**Alternativa: Resend (más moderna, mejor DX):**

```typescript
import { Resend } from 'resend';

const resend = new Resend(process.env.RESEND_API_KEY);

await resend.emails.send({
  from: 'no-reply@lti-platform.com',
  to: adminEmail,
  subject: 'Bienvenido a LTI',
  html: welcomeEmailHTML,
});
```

**Mejora futura (Dynamic Templates de SendGrid):**

En lugar de HTML hardcodeado, usar Dynamic Templates en SendGrid Dashboard:
```typescript
await sgMail.send({
  to: adminEmail,
  from: 'no-reply@lti-platform.com',
  templateId: 'd-abcd1234efgh5678', // ID del template en SendGrid
  dynamicTemplateData: {
    companyName: company.name,
    adminName: adminName,
    loginUrl: loginUrl,
  },
});
```
