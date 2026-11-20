# Propuestas a SDA

Choques entre DESY y modern-web-guidance (MWG) con riesgo real. Los skills siguen a DESY y declaran el choque; aquí se recoge lo que conviene proponer al equipo que mantiene la librería. Origen: `docs/modern-web-guidance-gap.md` (sección de choques) y el grill del 2026-11-19.

Verificado contra desy-html v17.0.2.

## 1. Migrar `modal` y `dialog` a `<dialog>` nativo (choque C1)

- **Hoy**: ambos componentes emiten un `<div>` (`src/templates/components/modal/_template.modal.njk:7`, `src/templates/components/dialog/_template.dialog.njk:6`). El cierre por Esc y por clic fuera y el foco atrapado los gestiona `src/js/aria/dialog.js` (líneas 97-105 y 277-285), que lanza un error si falta `role="dialog"` (líneas 131-141). En desy-angular pasa lo mismo: `desy-modals/components/dialog/dialog.component.html` usa `div.dialog-backdrop`.
- **MWG**: `guides/ui-components/dialog.md:20,32` usa `<dialog>` + `showModal()`, que ya trae top layer, `inert`, Esc y la vuelta del foco; y `closedby="any"` para el clic fuera (`dialog.md:88-91`).
- **Ganancia**: menos JS propio, comportamiento del sistema operativo (gesto de atrás en móvil) y no hay que añadir `role` a mano.

## 2. Anillo de foco con `outline` o fallback en `forced-colors` (choque C5)

- **Hoy**: el foco se dibuja con el token `--shadow-outline-focus` (`box-shadow`).
- **MWG**: `guides/accessibility/accessibility.md:331` prefiere `outline` o un fallback basado en `outline` para alto contraste; `guides/visual-design/color.md:33` recuerda que `forced-colors` elimina `box-shadow`.
- **Riesgo**: en Windows con alto contraste el foco puede no verse (WCAG 2.4.7).

## 3. Orden visual y orden de tabulación en `acciones-de-cabecera` (choque C9)

- **Hoy**: el patrón usa `flex-col-reverse lg:flex-row-reverse` sobre botones.
- **MWG**: `guides/css/css-layout.md:74` desaconseja `*-reverse` para reordenar contenido interactivo.
- **Riesgo**: en escritorio el orden de tabulación no coincide con el orden visual (WCAG 2.4.3).
