# Manual completo: cómo crear advisors especializados para tu proyecto

Este es el manual end-to-end. Si solo querés el prompt para spawneear research, anda a [`advisors/PROMPT-RESEARCH.md`](./advisors/PROMPT-RESEARCH.md). Si querés entender todo el proceso —desde decidir qué advisor necesitás hasta mantenerlo vivo seis meses después— leelo todo.

---

## Tabla de contenidos

1. [Antes de empezar: ¿qué es realmente un advisor?](#1-antes-de-empezar-qué-es-realmente-un-advisor)
2. [Cuándo crear un advisor (y cuándo NO)](#2-cuándo-crear-un-advisor-y-cuándo-no)
3. [Cómo elegir qué advisor necesitás](#3-cómo-elegir-qué-advisor-necesitás)
4. [Las 4 fases de creación](#4-las-4-fases-de-creación)
5. [Fase 1 — Definir el rol](#fase-1--definir-el-rol)
6. [Fase 2 — Investigación con subagent](#fase-2--investigación-con-subagent)
7. [Fase 3 — Post-procesado y sanitización](#fase-3--post-procesado-y-sanitización)
8. [Fase 4 — Activación y testing](#fase-4--activación-y-testing)
9. [Patrones avanzados](#9-patrones-avanzados)
10. [Mantenimiento y revisión](#10-mantenimiento-y-revisión)
11. [Errores comunes y cómo evitarlos](#11-errores-comunes-y-cómo-evitarlos)
12. [Catálogo de roles posibles](#12-catálogo-de-roles-posibles)

---

## 1. Antes de empezar: ¿qué es realmente un advisor?

Un advisor **no es un agente que ejecuta tareas**. Es una **memoria con perspectiva** que Claude trae al contexto cuando detecta que la conversación toca el dominio del advisor.

| Concepto | Qué hace | Ejemplo |
|---|---|---|
| **Skill** | Ejecuta una acción reusable | "Bajame 5 paletas de color" |
| **Subagent (Agent tool)** | Spawnea un proceso paralelo con tools | "Investigá esto en internet y devolveme un md" |
| **Advisor (memory `reference`)** | Aplica un punto de vista a la decisión actual | "Como CFO, tu pricing tiene este problema" |

La magia del advisor es que **se activa por contexto**, no por comando. No tenés que decir `/cfo` — Claude detecta "pricing", "tiers", "discount" y trae el archivo solo.

**Anatomía de un advisor que funciona:**

```
1. Frontmatter (name + description + type: reference) — Claude usa esto para decidir cuándo activarlo
2. Role & Expertise — quién es, qué dominios cubre, principios no-negociables
3. Key Research Findings — 8-10 secciones con NÚMEROS DUROS 2025-2026 (no opiniones)
4. Decision Frameworks — 4-6 matrices/árboles accionables
5. Product-Specific Recommendations — priorizadas P1/P2/P3/P4
6. Activation Triggers — palabras y situaciones que disparan el advisor
7. Sources — 20-30 links reales a research
```

Si falta alguna de estas 7 piezas, el advisor se siente "genérico" y Claude lo ignora.

---

## 2. Cuándo crear un advisor (y cuándo NO)

### Señales de que NECESITÁS un advisor nuevo

- Repetidamente le pedís a Claude el mismo tipo de decisión (pricing, deploy, copy) y cada vez te responde distinto
- En conversaciones reales tenés que pegar el mismo contexto largo ("recordá que estoy en LATAM, pre-revenue, solo founder...")
- Estás tomando una decisión recurrente sin un framework claro
- Hay un dominio donde sabés que no sabés (te falta un experto)
- Empezaste a notar que Claude te da consejos genéricos tipo "depende de cada caso"

### Señales de que NO necesitás un advisor

- La decisión es one-off (no volverá a aparecer)
- El dominio es tan específico de tu producto que no hay research público
- Ya tenés un advisor que cubre el 80% del tema (mejor expandilo)
- Es algo táctico, no estratégico (Claude no necesita un advisor para "armá una función Python")
- Tu producto cambió tanto que un advisor de hace 6 meses está obsoleto — actualizalo, no creés uno nuevo

### Regla del pulgar

> **Si tomaste la misma decisión tipo 3 veces y te quedaste con dudas, es momento de un advisor.**

---

## 3. Cómo elegir qué advisor necesitás

No todos los proyectos necesitan los mismos C-levels. Antes de copiar los 6 ejemplos, hacé este ejercicio:

### Ejercicio: las 5 preguntas

Listá las **5 decisiones más caras** (en plata, tiempo, o riesgo) que tomaste en los últimos 60 días. Por cada una:

1. ¿Qué dominio era? (técnico / financiero / marketing / producto / legal / operativo / etc.)
2. ¿Tenías un framework claro para decidir? (Sí / No / A medias)
3. ¿Te arrepentís de la decisión? (Sí / No / Aún no lo sé)
4. ¿Volverá a aparecer esta decisión? (Sí / No / Variante de esto)

Los dominios donde respondiste "No" a (2) Y "Sí" a (4) son tus **prioridades de advisor**.

### Mapping típico por stage del producto

| Stage | Advisors priorizados | Por qué |
|---|---|---|
| **Pre-build / discovery** | Competitive Analyst, Product Owner | Decidir QUÉ construir antes de escribir código |
| **Build / pre-launch** | CTO, Product Owner | Decisiones de arquitectura, qué features priorizar |
| **Launch** | CMO, CFO | Posicionamiento, pricing, primeros clientes |
| **Growth (0-50 clientes)** | CMO, CFO, Product Owner | Conversión, retention, qué features mover la aguja |
| **Scale (50-500 clientes)** | CTO, COO, Head of Customer Success | Infra, operaciones, retention sistemático |
| **Mature (500+)** | CFO, Head of Sales, CISO, Head of People | Unit economics, sales playbook, compliance, hiring |

### Mapping por tipo de proyecto

| Proyecto | Advisors clave |
|---|---|
| **SaaS B2B SMB** | CTO, CFO, CMO, Product Owner, Competitive Analyst (los 5 base) |
| **SaaS B2B Enterprise** | + Head of Sales, CISO, Head of Customer Success |
| **DTC / e-commerce** | + Head of Performance Marketing, COO (fulfillment), CFO (CAC/LTV) |
| **Marketplace** | + Head of Supply, Head of Demand, Trust & Safety |
| **Fintech** | + CISO, Compliance/Legal, CFO (mucho más complejo) |
| **Mobile app / consumer** | + Growth Lead (loops), Head of UA, Data Analyst |
| **AI/ML product** | + Head of ML/AI, Data Engineer, Head of Eval |
| **Open source / dev tool** | + DevRel, Head of Community |
| **Agencia / servicios** | + COO, Head of Delivery, Head of Sales |

### Anti-patrón: el "consejo de superhéroes"

No creés 12 advisors al mismo tiempo. **Empezá con 2-3** que cubren tus decisiones más caras. Agregá más a medida que aparecen los gaps. Un sistema de advisors con 3 perspectivas afiladas le gana siempre a uno con 12 perspectivas genéricas.

---

## 4. Las 4 fases de creación

```
┌─────────────────┐     ┌──────────────────┐     ┌────────────────┐     ┌───────────────┐
│  Fase 1         │     │  Fase 2          │     │  Fase 3        │     │  Fase 4       │
│  Definir el rol │ →   │  Investigación   │ →   │  Post-proceso  │ →   │  Activación   │
│  (15-30 min)    │     │  con subagent    │     │  + sanitización│     │  + testing    │
│                 │     │  (20-40 min)     │     │  (15-30 min)   │     │  (10-15 min)  │
└─────────────────┘     └──────────────────┘     └────────────────┘     └───────────────┘

Total: ~1-2 horas el primero. Los siguientes ~30-45 min cada uno.
```

---

## Fase 1 — Definir el rol

Antes de spawneear nada, completá esta ficha. **Si no podés completarla, el advisor va a salir genérico.**

```markdown
## Ficha del Advisor

### Rol
<CTO | CFO | CMO | CPO | CISO | COO | Head of X | etc.>

### Producto / contexto
- Nombre: <tu producto>
- Una línea: <qué hace, para quién>
- Vertical / mercado: <ej. B2B SaaS para PyMEs LATAM>
- Stage: <pre-revenue | $X MRR | Series A>
- Equipo: <solo founder | 3 personas | etc.>
- Stack relevante: <para CTO; para CFO sería "modelo de monetización", etc.>

### Restricciones reales
- Presupuesto: <tope mensual, lo que sea>
- Tiempo: <horas/semana disponibles>
- Ética / valores: <ej. "no vendemos data", "open source", etc.>

### Decisiones que el advisor debe poder guiar
1. <Decisión recurrente 1, lo más específico posible>
2. <Decisión recurrente 2>
3. <Decisión recurrente 3>
4. <Decisión recurrente 4>
5. <Decisión recurrente 5>

### Lo que YA sabés del dominio (para que el advisor no te repita lo obvio)
- <Asunción que tenés validada>
- <Decisión que ya tomaste y no querés revisitar>
- <Restricción dura que no negociás>

### Lo que NO querés del advisor
- <Tipo de advice que no te sirve, ej. "no me digas que contrate a alguien">
- <Tono que rechazás, ej. "no quiero formato consultor con MBA jargon">
```

**Las "Decisiones que debe poder guiar" son lo más importante.** Son los Activation Triggers naturales del advisor. Si tu lista es genérica ("decisiones técnicas"), el advisor va a salir genérico. Si es específica ("¿deberíamos migrar de Vercel a Cloudflare?", "¿cuándo agregar PgBouncer?"), el advisor va a tener filo.

---

## Fase 2 — Investigación con subagent

### Opción A: Spawneás el subagent con el prompt-receta

Usá la herramienta `Agent` en Claude Code con `subagent_type: general-purpose`. Tools que necesita: `WebSearch + WebFetch + Read + Write`.

Pegá el prompt completo de [`advisors/PROMPT-RESEARCH.md`](./advisors/PROMPT-RESEARCH.md) **reemplazando todos los `<...>`** con los datos de tu ficha.

### Opción B: Lo hacés en una conversación interactiva

Si preferís ir refinando, abrí una conversación con Claude y empezá con algo así:

```
Necesito que actúes como investigador externo. Vas a construir un advisor C-level
tipo "memory file" para Claude Code. NO actúes todavía como el advisor — primero
investigá, después armás el archivo.

[Pegás la ficha de Fase 1]

Plan:
1. Hacé WebSearch de los benchmarks/data más importantes para este rol en 2025-2026.
   Quiero números duros, no opiniones.
2. Hacé WebFetch de las 5 fuentes más confiables.
3. Identificá 4-6 frameworks de decisión que se usan en este rol.
4. Listá 8-10 secciones de research que el advisor debería cubrir.
5. Pasame ese plan ANTES de escribir el .md. Yo te confirmo o ajusto.
```

Esto te da control de qué research se hace antes de que el subagent escriba 500 líneas que vas a tener que tirar.

### Queries de research por rol (cheat sheet)

Si el subagent no sabe por dónde empezar, dale estas queries como punto de partida:

#### Roles ya cubiertos en el repo
Mirá las listas en [`advisors/PROMPT-RESEARCH.md`](./advisors/PROMPT-RESEARCH.md) — sección "Queries de research por rol".

#### Roles que NO están en el repo (catalogo expandido)

**CISO / Security Lead**
- "OWASP top 10 SaaS application 2026"
- "SOC2 readiness solo founder cost"
- "ISO 27001 small startup minimum requirements"
- "CVE monitoring open source dependencies"
- "GDPR / LGPD / CCPA compliance checklist startup"
- "Pen testing budget early-stage SaaS"
- "Incident response playbook small team"
- "Bug bounty program when to launch"

**COO / Head of Operations**
- "SaaS operations playbook scale 1-50 customers"
- "Customer support SLA SMB SaaS benchmarks"
- "Vendor management startup minimum"
- "Hiring framework 1-10 person team"
- "Async vs sync work distributed team"
- "Cash conversion cycle SaaS"
- "Operational runbook templates"

**Head of Sales**
- "B2B SaaS sales playbook SMB <$30K ACV"
- "Sales cycle benchmarks by ACV 2025-2026"
- "Discovery questions BANT MEDDIC SMB"
- "Cold outbound conversion rates by channel"
- "Sales objection handling SaaS"
- "When to hire first AE vs founder-led sales"
- "Commission structure first sales hire"

**Head of Customer Success**
- "SaaS onboarding completion rate benchmarks"
- "Customer health score model SMB"
- "Churn prediction signals SaaS"
- "QBR vs check-in cadence by ACV"
- "Expansion revenue playbook"
- "Customer success ratios CSM:ARR"

**Head of People / HR**
- "Early-stage startup hiring framework"
- "Compensation benchmarks <50 employees"
- "Equity allocation early hires"
- "Remote / async culture playbooks"
- "Performance review framework Latticeesque"
- "Founder-CEO transition signals"

**Data / Analytics Lead**
- "SaaS metrics framework north star metric"
- "Event taxonomy schema first 90 days"
- "Mixpanel / Amplitude / PostHog comparison SMB"
- "Cohort analysis methodology SaaS"
- "Attribution model multi-touch"
- "Data warehouse small team Snowflake vs BigQuery vs DuckDB"

**Head of AI / ML**
- "RAG vs fine-tuning when to use"
- "Eval methodology LLM applications"
- "AI cost per query benchmarks"
- "Model selection framework GPT / Claude / open source"
- "Production LLM monitoring (LangSmith, Helicone, Langfuse)"
- "AI safety guardrails small team"

**Legal / Compliance**
- "SaaS terms of service template generator"
- "Privacy policy GDPR LGPD SaaS"
- "Data Processing Agreement (DPA) when required"
- "B2B contract negotiation playbook small startup"
- "IP assignment founder agreements"
- "International tax SaaS multi-country"

**DevRel / Community**
- "Developer marketing playbook open source"
- "Community building stages 0-1K-10K"
- "Documentation strategy SaaS technical"
- "DevRel metrics activation engagement"
- "Conference sponsorship ROI early-stage"

**Performance Marketing / Growth**
- "Paid acquisition CAC by channel SaaS 2026"
- "Loops and viral mechanics consumer / B2B"
- "Attribution model first-touch vs last-touch vs MTA"
- "Experiment design statistical power"
- "Growth model spreadsheet templates"
- "When to hire first growth marketer vs founder"

---

## Fase 3 — Post-procesado y sanitización

El subagent te devuelve un `.md`. **No lo uses directo.** Hacé esto:

### Checklist de calidad

- [ ] **Frontmatter completo:** `name`, `description`, `type: reference`
- [ ] **8-10 Key Research Findings** (si tiene 4, está flojo; si tiene 15, está aguado)
- [ ] **Cada sección tiene NÚMEROS** — si encontrás "muchas empresas hacen X", reemplazalo o eliminalo
- [ ] **4-6 Decision Frameworks** accionables (matrices, árboles, checklists)
- [ ] **Recomendaciones priorizadas** P1/P2/P3/P4 con costo + tiempo
- [ ] **Activation Triggers** usan lenguaje natural (tus frases reales, no jargon)
- [ ] **20-30 sources** con links reales. Verificá 5 al azar con WebFetch
- [ ] **Tono específico al stage** — pre-revenue ≠ Series B
- [ ] **No hay platitudes** ("depende de cada caso", "las mejores prácticas dicen")

### Sanitización (si vas a compartir el advisor)

Si vas a hacer este advisor público en un repo (como hicimos acá), reemplazá:

| Tipo de info | Cómo sanitizar |
|---|---|
| Nombre del producto | `<TU_PRODUCTO>` |
| Nombres de clientes con nombre | `Customer A`, `Customer B` |
| MRR/ARR específico | rangos (`$0-5K MRR`) o quitalo |
| Precios reales tuyos | mantené como ejemplo pero marcalo "example pricing" |
| URLs propias | `yourapp.com` |
| Decisiones internas estratégicas | quitalas o generalizalas |
| Stack técnico específico si es propietario | generalizá |
| Métricas de churn/retention tuyas | benchmarks de industria, no tuyos |

Lo que **SÍ podés dejar**: frameworks de decisión, research público con sources, benchmarks de industria, metodología.

### Iteración con vos en el loop

Después de la primera versión:

1. Leelo entero. Si te aburrís a la mitad, está mal — el formato denso con tablas funciona mejor que párrafos largos
2. Marcá las 3 secciones que **más te van a servir**. Si no podés marcarlas, el advisor está aguado
3. Marcá las 3 secciones que **no te sirven**. Eliminalas o pediles que las reescriban con foco en tu stage
4. Pedí una v2 con esos ajustes específicos

---

## Fase 4 — Activación y testing

### Instalación

```bash
# Path en Windows
C:\Users\<tu-usuario>\.claude\projects\<slug-del-proyecto>\memory\advisor_<rol>.md

# Path en Mac/Linux
~/.claude/projects/<slug-del-proyecto>/memory/advisor_<rol>.md
```

El `<slug-del-proyecto>` es el path absoluto con `\` y `/` reemplazados por `-`. Ejemplo:
- Proyecto en `C:\Users\juan\dev\miapp` → slug: `C--Users-juan-dev-miapp`

### Registrar en MEMORY.md

Editá el archivo `MEMORY.md` en la misma carpeta `memory/` y agregá:

```markdown
## C-Level Advisory Team

- [<Rol> Advisor](advisor_<rol>.md) — when to activate <breve descripción>
```

Sin esta línea, Claude no sabe que el archivo existe.

### Test de activación

Cerrá Claude Code y abrilo de nuevo (o iniciá conversación nueva). Probá los triggers:

**Test 1 — Trigger directo:**
```
> [Mencionás una de las "Decisiones que el advisor debe poder guiar"]
```
Claude debería mencionar el advisor o aplicar sus frameworks. Si responde genérico, falló.

**Test 2 — Trigger oblicuo:**
```
> [Usás una frase parecida a un Activation Trigger pero no idéntica]
```
Si Claude activa el advisor, los triggers están bien calibrados. Si no, agregá la frase a la lista de triggers.

**Test 3 — Caso real:**
Esperá a la próxima decisión real del dominio. Ves cómo responde Claude. Anotá:
- ¿Activó el advisor solo? (sin que vos lo pidas)
- ¿Aplicó un framework específico del advisor o respondió genérico?
- ¿Los números que cita son los del advisor o se los inventó?

### Si el advisor no se activa

Debugging en orden:

1. **¿El archivo está en `memory/`?** Path correcto, sin typos.
2. **¿Está linkeado desde `MEMORY.md`?** Sin link, no se carga.
3. **¿El `name:` y `description:` del frontmatter son descriptivos?** "Reference doc" no activa nada. "CFO Advisor — pricing, unit economics, billing" sí.
4. **¿Los Activation Triggers cubren el lenguaje real que usás?** Vas a tener que ajustarlos a tus frases reales.
5. **¿Estás en el proyecto correcto?** Cada proyecto tiene su propia carpeta `memory/`. Un advisor en proyecto A no funciona en proyecto B.

### Si el advisor se activa pero da advice genérico

- La sección "<Producto>-Specific Recommendations" está vacía o con placeholders. Llenala con tus específicos.
- Los frameworks son demasiado abstractos. Hacelos más concretos con ejemplos de tu producto.
- Falta contexto de stage. Agregá "Para tu stage actual (<X MRR>): ...".

---

## 9. Patrones avanzados

### Patrón 1: Multi-advisor orchestration

Cuando una decisión cruza dominios, querés que Claude active varios advisors en orden y combine sus perspectivas. Esto lo manejás desde el `example-activation-system.md` (advisor maestro).

Ejemplo: "¿deberíamos lanzar este feature?"
- Product Owner: ¿encaja en el roadmap, qué tier?
- CTO: ¿qué cuesta construirlo, qué riesgo de infra?
- CMO: ¿se puede vender, qué historia?
- Competitive Analyst: ¿alguien ya lo tiene?
- CFO: ¿qué impacto en ARPU/CAC?

El advisor maestro le dice a Claude el orden y cómo presentar perspectivas contradictorias (NO promediarlas — presentarlas).

### Patrón 2: Sub-advisors / advisors especializados

Si un dominio se vuelve muy grande, podés tener un advisor general + sub-advisors. Ejemplo CTO:

```
advisor_cto.md                          ← general
advisor_cto_database.md                 ← especializado en DB
advisor_cto_security.md                 ← especializado en security
advisor_cto_infra.md                    ← especializado en infra/devops
```

El general tiene Activation Triggers amplios. Los sub-advisors tienen triggers específicos. Claude carga el sub-advisor solo cuando la pregunta es específica.

**Cuándo hacerlo:** si tu advisor general supera ~600 líneas, probablemente es momento de split.

### Patrón 3: Time-decay flags

Algunos advisors decaen rápido (Competitive Analyst, performance marketing). Agregá metadata de decay:

```markdown
---
name: Competitive Intelligence Analyst — <PRODUCTO>
description: ...
type: reference
last_research: 2026-05-15
decay_warning_after: 90_days
review_trigger: any major competitor announcement
---
```

Y al principio del archivo:

```markdown
> **⚠️ Last research: 2026-05-15.** Competitive data decays fast. Re-run research
> if any of these are true: 90 days passed, competitor launched new product,
> competitor changed pricing, new entrant funded > $5M.
```

Claude va a mencionar el warning cuando active el advisor, y te recuerda updatear.

### Patrón 4: Stage-aware advisors

Tu CFO advisor pre-revenue no sirve cuando estás en Series A. Dos opciones:

**Opción A — versionar:**
- `advisor_cfo_pre_revenue.md`
- `advisor_cfo_seed.md`
- `advisor_cfo_series_a.md`

Updateás el activation system cuando cambias de stage.

**Opción B — secciones por stage en el mismo archivo:**

```markdown
## Recommendations by Stage

### Pre-revenue ($0 MRR)
1. ...

### Early ($1-10K MRR)
1. ...

### Growth ($10-100K MRR)
1. ...
```

Y los triggers incluyen el stage actual ("estamos pre-revenue", "ya tenemos $5K MRR").

### Patrón 5: External voice advisors

A veces no querés tu propia perspectiva sino la de un experto/escuela específica. Ejemplo:

- `advisor_dhh_style.md` — perspectiva DHH (Rails / monolitos / boring tech)
- `advisor_jason_lemkin_style.md` — perspectiva SaaStr / Jason Lemkin pricing
- `advisor_paul_graham_style.md` — perspectiva PG / YC framework

Estos los llenás citando sus posts/charlas más relevantes en lugar de research industry-wide. Útil para tener "asesores honoris causa" disponibles cuando dudes.

### Patrón 6: Counterfactual advisor (devil's advocate)

Un advisor que **siempre busca razones para NO hacer algo**. Útil contra el confirmation bias del founder.

```markdown
---
name: Devil's Advocate Advisor
description: Activate before any major commitment (hire, big build, pricing change, market entry). Argues the opposing case from data.
type: reference
---

## Mandate
Your job is to find the strongest reason this decision is wrong. Not to be
contrarian — to be the strongest opposing case the user would face from a
skeptical investor / co-founder / mentor.

For every decision, produce:
1. The "obvious" reason this works (1 sentence, to acknowledge)
2. Three reasons it might fail, ranked by likelihood
3. The "kill criteria" — what data would prove this wrong in 30/60/90 days
4. The cheap version of this decision (smaller commitment, faster learning)
```

Útil pre-commit a decisiones grandes (contratar, raise, pivot).

---

## 10. Mantenimiento y revisión

### Cadencia de revisión por tipo de advisor

| Advisor | Cadencia | Trigger extra |
|---|---|---|
| Competitive Analyst | Trimestral | Cualquier movimiento de competidor |
| CFO (pricing) | Trimestral | Cambio de pricing tuyo o de competidor |
| CMO | Trimestral | Algoritmo cambia (LinkedIn, IG), nuevo canal |
| CTO | Semestral | Major version de tu stack, CVE crítico |
| Product Owner | Trimestral | Nueva métrica de activación/retention |
| CISO | Trimestral | Nuevo CVE crítico, regulación nueva |
| COO | Anual | Estructura del equipo cambia |
| Activation System | Cuando agregás/quitás advisor | — |

### Cómo revisar (sin reescribir entero)

1. Leelo entero. Markeá con `[STALE]` lo que ya no aplica.
2. Para cada `[STALE]`, decidí: actualizar, eliminar, o dejar como nota histórica.
3. Re-corré el research solo en las secciones que actualizaste. No reescribas todo.
4. Verificá las URLs de sources (las páginas de pricing de competidores cambian a menudo).
5. Updateá la fecha en el footer.

### Versionado

Mantené un `CHANGELOG` al final del advisor:

```markdown
## Changelog

- 2026-08-15: Updated competitor pricing (Alegra raised Pyme from $36 → $41)
- 2026-05-15: Added section on AI-assisted commerce
- 2026-03-29: Initial version
```

Esto te ayuda a detectar drift y entender por qué un consejo viejo tuyo era distinto.

---

## 11. Errores comunes y cómo evitarlos

### Error 1: Advisor demasiado genérico
**Síntoma:** "depende de cada caso", "las mejores prácticas dicen"
**Causa:** poco contexto en la ficha de Fase 1
**Fix:** rellená "Decisiones que debe poder guiar" con frases reales tuyas, no abstractas

### Error 2: Advisor con research falso
**Síntoma:** cita stats sin link, links a páginas inexistentes
**Causa:** subagent inventó URLs
**Fix:** verificá 5-10 links al azar con WebFetch antes de aprobar. Pedile al subagent que **solo cite sources que ya leyó con WebFetch** (no que infiera)

### Error 3: Advisor nunca se activa
**Síntoma:** Claude responde sin mencionar frameworks
**Causa:** triggers genéricos o no linkeado desde MEMORY.md
**Fix:** triggers en lenguaje natural ("¿deberíamos deployar esto?" no "deploy_decision_trigger"); verificá `MEMORY.md`

### Error 4: Advisor obsoleto que confunde
**Síntoma:** Claude cita un servicio que cerró, una herramienta que ya no usás, un precio viejo
**Causa:** sin cadencia de revisión
**Fix:** revisión trimestral programada (literal, en calendario); agregá time-decay flags

### Error 5: Demasiados advisors
**Síntoma:** Claude no sabe cuál activar, contradicciones constantes
**Causa:** 12 advisors creados antes de tener 3 funcionando
**Fix:** archivá los que no usaste en 60 días; quedate con 3-5 que realmente activás

### Error 6: Advisors que se contradicen sin orquestación
**Síntoma:** CFO dice "subí precios", CMO dice "estás caro, bajá"
**Causa:** sin activation system maestro
**Fix:** el activation system les dice cómo combinar perspectivas — siempre presentar ambas, nunca promediar

### Error 7: Advisor que es solo opiniones del founder
**Síntoma:** todo es lo que vos ya pensabas
**Causa:** no spawneaste research, lo escribiste solo
**Fix:** el valor del advisor está en la research externa. Si querés tu opinión documentada, llamalo "founder_principles.md", no advisor

### Error 8: Advisor que duplica MEMORY.md
**Síntoma:** mismo contenido en `MEMORY.md` y en advisor
**Causa:** confundir memoria operativa con advisor de referencia
**Fix:** `MEMORY.md` es para state actual del proyecto (qué está pasando hoy). Los advisors son frameworks atemporales.

### Error 9: Advisor sin Activation Triggers
**Síntoma:** Claude nunca lo carga solo
**Causa:** olvidaste la sección de triggers
**Fix:** agregá triggers EXACTAMENTE con las frases que vos usás (revisá tus últimas 5 conversaciones)

### Error 10: Compartir advisor sin sanitizar
**Síntoma:** competidores leen tus números, MRR, clientes
**Causa:** entusiasmo
**Fix:** sanitización checklist de Fase 3 antes de subir a repo público

---

## 12. Catálogo de roles posibles

Más allá de los 6 incluidos, estos son roles que han funcionado bien como advisors en distintos tipos de proyecto:

### Roles ejecutivos
- **CEO advisor** — Strategy, vision, fundraising decisions, board management
- **CFO advisor** ✓ (en repo)
- **COO advisor** — Operations, vendor mgmt, hiring framework, async culture
- **CTO advisor** ✓ (en repo)
- **CMO advisor** ✓ (en repo)
- **CPO / Product Owner advisor** ✓ (en repo)
- **CISO advisor** — Security posture, compliance, incident response
- **Chief of Staff advisor** — Founder time management, prioritization meta-framework

### Heads
- **Head of Sales** — Outbound, pipeline, sales hire decision
- **Head of Customer Success** — Onboarding, retention, expansion
- **Head of Performance Marketing** — Paid acquisition, CAC, attribution
- **Head of Content / SEO** — Content strategy, keyword targeting, link building
- **Head of Community / DevRel** — Community building, developer marketing
- **Head of Data / Analytics** — Metrics framework, event taxonomy, experiments
- **Head of People / HR** — Hiring, comp, equity, performance reviews
- **Head of AI / ML** — Model selection, evals, AI cost optimization

### Especializados
- **Legal / Compliance advisor** — Contracts, IP, international compliance
- **Tax advisor** — International tax structure, R&D credits
- **Investor relations advisor** — Fundraising narrative, board prep
- **PR / Communications advisor** — Press strategy, crisis comms
- **Brand advisor** — Positioning, naming, visual identity decisions
- **UX research advisor** — User interview frameworks, survey design
- **Pricing strategist** — Specific advisor solo para pricing si es central a tu negocio
- **M&A advisor** — Cuando empezás a evaluar compras o ser comprado

### Counterfactuals / éticos
- **Devil's Advocate** — Siempre argumenta el caso opuesto (ver Patrón 6)
- **Ethics / Trust & Safety advisor** — Particularmente para AI, marketplaces, fintech
- **Stakeholder voice advisor** — Una sola perspectiva: ¿qué diría tu cliente más enojado? ¿tu inversor más exigente?

### Por escuela / mentor
- **DHH / Rails-style advisor** — Boring tech, monolitos, anti-microservices
- **YC / Paul Graham advisor** — Frameworks YC (talk to users, do things that don't scale)
- **SaaStr / Jason Lemkin advisor** — SaaS pricing, sales playbooks
- **Brian Chesky / design-led advisor** — Design as competitive advantage
- **Geoffrey Moore / Crossing the Chasm advisor** — Tech adoption lifecycle decisions

---

## Cierre

Crear advisors es un proceso de **iteración lenta**. El primero te toma 1-2 horas y va a salir mediocre. El tercero te toma 30 minutos y va a ser útil de verdad. El quinto ya tenés un sistema de toma de decisiones que vale más que la mayoría de los consejos consultores que podrías pagar.

**Tres principios que me sirvieron:**

1. **Mejor 3 advisors filosos que 12 genéricos.** Si no estás usando un advisor en 60 días, archivalo.
2. **El valor está en los Decision Frameworks**, no en los Research Findings. La data sirve para sostener los frameworks. Los frameworks son los que actúan.
3. **Los advisors no piensan por vos.** Te dan estructura y data. La decisión sigue siendo tuya — y tu intuición + sus frameworks te van a llevar más lejos que cualquiera de los dos sueltos.

Si construís uno bueno, compartilo. PRs welcome al repo.

— Juan
