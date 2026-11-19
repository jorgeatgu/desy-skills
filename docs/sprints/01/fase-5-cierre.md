# Fase 5 — Cierre del sprint

## Objetivo

Comprobar que, con las cuatro fases mergeadas, los flujos completos generan HTML y Angular sin los huecos corregidos, que el texto común es idéntico en todos los skills, y dejar la auditoría al día y la retro escrita.

## Bloqueada por

F1, F2, F3 y F4.

## Lo que hay hoy

- `docs/skill-prompts-collection.md`: prompts combinados **2.1** (mockup → portal Nunjucks) y **2.2** (HTML gold → Angular).
- `docs/modern-web-guidance-gap.md`: tabla del eje 1, todavía sin columna de estado.

## Decisiones cerradas

Las del README. No se introducen cambios en los skills en esta fase, salvo correcciones de coherencia de una línea.

## Costuras de test de esta fase

- Los greps de F1-F4 sobre todo `skills/`, con `docs/mwg-audit-sprint-01` actualizada.
- Prompts combinados 2.1 y 2.2, ahora sí con los skills instalados (el checkout principal en `docs/mwg-audit-sprint-01`, porque los enlaces de `~/.claude/skills` apuntan a él).
- Los greps nuevos de F3 aplicados al HTML que generen 2.1 y 2.2.

## Alcance

1. Ejecutar todos los greps de F1-F4 sobre `skills/`.
2. Comprobar que la línea de choque de modales y el patrón de errores son idénticos en F1 y F3.
3. Lanzar 2.1 y 2.2 y pasar el checklist offline de F3 al resultado.
4. Añadir a la tabla del eje 1 de la auditoría una columna «Estado» (resuelto en sprint 01 / descartado / pendiente sprint 02 / fuera).
5. Lanzar `/retro` y volcar sus propuestas en `retro.md`.

## Fuera de alcance

Enviar `docs/propuestas-sda.md` a SDA (lo hace una persona cuando quiera) y el sprint 02.

## Tareas

- [ ] Trabajar en el checkout principal sobre `docs/mwg-audit-sprint-01`, con F1-F4 mergeadas y `git pull` hecho.
- [ ] Alcance 1-5.
- [ ] `/code-review` atendido.
- [ ] Push a `fork` y PR única `jorgeatgu:docs/mwg-audit-sprint-01` → `minilama2222/desy-skills:main`, con el skill `pr`.

## Criterios de aceptación

- `diff <(grep -A1 'Choque con MWG .dialog.' skills/desy-implement-component/SKILL.md) <(grep -A1 'Choque con MWG .dialog.' skills/desy-validate-accessibility/SKILL.md)` sin diferencias en la línea de choque.
- Los greps prohibidos de F1-F4 dan 0 sobre `skills/`.
- La salida de 2.1 pasa los 5 greps nuevos de F3; la de 2.2 no tiene `[disabled]` en el submit ni `role="button"` sobre `<desy-button>`.
- La tabla de la auditoría tiene la columna «Estado» rellena.
- `retro.md` contiene las cuatro secciones con contenido.

## Skills recomendados

- `retro`: retrospectiva del sprint.
- `desy-preflight-check` y `desy-validate-accessibility`: los usan los prompts combinados.

Cierre: `/implement` para ejecutar, `/code-review` antes de la PR, `pr` para la PR única contra main.

## PROMPT

```
Vamos a ejecutar la Fase 5 del Sprint 01 de "desy-skills".

Lee enteros docs/sprints/01/README.md y docs/sprints/01/fase-5-cierre.md, y además:
docs/skill-prompts-collection.md (§2.1 y §2.2) y las fases 1-4 de docs/sprints/01/.

Las decisiones y las costuras de test están cerradas en esos documentos: no se re-preguntan.
Orden: greps → coherencia del texto común → prompts combinados → tabla de la auditoría → /retro. Los prompts combinados se lanzan en sesiones nuevas con docs/mwg-audit-sprint-01 en el checkout principal.
Para los prompts de verificación, abre una sesión nueva e indica la ruta del SKILL.md del worktree (los enlaces de ~/.claude/skills apuntan al checkout principal).

Trabaja en el checkout principal sobre docs/mwg-audit-sprint-01 (sin worktree) y lanza:
/implement docs/sprints/01/fase-5-cierre.md
Las costuras de la fase son los greps y los prompts: ejecútalos antes (rojo) y después (verde). Para /code-review, el punto fijo es main (la PR final se compara con main) y la spec es
docs/sprints/01/fase-5-cierre.md. Atiende sus hallazgos, haz push a fork y abre la PR única contra minilama2222/desy-skills:main con el skill pr.
```
