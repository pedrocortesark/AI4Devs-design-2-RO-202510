# US-006-01 — Análisis de CV con IA

## Narrativa

**Como** recruiter  
**Quiero** que el sistema analice automáticamente los CVs de candidatos usando IA  
**Para** obtener un resumen estructurado y ahorrar tiempo de revisión manual

## Descripción

Esta historia implementa la integración con IA (OpenAI GPT-4 o Claude) para extraer información estructurada de CVs (experiencia, habilidades, educación) y generar resúmenes ejecutivos. Los insights se guardan en el perfil del candidato y se usan para scoring posterior.

## Criterios de Aceptación

```gherkin
Escenario 1: Análisis automático al subir CV
  Dado que un candidato sube un CV en formato PDF
  Cuando el archivo se guarda en S3
  Entonces el sistema:
    1. Descarga el PDF y extrae el texto
    2. Envía el texto a la API de IA con prompt específico
    3. Recibe respuesta estructurada (JSON):
       {
         "summary": "Ingeniero con 5 años de experiencia...",
         "skills": ["Python", "React", "AWS"],
         "experience_years": 5,
         "education": "Ingeniería en Computación, Universidad X",
         "highlights": ["Lideró equipo de 3 personas", "Proyecto destacado Y"]
       }
    4. Guarda los insights en Candidate.ai_insights (JSON)
    5. Muestra el resumen en el perfil del candidato

Escenario 2: Regenerar análisis manualmente
  Dado que un recruiter ve un análisis desactualizado
  Cuando hace clic en "Regenerar análisis con IA"
  Entonces el sistema:
    1. Reenvía el CV a la API de IA
    2. Actualiza Candidate.ai_insights
    3. Muestra el nuevo resumen
    4. Registra el evento en logs (auditoría)

Escenario 3: Manejo de error de API de IA
  Dado que la API de IA devuelve error 429 (rate limit)
  Cuando se intenta analizar un CV
  Entonces el sistema:
    1. Captura el error
    2. Reintenta después de 10 segundos (hasta 3 reintentos)
    3. Si falla definitivamente, guarda status=FAILED en metadata
    4. Muestra mensaje al usuario: "Análisis temporalmente no disponible"
    5. Permite reintento manual posterior

Escenario 4: Validación de costos de IA
  Dado que el análisis de un CV cuesta ~0.05 USD
  Cuando se ejecutan 100 análisis en un día
  Entonces el sistema:
    1. Registra cada llamada en AIUsageLog con tokens consumidos
    2. Muestra métricas de uso en panel de admin
    3. Alertar si se supera presupuesto diario configurado
```

## Notas Técnicas

- **IA Provider**: OpenAI GPT-4 Turbo o Claude 3.5 Sonnet (configurar via env vars).
- **Parsing PDF**: Usar librería `pdf-parse` o `pdfjs-dist` para extraer texto.
- **Prompt Engineering**: Template de prompt optimizado para CV parsing (system message + user message con texto del CV).
- **Rate Limiting**: Implementar retry con backoff exponencial para errores 429.
- **Cost Control**: Modelo de datos `AIUsageLog` para tracking de costos por company_id.
- **Caching**: No reanalizar mismo CV si ya tiene insights recientes (<7 días).

## Tareas

- [ ] Configurar integración con OpenAI API (SDK, API key)
- [ ] Implementar módulo de CV parsing (PDF → texto plano)
- [ ] Crear prompt template para análisis de CV estructurado
- [ ] Implementar endpoint `POST /api/candidates/:id/analyze-cv` en Backend
- [ ] Enviar texto del CV a IA y parsear respuesta JSON
- [ ] Guardar insights en Candidate.ai_insights (columna JSONB)
- [ ] Crear modelo AIUsageLog para tracking de costos
- [ ] Implementar retry logic para errores de API de IA
- [ ] Implementar validación de presupuesto diario de IA
- [ ] Crear vista de resumen de CV en Frontend (componente CVSummary)
- [ ] Añadir botón "Regenerar análisis" en perfil de candidato
- [ ] Crear panel de métricas de uso de IA en Admin
- [ ] Crear tests unitarios de parsing y llamada a IA
- [ ] Crear tests de integración: subir CV → análisis → guardar insights
- [ ] Documentar endpoints en OpenAPI/Swagger

## Estimación

**Story Points:** 8  
**Valor de Negocio:** 5 (Crítico — diferenciador de IA operacional)  
**Urgencia:** 5 (MVP core feature)

## Dependencias

- **US-003-01-RegistroCandidato**: CV debe estar subido en S3
- Cuenta y API key de OpenAI/Anthropic configurada

## Prioridad

**Crítica** — Core value proposition de LTI (IA operacional)
