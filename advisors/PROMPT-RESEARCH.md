# PROMPT-RESEARCH: cómo crear un advisor nuevo

Esta es la **receta** que usé para crear cada uno de los 6 advisors de ejemplo. La idea es spawneear un subagent con WebSearch + WebFetch que devuelve un archivo `.md` completo siguiendo la estructura de `TEMPLATE.md`.

---

## Paso 1: definí el rol y contexto

Antes de spawneear, completá esta ficha:

```
Rol: <CTO | CFO | CMO | CPO | CISO | COO | Head of Sales | etc.>
Producto: <nombre y 1 línea de qué es>
Mercado: <vertical + región>
Stage: <pre-revenue | $X MRR | Series A | etc.>
Equipo: <solo founder | 3 personas | etc.>
Stack relevante: <stack técnico, herramientas actuales>
Restricciones: <presupuesto, tiempo, ética>
3-5 decisiones recientes que necesitaste tomar y dudaste: <lista>
```

Las decisiones que listás al final son **oro** — son los triggers de activación naturales de tu advisor.

---

## Paso 2: prompt base para el subagent

Spawneá un agente con tools `WebSearch + WebFetch + Read + Write` (en Claude Code: `Agent` con `subagent_type: general-purpose` o `gsd-domain-researcher` si tenés el plugin GSD).

Pegale este prompt, reemplazando los `<...>`:

```
Necesito que crees un advisor C-level tipo "memory file" para Claude Code.

CONTEXTO:
- Rol: <CTO/CFO/CMO/etc.>
- Producto: <nombre y descripción>
- Mercado: <vertical + región>
- Stage actual: <stage>
- Equipo: <tamaño>
- Stack: <stack>
- Restricciones: <budget, tiempo>

DECISIONES QUE EL ADVISOR DEBE PODER GUIAR:
1. <Decisión 1>
2. <Decisión 2>
3. <Decisión 3>
4. <Decisión 4>
5. <Decisión 5>

ESTRUCTURA DEL OUTPUT (Markdown):

---
name: <ROL> Advisor — <PRODUCTO>
description: <una línea — usada por Claude para activar el advisor por contexto>
type: reference
---

## Role & Expertise
- Quién es el advisor
- 4 principios no-negociables específicos al stage del producto
- Dominios que cubre

## Key Research Findings
8-10 secciones numeradas. Cada una:
- "Current state" (qué hay hoy o cuál es el problema típico)
- Benchmarks 2025-2026 con NÚMEROS DUROS (no claims sin dato)
- Recommended action específica
- Code/config snippets si aplica (para CTO)
- Cita 3-5 fuentes con link al final de cada sección

## Decision Frameworks
4-6 frameworks accionables tipo matriz/árbol/checklist:
- Cuándo hacer X vs Y
- "Build vs Buy vs Skip"
- Triage matrices
- Priority scoring

## <PRODUCTO>-Specific Recommendations
Priorizadas P1 (esta semana), P2 (este mes), P3 (este trimestre), P4 (cuando llegues a milestone X).
Cada recomendación incluye: acción concreta, costo $, tiempo de implementación, y por qué importa.

## Activation Triggers
Lista bullet de keywords/situaciones que disparan al advisor — usá lenguaje natural ("¿deberíamos deployar esto?" no "deploy_decision_trigger").

## Quick Reference
Tabla/mapa/cheat sheet de lo más consultado.

## Sources
25+ links reales a research, papers, industry reports, vendor docs, benchmarks. Categorizá.

REGLAS NO-NEGOCIABLES:
1. Mínimo 25 sources con links reales (no inventes URLs)
2. Cada claim tiene un número duro (no "muchas empresas hacen X" — "73% de empresas hacen X según [source]")
3. Recomendaciones accionables por una sola persona — sin "contratá a un consultor"
4. Costo y tiempo en cada recomendación
5. Sin platitudes (no "depende de cada caso", no "las mejores prácticas dicen")
6. Tono: práctico, denso, sin marketing
7. Output completo en un solo archivo .md de 400-600 líneas

GUARDA EL ARCHIVO EN: <ruta>/advisor_<rol>.md
```

---

## Paso 3: queries de research por rol

El subagent va a usar WebSearch. Estas son las queries que mejor funcionaron por rol — pasalas si querés sesgar la búsqueda:

### CTO
- "SaaS monitoring solo founder 2026"
- "PostgreSQL multi-tenant tuning best practices"
- "Docker production checklist 2026"
- "Next.js / FastAPI security CVE 2025"
- "Cost-effective SaaS infrastructure $0-5K MRR"
- "Zero-downtime deploy single server"
- "Database backup strategy small SaaS"
- "When to scale beyond single server SaaS"

### CFO
- "SaaS pricing benchmarks 2025-2026"
- "Freemium vs free trial conversion rates"
- "Reverse trial conversion"
- "Referral program metrics benchmarks SaaS"
- "Unit economics SMB SaaS LTV CAC"
- "<TU_MERCADO> SaaS pricing localization"
- "3-tier vs 4-tier pricing conversion"
- "Annual discount benchmark SaaS"

### CMO
- "B2B SaaS landing page conversion 2026"
- "<TU_MERCADO> SMB marketing channel benchmarks"
- "Founder-led LinkedIn playbook B2B"
- "Vertical landing page conversion rates"
- "WhatsApp marketing LATAM benchmarks" (o tu canal/región)
- "SaaS onboarding email drip sequence"
- "Content repurposing solo founder"
- "Referral program design SaaS"

### Product Owner / CPO
- "SaaS feature-tier mapping good-better-best"
- "SMB onboarding time-to-value benchmarks"
- "RICE prioritization SaaS"
- "Feature gating freemium conversion"
- "Customer journey map SMB SaaS"
- "Retention vs acquisition features SaaS"
- "Feature discovery adoption strategies"
- "Multi-product SaaS retention impact"

### Competitive Analyst
- "<TU_VERTICAL> SaaS market size 2025-2026"
- "<COMPETIDOR> pricing features users funding"
- "TAM SAM SOM SaaS calculation methodology"
- "SaaS competitive positioning matrix"
- "Distribution channels <TU_MERCADO> SMB SaaS"
- "Why SMB SaaS fails <TU_REGIÓN>"
- "Competitor monitoring framework"

### CISO (si lo agregás)
- "OWASP top 10 SaaS application"
- "SOC2 / ISO27001 readiness SMB"
- "CVE feed monitoring open source"
- "GDPR / LGPD compliance startup"
- "Pen testing budget SaaS startup"
- "Incident response playbook small team"

### COO (si lo agregás)
- "SaaS operations playbook scale"
- "Customer support metrics SaaS SMB"
- "Vendor management small startup"
- "Hiring framework 1-10 person team"
- "Async vs sync work distributed team"

---

## Paso 4: post-procesado

Cuando el subagent te devuelva el archivo:

1. **Verificá las URLs** — pasale Read a 5-10 links random para confirmar que existen
2. **Sanitizalo** — si vas a compartir el advisor (este repo, p.ej.), quitá data sensible de tu negocio
3. **Probá los triggers** — abrí Claude Code en el proyecto, mencioná uno de los Activation Triggers y confirmá que el advisor se trae al contexto
4. **Iterá** — después de 2-3 semanas usándolo, anotá las decisiones donde el advisor falló o quedó genérico, y pedile al subagent una v2 que cubra esos gaps específicos

---

## Multi-advisor: cuándo activar varios

Cuando una decisión cruza dominios, necesitás varios advisors a la vez. Ejemplos:

| Decisión | Advisors a consultar |
|---|---|
| Cambiar pricing | CFO + Product Owner + Competitive Analyst |
| Rediseñar landing | CMO + Product Owner |
| Nueva feature grande | Product Owner + CTO + Competitive Analyst |
| Estrategia de lanzamiento | CMO + CFO |
| Deploy a prod (high-risk) | CTO (mandatorio) |
| Reportar incidente legal/data | CTO + CISO + CFO |

Esto está formalizado en `example-activation-system.md` — un advisor "master" que le dice a Claude qué advisors traer según el tipo de pregunta.

---

## Tip final

El primer advisor te va a tomar ~30-45 min de research + post-procesado. Después del 2do, ya tenés el flow internalizado y bajás a 15-20 min. **No los hagas todos a la vez** — hacé el más urgente para tu stage (típicamente CFO o CTO en pre-revenue, CMO cuando lanzás, Competitive Analyst cuando empieza a haber ruido en el mercado).
