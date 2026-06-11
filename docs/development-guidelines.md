# Guia tecnica de desarrollo

## 1. Estructura de carpetas

- [ ] Usar `src/app` para composicion raiz.
  Decision: providers, tema y layout base viven aqui.
- [ ] Usar `src/pages` solo si hay pantallas reales.
  Decision: no crear rutas antes de necesitarlas.
- [ ] Usar `src/features` para flujos de negocio.
  Decision: una feature agrupa UI, hooks y tipos propios.
- [ ] Usar `src/components` para UI compartida.
  Decision: solo componentes reutilizables entre features.
- [ ] Usar `src/hooks` para hooks compartidos.
  Decision: no mover hooks usados por una sola feature.
- [ ] Usar `src/services` para llamadas externas.
  Decision: React no conoce detalles de la API.
- [ ] Usar `src/types` para tipos globales.
  Decision: los tipos locales se quedan cerca de su uso.
- [ ] Usar `src/utils` para funciones puras.
  Decision: sin estado, sin efectos y sin dependencias de React.
- [ ] Usar `src/theme` para Material UI.
  Decision: tema, tokens y ajustes visuales globales.
- [ ] Crear carpetas solo cuando haya contenido real.
  Decision: evitar estructura vacia o preventiva.

## 2. Convenciones de nombres

- [ ] Nombrar componentes en `PascalCase`.
  Decision: archivo y componente comparten nombre.
- [ ] Nombrar hooks con prefijo `use`.
  Decision: ejemplo de forma, no de implementacion.
- [ ] Nombrar funciones en `camelCase`.
  Decision: usar verbos claros para acciones.
- [ ] Nombrar tipos en `PascalCase`.
  Decision: evitar prefijos como `I` o `T`.
- [ ] Nombrar handlers como `handleAccion`.
  Decision: el nombre describe el evento gestionado.
- [ ] Nombrar props booleanas con `is`, `has` o `can`.
  Decision: la lectura debe sonar afirmativa.
- [ ] Nombrar constantes globales en `SCREAMING_SNAKE_CASE`.
  Decision: solo para valores compartidos e inmutables.
- [ ] Nombrar archivos de hooks en `camelCase`.
  Decision: mantener el nombre del hook como referencia.
- [ ] Nombrar servicios por recurso.
  Decision: `taskService`, `iceService` o equivalente.
- [ ] Evitar abreviaturas ambiguas.
  Decision: preferir nombres largos pero claros.

## 3. Organizacion de componentes

- [ ] `App` compone la aplicacion.
  Decision: tema, providers y estructura principal.
- [ ] Las paginas coordinan flujos.
  Decision: reciben datos y conectan secciones.
- [ ] Las features contienen comportamiento de negocio.
  Decision: formularios, listas y acciones del dominio.
- [ ] Los componentes compartidos son presentacionales.
  Decision: sin reglas de negocio internas.
- [ ] Los formularios validan entrada local.
  Decision: emiten datos normalizados al guardar.
- [ ] Los items muestran una entidad.
  Decision: no hacen llamadas API directas.
- [ ] `IceScore` o equivalentes muestran calculos.
  Decision: no duplicar formulas en varios componentes.
- [ ] Los componentes aceptan props minimas.
  Decision: pasar datos ya preparados cuando sea posible.
- [ ] Dividir componentes por responsabilidad.
  Decision: no dividir solo por numero de lineas.
- [ ] Usar Material UI como base visual.
  Decision: preferir componentes MUI antes que UI propia.

## 4. Uso de hooks

- [ ] Usar `useState` para estado local simple.
  Decision: formularios, toggles y estados pequenos.
- [ ] Usar `useReducer` solo con transiciones relacionadas.
  Decision: evitarlo para estado trivial.
- [ ] Usar `useEffect` para sincronizar sistemas externos.
  Decision: no usarlo para calculos derivados.
- [ ] Crear custom hooks para logica reutilizable.
  Decision: reutilizable significa usada en mas de un lugar.
- [ ] Mantener hooks cerca de su feature.
  Decision: mover a `src/hooks` solo si son compartidos.
- [ ] Respetar dependencias exhaustivas.
  Decision: no silenciar reglas sin motivo documentado.
- [ ] Usar `useMemo` solo ante coste real.
  Decision: no optimizar por costumbre.
- [ ] Usar `useCallback` solo si estabiliza props necesarias.
  Decision: evitar ruido en componentes pequenos.
- [ ] Limpiar efectos asincronos o suscripciones.
  Decision: prevenir estados obsoletos.
- [ ] No llamar hooks en condiciones o bucles.
  Decision: mantener el orden de ejecucion estable.

## 5. Gestion del estado

- [ ] Mantener el estado cerca de quien lo usa.
  Decision: subirlo solo si varios componentes lo necesitan.
- [ ] Usar estado local para el MVP.
  Decision: `App` puede ser el propietario inicial.
- [ ] Evitar Redux, Zustand o Context global al inicio.
  Decision: agregarlos solo con necesidad demostrada.
- [ ] Separar estado de dominio y estado de UI.
  Decision: tareas por un lado, modales y cargas por otro.
- [ ] Calcular datos derivados al renderizar o con selector.
  Decision: evitar duplicar `iceScore` si se puede derivar.
- [ ] Actualizar colecciones de forma inmutable.
  Decision: crear nuevos arrays y objetos.
- [ ] Usar tipos union para estados finitos.
  Decision: `idle`, `loading`, `success`, `error`.
- [ ] Mantener filtros y ordenacion como estado minimo.
  Decision: la lista visible se deriva de tareas y criterio.
- [ ] Usar `localStorage` solo como mejora opcional.
  Decision: aislarlo en hook o servicio.
- [ ] No guardar respuestas externas sin normalizar.
  Decision: convertirlas al modelo interno.

## 6. Gestion de llamadas API

- [ ] Usar `fetch` como cliente HTTP base.
  Decision: no anadir Axios para el MVP.
- [ ] Centralizar llamadas en `src/services`.
  Decision: los componentes consumen funciones limpias.
- [ ] Leer claves desde variables `VITE_`.
  Decision: nunca hardcodear secretos en codigo.
- [ ] Tratar la clave frontend como expuesta.
  Decision: usar solo claves de demo restringidas.
- [ ] Mapear respuestas externas a tipos internos.
  Decision: no propagar formatos de proveedor.
- [ ] Validar JSON devuelto por la IA.
  Decision: rechazar datos incompletos o no parseables.
- [ ] Validar rangos ICE entre 1 y 10.
  Decision: no aplicar sugerencias fuera de rango.
- [ ] Cancelar peticiones obsoletas cuando aplique.
  Decision: usar `AbortController` en flujos sensibles.
- [ ] Devolver resultado, carga y error a la UI.
  Decision: no ocultar estados dentro del servicio.
- [ ] Reintentar solo por accion del usuario.
  Decision: evitar bucles automaticos innecesarios.

## 7. Manejo de errores y cargas

- [ ] Cada accion asincrona tiene estado explicito.
  Decision: `idle`, `loading`, `success` o `error`.
- [ ] Bloquear acciones duplicadas durante carga.
  Decision: deshabilitar botones relacionados.
- [ ] Mostrar errores cerca del origen.
  Decision: usar `Alert`, `Snackbar` o texto de ayuda MUI.
- [ ] Mantener alternativa manual para ICE.
  Decision: la IA ayuda, pero no bloquea el flujo.
- [ ] Validar antes de llamar a la API.
  Decision: evitar peticiones con descripcion vacia.
- [ ] No mostrar errores tecnicos crudos al usuario.
  Decision: traducirlos a mensajes accionables.
- [ ] Permitir recuperacion visible.
  Decision: reintentar, editar o continuar manualmente.
- [ ] Mostrar estados vacios de listas.
  Decision: diferenciar sin datos de error.
- [ ] Usar indicadores MUI de progreso.
  Decision: spinner o skeleton segun contexto.
- [ ] Registrar detalles solo en desarrollo.
  Decision: no depender de consola para la experiencia.

## 8. Librerias aprobadas

- [ ] Usar React.
  Decision: base de UI del proyecto.
- [ ] Usar TypeScript.
  Decision: tipos estrictos y contratos claros.
- [ ] Usar Vite.
  Decision: entorno simple para React.
- [ ] Usar Material UI.
  Decision: componentes alineados con Material Design.
- [ ] Usar `@mui/icons-material` cuando haya iconos.
  Decision: no dibujar iconos propios salvo necesidad.
- [ ] Usar Emotion solo como dependencia de MUI.
  Decision: no crear otro sistema de estilos paralelo.
- [ ] Usar `fetch` para HTTP.
  Decision: evitar clientes adicionales.
- [ ] Usar APIs nativas para fechas simples.
  Decision: no anadir libreria de fechas al MVP.
- [ ] Evitar librerias de estado externas.
  Decision: reevaluar solo si el estado crece.
- [ ] Aprobar nuevas librerias por necesidad concreta.
  Decision: cada dependencia debe resolver un problema real.
