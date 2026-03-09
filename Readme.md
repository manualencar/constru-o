# 🧠 AI-Powered Dev Workflow — Skills Ecosystem

Um ecossistema de skills para o **Claude Code** que cobre o ciclo completo de desenvolvimento de software: da especificação de produto até a implementação do banco de dados.

Cada skill é focada, reutilizável e encadeada com as demais — produzindo documentos e código prontos para uso, não apenas sugestões genéricas.

---

## 🗺️ Visão Geral do Ecossistema

```
context-checkpoint          ←── consolida múltiplas conversas (entrada opcional)
        │
        ▼
prd-generator               ←── O QUÊ construir (features, fluxos, regras)
        │
        ▼
architecture-spec-builder   ←── COMO construir (módulos, RNFs, decisões)
        │         │
        │         └──→ adr-builder   ←── decisão técnica com trade-offs
        ▼
supabase-data-modeler       ←── estrutura de dados (tabelas, RLS, migrations)
        │
        ▼
supabase-functions-builder  ←── lógica de servidor (🔜 em construção)
```

> **`context-checkpoint`** pode ser acionado a qualquer momento para preservar o estado entre sessões.

---

## 📦 Skills Disponíveis

### [`prd-generator`](./skills/prd-generator/) · *by [Promply](https://promply.lovable.app/)*
Gera PRDs técnicos executáveis a partir de dois blocos de intake. Foco em especificação de funcionalidades, modelo de dados, fluxos de usuário e edge cases — otimizado para times de engenharia implementarem diretamente.

**Quando usar:** início do projeto, especificação de uma nova feature, documentação de requisitos  
**Output:** documento Markdown estruturado com features, fluxos, regras de negócio e edge cases

> Skill desenvolvida pelo time da [Promply](https://promply.lovable.app/). Integrada a este ecossistema como ponto de entrada do ciclo de desenvolvimento.

---

### [`architecture-spec-builder`](./skills/architecture-spec-builder/)
Conduz uma entrevista de contexto e gera especificações de arquitetura completas. Cobre módulos, RNFs mensuráveis, integrações, decisões técnicas (ADRs) e riscos.

**Quando usar:** antes de começar a construir, ao documentar um sistema existente, ao onboarding de novos membros  
**Output:** `.docx` com 17 seções — do sumário executivo ao histórico de revisões  
**Integração:** detecta automaticamente quando uma decisão merece ser aprofundada com o `adr-builder`

---

### [`adr-builder`](./skills/adr-builder/) · *by [Promply](https://promply.lovable.app/)*
Conduz uma entrevista guiada para documentar decisões técnicas com raciocínio completo: contexto, alternativas descartadas, suposições implícitas e gatilho de revisão.

**Quando usar:** qualquer decisão com trade-offs reais, tecnologia nova para o time, ou que possa ser questionada no futuro  
**Output:** ADR no formato padrão, pronto para viver em `docs/decisions/`  
**Princípio:** documenta o *raciocínio*, não apenas o que foi decidido

> Skill desenvolvida pelo time da [Promply](https://promply.lovable.app/). Integrada a este ecossistema como camada de registro de decisões.

---

### [`supabase-data-modeler`](./skills/supabase-data-modeler/)
Modela e implementa a estrutura de dados completa no Supabase. Inclui entrevista de domínio, geração de tabelas com convenções corretas, políticas de RLS por modelo de acesso e migrations SQL organizadas.

**Quando usar:** criar ou evoluir o banco de dados, definir RLS, gerar migrations  
**Output:** migrations `.sql` versionadas + documentação do schema  
**Integração:** aplica diretamente via MCP do Supabase, ou exporta arquivos para download

---

### [`context-checkpoint`](./skills/context-checkpoint/)
Captura o estado de uma conversa ou múltiplas conversas e gera um documento de checkpoint com decisões tomadas, progresso, contexto crítico e prompt de retomada.

**Quando usar:** trocar de sessão, consolidar conversas de diferentes etapas do produto, iniciar uma nova fase  
**Output:** documento `.md` com prompt de retomada pronto para colar na próxima sessão  
**Modo especial:** consolidação de múltiplas conversas — deduplica decisões e detecta conflitos

---

## 🚀 Como Usar

### 1. Instale as skills no Claude Code

Baixe os arquivos `.skill` e instale via Settings → Skills no Claude Code, ou copie as pastas para `~/.claude/skills/`.

### 2. Configure o `CLAUDE.md` do seu projeto

Na raiz do projeto, crie um `CLAUDE.md`:

```markdown
# [Nome do Projeto]

## Stack
- Frontend: [ex: Next.js 14 + shadcn/ui]
- Backend: [ex: Supabase (DB + Auth + Storage)]
- MCP conectado: Supabase

## Convenções
- snake_case no banco, camelCase no código
- Commits: conventional commits

## Skills Instaladas
- `architecture-spec-builder` → spec técnico, módulos, RNFs
- `supabase-data-modeler`     → schema, RLS, migrations via MCP
- `prd-generator`            → features, fluxos, edge cases
- `adr-builder`              → decisões técnicas com trade-offs
- `context-checkpoint`       → preservar contexto entre sessões

## Referências
- Spec: @docs/architecture-spec.md
- Modelo de dados: @docs/data-model.md
- Decisões: @docs/decisions/index.md
```

### 3. Estrutura de diretório recomendada

```
projeto/
├── CLAUDE.md
├── docs/
│   ├── architecture-spec.docx
│   ├── data-model.md
│   ├── prd.md
│   ├── decisions/
│   │   ├── index.md
│   │   ├── ADR-001-nome-da-decisao.md
│   │   └── ADR-002-nome-da-decisao.md
│   └── migrations/
│       ├── 00001_shared_functions.sql
│       ├── 00002_core_schema.sql
│       └── 00003_rls_policies.sql
└── src/
```

---

## 🔄 Fluxo de Uso Típico

```
1. [prd-generator]              Especifica o produto
        ↓
2. [architecture-spec-builder]  Documenta a arquitetura
        ↓ (decisão complexa detectada)
   [adr-builder]                Registra o raciocínio
        ↓
3. [supabase-data-modeler]      Cria o schema + RLS + migrations
        ↓
4. [context-checkpoint]         Preserva o estado para a próxima sessão
```

**Entrada alternativa:** se você tem múltiplas conversas espalhadas sobre o mesmo produto, comece com `context-checkpoint` no modo de consolidação antes de entrar no ciclo principal.

---

## 📋 Conexões entre Skills

| Origem | Destino | O que passa |
|--------|---------|-------------|
| `prd-generator` | `architecture-spec-builder` | Features e requisitos definem os módulos |
| `architecture-spec-builder` | `adr-builder` | Decisões com trade-offs são aprofundadas |
| `architecture-spec-builder` | `supabase-data-modeler` | Seção 7 (modelo de dados) alimenta a modelagem |
| `architecture-spec-builder` | `supabase-data-modeler` | Seção 10 (segurança) define as políticas de RLS |
| qualquer skill | `context-checkpoint` | Estado da sessão é capturado e resumido |

---

## 📁 Estrutura deste Repositório

```
skills-ecosystem/
├── README.md
├── skills/
│   ├── prd-generator/
│   │   └── SKILL.md
│   ├── architecture-spec-builder/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── interview-guide.md
│   │       └── spec-template.md
│   ├── adr-builder/
│   │   └── SKILL.md
│   ├── supabase-data-modeler/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── interview-guide.md
│   │       ├── rls-patterns.md
│   │       └── migration-conventions.md
│   └── context-checkpoint/
│       └── SKILL.md
└── docs/
    └── claude-md-guide.md      ← guia completo de uso do CLAUDE.md
```

---

## 🏷️ Origem das Skills

| Skill | Origem |
|-------|--------|
| `prd-generator` | [Promply](https://promply.lovable.app/) |
| `adr-builder` | [Promply](https://promply.lovable.app/) |
| `architecture-spec-builder` | Construída neste projeto |
| `supabase-data-modeler` | Construída neste projeto |
| `context-checkpoint` | Construída neste projeto |

---

## 🛠️ Próximas Skills

- **`supabase-functions-builder`** — Edge Functions, triggers, RPCs com padrões de segurança e deploy via MCP  
  *(planejada após validação das skills atuais em projeto real)*

---

## 📄 Licença

MIT — use, adapte e contribua à vontade.
