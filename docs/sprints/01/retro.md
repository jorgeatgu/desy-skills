# Retrospectiva — Sprint 01

Escrita al cerrar la fase 5 siguiendo los pasos del skill `retro` (que no se puede invocar desde el agente: `disable-model-invocation`). Fuentes: las sesiones del sprint en `~/.claude/projects/-Users-jorgeatgu-github-desy-skills/`, los mensajes de merge de F1-F4 (`git log --format=%B main..docs/mwg-audit-sprint-01`) y las costuras de F5.

## Qué funcionó

- **Fases sin ficheros compartidos.** F1-F4 tocaban un SKILL.md distinto cada una y se mergearon sin conflictos, en paralelo y cada una en su worktree.
- **Texto común copiado literal del README.** La línea de choque de modales y el patrón de errores salieron idénticos en `implement-component` y `validate-accessibility`: el diff a tres bandas de F5 (README y los dos skills) da vacío sin retoques.
- **Greps como costura.** Todos los criterios prohibidos de F1-F4 dan rojo contra `main` y 0 en la rama. Son baratos, deterministas y sirven también de contraste en la PR.
- **«Manda el código» frente a la foto de la fase.** Cuando la fase y desy-html no coincidían (accordion con el botón dentro del heading en F2, desy-angular v16.0.4 que no estaba en local en F4), el agente siguió el código y lo dejó anotado en el merge en lugar de forzar el cambio.
- **Notas «para la retro» en el commit de merge.** Lo diferido quedó en el historial con su motivo, y esta retro se ha podido escribir desde ahí sin releer sesiones enteras.

## Qué no

- **Fotos de líneas desfasadas y greps que ya daban 0 en rojo.** En F1, «foco en el título» no existía en `implement-component` y el grep de `title=` + ventana ya daba 0 antes del cambio: hubo que inventar un grep reforzado para tener rojo. Un criterio que no puede fallar no es una costura.
- **Greps que marcan la prosa que prohíbe el marcado.** Para que el grep de `<button>` sin `type` diera 0 en F2, se reescribió prosa del skip-link («elemento `button`») sin cambiar su contenido. Pasa lo mismo con `title` + ventana en F3: las líneas que lo prohíben coinciden con el grep reforzado.
- **Prompts en rojo que no reproducen el fallo.** En F4 los dos agentes corrigieron el error por su cuenta leyendo la documentación oficial: el rojo solo lo demostraron los greps. F4 lanzó además los prompts en subagentes en lugar de en sesiones nuevas.
- **Los prompts combinados no distinguen rojo de verde.** En F5, 2.1 y 2.2 pasan sus criterios con los skills de `main` y con los de la rama. El mockup de 2.1 (acceso-cargando) no tiene formulario ni modal, así que los greps de submit y de `type` pasan sin nada que comprobar. En 2.2 el gold ya trae el marcado correcto y el agente lo copia. Además, en 2.1 ningún run recompiló la página generada: hubo que lanzar `npm run build` a mano para pasar el checklist sobre `dist/`.
- **Prompts combinados caros de montar.** 2.1 y 2.2 citan un mockup que no está en local y un servidor en `localhost:4300`. En F5 hubo que capturar el mockup a mano y adjuntar el gold desde `~/github/desy-direccion-postal`; la primera captura con `npx playwright` la bloqueó el clasificador de permisos y la segunda falló por versión del navegador.
- **`sed` con argumentos mal citados.** Dos errores repetidos en el sprint (`sed: can't read s/…`): `sed -i` de BSD tomó la expresión como fichero. Ediciones con backticks y comillas van mejor con la herramienta Edit o con un script.
- **Ramas locales sin borrar.** `fase-1` a `fase-4` siguen en el checkout principal tras el merge.

## Mejoras al entorno del agente

Por orden de severidad:

1. **Guardrail que ejecute los greps (check automático).** El repo no tiene CI ni pre-commit. Los greps de los criterios de F1-F4 (más el diff del texto común) caben en un script `scripts/check-skills.sh` y en un job de GitHub Actions. Así una regresión en un SKILL.md falla en la PR, no en la siguiente auditoría. Base: el script de F5 `greps.sh <ref|WORK>`, que ya compara una ref con el árbol de trabajo.
2. **Criterios que demuestren rojo antes de aceptarse (estándares de la spec).** En `/create-feature`, cada grep de criterio debe venir con su salida contra la rama base. Si da 0 antes del cambio, se reescribe antes de cerrar la fase.
3. **Separar marcado de prosa en los greps.** Que los greps busquen dentro de bloques de código (```` ```html ````/```` ```njk ````) y no en la prosa. Así no hace falta reescribir frases que prohíben el marcado para que el grep pase.
4. **Insumos de los prompts combinados en el repo de evals (acceso a información).** Guardar `mockup-portal.png` (acceso-cargando) y el gold de paso-3 en `desy-skills-evals` y citarlos por ruta en `docs/skill-prompts-collection.md` §2. Así nadie depende de capturar ni de servir nada en cada sprint. Para que el combinado sea costura de este tipo de huecos, su mockup debe tener formulario y un modal destructivo (p. ej. paso-3 + confirmación de borrado), y el prompt debe pedir `npm run build` antes de validar.
5. **Huecos en los skills que declaró el propio agente en 2.1**, para el sprint 02:
   - `desy-scaffold-project` dice «no ejecutes los bloques bash» aunque el usuario pida el scaffold en una sesión no interactiva.
   - El caso fijo «acceso-cargando» de `linter.py` propone `header-mini`, pero el gold usa `header`.
   - El catálogo cita desy-html v16.x y la versión instalada es la 17.0.2.
   - El `.md` del patrón en desy.aragon.es solo trae los títulos.
   - El botón de cerrar de la macro `notification` lleva `aria-label="X: Cerrar notificación"`: es para `docs/propuestas-sda.md`.
6. **Puntero de navegación a desy-angular.** F4 buscó la v16.0.4 en `~/desy-evals-work/*/node_modules` y no estaba; la fuente acabó siendo el starter oficial y `gorilas/desy-angular`. Una línea en `CONTRIBUTING.md` con dónde está la copia local de desy-html y de desy-angular de referencia ahorra esa búsqueda.

## Qué cambia para el siguiente

- **El plan del sprint 02 se amplía** con las filas que la auditoría marca *pendiente sprint 02* (12 y 20). Hoy el README solo lo plantea como sprint de forma de los skills.
- **Sprint 02 hereda** lo diferido en los merges:
  - `implement-component`: el ejemplo de desy-ionic sin foco en Cancelar (filas 3-4); un ítem destructivo para desy-angular; una línea `Choque con MWG` propia para `aria-invalid`/`aria-errormessage` (C3), que hoy solo se describe dentro del patrón de errores.
  - `layout-patterns`: la notificación escrita a mano; el skip-link (filas 23-26).
  - `validate-accessibility`: el borde de error del checklist (`alert-dark` frente a `border-alert-base ring-2` de la macro).
  - `scaffold-project`: «peer dependencies de Angular 19».
  - `angular-translator`: el `routerLink`, `*appCustomInnerContent`, `isSelect` y avisar de que `<desy-button>` es `submit` por defecto.
  - `component-recognizer`: `linter.py` desalineado con el SKILL.md.
- **Las fases citan texto, no números de línea**, y cada criterio de grep trae su salida en rojo contra la base.
- **Los prompts de verificación se lanzan con `claude -p`** en un directorio aislado (sesión nueva de verdad), no en subagentes, y con los insumos por ruta.
- **Borrar la rama de fase tras el merge** (`git branch -d fase-N-…`) como último paso de cada fase.
