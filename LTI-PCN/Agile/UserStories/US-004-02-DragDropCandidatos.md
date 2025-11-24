# US-004-02 — Mover Candidatos Entre Etapas (Drag & Drop)

## Narrativa

**Como** recruiter  
**Quiero** mover candidatos entre etapas arrastrando sus tarjetas  
**Para** actualizar su progreso en el pipeline de forma rápida e intuitiva

## Descripción

Esta historia implementa la funcionalidad de drag & drop para mover candidatos entre etapas del pipeline. Al soltar un candidato en una nueva etapa, se actualiza su estado en BD, se registra en historial y se disparan automatizaciones configuradas.

## Criterios de Aceptación

```gherkin
Escenario 1: Mover candidato entre etapas exitosamente
  Dado que un candidato "Laura Martínez" está en etapa "Applied"
  Cuando el recruiter arrastra su tarjeta a la columna "Screening"
  Y suelta la tarjeta
  Entonces se actualiza Application.current_stage_id al ID de "Screening"
  Y se crea registro en ApplicationStageHistory con:
    - from_stage_id = ID de "Applied"
    - to_stage_id = ID de "Screening"
    - changed_by_user_id = recruiter actual
    - changed_at = timestamp actual
  Y la tarjeta se mueve visualmente a la nueva columna
  Y se dispara evento de dominio "ApplicationStageChanged"

Escenario 2: Feedback visual durante drag
  Dado que el recruiter está arrastrando una tarjeta
  Cuando la tarjeta está sobre una columna válida
  Entonces la columna destino se resalta visualmente
  Y se muestra preview de la tarjeta en la nueva posición
  Cuando suelta la tarjeta
  Entonces se muestra loading spinner mientras se procesa
  Y se confirma visualmente cuando la operación completa

Escenario 3: Manejo de error en actualización
  Dado que el recruiter mueve un candidato a nueva etapa
  Cuando el Backend devuelve error (BD no disponible, timeout)
  Entonces la tarjeta vuelve a su posición original con animación
  Y se muestra mensaje de error "No se pudo actualizar el candidato. Intenta de nuevo"
  Y no se persiste ningún cambio en BD

Escenario 4: Restricción de movimientos inválidos
  Dado que existen etapas finales (Hired, Rejected)
  Cuando el recruiter intenta mover un candidato desde "Hired" a "Interview"
  Entonces el sistema no permite el movimiento
  Y muestra tooltip "No se puede mover candidatos desde etapas finales"
```

## Notas Técnicas

- **Drag & drop**: Usar react-beautiful-dnd con lógica de onDragEnd.
- **Optimistic UI**: Mover tarjeta visualmente antes de confirmar con Backend (rollback si falla).
- **Eventos de dominio**: Emitir `ApplicationStageChanged` al bus interno para disparar automatizaciones.
- **Validaciones**: No permitir movimientos desde etapas finales (is_final=true).

## Tareas

- [ ] Implementar onDragEnd handler en componente Kanban
- [ ] Implementar endpoint `PATCH /api/applications/:id/stage` en Backend
- [ ] Validar que etapa destino pertenece al mismo pipeline
- [ ] Actualizar Application.current_stage_id en BD
- [ ] Crear registro en ApplicationStageHistory
- [ ] Emitir evento de dominio "ApplicationStageChanged" al bus
- [ ] Implementar optimistic UI update con rollback en caso de error
- [ ] Implementar animaciones de drag & drop
- [ ] Validar movimientos desde etapas finales (bloquear si is_final=true)
- [ ] Implementar loading states y feedback visual
- [ ] Crear tests unitarios de validaciones
- [ ] Crear tests de integración: drag & drop → BD → evento → rollback
- [ ] Documentar endpoint en OpenAPI/Swagger

## Estimación

**Story Points:** 8  
**Valor de Negocio:** 5 (Crítico — interacción principal)  
**Urgencia:** 5 (Bloqueante para MVP funcional)

## Dependencias

- **US-004-01-VistaKanban**: Vista kanban debe existir
- Bus de eventos interno configurado (para disparar automatizaciones)

## Prioridad

**Crítica** — Sin drag & drop el pipeline es solo vista estática
