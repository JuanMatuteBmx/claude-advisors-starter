# Instalación

## 1. Dónde van los advisors

Claude Code mantiene memoria persistente por proyecto en:

**Windows:**
```
C:\Users\<tu-usuario>\.claude\projects\<slug-del-proyecto>\memory\
```

**Mac / Linux:**
```
~/.claude/projects/<slug-del-proyecto>/memory/
```

El `<slug-del-proyecto>` es el path absoluto de tu proyecto con `\` y `/` reemplazados por `-`. Ejemplo:

| Tu proyecto | Slug |
|---|---|
| `C:\Users\juan\Documents\miapp` | `C--Users-juan-Documents-miapp` |
| `/Users/juan/dev/miapp` | `-Users-juan-dev-miapp` |

**Tip:** la carpeta `memory/` se crea automáticamente la primera vez que Claude Code guarda algo. Si no existe, creala vos.

---

## 2. Copiar los advisors

Cloná este repo y copiá los archivos de `advisors/example-*.md` a tu carpeta `memory/`:

```bash
git clone https://github.com/JuanMatuteBmx/claude-advisors-starter.git
cd claude-advisors-starter/advisors

# Windows (PowerShell)
$dest = "$env:USERPROFILE\.claude\projects\<TU-SLUG>\memory"
New-Item -ItemType Directory -Force $dest
Copy-Item example-*.md $dest

# Mac/Linux
DEST=~/.claude/projects/<TU-SLUG>/memory
mkdir -p $DEST
cp example-*.md $DEST/
```

**Renombrá** los archivos cuando los pongas en `memory/`:
- `example-cto.md` → `advisor_cto.md`
- `example-cfo.md` → `advisor_cfo.md`
- etc.

---

## 3. Registrarlos en MEMORY.md

Claude solo lee `MEMORY.md` al iniciar. Tenés que agregar una línea por cada advisor para que sepa que existen.

Abrí `<memory>/MEMORY.md` (creá el archivo si no existe) y agregá al final:

```markdown
## C-Level Advisory Team

- [CTO Advisor](advisor_cto.md) — activate for deploys, infra, security, performance
- [CFO Advisor](advisor_cfo.md) — activate for pricing, unit economics, billing
- [CMO Advisor](advisor_cmo.md) — activate for landing, copy, social, conversion
- [Product Owner Advisor](advisor_product_owner.md) — activate for feature prioritization, gating, roadmap
- [Competitive Analyst](advisor_competitive_analyst.md) — activate for competitor moves, positioning
- [Activation System](advisor_activation_system.md) — master orchestrator: when to activate which advisor
```

---

## 4. Adaptar a tu producto

Los ejemplos están sanitizados con placeholders. Editá cada `advisor_*.md` y reemplazá:

| Placeholder | Reemplazar con |
|---|---|
| `<TU_PRODUCTO>` | El nombre de tu producto/empresa |
| `<TU_MERCADO>` | El mercado/región (ej. "B2B SaaS LATAM", "DTC USA") |
| `<TU_STAGE>` | Tu stage (ej. "pre-revenue", "$5K MRR", "Series A") |
| `<TU_STACK>` | Tu stack técnico |
| `<COMPETIDOR_X>` | Tus competidores reales |
| `<MONEDA_BASE>` | Tu moneda principal |

**Búsqueda recomendada:** abrí los 6 archivos en VS Code, `Ctrl+Shift+F`, buscá `<TU_` y reemplazá uno por uno.

---

## 5. Verificar que funcionan

Abrí Claude Code en tu proyecto y probá un trigger:

```
> ¿Cómo debería estructurar mi pricing para llegar a $5K MRR?
```

Claude debería leer `advisor_cfo.md` automáticamente y aplicar sus frameworks (LATAM benchmarks, 3-tier vs 4-tier conversion, reverse trial, etc.).

Si NO lo hace, verificá que:
- El archivo está en la ruta correcta
- `MEMORY.md` tiene la línea con el link al archivo
- El `name:` del frontmatter es descriptivo
- Los `Activation Triggers` cubren el lenguaje natural que estás usando

---

## 6. Crear advisors nuevos

Para roles que no incluí (CISO, COO, CPO, Head of Sales, etc.), seguí [`advisors/PROMPT-RESEARCH.md`](./advisors/PROMPT-RESEARCH.md). Es un prompt que spawneás como subagent y te devuelve un advisor completo con research, frameworks y triggers.

---

## Skills de terceros (opcional)

Si querés las skills externas que uso (UI/UX, GSD, claude-mem, etc.), mirá [`skills-recommended.md`](./skills-recommended.md). **Son de otros creadores, no mías** — los links te llevan al repo/sitio oficial.
