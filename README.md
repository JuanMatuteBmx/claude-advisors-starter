# Claude Advisors Starter

Un sistema de **C-level advisors** (CTO, CFO, CMO, Product Owner, Competitive Analyst) para Claude Code, construido como memorias persistentes que se activan por contexto.

> **Qué es esto:** la metodología que usé para crear un "equipo virtual de C-levels" que asesora cada decisión técnica, de pricing, de marketing y de producto en mi SaaS. Cada advisor tiene research real con benchmarks 2025-2026, frameworks de decisión, y triggers de activación.
>
> **Qué NO es:** no son skills propias mías. Las skills que uso (`ui-ux-pro-max`, `gsd`, `claude-mem`, etc.) son de otros creadores — abajo te paso los links para que las bajes vos mismo.

---

## ¿Para quién es esto?

- Founders/devs que usan **Claude Code** y quieren que Claude tome mejores decisiones, no solo escriba código
- Equipos chicos sin CTO/CMO/CFO real que necesitan "voces expertas" en momentos clave
- Quien quiera entender cómo funcionan las **memorias tipo `reference`** de Claude y cómo activarlas por contexto

---

## Cómo está organizado el repo

```
claude-advisors-starter/
├── README.md                    ← estás acá
├── INSTALL.md                   ← cómo instalarlo en tu .claude/
├── skills-recommended.md        ← skills de terceros que uso (con links a los creadores)
├── advisors/
│   ├── TEMPLATE.md              ← plantilla vacía para crear un advisor nuevo
│   ├── PROMPT-RESEARCH.md       ← el prompt-receta para spawneear research de un advisor
│   ├── example-cto.md           ← ejemplo CTO (sanitizado, basado en un SaaS LATAM real)
│   ├── example-cfo.md           ← ejemplo CFO
│   ├── example-cmo.md           ← ejemplo CMO
│   ├── example-product-owner.md ← ejemplo Product Owner
│   ├── example-competitive-analyst.md ← ejemplo Competitive Analyst
│   └── example-activation-system.md   ← orquestador: cuándo activar cuál
└── LICENSE
```

---

## ¿Cómo funcionan los advisors?

Cada advisor es un **archivo Markdown con frontmatter** que vive en la carpeta `memory/` del proyecto Claude Code. Son del tipo `reference` — Claude los lee cuando el contexto de la conversación coincide con los triggers de activación del advisor.

**Estructura canónica de un advisor:**

```markdown
---
name: <ROL> Advisor — <PRODUCTO>
description: <una línea sobre cuándo activar>
type: reference
---

## Role & Expertise
[Quién es, contexto, principios no-negociables, dominios]

## Key Research Findings
[8-10 secciones con benchmarks 2025-2026, números duros, sources]

## Decision Frameworks
[4-6 matrices/árboles de decisión accionables]

## <Producto>-Specific Recommendations
[Priorizadas P1/P2/P3]

## Activation Triggers
[Lista de keywords/situaciones que disparan el rol]

## Sources
[Links reales a la research]
```

**Activación:** Claude lee `MEMORY.md` en cada sesión. Cuando ve un trigger del advisor (ej. "deploy", "pricing", "landing page"), trae el archivo completo al contexto y aplica los frameworks.

---

## Quick start

1. Leé [`INSTALL.md`](./INSTALL.md) para meter los advisors en tu `.claude/projects/<tu-proyecto>/memory/`
2. Adaptá los advisors de ejemplo a tu producto (reemplazá los placeholders `<TU_PRODUCTO>`, `<TU_MERCADO>`, etc.)
3. Para crear advisors nuevos, seguí [`advisors/PROMPT-RESEARCH.md`](./advisors/PROMPT-RESEARCH.md)
4. Para skills externas que uso, mirá [`skills-recommended.md`](./skills-recommended.md)

---

## Créditos

Esta metodología y los advisors de ejemplo los creé yo ([@JuanMatuteBmx](https://github.com/JuanMatuteBmx)) construyendo Murett, un SaaS para PyMEs LATAM. Las skills que recomiendo en `skills-recommended.md` son de sus creadores originales — yo solo las uso.

Si te sirve, dale una ⭐ y compartilo con otros founders.

## Licencia

MIT — usalo, modificalo, sharealo.
