# Log de Prompts — LTI-PCN

> Registro cronológico de todas las interacciones con GitHub Copilot durante la ejecución del proyecto LTI.  
> **Regla importante**: Los resúmenes de respuestas deben ser muy breves (máximo un párrafo), resumiendo solo la acción tomada sin copiar contenido generado ni archivos creados.

---

## Prompt 1: Configuración, Roadmap y Estructura de Épicas

**Fecha:** 2025-11-24  
**Rol asignado:** Senior Product Manager y Business Analyst  
**Fuente de verdad:** `LTI-PCN.md`

**Instrucciones recibidas:**
1. Inicializar documento maestro `UserStories-PCN.md` con título H1 e introducción.
2. Desarrollar roadmap de 6 meses priorizando valor (MVP y evolutivos) con justificación del orden.
3. Desglozar roadmap en épicas con ubicación en `Agile/Epics/`, nomenclatura `EPIC-[NUM]-[Nombre].md`, contenido con título, descripción, valor de negocio y listado preliminar de funcionalidades.
4. Enlazar épicas en documento maestro.
5. Registrar interacción en `prompts-log.md` con resumen breve.
6. Incluir consideración sobre resúmenes breves en `github-instructions.md`.

**Resumen de la acción tomada:**  
Se creó el documento maestro `UserStories-PCN.md` con roadmap de 6 meses dividido en 3 fases (MVP, Colaboración, Insights) con 16 épicas priorizadas según valor de negocio y viabilidad técnica. Se generaron 16 archivos de épicas en `Agile/Epics/` con formato `EPIC-[NUM]-[Nombre].md`, cada uno documentando descripción, valor de negocio, funcionalidades preliminares, dependencias, estimación y criterios de éxito. Las épicas fueron enlazadas correctamente en el documento maestro. Se creó este log de prompts y se actualizó `github-instructions.md` con las consideraciones sobre resúmenes breves.

---

## Prompt 2: Desglose de User Stories para Épicas Prioritarias

**Fecha:** 2025-11-24  
**Rol asignado:** Senior Product Manager y Business Analyst  
**Fuente de verdad:** `LTI-PCN.md`, Roadmap en `UserStories-PCN.md`

**Instrucciones recibidas:**
1. Identificar las 2 épicas prioritarias del Roadmap (inicio del proyecto/MVP).
2. Generar 2-3 User Stories detalladas por épica con formato estándar, criterios de aceptación en Gherkin y tareas técnicas.
3. Crear subcarpeta `Agile/UserStories/` con nomenclatura `US-[#EPIC]-[#US]-[NombreBreve].md`.
4. Actualizar `UserStories-PCN.md` con sección "Product Backlog (Detalle)" enlazando las User Stories.
5. Registrar en `prompts-log.md` con resumen breve.

**Resumen de la acción tomada:**  
Se identificaron las 2 épicas más prioritarias del MVP (EPIC-001-GestionUsuarios y EPIC-002-GestionOfertas) y se generaron 6 User Stories detalladas (3 por épica) con formato estándar, criterios de aceptación en Gherkin, notas técnicas y tareas de implementación. Los archivos fueron creados en `Agile/UserStories/` con nomenclatura correcta y se añadió la sección "Product Backlog (Detalle)" en el documento maestro con enlaces a las historias agrupadas por épica.

---

## Prompt 3: Cobertura Total de Fase 1, Estimación y Priorización del Backlog

**Fecha:** 2025-11-24  
**Rol asignado:** Senior Product Manager y Business Analyst  
**Fuente de verdad:** `LTI-PCN.md`, Roadmap en `UserStories-PCN.md`

**Instrucciones recibidas:**
1. Generar las User Stories faltantes para completar las 6 épicas de la Fase 1 del roadmap (EPIC-003 a EPIC-006).
2. Asignar atributos completos a todas las User Stories: Esfuerzo (Fibonacci 1,2,3,5,8,13), Valor de Negocio (1-5), Urgencia (1-5), Dependencias (referencias a US bloqueantes).
3. Ordenar el Backlog aplicando algoritmo: Dependencias Críticas → Quick Wins → Higiénicos del MVP → Balance Valor/Complejidad.
4. Crear nueva sección "Metodología de Priorización" en `UserStories-PCN.md` explicando: (a) Por qué Fibonacci para esfuerzo, (b) Cómo se valora negocio y urgencia, (c) Cómo las dependencias anulan otros factores, (d) Justificación de enfoque híbrido sobre métodos únicos, (e) Aplicación práctica del algoritmo al backlog.
5. Generar tabla final en `UserStories-PCN.md` con columnas: Orden | ID | Título | Épica | SP | Valor | Urgencia | Justificación Breve.
6. Registrar esta interacción en `prompts-log.md` con resumen breve.

**Resumen de la acción tomada:**  
Se generaron 12 User Stories adicionales para completar las épicas EPIC-003, EPIC-004, EPIC-005 y EPIC-006 (total 18 US en Fase 1), todas con formato estándar, criterios Gherkin, notas técnicas y estimación completa (SP, Valor, Urgencia, Dependencias). Se documentó la "Metodología de Priorización" explicando el uso de Fibonacci, escalas de valor/urgencia, identificación de dependencias técnicas y el algoritmo de decisión en cascada aplicado al backlog. Se creó tabla priorizada con las 18 User Stories ordenadas según el algoritmo, reflejando dependencias críticas, quick wins y coherencia de roadmap. El backlog completo de la Fase 1 está estimado en 113 SP (equivalente a 5-6 sprints).

---

## Prompt 4: Sprint Planning — Refinement Multidisciplinar y Tickets Técnicos

**Fecha:** 2025-11-24  
**Rol asignado:** Equipo de Producto Completo (Product Owner + Arquitecto de Software + Tech Lead)  
**Contexto:** Sprint Planning para la User Story más prioritaria del backlog  

**Instrucciones recibidas:**
1. Seleccionar la US #1 del backlog priorizado (US-001-01-RegistroEmpresa).
2. Analizar la User Story desde 3 perspectivas antes de descomponer:
   - **Arquitecto:** Cambios de BD, seguridad, integraciones (evitar deuda técnica).
   - **Tech Lead:** Endpoints, componentes, lógica específica (viabilidad técnica).
   - **Product Owner:** Cobertura completa de criterios de aceptación (valor).
3. Generar tickets técnicos ejecutables en `Agile/Tasks/` con nomenclatura `TASK-[ID_US]-[Secuencial]-[NombreBreve].md`.
4. Estructura de cada ticket: Metadatos (roles, tipo, categoría, estimación), Contexto PO, Especificación Técnica (Tech Lead), Validación (Arquitecto), DoD.
5. Editar US-001-01 añadiendo sección "Plan de Implementación" con enlaces a tickets y distribución en sprints.
6. Registrar este prompt en `prompts-log.md`.

**Resumen de la acción tomada:**  
Se descompuso la US-001-01 (Registro de Empresa) en 6 tickets técnicos ejecutables: (1) Migración de BD para tabla Company con Prisma (2 SP), (2) Integración con Auth0/Clerk para OAuth + custom claims (3 SP), (3) Endpoint POST /api/companies con arquitectura hexagonal y validación de dominio (3 SP), (4) Formulario React con validación Zod y manejo de errores (3 SP), (5) Configuración de SendGrid para email de bienvenida (2 SP), (6) Tests de integración Backend (Supertest) y E2E Frontend (Playwright) con coverage >80% (2 SP). Cada ticket incluye contexto de producto, especificación técnica detallada con código de ejemplo, validaciones de arquitecto (seguridad, performance, escalabilidad) y DoD. Se añadió sección "Plan de Implementación" en US-001-01 con trazabilidad completa, distribución sugerida en sprint (8 SP semana 1 Backend, 7 SP semana 2 Frontend+QA) y mapa de dependencias entre tickets. Total estimado: 15 SP (revelando 3x la complejidad inicial debido a setup de infraestructura fundacional).

---

## Prompt 5: Re-calibración de Estimaciones basada en Hallazgos Técnicos

**Fecha:** 2025-11-24  
**Rol asignado:** Equipo de Producto Completo (Product Owner + Arquitecto de Software + Tech Lead)  
**Contexto:** Re-evaluación de estimaciones tras descubrir complejidad 3x superior en US-001-01 (5 SP → 15 SP real)

**Instrucciones recibidas:**
1. Tomar la complejidad descubierta en US-001-01 como nueva "Línea Base de Complejidad".
2. Revisar todas las User Stories del Backlog de Fase 1 identificando cuáles dependen de infraestructura compleja (Auth0, SendGrid, migraciones multi-tenant, arquitectura hexagonal).
3. Re-estimar Story Points (Fibonacci) de las historias afectadas sin generar nuevos tickets.
4. Actualizar tabla de backlog priorizado en `UserStories-PCN.md` con nuevos SP y justificaciones breves en columna correspondiente.
5. Añadir nota de "Actualización de Estimación" al final de cada archivo individual de User Story modificada.
6. Generar tabla resumen "Antes vs Después" de historias que cambiaron puntuación.
7. Registrar este prompt en `prompts-log.md`.

**Resumen de la acción tomada:**  
Se realizó análisis de ripple effect identificando 5 User Stories afectadas por la complejidad infraestructural descubierta en US-001-01: (1) US-001-01 ajustada 5→8 SP (hallazgo confirmado por descomposición), (2) US-001-02 ajustada 5→8 SP (hereda configuración Auth0 con middleware JWT multi-tenant), (3) US-001-03 ajustada 3→5 SP (requiere tokens temporales + SendGrid), (4) US-002-01 ajustada 8→13 SP (migraciones multi-tenant complejas + integración IA + permisos), (5) US-003-01 ajustada 5→8 SP (migraciones multi-tenant + integración S3 + permisos). Se actualizó tabla de backlog priorizado con nuevos SP y justificaciones en línea. Se añadieron notas de "Actualización de Estimación" detalladas en cada archivo .md individual explicando factores técnicos descubiertos (Auth0 custom claims, SendGrid verification, migraciones con índices multi-tenant, arquitectura hexagonal, integraciones con S3/IA). Total del backlog ajustado de 113 SP a 129 SP (+14% de complejidad), extendiendo duración estimada de 2.5-3 meses a 3-3.5 meses. Se mantuvieron intactas las 13 User Stories restantes al no heredar complejidad infraestructural fundacional.

---

## Prompt 6: Documentación del Sprint 1 y Conclusiones Finales

**Fecha:** 2025-11-24  
**Rol asignado:** Scrum Master y Agile Delivery Lead  
**Contexto:** Consolidación final del ejercicio de planificación con documentación del Sprint 1 y reflexiones sobre aprendizajes

**Instrucciones recibidas:**
1. Crear sección "Sprint 1: Fundación y MVP" en `UserStories-PCN.md` con:
   - Objetivo del sprint claro (establecer arquitectura base, autenticación, gestión de usuarios)
   - Tabla de Backlog del Sprint listando los 6 tickets de US-001-01 (columnas: ID, Título, Tipo Backend/Frontend/Infra, SP)
   - Carga total del sprint y velocidad comprometida
2. Añadir sección "Conclusiones del Ejercicio de Planificación" con 3 reflexiones clave:
   - Sobre la descomposición (revelación de complejidad real al pasar de US a tickets)
   - Sobre la priorización (utilidad de criterios objetivos vs intuición)
   - Sobre la re-estimación (importancia de grooming continuo tras hallazgos técnicos)
3. Validar estructura narrativa lógica del documento: Roadmap → Épicas → Metodología → Backlog → Sprint 1 → Conclusiones
4. Verificar funcionamiento de enlaces a carpeta `Agile/`
5. Registrar este prompt en `prompts-log.md`

**Resumen de la acción tomada:**  
Se creó sección completa "Sprint 1: Fundación y MVP" documentando objetivo del sprint (arquitectura multi-tenant + Auth0 + registro de empresas), tabla de 6 tickets técnicos con distribución temporal sugerida (Semana 1: Backend 10 SP, Semana 2: Frontend+QA 5 SP), velocidad comprometida de 15 SP, definición de hecho del sprint con 9 criterios verificables, riesgos identificados con mitigaciones (ej: retrasos SendGrid DNS), y métricas a capturar (velocidad, cycle time, coverage). Se añadió sección "Conclusiones del Ejercicio de Planificación" con 3 reflexiones profundas: (1) Descomposición reveló complejidad 3x (5→15 SP) al especificar Auth0, SendGrid, migraciones multi-tenant y arquitectura hexagonal, validando necesidad de spike stories en proyectos greenfield; (2) Priorización híbrida (Fibonacci + Valor + Dependencias) superó enfoques unidimensionales (ROI, MoSCoW, WSJF) al balancear valor de negocio con restricciones técnicas; (3) Re-estimación tras hallazgos técnicos (+14% backlog, +2 semanas timeline) demostró que grooming continuo es obligatorio, no opcional, estableciendo "Re-estimation Triggers" como práctica estándar. Se validó estructura narrativa completa del documento con flujo lógico desde roadmap estratégico hasta plan de ejecución táctico. Todos los enlaces internos a Agile/Epics/, Agile/UserStories/ y Agile/Tasks/ funcionan correctamente. El documento UserStories-PCN.md queda como artefacto completo y ejecutable para iniciar desarrollo del MVP.

---

## Prompt 7: Síntesis del Backlog en Formato Estándar

**Fecha:** 2025-12-02  
**Rol asignado:** Product Owner experto  
**Contexto:** Generación de backlog estandarizado con formato simplificado para documentación rápida

**Instrucciones recibidas:**
1. Actuar como Product Owner experto analizando `LTI-PCN.md` y `UserStories-PCN.md` para comprender alcance total del proyecto.
2. Sintetizar el Backlog de Producto inicial (18 User Stories de Fase 1 MVP) en formato estándar.
3. Crear archivo `Agile/Backlogs-Options/Backlog_Option_A_Standard.md` con:
   - Título, descripción en formato Como/Quiero/Para, y 3 criterios de aceptación claros por historia
   - Validación de criterio INVEST (Independent, Negotiable, Valuable, Estimable, Small, Testable)
   - Introducción explicando contexto del proyecto y segmento objetivo
4. Registrar este prompt en `prompts-log.md` siguiendo reglas establecidas sin modificar entradas previas.

**Resumen de la acción tomada:**  
Se generó archivo `Backlog_Option_A_Standard.md` con síntesis completa de las 18 User Stories del MVP en formato estandarizado: cada historia incluye título, descripción en formato Como/Quiero/Para, 3 criterios de aceptación verificables, estimación en SP, valor de negocio, y dependencias. Se añadió introducción explicando contexto LTI (plataforma de orquestación de talento para startups/scale-ups 50-1000 empleados combinando ATS + automatización no-code + IA operativa) y validación de criterio INVEST para todas las historias. Se incluyeron secciones adicionales: distribución de 129 SP por épica, estimación de duración (6-7 sprints), notas de validación INVEST y próximos pasos (refinement, sprint planning, descomposición en tasks, preparación de entorno). El documento funciona como alternativa simplificada al backlog detallado en `UserStories-PCN.md`, optimizado para presentaciones ejecutivas y onboarding rápido de stakeholders.

---

## Prompt 8: Priorización Estratégica con Matriz MoSCoW

**Fecha:** 2025-12-02  
**Rol asignado:** Product Manager con enfoque en estrategia de MVP  
**Contexto:** Generación de variante de backlog priorizada estratégicamente para decisiones de alcance del MVP

**Instrucciones recibidas:**
1. Extraer funcionalidades descritas en `LTI-PCN.md` y `UserStories-PCN.md`.
2. Clasificar rigurosamente usando matriz MoSCoW (Must, Should, Could, Won't).
3. Generar output en formato Markdown para `Agile/Backlogs-Options/Backlog_Option_B_MoSCoW.md`.
4. Para cada ítem, añadir "Justificación Estratégica" explicando asignación de prioridad basándose en objetivos del PRD.

**Resumen de la acción tomada:**  
Se generó archivo `Backlog_Option_B_MoSCoW.md` con clasificación estratégica de las 18 User Stories del MVP en 4 categorías: (1) **Must Have** (10 historias, 81 SP): funcionalidades bloqueantes sin las cuales el producto no puede lanzarse (autenticación multi-tenant, ofertas con IA, candidatos, pipeline kanban, drag & drop, motor de automatización, análisis CV y scoring IA); (2) **Should Have** (6 historias, 48 SP): funcionalidades importantes que reducen fricciones pero el producto puede lanzarse sin ellas (invitaciones, workflow de ofertas completo, builder visual); (3) **Could Have** (3 historias, 13 SP): deseables para iteraciones futuras (perfil detallado, historial etapas, logs); (4) **Won't Have** (10 épicas, Fase 2-3): explícitamente fuera del MVP (feedback colaborativo, decisiones finales, candidate experience, notificaciones avanzadas, reporting, reutilización de talento, movilidad interna, optimización IA, integraciones extendidas). Cada categoría incluye justificación estratégica detallada basada en: objetivos del PRD (diferenciación, validación, feedback temprano), análisis de mercado ATS, viabilidad técnica, segmento objetivo (startups 50-300 empleados), y bloqueos técnicos vs diferenciación competitiva. Se añadieron secciones: roadmap recomendado por sprints (Must Have Core → Must Have + Should Have UI/UX → Could Have + Polish), principios de re-priorización dinámica basados en feedback/métricas, y criterios de éxito del MVP con validación técnica/producto/negocio. El documento proporciona visión estratégica clara para tomar decisiones de alcance, trade-offs y phasing del MVP vs fases posteriores.

---

## Prompt 9: Priorización Data-Driven con Método RICE

**Fecha:** 2025-12-02  
**Rol asignado:** Product Owner "Data-Driven"  
**Contexto:** Creación de variante de backlog priorizada numéricamente para decisiones objetivas basadas en impacto cuantificable

**Instrucciones recibidas:**
1. Listar historias de usuario identificadas en `LTI-PCN.md` y `UserStories-PCN.md`.
2. Aplicar estimación RICE (Reach, Impact, Confidence, Effort) a cada historia, infiriendo valores de importancia descrita en PRD.
3. Generar tabla Markdown ordenada de mayor a menor RICE Score.
4. Preparar output para `Agile/Backlogs-Options/Backlog_Option_C_RICE.md`.
5. Añadir conclusión sobre historias que deberían entrar en Sprint 1 según scoring.

**Resumen de la acción tomada:**  
Se generó archivo `Backlog_Option_C_RICE.md` con análisis cuantitativo completo de las 18 User Stories del MVP usando método RICE. Para cada historia se calculó: (1) **Reach** (% usuarios impactados: 100% universal, 75% recruiters, 50% admins, 25% nicho); (2) **Impact** (escala 3=Massive, 2=High, 1=Medium, 0.5=Low basada en diferenciación vs ATS, reducción trabajo manual, adopción); (3) **Confidence** (100%=High validado por research, 80%=Medium hipótesis fuerte, 50%=Low especulativa); (4) **Effort** (conversión SP a persona-mes: 3SP=0.5PM, 5SP=0.75PM, 8SP=1.5PM, 13SP=2.5PM). RICE Score = (Reach × Impact × Confidence) ÷ Effort. Resultados clave: US-003-02 (Vincular Candidato) lidera con score 450 (quick win crítico: alto impacto, bajo esfuerzo); empate técnico en infraestructura (US-001-01, US-001-02, US-002-03) con score 200; features de 13 SP tienen scores bajos (24-72) por alto esfuerzo pero son core differentiators estratégicos (Crear Oferta IA, Motor Automatización, Builder Visual); features admin-facing (Builder, Logs) penalizadas por reach bajo (25%) pero habilitan valor downstream. Se incluyeron: tabla resumen ordenada por RICE descendente, insights del análisis (quick win inesperado, empates técnicos, penalización de complejidad, baja confianza en IA avanzada), limitaciones del método RICE (no captura dependencias, confidence subjetiva, penaliza inversiones estratégicas), y recomendación de Sprint 1 (24 SP): US-001-01, US-001-02, US-003-02, US-001-03 balanceando scores altos, bloqueantes técnicos, y quick wins. El documento proporciona framework cuantitativo objetivo para priorización complementando análisis cualitativo del PRD y dependencias técnicas.

---

*Última actualización: 2025-12-02*
