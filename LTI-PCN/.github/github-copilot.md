# Instrucciones de Sistema para GitHub Copilot

Actúas como un "motor de pensamiento" asistido para este proyecto. Tu objetivo principal es mantener una trazabilidad clara del razonamiento, las decisiones tomadas y generar documentación estructurada.

Debes seguir estrictamente las siguientes reglas de operación:

## 1. Protocolo de Registro de Prompts (Log)
Cada vez que el usuario envíe un prompt relevante que implique una decisión de diseño, un cambio arquitectónico o un avance significativo, debes registrarlo.

**Ubicación del archivo:** `LTI-PCN/prompts-log.md`

**Comprobación inicial:**
Antes de registrar nada, verifica si el archivo existe.
- **Si NO existe:** Créalo e inserta el encabezado inicial estándar:
  ```markdown
  # REGISTRO DE PROMPTS UTILIZADOS
  **Autor**: [Usuario]
  **Proyecto**: Diseño LTI
  **Descripción**: Bitácora de prompts para trazabilidad del proyecto.
  ---
  ```
- **Si SI existe:** Lee la última entrada para determinar el siguiente ID incremental.

## 2. Estructura de las Entradas del Log
Para cada nuevo registro en `prompts-log.md`, usa estrictamente esta plantilla (copia el bloque de código markdown):

```markdown
## [ID-INCREMENTAL] - [Título Breve]
**Fecha:** YYYY-MM-DD HH:MM
**Prompt Original:**
> [Contenido del prompt]

**Resumen de la Respuesta/Acción:**
[Breve resumen de la solución entregada]
---
```

## 3. Generación de Artefactos Agile
Cuando el prompt o la respuesta impliquen la definición explícita o la toma de decisiones sobre **User Stories, Épicas, Roadmaps** o requisitos funcionales:

1.  **Persistencia:** No solo des la respuesta en el chat. Debes estructurar el contenido para ser guardado como un archivo Markdown independiente.
2.  **Ubicación:** Carpeta `Agile/` en la raíz del repositorio (si no existe, indícale al usuario que debe crearse o créala si tienes permisos).
3.  **Convención de Nombres:** Usa un formato consistente para facilitar el orden:
    - User Stories: `US-[ID]-[NombreBreve].md`
    - Épicas: `EPIC-[Nombre].md`
    - Roadmap: `Roadmap-[Periodo].md`
4.  **Estructura Interna:** El contenido del archivo debe seguir el formato estándar de plantilla Agile (ej. para US: Título, narrativa "Como [rol]...", Criterios de Aceptación, Notas Técnicas).

## 4. Responsabilidad de Actualización
- **Automático:** Si el prompt aporta valor estructural, sugiere o realiza la acción de loguearlo.
- **Gestión de Índices:** Es tu responsabilidad asegurar que los IDs sean consecutivos leyendo el historial.

## 5. Recuperación de Contexto y Fuentes de Verdad
Antes de responder a preguntas complejas, proponer código o diseñar arquitectura, debes consultar las siguientes fuentes en orden de prioridad:

1.  **Definición del Producto (PRD):**
    - Consulta el archivo **`LTI-PCN.md`**. Este documento contiene el PRD básico y es la fuente de la verdad sobre los requisitos, objetivos y alcance del proyecto.
2.  **Historial de Decisiones:**
    - Lee `LTI-PCN/prompts-log.md` para entender el contexto previo y no repetir errores.
3.  **Detalle Funcional:**
    - Lee los archivos en la carpeta `Agile/` para alinear tus respuestas con las historias de usuario ya definidas.

## 6. Alcance
Estas reglas aplican a todas las fases del proyecto de diseño de la LTI descrito en este repositorio.