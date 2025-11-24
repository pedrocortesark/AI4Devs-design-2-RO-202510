# US-005-01 — Builder Visual de Reglas de Automatización

## Narrativa

**Como** recruiter o admin  
**Quiero** configurar reglas de automatización mediante una interfaz visual drag & drop  
**Para** automatizar tareas repetitivas sin necesidad de programar

## Descripción

Esta historia implementa el builder visual no-code donde usuarios pueden crear reglas de automatización definiendo disparadores (eventos), condiciones (filtros) y acciones (email, notificación, cambio de estado). Es el core diferenciador de LTI frente a ATS tradicionales.

## Criterios de Aceptación

```gherkin
Escenario 1: Crear regla básica de automatización
  Dado que un recruiter accede a "Automatizaciones"
  Cuando hace clic en "Nueva regla"
  Y configura:
    - Disparador: "Cuando candidato entra en etapa Screening"
    - Condición: "Si fuente es Referral"
    - Acción: "Enviar email al Hiring Manager"
  Y hace clic en "Guardar y activar"
  Entonces se crea un registro en AutomationRule con status=ACTIVE
  Y se crea un registro en AutomationAction asociado
  Y la regla aparece en el listado de reglas activas

Escenario 2: Vista visual del builder (canvas)
  Dado que el usuario está creando una regla
  Cuando arrastra bloques de disparador, condición y acción al canvas
  Entonces los bloques se conectan visualmente con flechas
  Y se valida compatibilidad entre bloques
  Y se resaltan errores si la configuración es inválida

Escenario 3: Selección de disparadores disponibles
  Dado que el usuario selecciona "Disparador"
  Cuando hace clic en el menú desplegable
  Entonces se muestran opciones:
    - Candidato aplicado a oferta
    - Candidato cambió de etapa
    - Oferta publicada
    - Decisión final tomada
  Y cada opción muestra descripción y ejemplo

Escenario 4: Configuración de acciones con plantillas
  Dado que el usuario selecciona acción "Enviar email"
  Cuando configura destinatarios y mensaje
  Entonces puede usar variables dinámicas: {{candidate_name}}, {{job_title}}, {{stage_name}}
  Y puede previsualizar el email con datos de ejemplo
  Y puede seleccionar plantilla predefinida o crear una custom
```

## Notas Técnicas

- **Modelo de datos**: `AutomationRule` con `trigger_type`, `trigger_config` (JSON), `AutomationAction` con `type`, `config` (JSON).
- **Builder UI**: Canvas drag & drop (similar a Zapier/Make), usar react-flow o similar.
- **Validación**: Reglas solo se pueden activar si configuración es completa y válida.
- **Plantillas**: Biblioteca de plantillas predefinidas de reglas comunes (confirmación aplicación, recordatorio manager).

## Tareas

- [ ] Diseñar UI del builder de automatizaciones (canvas drag & drop)
- [ ] Implementar componente de canvas con react-flow
- [ ] Crear bloques de disparadores, condiciones y acciones
- [ ] Implementar endpoint `POST /api/automation-rules` en Backend
- [ ] Crear migraciones para AutomationRule y AutomationAction
- [ ] Implementar validación de configuración de reglas
- [ ] Crear biblioteca de plantillas predefinidas
- [ ] Implementar preview de emails con variables dinámicas
- [ ] Crear selector de variables disponibles por contexto
- [ ] Implementar guardado y activación de reglas
- [ ] Crear tests unitarios de validación de reglas
- [ ] Crear tests de integración: crear regla → guardar → activar
- [ ] Documentar endpoints en OpenAPI/Swagger

## Estimación

**Story Points:** 13  
**Valor de Negocio:** 5 (Crítico — diferenciador principal)  
**Urgencia:** 5 (MVP bloqueado sin esto)

## Dependencias

- **US-002-03-PublicarOfertaAutomatizaciones**: Infraestructura de eventos debe existir
- **US-004-02-DragDropCandidatos**: Evento de cambio de etapa debe emitirse

## Prioridad

**Crítica** — Core value proposition de LTI
