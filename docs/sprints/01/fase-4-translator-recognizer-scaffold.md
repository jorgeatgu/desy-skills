# Fase 4 — angular-translator, component-recognizer y scaffold-project

## Objetivo

Que la traducción a Angular no meta ARIA redundante ni prefiera `ariaLabel` a un label visible, que el reconocedor no diga que el `dialog` de DESY es `<dialog>` nativo, y que el scaffold y el traductor coincidan en `standalone`.

## Bloqueada por

Ninguna (puede empezar ya). Puede ir en paralelo con F1, F2 y F3.

## Lo que hay hoy

Foto del 2026-11-19. Si el código ha cambiado, manda el código.

- `skills/desy-angular-translator/SKILL.md` **158-160**: `role="button"` y `[tabindex]="0"` sobre `<desy-button>`, que ya renderiza `<button>` (hallazgo 28).
- `skills/desy-angular-translator/SKILL.md` **167**: «En Angular se prefiere `ariaLabel`» (hallazgo 29, en contra de `forms.md:75`).
- `skills/desy-component-recognizer/SKILL.md` **133**: afirma que el componente `dialog` de DESY usa `<dialog>` nativo. Es falso: `components/dialog/_template.dialog.njk:6` emite un `<div>`.
- `skills/desy-scaffold-project/SKILL.md` **183-187**: «`standalone: true` por defecto», que contradice `desy-angular-translator/SKILL.md:409-422` (`standalone: false`, validado leyendo el código) (hallazgo 31).

## Decisiones cerradas

| Tema | Decisión |
|---|---|
| `<desy-button>` | Sin `role` ni `tabindex`. |
| Nombre accesible | Label visible primero. `ariaLabel` solo si no hay texto visible. |
| recognizer:133 | Decir que `modal` y `dialog` son `<div>` con `role="dialog"`, gestionados por `dialog.js`. |
| `standalone` | Manda lo validado en el traductor; antes de editar, comprobar en `desy-angular` (v16.0.4 local) qué usa el starter oficial y dejar el mismo valor en los dos skills. |

## Costuras de test de esta fase

1. Greps (criterios de aceptación).
2. Prompts de §1 `desy-angular-translator` y `desy-component-recognizer`.

## Alcance

1. angular-translator 158-160 y 167.
2. component-recognizer 133.
3. Comprobar `standalone` en desy-angular y alinear scaffold-project 183-187 (o el traductor, si el código lo contradice).

## Fuera de alcance

Mover los logs de validación del traductor (532-652) y la carga de `catalog.json` (`recognizer:54`): son del sprint 02.

## Tareas

- [ ] Rama `fase-4-translator-recognizer-scaffold` desde `docs/mwg-audit-sprint-01` (worktree propio).
- [ ] Greps en rojo.
- [ ] Alcance 1-3, con la cita de desy-angular para `standalone` en la PR.
- [ ] `/code-review` atendido.
- [ ] Merge `--no-ff` en `docs/mwg-audit-sprint-01`, push a `fork` y borrar el worktree (sin PR).

## Criterios de aceptación

- `grep -nE 'role="button"|\[tabindex\]="0"' skills/desy-angular-translator/SKILL.md` = 0 en ejemplos con `<desy-button>`.
- `grep -n 'se prefiere .ariaLabel' skills/desy-angular-translator/SKILL.md` = 0.
- `grep -n '<dialog>\|dialog nativo\|nativo' skills/desy-component-recognizer/SKILL.md` no afirma que DESY use `<dialog>` nativo.
- `grep -n 'standalone' skills/desy-scaffold-project/SKILL.md skills/desy-angular-translator/SKILL.md`: el mismo valor en los dos, con la fuente citada.

## Skills recomendados

- `writing-for-agents`: al reescribir la prosa del SKILL.md.
- `modern-web-guidance`: para recuperar la guía citada y verificar la regla antes de reescribir.
- `desy-validate-accessibility`: su checklist, para revisar los ejemplos tocados.

Cierre: `/implement` para ejecutar, `/code-review` antes de la PR, merge en `docs/mwg-audit-sprint-01` (sin PR por fase; la PR única la abre la fase 5 con `pr`).

## PROMPT

```
Vamos a ejecutar la Fase 4 del Sprint 01 de "desy-skills".

Lee enteros docs/sprints/01/README.md y docs/sprints/01/fase-4-translator-recognizer-scaffold.md, y además:
las líneas citadas de los tres SKILL.md, y en desy-angular (búscalo en ~/desy-evals-work/*/node_modules) cómo se declaran los componentes.

Las decisiones y las costuras de test están cerradas en esos documentos: no se re-preguntan.
Orden: recognizer → translator → standalone (este último requiere leer desy-angular antes de decidir el valor).
Para los prompts de verificación, abre una sesión nueva e indica la ruta del SKILL.md del worktree (los enlaces de ~/.claude/skills apuntan al checkout principal).

Crea el worktree y la rama fase-4-translator-recognizer-scaffold desde docs/mwg-audit-sprint-01 y lanza:
/implement docs/sprints/01/fase-4-translator-recognizer-scaffold.md
Las costuras de la fase son los greps y los prompts: ejecútalos antes (rojo) y después (verde). Para /code-review, el punto fijo es docs/mwg-audit-sprint-01 y la spec es
docs/sprints/01/fase-4-translator-recognizer-scaffold.md. Atiende sus hallazgos y después mergea con --no-ff en docs/mwg-audit-sprint-01 y haz push a fork. No abras PR: la única PR la abre la fase 5.
```
