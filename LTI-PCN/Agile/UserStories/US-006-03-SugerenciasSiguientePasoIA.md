# US-006-03 — Sugerencias de Siguiente Paso con IA

## Narrativa

**Como** recruiter  
**Quiero** que el sistema me sugiera el siguiente paso recomendado para cada candidato  
**Para** optimizar el proceso de selección y no olvidar seguimientos importantes

## Descripción

Esta historia implementa sugerencias proactivas generadas por IA basadas en el contexto del candidato (etapa actual, tiempo en etapa, score, historial). Las sugerencias aparecen como notificaciones o badges en el Kanban y pueden ser descartadas o aceptadas por el recruiter.

## Criterios de Aceptación

```gherkin
Escenario 1: Sugerencia para candidato estancado
  Dado que un candidato lleva 7 días en etapa "Screening" sin acción
  Y su ai_score es 85 (alto)
  Cuando el sistema ejecuta el job diario de sugerencias
  Entonces genera sugerencia:
    {
      "type": "NEXT_ACTION",
      "candidate_id": 123,
      "priority": "HIGH",
      "message": "Candidato con alto score (85) lleva 7 días en Screening. Sugerencia: mover a Entrevista Técnica.",
      "suggested_action": "MOVE_TO_STAGE",
      "target_stage": "Entrevista Técnica",
      "reasoning": "Alto fit + tiempo prolongado sin avance."
    }
  Y se muestra badge naranja en la card del candidato en Kanban
  Y aparece en panel de "Sugerencias" del recruiter

Escenario 2: Sugerencia de descarte por bajo score
  Dado que un candidato tiene ai_score de 25 y lleva 3 días sin revisión
  Cuando el sistema genera sugerencias
  Entonces sugiere:
    - Tipo: CONSIDER_REJECT
    - Mensaje: "Candidato con bajo score (25). Revisar si cumple requisitos mínimos o descartar."
    - Reasoning: "Score bajo indica pobre fit con requisitos clave."
  Y se muestra badge rojo en la card

Escenario 3: Aceptar sugerencia (aplicar acción)
  Dado que el recruiter ve una sugerencia "Mover a Entrevista Técnica"
  Cuando hace clic en "Aplicar sugerencia"
  Entonces el sistema:
    1. Mueve el candidato a la etapa sugerida (ejecuta lógica de US-004-02)
    2. Registra que la sugerencia fue aceptada (AISuggestion.status=ACCEPTED)
    3. Oculta la badge de sugerencia
    4. Envía evento para aprendizaje futuro (IA aprende que sugerencias son útiles)

Escenario 4: Descartar sugerencia
  Dado que el recruiter ve una sugerencia que no aplica
  Cuando hace clic en "Descartar"
  Entonces el sistema:
    1. Marca AISuggestion.status=DISMISSED
    2. Oculta la badge
    3. No vuelve a sugerir lo mismo para ese candidato (por 14 días)

Escenario 5: Panel de sugerencias global
  Dado que el recruiter accede a "Mis Sugerencias"
  Cuando carga la vista
  Entonces se muestra lista priorizada:
    | Prioridad | Candidato    | Oferta        | Sugerencia                        | Acción   |
    | ALTA      | Juan Pérez   | Backend Dev   | Mover a Entrevista Técnica        | Aplicar  |
    | MEDIA     | Ana López    | Frontend Dev  | Solicitar referencias             | Aplicar  |
    | BAJA      | Carlos Ruiz  | QA Engineer   | Considerar descarte (bajo score)  | Descartar|
  Y puede filtrar por prioridad, oferta o tipo de sugerencia
```

## Notas Técnicas

- **Modelo de datos**: `AISuggestion` con `candidate_id`, `type`, `priority`, `message`, `suggested_action`, `status` (PENDING/ACCEPTED/DISMISSED).
- **Job diario**: Ejecutar análisis de candidatos activos y generar sugerencias con IA.
- **Prompt Engineering**: Contexto incluye: candidate profile, ai_score, current stage, days_in_stage, stage history.
- **Learning loop**: Registrar si sugerencias fueron aceptadas/descartadas para mejorar modelo (futuro).
- **Notificaciones**: Opcionalmente enviar notificación push/email con sugerencias urgentes.

## Tareas

- [ ] Diseñar modelo de datos AISuggestion
- [ ] Crear migración para tabla ai_suggestions
- [ ] Implementar job diario de generación de sugerencias (cron/scheduler)
- [ ] Crear prompt template para análisis de contexto + sugerencia
- [ ] Enviar contexto de candidato a IA y parsear sugerencias
- [ ] Guardar sugerencias en BD con prioridad y reasoning
- [ ] Crear endpoint `GET /api/suggestions` para listar sugerencias del recruiter
- [ ] Implementar endpoint `POST /api/suggestions/:id/accept` (aplicar acción)
- [ ] Implementar endpoint `POST /api/suggestions/:id/dismiss` (descartar)
- [ ] Crear componente SuggestionBadge en Kanban (overlay en card)
- [ ] Crear panel de "Mis Sugerencias" en Frontend
- [ ] Implementar lógica de aplicar acción sugerida (mover etapa, etc.)
- [ ] Implementar filtros y ordenamiento en panel de sugerencias
- [ ] Crear tests unitarios de lógica de sugerencias
- [ ] Crear tests de integración: generar sugerencia → aceptar → aplicar acción
- [ ] Documentar endpoints en OpenAPI/Swagger

## Estimación

**Story Points:** 8  
**Valor de Negocio:** 4 (Alta — productividad y UX proactiva)  
**Urgencia:** 4 (Importante para demostrar IA proactiva en MVP)

## Dependencias

- **US-006-02-ScoringCandidatosIA**: Necesita ai_score para priorizar sugerencias
- **US-004-02-DragDropCandidatos**: Necesita lógica de mover candidato entre etapas

## Prioridad

**Alta** — Demuestra que la IA es proactiva, no solo reactiva
