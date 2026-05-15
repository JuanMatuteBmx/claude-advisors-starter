---
name: Advisory Team Activation System
description: Master orchestrator defining when and how to activate each C-level advisor. ALWAYS consult this when processing user requests to ensure the right advisor's perspective is applied.
type: reference
---

> **Ejemplo sanitizado.** Este es el "advisor maestro" que le dice a Claude qué advisor traer al contexto según el tipo de pregunta. Si agregás roles nuevos (CISO, COO, CPO), expandí esta tabla.

## Advisory Team — <TU_PRODUCTO>

You have <N> domain advisors with deep research profiles. Each lives in `memory/advisor_<rol>.md` with frameworks, data, and recommendations.

### Activation Rules

**ALWAYS check if an advisor is relevant before responding to user requests.** If relevant, read the advisor file and incorporate their perspective into your response.

| Advisor | File | Activate When |
|---|---|---|
| **CMO** | `advisor_cmo.md` | Landing pages, copy, social media, content calendar, messaging, positioning, conversion, emails, WhatsApp outreach, blog posts, ads |
| **CTO** | `advisor_cto.md` | Deploy, infrastructure, security, performance, monitoring, backups, scaling, CI/CD, Docker, database, architecture decisions |
| **CFO** | `advisor_cfo.md` | Pricing, plans, tiers, discounts, coupons, referrals, billing, unit economics, revenue projections, cost structure, payment methods |
| **Product Owner** | `advisor_product_owner.md` | Feature prioritization, what goes in which plan, onboarding, user journey, roadmap, feature gating, retention vs acquisition features |
| **Competitive Analyst** | `advisor_competitive_analyst.md` | Competitor mentions, market sizing, positioning decisions, differentiation, pricing comparisons |

> Add rows when you add advisors (CISO, COO, CPO, Head of Sales, Head of Customer Success, etc.)

### Multi-Advisor Topics

Some decisions cross domains. Activate multiple advisors:

| Decision | Advisors |
|---|---|
| **Pricing restructure** | CFO + Product Owner + Competitive Analyst |
| **Landing page redesign** | CMO + Product Owner |
| **New feature decision** | Product Owner + CTO + Competitive Analyst |
| **Launch strategy** | CMO + CFO |
| **Deploy to production** | CTO (mandatory) |
| **Social media content** | CMO + Competitive Analyst |
| **Incident / data breach** | CTO + CFO + (CISO if you have one) |
| **New market entry** | Competitive Analyst + CMO + CFO |
| **Hiring decision** | CFO + (COO if you have one) |

### How to Use

1. Read the relevant advisor file(s) at the start of the relevant conversation
2. Apply their frameworks and data to the current decision
3. If advisors disagree, present BOTH perspectives transparently to the user — don't pick a side silently
4. Always cite specific data points (not vague advice)
5. Reference the framework being applied ("Using Framework 2 from CTO advisor: ...")

### Integration with Workflow Plugins (e.g., GSD)

If you use plugins like `gsd-*` (Get Shit Done) for planning/executing phases, integrate advisors into the workflow:

- **plan-phase:** Consult Product Owner + relevant domain advisor
- **execute-phase:** Consult CTO for any backend/infra changes
- **verify-work:** Consult the advisor whose domain was affected
- **pricing/marketing phases:** CFO + CMO must review before execution
- **ai-integration-phase:** CTO + (CISO if data sensitive)

### When to Update the Advisors

Advisors become stale. Schedule quarterly review:
- **CFO:** Pricing changes from competitors, new payment regulations
- **CTO:** New CVEs, stack version upgrades, infrastructure costs changed
- **CMO:** New channel benchmarks, algorithm changes (LinkedIn, IG)
- **Product Owner:** Activation/retention metrics from your own product
- **Competitive Analyst:** Every quarter or after any major competitor move

To update an advisor: re-run the research prompt from `PROMPT-RESEARCH.md` with the new context. Diff against the old version. Merge the genuine updates, drop outdated claims.

### Failure Modes

**The advisor is being ignored** — Claude responds without referencing frameworks. Fix:
- Sharpen the `Activation Triggers` section with the exact phrases the user uses
- Verify the file is in `memory/` and linked from `MEMORY.md`
- Test by mentioning a trigger keyword explicitly

**The advisor is too generic** — recommendations could apply to any business. Fix:
- Add more `<TU_PRODUCTO>-Specific Recommendations`
- Replace placeholders with your real product context
- Add stage-specific guidance (pre-revenue vs $5K MRR vs $50K MRR)

**Two advisors give contradictory advice** — feature: that's the right behavior. Present both views with the user's decision criteria, don't average them.

**The advisor cites a tool/service that no longer exists** — quarterly review caught it. Update the section.
