# US-004-03 — Historial de Etapas de Candidato

## Narrativa

**Como** recruiter o hiring manager  
**Quiero** ver el historial completo de cambios de etapa de un candidato  
**Para** entender su progreso, auditar decisiones y detectar cuellos de botella

## Descripción

Esta historia implementa la vista de historial de cambios de etapa de una candidatura específica, mostrando todas las transiciones (etapa origen → destino, quién lo movió, cuándo) en orden cronológico. Permite auditar procesos y detectar patrones (ej: candidatos que regresan a etapas anteriores).

## Criterios de Aceptación

```gherkin
Escenario 1: Ver historial completo de etapas
  Dado que un candidato "Laura Martínez" ha pasado por 4 etapas en su proceso
  Cuando el recruiter abre el perfil del candidato en el pipeline
  Y hace clic en "Ver historial"
  Entonces se muestra una línea temporal con:
    | Fecha       | De           | A           | Movido Por      | Tiempo en Etapa |
    | 2025-11-20  | -            | Applied     | Sistema         | -               |
    | 2025-11-21  | Applied      | Screening   | Ana Recruiter   | 1 día           |
    | 2025-11-23  | Screening    | Interview   | Ana Recruiter   | 2 días          |
    | 2025-11-24  | Interview    | Offer       | Carlos Manager  | 1 día           |

Escenario 2: Identificar movimientos inversos
  Dado que un candidato fue movido de "Interview" a "Screening" (regresó)
  Cuando se visualiza el historial
  Entonces el movimiento inverso se resalta con color diferente (ej: amarillo)
  Y se muestra icono de advertencia
  Y se calcula tiempo total en proceso (sumando todos los periodos)

Escenario 3: Filtrar por usuario que movió
  Dado que múltiples usuarios han movido candidatos en el pipeline
  Cuando el recruiter filtra historial por "Ana Recruiter"
  Entonces solo se muestran movimientos realizados por Ana
  Y se mantiene el orden cronológico

Escenario 4: Exportar historial
  Dado que el recruiter visualiza el historial de un candidato
  Cuando hace clic en "Exportar historial"
  Entonces se descarga un CSV con todas las columnas del historial
  Y el archivo incluye metadata (nombre candidato, oferta, fecha exportación)
```

## Notas Técnicas

- **Consulta BD**: Query a `ApplicationStageHistory` con joins a `PipelineStage` (origen y destino) y `User` (quien movió).
- **Cálculo tiempo en etapa**: Diferencia entre `changed_at` de registro actual y anterior.
- **Movimientos inversos**: Comparar `position` de etapas origen/destino para detectar retrocesos.
- **Performance**: Índice en `application_id` + `changed_at` para optimizar query.

## Tareas

- [ ] Diseñar componente de línea temporal en Frontend
- [ ] Implementar endpoint `GET /api/applications/:id/history` en Backend
- [ ] Consultar ApplicationStageHistory con joins a PipelineStage y User
- [ ] Calcular tiempo en cada etapa (diff entre timestamps)
- [ ] Identificar movimientos inversos (comparar posición de etapas)
- [ ] Implementar resaltado visual de movimientos inversos
- [ ] Añadir filtros por usuario y rango de fechas
- [ ] Implementar exportación a CSV
- [ ] Optimizar query con índices en BD
- [ ] Crear tests unitarios de lógica de cálculo de tiempos
- [ ] Crear tests de integración de carga de historial
- [ ] Documentar endpoint en OpenAPI/Swagger

## Estimación

**Story Points:** 5  
**Valor de Negocio:** 3 (Útil para auditoría y análisis)  
**Urgencia:** 3 (Nice-to-have para MVP, crítico para mejora continua)

## Dependencias

- **US-004-02-DragDropCandidatos**: Historial se genera al mover candidatos

## Prioridad

**Media** — Mejora transparencia y permite análisis post-mortem
