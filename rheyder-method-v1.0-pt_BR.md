# Rheyder method — workflow de entrega assistida por IA (v1.0 — materialização fundida)

Método para organizar trabalho com agentes e skills: triagem por risco → trilhas com etapas, artefatos nomeados e papéis canônicos. **Princípios transversais** (definição única em **Conceitos centrais** e **Interface-design e slices verticais**): contratos respeitados; fatias verticais; concisão (`caveman`).

**Este documento** é o baseline normativo **v1.0** (trilhas, convenções estendidas, templates) **alinhado** ao `.cursor/` deste checkout (skills, agents, rules). **Última revisão:** 2026-05-04.

**Público-alvo:** desenvolvedor solo (modo default deste repo) com agentes em IDE; equipe entra apenas via revisão de PR no GitHub.

**Pré-requisitos:** skills locais; `git worktree` sob demanda; harness/CI quando a trilha exigir.

**Manutenção:** ao criar ou remover artefatos em `.cursor/` ou novo `.md`/`.mdc` de método, atualize **esta** página e [.cursor/README.md](../README.md).

**Implementação incremental:** ao materializar nova slug, manter **autocontenção** sob `.cursor/` e o critério **skill mínima viável** em [Skills e agentes / Roadmap](#roadmap-de-materialização). **Derivar** novo conteúdo a partir das bases upstream **mattpocock/skills** e **obra/superpowers** (identificadores de repositório — URLs em [Apêndice B — Bases metodológicas](#apêndice-b--bases-metodológicas)); fundir sempre no texto publicado sob `.cursor/`. **Autoria de `SKILL.md` (writing-skills + write-a-skill):** cópia canónica em **`.cursor/reference/skill-authoring/`** e consolidação pelo agente **`skill-writer`** (ver [Catálogo](#catálogo)). Para execução pelo agente, a referência são os ficheiros em `.cursor/` (proibido usar só “ler § deste método” como passo único — ver [Autocontenção](#autocontenção-da-materialização-cursor)). **Candidatas adiadas** de rules (ex.: `interface-design-slices.mdc`) permanecem válidas até promoção — tabela em [Rules `.mdc`](#rules-mdc).

A seção **00** é triagem; as seções **1–4** mapeiam 1:1 às trilhas **Simple**, **Normal**, **Complex** e **Hotfix**. Tudo que é norma transversal está em **Conceitos centrais**, **Interface-design e slices verticais** ou **Convenções de execução** — a redação é única ali e referenciada nas trilhas.

## Em 30 segundos

- **Padrão de execução:** ver [**Interface-design e slices verticais**](#interface-design-e-slices-verticais) e [**Concisão (`caveman`)**](#concisão-caveman).
- Escolha **uma** trilha em `00 — Triagem`; monitore sinais de trilha errada durante a execução.
- Materialize artefatos com **cabeçalho YAML padrão** (`status`, `version`, `date`).
- Use `task-orchestrator` pra coordenar slices e critérios de parada; **`workflow-harness`** (skill dedicada ainda não materializada neste repo — ver Catálogo) ou comandos repetíveis documentados no `plan-*`/README: aplicar sempre o princípio `verification-before-completion`.
- Dentro da fatia **antes de avançar:** **duas revisões intra-slice** (`slice-spec-reviewer` → `slice-quality-reviewer`, ver agentes Cursor) + **`code-review`** na redação aplicável; **passe final** antes de entrega mantém uso de **`code-review`** e materializa **`validation-*`**. Cumpra **DoD** da trilha e do slice.
- **Commit e push são sempre HITL do usuário** (Convenções/Commit e entrega) — agente anuncia e aguarda.
- Cumpra a política de `knowledge-*` por trilha; equipe revisa o **PR no GitHub** — gate externo final.

## Modo solo (default deste repo)

Você acumula os papéis `task-orchestrator`, `developer` e `reviewer`. Eles existem como **roles de método** disparados como **skills, rules e `.cursor/agents/*`**. Neste checkout, **`developer`** mapeia para o subagent **`slice-developer`**; as funções **`reviewer` intra-slice** mapeiam para **`slice-spec-reviewer`** (alinhamento plano/spec) e **`slice-quality-reviewer`** (qualidade + evidência); o **orquestrador** é **`task-orchestrator`**. **Autoria/refino de skills Cursor:** use o agente **`skill-writer`**, que aponta para **`.cursor/reference/skill-authoring/`** (pacotes writing-skills + write-a-skill); a materialização aplicável pelo agente fica sempre sob `.cursor/`.

- **HITL = você decidindo conscientemente.** Pontos canônicos: **todo commit/push** (ver Convenções/Commit e entrega), estouros de Ralph, gates entre fases em Complex, sobreposição de área entre branches paralelos.
- **Único revisor externo é o time, no PR no GitHub.** Trate `validation-*` como **autorrevisão** antes de abrir o PR.
- Tudo que o método chama de "barreira de processo" (gates, escopo de revisão, DoD) vira **disciplina sua**, não trâmite entre pessoas.
- Quando o texto disser "agente X" ou "papel Y", entenda como **prompt/skill que você dispara**, com saída clara. A separação é mental — não tente performar duas pessoas no mesmo passe.

## Índice

- [Conceitos centrais](#conceitos-centrais)
- [Interface-design e slices verticais](#interface-design-e-slices-verticais)
- [00 — Triagem](#00--triagem)
- [1 — Simple](#1--simple)
- [2 — Normal](#2--normal)
- [3 — Complex](#3--complex)
- [4 — Hotfix](#4--hotfix)
- [Convenções de execução](#convenções-de-execução)
- [Templates](#templates)
- [Skills e agentes](#skills-e-agentes)
  - [Autocontenção da materialização Cursor](#autocontenção-da-materialização-cursor)
- [Pacote workflow-pack e symlink](#pacote-workflow-pack-e-symlink-em-projetos-consumidores)
- [Apêndice A — Inventário vs derivação](#apêndice-a--inventário-vs-derivação)
- [Apêndice B — Bases metodológicas](#apêndice-b--bases-metodológicas)

**Ordem de leitura recomendada:** Modo solo → Triagem → Conceitos centrais → trilha escolhida → Convenções/Templates conforme precisar.

---

## Conceitos centrais


| Termo                | Significado                                                                                                                                                                                                                   |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **HITL**             | *Human-in-the-loop*. Solo: pausa explícita onde você **escolhe** algo que o agente não deveria escolher sozinho. Pontos canônicos listados em **Modo solo**; commit/push detalhado em **Convenções/Commit e entrega**.        |
| **Slice vertical**   | Fatia fina de entrega ponta a ponta, ligada a uma porção nomeada do interface-design (não "só uma camada").                                                                                                                   |
| **Interface-design** | Contratos e fronteiras já acordados (API, tipos, schemas, UI, ADR, OpenAPI, Storybook, seções de spec/plan) que o código deve respeitar. Pode ser **bullets de contrato mínimo** quando ainda não houver desenho estruturado. |
| **Ralph loop**       | Ciclo iterativo do agente até critério de parada (testes, fatia verde, limite de tentativas). Tem **teto duro** — ver Convenções/Loop.                                                                                        |
| **Gate**             | Barreira obrigatória antes de avançar (revisão entre slices, gate de segurança, fim de fase em Complex).                                                                                                                      |
| **Harness**          | Camada repetível ao redor da execução (CI, smoke, testes, eval) com critérios de parada explícitos. Não decide escopo nem substitui HITL.                                                                                     |
| **Slug**             | Identificador estável em `kebab-case` para skills, agentes e referências.                                                                                                                                                     |


### Tags em tabelas


| Tag       | Significado                                                                 |
| --------- | --------------------------------------------------------------------------- |
| `[skill]` | Instrução reutilizável (checklist, formato, critério). Não executa sozinha. |
| `[agent]` | Papel que executa com ferramentas; carrega skills.                          |


---

## Interface-design e slices verticais

Regra global única do método — **prioridade sobre réplicas divergentes** em `.mdc` ou em skills até promoção deliberada. Para **humanos** lendo o método, esta seção é a fonte; para **agentes** operando só com artefatos Cursor, cada skill, rule ou agente afetado deve **incorporar no próprio ficheiro sob `.cursor/`** o extrato necessário (o que verificar, o que é proibido, o que nomear no `plan-*`) — ver [Autocontenção da materialização Cursor](#autocontenção-da-materialização-cursor).

Marcador `**REQUIRED:**` numa skill significa obrigação de fluxo **descrita na própria skill** (ou na rule injetada), não “abrir o método como único passo”. Citação opcional ao slug ou âncora deste doc é lembrete pra pessoa, não substituto de extrato em `.cursor`.

**Interface-design — regra:** todo código produzido ou alterado deve respeitar o **interface-design vigente** (contratos e fronteiras já acordados em spec/plan/ADR/OpenAPI/Storybook/UI/tokens, ou em **bullets de contrato mínimo** quando ainda não houver desenho estruturado).

- **Proibido:** implementar contratos novos "de cabeça" que discordem do desenho ou antecipem desenho ainda não materializado.
- **Se o desenho precisar mudar:** atualizar **primeiro** (ou em lockstep verificável) o artefato — alinhado a **Spec drift** (ver Convenções/Artefatos).

**Slices verticais — regra:** planejamento e execução são **sempre** organizados em fatias verticais — incremento ponta a ponta traceável a uma porção definida do interface-design. Plano "só infra" / "só camada" sem contrato de valor entregável **quebra** a regra.

**No `plan-`*, cada slice deve nomear:**

- (a) qual trecho do interface-design ela cobre (link/seção, ou bullets de contrato mínimo);
- (b) critério de valor ponta a ponta (DoD do slice — ver Templates).

**Na revisão (entre slices e final):** confronte diff/código contra o `plan-`* em (i) interface-design citado e (ii) corte vertical. Divergência material vira spec drift, não é aprovada silenciosamente.

**Atalhos (Simple Express, Hotfix trivial)** não dispensam a regra: o patch deve ficar aderente ao desenho existente; se não houver desenho escrito, registrar contrato mínimo em bullets antes de expandir superfície pública.

---

## 00 — Triagem

**Pergunta decisória rápida:**

- Incidente em produção / degradação ativa? → **Hotfix**
- Sem incidente: vários sistemas ou alto custo de erro? → **Complex**
- Sem incidente: precisa de spec formal pra fechar escopo? → **Normal**
- Caso contrário (mudança localizada, baixo risco) → **Simple**

**Check de segurança (independente da trilha):** a mudança toca authN/authZ, crypto, PII ou dado sensível?
→ Se sim: gate explícito de revisão de segurança antes do merge (skill `security-review` ou checklist no `plan-`*). Registrar no `plan-`*.


| Trilha      | Quando usar                                                                                           |
| ----------- | ----------------------------------------------------------------------------------------------------- |
| **Simple**  | Pouca ambiguidade, mudança localizada, baixo risco; sem spec formal.                                  |
| **Normal**  | Feature-sized; escopo claro após especificação; rastreabilidade com artefatos padrão.                 |
| **Complex** | Múltiplos sistemas, decisões de arquitetura, alto custo de erro; discovery/spike e gates entre fases. |
| **Hotfix**  | Produção quebrada / incidente; correção mínima com registro pós-fix.                                  |


**Distinção explícita:** **Hotfix** prioriza contenção/restauração de incidente; **Simple** prioriza entrega rápida sem incidente.

### Sinais de trilha errada (checar durante execução)

→ Reclassificar para **Complex** se:

- Apareceram 2+ sistemas não mapeados na triagem.
- O plano precisou ser reescrito após início da implementação.
- Surgiu risco de dados, segurança ou contrato público não antecipado.
- `plan-`* passou de 7 slices.
- Brainstorming gerou 3+ perguntas sem resposta clara.

→ Reclassificar para **Normal** se:

- A **Simple** gerou um `spec-`* formal (escopo cresceu).
- O **Hotfix** exigiu refactor além da correção mínima.

→ Reclassificar para **Simple/Hotfix** (descendente) se:

- O discovery do **Complex** reduziu o risco a um único slice controlado.

---

## 1 — Simple

**Objetivo:** entregar rápido com barreira mínima.

### Modos: Express vs Assistida


| Modo          | Critério de entrada (todos)                                                           | Etapas                                                                                                  |
| ------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Express**   | Uma área · sem contrato público alterado · sem migração · validação rápida disponível | Brainstorming → `developer` → anúncio pro usuário → **HITL: commit/push** (Convenções/Commit e entrega) |
| **Assistida** | Qualquer critério Express falha                                                       | Fluxo completo de 6 etapas abaixo                                                                       |


Se um critério Express falhar durante a execução, migrar para Assistida sem negociação.

**Validação rápida disponível (critério Simple Express):** existe pelo menos **um** dentre — linter do projeto, testes automatizados focados no que mudou, smoke documentado no `plan-`* ou README — **executável localmente em ≤ 2 minutos** após a mudança e rodado **nesta sessão** antes de declarar o slice pronto. Conferência manual de UI só vale com **passos reproduzíveis** (1–2 linhas no artefato). Se o repo ainda não tiver comando assim, **não usar Express** até o `plan-`* listar um comando mínimo (pode ser slice dedicado).

### Etapas — Assistida


| #   | Etapa          | Skill / Agente                                                                      | Artefato                        |
| --- | -------------- | ----------------------------------------------------------------------------------- | ------------------------------- |
| 01  | Brainstorming  | `brainstorming`                                                                     | bullets no chat                 |
| 02  | Plano          | `plan-writing`                                                                      | `plan-YYYY-MM-DD-name.md`       |
| 03  | Implementação  | `task-orchestrator` (+ `developer`, `tdd-cycle`, `workflow-harness`)                | —                               |
| 04  | Revisão (diff) | `reviewer` passe final (carrega `code-review`; escopo: diff + dependências diretas) | `validation-YYYY-MM-DD-name.md` |
| 05  | Entrega        | `task-delivery`                                                                     | `changelog-YYYY-MM-DD-name.md`  |
| 06  | Aprendizado    | `knowledge-base`                                                                    | `knowledge-`* (opcional)        |


**Notas:**

- O `plan-`*, mesmo simples, segue a regra global de **slices verticais** + **interface-design** por fatia.
- Worktree apenas se houver outra iniciativa ativa em paralelo (ver Convenções/Branches e worktrees).
- Ralph com teto padrão; harness peso Simple — ver Convenções/Loop e orquestração.

---

## 2 — Normal

**Objetivo:** especificar e planejar antes de codificar em fatias; revisão sobre a superfície total da mudança no branch.


| #   | Etapa            | Skill / Agente                                                                  | Artefato                        |
| --- | ---------------- | ------------------------------------------------------------------------------- | ------------------------------- |
| 01  | Brainstorming    | `brainstorming`                                                                 | `spec-YYYY-MM-DD-name.md`       |
| 02  | Plano            | `plan-writing`                                                                  | `plan-YYYY-MM-DD-name.md`       |
| 03  | Implementação    | `task-orchestrator` (+ `developer`, `tdd-cycle`, `workflow-harness`)            | —                               |
| 04  | Revisão (branch) | `reviewer` passe final (carrega `code-review`; escopo: branch vs alvo de merge) | `validation-YYYY-MM-DD-name.md` |
| 05  | Entrega          | `task-delivery`                                                                 | `changelog-YYYY-MM-DD-name.md`  |
| 06  | Aprendizado      | `knowledge-base`                                                                | `knowledge-`* (recomendado)     |


**Notas:**

- `plan-`* materializa slices verticais + interface-design por fatia antes de codificar escopo maior (regra global).
- Rollback no `plan-`* quando aplicável (estrutura: Templates/Rollback).
- Harness peso Normal — ver Convenções/Loop e orquestração.

---

## 3 — Complex

**Objetivo:** reduzir incerteza antes de codificar; gates entre fases sensíveis.

**Spike puro:** discovery pode terminar como `spec-YYYY-MM-DD-name-spike.md` sem código se a decisão for "não construir".


| #   | Etapa                                    | Skill / Agente                                                                       | Artefato                                 |
| --- | ---------------------------------------- | ------------------------------------------------------------------------------------ | ---------------------------------------- |
| 01  | Discovery / spike                        | `technical-discovery`                                                                | curto; pode embutir em `plan-`*/`spec-`* |
| 02  | Brainstorming + spec hardening           | `brainstorming` (+ NFR, limites, rollback)                                           | `spec-YYYY-MM-DD-name.md`                |
| 03  | Plano em fases (com decisões / ADR-lite) | `plan-writing`                                                                       | `plan-YYYY-MM-DD-name.md`                |
| 04  | Implementação                            | `task-orchestrator` (+ `developer`, `tdd-cycle`, `workflow-harness` **obrigatório**) | —                                        |
| 05  | Revisão (branch)                         | `reviewer` passe final (carrega `code-review`)                                       | `validation-YYYY-MM-DD-name.md`          |
| 06  | Entrega                                  | `task-delivery`                                                                      | `changelog-YYYY-MM-DD-name.md`           |
| 07  | Aprendizado                              | `knowledge-base`                                                                     | `knowledge-`* (**obrigatório**)          |


**Notas:**

- **Spec hardening** (parte do passo 02) sai quando: (a) NFRs definidos ou marcados N/A; (b) limites e restrições documentados; (c) rollback registrado **ou** decisão explícita de que não se aplica (estrutura: Templates/Rollback).
- **Decisões / ADR-lite** vivem como seção do `plan-`* (ou ADR separado quando o repo tiver convenção).
- **Pausas HITL** entre fases sensíveis quando o `plan-`* definir (além das HITL canônicas: Convenções/Commit e entrega).
- Mesmos artefatos do Normal — diferença é **rigor + gates + pausas HITL**, não novos tipos de arquivo.

---

## 4 — Hotfix

**Objetivo:** restaurar / conter produção; sem refactor "bônus".


| #   | Etapa                | Skill / Agente                                              | Artefato                        |
| --- | -------------------- | ----------------------------------------------------------- | ------------------------------- |
| 01  | Brainstorming rápido | `brainstorming`                                             | bullets                         |
| 02  | Debug sistemático    | `systematic-debugging`                                      | —                               |
| 03  | Implementação        | `task-orchestrator` (loop enxuto; trivial = só `developer`) | —                               |
| 04  | Revisão (diff focal) | `reviewer` passe final (carrega `code-review`)              | `validation-YYYY-MM-DD-name.md` |
| 05  | Entrega              | `task-delivery`                                             | `changelog-YYYY-MM-DD-name.md`  |
| 06  | Aprendizado          | `knowledge-base` (opcional)                                 | ver post-mortem em Templates    |


**Notas:**

- **Em relação ao Normal:** omite `spec-`* longa e `plan-`* estruturado — bullets + debug.
- **Não confundir** com pular o passo 01 (esclarecimento mínimo continua).
- **Rollback** mínimo em `plan-`* ou `changelog-`* quando houver plano escrito.
- **Incidente sério** → variante `knowledge-YYYY-MM-DD-name-postmortem.md` (Templates/Post-mortem).
- Harness mínimo amarrado ao sintoma (reprodução + regressão/smoke) — ver Convenções/Loop e orquestração.

---

## Convenções de execução

### Artefatos

**Nome:** `tipo-YYYY-MM-DD-name.md`. **Tipos:** `spec-`*, `plan-`*, `validation-*`, `changelog-*`, `knowledge-*`. Várias iniciativas no mesmo dia exigem `name` discriminativo. Revisões no mesmo dia: `spec-2026-05-01-payment-v2.md`.

**Cabeçalho YAML padrão:**

```yaml
---
status: draft | approved | superseded
version: v1
date: YYYY-MM-DD
---
```

Artefato substituído → marcar anterior como `status: superseded` (não deletar).

**Spec drift:** se a implementação divergir da `spec-`* de forma **material** antes do merge: registrar a divergência no `plan-`* **ou** atualizar a `spec-`* para nova versão antes do merge. Material = mudança de escopo, contrato público, interface-design, risco, rollback, restrição técnica ou comportamento observável.

**Regra de merge:** não fazer merge com `spec-`* conscientemente desatualizada sem anotação explícita no `plan-`*.

**Dados sensíveis:** não registrar segredos em artefatos; mascarar tokens/credenciais em evidências; para incidentes, prefira link a evidência oficial em vez de dump bruto.

### Branches e worktrees

**Branch naming:** `<trilha>/<slug-do-artefato>` (ex.: `normal/2026-05-01-payment-refactor`). O slug deve casar com o nome dos artefatos.

**Worktree — quando criar (qualquer um basta):**

- Há outra iniciativa ativa no mesmo repo.
- Trilha é **Complex**.
- **Hotfix** rodando em paralelo a Normal/Complex em andamento.

Senão: branch direto, sem worktree.

**Branches paralelos com sobreposição de área:**

- Detectar antes: `git diff main...branch-a -- <path>` vs `git diff main...branch-b -- <path>`.
- Sobreposição → ordem de merge explícita no `plan-`* de ambos; segundo rebaseia sobre o primeiro após merge.
- Sobreposição em contrato público / segurança → HITL antes de qualquer implementação avançar.

### Commit e entrega (HITL obrigatório)

**Regra canônica única:** **commit e push são sempre feitos pelo usuário.** O agente nunca executa `git commit` / `git push` por iniciativa própria — independente de trilha, modo ou tamanho do slice.

**Fluxo:**

1. Slice (ou entrega final) atinge o **DoD** correspondente — incluindo passe de revisão verde e `verification-before-completion` cumprido.
2. O agente **anuncia explicitamente** ao usuário: *slice pronto pra commit*, listando (a) arquivos tocados, (b) sumário do que mudou (1-3 bullets), (c) mensagem de commit sugerida, (d) próximo slice ou status.
3. O usuário decide:
  - **(a) commita por conta própria** — o agente segue pro próximo slice ou aguarda nova instrução;
  - **(b) instrui o agente** a executar (`stage + commit + push`) — só então `task-delivery` roda os comandos de Git.

`**task-delivery` no fluxo solo:** **prepara** o commit (stage seletivo, sumário, mensagem sugerida, atualização do `changelog-`*) e **anuncia**. Só executa Git quando o usuário pedir explicitamente nessa interação. **Não há commit implícito** ao final de um slice.

**Anúncio pós-slice — formato mínimo:**

```markdown
**Slice N pronto pra commit.**
- Arquivos: `<path1>`, `<path2>` ...
- Mudança: <1-3 bullets>
- Mensagem sugerida: `<tipo>(<escopo>): <descrição curta>`
- Próximo: <slice N+1 / aguardando decisão / DoD da trilha cumprido — pronto pra push e PR>

Commitar você ou quer que eu execute?
```

**Variantes:**

- **Slice intermediário** → anúncio acima; sem push (a menos que o `plan-*` defina push por slice).
- **Entrega final (DoD da trilha cumprido)** → anúncio inclui `validation-*` materializado, status do CI/harness e proposta de abertura de PR. Push e abertura de PR também são HITL.
- **Hotfix** → mesmo fluxo; anúncio pode ser mais curto, mas **commit continua HITL**.

**Por que canônico:** evita commit silencioso por agente, preserva o `git push` como ponto natural de HITL no modo solo (ver Conceitos centrais / HITL), e mantém autoria/auditoria sob controle do usuário.

### Revisão

**Reviewer (método) em dois momentos — materialização Cursor neste repo:**

| Momento | Escopo | Quando | Artefato |
| ------- | ------ | ------ | -------- |
| **Intra-slice (gate duplo)** | (1) Plano/spec/slice ↔ código — agent **`slice-spec-reviewer`** + skill **`code-review`** conforme rubrica; (2) Qualidade + evidência — **`slice-quality-reviewer`** + **`code-review`**. Ordem **obrigatória** no orquestrador: spec antes de quality. | Gate antes de encerrar a rodada/slice corrente (fluxo numerado em [.cursor/agents/task-orchestrator.md](../agents/task-orchestrator.md)) | `validation-*` típico só no **passe final**; intra-slice: registo no chat ou `plan-*` |
| **Passe final (branch / entrega)** | Contexto geral: diff focal em Simple/Hotfix; superfície completa do branch em Normal/Complex — skill **`code-review`**, materializa **`validation-*`** | Antes de `task-delivery` | **Materializa `validation-*`** |

Não são duas pessoas — são passes distintos que no IDE podem ser subagents/prompts separados ou disciplina mental no mesmo usuário.

**Variante PR sem autoria (revisão de PR de outro):** apenas o passe final, com diff completo; `validation-`* referencia o PR e a interface mínima tocada se não houver plano.

**Rubrica code-review (checklist binário):**

- Implementação aderente ao interface-design citado no `plan-`*/slice (sem contratos "surpresa")
- Estrutura alinhada aos slices verticais do `plan-`* (sem entrega só em camada)
- Nenhum método/função acima de 20 linhas sem comentário de justificativa
- Zero segredo hardcoded (verificável com grep ou secret-scanner)
- Cada classe tem responsabilidade única identificável pelo nome
- **Testes / cobertura:** se o repo expõe métrica no **CI do alvo de merge**, cobertura não regrediu vs essa baseline nesta evidência; senão, se existe comando local de cobertura, não regrediu vs valor registrado no `validation-`* ou slice anterior **desta** iniciativa (primeira vez: registrar “antes” uma vez); se **não** há métrica de cobertura — testes existentes do comportamento alterado passam e caminhos críticos novos têm teste **ou** justificativa explícita no `plan-`*/PR (marcar N/A com lista do que rodou)
- Nenhum `TODO` novo sem issue/ticket vinculado
- Sem anti-padrões conhecidos da **stack e do estilo deste repo** (guia/ADR/README de convenções, se houver); se não houver guia — checklist mínima: sem N+1 óbvio nos caminhos alterados, sem misturar camadas de forma inconsistente com o projeto, sem ignorar padrão de erro/transação da stack
- Contratos públicos / APIs externas não quebrados sem versionamento

**Definition of Done por trilha:**


| Trilha      | DoD                                                                                    |
| ----------- | -------------------------------------------------------------------------------------- |
| **Simple**  | Rubrica verde + CI ok + `changelog-`*                                                  |
| **Normal**  | Rubrica verde + CI ok + `validation-`* aprovado + PR aberto                            |
| **Complex** | Rubrica verde + CI ok + `validation-`* aprovado + gates HITL concluídos + PR aprovado  |
| **Hotfix**  | Rubrica verde (diff focal) + regressão/smoke passando + `changelog-`* ou PR com aceite |


Em todas, o DoD pressupõe cumprimento de **interface-design e slices verticais** quando houver plano explícito.

**DoD do slice** (por fatia no `plan-`*):

Slice concluído quando:

- é **vertical** (ponta a ponta, com critério de valor/contrato);
- código respeita o interface-design referenciado (ou divergência tratada via Spec drift);
- objetivo entregue sem expandir escopo;
- testes do slice passaram + regressão local relevante;
- pendências viraram novo slice / risco / decisão pendente;
- sem divergência material não registrada vs `spec-`*/`plan-`*;
- **anúncio "slice pronto pra commit" entregue ao usuário** no formato canônico (ver Convenções/Commit e entrega).

Próximo slice só inicia quando: (a) o anterior está verde **e** o usuário decidiu sobre o commit (commitou ou pediu pro agente commitar ou explicitamente liberou pra seguir sem commit), **ou** (b) o anterior está explicitamente `blocked`.

**Princípio transversal (`verification-before-completion`):** nenhum item da rubrica, do DoD da trilha ou do DoD do slice pode ser marcado verde sem **evidência fresca produzida agora** — não "deve passar", não "passou na run anterior". Aplica-se também ao gate de Commit e entrega e antes de declarar fix de Hotfix aplicado. Ver skill em **Adoção direta**.

### Loop e orquestração

**Teto Ralph:**

- Máximo **3 tentativas por slice** antes de HITL obrigatório.
- **Tentativa (Ralph) = uma rodada dos passos do orquestrador até qualidade aceite (implementação `slice-developer` + `tdd-cycle` onde couber → `slice-spec-reviewer` → `slice-quality-reviewer`)**, conforme [task-orchestrator.md](../agents/task-orchestrator.md). Ciclos abortados só por tooling/ambiente **não contam** — corrigir o ambiente e seguir.
- O `plan-`* pode sobrescrever o teto com justificativa explícita.
- Ao acionar HITL, registrar em `validation-`* ou `plan-*`: tentativa atual; hipótese de bloqueio; decisão; próximo passo autorizado.

**Critério:** se a 3ª tentativa não produziu avanço verificável, o próximo passo não é "tentar de novo" — é escalar (recortar escopo, mudar abordagem, pedir ajuda).

**Critérios de parada:** definir **antes** do loop critérios mensuráveis (testes verdes, tempo máximo, "done" do slice). O gate de revisão entre slices inclui a checagem obrigatória plano ↔ código.

**Trivial vs orquestrado:**

Trivial quando **todos**:

- Muda área localizada (sem dependência transversal relevante).
- Não altera contrato público fora do que o interface-design já define.
- Sem migração de dados, infra ou segurança sensível.
- Validação rápida disponível (regressão ou smoke).

Falhou um → fluxo orquestrado da trilha (loop / slices / gates).

**Harness — peso por trilha** (skill **`workflow-harness`** quando existir `.cursor/skills/workflow-harness/SKILL.md` *ou* procedimento equivalente documentado):

- **Simple/Hotfix** — só o necessário pra provar o patch (reprodução + teste/regressão ou smoke).
- **Normal** — matriz de verificação explícita ao longo da implementação; o `plan-`* cita quais checks rodam por slice.
- **Complex** — idem Normal + gates entre fatias sensíveis quando o `plan-`* definir; harness é **obrigatório** no sentido de **evidência repetível** (CI ou comandos no README/`plan-*`); se a skill dedicada ainda não existir no repo, documente comandos por slice.

**Harness mínimo no repo (nível 0):** **um comando** documentado (README ou `plan-`*) que prove sanidade — ex.: `npm run lint`, `npm test`, `pytest`, equivalente da stack. **CI não é obrigatório** para esse nível (CI é evolução). **Primeira iniciativa** que precise de verificação repetível: incluir no `plan-`* um slice para **adicionar + documentar** esse comando quando ainda não existir.

**Quando não há sequer comando mínimo:** o gate vira execução manual de teste/lint/smoke com registo em `validation-`* (princípio `verification-before-completion`). Evitar estado eterno sem comando: tratar construção do nível 0 como trabalho explícito no `plan-*`.

`validation-*` pode (e deve, quando existir) citar saídas do harness: logs, eval, status de CI, rastros de smoke.

### Conhecimento

*Política `knowledge-` por trilha:**


| Trilha      | `knowledge-`*                                   |
| ----------- | ----------------------------------------------- |
| **Complex** | Obrigatório                                     |
| **Normal**  | Recomendado                                     |
| **Simple**  | Opcional                                        |
| **Hotfix**  | Opcional — aceite mínimo em `changelog-`* ou PR |


**Aprendizado em Complex** — não dispensar sem substituto equivalente documentado. **Hotfix sério** usa a variante post-mortem — definição única em Templates/Post-mortem.

**Métricas mínimas (opcional, recomendado em Normal/Complex):**

```markdown
## Métricas
- Tempo total (brainstorming → push):
- Reclassificação de trilha: (não | sim — qual e por quê)
```

Métricas extras (iterações Ralph, retrabalho, defeitos escapados) — opcional após volume (ex.: 6+ iniciativas).

### Concisão (`caveman`)

Em qualquer trilha e cada etapa, agentes e skills devem ser **concisos**: bullets, critérios verificáveis, só o necessário pra decisão / execução / artefato.

A skill `caveman` vive em **`.cursor/skills/caveman/SKILL.md`** (sem rule `.mdc` sempre-on neste repo) — **opt-in** por prompt; **opt-out** explícito válido ("não use caveman nesta tarefa", modo verboso).

### Realinhamento de trilha

Execução diferente da triagem? Volume de slices crescendo? Perguntas em aberto? → ver **00 — Triagem / Sinais de trilha errada**.

---

## Templates

### `plan-`*

**Estrutura mínima por slice:**

```markdown
### Slice N — <nome>
interface-design: <links/seções em spec/ADR/OpenAPI/UI — ou contrato mínimo em bullets>
vertical-slice: <critério ponta a ponta / valor entregue>
status: pending | in-progress | done | blocked
done-when:
  - <critério verificável 1>
  - <critério verificável 2>
```

**Slice bloqueado:**

```markdown
### Slice N — <nome>
status: blocked
blocked-by: <dependência externa>
owner: <pessoa/time/sistema>
next-check: YYYY-MM-DD
fallback: <seguir outro slice | pausar | reduzir escopo | HITL>
```

**Rollback (obrigatório em Complex; opcional/quando aplicável nas demais):**

```markdown
## Rollback
- **Condição de ativação:** (ex.: erro > X%, falha de smoke pós-deploy)
- **Passos:**
  1. ...
  2. ...
- **Responsável:** (você / papel)
- **Tempo estimado:**
- **Validação pós-rollback:** (como confirmar restauração)
```

**Timebox (opcional):** se Ralph ou slice estourar tempo/tentativas acordados → HITL para re-cortar.

**Contexto no chat:** preferir **caminhos** e trechos já em artefatos a colar blocos grandes de código.

### Handoff e retomada

**Gatilhos:**

- (a) ~70% do contexto consumido (sinal: respostas mais curtas/repetitivas);
- (b) ao fechar entrega vertical (feature, PR, milestone);
- (c) antes de encerrar voluntariamente sessão com trabalho em andamento.

No composer-2 do Cursor: até ~75% quando a sessão for predominantemente leitura de código já indexado.

**Pacote de handoff:**

```markdown
## Pacote de retomada — colar no topo do chat novo

- **Objetivo (uma frase):**
- **Trilha / fase:**
- **Branch / worktree:**
- **Artefatos:** (caminhos para @)
- **O que já está feito:**
- **Próximo passo concreto:**
- **Decisões / não fazer:**
- **Riscos abertos:**
  - [ ] [RISCO] algo que pode dar errado — impacto — mitigação
  - [ ] [INCERTEZA] algo a validar — o que checar
  - [ ] [DECISÃO PENDENTE] escolha adiada — quem decide / quando
- **Contexto acumulado:** ~XX% — gerado por [automático | fim de entrega | manual]
- **Comandos de verificação:** (testes, lint, harness)
- **Anexar no chat novo (@):**
```

**Pacote inicial — iniciativa existente** (sem handoff prévio):

```markdown
## Pacote inicial — iniciativa existente

- **Objetivo conhecido:**
- **Branch / worktree existente:**
- **Artefatos encontrados:**
- **Estado aparente do código:**
- **Último commit relevante:**
- **Riscos / incertezas detectadas:**
- **Primeira ação segura:**
```

**Mesmo chat, outro dia (opcional):** anotar no fim do `plan-`* três bullets — slice atual (status), próxima ação, algo que estranhou — ou usar o **Pacote de retomada** se for contexto novo.

**Slug `chat-handoff-bullets`:** o **formato modelo** das caixas abaixo permanece canônico nesta secção; uma skill com este slug ainda pode ser materializada sob `.cursor/skills/` quando fizer sentido (hoje: usar o template aqui e em `plan-*`/chat).

### PR (para o time)

Como o time só vê o PR, ele é o **artefato de comunicação principal** — trate como gate externo.

```markdown
## Resumo
<2–4 bullets do que muda>

## Trilha e contexto
- Trilha: Simple | Normal | Complex | Hotfix
- Iniciativa / artefatos: <caminhos para spec-*, plan-*, validation-*>

## Slices entregues
- Slice 1 — <nome> (link)
- Slice 2 — <nome> (link)

## Pontos pra olhar primeiro
- <onde você quer revisão atenta do time>

## Rollback
<link para seção do plan-* ou bullets>

## Auto-checklist
- [ ] Rubrica binária verde
- [ ] CI ok
- [ ] DoD da trilha cumprido
```

### `SKILL.md` — versionamento

Cabeçalho YAML (convenção Rheyder) em todo `SKILL.md`; para **discovery / description só com gatilhos** (“description trap”), ver o pacote **writing-skills** em **`.cursor/reference/skill-authoring/writing-skills/`**; para estrutura e split de ficheiros, ver **write-a-skill** em **`.cursor/reference/skill-authoring/write-a-skill/`**. Fluxo consolidado: agente **`skill-writer`** + mesma árvore em `skill-authoring/`:

```yaml
---
name: nome-do-slug
description: Use when <gatilhos em terceira pessoa — só o que dispara o carregamento; sem mini-workflow no description — ver [Templates / SKILL.md — versionamento](#skillmd--versionamento) e `.cursor/reference/skill-authoring/`>.
version: 1.0.0
last-updated: YYYY-MM-DD
method-version-compatible: rheyder-method >= 1.0
---
```

**Convenção de bump:** `patch` — texto; `minor` — novos critérios/etapas; `major` — mudança de interface (slug, artefatos esperados, agentes).

### Post-mortem (Hotfix sério)

Variante: `knowledge-YYYY-MM-DD-name-postmortem.md`.

```markdown
## Post-mortem
- **Causa raiz:**
- **Linha do tempo:**
- **Impacto:**
- **Detecção:**
- **Correção aplicada:**
- **Ação preventiva:**
- **Owner da ação preventiva:**
```

Obrigatório quando qualquer um: **(a)** usuários ou sistema externo **perceberam** indisponibilidade, perda de dados visível ou degradação grave (duração não é critério no modo solo — ver nota); **(b)** o **mesmo sintoma** reaparece após correção (incidente recorrente); **(c)** envolve authN/authZ, crypto, PII ou dado sensível. **Nota:** quando houver SLA/negócio, acrescentar limiar quantitativo (ex.: minutos fora) no `plan-`* ou ADR — até lá, gatilhos qualitativos bastam no modo solo.

---

## Skills e agentes

### Autocontenção da materialização Cursor

Artefatos em `**.cursor/skills/`**, `**.cursor/agents/**` e `**.cursor/rules/**` obedecem a:

1. **Execução só com o arquivo carregado** — salvo o que o Cursor injeta (`alwaysApply`, descoberta de agente) e paths **opcionais** para outras skills do **mesmo** `.cursor/` quando o produto exige; cada arquivo assim referenciado **também** é autocontido (sem cadeia A→B→C onde B só diz “ver A”).
2. **Bases upstream** — **mattpocock/skills** e **obra/superpowers** (Git em [Apêndice B](#apêndice-b--bases-metodológicas)), mais **este documento** ou trechos em `plan-*` / `validation-*`, são **insumo de autoria/revisão** onde ainda **não** houver texto consolidado sob `.cursor/`; o publicado em `.cursor/` **incorpora** o extrato conforme [Apêndice A](#apêndice-a--inventário-vs-derivação). **Exceção explícita:** **writing-skills** e **write-a-skill** têm **cópia canónica** em **`.cursor/reference/skill-authoring/`** — prefira esse caminho (e **`skill-writer`**) para autoria de `SKILL.md`.
3. **Proibido** usar “ler seção § … deste doc” (ou só um link externo) como **único** passo normativo sem resumo/checklist **no** skill, agente ou rule — exceto que, para **metodologia de escrita de skills**, a leitura obrigatória pode ser **só** o que está sob `.cursor/reference/skill-authoring/` + instruções do `skill-writer`, desde que o **artefato final** (`SKILL.md` em `.cursor/skills/`) permaneça autocontido.

Para **profundidade** em checklist/TDD aplicado a skills (incl. “description trap”), use **`.cursor/reference/skill-authoring/writing-skills/`** (e subagentes de pressão conforme `testing-skills-with-subagents.md` no mesmo diretório). **Não** há slug de skill invocável `writing-skills` no Catálogo — quem consolida o fluxo é o **agente** `skill-writer`.

### Catálogo

Versão **v1.0 fundida**: tabela abaixo alinha o método à **primeira leva materializada** em `.cursor/` neste checkout. Entradas **(pendente)** são slug canónico do método **sem** `SKILL.md` ainda — manter etapa no fluxo e documentar no `plan-*`/chat até existir ficheiro.

| Slug | Tipo | Cobertura |
| -------------------------------- | ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `brainstorming` | skill | Esclarecimento / grill-me; perfil pela **00 — Triagem** — ver **Brainstorming — perfil por triagem**. **Arquivo:** `.cursor/skills/brainstorming/SKILL.md`. (alias humano: *grill-me*) |
| `plan-writing` | skill | `plan-*` com slices verticais + interface-design por fatia + rollback (quando aplicável). **Arquivo:** `.cursor/skills/plan-writing/SKILL.md`. |
| `tdd-cycle` | skill | Red / green / refactor por slice. **Arquivo:** `.cursor/skills/tdd-cycle/SKILL.md`. (bases: mattpocock `tdd` + superpowers TDD — inventário) |
| `git-worktrees` | skill | Worktree quando o critério "quando criar" for verdadeiro. **Arquivo:** `.cursor/skills/git-worktrees/SKILL.md`. |
| `code-review` | skill | Rubrica binária + aderência (interface-design, slices, segurança). Usada nos passes de revisão. **Arquivo:** `.cursor/skills/code-review/SKILL.md`. |
| `verification-before-completion` | skill (norma injetada por rule) | Evidência antes de afirmar conclusão. **Rule sempre-on:** `.cursor/rules/verification-before-completion.mdc`. |
| `task-delivery` | skill | Prepara entrega + anúncio HITL. **Arquivo:** `.cursor/skills/task-delivery/SKILL.md`. |
| `systematic-debugging` | skill | Hotfix e debug sistemático. **Arquivo:** `.cursor/skills/systematic-debugging/SKILL.md`. |
| `improve-codebase-architecture` | skill | Deepening arquitetural sob interface-design. **Arquivo:** `.cursor/skills/improve-codebase-architecture/SKILL.md`. |
| `caveman` | skill | Concisão; só `SKILL.md`, sem `.mdc` sempre-on. **Arquivo:** `.cursor/skills/caveman/SKILL.md`. |
| `language` | rule `.mdc` | pt-BR para texto ao usuário; resto `en_US`. **Arquivo:** `.cursor/rules/language.mdc`. |
| `base-creation` | rule `.mdc` | Autoria autocontida no clone. **Arquivo:** `.cursor/rules/base-creation.mdc`. |
| `karpathy-guidelines` | rule `.mdc` | Disciplina de codificação LLM. **Arquivo:** `.cursor/rules/karpathy-guidelines.mdc`. |
| `knowledge-base` | skill **(pendente)** | Etapa learn / `knowledge-*` — **ainda sem** `.cursor/skills/knowledge-base/` neste repo; usar processo manual ou próximo slice de materialização. |
| `technical-discovery` | skill **(pendente)** | Discovery / spike (Complex passo 01) — **sem** `SKILL.md` dedicado aqui; repetir bullets em `spec-*`/`plan-*` até materializar. |
| `security-review` | skill **(pendente)** | Gate segurança — checklist no `plan-*` ou futura skill. |
| `workflow-harness` | skill **(pendente)** | Comandos repetíveis — usar README/`plan-*` + evidência; **sem** skill dedicada neste checkout. |
| `chat-handoff-bullets` | skill **(pendente)** | Formato em [Templates / Handoff](#handoff-e-retomada); skill opcional futura. |
| `task-orchestrator` | agent | Orquestra fatia vertical, Ralph, handoffs. **Arquivo:** `.cursor/agents/task-orchestrator.md`. |
| `slice-developer` | agent | Papel **developer** — implementação + TDD. **Arquivo:** `.cursor/agents/slice-developer.md`; prompt: `.cursor/agents/slice-subagents/slice-developer-prompt.md`. |
| `slice-spec-reviewer` | agent | Revisão intra-slice SPEC/plan ↔ código. **Arquivo:** `.cursor/agents/slice-spec-reviewer.md`; prompt: `.cursor/agents/slice-subagents/slice-spec-reviewer-prompt.md`. |
| `slice-quality-reviewer` | agent | Revisão intra-slice qualidade + evidência. **Arquivo:** `.cursor/agents/slice-quality-reviewer.md`; prompt: `.cursor/agents/slice-subagents/slice-quality-reviewer-prompt.md`. |
| `skill-writer` | agent | Autoria/refino de `SKILL.md`: consolida TDD-for-skills (writing-skills), estrutura progressiva (write-a-skill) e `anthropic-best-practices.md` **a partir de** `.cursor/reference/skill-authoring/`. **Arquivo:** `.cursor/agents/skill-writer.md`. |

**Mapeamento `developer` / `reviewer` / modo solo:** ver [Modo solo](#modo-solo-default-deste-repo) e [Convenções / Revisão](#revisão). **Neste repo:** ficheiros em `.cursor/agents/` (sem `workflow-pack/`). **Consumo noutro clone:** [Pacote workflow-pack e symlink](#pacote-workflow-pack-e-symlink-em-projetos-consumidores).

### Rules `.mdc`

Normas transversais materializadas como **rules** Cursor (`.mdc`) — paralelo ao **Catálogo** de skills/agentes: skills descrevem fluxos com critério de saída; rules injetam política **sempre-ativa** ou **escopada por arquivo** sem substituir o Catálogo como slug de workflow. **Neste repositório** vivem em **`.cursor/rules/*.mdc`** (ver tabela abaixo).

**Frontmatter canônico (`.mdc`):**

```yaml
---
description: <texto ≤ 1024 caracteres — propósito da rule>
alwaysApply: true | false
globs: [<opcional — padrões glob quando a rule for escopada por caminho>]
---
```

**Categorias:**

1. **Discipline-enforcing sempre-on** — pressão comportamental constante no workspace (ex.: evidência antes de conclusão; diretrizes tipo Karpathy).
2. **Convenção de repo escopada por glob** — norma aplicada só onde `globs` casam (ex.: `plan-*.md`, `spec-*.md` na raiz do repo ou `.cursor/rheyder-method*.md`).
3. **Override de comportamento default** — redefine expectativa padrão do agente (ex.: proibir commit/push por iniciativa própria).
4. **Contexto persistente** — modo de trabalho ou política transversal que não é skill com critério binário próprio (ex.: modo solo — papéis acumulados, HITL).

**Quando promover conteúdo → rule `.mdc` canônica**

Regra para norma **nova** que **não** está já coberta pelo programa de implementação v1.0 (`plan-*`). Crie `.mdc` quando **todos**:

- (a) o texto é **norma transversal** (aplica-se além de um fluxo com critério de saída de skill);
- (b) precisa ficar **carregada por padrão** (`alwaysApply: true`) ou **repetida de forma estável** via `globs`;
- (c) mantê-la só em prompts ou bullets dispersos geraria **drift** face ao método.

Antes disso: seção neste documento, template em `plan-*`, ou skill **autocontida** que já embute o extrato necessário da norma — não rule separada.

**Exceção:** as rules previstas no `**plan-*` de implementação v1.0** entram pelo roadmap (incluindo candidatas **adiadas** — não materializar até o gatilho definido no plano).

**Rules `.mdc` ativas** (escopo do programa; slugs dedicados fora do Catálogo como skill/agent):


| Rule                                 | Categoria                         | Frontmatter         | Fonte canônica no método                                                   | Caminho neste repo                                                                                                                                                              |
| ------------------------------------ | --------------------------------- | ------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `karpathy-guidelines.mdc`            | discipline-enforcing sempre-on    | `alwaysApply: true` | [Apêndice B — Bases metodológicas](#apêndice-b--bases-metodológicas)       | `.cursor/rules/karpathy-guidelines.mdc`                                                                                                                                 |
| `verification-before-completion.mdc` | discipline-enforcing sempre-on    | `alwaysApply: true` | [Convenções / Revisão](#revisão) (rubrica / princípio) + entrada no Catálogo | `.cursor/rules/verification-before-completion.mdc`                                                                                              |
| `language.mdc`                       | contexto persistente / sempre-on  | `alwaysApply: true` | Convenção de idioma (pt-BR vs `en_US`) para o projecto                                              | `.cursor/rules/language.mdc` |
| `base-creation.mdc`                  | contexto persistente / sempre-on   | `alwaysApply: true` | [Autocontenção da materialização Cursor](#autocontenção-da-materialização-cursor) | `.cursor/rules/base-creation.mdc` |
| `modo-solo.mdc`                      | convenção / contexto persistente  | `alwaysApply: true` | [Modo solo](#modo-solo-default-deste-repo)            | `.cursor/rules/modo-solo.mdc`                                                                                                                   |
| `commit-push-hitl.mdc`               | override de comportamento default | `alwaysApply: true` | [Convenções / Commit e entrega](#commit-e-entrega-hitl-obrigatório)          | `.cursor/rules/commit-push-hitl.mdc`                                                                                                            |


`**caveman`:** **não** vira `.mdc` — skill em **`.cursor/skills/caveman/SKILL.md`**; concisão permanece carregável, não sempre-on.

**Adiadas para v1.1 reativa** (só promover se o piloto registrar falha real — ver `plan-*`):


| Rule candidata                             | Por que adiar                                                               | Gatilho de promoção                                           |
| ------------------------------------------ | --------------------------------------------------------------------------- | ------------------------------------------------------------- |
| `interface-design-slices.mdc`              | Norma já referenciável na skill `plan-writing`; rule duplicaria texto longo | Uma falha real onde código foi escrito sem `plan-*` carregado |
| `artefatos.mdc` (`globs: ["**/plan-*.md", "**/spec-*.md", ".cursor/rheyder-method*.md"]`) | Sem volume, convenção cabe nos templates do método                          | 3+ artefatos divergindo do cabeçalho YAML padrão              |


**Não viram rule (decisão registrada):**

- `branches-worktrees.mdc` — coberto pela skill `git-worktrees` (nível 5); convenção curta de branch `<trilha>/<slug>` pode viver em bullet da rule `modo-solo.mdc`.

### Brainstorming — perfil por triagem (skill única)

Slug canônico: `brainstorming`. O **perfil** segue o resultado da triagem (não é escolha informal). Modos **hotfix**, **express**, **express-assistida** e **full** são perfis internos documentados em `.cursor/skills/brainstorming/SKILL.md`, não slugs separados no Catálogo.


| Resultado da triagem   | Perfil                | Notas                                                                                                                 |
| ---------------------- | --------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Hotfix**             | **hotfix**            | Bullets mínimos: sintoma reproduzível ou evidência equivalente, escopo mínimo da correção, **não-fazer** neste fix.   |
| **Simple — Express**   | **express**           | Bullets: **o quê** + **como validar** (ver critério *Validação rápida* em **1 — Simple**); sem obrigação de `spec-`*. |
| **Simple — Assistida** | **express-assistida** | Como **express**, com saída suficiente para alimentar `plan-writing` (ligação clara a slices ou riscos explícitos).   |
| **Normal**             | **full**              | Saída orientada a materializar `spec-`* (estrutura PRD-like quando aplicável).                                        |
| **Complex**            | **full**              | Igual **full** no brainstorming; **spec hardening** (NFR, limites, rollback) nos passos seguintes da trilha.          |


**Critério de saída (resumo):** *hotfix* — reprodução/evidência + escopo mínimo + não-fazer; *express* — mudança + validação; *express-assistida* — o anterior + ponte ao plano; *full* — sem lacunas de decisão que obriguem inventar escopo no `plan-`*.

**Anti-drift:** ao usar a skill, preâmbulo explícito no prompt: `Perfil: <hotfix \| express \| express-assistida \| full> | Trilha: <...> | Saída: bullets no chat \| spec-* (Normal/Complex).` Em *hotfix* e *express*, no máximo **dois níveis** (título + bullets); sem seções PRD longas — preferir **perguntas** se faltar profundidade. Se em Simple Express surgirem **3+ decisões abertas** ou necessidade grande de contrato novo → **parar** e reclassificar (ver **00 — Triagem / Sinais de trilha errada**).

### Composição típica

`task-orchestrator` + `git-worktrees` (quando aplicável) + harness **documentado** (comandos no `plan-*`/README — skill `workflow-harness` ainda pendente) →
**por slice:** `slice-developer` + `tdd-cycle` onde couber → **`slice-spec-reviewer`** (com `code-review`) → **`slice-quality-reviewer`** (com `code-review`) → **resumo ao usuário** (Commit e entrega) → **HITL: commit** →
**passe final** com `code-review` + **`validation-*`** → `task-delivery` → **HITL: push / PR** → (etapa aprendizado / `knowledge-*` conforme trilha — skill **`knowledge-base`** ainda pendente).

**Rules sempre-on:** `.cursor/rules/*.mdc`. **Concisão opt-in (`caveman`):** ver [Concisão (`caveman`)](#concisão-caveman).

### Roadmap de materialização

Leitura **v1.0 fundida** — estado deste checkout. Os **níveis 0–5** no texto genérico descrevem ordem de construção; **neste repo** grande parte já está sob `.cursor/` **sem** pasta `workflow-pack/`. Use a tabela abaixo como **status** (não como promessa futura):

| Nível | Slugs | Estado no repo |
| ----- | ----- | ---------------- |
| **0 — bootstrap (autoria)** | bases **mattpocock/skills** + **obra/superpowers** (clone opcional — Apêndice B); **writing-skills + write-a-skill** em `.cursor/reference/skill-authoring/` | Derivar sempre para texto sob `.cursor/`; **autoria de `SKILL.md`** prefira `skill-authoring/` + agente `skill-writer` |
| **1 — políticas transversais** | `caveman`, `verification-before-completion`, `karpathy-guidelines` (+ `language`, `base-creation` **adicionadas** neste projeto) | **Materializado** — skills + rules em `.cursor/` |
| **2 — entrada do fluxo** | `brainstorming`, `plan-writing` | **Materializado** |
| **3 — execução nuclear** | `tdd-cycle`, `code-review`, `task-delivery` | **Materializado** |
| **4 — agentes** | `task-orchestrator`, `slice-developer`, `slice-spec-reviewer`, `slice-quality-reviewer`, `skill-writer` + prompts em `slice-subagents/` (exceto `skill-writer`) | **Materializado** (papéis normativos `developer`/`reviewer` mapeiam para os três primeiros subagents) |
| **5 — complementares** | `git-worktrees`, `systematic-debugging`, `improve-codebase-architecture` | **Materializado** |
| *(pendente programa ampliado)* | `technical-discovery`, `security-review`, `workflow-harness`, `knowledge-base`, `chat-handoff-bullets` | **Sem** `.cursor/skills/<slug>/` — seguir método com bullets em `spec-*`/`plan-*` ou materializar depois |


**Critério "skill mínima viável" (v1.0.0):**

- (a) cabeçalho YAML padrão preenchido;
- (b) **gatilho** explícito (quando carregar);
- (c) **input** esperado (contexto / artefatos requeridos);
- (d) **passos** numerados e verificáveis;
- (e) **critério de saída** binário (não "estilo");
- (f) referência ao(s) artefato(s) que materializa, se houver.

Skill que não cumpre (a)-(e) volta pra prompt salvo até a próxima rodada.

### Mapeamento Cursor (paths reais)

| Slug | Caminho | Como disparar |
| ---- | ------- | ------------- |
| Rules sempre-on (6) | `.cursor/rules/*.mdc` | Carregamento automático conforme Cursor |
| `brainstorming` … `task-delivery` | `.cursor/skills/<slug>/SKILL.md` | Menção pela skill / etapa da trilha |
| `task-orchestrator` | `.cursor/agents/task-orchestrator.md` | Subagent / chat dedicado |
| `slice-developer` | `.cursor/agents/slice-developer.md` + `slice-subagents/slice-developer-prompt.md` | Passo implementação |
| `slice-spec-reviewer` | `.cursor/agents/slice-spec-reviewer.md` + `slice-spec-reviewer-prompt.md` | Gate intra-slice 1 |
| `slice-quality-reviewer` | `.cursor/agents/slice-quality-reviewer.md` + `slice-quality-reviewer-prompt.md` | Gate intra-slice 2 |
| `skill-writer` | `.cursor/agents/skill-writer.md` | Autoria ou revisão de skills (disparo sob demanda) |
| Referências | `.cursor/reference/workflow-conventions.md`, `tdd-public-api-tests.md`, **`skill-authoring/`** (writing-skills + write-a-skill) | Citadas por skills ou pelo `skill-writer` |

**Referência IDE:** [.cursor/README.md](../README.md).

### Pacote workflow-pack e symlink em projetos consumidores

**Neste monorepo de metodologia:** o workflow está **diretamente** em `.cursor/skills/`, `.cursor/agents/` e `.cursor/rules/` — **não** há `.cursor/workflow-pack/` versionado aqui.

**Consumo noutro repositório:** podes **copiar ou symlinkar** a pasta `.cursor/` (ou só subpastas) deste projeto; em alternativa, algumas equipas empacotam um subconjunto numa pasta `workflow-pack/` e ligam ao destino conforme SO — o mesmo conteúdo, outro layout. **Rules locais** da stack do app ficam em `.mdc` que complementam (não contradizem sem registo) as rules do pack.

**Subagents:** o Cursor expõe definições em **`/.cursor/agents/*.md`**. Este repo segue o modelo **orquestrador + três agents por fatia** (implementação e revisão dupla) listados no [Catálogo](#catálogo), mais **`skill-writer`** para autoria de skills sob demanda.

**Hooks:** automações por hooks do Cursor — **após** skills/agentes/rules estáveis — continuam fora do âmbito mínimo v1.0 deste documento.

### Quando promover prompt → skill canônica

Regra para **novos** prompts que **não** são slugs já listados no **Catálogo** nem cobertos pelo **programa de implementação v1.0** (`plan-*`). Crie `SKILL.md` quando **todos**:

- (a) você usou o mesmo prompt em **3+ iniciativas distintas**;
- (b) o prompt tem **critério de saída claro** (não é só "estilo de escrever");
- (c) o agente carrega errado quando você não dá o contexto manualmente.

Antes disso: prompt salvo, não skill.

**Exceção:** skills e agents listados no roadmap de **materialização** entram por `plan-*` de implementação, não só pela regra dos 3 usos; slugs **pendentes** no [Catálogo](#catálogo) dependem de novo slice de materialização.

---

## Apêndice A — Inventário vs derivação

Skills entram **direto no inventário** (adoção/adaptação mínima) ou servem de **base** para skills canônicas com slug Rheyder. **mattpocock/skills** e **obra/superpowers** são os **pacotes upstream** para autoria (ver [Apêndice B](#apêndice-b--bases-metodológicas)); fundir conteúdo nos artefatos em `.cursor/` conforme [Autocontenção da materialização Cursor](#autocontenção-da-materialização-cursor). **Pacotes writing-skills e write-a-skill** têm **cópia espelhada** em **`.cursor/reference/skill-authoring/`** — não são leitura obrigatória do agente em produção, exceto quando **autorar** skills via `skill-writer`.

### Adoção direta


| Slug Rheyder                     | Base upstream (paths relativos dentro do pacote)                                                         | Nota                                                                 |
| -------------------------------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `improve-codebase-architecture`  | mattpocock/skills — `skills/engineering/improve-codebase-architecture/`                                | Implementa a regra interface-design.                                 |
| `caveman`                        | mattpocock/skills — `skills/productivity/caveman/`                                                     | Política de concisão.                                                |
| `verification-before-completion` | obra/superpowers — `skills/verification-before-completion/`                                            | Princípio meta: nenhuma afirmação de conclusão sem evidência fresca. |


### Derivação (consolidação a partir de bases)


| Slug Rheyder           | Bases                                                                                                                                                                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `brainstorming`        | mattpocock `grill-me` + `grill-with-docs` + `to-prd` + superpowers `brainstorming`                                                                                                                                                         |
| `plan-writing`         | superpowers `writing-plans` + mattpocock `to-issues`                                                                                                                                                                                       |
| `tdd-cycle`            | mattpocock `tdd` + superpowers `test-driven-development`                                                                                                                                                                                   |
| `git-worktrees`        | superpowers `using-git-worktrees`                                                                                                                                                                                                          |
| `code-review`          | superpowers `requesting-code-review` + `receiving-code-review`                                                                                                                                                                             |
| `task-delivery`        | superpowers `finishing-a-development-branch`                                                                                                                                                                                               |
| `karpathy-guidelines`  | observações públicas Karpathy sobre armadilhas comuns de LLMs em código (ver Apêndice B); consolidadas em texto canônico no `SKILL.md` deste método                                                                                        |
| `systematic-debugging` | superpowers `systematic-debugging` + mattpocock `diagnose` (enriquecimento: feedback loop como skill central + lista de técnicas pra construir o loop + convenção `[DEBUG-<id>]` pra cleanup + perf branch para regressões de performance) |


**Sobre `brainstorming` com 4 bases:** ao materializar o `SKILL.md` único, absorver só o que é distintivo de cada fonte:

- `grill-me` (mattpocock) — interview *relentless*, perguntas *uma por vez*, recomendação por questão; no método Rheyder o perfil de `brainstorming` é **fixado pela 00 — Triagem** (ver **Skills / Brainstorming — perfil por triagem**).
- `grill-with-docs` (mattpocock) — manutenção *lazy* de glossário (`CONTEXT.md`) e ADRs *sparingly* (3 critérios: hard to reverse, surprising without context, real trade-off);
- `to-prd` (mattpocock) — estrutura de saída em formato PRD (Problem Statement, Solution, User Stories, Implementation Decisions, Out of Scope) quando o output é `spec-`*;
- `brainstorming` (superpowers) — contrato de finalização e critério de quando parar.

Se na materialização o slug ficar largo demais, separar: `to-prd` vira base de uma skill própria de síntese de requisitos e `brainstorming` mantém só o grilling.

**Sobre `code-review` (`requesting-code-review` + `receiving-code-review`):** no modelo deste repo, o isolamento de contexto vira **subagents** `slice-spec-reviewer` e `slice-quality-reviewer` + skill `code-review` para a rubrica; no modo solo continuam **passes** distintos no tempo, não pessoas.

### Autoria de SKILL.md (bases sem slug Rheyder)

Este método **não** define slug Cursor invocável `**writing-skills**`. Ao criar ou refinar artefatos `SKILL.md` em `.cursor/skills/`, use (autoria):


| Ferramenta | Caminho neste checkout |
| ---------- | ------------- |
| superpowers (writing-skills) | `.cursor/reference/skill-authoring/writing-skills/` — `SKILL.md`, `testing-skills-with-subagents.md`, `anthropic-best-practices.md`, etc. |
| mattpocock (write-a-skill) | `.cursor/reference/skill-authoring/write-a-skill/SKILL.md` |
| agente consolidador | `.cursor/agents/skill-writer.md` — lê os dois pacotes acima e aplica § [Templates / `SKILL.md` — versionamento](#skillmd--versionamento) + § Autocontenção + critério **skill mínima viável** no roadmap |
| clones upstream (opcional) | Clones separados de **mattpocock/skills** e **obra/superpowers** (URLs no Apêndice B) apenas para diff com upstream — **não** como dependência da execução neste checkout |

**Nota:** atualize `.cursor/reference/skill-authoring/` quando integrar novas versões **obra/superpowers** / **mattpocock/skills** (slice de manutenção explícito se necessário).


### Agentes canônicos

Os agentes do [Catálogo](#catálogo) materializam orquestração, implementação, revisão das trilhas e **autoria de skills** (`skill-writer`); mapeamento solo e de papéis em [Modo solo](#modo-solo-default-deste-repo). Paralelismo de slices segue a lógica de superpowers `dispatching-parallel-agents` quando documentado no `plan-*`.

---

## Apêndice B — Bases metodológicas

**Repositórios Git das bases** (ao longo do doc, **mattpocock/skills** e **obra/superpowers** referem-se a estes projetos):

- **mattpocock/skills** — [https://github.com/mattpocock/skills](https://github.com/mattpocock/skills)
- **obra/superpowers** — [https://github.com/obra/superpowers](https://github.com/obra/superpowers)

**Referência pontual:**

- **Karpathy — armadilhas de LLMs em código (referência)** — [https://x.com/karpathy/status/2015883857489522876](https://x.com/karpathy/status/2015883857489522876) — base conceitual de `karpathy-guidelines`
