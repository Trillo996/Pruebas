# Alcance funcional del MVP: Gestor de Tareas Inteligente con modelo ICE

## 1. Objetivo del MVP

Construir una aplicacion React simple para gestionar tareas tipo ToDo y priorizarlas automaticamente mediante el modelo ICE:

**ICE = Impacto x Confianza x Facilidad**

El usuario podra escribir una descripcion de una tarea y la aplicacion usara una API de IA gratuita para sugerir valores de Impacto, Confianza y Facilidad. El resultado permitira ordenar visualmente las tareas por prioridad.

El MVP esta pensado para un curso de React, por lo que debe priorizar aprendizaje practico, claridad y alcance acotado.

## 2. Alcance incluido

### 2.1 Gestion basica de tareas

La aplicacion debe permitir:

- Crear una tarea con titulo obligatorio.
- Agregar una descripcion opcional para tareas creadas manualmente.
- Ver una lista de tareas creadas durante la sesion.
- Marcar una tarea como completada o pendiente.
- Editar titulo, descripcion y valores ICE.
- Eliminar una tarea.
- Ver la fecha/hora de creacion en formato simple.

### 2.2 Priorizacion con modelo ICE

Cada tarea tendra tres valores numericos:

- **Impacto:** beneficio esperado si se completa la tarea.
- **Confianza:** seguridad de que la tarea aporta el valor esperado.
- **Facilidad:** que tan simple es completar la tarea.

Cada valor se puntuara de **1 a 10**.

La aplicacion calculara:

```text
ICE = Impacto x Confianza x Facilidad
```

Ejemplo:

```text
Impacto: 8
Confianza: 7
Facilidad: 6
ICE: 336
```

### 2.3 Calculo inteligente de ICE con IA

El usuario podra pulsar un boton como **"Calcular ICE con IA"** despues de escribir la descripcion de una tarea.

Para crear una tarea, la descripcion no sera obligatoria. Para usar el calculo con IA, la descripcion si sera obligatoria porque es el texto que la API analizara para sugerir los valores ICE.

La aplicacion enviara la descripcion a una API de IA gratuita y recibira una sugerencia con:

- Impacto sugerido.
- Confianza sugerida.
- Facilidad sugerida.
- Breve justificacion.

El usuario podra aceptar o modificar manualmente los valores sugeridos.

API recomendada para el curso:

- **Google Gemini API**, usando su nivel gratuito para pruebas.
- Documentacion oficial de precios: https://ai.google.dev/gemini-api/docs/pricing
- Documentacion oficial de limites: https://ai.google.dev/gemini-api/docs/rate-limits

Nota importante: al ser una aplicacion sin backend, cualquier API key usada desde React quedara expuesta en el navegador. Esto es aceptable solo para una demo educativa local con una clave restringida y de bajo riesgo. En una aplicacion real se deberia usar un backend o proxy seguro, pero eso queda fuera del MVP.

### 2.4 Ordenacion y visualizacion

La lista de tareas debe mostrar:

- Titulo.
- Estado: pendiente o completada.
- Puntuaciones de Impacto, Confianza y Facilidad.
- Puntuacion ICE total.
- Justificacion generada por IA, si existe.

La lista debe poder ordenarse automaticamente por mayor puntuacion ICE.

Tambien se recomienda mostrar una etiqueta visual simple:

- **Alta prioridad:** ICE alto.
- **Media prioridad:** ICE medio.
- **Baja prioridad:** ICE bajo.

Propuesta de rangos:

```text
Alta prioridad: ICE >= 500
Media prioridad: ICE entre 200 y 499
Baja prioridad: ICE < 200
```

## 3. Alcance no incluido

El MVP no incluye:

- Backend.
- Autenticacion.
- Persistencia real.
- Paginacion.
- Multiusuario.
- Tags.
- Roles o permisos.
- Subtareas.
- Adjuntos.
- Fechas limite avanzadas.
- Notificaciones.
- Integracion con calendarios.
- Filtros complejos.

La informacion puede mantenerse solo durante la sesion. Si se usa `localStorage`, debe tratarse como mejora opcional de clase, no como persistencia real del MVP.

## 4. Pantallas o secciones principales

### 4.1 Formulario de nueva tarea

Campos:

- Titulo.
- Descripcion.
- Impacto.
- Confianza.
- Facilidad.

Acciones:

- Crear tarea.
- Calcular ICE con IA.
- Limpiar formulario.

Estados esperados:

- Cargando mientras se consulta la IA.
- Error si la API falla.
- Resultado sugerido por IA.

### 4.2 Lista de tareas

Cada tarea debe mostrar:

- Titulo.
- Descripcion resumida.
- Estado.
- Valores ICE.
- Puntuacion total.
- Prioridad visual.

Acciones por tarea:

- Completar/reabrir.
- Editar.
- Eliminar.

### 4.3 Resumen simple

La aplicacion puede mostrar indicadores basicos:

- Total de tareas.
- Tareas pendientes.
- Tareas completadas.
- Tarea con mayor ICE.

## 5. Modelo de datos propuesto

```js
const task = {
  id: "uuid-o-timestamp",
  title: "Preparar presentacion del proyecto",
  description: "Crear una presentacion para explicar el MVP en clase",
  impact: 8,
  confidence: 7,
  ease: 6,
  iceScore: 336,
  aiReason: "Tiene impacto alto porque ayuda a comunicar el proyecto, con dificultad moderada.",
  completed: false,
  createdAt: "2026-06-09T10:30:00.000Z"
};
```

## 6. Flujo principal de usuario

1. El usuario escribe el titulo de una tarea.
2. El usuario puede agregar una descripcion.
3. Si agrego una descripcion, puede pulsar **Calcular ICE con IA**.
4. Si usa IA, la aplicacion envia la descripcion a la API.
5. La IA devuelve Impacto, Confianza, Facilidad y una justificacion.
6. La aplicacion calcula el ICE total.
7. El usuario revisa los valores sugeridos o introduce valores manuales.
8. El usuario crea la tarea.
9. La tarea aparece en la lista ordenada por prioridad.
10. El usuario puede completar, editar o eliminar la tarea.

## 7. Prompt sugerido para la IA

```text
Eres un asistente que prioriza tareas usando el modelo ICE.

Analiza la siguiente tarea y devuelve un JSON valido con:
- impact: numero entero de 1 a 10
- confidence: numero entero de 1 a 10
- ease: numero entero de 1 a 10
- reason: explicacion breve en una frase

Reglas:
- Impacto mide el beneficio esperado.
- Confianza mide la seguridad sobre ese beneficio.
- Facilidad mide que tan simple es completar la tarea.
- No devuelvas texto fuera del JSON.

Tarea:
"{{descripcion}}"
```

Respuesta esperada:

```json
{
  "impact": 8,
  "confidence": 7,
  "ease": 6,
  "reason": "La tarea tiene buen impacto para el proyecto, aunque requiere preparacion moderada."
}
```

## 8. Validaciones y alternativas funcionales

La aplicacion debe contemplar:

- Descripcion vacia al intentar calcular ICE con IA.
- Error de conexion con la API.
- Respuesta invalida de la IA.
- Limite gratuito agotado.
- Valores fuera de rango.

En caso de error, el usuario debe poder introducir los valores manualmente.

## 9. Criterios de aceptacion

El MVP se considera completo cuando:

- Se pueden crear tareas desde un formulario.
- Cada tarea tiene Impacto, Confianza, Facilidad e ICE calculado.
- Se puede solicitar una sugerencia ICE usando una API de IA gratuita.
- La tarea se puede editar, completar y eliminar.
- La lista muestra primero las tareas con mayor ICE.
- La aplicacion funciona sin backend.
- La aplicacion no requiere autenticacion.
- Los datos no necesitan mantenerse al recargar la pagina.
- El codigo es comprensible para estudiantes de React.

## 10. Propuesta de iteraciones de desarrollo

### Iteracion 1: ToDo basico

- Crear proyecto React.
- Crear formulario.
- Agregar tareas en memoria.
- Listar tareas.
- Completar y eliminar tareas.

### Iteracion 2: Modelo ICE manual

- Agregar campos Impacto, Confianza y Facilidad.
- Calcular ICE.
- Ordenar tareas por ICE.
- Mostrar prioridad alta, media o baja.

### Iteracion 3: Integracion con IA

- Crear funcion para llamar a la API.
- Enviar descripcion de la tarea.
- Parsear respuesta JSON.
- Rellenar valores ICE sugeridos.
- Mostrar justificacion.
- Manejar errores.

### Iteracion 4: Pulido final

- Editar tareas.
- Agregar resumen simple.
- Mejorar estados de carga y error.
- Revisar accesibilidad basica del formulario.

## 11. Riesgos y decisiones

- **Riesgo:** la API gratuita puede cambiar limites o disponibilidad.
  **Decision:** mantener el calculo manual como alternativa obligatoria.

- **Riesgo:** la API key queda visible en frontend.
  **Decision:** usar solo una clave de demo restringida para el curso.

- **Riesgo:** la IA puede devolver valores inconsistentes.
  **Decision:** validar que los valores esten entre 1 y 10 antes de aplicarlos.

- **Riesgo:** el alcance crece demasiado.
  **Decision:** mantener fuera backend, autenticacion, persistencia real, paginacion, multiusuario y tags.

## 12. Resultado esperado

Una aplicacion React sencilla, funcional y didactica que permita crear tareas, calcular su prioridad con ICE y usar IA como apoyo para estimar las puntuaciones desde una descripcion.

El foco del MVP es aprender React construyendo una experiencia completa, pero acotada.
