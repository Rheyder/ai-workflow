# AI-Workflow v2.0

Workflow agnóstico de ferramenta para conduzir mudanças de software com clareza, qualidade e baixo consumo de contexto.

As skills são baseadas principalmente em [mattpocock/skills](https://github.com/mattpocock/skills), com influência adicional de [obra/superpowers](https://github.com/obra/superpowers).

> **Idioma das skills:** o conteúdo operacional (`SKILL.md`, checklists, templates) permanece em **inglês**. Este arquivo é apenas o guia em português.

Regras transversais: [CONVENTIONS.md](CONVENTIONS.md).

## Core Workflow

**Spec → Plan → Branching → Build → Verify → Review → Simplify → Ship**

Por slice: Branching → Build → Verify → (commit HITL) → repetir. Depois de todos os slices: Review → Simplify (se precisar) → Ship.

| Fase | Skill |
|------|-------|
| Spec | `to-spec` |
| Plan | `to-issues` |
| Branching | `git-workflow-and-versioning` |
| Build | `tdd` |
| Verify | `slice-verification` (por slice) |
| Review | `code-review` |
| Simplify | `code-simplification` |
| Ship | `finishing-a-development-branch` |

## Camadas

| Camada | Papel |
|--------|-------|
| **Core Workflow** | Caminho padrão — uma skill por fase |
| **Review checklists** | Carregadas sob demanda por `code-review` |
| **Toolbox** | Setup, diagnóstico, ensino, handoff, arquitetura, provas de UI, criação de skills |
| **Adapters** | Integrações opcionais — veja [ADAPTERS.md](ADAPTERS.md) |

**Regra:** o Core Workflow deve funcionar **sem** adapters.

## Por onde começar

1. Leia [WORKFLOW.md](WORKFLOW.md) — fases e fast path.
2. Use [DEFINITION_OF_DONE.md](DEFINITION_OF_DONE.md) para saber quando a mudança está pronta.
3. Use [CONTEXT_POLICY.md](CONTEXT_POLICY.md) para limitar o que carregar em cada fase.
4. Siga as skills do Core no caminho padrão; use a Toolbox só quando precisar.

**Bootstrap é opcional.** Use `setup-workflow` apenas quando o repositório alvo não tiver `docs/ai-workflow/` ou documentação neutra do workflow. Repositórios já configurados podem ignorar essa etapa.

## Arquivos principais

| Arquivo | Papel |
|---------|-------|
| [WORKFLOW.md](WORKFLOW.md) | Oito fases, regras |
| [workflow/contracts.md](skills/workflow/contracts.md) | Entrada / ação / saída por fase |
| [workflow/pipeline-reference.md](skills/workflow/pipeline-reference.md) | HITL, tabelas, layout no repo alvo |
| [DEFINITION_OF_DONE.md](DEFINITION_OF_DONE.md) | Checklist de pronto para entrega |
| [CONTEXT_POLICY.md](CONTEXT_POLICY.md) | Contexto mínimo entre fases |
| [CONVENTIONS.md](CONVENTIONS.md) | Evidência, sessão, comunicação concisa |
| [ADAPTERS.md](ADAPTERS.md) | Integrações opcionais |

## Checklists de review

Carregadas **sob demanda** por `code-review` — não fazem parte do caminho padrão:

- [correctness-and-tests.md](skills/code-review/correctness-and-tests.md)
- [maintainability-and-design.md](skills/code-review/maintainability-and-design.md)
- [security-and-configuration.md](skills/code-review/security-and-configuration.md)
- [reliability-and-observability.md](skills/code-review/reliability-and-observability.md)
- [integrations-and-data.md](skills/code-review/integrations-and-data.md)
- [delivery-quality-gates.md](skills/code-review/delivery-quality-gates.md)
- [spec-audition.md](skills/code-review/spec-audition.md)

## Toolbox

Fora do caminho padrão. Use quando o problema exigir:

| Skill | Quando usar |
|-------|-------------|
| `setup-workflow` | Bootstrap opcional de `docs/ai-workflow/` no repo alvo |
| `context-discovery` | Plano vs `CONTEXT.md` / ADRs precisa de validação |
| `diagnose` | Bug difícil, falha de verificação pouco clara |
| `improve-codebase-architecture` | Costuras estruturais, não só legibilidade |
| `handoff` | Estado compacto para a próxima sessão |
| `teach` | Aprendizado guiado no workspace |
| `zoom-out` | Mapear módulo desconhecido |
| `write-a-skill` | Estender este pacote |
| `caveman` | Usuário pede respostas ultra-curtas |
| `frontend-testing` | Evidência de UI no slice ou no ship |

## Instalar as skills

Cada skill é uma **pasta** com `SKILL.md` (frontmatter YAML + corpo). Registre as pastas no seu **agent host**; invoque pelo nome ou pela `description`.

As skills descrevem **resultados** (ler arquivos, rodar testes), não uma IDE específica. Comandos do repositório vêm do **projeto alvo**.

## Exemplo de jornada

Feature *exportar relatório como CSV* (planejamento em `.scratch/export-csv/`):

| Etapa | Skill | Resultado |
|-------|-------|-----------|
| Spec | `to-spec` | Spec curta no local de planejamento configurado |
| Plan | `to-issues` | Tarefas em slices com critérios de aceite |
| Branch | `git-workflow-and-versioning` | `feature/export-csv` |
| Build + verify | `tdd` → `slice-verification` | RED→GREEN + evidência |
| Review | `code-review` | Relatório do diff |
| Ship | `finishing-a-development-branch` | Verificação da branch; resumo de entrega |

Detalhes: [workflow/pipeline-reference.md](skills/workflow/pipeline-reference.md).

## Filosofia

- **Simples antes de completo** — carregue só a fase necessária.
- **Verificável antes de opinião** — evidência antes de afirmações.
- **Agnóstico antes de acoplado** — qualquer host ou humano pode seguir a documentação.
- **Menos contexto antes de mais** — [CONTEXT_POLICY.md](CONTEXT_POLICY.md).

## Estender

[write-a-skill/SKILL.md](skills/write-a-skill/SKILL.md). Opcionalmente, rode `setup-workflow` de novo para atualizar `docs/ai-workflow/` após mudanças no pacote.

## Versão em inglês

Documentação canônica do pacote: [README.md](README.md).
