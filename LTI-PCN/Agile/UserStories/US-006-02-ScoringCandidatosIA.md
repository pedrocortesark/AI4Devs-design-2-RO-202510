# US-006-02 — Scoring de Candidatos con IA

## Narrativa

**Como** recruiter  
**Quiero** que el sistema asigne automáticamente un score a cada candidato en base a su fit con la oferta  
**Para** priorizar revisión de candidatos más relevantes primero

## Descripción

Esta historia implementa el algoritmo de scoring que compara los requisitos de la oferta con el perfil del candidato (extraído vía IA en US-006-01) y asigna un score de 0-100. El score se muestra en el Kanban y se puede ordenar candidatos por esta métrica.

## Criterios de Aceptación

```gherkin
Escenario 1: Calcular score automáticamente al aplicar candidato
  Dado que una oferta requiere: "Python, 3+ años exp, título en CS"
  Y un candidato tiene: skills=["Python", "Django"], experience_years=5, education="CS"
  Cuando el candidato aplica a la oferta
  Entonces el sistema:
    1. Envía a la IA:
       - Job requirements (texto de la oferta)
       - Candidate profile (ai_insights extraídos del CV)
    2. Recibe score de 0-100 y justificación:
       {
         "score": 85,
         "reasoning": "Cumple requisitos clave (Python, 5 años exp, CS). Falta mención de framework específico.",
         "strengths": ["Experiencia superior a requerida", "Match exacto en educación"],
         "gaps": ["No menciona testing automatizado"]
       }
    3. Guarda en Application.ai_score y Application.ai_score_reasoning
    4. Muestra badge de score en el Kanban

Escenario 2: Visualización de score en perfil
  Dado que un candidato tiene ai_score=85
  Cuando el recruiter ve el perfil del candidato
  Entonces se muestra:
    - Score visual (85/100) con color (verde >70, amarillo 50-70, rojo <50)
    - Sección "Análisis de Fit":
      - Fortalezas: "Experiencia superior...", "Match exacto..."
      - Gaps: "No menciona testing..."
    - Fecha de último cálculo

Escenario 3: Recalcular score si oferta cambia
  Dado que una oferta tenía requisitos X y ya tiene candidatos con scores
  Cuando el hiring manager actualiza los requisitos a Y
  Entonces el sistema:
    1. Marca todos los scores de candidatos de esa oferta como outdated
    2. Muestra mensaje: "Requisitos actualizados. Recalcular scores?"
    3. Al confirmar, recalcula todos los scores en background job
    4. Notifica cuando termine: "Scores actualizados para 15 candidatos"

Escenario 4: Ordenar candidatos por score en Kanban
  Dado que una oferta tiene 20 candidatos en etapa "Screening"
  Cuando el recruiter selecciona "Ordenar por Score"
  Entonces los candidatos se ordenan descendente (mayor score primero)
  Y se muestra el score en cada card del Kanban
```

## Notas Técnicas

- **Prompt Engineering**: Template que compara job_requirements (texto) con candidate_profile (JSON de insights).
- **Score Storage**: Columnas `ai_score` (INT 0-100), `ai_score_reasoning` (TEXT), `ai_score_date` (TIMESTAMP) en Application.
- **Background Jobs**: Usar queue (Bull/BullMQ) para recalcular scores en lote sin bloquear UI.
- **Caching**: No recalcular si oferta no cambió y score tiene <7 días.
- **Cost Optimization**: Batch requests si es posible (calcular múltiples scores en una llamada).

## Tareas

- [ ] Crear prompt template para scoring de candidatos vs oferta
- [ ] Implementar endpoint `POST /api/applications/:id/calculate-score` en Backend
- [ ] Enviar job_requirements + candidate_profile a IA
- [ ] Parsear respuesta y guardar score + reasoning en Application
- [ ] Añadir columnas ai_score, ai_score_reasoning, ai_score_date a Application
- [ ] Implementar cálculo automático al crear Application
- [ ] Crear componente ScoreBadge en Frontend (visual con colores)
- [ ] Mostrar score y análisis en perfil de candidato
- [ ] Implementar ordenamiento por score en Kanban
- [ ] Crear job de background para recálculo en lote
- [ ] Implementar lógica de detección de cambios en oferta (trigger recalculation)
- [ ] Crear tests unitarios de lógica de scoring
- [ ] Crear tests de integración: crear Application → calcular score → guardar
- [ ] Documentar algoritmo de scoring en docs técnicas

## Estimación

**Story Points:** 8  
**Valor de Negocio:** 5 (Crítico — ahorro de tiempo masivo para recruiters)  
**Urgencia:** 5 (MVP diferenciador)

## Dependencias

- **US-006-01-AnalisisCVconIA**: Necesita ai_insights del candidato
- **US-002-01-CrearOfertaConIA**: Necesita descripción estructurada de oferta

## Prioridad

**Crítica** — Sin scoring, la IA es solo cosmética
