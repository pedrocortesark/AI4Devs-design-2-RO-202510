# Backlog de Producto — LTI Platform (Opción RICE Score)

> **Documento:** Backlog priorizado mediante método RICE (Reach × Impact × Confidence ÷ Effort)  
> **Guardar en:** `Agile/Backlogs-Options/Backlog_Option_C_RICE.md`  
> **Fecha de generación:** 2025-12-02  
> **Contexto:** Basado en análisis cuantitativo de impacto, alcance y esfuerzo documentados en `LTI-PCN.md` y `UserStories-PCN.md`

---

## Introducción al Método RICE

**RICE** es un framework de priorización data-driven desarrollado por Intercom que permite tomar decisiones objetivas sobre qué features construir primero. Cada historia de usuario se evalúa en 4 dimensiones:

### Componentes del RICE Score

**1. Reach (Alcance) — ¿Cuántos usuarios impacta?**

Estimación del número de usuarios/empresas que serán afectados por esta funcionalidad en un período de tiempo definido (trimestre).

- **Escala:** Número absoluto o porcentaje de user base
- **Para LTI (MVP con early adopters):**
  - **100%** = Todos los usuarios (recruiters + managers + admins) en todas las empresas
  - **75%** = Mayoría de usuarios pero no universal (ej: solo recruiters, no managers)
  - **50%** = Mitad de los usuarios o uso frecuente pero no diario
  - **25%** = Segmento específico o uso ocasional
  - **10%** = Funcionalidad de nicho o administrativa

**Ejemplo:** Login multi-tenant (US-001-02) tiene Reach = 100% porque **todos** los usuarios deben autenticarse para usar LTI.

---

**2. Impact (Impacto) — ¿Cuánto mejora la experiencia cuando se usa?**

Contribución de la feature a los objetivos clave del producto cuando un usuario interactúa con ella.

- **Escala:** 3 = Massive | 2 = High | 1 = Medium | 0.5 = Low | 0.25 = Minimal
- **Para LTI, medimos impacto en:**
  - **Diferenciación** vs ATS tradicionales (automatización + IA)
  - **Reducción de trabajo manual** de recruiters (objetivo: -40%)
  - **Adopción y engagement** de usuarios (daily active usage)

**Ejemplo:** Motor de automatización (US-005-02) tiene Impact = 3 (Massive) porque reduce trabajo manual en 40% cuando se usa activamente.

---

**3. Confidence (Confianza) — ¿Qué tan seguros estamos de Reach e Impact?**

Nivel de certeza en las estimaciones de Reach e Impact basado en datos, investigación de usuarios o hipótesis validadas.

- **Escala:** 100% = High | 80% = Medium | 50% = Low
- **Criterios para LTI:**
  - **100% (High):** Validado por research de mercado ATS (LTI-PCN.md, sección 1-3) o feature estándar de ATS
  - **80% (Medium):** Hipótesis fuerte basada en análisis de competidores pero sin validación directa de usuarios
  - **50% (Low):** Feature innovadora sin precedente claro en mercado ATS (especulativa)

**Ejemplo:** Vista kanban del pipeline (US-004-01) tiene Confidence = 100% porque el análisis de mercado confirma que UX visual es diferenciador probado.

---

**4. Effort (Esfuerzo) — ¿Cuánto tiempo/recursos requiere?**

Estimación de esfuerzo de desarrollo, medido en **persona-mes** (1 developer full-time trabajando 1 mes).

- **Conversión desde Story Points:**
  - **1-2 SP** = 0.25 persona-mes (~1 semana)
  - **3 SP** = 0.5 persona-mes (~2 semanas)
  - **5 SP** = 0.75 persona-mes (~3 semanas)
  - **8 SP** = 1.5 persona-mes (~6 semanas con 1 dev, 3 semanas con 2 devs)
  - **13 SP** = 2.5 persona-mes (~10 semanas con 1 dev, 5 semanas con 2 devs)

**Nota:** Asumimos equipo de 2 developers trabajando en paralelo, por lo que esfuerzos se pueden distribuir.

---

### Fórmula RICE Score

```
RICE Score = (Reach × Impact × Confidence) ÷ Effort
```

**Interpretación:**
- **Score > 100:** Prioridad crítica (alto impacto, bajo esfuerzo, alta confianza)
- **Score 50-100:** Alta prioridad (balance favorable)
- **Score 20-50:** Prioridad media (evaluación caso por caso)
- **Score < 20:** Baja prioridad (alto esfuerzo vs impacto limitado)

---

## Objetivos Estratégicos del PRD (Fuente de Inferencia)

Para asignar valores RICE precisos, recordamos los **KPIs del MVP** documentados en LTI-PCN.md:

1. **Diferenciación Inmediata:** Demostrar automatización no-code + IA operativa desde día 1 → Impact = 3 (Massive)
2. **Reducción de Trabajo Manual:** Objetivo cuantificable de -40% tiempo de recruiters → Reach alto en features core
3. **Validación con Early Adopters:** 3-5 pilotos ejecutando procesos completos → Confidence basada en factibilidad técnica
4. **Experiencia Visual Competitiva:** Pipeline kanban como "wow effect" → Impact alto en UX
5. **Arquitectura Multi-Tenant Escalable:** Base técnica desde día 1 → Effort alto pero Reach/Impact máximos

---

## Backlog Priorizado — RICE Score Descendente

### Análisis Individual de User Stories

---

### **🥇 #1 — US-001-02 — Login Multi-Tenant**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 100%          | **Todos** los usuarios (recruiters, managers, admins, candidatos) deben autenticarse para usar LTI. Feature universal. |
| **Impact**    | 3 (Massive)   | Sin autenticación segura no hay producto legalmente viable (GDPR, SOC2). Habilita multi-tenancy (aislamiento de datos por empresa). |
| **Confidence**| 100%          | Autenticación es tabla de stakes en SaaS. Certeza absoluta de necesidad y técnica (Auth0/Clerk probados). |
| **Effort**    | 1.5 PM        | 8 SP = ~6 semanas 1 dev, 3 semanas 2 devs. Incluye configuración Auth0, custom claims, middleware JWT. |
| **RICE Score**| **200.0**     | (100 × 3 × 1.0) ÷ 1.5 = 200                                                       |

**Prioridad:** #1 — Bloqueante absoluto de seguridad y compliance.

---

### **🥈 #2 — US-001-01 — Registro de Empresa (Company)**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 100%          | Cada empresa que adopta LTI debe registrarse primero. Feature universal para onboarding. |
| **Impact**    | 3 (Massive)   | Establece arquitectura multi-tenant (tabla `Company`, aislamiento por `company_id`). Sin esto, no hay SaaS escalable. |
| **Confidence**| 100%          | Registro de empresa es paso 1 estándar de cualquier SaaS B2B. Técnicamente trivial conceptualmente, complejo en implementación (Auth0 + SendGrid + migraciones). |
| **Effort**    | 1.5 PM        | 8 SP = ~3 semanas con 2 devs. Incluye migraciones Prisma, integración Auth0, emails SendGrid, validaciones. |
| **RICE Score**| **200.0**     | (100 × 3 × 1.0) ÷ 1.5 = 200                                                       |

**Prioridad:** #2 — Bloqueante técnico fundacional (empate con US-001-02, orden por dependencia).

---

### **🥉 #3 — US-003-02 — Vincular Candidato a Oferta (Application)**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 75%           | Usado por **recruiters** (no managers ni admins) para cada candidato que ingresa al proceso. Alta frecuencia de uso. |
| **Impact**    | 3 (Massive)   | Tabla `Application` conecta candidatos con ofertas y habilita **todo** el pipeline (estados, etapas, automatizaciones). Sin esto, no hay proceso de reclutamiento ejecutable. |
| **Confidence**| 100%          | Funcionalidad core de ATS. Certeza absoluta de necesidad. Técnicamente simple (3 SP). |
| **Effort**    | 0.5 PM        | 3 SP = ~2 semanas con 1 dev. Operación de asociación estándar en BD. |
| **RICE Score**| **450.0**     | (75 × 3 × 1.0) ÷ 0.5 = 450                                                        |

**Prioridad:** #3 — **Quick win crítico**: alto impacto, bajo esfuerzo, habilita pipeline completo.

---

### **#4 — US-002-01 — Crear Oferta con IA**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 75%           | Usado por **recruiters** y ocasionalmente **admins**. No todos los usuarios crean ofertas, pero todos las consumen. |
| **Impact**    | 3 (Massive)   | **Primer diferenciador visible** de LTI: IA embebida para mejorar descripciones. Core del pitch de ventas ("no es otro ATS más"). Habilita dominio `JobPosting`. |
| **Confidence**| 80%           | Validación fuerte: análisis de mercado (LTI-PCN.md) confirma que IA en creación de contenido es gap actual. Confianza media (no 100%) porque integración OpenAI/Claude tiene incertidumbre de latencia. |
| **Effort**    | 2.5 PM        | 13 SP = ~5 semanas con 2 devs. Incluye migraciones multi-tenant, integración IA, permisos, formularios complejos. |
| **RICE Score**| **72.0**      | (75 × 3 × 0.8) ÷ 2.5 = 72                                                         |

**Prioridad:** #4 — Core differentiator pero alto esfuerzo (13 SP).

---

### **#5 — US-006-01 — Análisis de CV con IA**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 75%           | Usado por **recruiters** para cada candidato nuevo. Alta frecuencia. Managers ven resultados pero no ejecutan. |
| **Impact**    | 2 (High)      | Reduce tiempo de screening de 10-15 min/candidato a 30 seg. Ahorro masivo de tiempo. Bloquea scoring (US-006-02). |
| **Confidence**| 80%           | Parsing de CVs con IA es técnica probada (muchos ATS ya lo tienen). Confianza alta pero no 100% por variabilidad de formatos CV. |
| **Effort**    | 1.5 PM        | 8 SP = ~3 semanas con 2 devs. Integración OpenAI/Claude + parsing PDFs con librerías especializadas. |
| **RICE Score**| **80.0**      | (75 × 2 × 0.8) ÷ 1.5 = 80                                                         |

**Prioridad:** #5 — IA operativa core, bloquea scoring.

---

### **#6 — US-003-01 — Registro Manual de Candidato**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 75%           | Usado por **recruiters** para sourcing activo (LinkedIn, referidos). No todos los candidatos se registran manualmente (algunos aplican directo). |
| **Impact**    | 3 (Massive)   | Sin candidatos no hay ATS. Habilita dominio `Candidate` y almacenamiento de CVs en S3. Bloqueante absoluto del flujo. |
| **Confidence**| 100%          | Registro de candidatos es tabla de stakes de ATS. Certeza técnica absoluta. |
| **Effort**    | 1.5 PM        | 8 SP = ~3 semanas con 2 devs. Incluye migraciones, integración S3, validaciones multi-tenant. |
| **RICE Score**| **150.0**     | (75 × 3 × 1.0) ÷ 1.5 = 150                                                        |

**Prioridad:** #6 — Core ATS, bloqueante del pipeline.

---

### **#7 — US-004-01 — Vista Kanban del Pipeline**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 100%          | **Todos** los usuarios (recruiters + managers) usan la vista kanban como interfaz principal para gestionar candidatos. |
| **Impact**    | 2 (High)      | **Diferenciador UX principal** y "wow effect" en demos. Convierte LTI en herramienta de trabajo diaria vs repositorio pasivo. |
| **Confidence**| 100%          | Análisis de mercado (LTI-PCN.md, sección 3) confirma que UX visual es ventaja competitiva probada vs ATS legacy. |
| **Effort**    | 1.5 PM        | 8 SP = ~3 semanas con 2 devs. UI compleja con React + real-time updates. |
| **RICE Score**| **133.3**     | (100 × 2 × 1.0) ÷ 1.5 = 133.3                                                     |

**Prioridad:** #7 — UX diferenciadora crítica.

---

### **#8 — US-004-02 — Mover Candidatos (Drag & Drop)**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 75%           | Usado por **recruiters** principalmente (managers ven, pero no mueven candidatos frecuentemente en MVP). |
| **Impact**    | 3 (Massive)   | Emite evento `ApplicationStageChanged` que **dispara motor de automatización**. Sin esto, automatizaciones no tienen trigger real. Crítico para arquitectura event-driven. |
| **Confidence**| 100%          | Drag & drop es patrón UX estándar en herramientas kanban (Trello, Jira). Técnicamente factible con librerías React. |
| **Effort**    | 1.5 PM        | 8 SP = ~3 semanas con 2 devs. Incluye drag & drop UI + eventos de dominio + registro en `ApplicationStageHistory`. |
| **RICE Score**| **150.0**     | (75 × 3 × 1.0) ÷ 1.5 = 150                                                        |

**Prioridad:** #8 — Bloquea automatizaciones (empate con US-003-01 en score, orden por dependencia lógica de pipeline).

---

### **#9 — US-006-02 — Scoring de Candidatos con IA**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 100%          | Score visible en **todas** las tarjetas del kanban para **todos** los usuarios (recruiters + managers). Feature universal de LTI. |
| **Impact**    | 2 (High)      | Priorización automática de candidatos top 20%. Reduce sesgo subjetivo. Diferenciador ético (DEI). Ahorro de tiempo cuantificable. |
| **Confidence**| 80%           | Scoring algorítmico existe en ATS modernos pero con precisión variable. Confianza media (no 100%) por dependencia de calidad de datos de input (CVs y job descriptions). |
| **Effort**    | 1.5 PM        | 8 SP = ~3 semanas con 2 devs. Modelo de scoring + UI + explicabilidad. |
| **RICE Score**| **106.7**     | (100 × 2 × 0.8) ÷ 1.5 = 106.7                                                     |

**Prioridad:** #9 — IA operativa visible, depende de US-006-01.

---

### **#10 — US-001-03 — Invitación de Usuarios**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 50%           | Usado por **admins** solo (no recruiters ni managers). Uso ocasional (onboarding inicial de equipo). |
| **Impact**    | 2 (High)      | Habilita **colaboración multi-usuario** (recruiters + managers trabajando juntos). Sin esto, LTI es herramienta single-user. |
| **Confidence**| 100%          | Invitación por email es patrón estándar de SaaS B2B. Técnicamente straightforward (tokens temporales + SendGrid). |
| **Effort**    | 0.75 PM       | 5 SP = ~3 semanas con 1 dev o 1.5 semanas con 2 devs. |
| **RICE Score**| **133.3**     | (50 × 2 × 1.0) ÷ 0.75 = 133.3                                                     |

**Prioridad:** #10 — Colaboración crítica pero no bloqueante de MVP técnico.

---

### **#11 — US-005-02 — Motor de Ejecución de Automatizaciones**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 75%           | Motor ejecuta automatizaciones para **todos** los procesos activos. Reach alto aunque usuarios no interactúan directamente (trabaja en background). |
| **Impact**    | 3 (Massive)   | **Core differentiator de LTI**: reduce trabajo manual en 40%. Sin motor, builder visual (US-005-01) es inútil. Propuesta de valor central del PRD. |
| **Confidence**| 80%           | Automatización event-driven es arquitectura probada (Zapier, Make). Confianza media (no 100%) por complejidad de implementación (suscripción a eventos, evaluación de reglas, ejecución de acciones, manejo de errores). |
| **Effort**    | 2.5 PM        | 13 SP = ~5 semanas con 2 devs. Arquitectura event-driven compleja. |
| **RICE Score**| **72.0**      | (75 × 3 × 0.8) ÷ 2.5 = 72                                                         |

**Prioridad:** #11 — Core differentiator pero alto esfuerzo. Depende de US-004-02 (eventos) y US-002-03 (publicación ofertas).

---

### **#12 — US-002-02 — Revisión y Aprobación de Oferta**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 50%           | Usado por **hiring managers** para aprobar ofertas. No todos los procesos requieren aprobación formal en MVP inicial. |
| **Impact**    | 1 (Medium)    | Mejora calidad de ofertas (reduce errores en requisitos) y añade trazabilidad. Workflow colaborativo deseable pero no bloqueante. |
| **Confidence**| 100%          | Aprobación de documentos es patrón estándar en tools colaborativas (Google Docs comments, Notion approvals). Técnicamente simple. |
| **Effort**    | 0.75 PM       | 5 SP = ~1.5 semanas con 2 devs. Workflow de estados + notificaciones + comentarios. |
| **RICE Score**| **66.7**      | (50 × 1 × 1.0) ÷ 0.75 = 66.7                                                      |

**Prioridad:** #12 — Colaboración deseable, no crítica para MVP mínimo.

---

### **#13 — US-002-03 — Publicar Oferta y Activar Automatizaciones**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 75%           | Usado por **recruiters** para cada oferta que sale a producción. Alta frecuencia. |
| **Impact**    | 2 (High)      | Crea evento `JobPostingPublished` que dispara automatizaciones iniciales. Habilita motor (US-005-02). Transición crítica de borrador a activo. |
| **Confidence**| 100%          | Publicación de ofertas con workflow de estados es lógica estándar de ATS. |
| **Effort**    | 0.75 PM       | 5 SP = ~1.5 semanas con 2 devs. Cambio de estado + creación de pipeline + eventos. |
| **RICE Score**| **200.0**     | (75 × 2 × 1.0) ÷ 0.75 = 200                                                       |

**Prioridad:** #13 — Bloquea automatizaciones, score alto pero depende de US-002-01 y US-002-02.

---

### **#14 — US-005-01 — Builder Visual de Automatizaciones**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 25%           | Usado por **admins** y algunos **recruiters avanzados** para configurar automatizaciones. No es daily-use feature para mayoría. |
| **Impact**    | 3 (Massive)   | **Core differentiator de LTI**: no-code automation accesible a RRHH sin IT. Sin builder, solo pueden usarse automatizaciones pre-configuradas (limita valor). |
| **Confidence**| 80%           | Builders no-code (Zapier, Make, Notion automations) son UX probada. Confianza media (no 100%) por complejidad de implementación (drag & drop builder es UI compleja). |
| **Effort**    | 2.5 PM        | 13 SP = ~5 semanas con 2 devs. UI drag & drop + configuración de acciones + guardado de reglas. |
| **RICE Score**| **24.0**      | (25 × 3 × 0.8) ÷ 2.5 = 24                                                         |

**Prioridad:** #14 — Diferenciador crítico pero reach limitado (admin-facing). Alto esfuerzo (13 SP) baja el score. Depende de US-005-02 (motor).

---

### **#15 — US-006-03 — Sugerencias de Siguiente Paso con IA**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 75%           | Visible para **recruiters** en cada candidato del pipeline. Uso frecuente. |
| **Impact**    | 1 (Medium)    | IA proactiva que acelera decisiones. "Nice to have" que mejora eficiencia pero no es bloqueante. Precisión mejora con datos históricos (limitado en MVP). |
| **Confidence**| 50%           | Feature innovadora sin precedente claro en ATS. Hipótesis especulativa sobre adopción real por usuarios. Baja confianza por falta de validación. |
| **Effort**    | 1.5 PM        | 8 SP = ~3 semanas con 2 devs. Modelo predictivo + integración con acciones + UI. |
| **RICE Score**| **25.0**      | (75 × 1 × 0.5) ÷ 1.5 = 25                                                         |

**Prioridad:** #15 — IA avanzada, baja confianza en adopción. Depende de US-006-02 y US-004-02.

---

### **#16 — US-003-03 — Vista de Perfil de Candidato**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 75%           | Usado por **recruiters** y **managers** para revisar candidatos en detalle antes de entrevistas. |
| **Impact**    | 1 (Medium)    | UX mejorada que añade contexto (historial, feedback). Deseable pero no bloqueante (info básica ya visible en tarjetas kanban). |
| **Confidence**| 100%          | Vista de perfil detallada es patrón estándar de ATS. Técnicamente trivial (UI con datos existentes). |
| **Effort**    | 0.5 PM        | 3 SP = ~2 semanas con 1 dev. UI con queries a datos ya almacenados. |
| **RICE Score**| **150.0**     | (75 × 1 × 1.0) ÷ 0.5 = 150                                                        |

**Prioridad:** #16 — **Quick win** de UX pero no crítico para MVP mínimo.

---

### **#17 — US-004-03 — Historial de Cambios de Etapa**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 50%           | Usado ocasionalmente por **recruiters** y **managers** para auditoría o análisis post-mortem. No es daily-use feature. |
| **Impact**    | 1 (Medium)    | Transparencia y auditoría valiosas para compliance. Mejora confianza en sistema pero no impacta eficiencia operativa diaria. |
| **Confidence**| 100%          | Historial de cambios es patrón estándar de auditoría. Datos ya se registran en `ApplicationStageHistory` desde US-004-02. |
| **Effort**    | 0.75 PM       | 5 SP = ~1.5 semanas con 2 devs. Query + UI timeline. |
| **RICE Score**| **66.7**      | (50 × 1 × 1.0) ÷ 0.75 = 66.7                                                      |

**Prioridad:** #17 — Auditoría deseable pero no urgente.

---

### **#18 — US-005-03 — Logs y Monitoreo de Automatizaciones**

| **Dimensión** | **Valor**     | **Justificación**                                                                 |
|---------------|---------------|-----------------------------------------------------------------------------------|
| **Reach**     | 25%           | Usado por **admins** y **soporte técnico** para debugging. No es user-facing feature para recruiters. |
| **Impact**    | 1 (Medium)    | Mejora confianza en automatizaciones y facilita self-service debugging. Deseable pero no bloqueante (logs existen en sistema, solo falta UI). |
| **Confidence**| 100%          | Logs de ejecución son patrón estándar de sistemas automatizados. Técnicamente straightforward. |
| **Effort**    | 0.75 PM       | 5 SP = ~1.5 semanas con 2 devs. UI tabla filtrable + queries sobre `AutomationEvent`. |
| **RICE Score**| **33.3**      | (25 × 1 × 1.0) ÷ 0.75 = 33.3                                                      |

**Prioridad:** #18 — Admin-facing, bajo reach. Útil para soporte pero no para adopción.

---

## Tabla Resumen — Backlog Ordenado por RICE Score

| **Orden** | **ID**       | **Título**                                 | **Reach** | **Impact** | **Confidence** | **Effort (PM)** | **RICE Score** | **Épica**                |
|-----------|--------------|-------------------------------------------|-----------|------------|----------------|-----------------|----------------|--------------------------|
| **1**     | US-003-02    | Vincular Candidato a Oferta               | 75%       | 3          | 100%           | 0.5             | **450.0**      | EPIC-003 (Candidatos)    |
| **2**     | US-001-02    | Login Multi-Tenant                        | 100%      | 3          | 100%           | 1.5             | **200.0**      | EPIC-001 (Usuarios)      |
| **3**     | US-001-01    | Registro de Empresa                       | 100%      | 3          | 100%           | 1.5             | **200.0**      | EPIC-001 (Usuarios)      |
| **4**     | US-002-03    | Publicar Oferta y Activar Automatizaciones| 75%       | 2          | 100%           | 0.75            | **200.0**      | EPIC-002 (Ofertas)       |
| **5**     | US-003-01    | Registro Manual de Candidato              | 75%       | 3          | 100%           | 1.5             | **150.0**      | EPIC-003 (Candidatos)    |
| **6**     | US-004-02    | Mover Candidatos (Drag & Drop)            | 75%       | 3          | 100%           | 1.5             | **150.0**      | EPIC-004 (Pipeline)      |
| **7**     | US-003-03    | Vista de Perfil de Candidato              | 75%       | 1          | 100%           | 0.5             | **150.0**      | EPIC-003 (Candidatos)    |
| **8**     | US-004-01    | Vista Kanban del Pipeline                 | 100%      | 2          | 100%           | 1.5             | **133.3**      | EPIC-004 (Pipeline)      |
| **9**     | US-001-03    | Invitación de Usuarios                    | 50%       | 2          | 100%           | 0.75            | **133.3**      | EPIC-001 (Usuarios)      |
| **10**    | US-006-02    | Scoring de Candidatos con IA              | 100%      | 2          | 80%            | 1.5             | **106.7**      | EPIC-006 (IA Operativa)  |
| **11**    | US-006-01    | Análisis de CV con IA                     | 75%       | 2          | 80%            | 1.5             | **80.0**       | EPIC-006 (IA Operativa)  |
| **12**    | US-002-01    | Crear Oferta con IA                       | 75%       | 3          | 80%            | 2.5             | **72.0**       | EPIC-002 (Ofertas)       |
| **13**    | US-005-02    | Motor de Ejecución de Automatizaciones    | 75%       | 3          | 80%            | 2.5             | **72.0**       | EPIC-005 (Automatización)|
| **14**    | US-002-02    | Revisión y Aprobación de Oferta           | 50%       | 1          | 100%           | 0.75            | **66.7**       | EPIC-002 (Ofertas)       |
| **15**    | US-004-03    | Historial de Cambios de Etapa             | 50%       | 1          | 100%           | 0.75            | **66.7**       | EPIC-004 (Pipeline)      |
| **16**    | US-005-03    | Logs y Monitoreo de Automatizaciones      | 25%       | 1          | 100%           | 0.75            | **33.3**       | EPIC-005 (Automatización)|
| **17**    | US-006-03    | Sugerencias de Siguiente Paso con IA      | 75%       | 1          | 50%            | 1.5             | **25.0**       | EPIC-006 (IA Operativa)  |
| **18**    | US-005-01    | Builder Visual de Automatizaciones        | 25%       | 3          | 80%            | 2.5             | **24.0**       | EPIC-005 (Automatización)|

---

## Insights del Análisis RICE

### Hallazgos Clave

**1. Quick Win Inesperado: US-003-02 (RICE Score 450) domina el ranking**

- **Vincular Candidato a Oferta** es la historia con mayor RICE score (450) porque combina:
  - **Alto impacto** (3): Habilita todo el pipeline y automatizaciones
  - **Bajo esfuerzo** (3 SP = 0.5 PM): Operación simple de asociación en BD
  - **Alta confianza** (100%): Feature estándar de ATS
  
- **Recomendación:** Priorizar US-003-02 inmediatamente después de infraestructura básica (US-001-01, US-001-02) para desbloquear valor rápidamente.

---

**2. Empate Técnico en Infraestructura Fundacional (RICE Score 200)**

- **US-001-01** (Registro Empresa), **US-001-02** (Login), y **US-002-03** (Publicar Oferta) empatan con RICE = 200.
- Todos son bloqueantes técnicos con máximo reach/impact pero esfuerzo moderado-alto.
- **Orden de implementación por dependencias:**
  1. US-001-01 (crea tabla `Company`)
  2. US-001-02 (habilita autenticación)
  3. US-002-03 (requiere ofertas existentes)

---

**3. Features de 13 SP tienen RICE scores bajos (24-72) por alto esfuerzo**

- **US-002-01** (Crear Oferta con IA): RICE = 72
- **US-005-02** (Motor Automatización): RICE = 72
- **US-005-01** (Builder Visual): RICE = 24

**Insight:** Features complejas (13 SP = 2.5 PM) tienen scores bajos porque el denominador "Effort" penaliza fuertemente. Sin embargo, estas features son **core differentiators estratégicos** del PRD:
- US-002-01: Primera demo de IA operativa
- US-005-02: Reduce trabajo manual en 40% (objetivo MVP)
- US-005-01: No-code automation es propuesta de valor central

**Recomendación:** No descartar features de 13 SP solo por RICE score bajo. Evaluar impacto estratégico en diferenciación vs competidores (cualitativo) además de RICE (cuantitativo).

---

**4. Features de IA Avanzada (US-006-03) tienen baja confianza (50%)**

- **US-006-03** (Sugerencias de IA): RICE = 25 debido a **Confidence = 50%**
- Es feature innovadora sin precedente claro en mercado ATS → alta incertidumbre de adopción.

**Recomendación:** Postergar US-006-03 a fase post-MVP hasta validar adopción de IA básica (US-006-01, US-006-02) con early adopters.

---

**5. Features Admin-Facing tienen reach bajo (25-50%)**

- **US-005-01** (Builder): Reach = 25% (solo admins)
- **US-005-03** (Logs): Reach = 25% (solo admins/soporte)
- **US-001-03** (Invitaciones): Reach = 50% (solo admins)

**Insight:** Reach bajo impacta score aunque impact sea alto. Sin embargo, features admin-facing son **habilitadoras** de valor para otros usuarios:
- Builder (US-005-01) permite a admins configurar automatizaciones que benefician a todos los recruiters.
- Invitaciones (US-001-03) habilitan colaboración multi-usuario.

**Recomendación:** No ignorar features admin-facing solo por reach bajo. Evaluar si habilitan valor downstream para usuarios finales.

---

## Conclusión: Composición del Sprint 1 según RICE

### Criterio de Selección

Para Sprint 1 (2 semanas, velocidad estimada 20-25 SP), seleccionamos historias que:

1. **RICE Score > 100** (prioridad crítica o alta)
2. **Bloqueantes técnicos** (dependencias de 0)
3. **Mix de backend/frontend** para distribución de trabajo
4. **Total ≤ 25 SP** para no exceder capacidad del equipo

---

### Sprint 1 Recomendado (RICE-Driven)

| **Historia** | **RICE Score** | **SP** | **Justificación**                                                       |
|--------------|----------------|--------|-------------------------------------------------------------------------|
| US-001-01    | 200.0          | 8      | Bloqueante técnico #1: arquitectura multi-tenant                        |
| US-001-02    | 200.0          | 8      | Bloqueante técnico #2: autenticación segura                             |
| US-003-02    | 450.0          | 3      | Quick win crítico: habilita pipeline (bajo esfuerzo, alto impacto)      |
| US-001-03    | 133.3          | 5      | Colaboración multi-usuario (completa EPIC-001)                          |
| **TOTAL**    | —              | **24 SP** | Dentro de velocidad estimada (20-25 SP)                              |

---

### Justificación Estratégica del Sprint 1

**✅ Cobertura de Bloqueantes:**
- US-001-01 y US-001-02 establecen base técnica (multi-tenancy + auth) para todas las demás historias.

**✅ Quick Win Temprano:**
- US-003-02 (RICE 450) puede completarse en ~2 semanas y demuestra valor inmediato (candidatos vinculados a ofertas).

**✅ Habilitación de Colaboración:**
- US-001-03 cierra EPIC-001 (Gestión de Usuarios) y permite invitar hiring managers para pilotos reales.

**✅ Balance Backend/Frontend:**
- Backend: US-001-01 (migraciones, Auth0), US-001-02 (middleware JWT)
- Frontend: US-001-03 (formulario invitaciones), US-003-02 (vinculación UI)

**✅ Preparación para Sprint 2:**
- Con US-001-01/02/03 completados, Sprint 2 puede abordar US-002-01 (Crear Oferta con IA) y US-003-01 (Registro Candidato) sin blockers.

---

### Roadmap Completo basado en RICE (6-7 Sprints)

#### **Sprint 1 (24 SP): Infraestructura + Quick Win**
- US-001-01, US-001-02, US-003-02, US-001-03

#### **Sprint 2 (23 SP): Core ATS + Primera IA**
- US-002-01 (13 SP) — Crear Oferta con IA
- US-003-01 (8 SP) — Registro Manual de Candidato
- US-003-03 (3 SP) — Vista Perfil Candidato (quick win)

#### **Sprint 3 (21 SP): Pipeline Visual**
- US-004-01 (8 SP) — Vista Kanban
- US-004-02 (8 SP) — Drag & Drop
- US-002-03 (5 SP) — Publicar Oferta

#### **Sprint 4 (21 SP): IA Operativa Completa**
- US-006-01 (8 SP) — Análisis CV con IA
- US-006-02 (8 SP) — Scoring Candidatos
- US-004-03 (5 SP) — Historial Etapas

#### **Sprint 5 (26 SP): Automatización Core**
- US-005-02 (13 SP) — Motor de Automatización
- US-005-01 (13 SP) — Builder Visual
- **Nota:** Sprint intenso con 2 features de 13 SP. Considerar split o pair programming.

#### **Sprint 6 (10 SP): Revisión + Features Admin**
- US-002-02 (5 SP) — Revisión y Aprobación Oferta
- US-005-03 (5 SP) — Logs Automatizaciones

#### **Sprint 7 (Opcional, 8 SP): IA Avanzada**
- US-006-03 (8 SP) — Sugerencias Siguiente Paso IA
- Solo si Sprint 5 no pudo absorber US-005-01 completo.

---

### Métricas de Éxito del Sprint 1

**Validación Técnica:**
- ✅ Arquitectura multi-tenant funcional con 0 cross-tenant data leaks
- ✅ Autenticación Auth0/Clerk integrada con JWT + custom claims
- ✅ 3 empresas de prueba registradas con usuarios invitados

**Validación de Producto:**
- ✅ 1 candidato vinculado a 1 oferta (tabla `Application` funcional)
- ✅ Invitaciones por email entregadas con tasa de éxito > 95%

**Validación de Velocidad:**
- ✅ 24 SP completados en 2 semanas (validación de velocidad estimada)
- ✅ 0 deuda técnica crítica arrastrada a Sprint 2

---

## Limitaciones del Método RICE

**1. No captura dependencias técnicas directamente**

- RICE es puramente cuantitativo (Reach × Impact × Confidence ÷ Effort).
- No modela explícitamente que US-001-01 **bloquea** US-002-01.
- **Mitigación:** Reordenar manualmente historias con RICE similar según grafo de dependencias.

**2. Confidence es subjetivo sin datos de usuarios reales**

- En MVP sin clientes, Confidence es inferencia de análisis de mercado.
- Ejemplo: US-006-03 (Sugerencias IA) tiene Confidence = 50% porque es feature innovadora, pero podría ser 100% si validamos con prototipos.
- **Mitigación:** Iterar valores de Confidence tras sprints 1-2 con feedback de pilotos.

**3. Penaliza fuertemente features complejas (13 SP)**

- US-005-01 (Builder) tiene RICE = 24 pese a ser core differentiator porque Effort = 2.5 PM.
- Método RICE favorece quick wins sobre inversiones estratégicas de largo plazo.
- **Mitigación:** Balancear RICE score con análisis cualitativo de diferenciación competitiva del PRD.

**4. No modela valor acumulativo de épicas completas**

- Completar EPIC-001 (Usuarios) al 100% tiene valor mayor que suma de historias individuales (coherencia de experiencia).
- RICE evalúa historias aisladamente.
- **Mitigación:** Preferir completar épicas enteras antes de saltar a nueva épica (evita work-in-progress fragmentado).

---

## Recomendación Final

El método **RICE** es excelente para **priorización objetiva inicial** basada en datos cuantitativos (reach, impact, effort). Sin embargo, debe **complementarse** con:

1. **Análisis de dependencias técnicas** (grafo de bloqueantes)
2. **Evaluación cualitativa de diferenciación** vs competidores (del PRD)
3. **Feedback continuo de early adopters** (ajustar Confidence tras validación)
4. **Coherencia de roadmap** (completar épicas, no fragmentar features)

**Sprint 1 propuesto (24 SP):** US-001-01, US-001-02, US-003-02, US-001-03

Este sprint establece infraestructura crítica (multi-tenancy + auth), demuestra quick win (vinculación candidatos), y habilita colaboración multi-usuario para pilotos reales.

---

*Documento generado para priorización data-driven del MVP de LTI Platform*  
*Método RICE aplicado con valores inferidos del PRD y análisis de mercado ATS*  
*Última actualización: 2025-12-02*
