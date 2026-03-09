# CHECKPOINT — Ecossistema de Skills para Desenvolvimento de Software
**Data:** 06/03/2026
**Sessão:** Construção e curadoria do ecossistema de skills

---

## 🎯 Objetivo

Construir um ecossistema de skills encadeadas para cobrir o ciclo completo de desenvolvimento de software — desde a especificação de produto até a implementação do banco de dados — com cada skill sendo reutilizável em projetos futuros, inclusive os mais complexos.

---

## ✅ Decisões Consolidadas

- Skills são **separadas por responsabilidade**, não fundidas — cada uma tem seu momento no fluxo
- A ordem natural do ecossistema é: PRD → Architecture Spec → Supabase Data Model → (futuro) Functions
- O `adr-builder` é uma skill **satélite** que o `architecture-spec-builder` delega quando a decisão tem trade-offs reais
- Skills com entrevista primeiro são preferidas para evitar docs genéricos com "TBDs"
- Output do `architecture-spec-builder` é `.docx`; do `supabase-data-modeler` é `.sql` + aplicação via MCP
- `prd-generator` e `architecture-spec-builder` são **complementares**, não concorrentes
  → Por quê: PRD responde "o quê" (features, fluxos), spec responde "como" (módulos, RNFs, decisões)
- Skills de funções Supabase (Edge Functions, triggers, RPCs) ficam para uma **próxima skill** (`supabase-functions-builder`)
  → Por quê: funções dependem do schema existir — vêm depois da modelagem

---

## 📦 Progresso / Entregas

- ✅ **`architecture-spec-builder.skill`** — criada, empacotada e disponível para download
  - SKILL.md com fluxo em 2 fases (entrevista + geração)
  - `references/interview-guide.md` — 9 blocos de perguntas por domínio
  - `references/spec-template.md` — template completo com 17 seções
  - **Atualizada** com seção "Quando delegar para o adr-builder" (tabela de critérios + sinais de alerta + script de transição)

- ✅ **`supabase-data-modeler.skill`** — criada, empacotada e disponível para download
  - SKILL.md com fluxo em 6 fases (entrevista → modelagem → RLS → migrations → MCP → documentação)
  - `references/interview-guide.md` — 6 blocos: domínio, acesso, atributos, ciclo de vida, integrações, performance
  - `references/rls-patterns.md` — 6 padrões prontos (owner, role, multi-tenant, público, service role, relacionamento)
  - `references/migration-conventions.md` — convenções de nomenclatura, estrutura, templates e sequência MCP

- 📋 **`prd-generator.skill`** — já existia, analisada e posicionada no ecossistema (não modificada)
- 📋 **`adr-builder.skill`** — já existia, analisada e integrada via referência no `architecture-spec-builder`

---

## ⚠️ Contexto Crítico

- O usuário tem **Supabase MCP conectado** — a skill de data modeler deve usar `Supabase:apply_migration` diretamente
- O stack preferido do usuário inclui **Supabase** (DB + Auth + Storage)
- Preferência por skills com **entrevista guiada** antes de gerar qualquer documento
- Complexidade do projeto atual: **média** (alguns módulos integrados)
- O contexto do projeto atual ainda está **superficial** — a entrevista do spec não foi feita

---

## 🔄 Em Andamento / Parcial

- **Projeto real do usuário**: contexto ainda não levantado — a próxima sessão é para usar as skills no projeto
- **`supabase-functions-builder`**: decidida como próxima skill a criar, mas ainda não construída

---

## ➡️ Próximos Passos

1. **Instalar as skills** em Settings → Skills: `architecture-spec-builder.skill` e `supabase-data-modeler.skill`
2. **Iniciar nova sessão** e usar o `architecture-spec-builder` no projeto real (entrevista de contexto)
3. Com o spec pronto, usar o `supabase-data-modeler` para modelar e aplicar o schema via MCP
4. Após validar as duas skills no projeto real, construir a `supabase-functions-builder`

---

## 🚫 Descartado (não recarregar)

- **Fundir prd-generator com architecture-spec-builder**: descartado — propósitos distintos, audiências diferentes
- **Criar skill única de "tudo Supabase"**: descartado — funções vêm depois do schema, skills separadas
- **Ir direto ao spec sem entrevista**: descartado — contexto superficial gera doc genérico

---

## 📋 Prompt de Retomada

> Estou construindo um projeto de complexidade média com Supabase. Já criei um ecossistema de skills: `architecture-spec-builder` (spec técnico completo com entrevista + .docx), `supabase-data-modeler` (tabelas, RLS, migrations + aplicação via MCP), `prd-generator` (features e fluxos) e `adr-builder` (decisões técnicas com trade-offs). O `architecture-spec-builder` já foi atualizado para delegar ao `adr-builder` quando a decisão merece profundidade. As skills estão instaladas.
>
> Próximo passo: usar o `architecture-spec-builder` para levantar o contexto e gerar o spec do meu projeto. O contexto ainda está superficial — preciso fazer a entrevista completa.
>
> Stack: Supabase (DB + Auth + Storage) com MCP conectado.
