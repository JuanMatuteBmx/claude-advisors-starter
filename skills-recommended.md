# Skills recomendadas (de terceros)

**Importante:** ninguna de estas skills es mía. Son de sus creadores originales. Acá te paso los links para que las bajes directamente de la fuente.

---

## UI / UX

### `ui-ux-pro-max` — Sistema de diseño completo
- **Qué hace:** 67 estilos, 96 paletas, 57 pares tipográficos, 25 charts, 13 stacks (React, Next.js, Vue, Svelte, SwiftUI, React Native, Flutter, Tailwind, shadcn/ui, Astro, Nuxt, Jetpack Compose, HTML+Tailwind). Plan, build, design, review, fix, refactor de cualquier UI.
- **Creador:** [nextlevelbuilder.io](https://ui-ux-pro-max-skill.nextlevelbuilder.io/)
- **Cuándo usarla:** diseño de landing pages, dashboards, componentes, paletas, decisiones de tipografía.

---

## Workflow / Project management

### `gsd:*` (Get Shit Done) — 60+ comandos de workflow
- **Qué hace:** `plan-phase`, `execute-phase`, `discuss-phase`, `debug`, `audit-milestone`, `ui-phase`, `ai-integration-phase`, `code-review`, `secure-phase`, `verify-work`, y muchos más. Convierte Claude en un orquestador de proyecto completo con planes, fases, dependencias, auditorías.
- **Buscar como:** "claude code gsd plugin" o "get shit done claude"
- **Cuándo usarla:** proyectos grandes que querés dividir en fases con planning + ejecución + verificación.

### `claude-mem:*` — Memoria persistente cross-session
- **Qué hace:** `mem-search`, `make-plan`, `do`, `knowledge-agent`, `timeline-report`. Mantiene contexto entre sesiones, busca conversaciones anteriores, arma planes basados en lo que ya hiciste.
- **Buscar como:** "claude-mem plugin" o "claude code memory plugin"
- **Cuándo usarla:** trabajo de largo plazo donde necesitás que Claude recuerde decisiones de hace semanas/meses.

---

## Built-in del Claude Code CLI

Estas vienen con Claude Code, no hay que instalarlas:

| Skill | Para qué sirve |
|---|---|
| `init` | Crear CLAUDE.md inicial del proyecto |
| `review` | Review automático de una PR |
| `security-review` | Audit de seguridad de la rama actual |
| `simplify` | Revisar código para reuso, calidad, eficiencia |
| `loop` | Correr un prompt/comando en intervalos recurrentes |
| `schedule` | Crear agentes remotos con cron schedule |
| `update-config` | Configurar settings.json del harness |
| `keybindings-help` | Customizar atajos de teclado |
| `fewer-permission-prompts` | Auto-permitir tools comunes (Bash, MCP) |
| `claude-api` | Build/debug apps que usan la Anthropic API |

---

## Cómo instalar una skill

Las skills viven en una de estas rutas:

```
~/.claude/skills/<skill-name>/SKILL.md          ← global, todos los proyectos
<tu-proyecto>/.claude/skills/<skill-name>/SKILL.md  ← solo este proyecto
```

Para instalar:

1. Bajá la skill del link del creador (suele ser un repo GitHub o sitio del creador)
2. Copiá la carpeta completa a `.claude/skills/`
3. Reiniciá Claude Code (o esperá la próxima sesión)
4. Probala con `/<skill-name>` o dejá que se active por contexto

**Si la skill tiene `scripts/` con código Python u otros assets**, asegurate de copiar toda la carpeta — no solo `SKILL.md`.

---

## ¿Por qué separo skills de advisors?

- **Skills** son herramientas: capacidades reusables que ejecutan acciones (buscar componentes, generar diseños, correr workflows).
- **Advisors** son perspectivas: memorias que se activan por contexto y aplican frameworks de decisión.

Una skill te dice "acá tenés 5 paletas de color". Un advisor te dice "según los benchmarks de SaaS LATAM 2026, deberías ir con 3 tiers de pricing por estas razones".

Ambos se combinan: el CMO advisor te dice qué hacer; `ui-ux-pro-max` te ayuda a construirlo.
