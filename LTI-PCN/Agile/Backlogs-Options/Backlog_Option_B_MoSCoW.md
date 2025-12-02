# Backlog de Producto — LTI Platform (Opción MoSCoW)

> **Documento:** Backlog priorizado mediante matriz MoSCoW con justificación estratégica  
> **Guardar en:** `Agile/Backlogs-Options/Backlog_Option_B_MoSCoW.md`  
> **Fecha de generación:** 2025-12-02  
> **Contexto:** Basado en análisis estratégico de mercado ATS, posicionamiento competitivo y objetivos del PRD documentados en `LTI-PCN.md` y `UserStories-PCN.md`

---

## Introducción al Framework MoSCoW

La matriz **MoSCoW** es una técnica de priorización estratégica que clasifica funcionalidades en cuatro categorías según su impacto en el éxito del MVP y la viabilidad del negocio:

- **Must Have (Debe Tener):** Funcionalidades sin las cuales el producto no puede lanzarse. Son **bloqueantes absolutos** para el MVP. Sin ellas, LTI no cumple su propuesta de valor mínima ni valida su diferenciación en el mercado.

- **Should Have (Debería Tener):** Funcionalidades importantes que añaden valor significativo y reducen riesgos de adopción, pero el producto puede lanzarse sin ellas en una primera versión. Se priorizan para entrega inmediatamente después del MVP.

- **Could Have (Podría Tener):** Funcionalidades deseables que mejoran la experiencia o amplían capacidades, pero no son críticas para validación inicial. Se consideran para iteraciones posteriores basándose en feedback de early adopters.

- **Won't Have (No Tendrá - por ahora):** Funcionalidades que, aunque valiosas a largo plazo, quedan explícitamente fuera del alcance del MVP y primeras iteraciones. Se posponen para fases de escalabilidad o madurez del producto.

---

## Objetivos Estratégicos del PRD (Fuente: LTI-PCN.md)

Para garantizar que la priorización MoSCoW esté alineada con la estrategia de negocio, recordamos los **objetivos fundacionales** de LTI:

1. **Diferenciación Inmediata:** Posicionar LTI como "Talent Orchestration Platform", no como "otro ATS más". Esto requiere demostrar desde el día 1 la **automatización no-code** y la **IA operativa** embebida en el flujo de trabajo.

2. **Validación de Propuesta de Valor:** El MVP debe permitir a startups tecnológicas (50-300 empleados) **reducir el trabajo manual de recruiters** en al menos 40% mediante automatizaciones y recomendaciones de IA.

3. **Feedback Temprano con Early Adopters:** El producto debe ser suficientemente funcional para ejecutar procesos de reclutamiento completos (desde oferta hasta decisión final) y capturar métricas de éxito real.

4. **Base Técnica Escalable:** La arquitectura multi-tenant, autenticación segura y eventos de dominio deben estar implementados desde el inicio para evitar refactorizaciones costosas.

5. **Experiencia de Usuario Competitiva:** La UX del pipeline visual (kanban, drag & drop) debe ser **visualmente superior** a ATS tradicionales para generar "wow effect" en demos y pruebas piloto.

---

## Backlog Priorizado — Matriz MoSCoW

---

## 🔴 MUST HAVE — Funcionalidades Bloqueantes del MVP

Estas funcionalidades son **absolutamente críticas** para lanzar el MVP de LTI. Sin ellas, el producto no puede ejecutar su propuesta de valor fundamental ni diferenciarse de ATS tradicionales.

---

### **US-001-01 — Registro de Empresa (Company)**

**Categoría:** Must Have  
**Estimación:** 8 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-001-GestionUsuarios

**Descripción:**  
Como **administrador de una nueva empresa que quiere usar LTI**  
Quiero **registrar mi empresa en la plataforma**  
Para **crear un tenant aislado donde mi equipo pueda gestionar procesos de reclutamiento**

**Justificación Estratégica:**  
🎯 **Bloqueante técnico absoluto.** Sin arquitectura multi-tenant no existe producto SaaS escalable. Esta historia establece el modelo de datos fundacional (tabla `Company`, aislamiento por `company_id`) y la integración con Auth0/Clerk que heredan todas las demás funcionalidades. Sin esta base, no se puede garantizar seguridad, compliance ni escalabilidad futura. Es la piedra angular del sistema.

**Criterios de Aceptación (resumen):**
1. Registro exitoso con email de bienvenida
2. Validación de dominio único
3. Integración con proveedor de autenticación (Auth0/Clerk)

**Dependencias:** Configuración de Auth0/Clerk, SendGrid

---

### **US-001-02 — Login Multi-Tenant**

**Categoría:** Must Have  
**Estimación:** 8 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-001-GestionUsuarios

**Descripción:**  
Como **usuario registrado de una empresa en LTI**  
Quiero **iniciar sesión de forma segura**  
Para **acceder únicamente a los datos y funcionalidades de mi empresa (multi-tenancy)**

**Justificación Estratégica:**  
🎯 **Bloqueante de seguridad y compliance.** Un ATS gestiona datos personales sensibles (CVs, historial de candidatos, decisiones de contratación). La autenticación multi-tenant con JWT y custom claims de `company_id` garantiza aislamiento absoluto entre empresas y previene violaciones de privacidad catastróficas. Sin esto, LTI no es legalmente viable ni certificable (GDPR, SOC2). Además, habilita el control de acceso basado en roles (RBAC) necesario para todas las historias posteriores.

**Criterios de Aceptación (resumen):**
1. Login exitoso con JWT y custom claims de `company_id`
2. Multi-tenancy seguro: todas las queries filtradas por empresa
3. Redirección al dashboard con permisos aplicados

**Dependencias:** US-001-01 (Registro de Empresa)

---

### **US-002-01 — Crear Oferta con IA**

**Categoría:** Must Have  
**Estimación:** 13 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-002-GestionOfertas

**Descripción:**  
Como **recruiter**  
Quiero **crear una nueva oferta de trabajo con ayuda de IA para mejorar la descripción**  
Para **atraer candidatos de calidad y ahorrar tiempo en redacción de textos**

**Justificación Estratégica:**  
🎯 **Primer diferenciador visible frente a ATS tradicionales.** Esta historia demuestra el concepto de "IA operativa embebida" desde el primer uso del producto. La asistencia de IA para mejorar descripciones de ofertas no es un "nice to have", sino el **mensaje central del pitch de ventas**: "LTI te ayuda a trabajar mejor, no solo a organizar datos". Sin esta funcionalidad, LTI se percibe como un ATS genérico más. Además, habilita el dominio core (tabla `JobPosting`) necesario para todas las funcionalidades posteriores de pipeline y candidatos.

**Criterios de Aceptación (resumen):**
1. Creación de oferta con formulario básico (guardar como borrador)
2. Asistencia de IA: mejora de descripción usando OpenAI/Claude
3. Asignación de Hiring Manager responsable

**Dependencias:** US-001-02, configuración de OpenAI/Claude API

---

### **US-003-01 — Registro Manual de Candidato**

**Categoría:** Must Have  
**Estimación:** 8 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-003-GestionCandidatos

**Descripción:**  
Como **recruiter**  
Quiero **registrar manualmente un candidato en el sistema**  
Para **agregar perfiles interesantes que encontré por sourcing activo (LinkedIn, referidos) y vincularlos a ofertas**

**Justificación Estratégica:**  
🎯 **Sin candidatos no hay ATS.** Esta historia establece el segundo dominio core del producto (tabla `Candidate`), permitiendo ingresar el "combustible" del proceso de reclutamiento. Incluye integración con AWS S3 para almacenamiento de CVs, un requerimiento técnico esencial para cualquier ATS moderno. Sin esta funcionalidad, no se puede ejecutar ningún proceso de evaluación ni validar el resto del flujo (pipeline, scoring IA, automatizaciones). Es bloqueante absoluto para las demos y pilotos con clientes.

**Criterios de Aceptación (resumen):**
1. Creación de perfil con datos básicos + subida de CV a S3
2. Validación de duplicados por email
3. Asociación automática a empresa (multi-tenancy)

**Dependencias:** US-001-02, configuración de AWS S3

---

### **US-003-02 — Vincular Candidato a Oferta (Application)**

**Categoría:** Must Have  
**Estimación:** 3 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-003-GestionCandidatos

**Descripción:**  
Como **recruiter**  
Quiero **vincular un candidato existente a una oferta publicada**  
Para **crear una candidatura (Application) y comenzar el proceso de evaluación en el pipeline**

**Justificación Estratégica:**  
🎯 **Bloqueante del pipeline.** La tabla `Application` es la entidad que conecta candidatos con ofertas y habilita toda la lógica de estados, etapas y automatizaciones. Sin esta vinculación, no existe proceso de reclutamiento ejecutable. Esta historia es técnicamente simple (3 SP) pero estratégicamente crítica: conecta los dos dominios core (ofertas + candidatos) y desbloquea el valor del pipeline visual, las automatizaciones y el scoring de IA. Sin ella, LTI es solo una base de datos de CVs desconectados.

**Criterios de Aceptación (resumen):**
1. Creación de candidatura (Application) en estado inicial
2. Vinculación automática a primera etapa del pipeline
3. Notificación opcional al candidato (si configurado)

**Dependencias:** US-003-01, US-002-03 (publicación de oferta)

---

### **US-004-01 — Vista Kanban del Pipeline**

**Categoría:** Must Have  
**Estimación:** 8 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-004-PipelineVisual

**Descripción:**  
Como **recruiter o hiring manager**  
Quiero **visualizar el pipeline de una oferta en formato kanban**  
Para **tener una vista rápida del estado de todos los candidatos y priorizar acciones**

**Justificación Estratégica:**  
🎯 **Diferenciador UX principal y "wow effect" en demos.** El análisis de mercado del PRD (LTI-PCN.md) identifica que los ATS tradicionales (Greenhouse, Lever, Workable) tienen UX complejas orientadas a back-office. La vista kanban es el **diferenciador visual más potente** de LTI: permite a recruiters y managers entender el estado de un proceso en 3 segundos, priorizar candidatos y movilizarlos con drag & drop (US-004-02). Esta historia es lo que convierte a LTI en una "herramienta de trabajo diaria", no en un repositorio pasivo. Es crítica para generar adopción y engagement en pilotos.

**Criterios de Aceptación (resumen):**
1. Columnas por etapa del pipeline con tarjetas de candidatos
2. Tarjetas muestran nombre, foto, scoring IA y etiquetas
3. Orden inteligente por scoring o criterios configurables

**Dependencias:** US-003-02, US-006-02 (Scoring IA)

---

### **US-004-02 — Mover Candidatos (Drag & Drop)**

**Categoría:** Must Have  
**Estimación:** 8 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-004-PipelineVisual

**Descripción:**  
Como **recruiter**  
Quiero **mover candidatos entre etapas del pipeline arrastrando y soltando sus tarjetas**  
Para **actualizar el estado del proceso de forma visual e intuitiva**

**Justificación Estratégica:**  
🎯 **Habilita arquitectura event-driven y bloquea automatizaciones.** Esta historia no es solo UX: al mover un candidato, se emite un evento de dominio (`ApplicationStageChanged`) que dispara el motor de automatización (US-005-02). Sin esta funcionalidad, las automatizaciones no tienen "trigger" real y el builder no-code (US-005-01) queda inútil. Es la conexión entre la interacción del usuario y la inteligencia del sistema. Además, completa la experiencia kanban iniciada en US-004-01, validando la propuesta de "orquestación visual" de LTI.

**Criterios de Aceptación (resumen):**
1. Drag & drop funcional con actualización en tiempo real
2. Registro en `ApplicationStageHistory` y emisión de evento de dominio
3. Automatizaciones disparadas según reglas configuradas

**Dependencias:** US-004-01, US-005-02 (Motor de Automatización)

---

### **US-005-02 — Motor de Ejecución de Automatizaciones**

**Categoría:** Must Have  
**Estimación:** 13 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-005-AutomatizacionBasica

**Descripción:**  
Como **sistema LTI**  
Quiero **ejecutar automáticamente las reglas configuradas cuando ocurran eventos de dominio**  
Para **reducir trabajo manual de recruiters y garantizar consistencia en comunicaciones y procesos**

**Justificación Estratégica:**  
🎯 **Core differentiator y propuesta de valor central del PRD.** El análisis de mercado (LTI-PCN.md, sección 4.1) identifica que **ningún ATS actual ofrece automatización no-code profunda y usable**. El motor de automatización es lo que diferencia a LTI de ser "otro ATS con IA decorativa". Sin este motor, el builder visual (US-005-01) no ejecuta nada y la promesa de "reducir trabajo manual en 40%" no es verificable. Esta historia implementa la arquitectura event-driven que permite orquestar emails, notificaciones, cambios de estado y acciones complejas sin programación. **Es bloqueante absoluto del MVP**.

**Criterios de Aceptación (resumen):**
1. Escucha de eventos de dominio (ApplicationStageChanged, JobPostingPublished, etc.)
2. Evaluación de reglas activas aplicables
3. Ejecución de acciones y registro en `AutomationEvent`

**Dependencias:** US-005-01, US-002-03 (publicación de ofertas)

---

### **US-006-01 — Análisis de CV con IA**

**Categoría:** Must Have  
**Estimación:** 8 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-006-IAOperativaInicial

**Descripción:**  
Como **recruiter**  
Quiero **que la IA analice automáticamente el CV de un candidato**  
Para **obtener un resumen de skills, experiencia y ajuste al rol sin leer el documento completo**

**Justificación Estratégica:**  
🎯 **Segundo diferenciador visible de IA operativa.** El PRD (LTI-PCN.md, sección 4.1) enfatiza que la IA debe actuar como "operador del proceso, no como plugin". El análisis automático de CVs reduce el tiempo de screening manual de 10-15 minutos por candidato a 30 segundos de revisión de resumen. Sin esta funcionalidad, LTI no cumple su promesa de "ahorrar tiempo mediante IA embebida". Además, alimenta el scoring de candidatos (US-006-02) y las sugerencias de siguiente paso (US-006-03). Es la primera interacción tangible con IA que experimenta un recruiter en LTI.

**Criterios de Aceptación (resumen):**
1. Extracción automática de información estructurada del CV
2. Resumen en lenguaje natural (3-5 puntos)
3. Almacenamiento en `AIInsight` para consultas posteriores

**Dependencias:** US-003-01, configuración de OpenAI/Claude

---

### **US-006-02 — Scoring de Candidatos con IA**

**Categoría:** Must Have  
**Estimación:** 8 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-006-IAOperativaInicial

**Descripción:**  
Como **recruiter o hiring manager**  
Quiero **que la IA asigne un score (0-100) a cada candidato según su ajuste al rol**  
Para **priorizar automáticamente a quién revisar primero y reducir sesgo subjetivo**

**Justificación Estratégica:**  
🎯 **Priorización inteligente y reducción de sesgo.** Esta historia traduce el análisis de CVs (US-006-01) en acción concreta: ordenar candidatos por potencial de éxito. El scoring visible en las tarjetas del kanban (US-004-01) permite a recruiters enfocarse en los top 20% de candidatos primero, optimizando su tiempo. Más importante, el scoring basado en IA reduce el sesgo inconsciente al evaluar candidatos por métricas objetivas (skills técnicas, experiencia, educación) antes que por variables demográficas. Es un diferenciador ético y operativo clave para startups tecnológicas que valoran DEI (Diversity, Equity, Inclusion).

**Criterios de Aceptación (resumen):**
1. Scoring automático (0-100) comparando CV con requisitos de oferta
2. Visualización en pipeline (badge "Match: X%")
3. Explicabilidad: desglose de factores contribuyentes

**Dependencias:** US-006-01, US-004-01

---

## 🟡 SHOULD HAVE — Funcionalidades Importantes para Adopción

Estas funcionalidades **añaden valor significativo** y reducen fricciones en la adopción del MVP, pero el producto puede lanzarse sin ellas. Se priorizan para entrega inmediatamente después del MVP (Fase 1+).

---

### **US-001-03 — Invitación de Usuarios**

**Categoría:** Should Have  
**Estimación:** 5 SP  
**Valor de Negocio:** Medio-Alto (4/5)  
**Épica:** EPIC-001-GestionUsuarios

**Descripción:**  
Como **administrador de una empresa en LTI**  
Quiero **invitar a miembros de mi equipo de reclutamiento**  
Para **habilitar colaboración entre recruiters y hiring managers en la plataforma**

**Justificación Estratégica:**  
⚠️ **Importante para colaboración multi-usuario, pero no bloqueante del MVP técnico.** En pilotos iniciales con early adopters (equipos de 1-3 recruiters), el admin puede crear usuarios manualmente o el proveedor de soporte puede hacerlo vía backend. Sin embargo, para escalar adopción y demostrar UX pulida, la invitación automática por email con onboarding fluido es crítica. Se clasifica como "Should Have" porque habilita colaboración real (hiring managers, múltiples recruiters) pero no impide ejecutar procesos de reclutamiento con un solo usuario. Se entrega en sprint 2-3 del MVP.

**Criterios de Aceptación (resumen):**
1. Envío de invitación por email con enlace temporal
2. Aceptación de invitación con registro completo
3. Gestión de permisos por rol (Recruiter, Hiring Manager)

**Dependencias:** US-001-01, US-001-02

---

### **US-002-02 — Revisión y Aprobación de Oferta**

**Categoría:** Should Have  
**Estimación:** 5 SP  
**Valor de Negocio:** Medio-Alto (4/5)  
**Épica:** EPIC-002-GestionOfertas

**Descripción:**  
Como **hiring manager**  
Quiero **revisar y aprobar ofertas creadas por recruiters**  
Para **asegurar que el contenido refleja correctamente las necesidades de mi equipo antes de publicarlas**

**Justificación Estratégica:**  
⚠️ **Workflow colaborativo deseable pero no bloqueante del flujo básico.** En el MVP mínimo, un recruiter puede crear y publicar ofertas directamente sin aprobación formal. Sin embargo, en organizaciones con procesos maduros, la revisión por hiring managers es **best practice** y reduce errores en descripciones de rol (requisitos incorrectos, salario no alineado, etc.). Esta historia habilita trazabilidad y calidad, pero no es bloqueante para ejecutar procesos de reclutamiento end-to-end. Se prioriza para Sprint 3-4 cuando hay managers activos en pilotos.

**Criterios de Aceptación (resumen):**
1. Notificación al hiring manager cuando oferta está en revisión
2. Aprobación o solicitud de cambios con comentarios
3. Registro de historial de aprobaciones para auditoría

**Dependencias:** US-002-01, US-001-03

---

### **US-002-03 — Publicar Oferta y Activar Automatizaciones**

**Categoría:** Should Have  
**Estimación:** 5 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-002-GestionOfertas

**Descripción:**  
Como **recruiter**  
Quiero **publicar una oferta aprobada y activar automatizaciones iniciales**  
Para **comenzar a recibir candidaturas y gestionar el proceso con reglas predefinidas**

**Justificación Estratégica:**  
⚠️ **Habilita automatizaciones, pero ofertas pueden existir en estado "borrador" en MVP inicial.** Esta historia crea el concepto de "oferta publicada" (estado activo) y dispara las automatizaciones configuradas (ej: email de confirmación a candidatos, notificación a hiring manager). Sin embargo, en un MVP extremadamente mínimo, se pueden vincular candidatos a ofertas en estado borrador para validar el pipeline. Se clasifica como "Should Have" porque es crítica para demostrar el motor de automatización (US-005-02) en funcionamiento, pero técnicamente no bloquea la creación de candidaturas. Se entrega en Sprint 2.

**Criterios de Aceptación (resumen):**
1. Publicación de oferta con cambio de estado a "Publicada"
2. Creación automática de etapas del pipeline
3. Activación de reglas de automatización predefinidas

**Dependencias:** US-002-02, US-005-02

---

### **US-005-01 — Builder Visual de Automatizaciones**

**Categoría:** Should Have  
**Estimación:** 13 SP  
**Valor de Negocio:** Alto (5/5)  
**Épica:** EPIC-005-AutomatizacionBasica

**Descripción:**  
Como **recruiter o admin**  
Quiero **configurar reglas de automatización mediante un editor visual no-code**  
Para **orquestar flujos complejos (emails, notificaciones, cambios de estado) sin depender de IT**

**Justificación Estratégica:**  
⚠️ **Diferenciador clave, pero el motor puede funcionar con reglas pre-configuradas en MVP inicial.** El builder visual drag & drop es la interfaz que hace la automatización accesible a recruiters sin conocimientos técnicos. Sin embargo, en un MVP extremadamente mínimo, se pueden pre-configurar 3-5 reglas comunes (ej: "Enviar email de confirmación al aplicar", "Notificar a hiring manager al pasar a entrevista") directamente en la base de datos. El motor (US-005-02) ejecuta esas reglas sin necesidad de UI de configuración. Se clasifica como "Should Have" porque es **crítico para demos y diferenciación de ventas**, pero técnicamente no bloquea la ejecución de automatizaciones. Se entrega en Sprint 3-4.

**Criterios de Aceptación (resumen):**
1. Editor drag & drop con disparadores y acciones
2. Configuración de parámetros específicos por acción
3. Guardado de reglas como `AutomationRule` + `AutomationAction`

**Dependencias:** US-002-03, US-004-02

---

### **US-006-03 — Sugerencias de Siguiente Paso con IA**

**Categoría:** Should Have  
**Estimación:** 8 SP  
**Valor de Negocio:** Medio-Alto (4/5)  
**Épica:** EPIC-006-IAOperativaInicial

**Descripción:**  
Como **recruiter**  
Quiero **que la IA sugiera el siguiente paso más adecuado para cada candidato**  
Para **tomar decisiones más rápidas y basadas en patrones de éxito históricos**

**Justificación Estratégica:**  
⚠️ **IA proactiva que mejora eficiencia, pero requiere datos históricos para ser precisa.** Esta historia convierte la IA de "reactiva" (analizar, puntuar) a "proactiva" (sugerir acciones). Las sugerencias tipo "Mover a entrevista técnica", "Solicitar referencias", "Rechazar cortésmente" aceleran decisiones de recruiters. Sin embargo, la precisión de estas sugerencias mejora con datos históricos de procesos exitosos previos. En un MVP sin historial, las sugerencias serán heurísticas básicas. Se clasifica como "Should Have" porque añade "wow factor" en demos pero no es operativamente crítica hasta tener 20-30 procesos completados. Se entrega en Sprint 4-5.

**Criterios de Aceptación (resumen):**
1. Sugerencia contextual por candidato (acción + justificación)
2. Explicación breve basada en score y tiempo en etapa
3. Ejecución con un clic de la acción propuesta

**Dependencias:** US-006-02, US-004-02

---

## 🟢 COULD HAVE — Funcionalidades Deseables para Iteraciones Futuras

Estas funcionalidades **mejoran la experiencia** y añaden capacidades avanzadas, pero no son críticas para validar la propuesta de valor del MVP. Se consideran para Fase 2-3 según feedback de early adopters.

---

### **US-003-03 — Vista de Perfil de Candidato**

**Categoría:** Could Have  
**Estimación:** 3 SP  
**Valor de Negocio:** Medio-Alto (4/5)  
**Épica:** EPIC-003-GestionCandidatos

**Descripción:**  
Como **recruiter o hiring manager**  
Quiero **visualizar el perfil completo de un candidato**  
Para **revisar su CV, historial de interacciones, feedback recibido y tomar decisiones informadas**

**Justificación Estratégica:**  
💡 **UX mejorada pero no bloqueante del flujo básico.** En un MVP mínimo, la información del candidato (nombre, email, CV, scoring) se muestra en las tarjetas del kanban (US-004-01). Una vista de perfil dedicada añade contexto (historial de candidaturas previas, comentarios de evaluadores, timeline de interacciones) pero no es imprescindible para tomar decisiones de avance/rechazo. Se clasifica como "Could Have" porque mejora la experiencia de evaluación detallada, especialmente para hiring managers que quieren contexto profundo, pero no bloquea procesos. Se entrega en Fase 2 (Mes 3-4).

**Criterios de Aceptación (resumen):**
1. Información básica del candidato y CV descargable
2. Historial de candidaturas a otras ofertas
3. Feedback y comentarios de evaluadores

**Dependencias:** US-003-01, US-003-02

---

### **US-004-03 — Historial de Cambios de Etapa**

**Categoría:** Could Have  
**Estimación:** 5 SP  
**Valor de Negocio:** Medio (3/5)  
**Épica:** EPIC-004-PipelineVisual

**Descripción:**  
Como **recruiter o hiring manager**  
Quiero **ver el historial completo de cambios de etapa de un candidato**  
Para **entender su progreso en el proceso y detectar posibles bloqueos o retrasos**

**Justificación Estratégica:**  
💡 **Transparencia y auditoría valiosas, pero no críticas para ejecución operativa.** El historial de cambios de etapa (timeline con fechas, usuarios responsables, tiempo en cada etapa) es útil para análisis post-mortem ("¿por qué tardamos 3 semanas en mover este candidato?") y para auditorías de compliance. Sin embargo, no afecta la capacidad de recruiters para ejecutar procesos diarios. La información se registra en `ApplicationStageHistory` desde el día 1 (US-004-02), por lo que la funcionalidad es solo exponer esos datos en UI. Se clasifica como "Could Have" porque añade valor analítico pero no operativo inmediato. Se entrega en Fase 2.

**Criterios de Aceptación (resumen):**
1. Timeline de cambios con fecha, etapa origen/destino, usuario
2. Cálculo de tiempo transcurrido en cada etapa
3. Accesibilidad desde perfil de candidato o pipeline

**Dependencias:** US-004-02

---

### **US-005-03 — Logs y Monitoreo de Automatizaciones**

**Categoría:** Could Have  
**Estimación:** 5 SP  
**Valor de Negocio:** Medio-Alto (4/5)  
**Épica:** EPIC-005-AutomatizacionBasica

**Descripción:**  
Como **recruiter o admin**  
Quiero **visualizar logs de ejecución de automatizaciones**  
Para **verificar que las reglas funcionan correctamente y debuggear problemas (emails no enviados, notificaciones fallidas)**

**Justificación Estratégica:**  
💡 **Debugging y confianza en automatizaciones, pero no bloqueante del MVP funcional.** Los logs de automatización (tabla `AutomationEvent`) se registran automáticamente desde el día 1 por el motor (US-005-02). Sin embargo, la UI para visualizarlos (tabla filtrable, detalle de errores) no es imprescindible para ejecutar procesos: los errores pueden monitorearse vía logs de sistema o soporte técnico en MVP. Se clasifica como "Could Have" porque mejora la confianza de usuarios en las automatizaciones y facilita self-service debugging, pero no impide la ejecución operativa. Se entrega en Fase 2 cuando hay mayor volumen de automatizaciones activas.

**Criterios de Aceptación (resumen):**
1. Listado de ejecuciones con estado (SUCCESS/FAILED)
2. Detalle de errores con mensaje específico
3. Filtros por oferta, candidato, regla, fecha, estado

**Dependencias:** US-005-02

---

## ⚪ WON'T HAVE — Funcionalidades Fuera de Alcance del MVP

Estas funcionalidades son **valiosas a largo plazo** pero quedan explícitamente fuera del MVP y primeras iteraciones (Fase 1-2). Se posponen a Fase 3 (Mes 5-6) o posteriores según evolución del producto y feedback del mercado.

---

### **EPIC-007 — Feedback Colaborativo**

**Categoría:** Won't Have (en MVP)  
**Estimación épica:** 18 SP  
**Valor de Negocio:** Alto (5/5) - pero postergable  
**Fase:** 2 — Colaboración y Experiencia (Mes 3-4)

**Descripción General:**  
Sistema de feedback estructurado para entrevistas, scorecards, comentarios de hiring managers y recruiters. Incluye formularios de evaluación, ponderación de criterios y agregación de feedback para decisiones finales.

**Justificación Estratégica:**  
🔵 **Crítico para colaboración avanzada, pero no para validar diferenciación inicial de LTI.** El feedback estructurado es una capacidad estándar de ATS enterprise (Greenhouse, Lever) y no es un diferenciador de LTI. En un MVP, recruiters y managers pueden tomar decisiones basadas en scoring de IA (US-006-02), análisis de CV (US-006-01) y conversaciones ad-hoc (Slack, email). El feedback formal con scorecards se vuelve crítico cuando:
1. Hay múltiples evaluadores por candidato (entrevistas técnicas, culturales, manager final).
2. El cliente exige trazabilidad de decisiones para compliance o auditoría.
3. Se implementan procesos de calibración de evaluadores.

En pilotos con startups pequeñas (equipos de 2-3 personas), estas necesidades no son bloqueantes. Se posterga a **Fase 2** (Mes 3-4) cuando hay adopción validada y clientes demandan colaboración más sofisticada.

**Historias incluidas (referencia):**
- Crear formulario de feedback para entrevista
- Asignar evaluadores a candidatos
- Registrar scoring por criterio (técnico, cultural, comunicación)
- Agregar feedback de múltiples evaluadores
- Vista consolidada de evaluaciones

---

### **EPIC-008 — Decisiones Finales**

**Categoría:** Won't Have (en MVP)  
**Estimación épica:** 13 SP  
**Valor de Negocio:** Alto (5/5) - pero postergable  
**Fase:** 2 — Colaboración y Experiencia (Mes 3-4)

**Descripción General:**  
Registro explícito de decisiones finales (Enviar oferta, Rechazar, Poner en hold) con trazabilidad, automatizaciones asociadas (generar contrato, enviar email de rechazo) y métricas de conversión.

**Justificación Estratégica:**  
🔵 **Cierre formal de procesos deseable, pero no bloqueante del flujo operativo.** En el MVP, un candidato que llega a la última etapa del pipeline ("Offer" o "Hired") implica una decisión tácita de contratación. El recruiter puede enviar un email manual con la oferta y mover la tarjeta. La formalización de decisiones (tabla `Decision`, triggers de automatización específicos, generación de documentos) añade trazabilidad y automatización avanzada, pero no cambia el resultado final. Se posterga a **Fase 2** porque:
1. Requiere integración con sistemas de generación de contratos (Docusign, PandaDoc).
2. Es más relevante para clientes con procesos regulados o auditorías estrictas.
3. No aporta diferenciación vs ATS tradicionales (es tabla de stakes).

**Historias incluidas (referencia):**
- Registrar decisión final con justificación
- Enviar oferta automatizada con plantilla
- Email de rechazo con feedback constructivo (opcional)
- Métricas de offer acceptance rate

---

### **EPIC-009 — Candidate Experience**

**Categoría:** Won't Have (en MVP)  
**Estimación épica:** 21 SP  
**Valor de Negocio:** Medio-Alto (4/5) - diferenciador de marca  
**Fase:** 2 — Colaboración y Experiencia (Mes 3-4)

**Descripción General:**  
Portal para candidatos con estado del proceso, mensajes personalizados, feedback post-rechazo y posibilidad de actualizar perfil. Mejora transparencia y employer branding.

**Justificación Estratégica:**  
🔵 **Experiencia del candidato es diferenciador de employer branding, pero no de eficiencia operativa de recruiters (foco del MVP).** El PRD (LTI-PCN.md) identifica dos segmentos de dolor: (1) recruiters con cansancio operativo y (2) candidatos con sensación de "agujero negro". El MVP prioriza resolver el dolor #1 primero porque **los compradores de LTI son empresas/recruiters, no candidatos**. Un portal de candidatos:
- No reduce trabajo manual de recruiters (objetivo primario del MVP).
- No demuestra automatización ni IA operativa (diferenciadores core).
- Requiere inversión significativa en UX mobile, autenticación sin fricción y soporte multilingüe.

Se posterga a **Fase 2** cuando LTI ya tiene tracción con clientes y puede invertir en mejorar la marca empleadora de esos clientes. Es un "nice to have" para demos de ventas pero no bloqueante de adopción.

**Historias incluidas (referencia):**
- Portal de candidato con login passwordless
- Vista de estado del proceso (etapa actual, próximos pasos)
- Mensajes personalizados del recruiter
- Feedback post-rechazo automatizado por IA
- Actualización de perfil y CV

---

### **EPIC-010 — Notificaciones Avanzadas (Slack/Teams)**

**Categoría:** Won't Have (en MVP)  
**Estimación épica:** 13 SP  
**Valor de Negocio:** Medio-Alto (4/5) - integración avanzada  
**Fase:** 2 — Colaboración y Experiencia (Mes 3-4)

**Descripción General:**  
Integración nativa con Slack y Microsoft Teams para notificaciones en tiempo real, resúmenes de candidatos, aprobaciones rápidas y menciones contextuales. Convierte LTI en hub operativo del día a día de recruiters.

**Justificación Estratégica:**  
🔵 **Integración valiosa para workflow diario, pero no bloqueante del MVP funcional.** El PRD identifica que recruiters "viven en email, LinkedIn, Slack". Sin embargo, en el MVP, las notificaciones por **email** (via SendGrid, ya integrado en US-001-01) son suficientes para alertas críticas (nuevo candidato, cambio de etapa, decisión pendiente). Las integraciones con Slack/Teams:
- Requieren OAuth adicional, webhooks, manejo de rate limits y UX específica por plataforma.
- Son más valiosas cuando hay múltiples usuarios colaborando activamente (managers + recruiters).
- No demuestran el core differentiator (automatización + IA) sino integración con ecosistema SaaS.

Se posterga a **Fase 2** porque es un "quality of life improvement" que aumenta adopción pero no valida la propuesta de valor fundamental de LTI. Las notificaciones por email cumplen el objetivo mínimo en MVP.

**Historias incluidas (referencia):**
- Configurar integración con Slack/Teams (OAuth)
- Notificación en canal cuando candidato cambia etapa
- Mensaje directo a hiring manager con resumen de candidato
- Aprobación rápida desde Slack (botones interactivos)
- Menciones contextuales (@usuario en comentarios)

---

### **EPIC-011 — Automatización Avanzada**

**Categoría:** Won't Have (en MVP)  
**Estimación épica:** 21 SP  
**Valor de Negocio:** Alto (5/5) - pero requiere madurez del motor  
**Fase:** 2 — Colaboración y Experiencia (Mes 3-4)

**Descripción General:**  
Ampliación del builder no-code con condiciones complejas (AND/OR, comparaciones numéricas), acciones múltiples en paralelo, delays programados, y conectores a herramientas externas (Calendly, Zoom, HackerRank, Codility).

**Justificación Estratégica:**  
🔵 **Automatización profunda es diferenciador estratégico, pero requiere validación del motor básico primero.** El MVP incluye el motor de automatización (US-005-02) y el builder inicial (US-005-01) con funcionalidad básica:
- Triggers simples: "Candidato cambia a etapa X".
- Acciones simples: "Enviar email Y", "Notificar a usuario Z".
- Condiciones básicas: "Si score > 80".

La automatización avanzada (múltiples condiciones booleanas, acciones secuenciales con delays, integraciones con APIs externas) requiere:
1. Validar que el motor básico es estable y escalable.
2. Feedback de clientes sobre qué automatizaciones avanzadas necesitan (no especular).
3. Inversión en conectores específicos por herramienta (cada uno 3-5 SP).

Se posterga a **Fase 2** porque el MVP debe demostrar que el concepto de automatización funciona antes de invertir en sofisticación. Es mejor iterar basándose en uso real de clientes que construir complejidad especulativa.

**Historias incluidas (referencia):**
- Condiciones complejas con operadores booleanos (AND/OR/NOT)
- Acciones secuenciales con delays (esperar 2 días, luego enviar recordatorio)
- Webhooks salientes para integrar con herramientas custom
- Conectores pre-configurados (Calendly, Zoom, evaluación técnica)
- Testing y simulación de automatizaciones antes de activarlas

---

### **EPIC-012 — Reporting Esencial**

**Categoría:** Won't Have (en MVP)  
**Estimación épica:** 18 SP  
**Valor de Negocio:** Alto (5/5) - pero no crítico para ejecución operativa  
**Fase:** 3 — Insights y Escalabilidad (Mes 5-6)

**Descripción General:**  
Dashboards de métricas clave (time-to-hire, conversión por etapa, source-of-hire, offer acceptance rate), vistas por oferta, por empresa y exportación de datos. Orientado a C-level y People Ops para decisiones estratégicas.

**Justificación Estratégica:**  
🔵 **Reporting es crítico para C-level y renovación de contratos, pero no para adopción inicial de recruiters.** El MVP debe resolver el dolor operativo diario de recruiters (trabajo manual, coordinación, priorización). Los dashboards de métricas:
- Son consumidos por stakeholders que no usan LTI día a día (CEOs, CFOs, VPs de People).
- Requieren volumen significativo de datos históricos para ser accionables (20-30 procesos completados).
- No demuestran la diferenciación core de LTI (automatización + IA) sino capacidad de reporting estándar de ATS.

En MVP, los clientes pueden:
- Exportar datos manualmente (CSV) para análisis ad-hoc.
- Consultar métricas básicas via queries SQL si es necesario.

Se posterga a **Fase 3** (Mes 5-6) porque es crítico para **retención y expansión** (upsell a clientes satisfechos) pero no para **adquisición** (validar encaje producto-mercado). Los early adopters priorizan eficiencia operativa sobre reporting sofisticado.

**Historias incluidas (referencia):**
- Dashboard de time-to-hire por oferta y promedio empresa
- Conversión por etapa (funnel de candidatos)
- Source-of-hire (LinkedIn, referidos, job boards)
- Offer acceptance rate
- Exportación de métricas a CSV/Excel

---

### **EPIC-013 — Reutilización de Talento**

**Categoría:** Won't Have (en MVP)  
**Estimación épica:** 21 SP  
**Valor de Negocio:** Alto (5/5) - diferenciador estratégico pero requiere histórico  
**Fase:** 3 — Insights y Escalabilidad (Mes 5-6)

**Descripción General:**  
Sistema de recomendación de candidatos previos (silver medalists, talent pools) para nuevas ofertas. Incluye matching semántico de skills, filtros avanzados, campañas de reactivación automatizadas y métricas de reutilización.

**Justificación Estratégica:**  
🔵 **Reutilización de talento es diferenciador potente vs ATS tradicionales, pero requiere base de candidatos históricos significativa.** El PRD (LTI-PCN.md, sección 4.1) identifica la "visión de talento unificada" como oportunidad disruptiva. Sin embargo:
- En las primeras 8-12 semanas de uso de LTI, los clientes no tienen suficientes candidatos previos para reutilizar (están construyendo su base de datos).
- El matching semántico de skills requiere IA avanzada (embeddings, cosine similarity) que debe entrenarse con datos reales.
- La funcionalidad genera valor cuando un cliente tiene 100+ candidatos previos y 5+ procesos completados.

Se posterga a **Fase 3** porque es una capacidad de **madurez y escalabilidad**, no de validación inicial. Los early adopters no pueden aprovecharla hasta tener histórico. Es mejor invertir ese esfuerzo (21 SP) en funcionalidades que generen valor desde el día 1.

**Historias incluidas (referencia):**
- Algoritmo de matching de candidatos previos a nueva oferta
- Vista de candidatos recomendados (silver medalists, talent pools)
- Filtros avanzados por skills, experiencia, ubicación
- Campaña de reactivación automatizada (emails personalizados IA)
- Métricas de tasa de reutilización

---

### **EPIC-014 — Movilidad Interna**

**Categoría:** Won't Have (en MVP ni Fase 2)  
**Estimación épica:** 26 SP  
**Valor de Negocio:** Alto (5/5) - pero relevante solo para mid-market/enterprise  
**Fase:** 3+ — Escalabilidad (Mes 5-6 o posterior)

**Descripción General:**  
Integración con HRIS (BambooHR, Personio, Workday) para identificar talento interno apto para vacantes abiertas, sugerencias de movilidad lateral/vertical, y tracking de movimientos internos. Convierte LTI en plataforma de "talent orchestration" completa (externo + interno).

**Justificación Estratégica:**  
🔵 **Movilidad interna es relevante para empresas 300+ empleados, no para segmento objetivo del MVP (startups 50-300).** El PRD (LTI-PCN.md, sección 5.2) prioriza startups y scale-ups tecnológicas como segmento inicial porque:
- Tienen alto urgencia en eficiencia de reclutamiento (crecimiento acelerado).
- Son early adopters de SaaS innovador.
- Tienen presupuesto suficiente para ATS de valor añadido.

Sin embargo, startups de 50-200 empleados **no tienen problema de movilidad interna** (equipos pequeños, todos se conocen, movimientos ocurren informalmente). La movilidad interna se vuelve crítica cuando:
- La empresa tiene 300+ empleados distribuidos en múltiples oficinas/países.
- Existe desconexión entre departamentos (no todos conocen skills de todos).
- Hay estrategia formal de career pathing y retención de talento.

Se posterga a **Fase 3+** porque:
1. Requiere integraciones complejas con HRIS (APIs no estandarizadas, mapeo de datos custom).
2. Es más relevante para expansión a mid-market (no para validar MVP con startups).
3. Añade complejidad de producto significativa (permisos, privacidad de datos internos, política de RRHH).

**Historias incluidas (referencia):**
- Integración con HRIS (BambooHR, Personio, Workday)
- Matching de empleados internos con vacantes abiertas
- Sugerencias de movilidad lateral/vertical con IA
- Proceso de aplicación interna simplificado
- Tracking de movimientos internos y métricas de retention

---

### **EPIC-015 — Optimización de IA**

**Categoría:** Won't Have (en MVP)  
**Estimación épica:** 21 SP  
**Valor de Negocio:** Alto (5/5) - pero requiere datos de entrenamiento  
**Fase:** 3 — Insights y Escalabilidad (Mes 5-6)

**Descripción General:**  
Mejoras en recomendaciones de IA: priorización predictiva de candidatos (probabilidad de éxito), detección de riesgos de caída (alertas de candidatos estancados), aprendizaje continuo basado en feedback de decisiones reales, y explicabilidad avanzada de scoring.

**Justificación Estratégica:**  
🔵 **Optimización de IA requiere datos de entrenamiento y ciclos de feedback que no existen en MVP.** El MVP incluye IA operativa básica:
- Análisis de CVs (US-006-01): extracción de skills + resumen.
- Scoring de candidatos (US-006-02): comparación semántica con requisitos.
- Sugerencias de siguiente paso (US-006-03): heurísticas básicas.

Sin embargo, estas capacidades son **estáticas** (modelos pre-entrenados de OpenAI/Claude aplicados directamente). La optimización de IA implica:
1. **Aprendizaje supervisado:** Entrenar modelos con datos de decisiones reales (candidatos contratados vs rechazados).
2. **Priorización predictiva:** Predecir probabilidad de éxito de candidato (requiere 100+ procesos completados con outcomes conocidos).
3. **Detección de riesgos:** Identificar candidatos estancados en una etapa (requiere histórico de tiempos normales vs anómalos).

Estos avances requieren:
- Volumen significativo de datos de producción (3-6 meses de uso real).
- Expertise en ML/Data Science (no solo integración de APIs).
- Infraestructura de entrenamiento de modelos (no prioritaria en MVP).

Se posterga a **Fase 3** porque es una capacidad de **madurez y mejora continua**, no de validación inicial. Los modelos básicos de OpenAI/Claude son suficientes para demostrar valor de IA operativa en MVP.

**Historias incluidas (referencia):**
- Priorización predictiva con probabilidad de éxito (ML model)
- Detección de riesgos de caída (alertas de candidatos estancados)
- Aprendizaje continuo basado en feedback de decisiones
- Explicabilidad avanzada de scoring (SHAP values, feature importance)
- A/B testing de modelos de IA

---

### **EPIC-016 — Integraciones Extendidas**

**Categoría:** Won't Have (en MVP)  
**Estimación épica:** 26 SP  
**Valor de Negocio:** Medio-Alto (4/5) - relevante para enterprise  
**Fase:** 3+ — Escalabilidad (Mes 5-6 o posterior)

**Descripción General:**  
Conectores adicionales con job boards (LinkedIn, Indeed, InfoJobs), herramientas de evaluación técnica (HackerRank, Codility, CodeSignal), CRMs de reclutamiento (Gem, Ashby), y APIs abiertas para clientes enterprise con sistemas internos custom.

**Justificación Estratégica:**  
🔵 **Integraciones extendidas son relevantes para escalar a enterprise y mid-market, no para MVP con startups.** El MVP incluye integraciones esenciales:
- **Auth0/Clerk:** Autenticación y SSO.
- **SendGrid:** Emails transaccionales.
- **AWS S3:** Almacenamiento de CVs.
- **OpenAI/Claude:** IA operativa.

Estas integraciones permiten ejecutar procesos de reclutamiento end-to-end. Las integraciones adicionales (job boards, evaluación técnica, CRMs) son valiosas pero:
- Cada conector requiere 3-5 SP de desarrollo (OAuth, mapeo de datos, manejo de errores, rate limits).
- Job boards (LinkedIn, Indeed) son más relevantes para alto volumen de aplicaciones (retail, hospitality), no para tech hiring con sourcing activo.
- Herramientas de evaluación técnica son necesarias cuando el proceso incluye pruebas estandarizadas, pero muchas startups usan GitHub/portfolios o pruebas ad-hoc.
- CRMs de reclutamiento (Gem, Ashby) son usados por equipos de reclutamiento sofisticados (> 10 recruiters), no por startups pequeñas.

Se posterga a **Fase 3+** porque:
1. Es mejor validar el core product antes de dispersar esfuerzo en conectores.
2. Los clientes enterprise que demandan integraciones complejas lleguen después de validar product-market fit con early adopters.
3. Cada integración debe priorizarse según demanda real de clientes, no especulativamente.

**Historias incluidas (referencia):**
- Conectores con job boards (LinkedIn, Indeed, InfoJobs)
- Integración con herramientas de evaluación técnica (HackerRank, Codility)
- Integración con CRMs de reclutamiento (Gem, Ashby)
- API abierta para clientes enterprise (webhooks, REST API documentada)
- Marketplace de integraciones (partners pueden desarrollar conectores)

---

## Resumen Ejecutivo de Priorización MoSCoW

### Distribución de Story Points por Categoría

| **Categoría**      | **Historias** | **Story Points** | **% del Total** |
|--------------------|---------------|------------------|-----------------|
| **Must Have**      | 10            | 81 SP            | 62.8%           |
| **Should Have**    | 6             | 48 SP            | 37.2%           |
| **Could Have**     | 3             | 13 SP            | 10.1%           |
| **Won't Have**     | 10 épicas     | 198 SP (est.)    | —               |
| **Total MVP**      | 16            | 129 SP           | 100%            |

### Roadmap Recomendado basado en MoSCoW

#### **Sprint 1-3 (Semanas 1-6): Must Have Core**
- **Objetivo:** Lanzar funcionalidades bloqueantes del MVP que demuestran diferenciación.
- **Historias:** US-001-01, US-001-02, US-002-01, US-003-01, US-003-02, US-006-01, US-006-02 (81 SP)
- **Entregable:** Sistema funcional con autenticación multi-tenant, creación de ofertas con IA, registro de candidatos, scoring automático.
- **Validación:** Early adopters pueden crear ofertas, agregar candidatos y ver scoring de IA en acción.

#### **Sprint 4-5 (Semanas 7-10): Must Have + Should Have UI/UX**
- **Objetivo:** Completar pipeline visual y automatizaciones para demo completa.
- **Historias:** US-004-01, US-004-02, US-005-02 (29 SP) + US-001-03, US-002-02, US-002-03, US-005-01 (28 SP)
- **Entregable:** Pipeline kanban con drag & drop, motor de automatización funcional, builder visual inicial.
- **Validación:** Clientes pueden mover candidatos, disparar automatizaciones y ver impacto en reducción de trabajo manual.

#### **Sprint 6-7 (Semanas 11-14): Could Have + Polish**
- **Objetivo:** Añadir funcionalidades de UX mejorada y preparar para lanzamiento.
- **Historias:** US-003-03, US-004-03, US-005-03, US-006-03 (21 SP)
- **Entregable:** Perfil de candidato detallado, historial de etapas, logs de automatizaciones, sugerencias de IA proactivas.
- **Validación:** Producto pulido listo para primeros clientes de pago.

#### **Fase 2-3 (Meses 3-6): Won't Have (Backlog Futuro)**
- **Objetivo:** Escalar funcionalidades basadas en feedback de clientes.
- **Épicas:** EPIC-007 a EPIC-016 según priorización dinámica post-MVP.

---

## Principios de Re-Priorización Dinámica

Este backlog MoSCoW **no es estático**. Debe ajustarse basándose en:

1. **Feedback de Early Adopters (Weeks 4-8):**
   - Si clientes demandan urgentemente colaboración con managers → promover EPIC-007 (Feedback) de Won't Have a Should Have.
   - Si candidatos reportan experiencia negativa recurrentemente → promover EPIC-009 (Candidate Experience) a Fase 2 temprana.

2. **Métricas de Adopción (Weeks 8-12):**
   - Si uso del builder de automatizaciones es bajo → postergar US-005-01 y priorizar automatizaciones pre-configuradas.
   - Si scoring de IA tiene baja confianza de usuarios → invertir en US-015 (Optimización IA) antes de reporting.

3. **Bloqueos Técnicos:**
   - Si Auth0/Clerk tiene problemas de performance → considerar migración a Clerk o auth custom (impacta todas las historias de autenticación).
   - Si OpenAI API tiene alta latencia → evaluar cache de resultados o modelos locales.

4. **Oportunidades de Mercado:**
   - Si competitor lanza feature breakthrough → ajustar prioridad de épicas Won't Have para mantener diferenciación.
   - Si segmento mid-market muestra interés temprano → acelerar EPIC-012 (Reporting) y EPIC-014 (Movilidad Interna).

---

## Criterios de Éxito del MVP (Validación de Must Have)

Para considerar el MVP exitoso, debe cumplir:

✅ **Validación Técnica:**
- 100% de las historias Must Have implementadas y testeadas (81 SP).
- Arquitectura multi-tenant funcional con 0 cross-tenant data leaks en testing.
- IA operativa con latencia < 3 segundos en análisis de CVs.

✅ **Validación de Producto:**
- 3-5 early adopters ejecutando procesos de reclutamiento reales (5+ ofertas, 20+ candidatos por cliente).
- Reducción medida de 30-40% en tiempo manual de recruiters (baseline vs LTI).
- Net Promoter Score (NPS) > 40 entre usuarios piloto.

✅ **Validación de Negocio:**
- 2-3 clientes dispuestos a convertirse de piloto a pago (validación de willingness to pay).
- Feedback cualitativo positivo sobre diferenciación ("esto no lo tiene mi ATS actual").
- 0 churns por bugs críticos o falta de funcionalidades bloqueantes.

---

*Documento generado para priorización estratégica del MVP de LTI Platform*  
*Última actualización: 2025-12-02*
