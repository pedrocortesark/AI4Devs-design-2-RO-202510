# Backlog de Producto — LTI Platform (Opción Estándar)

> **Documento:** Backlog simplificado con User Stories principales siguiendo criterio INVEST  
> **Fecha de generación:** 2025-12-02  
> **Contexto:** Basado en análisis de mercado ATS, modelo de datos, casos de uso y arquitectura documentados en `LTI-PCN.md` y `UserStories-PCN.md`

---

## Introducción

Este backlog sintetiza las **18 User Stories principales de la Fase 1 (MVP)** de LTI, una plataforma de orquestación de talento que combina ATS moderno, automatización no-code e IA operativa. Cada historia cumple con el criterio **INVEST**:

- **I**ndependent: Puede desarrollarse sin depender estrictamente de otras (salvo dependencias técnicas explícitas)
- **N**egotiable: El detalle se puede ajustar en refinement
- **V**aluable: Aporta valor directo a usuarios (recruiters, hiring managers, candidatos)
- **E**stimable: Incluye esfuerzo estimado en Story Points
- **S**mall: Completable en 1-2 semanas por un equipo pequeño
- **T**estable: Criterios de aceptación verificables

**Segmento objetivo:** Startups y scale-ups tecnológicas (50-1000 empleados)  
**Roles de usuario:** Recruiter, Hiring Manager, Admin, Candidato

---

## US-001-01 — Registro de Empresa

**Título:** Registro de Empresa (Company)

**Descripción:**  
Como **administrador de una nueva empresa que quiere usar LTI**  
Quiero **registrar mi empresa en la plataforma**  
Para **crear un tenant aislado donde mi equipo pueda gestionar procesos de reclutamiento**

**Criterios de Aceptación:**

1. **Registro exitoso:** El administrador puede completar un formulario con nombre de empresa, dominio y aceptación de términos, creando un registro en la tabla `Company` con estado activo y recibiendo un email de bienvenida.

2. **Validación de dominio único:** Si el dominio ya está registrado por otra empresa, el sistema muestra un error específico y no permite duplicados.

3. **Integración con Auth:** El sistema crea un registro en el proveedor de autenticación (Auth0/Clerk) asociando el `user_id` externo con el primer usuario Admin de la empresa.

**Estimación:** 8 SP (ajustado tras análisis técnico)  
**Valor de Negocio:** Alto (5/5) — Bloqueante fundacional  
**Dependencias:** Configuración de Auth0/Clerk y SendGrid

---

## US-001-02 — Login Multi-Tenant

**Título:** Login de Usuario con Autenticación Multi-Tenant

**Descripción:**  
Como **usuario registrado de una empresa en LTI**  
Quiero **iniciar sesión de forma segura**  
Para **acceder únicamente a los datos y funcionalidades de mi empresa (multi-tenancy)**

**Criterios de Aceptación:**

1. **Login exitoso:** El usuario puede iniciar sesión con email y contraseña (o SSO), obteniendo un JWT con custom claims de `company_id` que filtra todas las queries posteriores.

2. **Multi-tenancy seguro:** El sistema valida que todas las peticiones al backend incluyan el `company_id` del token y bloquea acceso a datos de otras empresas.

3. **Redirección post-login:** Tras autenticación exitosa, el usuario es redirigido al dashboard principal con sus permisos aplicados según rol (Recruiter, Hiring Manager, Admin).

**Estimación:** 8 SP (hereda complejidad de Auth0)  
**Valor de Negocio:** Alto (5/5) — Crítico para seguridad  
**Dependencias:** US-001-01 (Registro de Empresa)

---

## US-001-03 — Invitación de Usuarios

**Título:** Invitación de Usuarios por Admin

**Descripción:**  
Como **administrador de una empresa en LTI**  
Quiero **invitar a miembros de mi equipo de reclutamiento**  
Para **habilitar colaboración entre recruiters y hiring managers en la plataforma**

**Criterios de Aceptación:**

1. **Envío de invitación:** El admin puede ingresar emails de usuarios a invitar, asignarles un rol (Recruiter, Hiring Manager) y enviar invitaciones por email con un enlace temporal de activación.

2. **Aceptación de invitación:** El usuario invitado puede hacer clic en el enlace, completar su registro (contraseña, nombre completo) y quedar asociado automáticamente a la empresa correcta.

3. **Gestión de permisos:** Los usuarios invitados solo pueden acceder a datos de su empresa y ejecutar acciones permitidas por su rol asignado.

**Estimación:** 5 SP (requiere tokens temporales + SendGrid)  
**Valor de Negocio:** Medio-Alto (4/5) — Habilita colaboración  
**Dependencias:** US-001-01, US-001-02

---

## US-002-01 — Crear Oferta con IA

**Título:** Crear Oferta de Trabajo con Asistencia de IA

**Descripción:**  
Como **recruiter**  
Quiero **crear una nueva oferta de trabajo con ayuda de IA para mejorar la descripción**  
Para **atraer candidatos de calidad y ahorrar tiempo en redacción de textos**

**Criterios de Aceptación:**

1. **Creación básica:** El recruiter puede completar un formulario con título, descripción inicial, requisitos, ubicación y tipo de contrato, guardando la oferta como borrador.

2. **Asistencia de IA:** El recruiter puede solicitar al sistema que mejore la descripción usando IA (OpenAI/Claude), obteniendo una versión optimizada que puede aceptar o editar antes de guardar.

3. **Asignación de responsable:** El recruiter puede seleccionar un Hiring Manager como responsable de la oferta, quien recibirá notificaciones para aprobarla antes de publicación.

**Estimación:** 13 SP (migraciones multi-tenant + IA + permisos)  
**Valor de Negocio:** Alto (5/5) — Core ATS + diferenciador IA  
**Dependencias:** US-001-02, configuración de OpenAI/Claude API

---

## US-002-02 — Revisión y Aprobación de Oferta

**Título:** Revisión y Aprobación de Oferta por Hiring Manager

**Descripción:**  
Como **hiring manager**  
Quiero **revisar y aprobar ofertas creadas por recruiters**  
Para **asegurar que el contenido refleja correctamente las necesidades de mi equipo antes de publicarlas**

**Criterios de Aceptación:**

1. **Notificación de revisión:** El hiring manager recibe una notificación (email + in-app) cuando un recruiter envía una oferta a revisión.

2. **Aprobación o rechazo:** El hiring manager puede revisar la oferta y aprobarla (cambiando estado a "Aprobada") o solicitar cambios (devolviendo a "Borrador en revisión" con comentarios).

3. **Trazabilidad:** El sistema registra el historial de aprobaciones y cambios solicitados en la oferta para auditoría.

**Estimación:** 5 SP (workflow colaborativo estándar)  
**Valor de Negocio:** Medio-Alto (4/5) — Necesario antes de publicar  
**Dependencias:** US-002-01, US-001-03

---

## US-002-03 — Publicar Oferta y Activar Automatizaciones

**Título:** Publicar Oferta y Activar Automatizaciones Iniciales

**Descripción:**  
Como **recruiter**  
Quiero **publicar una oferta aprobada y activar automatizaciones iniciales**  
Para **comenzar a recibir candidaturas y gestionar el proceso con reglas predefinidas**

**Criterios de Aceptación:**

1. **Publicación exitosa:** El recruiter puede publicar una oferta aprobada, cambiando su estado a "Publicada" y haciéndola visible para recibir aplicaciones.

2. **Activación de pipeline:** El sistema crea automáticamente las etapas del pipeline configuradas (ej: Applied, Screening, Interview, Offer, Rejected) asociadas a la oferta.

3. **Automatizaciones iniciales:** El motor de automatización evalúa reglas predefinidas (ej: enviar email de confirmación a candidatos, notificar al hiring manager) y las activa tras la publicación.

**Estimación:** 5 SP (evento de dominio + automatización)  
**Valor de Negocio:** Alto (5/5) — Bloquea motor de automatización  
**Dependencias:** US-002-02, US-005-02 (Motor de Ejecución)

---

## US-003-01 — Registro Manual de Candidato

**Título:** Registro Manual de Candidato

**Descripción:**  
Como **recruiter**  
Quiero **registrar manualmente un candidato en el sistema**  
Para **agregar perfiles interesantes que encontré por sourcing activo (LinkedIn, referidos) y vincularlos a ofertas**

**Criterios de Aceptación:**

1. **Creación de perfil:** El recruiter puede ingresar datos básicos del candidato (nombre, email, teléfono, LinkedIn) y subir su CV en formato PDF/DOCX a S3.

2. **Validación de duplicados:** El sistema valida que no exista otro candidato con el mismo email dentro de la misma empresa antes de crear el registro.

3. **Asociación a empresa:** El candidato queda asociado al `company_id` del recruiter para garantizar aislamiento multi-tenant.

**Estimación:** 8 SP (migraciones + S3 + permisos)  
**Valor de Negocio:** Alto (5/5) — Core ATS  
**Dependencias:** US-001-02, configuración de AWS S3

---

## US-003-02 — Vincular Candidato a Oferta

**Título:** Vincular Candidato a Oferta (Application)

**Descripción:**  
Como **recruiter**  
Quiero **vincular un candidato existente a una oferta publicada**  
Para **crear una candidatura (Application) y comenzar el proceso de evaluación en el pipeline**

**Criterios de Aceptación:**

1. **Creación de Application:** El recruiter puede seleccionar un candidato y una oferta activa, creando un registro de candidatura en estado inicial (ej: "Applied").

2. **Vinculación automática al pipeline:** La candidatura se asocia automáticamente a la primera etapa del pipeline de la oferta (ej: "Screening").

3. **Notificación opcional:** Si está configurado, el sistema puede enviar un email de confirmación al candidato informando que su perfil está siendo revisado.

**Estimación:** 3 SP (operación simple de asociación)  
**Valor de Negocio:** Alto (5/5) — Bloquea pipeline  
**Dependencias:** US-003-01, US-002-03

---

## US-003-03 — Vista de Perfil de Candidato

**Título:** Vista de Perfil de Candidato

**Descripción:**  
Como **recruiter o hiring manager**  
Quiero **visualizar el perfil completo de un candidato**  
Para **revisar su CV, historial de interacciones, feedback recibido y tomar decisiones informadas**

**Criterios de Aceptación:**

1. **Información básica:** La vista muestra datos del candidato (nombre, email, teléfono, LinkedIn, ubicación, CV descargable).

2. **Historial de candidaturas:** Se lista el historial de ofertas a las que el candidato ha aplicado, su estado actual y etapas previas.

3. **Feedback asociado:** Si existen evaluaciones o comentarios de recruiters/managers, se muestran en la ficha del candidato para contexto.

**Estimación:** 3 SP (UI con datos existentes)  
**Valor de Negocio:** Medio-Alto (4/5) — UX crítica  
**Dependencias:** US-003-01, US-003-02

---

## US-004-01 — Vista Kanban del Pipeline

**Título:** Vista Kanban del Pipeline de Candidatos

**Descripción:**  
Como **recruiter o hiring manager**  
Quiero **visualizar el pipeline de una oferta en formato kanban**  
Para **tener una vista rápida del estado de todos los candidatos y priorizar acciones**

**Criterios de Aceptación:**

1. **Columnas por etapa:** El sistema muestra columnas para cada etapa del pipeline (Applied, Screening, Interview, Offer, Rejected) con tarjetas de candidatos.

2. **Tarjetas de candidato:** Cada tarjeta muestra nombre, foto (opcional), scoring de IA (si disponible) y etiquetas relevantes (ej: "Match alto", "Urgente").

3. **Orden inteligente:** Los candidatos dentro de cada columna se ordenan según scoring de IA o criterios configurables (fecha de aplicación, prioridad manual).

**Estimación:** 8 SP (UI compleja con real-time)  
**Valor de Negocio:** Alto (5/5) — Diferenciador UX principal  
**Dependencias:** US-003-02, US-006-02 (Scoring IA)

---

## US-004-02 — Mover Candidatos (Drag & Drop)

**Título:** Mover Candidatos entre Etapas (Drag & Drop)

**Descripción:**  
Como **recruiter**  
Quiero **mover candidatos entre etapas del pipeline arrastrando y soltando sus tarjetas**  
Para **actualizar el estado del proceso de forma visual e intuitiva**

**Criterios de Aceptación:**

1. **Drag & drop funcional:** El recruiter puede arrastrar una tarjeta de candidato de una columna a otra, actualizando su etapa en tiempo real.

2. **Evento de cambio de etapa:** El sistema registra el cambio en `ApplicationStageHistory` y emite un evento de dominio (`ApplicationStageChanged`).

3. **Automatizaciones disparadas:** Si existen reglas de automatización configuradas para el cambio de etapa (ej: enviar email al candidato), se ejecutan inmediatamente.

**Estimación:** 8 SP (drag & drop + eventos + automatización)  
**Valor de Negocio:** Alto (5/5) — Bloquea automatizaciones  
**Dependencias:** US-004-01, US-005-02

---

## US-004-03 — Historial de Cambios de Etapa

**Título:** Historial de Cambios de Etapa

**Descripción:**  
Como **recruiter o hiring manager**  
Quiero **ver el historial completo de cambios de etapa de un candidato**  
Para **entender su progreso en el proceso y detectar posibles bloqueos o retrasos**

**Criterios de Aceptación:**

1. **Timeline de cambios:** El sistema muestra una línea de tiempo con todos los cambios de etapa del candidato (fecha, etapa origen, etapa destino, usuario responsable).

2. **Tiempo en cada etapa:** Se calcula y muestra el tiempo transcurrido en cada etapa (ej: "5 días en Screening", "2 días en Interview").

3. **Accesibilidad:** El historial es visible tanto desde el perfil del candidato como desde la vista del pipeline al hacer clic en una tarjeta.

**Estimación:** 5 SP (query + UI timeline)  
**Valor de Negocio:** Medio (3/5) — Transparencia y auditoría  
**Dependencias:** US-004-02

---

## US-005-01 — Builder Visual de Automatizaciones

**Título:** Builder Visual de Reglas de Automatización

**Descripción:**  
Como **recruiter o admin**  
Quiero **configurar reglas de automatización mediante un editor visual no-code**  
Para **orquestar flujos complejos (emails, notificaciones, cambios de estado) sin depender de IT**

**Criterios de Aceptación:**

1. **Editor drag & drop:** El sistema provee un builder visual donde el usuario puede arrastrar disparadores (ej: "Candidato cambia a etapa X") y acciones (ej: "Enviar email Y", "Notificar a hiring manager").

2. **Configuración de acciones:** Cada acción permite configurar parámetros específicos (plantilla de email, destinatarios, mensaje de notificación, etapa destino).

3. **Guardado de reglas:** Las reglas creadas se guardan como `AutomationRule` + `AutomationAction` en la base de datos y quedan activas para ejecución por el motor.

**Estimación:** 13 SP (UI compleja + lógica de builder)  
**Valor de Negocio:** Alto (5/5) — Core differentiator  
**Dependencias:** US-002-03, US-004-02

---

## US-005-02 — Motor de Ejecución de Automatizaciones

**Título:** Motor de Ejecución de Reglas de Automatización

**Descripción:**  
Como **sistema LTI**  
Quiero **ejecutar automáticamente las reglas configuradas cuando ocurran eventos de dominio**  
Para **reducir trabajo manual de recruiters y garantizar consistencia en comunicaciones y procesos**

**Criterios de Aceptación:**

1. **Escucha de eventos:** El motor se suscribe a eventos de dominio (ej: `ApplicationStageChanged`, `JobPostingPublished`, `DecisionFinalized`) emitidos por el Core ATS.

2. **Evaluación de reglas:** Cuando ocurre un evento, el motor consulta las reglas activas aplicables (por `company_id`, `job_posting_id`, tipo de disparador) y evalúa sus condiciones.

3. **Ejecución de acciones:** Si las condiciones se cumplen, el motor ejecuta las acciones configuradas (enviar emails via SendGrid, notificar via Slack, actualizar estado) y registra el resultado en `AutomationEvent`.

**Estimación:** 13 SP (arquitectura event-driven + lógica compleja)  
**Valor de Negocio:** Alto (5/5) — Sin motor, el builder no funciona  
**Dependencias:** US-005-01, US-002-03

---

## US-005-03 — Logs y Monitoreo de Automatizaciones

**Título:** Logs y Monitoreo de Automatizaciones

**Descripción:**  
Como **recruiter o admin**  
Quiero **visualizar logs de ejecución de automatizaciones**  
Para **verificar que las reglas funcionan correctamente y debuggear problemas (emails no enviados, notificaciones fallidas)**

**Criterios de Aceptación:**

1. **Listado de ejecuciones:** El sistema muestra una tabla con todas las ejecuciones de automatizaciones (regla ejecutada, entidad afectada, fecha, estado: SUCCESS/FAILED).

2. **Detalle de errores:** Si una ejecución falló, se muestra el mensaje de error específico (ej: "SendGrid API timeout", "Destinatario inválido").

3. **Filtros y búsqueda:** El usuario puede filtrar logs por oferta, candidato, regla específica, rango de fechas o estado de ejecución.

**Estimación:** 5 SP (UI + queries sobre AutomationEvent)  
**Valor de Negocio:** Medio-Alto (4/5) — Debugging y confianza  
**Dependencias:** US-005-02

---

## US-006-01 — Análisis de CV con IA

**Título:** Análisis de CV con IA

**Descripción:**  
Como **recruiter**  
Quiero **que la IA analice automáticamente el CV de un candidato**  
Para **obtener un resumen de skills, experiencia y ajuste al rol sin leer el documento completo**

**Criterios de Aceptación:**

1. **Extracción automática:** Cuando se sube un CV (en US-003-01), el sistema lo envía al servicio de IA (OpenAI/Claude) para extraer información estructurada (skills, años de experiencia, educación, empresas previas).

2. **Resumen en lenguaje natural:** La IA genera un resumen de 3-5 puntos destacando fortalezas del candidato y cómo encajan con el perfil de la oferta (si está asociado).

3. **Almacenamiento de insights:** Los resultados del análisis se guardan en `AIInsight` asociados a la candidatura para consultas posteriores.

**Estimación:** 8 SP (integración IA + parsing de PDFs)  
**Valor de Negocio:** Alto (5/5) — Core differentiador IA  
**Dependencias:** US-003-01, configuración de OpenAI/Claude

---

## US-006-02 — Scoring de Candidatos con IA

**Título:** Scoring de Candidatos con IA

**Descripción:**  
Como **recruiter o hiring manager**  
Quiero **que la IA asigne un score (0-100) a cada candidato según su ajuste al rol**  
Para **priorizar automáticamente a quién revisar primero y reducir sesgo subjetivo**

**Criterios de Aceptación:**

1. **Scoring automático:** El sistema calcula un score para cada candidato comparando su CV con los requisitos de la oferta usando IA (análisis semántico de skills, experiencia, keywords).

2. **Visualización en pipeline:** El score se muestra en la tarjeta del candidato en la vista kanban (ej: badge con "Match: 85%") y permite ordenar candidatos por score.

3. **Explicabilidad:** Al hacer clic en el score, se muestra un desglose de factores que contribuyeron a la puntuación (ej: "Skills técnicas: 90%, Experiencia: 80%, Educación: 70%").

**Estimación:** 8 SP (modelo de scoring + UI)  
**Valor de Negocio:** Alto (5/5) — Ahorro masivo de tiempo  
**Dependencias:** US-006-01, US-004-01

---

## US-006-03 — Sugerencias de Siguiente Paso con IA

**Título:** Sugerencias de Siguiente Paso con IA

**Descripción:**  
Como **recruiter**  
Quiero **que la IA sugiera el siguiente paso más adecuado para cada candidato**  
Para **tomar decisiones más rápidas y basadas en patrones de éxito históricos**

**Criterios de Aceptación:**

1. **Sugerencia contextual:** Para cada candidato en el pipeline, la IA sugiere una acción (ej: "Mover a entrevista técnica", "Solicitar referencias", "Rechazar cortésmente") basada en su score, tiempo en etapa y datos históricos.

2. **Explicación de la sugerencia:** El sistema muestra una justificación breve de la sugerencia (ej: "Candidato con perfil similar tuvo éxito en procesos previos").

3. **Acción con un clic:** El recruiter puede aceptar la sugerencia con un solo clic, ejecutando automáticamente la acción propuesta (cambio de etapa, envío de email, etc.).

**Estimación:** 8 SP (modelo predictivo + integración con acciones)  
**Valor de Negocio:** Medio-Alto (4/5) — IA proactiva  
**Dependencias:** US-006-02, US-004-02

---

## Resumen de Story Points

**Total de Story Points del MVP (18 User Stories):** 129 SP

**Distribución por épica:**
- EPIC-001 (Usuarios): 21 SP (3 historias)
- EPIC-002 (Ofertas): 23 SP (3 historias)
- EPIC-003 (Candidatos): 14 SP (3 historias)
- EPIC-004 (Pipeline): 21 SP (3 historias)
- EPIC-005 (Automatización): 31 SP (3 historias)
- EPIC-006 (IA Operativa): 24 SP (3 historias)

**Estimación de duración:** 6-7 sprints (~3-3.5 meses) con un equipo de 2 developers full-time (velocidad estimada: 20-25 SP/sprint)

---

## Notas de Validación INVEST

Todas las historias cumplen con el criterio INVEST:

✅ **Independent:** Cada historia puede desarrollarse de forma autónoma (salvo dependencias técnicas explícitas documentadas)  
✅ **Negotiable:** El detalle de implementación puede ajustarse en refinement sin cambiar el valor de negocio  
✅ **Valuable:** Cada historia aporta valor directo a usuarios finales o habilita capacidades críticas del sistema  
✅ **Estimable:** Story Points asignados tras descomposición técnica y análisis de complejidad  
✅ **Small:** Completables en 1-2 semanas por un equipo pequeño (salvo historias de 13 SP que pueden requerir pair programming)  
✅ **Testable:** Criterios de aceptación verificables con tests automatizados (unitarios, integración, E2E)

---

## Próximos Pasos

1. **Refinement Session:** Revisar este backlog con el equipo de desarrollo para validar estimaciones y detectar riesgos técnicos adicionales.
2. **Sprint Planning:** Seleccionar las primeras 3-4 historias (US-001-01, US-001-02, US-001-03) para el Sprint 1.
3. **Descomposición en Tasks:** Generar tickets técnicos ejecutables para cada historia seleccionada (ver ejemplo en US-001-01).
4. **Preparación de Entorno:** Configurar Auth0/Clerk, SendGrid, AWS S3, OpenAI API antes del inicio del desarrollo.

---

*Documento generado automáticamente desde la documentación maestra de LTI Platform*  
*Última actualización: 2025-12-02*
