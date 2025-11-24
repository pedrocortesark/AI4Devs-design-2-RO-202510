# User Stories y Product Backlog — LTI Platform

> Documento único de entrega del desglose funcional y priorización del proyecto LTI.  
> Fuente de verdad: `LTI-PCN.md` — Todas las decisiones de roadmap, épicas y user stories se fundamentan en el análisis de mercado, posicionamiento estratégico y arquitectura ya definidos.

---

## Introducción

Este documento consolida el **Product Backlog** de LTI, organizado en un roadmap de 6 meses que prioriza valor de negocio y viabilidad técnica. LTI es una **plataforma de orquestación de talento** que combina un ATS moderno con automatización no-code y una capa de IA operativa. Está diseñada para startups y scale-ups tecnológicas (50–1000 empleados) que buscan reducir drásticamente el trabajo manual en reclutamiento, mejorar la colaboración con hiring managers y ofrecer una experiencia transparente a los candidatos.

El roadmap se ha construido siguiendo los principios de un MVP serio: **valor rápido, aprendizaje temprano y base sólida para escalar**. Cada iteración agrega funcionalidades clave que demuestran la diferenciación de LTI frente a ATS tradicionales y habilitan ciclos de feedback con clientes early adopter.

---

## Roadmap de 6 Meses — Estrategia de Valor

El roadmap se divide en **3 fases principales**, cada una con épicas asociadas y un objetivo de negocio claro:

### **Fase 1 — MVP Fundacional (Meses 1-2): Core ATS + Automatización Básica + IA Operativa**

**Objetivo:** Lanzar un producto mínimo viable que permita a recruiters gestionar ofertas y candidatos con un pipeline visual y configurar automatizaciones básicas. Incluir IA desde el día 1 para diferenciación inmediata.

**Valor de negocio:**
- Demostrar que LTI no es "otro ATS más", sino una plataforma con inteligencia operativa.
- Reducir el tiempo de coordinación de recruiters mediante automatizaciones simples (emails automáticos, notificaciones).
- Validar la propuesta de valor con early adopters (startups tecnológicas de 50-300 empleados).

**Épicas incluidas:**
1. **EPIC-001-GestionUsuarios**: Registro, autenticación multi-tenant y permisos por rol (Recruiter, Hiring Manager, Admin).
2. **EPIC-002-GestionOfertas**: Crear, editar, revisar y publicar ofertas con asistencia de IA (mejora de descripciones).
3. **EPIC-003-GestionCandidatos**: Registro de candidatos, aplicaciones a ofertas, perfil básico y vinculación con fuentes.
4. **EPIC-004-PipelineVisual**: Vista kanban del pipeline, movimiento de candidatos entre etapas, histórico básico.
5. **EPIC-005-AutomatizacionBasica**: Builder no-code inicial para reglas simples (emails automáticos al cambiar etapa, notificaciones a managers).
6. **EPIC-006-IAOperativaInicial**: Resúmenes de CVs, mejora de descripciones de rol, scoring básico de candidatos por etapa.

**Justificación del orden:**
- Empezamos con infraestructura de usuarios y autenticación (EPIC-001) porque es transversal y habilita el resto.
- Ofertas y candidatos (EPIC-002, EPIC-003) son el dominio core del ATS: sin ellos no hay producto.
- El pipeline visual (EPIC-004) es el diferenciador UX más visible y valida la usabilidad de LTI.
- Automatización básica (EPIC-005) es lo que separa LTI de ATS tradicionales: debe estar en el MVP para demostrar propuesta de valor.
- La IA operativa (EPIC-006) cierra el MVP mostrando inteligencia embebida en el flujo de trabajo, no como feature aislada.

---

### **Fase 2 — Colaboración y Experiencia (Meses 3-4): Feedback, Decisiones y Candidate Experience**

**Objetivo:** Habilitar la colaboración estructurada entre recruiters y hiring managers, mejorar la experiencia del candidato y enriquecer las automatizaciones.

**Valor de negocio:**
- Reducir la fricción entre recruiters y managers (feedback tardío, decisiones repartidas entre canales).
- Aumentar la transparencia hacia candidatos, mejorando employer branding y reduciendo "agujeros negros".
- Consolidar LTI como hub operativo de reclutamiento, no solo como repositorio de CVs.

**Épicas incluidas:**
7. **EPIC-007-FeedbackColaborativo**: Sistema de feedback estructurado para entrevistas, scorecards, comentarios de hiring managers y recruiters.
8. **EPIC-008-DecisionesFinales**: Registro explícito de decisiones (Enviar oferta, Rechazar, Seguir en proceso) con trazabilidad y automatizaciones asociadas.
9. **EPIC-009-CandidateExperience**: Portal para candidatos con estado del proceso, mensajes personalizados y feedback post-rechazo.
10. **EPIC-010-NotificacionesAvanzadas**: Integración con Slack/Teams para notificaciones en tiempo real, resúmenes de candidatos y decisiones rápidas.
11. **EPIC-011-AutomatizacionAvanzada**: Ampliación del builder no-code con condiciones complejas, acciones múltiples y conectores a herramientas externas (calendarios, Zoom, evaluaciones técnicas).

**Justificación del orden:**
- Feedback y decisiones (EPIC-007, EPIC-008) son críticos para colaboración: sin ellos, managers no adoptan la herramienta.
- Candidate experience (EPIC-009) es un diferenciador de marca y reduce carga operativa de recruiters (menos consultas de candidatos).
- Notificaciones avanzadas (EPIC-010) y automatización avanzada (EPIC-011) profundizan en la propuesta de valor de "orquestación inteligente".

---

### **Fase 3 — Insights y Escalabilidad (Meses 5-6): Reporting, Reutilización de Talento y Optimización**

**Objetivo:** Convertir LTI en una plataforma de decisiones basadas en datos, habilitar reutilización de candidatos y preparar el sistema para escalar.

**Valor de negocio:**
- Proveer visibilidad de métricas clave (time-to-hire, conversión por etapa, source-of-hire) orientadas a acción.
- Activar el grafo de talento interno/externo para movilidad interna y reducción de costes de sourcing.
- Demostrar impacto cuantificable de LTI en eficiencia de reclutamiento.

**Épicas incluidas:**
12. **EPIC-012-ReportingEsencial**: Dashboards de métricas clave (tiempo medio por etapa, conversión, fuentes de candidatos), vistas por oferta y por empresa.
13. **EPIC-013-ReutilizacionTalento**: Sistema de recomendación de candidatos previos para nuevas ofertas (silver medalists, talent pools).
14. **EPIC-014-MovilidadInterna**: Integración con HRIS para identificar talento interno apto para nuevas vacantes y facilitar movilidad.
15. **EPIC-015-OptimizacionIA**: Mejoras en recomendaciones de IA (priorización predictiva, detección de riesgos de caída, sugerencias de próximos pasos).
16. **EPIC-016-IntegracionesExtendidas**: Conectores adicionales con job boards, herramientas de evaluación técnica, CRMs de reclutamiento y APIs abiertas para clientes enterprise.

**Justificación del orden:**
- Reporting (EPIC-012) da visibilidad estratégica a C-level y justifica ROI de LTI.
- Reutilización de talento (EPIC-013) y movilidad interna (EPIC-014) son diferenciadores potentes frente a ATS tradicionales y activan el concepto de "Talent Orchestration Platform".
- Optimización de IA (EPIC-015) e integraciones extendidas (EPIC-016) preparan a LTI para clientes más sofisticados y para escalar hacia mid-market.

---

## Resumen del Roadmap

| **Fase**               | **Duración** | **Épicas**                                                                 | **Objetivo de Negocio**                                      |
|------------------------|--------------|---------------------------------------------------------------------------|-------------------------------------------------------------|
| **Fase 1 — MVP**       | Meses 1-2    | EPIC-001 a EPIC-006                                                       | Lanzar producto diferenciado (ATS + Automatización + IA)    |
| **Fase 2 — Colaboración** | Meses 3-4    | EPIC-007 a EPIC-011                                                       | Habilitar colaboración y mejorar experiencia de candidatos   |
| **Fase 3 — Insights**  | Meses 5-6    | EPIC-012 a EPIC-016                                                       | Convertir LTI en plataforma de decisiones y escalabilidad    |

---

## Épicas del Backlog

A continuación se listan las épicas del roadmap. Cada épica está documentada en detalle en su archivo correspondiente dentro de la subcarpeta `Agile/Epics/`.

### Fase 1 — MVP Fundacional (Meses 1-2)

1. [EPIC-001-GestionUsuarios](Agile/Epics/EPIC-001-GestionUsuarios.md)
2. [EPIC-002-GestionOfertas](Agile/Epics/EPIC-002-GestionOfertas.md)
3. [EPIC-003-GestionCandidatos](Agile/Epics/EPIC-003-GestionCandidatos.md)
4. [EPIC-004-PipelineVisual](Agile/Epics/EPIC-004-PipelineVisual.md)
5. [EPIC-005-AutomatizacionBasica](Agile/Epics/EPIC-005-AutomatizacionBasica.md)
6. [EPIC-006-IAOperativaInicial](Agile/Epics/EPIC-006-IAOperativaInicial.md)

### Fase 2 — Colaboración y Experiencia (Meses 3-4)

7. [EPIC-007-FeedbackColaborativo](Agile/Epics/EPIC-007-FeedbackColaborativo.md)
8. [EPIC-008-DecisionesFinales](Agile/Epics/EPIC-008-DecisionesFinales.md)
9. [EPIC-009-CandidateExperience](Agile/Epics/EPIC-009-CandidateExperience.md)
10. [EPIC-010-NotificacionesAvanzadas](Agile/Epics/EPIC-010-NotificacionesAvanzadas.md)
11. [EPIC-011-AutomatizacionAvanzada](Agile/Epics/EPIC-011-AutomatizacionAvanzada.md)

### Fase 3 — Insights y Escalabilidad (Meses 5-6)

12. [EPIC-012-ReportingEsencial](Agile/Epics/EPIC-012-ReportingEsencial.md)
13. [EPIC-013-ReutilizacionTalento](Agile/Epics/EPIC-013-ReutilizacionTalento.md)
14. [EPIC-014-MovilidadInterna](Agile/Epics/EPIC-014-MovilidadInterna.md)
15. [EPIC-015-OptimizacionIA](Agile/Epics/EPIC-015-OptimizacionIA.md)
16. [EPIC-016-IntegracionesExtendidas](Agile/Epics/EPIC-016-IntegracionesExtendidas.md)

---

## Criterios de Priorización Aplicados

El roadmap ha sido construido siguiendo estos criterios:

- **Valor de negocio inmediato**: Funcionalidades que demuestran diferenciación (automatización, IA) están en el MVP.
- **Viabilidad técnica**: Las épicas más complejas (movilidad interna, integraciones extendidas) se posponen a fase 3.
- **Feedback temprano**: El MVP (Fase 1) incluye suficiente valor para validar encaje con early adopters.
- **Secuencia lógica**: Infraestructura (usuarios, auth) → dominio core (ofertas, candidatos, pipeline) → diferenciadores (automatización, IA) → colaboración → insights.
- **Escalabilidad progresiva**: Cada fase construye sobre la anterior, sin rupturas arquitectónicas.

---

## Product Backlog (Detalle)

Las siguientes User Stories detallan las funcionalidades de las 6 épicas de la Fase 1 (MVP). Cada historia incluye narrativa en formato estándar, criterios de aceptación en Gherkin, tareas técnicas y atributos de estimación (Esfuerzo, Valor, Urgencia, Dependencias).

### EPIC-001 — Gestión de Usuarios

1. [US-001-01-RegistroEmpresa](Agile/UserStories/US-001-01-RegistroEmpresa.md) — Registro de Empresa (Company)
2. [US-001-02-LoginUsuario](Agile/UserStories/US-001-02-LoginUsuario.md) — Login de Usuario con Autenticación Multi-Tenant
3. [US-001-03-InvitacionUsuarios](Agile/UserStories/US-001-03-InvitacionUsuarios.md) — Invitación de Usuarios por Admin

### EPIC-002 — Gestión de Ofertas

1. [US-002-01-CrearOfertaConIA](Agile/UserStories/US-002-01-CrearOfertaConIA.md) — Crear Oferta de Trabajo con Asistencia de IA
2. [US-002-02-RevisionAprobacionOferta](Agile/UserStories/US-002-02-RevisionAprobacionOferta.md) — Revisión y Aprobación de Oferta por Hiring Manager
3. [US-002-03-PublicarOfertaAutomatizaciones](Agile/UserStories/US-002-03-PublicarOfertaAutomatizaciones.md) — Publicar Oferta y Activar Automatizaciones Iniciales

### EPIC-003 — Gestión de Candidatos

1. [US-003-01-RegistroCandidato](Agile/UserStories/US-003-01-RegistroCandidato.md) — Registro Manual de Candidato
2. [US-003-02-AplicacionCandidato](Agile/UserStories/US-003-02-AplicacionCandidato.md) — Vincular Candidato a Oferta (Application)
3. [US-003-03-PerfilCandidato](Agile/UserStories/US-003-03-PerfilCandidato.md) — Vista de Perfil de Candidato

### EPIC-004 — Pipeline Visual

1. [US-004-01-VistaKanban](Agile/UserStories/US-004-01-VistaKanban.md) — Vista Kanban del Pipeline de Candidatos
2. [US-004-02-DragDropCandidatos](Agile/UserStories/US-004-02-DragDropCandidatos.md) — Mover Candidatos entre Etapas (Drag & Drop)
3. [US-004-03-HistorialEtapas](Agile/UserStories/US-004-03-HistorialEtapas.md) — Historial de Cambios de Etapa

### EPIC-005 — Automatización Básica

1. [US-005-01-BuilderAutomatizaciones](Agile/UserStories/US-005-01-BuilderAutomatizaciones.md) — Builder Visual de Reglas de Automatización
2. [US-005-02-MotorEjecucionAutomatizaciones](Agile/UserStories/US-005-02-MotorEjecucionAutomatizaciones.md) — Motor de Ejecución de Reglas de Automatización
3. [US-005-03-LogsAutomatizaciones](Agile/UserStories/US-005-03-LogsAutomatizaciones.md) — Logs y Monitoreo de Automatizaciones

### EPIC-006 — IA Operativa Inicial

1. [US-006-01-AnalisisCVconIA](Agile/UserStories/US-006-01-AnalisisCVconIA.md) — Análisis de CV con IA
2. [US-006-02-ScoringCandidatosIA](Agile/UserStories/US-006-02-ScoringCandidatosIA.md) — Scoring de Candidatos con IA
3. [US-006-03-SugerenciasSiguientePasoIA](Agile/UserStories/US-006-03-SugerenciasSiguientePasoIA.md) — Sugerencias de Siguiente Paso con IA

---

## Metodología de Priorización

Esta sección documenta el enfoque sistemático utilizado para priorizar el backlog de la Fase 1 del MVP de LTI. La priorización combina análisis cuantitativo de atributos (esfuerzo, valor, urgencia) con evaluación cualitativa de dependencias y riesgos técnicos.

### Marco de Estimación

**1. Esfuerzo (Story Points — Escala Fibonacci)**

Estimación relativa de complejidad técnica y tamaño de la historia, utilizando la serie de Fibonacci: **1, 2, 3, 5, 8, 13**.

- **1 SP**: Tarea trivial, < 2 horas (ej. cambio de texto, config menor)
- **2 SP**: Cambio pequeño en componente existente, sin lógica compleja
- **3 SP**: Feature pequeña, 1-2 archivos, sin integraciones externas
- **5 SP**: Feature mediana, varios componentes, lógica de negocio estándar
- **8 SP**: Feature compleja, integraciones, varios dominios, alto acoplamiento
- **13 SP**: Feature muy compleja, arquitectura nueva, incertidumbre técnica alta

**Justificación**: Fibonacci refleja incertidumbre creciente a medida que el esfuerzo aumenta. Es compatible con planning poker y facilita consenso en equipos Agile.

**2. Valor de Negocio (Escala 1-5)**

Impacto de la historia en la propuesta de valor de LTI y adopción por early adopters.

- **1**: Nice-to-have, no impacta experiencia core
- **2**: Mejora incremental de UX o eficiencia operativa menor
- **3**: Feature relevante pero no crítica para MVP
- **4**: Alta contribución a diferenciación o reducción de fricción clave
- **5**: Crítica para propuesta de valor o bloqueante para uso del producto

**Justificación**: Prioriza historias que demuestran diferenciación de LTI (automatización, IA) y eliminan blockers críticos.

**3. Urgencia (Escala 1-5)**

Presión temporal o riesgo de bloqueo del roadmap si la historia se pospone.

- **1**: Puede esperar múltiples sprints sin impacto
- **2**: Deseable en próximos 2-3 sprints
- **3**: Importante incluir en MVP pero no inmediato
- **4**: Necesaria en corto plazo (riesgo de bloqueo de otras historias)
- **5**: Bloqueante crítico, debe ejecutarse inmediatamente

**Justificación**: Urgencia complementa valor — una historia puede tener alto valor pero baja urgencia si no hay dependencias críticas inmediatas.

**4. Dependencias Técnicas**

Identificación explícita de historias que bloquean la ejecución de otras. Ejemplos:
- `US-001-01-RegistroEmpresa` bloquea toda la aplicación (sin multi-tenancy no hay aislamiento de datos)
- `US-002-03-PublicarOfertaAutomatizaciones` debe preceder a `US-005-02-MotorEjecucionAutomatizaciones` (sin eventos no hay motor)
- `US-006-01-AnalisisCVconIA` debe preceder a `US-006-02-ScoringCandidatosIA` (scoring requiere insights extraídos del CV)

**Justificación**: Dependencias tienen prioridad absoluta sobre valor/urgencia — no se puede construir sobre cimientos inexistentes.

### Algoritmo de Priorización

El backlog se ordena aplicando el siguiente algoritmo de decisión en cascada:

```
PASO 1: Ordenar por Dependencias Críticas
  ↓ Historias con dependencias de 0 (no bloquean otras) van al final
  ↓ Historias que bloquean múltiples otras van al principio

PASO 2: Identificar Quick Wins (dentro de historias sin bloqueos pendientes)
  ↓ Ratio Valor/Esfuerzo > 1 (ej. Valor=5, Esfuerzo=3 → Ratio=1.67)
  ↓ Quick wins se ejecutan temprano para generar momentum

PASO 3: Ordenar Higiénicos del MVP (historias con Urgencia=5)
  ↓ Security, autenticación, permisos críticos antes de features vistosas
  ↓ Infraestructura técnica (logging, eventos) antes de automatización

PASO 4: Balancear Valor vs. Complejidad
  ↓ Si dos historias tienen similar prioridad, preferir menor esfuerzo
  ↓ Evitar acumular múltiples 13 SP consecutivos (riesgo de burnout)

PASO 5: Validación de Coherencia de Roadmap
  ↓ Verificar que cada sprint tenga mix de backend/frontend
  ↓ Verificar que épicas se completen secuencialmente (no saltar entre épicas)
```

**Justificación del enfoque híbrido**:
- **No usamos solo ROI** (Valor/Esfuerzo) porque ignora dependencias y urgencia
- **No usamos solo MoSCoW** (Must/Should/Could/Won't) porque es subjetivo y no cuantificable
- **No usamos solo WSJF** (Weighted Shortest Job First) porque no refleja dependencias técnicas del MVP

El algoritmo híbrido combina lo mejor de métodos cuantitativos (scoring) y cualitativos (dependencias, coherencia de roadmap).

### Aplicación al Backlog de Fase 1

**Dependencias Críticas Identificadas:**

- `US-001-01` bloquea: `US-001-02`, `US-001-03`, `US-002-01`, todas las demás
- `US-001-02` bloquea: `US-002-01`, `US-003-01`, todas las demás
- `US-002-03` bloquea: `US-005-01`, `US-005-02` (necesita bus de eventos)
- `US-004-02` bloquea: `US-005-02` (motor necesita evento de cambio de etapa)
- `US-006-01` bloquea: `US-006-02`, `US-006-03` (scoring necesita insights del CV)

**Quick Wins Detectados:**

- `US-003-03` (Perfil candidato): Valor=4, Esfuerzo=3 → Ratio=1.33
- `US-004-03` (Historial etapas): Valor=3, Esfuerzo=5 → Ratio=0.60 (no es quick win)
- `US-005-03` (Logs automatizaciones): Valor=4, Esfuerzo=5 → Ratio=0.80 (balanceado)

**Higiénicos del MVP:**

- Autenticación y permisos: `US-001-01`, `US-001-02` (Urgencia=5)
- Infraestructura de eventos: `US-002-03` (Urgencia=5)
- Motor de automatización: `US-005-02` (Urgencia=5, sin motor el builder es inútil)
- IA operativa: `US-006-01`, `US-006-02` (Urgencia=5, core differentiator)

**Resultado de Aplicación del Algoritmo:**

El backlog ordenado se muestra en la tabla siguiente, reflejando la lógica de dependencias → quick wins → higiénicos → valor vs. complejidad.

---

## Backlog Priorizado — Fase 1 (MVP)

| **Orden** | **ID**       | **Título**                                      | **Épica**                | **SP** | **Valor** | **Urgencia** | **Justificación Breve**                                                      |
|-----------|--------------|-------------------------------------------------|--------------------------|--------|-----------|--------------|-------------------------------------------------------------------------------|
| 1         | US-001-01    | Registro de Empresa                             | EPIC-001 (Usuarios)      | 8      | 5         | 5            | Base de multi-tenancy. Bloquea toda la aplicación. **Ajustado 5→8 SP tras análisis técnico (Auth0, migraciones, emails).** |
| 2         | US-001-02    | Login Multi-Tenant                              | EPIC-001 (Usuarios)      | 8      | 5         | 5            | Autenticación segura. Bloquea acceso a cualquier feature. **Ajustado 5→8 SP (hereda configuración Auth0 y middleware complejo).** |
| 3         | US-001-03    | Invitación de Usuarios                          | EPIC-001 (Usuarios)      | 5      | 4         | 4            | Habilita colaboración recruiter + manager. **Ajustado 3→5 SP (requiere tokens temporales + SendGrid).** |
| 4         | US-002-01    | Crear Oferta con IA                             | EPIC-002 (Ofertas)       | 13     | 5         | 5            | Core ATS + primera demo de IA. Crítico para propuesta de valor. **Ajustado 8→13 SP (migración multi-tenant + IA + permisos).** |
| 5         | US-002-02    | Revisión y Aprobación de Oferta                 | EPIC-002 (Ofertas)       | 5      | 4         | 4            | Workflow de colaboración manager. Necesario antes de publicar.               |
| 6         | US-002-03    | Publicar Oferta y Activar Automatizaciones      | EPIC-002 (Ofertas)       | 5      | 5         | 5            | Infraestructura de eventos. Bloquea motor de automatización (US-005-02).     |
| 7         | US-003-01    | Registro Manual de Candidato                    | EPIC-003 (Candidatos)    | 8      | 5         | 5            | Core ATS. Sin candidatos no hay pipeline. Incluye upload a S3. **Ajustado 5→8 SP (migración + S3 + permisos multi-tenant).** |
| 8         | US-003-02    | Vincular Candidato a Oferta                     | EPIC-003 (Candidatos)    | 3      | 5         | 5            | Crea Application. Bloquea pipeline visual. Quick win (Ratio 1.67).           |
| 9         | US-003-03    | Vista de Perfil de Candidato                    | EPIC-003 (Candidatos)    | 3      | 4         | 4            | UX crítica para recruiters. Quick win (Ratio 1.33).                          |
| 10        | US-004-01    | Vista Kanban del Pipeline                       | EPIC-004 (Pipeline)      | 8      | 5         | 5            | Diferenciador UX principal. Primera impresión de LTI vs. ATS tradicionales. |
| 11        | US-004-02    | Mover Candidatos (Drag & Drop)                  | EPIC-004 (Pipeline)      | 8      | 5         | 5            | Emite evento de cambio de etapa. Bloquea automatizaciones (US-005-02).      |
| 12        | US-004-03    | Historial de Cambios de Etapa                   | EPIC-004 (Pipeline)      | 5      | 3         | 3            | Transparencia y auditoría. Menos urgente pero necesario en MVP.              |
| 13        | US-006-01    | Análisis de CV con IA                           | EPIC-006 (IA Operativa)  | 8      | 5         | 5            | Extrae insights del CV. Bloquea scoring (US-006-02). Core differentiator.    |
| 14        | US-006-02    | Scoring de Candidatos con IA                    | EPIC-006 (IA Operativa)  | 8      | 5         | 5            | Priorización automática de candidatos. Ahorro masivo de tiempo.              |
| 15        | US-005-01    | Builder Visual de Automatizaciones              | EPIC-005 (Automatización)| 13     | 5         | 5            | Core differentiator. Builder no-code es propuesta de valor principal.        |
| 16        | US-005-02    | Motor de Ejecución de Automatizaciones          | EPIC-005 (Automatización)| 13     | 5         | 5            | Sin motor, el builder no funciona. Crítico.                                  |
| 17        | US-006-03    | Sugerencias de Siguiente Paso con IA            | EPIC-006 (IA Operativa)  | 8      | 4         | 4            | IA proactiva. Demuestra orquestación inteligente.                            |
| 18        | US-005-03    | Logs y Monitoreo de Automatizaciones            | EPIC-005 (Automatización)| 5      | 4         | 4            | Debugging y confianza en automatizaciones. Necesario para soporte.           |

**Totales de Estimación (Fase 1 MVP):**
- **Story Points totales**: 129 SP *(actualizado de 113 SP tras re-calibración técnica)*
- **Velocidad estimada del equipo** (2 devs full-time): ~20-25 SP por sprint (2 semanas)
- **Duración estimada**: 6-7 sprints (~12-14 semanas = 3-3.5 meses) *(ajustado de 2.5-3 meses)*

**Notas de Planificación:**
- Sprints 1-2: EPIC-001 + EPIC-002 (fundamentos de usuarios y ofertas)
- Sprints 3-4: EPIC-003 + EPIC-004 (candidatos y pipeline visual)
- Sprints 5-6: EPIC-006 (IA operativa) + inicio de EPIC-005 (builder de automatizaciones)
- Sprint 7 (opcional): Completar EPIC-005 (motor + logs de automatizaciones)

---

---

## Sprint 1: Fundación y MVP

### Objetivo del Sprint

**"Establecer la arquitectura base multi-tenant, sistema de autenticación seguro y gestión inicial de empresas"**

Este primer sprint tiene un carácter fundacional crítico: implementaremos la infraestructura de autenticación con Auth0, el modelo multi-tenant sobre PostgreSQL + Prisma, y la capacidad de registro de empresas con notificaciones por email. Estos cimientos habilitan el resto del MVP y establecen patrones arquitectónicos (hexagonal, repository pattern, testing) que se replicarán en sprints futuros.

**Duración:** 2 semanas (10 días hábiles)  
**Equipo:** 2 developers full-time (1 Backend specialist, 1 Full-stack)  
**User Story abordada:** US-001-01 — Registro de Empresa

---

### Backlog del Sprint (Tickets de Trabajo)

El Sprint 1 aborda únicamente la **US-001-01 (Registro de Empresa)**, que fue descompuesta en 6 tickets técnicos ejecutables con trazabilidad completa:

| **ID Ticket** | **Título** | **Tipo** | **SP** | **Assignee Sugerido** | **Prioridad** |
|---------------|------------|----------|--------|-----------------------|---------------|
| [TASK-001-01-01](Agile/Tasks/TASK-001-01-01-MigracionCompany.md) | Migración de BD para Tabla Company | DB | 2 | Backend Developer | P0 (Bloqueante) |
| [TASK-001-01-02](Agile/Tasks/TASK-001-01-02-IntegracionAuth0.md) | Integración con Auth0/Clerk | Infra | 3 | Backend Developer | P0 (Bloqueante) |
| [TASK-001-01-03](Agile/Tasks/TASK-001-01-03-EndpointPostCompanies.md) | Endpoint POST /api/companies | Backend | 3 | Backend Developer | P0 (Bloqueante) |
| [TASK-001-01-04](Agile/Tasks/TASK-001-01-04-FormularioRegistro.md) | Formulario de Registro (Frontend) | Frontend | 3 | Full-stack Developer | P1 (Alta) |
| [TASK-001-01-05](Agile/Tasks/TASK-001-01-05-ConfiguracionEmail.md) | Configuración de SendGrid | Infra | 2 | Backend Developer | P1 (Alta) |
| [TASK-001-01-06](Agile/Tasks/TASK-001-01-06-TestsIntegracion.md) | Tests de Integración E2E | Testing | 2 | QA + Developers | P1 (Alta) |

**Carga Total del Sprint:** 15 Story Points  
**Velocidad Comprometida:** 15 SP (primera iteración, ajustaremos en Sprint 2 según velocidad real observada)

---

### Distribución Temporal Sugerida

**Semana 1 (Backend & Infraestructura):**
- **Días 1-2:** TASK-001-01-01 (Migración Company) + TASK-001-01-02 (Auth0 setup) — *5 SP*
- **Días 3-5:** TASK-001-01-03 (Endpoint Backend) + TASK-001-01-05 (SendGrid) — *5 SP*

**Semana 2 (Frontend & QA):**
- **Días 6-8:** TASK-001-01-04 (Formulario React) — *3 SP*
- **Días 9-10:** TASK-001-01-06 (Tests E2E) + Refinamiento — *2 SP*

**Daily Standups:** 15 minutos diarios (9:00 AM) para sincronización.  
**Mid-Sprint Review:** Día 5 — Demo de Backend funcional a stakeholders.  
**Sprint Review:** Día 10 — Demo de flujo completo (Frontend + Backend + Email).  
**Retrospective:** Día 10 (tarde) — Identificar mejoras para Sprint 2.

---

### Definición de Hecho del Sprint

El Sprint 1 se considera **COMPLETO** cuando:

✅ La migración de Prisma crea tabla `Company` con constraints e índices correctos  
✅ Auth0 está configurado y emite JWTs con custom claim `company_id`  
✅ Endpoint `POST /api/companies` responde 201 para registros válidos y 409 para dominios duplicados  
✅ Formulario React permite registrar empresa y muestra errores de validación  
✅ SendGrid envía email de bienvenida tras registro exitoso  
✅ Tests de integración Backend (Supertest) cubren >80% de `CreateCompanyUseCase`  
✅ Tests E2E Frontend (Playwright) validan flujo completo (happy path + errores)  
✅ Código mergeado a `main` tras code review y paso de CI/CD  
✅ Documentación técnica actualizada en README.md (setup de Auth0 y SendGrid)

---

### Riesgos Identificados y Mitigaciones

| **Riesgo** | **Probabilidad** | **Impacto** | **Mitigación** |
|------------|------------------|-------------|----------------|
| Retrasos en verificación de dominio de SendGrid | Media | Alto | Iniciar verificación DNS el Día 1 (puede tardar 24-48h) |
| Complejidad de custom claims en Auth0 mayor de lo esperado | Media | Medio | Dedicar pair programming en TASK-001-01-02 |
| Problemas de CORS entre Frontend y Backend | Baja | Medio | Configurar CORS desde Día 1 con wildcard en desarrollo |
| Subestimación de tests E2E con Playwright | Media | Bajo | Reservar 1 SP de buffer en Día 10 para ajustes |

---

### Métricas a Capturar

Durante el Sprint 1, registraremos las siguientes métricas para calibrar la velocidad del equipo:

- **Velocidad real observada:** Story Points completados vs. comprometidos (15 SP)
- **Cycle Time por ticket:** Tiempo desde "In Progress" hasta "Done"
- **Tasa de defectos:** Bugs encontrados en QA vs. bugs en producción
- **Coverage de tests:** Backend (objetivo >80%), Frontend (objetivo >70%)
- **Tiempo de setup de infraestructura:** Auth0 + SendGrid (para estimar futuras integraciones)

Estas métricas informarán la planificación del **Sprint 2** (US-001-02 Login + US-001-03 Invitación).

---

## Conclusiones del Ejercicio de Planificación

### 1. Sobre la Descomposición: El Valor de Aterrizar las User Stories en Tickets Técnicos

La descomposición de la **US-001-01 (Registro de Empresa)** reveló una lección crítica sobre estimación: **la complejidad real solo emerge al bajar a tareas ejecutables**.

**Hallazgo clave:**  
La estimación inicial de **5 Story Points** asumía un flujo simple de "formulario → endpoint → BD". Sin embargo, al adoptar las perspectivas multidisciplinares (Arquitecto, Tech Lead, Product Owner), descubrimos:

- **Auth0 no es "plug and play"**: Configurar custom claims, middleware JWT, y contexto multi-tenant añadió **3 SP** de complejidad oculta.
- **SendGrid requiere infraestructura**: Sender verification con DNS, plantillas HTML, manejo de errores y tracking añadieron **2 SP** adicionales.
- **Multi-tenancy no es trivial**: Migraciones con FKs, índices compuestos, y validación de ownership añadieron **2 SP** de overhead.
- **Arquitectura hexagonal tiene costo inicial**: Repository pattern, use cases, y dependency injection añadieron **1 SP** de estructura.
- **Tests completos son obligatorios**: E2E con Playwright + mocking de servicios externos añadieron **2 SP** de QA.

**Resultado:** La historia pasó de **5 SP a 15 SP reales** (3x la estimación inicial).

**Aprendizaje:**  
Las historias fundacionales que establecen patrones arquitectónicos y setup de infraestructura **siempre serán subestimadas** si se estiman a nivel abstracto. La descomposición en tickets técnicos con especificaciones detalladas (nombres de endpoints, schemas de BD, configuraciones de servicios externos) fuerza al equipo a enfrentar la complejidad antes del desarrollo, no durante él.

**Recomendación para futuros proyectos:**  
En proyectos greenfield, aplicar **Spike Stories** (timeboxed research) a las primeras 1-2 historias fundacionales para validar la arquitectura antes de comprometer estimaciones al resto del backlog.

---

### 2. Sobre la Priorización: Objetividad vs. Intuición en la Construcción del Backlog

La priorización del backlog de LTI combinó **métodos cuantitativos** (Fibonacci + Valor de Negocio + Urgencia) con **análisis cualitativo** (Dependencias técnicas + Coherencia de roadmap). Este enfoque híbrido demostró ser superior a enfoques unidimensionales.

**Comparación de enfoques:**

| **Método** | **Ventajas** | **Limitaciones en LTI** |
|------------|--------------|-------------------------|
| **Solo ROI (Valor/Esfuerzo)** | Simple, maximiza valor por unidad de tiempo | Ignora dependencias técnicas (US-001-01 bloquea todo aunque tenga ROI bajo) |
| **Solo MoSCoW (Must/Should/Could/Won't)** | Fácil de comunicar a stakeholders | Subjetivo, no cuantifica esfuerzo, dificulta planificación de sprints |
| **Solo WSJF (Weighted Shortest Job First)** | Optimiza time-to-market | No refleja dependencias arquitectónicas del MVP |
| **Híbrido (LTI)** | Balancea valor, esfuerzo, dependencias y coherencia | Requiere más tiempo de análisis inicial |

**Hallazgo clave:**  
El uso de **Fibonacci para esfuerzo** forzó al equipo a consensuar la complejidad relativa entre historias. Historias estimadas en **13 SP** (US-002-01 Crear Oferta con IA, US-005-01 Builder de Automatizaciones) señalaban inmediatamente que requerían descomposición adicional o asignación de múltiples developers.

El sistema de **Valor (1-5) + Urgencia (1-5)** permitió distinguir entre:
- **Alto Valor + Alta Urgencia** (US-001-01, US-006-01): Implementar inmediatamente.
- **Alto Valor + Baja Urgencia** (US-004-03 Historial de Etapas): Posponer a sprints medios del MVP.
- **Bajo Valor + Alta Urgencia** (casos edge de seguridad): Resolver como bugs críticos.

**Aprendizaje:**  
La intuición sola subestima historias "que parecen fáciles" (US-001-03 pasó de 3 a 5 SP). Los criterios objetivos obligan a **justificar** cada decisión de priorización, lo que genera consenso en equipos multidisciplinares y reduce conflictos entre Product Owner (maximizar valor) y Tech Lead (minimizar deuda técnica).

**Recomendación para futuros proyectos:**  
Adoptar el algoritmo de decisión en cascada (**Dependencias → Quick Wins → Higiénicos → Valor/Complejidad**) como estándar en refinement sessions. Documentar la justificación de cada priorización en la columna "Justificación Breve" del backlog para auditorías futuras.

---

### 3. Sobre la Re-estimación: Grooming Continuo como Práctica Obligatoria

El hallazgo técnico en **US-001-01** desencadenó un **ripple effect** que afectó a **5 de 18 User Stories** (27.8% del backlog). Este fenómeno demuestra que **el backlog es un artefacto vivo** que debe re-calibrarse continuamente.

**Impacto de la re-estimación:**
- **Total del backlog:** 113 SP → 129 SP (+14%)
- **Duración del MVP:** 2.5-3 meses → 3-3.5 meses (+2 semanas)
- **Historias ajustadas:**
  - US-001-02 (Login): 5 → 8 SP (hereda Auth0)
  - US-001-03 (Invitación): 3 → 5 SP (hereda SendGrid)
  - US-002-01 (Crear Oferta): 8 → 13 SP (migraciones complejas + IA)
  - US-003-01 (Registro Candidato): 5 → 8 SP (hereda S3 + multi-tenancy)

**Hallazgo clave:**  
La complejidad infraestructural descubierta en la **primera historia fundacional** tiene efecto dominó en **todas las historias que heredan esa infraestructura**. No re-estimar el backlog tras este hallazgo habría resultado en:
- **Sprint Planning incorrecto:** Comprometer más historias de las que el equipo puede completar.
- **Burndown engañoso:** Proyecciones de velocidad irreales que ocultan problemas hasta el final del proyecto.
- **Expectativas desalineadas:** Stakeholders esperando MVP en 2.5 meses cuando la realidad es 3.5 meses.

**Aprendizaje:**  
El **Grooming (Refinement)** no es una ceremonia opcional pre-sprint, es un **proceso continuo** que debe activarse cada vez que:
1. Una historia descompuesta revela complejidad 2x+ superior a la estimación inicial.
2. Se introduce una nueva tecnología o servicio externo no contemplado originalmente.
3. Un spike técnico invalida suposiciones arquitectónicas del backlog.

**Recomendación para futuros proyectos:**  
Institucionalizar **"Re-estimation Triggers"** en la Definition of Done de historias fundacionales:
- Si la descomposición en tickets revela complejidad >1.5x la estimación original, **pausar desarrollo** y ejecutar sesión de grooming para revisar historias dependientes.
- Documentar hallazgos técnicos en una sección "Actualización de Estimación" en cada User Story afectada (como hicimos en LTI).
- Comunicar cambios de timeline a stakeholders **inmediatamente** tras re-estimación, no al final del sprint.

---

### Reflexión Final: De la Planificación a la Ejecución

Este ejercicio de planificación de LTI ha validado principios clave de Agile:

1. **"Responding to change over following a plan"**: El backlog cambió un 14% tras el primer análisis técnico, y esto es **saludable**. Ignorar el hallazgo habría sido rígido y peligroso.

2. **"Working software over comprehensive documentation"**: Los tickets técnicos incluyen código de ejemplo (Auth0 middleware, Prisma schemas, tests) porque **la especificación es el código**. La documentación narrativa es secundaria.

3. **"Individuals and interactions over processes and tools"**: La perspectiva multidisciplinar (Arquitecto + Tech Lead + PO) en la descomposición de US-001-01 generó consenso sobre complejidad. Un proceso mecánico de estimación habría fallado.

**Próximo hito crítico:**  
La **Retrospective del Sprint 1** validará si nuestras estimaciones ajustadas son correctas. Si la velocidad real observada es <15 SP, deberemos re-calibrar nuevamente el backlog. Si es ≥15 SP, habremos establecido una línea base confiable para los próximos 6-7 sprints del MVP.

**El verdadero test de esta planificación no es su perfección inicial, sino su capacidad de adaptarse a la realidad que emerge sprint a sprint.**

---

## Próximos Pasos

1. ✅ **Desglose de User Stories por Épica**: Completado para las 6 épicas de la Fase 1 (MVP).
2. ✅ **Estimación de Story Points, Valor y Urgencia**: Completado para todas las 18 user stories.
3. ✅ **Priorización del Backlog**: Aplicado algoritmo de decisión basado en dependencias → quick wins → higiénicos.
4. ✅ **Re-calibración de Estimaciones**: Ajustado backlog tras hallazgos técnicos de US-001-01.
5. ✅ **Planificación del Sprint 1**: Definido objetivo, backlog de tickets, distribución temporal y DoD.
6. **Ejecución del Sprint 1**: Iniciar desarrollo el próximo lunes con Kickoff Meeting.
7. **Daily Standups**: Sincronización diaria del equipo (9:00 AM, 15 minutos).
8. **Mid-Sprint Review** (Día 5): Demo de Backend funcional a stakeholders.
9. **Sprint Review & Retrospective** (Día 10): Evaluar velocidad real y ajustar Sprint 2.
10. **Planificación del Sprint 2**: Descomponer US-001-02 (Login) y US-001-03 (Invitación).

---

*Última actualización: 2025-11-24*
