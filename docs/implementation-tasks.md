# Tareas de implementacion

## 1. Preparar base del proyecto

**Objetivo:** crear la aplicacion React + TypeScript con Material UI y la estructura minima acordada.

**Alcance incluido:**

- Crear proyecto con Vite, React y TypeScript.
- Instalar Material UI, Emotion e iconos MUI.
- Definir `AppShell` con `ThemeProvider`, `CssBaseline` y layout base.
- Crear estructura inicial de carpetas sin sobrearquitectura.
- Configurar scripts basicos de desarrollo y build.

**Archivos o carpetas previsibles a modificar:**

- `package.json`
- `vite.config.ts`
- `index.html`
- `src/main.tsx`
- `src/app/App.tsx`
- `src/app/AppShell.tsx`
- `src/theme/`
- `src/features/tasks/`

**Criterios de aceptacion:**

- La app arranca en local con Vite.
- Material UI esta disponible y aplicado desde el tema global.
- Existe una pantalla base con AppBar y area principal.
- La estructura sigue `docs/development-guidelines.md`.
- No hay logica de negocio implementada todavia.

**Dependencias:** ninguna.

## 2. Definir modelo de tareas y logica ICE local

**Objetivo:** implementar la base funcional del dominio sin depender aun de la IA.

**Alcance incluido:**

- Definir tipos `Task`, `IceValues`, `PriorityLevel` y estados relacionados.
- Crear utilidades para calcular `iceScore`.
- Crear utilidades para obtener prioridad alta, media o baja.
- Implementar estado local de tareas en memoria.
- Preparar acciones para crear, editar, completar/reabrir y eliminar.

**Archivos o carpetas previsibles a modificar:**

- `src/features/tasks/types.ts`
- `src/features/tasks/utils/ice.ts`
- `src/features/tasks/hooks/useTasks.ts`
- `src/features/tasks/constants.ts`
- `src/app/App.tsx`

**Criterios de aceptacion:**

- Se pueden mantener tareas en memoria durante la sesion.
- El ICE se calcula como `impact * confidence * ease`.
- La prioridad respeta los rangos del alcance funcional.
- Las acciones de tarea estan tipadas y son inmutables.
- No se usa libreria externa de estado.

**Dependencias:** tarea 1.

## 3. Construir dashboard, resumen y listado

**Objetivo:** mostrar la pantalla principal con resumen, listado ordenado por ICE y acciones visibles por tarea.

**Alcance incluido:**

- Crear `TaskDashboardPage`.
- Crear `SummarySection` con contadores basicos.
- Crear `TaskListSection`, `TaskList`, `TaskItem` y `EmptyTasksState`.
- Mostrar titulo, descripcion resumida, estado, valores ICE, puntuacion total y prioridad.
- Ordenar tareas por mayor ICE.
- Mostrar acciones de completar/reabrir, editar y eliminar.

**Archivos o carpetas previsibles a modificar:**

- `src/pages/TaskDashboardPage.tsx`
- `src/features/tasks/components/SummarySection.tsx`
- `src/features/tasks/components/TaskListSection.tsx`
- `src/features/tasks/components/TaskList.tsx`
- `src/features/tasks/components/TaskItem.tsx`
- `src/features/tasks/components/EmptyTasksState.tsx`
- `src/features/tasks/components/IceScore.tsx`

**Criterios de aceptacion:**

- La ruta principal muestra el dashboard.
- Si no hay tareas, aparece un estado vacio con accion de crear.
- Si hay tareas, aparecen ordenadas por ICE descendente.
- Cada tarea muestra estado, ICE y prioridad visual.
- Las acciones por tarea estan presentes y conectadas al estado local.

**Dependencias:** tareas 1 y 2.

## 4. Implementar formulario de creacion y edicion manual

**Objetivo:** permitir crear y editar tareas con valores ICE manuales y validaciones basicas.

**Alcance incluido:**

- Crear `TaskFormPage` o `TaskFormDialog` segun la navegacion elegida.
- Crear campos de titulo, descripcion, impacto, confianza y facilidad.
- Validar titulo obligatorio.
- Validar rangos ICE de 1 a 10.
- Permitir limpiar el formulario.
- Confirmar creacion y edicion de tareas sin usar IA.

**Archivos o carpetas previsibles a modificar:**

- `src/features/tasks/components/TaskForm.tsx`
- `src/features/tasks/components/IceManualFields.tsx`
- `src/features/tasks/components/TaskFormActions.tsx`
- `src/features/tasks/hooks/useTaskForm.ts`
- `src/pages/TaskFormPage.tsx`
- `src/app/App.tsx`

**Criterios de aceptacion:**

- El usuario puede abrir el formulario desde el dashboard.
- El usuario puede crear una tarea con titulo y valores ICE manuales.
- El usuario puede editar titulo, descripcion y valores ICE.
- Los errores aparecen cerca de los campos correspondientes.
- Al guardar, la tarea aparece o se actualiza en el listado ordenado.

**Dependencias:** tareas 1, 2 y 3.

## 5. Completar flujo con sugerencia ICE por IA

**Objetivo:** cerrar el flujo principal: crear tarea, solicitar sugerencia IA, revisar, ajustar y confirmar.

**Alcance incluido:**

- Crear servicio para solicitar sugerencia ICE a Gemini API.
- Usar `VITE_GEMINI_API_KEY` como variable de entorno.
- Crear `AiSuggestionPanel` con impacto, confianza, facilidad, ICE y justificacion.
- Mostrar carga durante la solicitud.
- Manejar descripcion vacia, error de conexion, respuesta invalida, limite agotado y valores fuera de rango.
- Permitir aceptar sugerencia o editar valores manualmente.
- Mostrar confirmacion con `TaskConfirmedSummary` o `Snackbar`.

**Archivos o carpetas previsibles a modificar:**

- `src/services/iceService.ts`
- `src/features/tasks/components/AiSuggestionPanel.tsx`
- `src/features/tasks/components/TaskConfirmedSummary.tsx`
- `src/features/tasks/hooks/useIceSuggestion.ts`
- `src/features/tasks/components/TaskForm.tsx`
- `.env.example`

**Criterios de aceptacion:**

- El boton de calcular ICE con IA exige descripcion.
- Durante la llamada se muestra estado de carga y se evitan solicitudes duplicadas.
- Una respuesta valida rellena los valores sugeridos y muestra justificacion.
- Una respuesta invalida o error permite continuar con edicion manual.
- La tarea confirmada queda visible en el listado ordenado por ICE.
- El flujo completo coincide con `docs/task-creation-flow.mmd`.

**Dependencias:** tareas 1, 2, 3 y 4.
