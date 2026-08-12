# Referências — Fase de Spec (Novatech)

> Arquivo de apoio para consulta durante a construção da spec. Atualizado à medida que o material for recebido e o contexto for refinado com o Product Specialist Senior.

## Como usar este arquivo
- Cada material recebido é registrado na seção **Índice de Materiais**, com caminho, resumo curto e tags.
- Decisões tomadas durante o refinamento vão em **Decisões**.
- Perguntas ainda sem resposta ficam em **Dúvidas em Aberto** até serem resolvidas (aí migram para Decisões).
- Observações e aprendizados que não se encaixam nas seções acima vão em **Notas de Refinamento**.

---

## 1. Contexto Geral da Fase

- **Projeto:** Novatech
- **Fase anterior (Cenário 1 / Prática 1):** Discovery + Entendimento (concluída) — produziu ADRs de arquitetura, spec de requisitos do pipeline de RAG, protótipo funcional, cenários de falha (QA) e plano de testes inicial.
- **Fase atual (Cenário 2 / Prática 2):** Estruturação do Trabalho — preparar ambiente, padrões e artefatos que vão governar o desenvolvimento, antes da primeira linha de código de produção.
- **Tópicos cobertos nesta fase:** MCP (Model Context Protocol), Recorte de Domínio e Spec Driven Development (SDD), AGENTS.md, Skills.
- **Ferramentas disponíveis:** Claude (chat, todos os papéis), GitHub Copilot (devs e Tech Lead), Claude Cowork (Delivery Manager, Product Specialist, QA), Claude Design (Product Specialist).
- **Time:** 1 Tech Lead, 2 Desenvolvedores (1 pleno, 1 sênior), 1 QA, 1 Product Specialist, 1 Delivery Manager.
- **Repositório:** `novatech-assistant` (prefixo `db1/` é narrativo) — trabalhado como Git **local** nesta fase, sem remoto/GitHub.

### Decisões já fechadas na fase anterior (não rediscutir, usar como premissa)
- **Modelo LLM:** Azure OpenAI GPT-4o, janela 128K (ADR-0001).
- **Pipeline de RAG:** Azure AI Search + Azure OpenAI (protótipo open-source ChromaDB + sentence-transformers validou a abordagem; identificou problemas de chunking em tabelas) (ADR-0004).
- **Estratégia de contexto:** budget ~4K tokens system prompt + ~8K chunks (5 chunks de ~1.500 tokens) + pergunta + histórico limitado a 3 turnos (ADR-0002).
- **Documentos contraditórios:** metadado de vigência no pipeline; prompt prioriza versão mais recente; obsoletos marcados, não excluídos (ADR-0003).
- **Integração:** Microsoft Teams (bot) + painel web interno.
- **Base documental:** de ~1.250 fontes brutas, 847 documentos válidos consolidados (12 com contradições pendentes de resolução pelo Compliance), 63 descartados por obsolescência, ~340 eliminados como duplicatas/redundâncias.
- **Arquitetura:** 4 componentes — (1) pipeline de ingestão, (2) API do assistente (Azure Functions + Azure AI Search + Azure OpenAI), (3) bot Teams (Bot Framework), (4) painel web interno.
- **Stack:** TypeScript (backend/bot), React (painel web), Bicep (IaC).

### O desafio desta fase (o que a spec precisa endereçar)
1. Definir como agentes de IA (Copilot, Claude Code) serão usados no desenvolvimento — regras, limites, padrões.
2. Recortar o domínio do projeto (bounded contexts, linguagem ubíqua) e especificar o que será construído usando Spec Driven Development.
3. Configurar as conexões que os agentes precisam para operar (MCP servers para acessar repositório, docs, Azure).
4. Criar skills reutilizáveis que encapsulam os padrões do projeto para geração consistente de código e artefatos.

### Princípios de trabalho (válidos para todos os artefatos desta fase)
> **"O óbvio deve ser escrito e falado."** Não omitir uma regra, comportamento ou definição só porque parece implícita ou evidente — para um agente de IA (ou um leitor sem o contexto da conversa), o que é óbvio para quem escreveu não é óbvio para quem lê. Aplicar isso em toda spec, domain model, AGENTS.md e skill desta fase.
>
> **"Toda decisão deve passar pelo usuário."** Não basta sinalizar só os pontos que Claude julga "bloqueantes" — qualquer escolha editorial/de design feita durante a redação de um rascunho (wording de um outcome, campos de um schema de resposta, nível de confiança atribuído a um caso de teste, um detalhe adicionado que não estava literalmente na fonte) deve ser listada explicitamente para aprovação do usuário, separada por categoria, antes ou junto da apresentação do rascunho.

### Diretriz de trabalho desta sessão (passada diretamente pelo usuário)
> "Antes de escrever a spec, você precisa recortar o domínio: quais são os bounded contexts do projeto, qual a linguagem ubíqua do domínio de logística que o time (e os agentes) devem usar, e quais são as fronteiras do que o assistente faz e não faz. Depois, você escreve a spec de requisitos do módulo principal usando SDD."

Ordem de trabalho definida:
1. **Recorte de domínio** — bounded contexts do projeto, linguagem ubíqua (domínio de logística), fronteiras do que o assistente faz/não faz.
2. **Spec de requisitos** (`requirements.md`, formato SDD) do **módulo principal** — ainda a confirmar qual dos 5 (ver Dúvidas em Aberto).

### Estrutura do repositório e artefatos a produzir (Anexo C)
- **Repositório local** `novatech-assistant` (sem remoto/GitHub/Azure reais nesta fase; infra Bicep é "estado narrativo").
- **5 módulos com specs SDD** (pasta `/specs/<slug>/`, cada um com `requirements.md` + `plan.md` + `tasks.md`, hoje vazios): `pipeline-ingestao`, `query-endpoint`, `feedback-api`, `teams-bot`, `painel-web`.
  - `requirements.md` → escrito pelo **Product Specialist** (nosso papel nesta spec), aprovado pelo Tech Lead.
  - `plan.md` → Tech Lead, aprovado por Product Specialist + Dev Sênior.
  - `tasks.md` → Dev com apoio de IA, aprovado pelo Tech Lead.
- **AGENTS.md** (constitution do projeto) — ainda vazio, a ser escrito nesta fase.
- **Skills** em 3 níveis — Foundation → Domain → Artifact — pastas criadas, arquivos vazios.
- **MCP** — `.mcp/mcp.json` ainda não criado; exemplo de referência usa servers locais/gratuitos: `filesystem` (código, specs, skills, docs, dados), `git` (histórico local), `memory` (glossário/linguagem ubíqua), `everything` (aprendizado de primitivas MCP). Sem servers pagos/Azure/GitHub nesta fase.

---

## 2. Índice de Materiais

### 2.0 Base Principal desta Fase — Prática 2 (usar como fonte primária)

| # | Documento | Caminho | Resumo | Tags |
|---|-----------|---------|--------|------|
| B1 | Cenário-Âncora 2 — Fase de Estruturação do Trabalho | `Prática 2\Product-Specialist\z01-ContextoGeral.md` | Documento de contexto oficial desta fase: tópicos (MCP, SDD, AGENTS.md, Skills), ferramentas por papel, anexos de apoio, recapitulação das decisões da fase anterior e os 4 desafios a endereçar na spec. | contexto, cenário 2, estruturação, SDD, MCP, skills |
| B2 | Anexo A — Documentação Simulada da NovaTech | `Prática 2\Especificações-Exercicio\anexo-a-documentacao-simulada-novatech.md` | Fonte de verdade do projeto: os 5 documentos-chave completos (POL-001 devolução, PROC-042 v1 e v2 frete especial, SLA-2024, FAQ-Atendimento informal) + meta-notas com contradições e gaps já identificados. Também disponível como arquivos individuais em `anexo-a-documentos-individuais/` para ingestão em pipeline. | documentação-fonte, guardrails, glossário, dados de teste, contradições |
| B3 | Anexo B — Chunks de Referência do Pipeline de RAG | `Prática 2\Especificações-Exercicio\anexo-b-chunks-referencia-rag.md` | Simula o output do pipeline de RAG (chunks que o Azure AI Search retornaria): chunks extraídos dos 5 documentos, mapa de cobertura pergunta→chunks esperados, e "armadilhas" propositais para exercícios de avaliação (contradição de versões, FAQ não confiável, tier inexistente, inversão de regra, pergunta sem cobertura). | chunks, RAG, retrieval, dados de teste, avaliação |
| B4 | Anexo C — Estrutura do Repositório NovaTech Assistant | `Prática 2\Especificações-Exercicio\anexo-c-estrutura-repositorio.md` | Mapa de diretórios do repositório local `novatech-assistant` (início da fase): AGENTS.md vazio, 5 pastas de specs SDD (`pipeline-ingestao`, `query-endpoint`, `feedback-api`, `teams-bot`, `painel-web`) com requirements/plan/tasks vazios, skills em 3 níveis (foundation/domain/artifact) vazias, prompts, src, tests, infra (Bicep narrativo). Inclui convenções de nomenclatura e exemplo de `.mcp/mcp.json` com servers locais gratuitos (filesystem, git, memory, everything). | repositório, estrutura, specs, skills, MCP, convenções |

### 2.0.1 Inputs Confirmados para o Recorte de Domínio + Spec do query-endpoint (enunciado do Exercicio 2.1)

Conjunto de inputs fornecido pelo usuário como base direta para produzir `domain-model.md` e `requirements.md`:

| Input | Conteúdo | Alimenta |
|---|---|---|
| Cenário completo | (já registrado nas seções 1 e 2.0 — B1) | Contexto geral, decisões da fase anterior |
| Documentação NovaTech | Anexo A (B2) — usar para extrair termos do domínio e identificar bounded contexts | `domain-model.md` |
| Spec de requisitos de RAG (fase anterior, simulada) | *"O assistente responde perguntas sobre SLAs, frete e devoluções. Fontes contraditórias devem mostrar ambas as versões. O assistente nunca inventa informações. Toda resposta cita fonte. Atualização em até 24h."* | Escopo e constraints do `requirements.md` do query-endpoint |
| Fluxo SDD (estrutura do requirements.md) | *"requirements.md contém: outcomes, scope boundaries, constraints, prior decisions, verification criteria."* | Template/estrutura do `requirements.md` |
| Dados do discovery (quantitativos) | *"As perguntas mais frequentes caem em 4 categorias: prazos de entrega, regras de frete, política de devolução e SLAs. Em 15% dos casos, a pergunta cruza duas categorias. Os atendentes precisam da resposta em menos de 30 segundos."* | Outcomes/verification criteria do `requirements.md`; possíveis bounded contexts ou fronteiras entre categorias |
| Conceito de recorte de domínio | *"Bounded contexts definem fronteiras claras entre subdomínios. Linguagem ubíqua é o vocabulário compartilhado que todo membro do time (e todo agente) usa da mesma forma. Para IA, recorte de domínio é especialmente importante porque agentes sem domínio claro geram outputs genéricos."* | Guia conceitual para o `domain-model.md` |

> **Observação:** o input de discovery cita "prazos de entrega" como uma das 4 categorias mais frequentes, mas nenhum documento do Anexo A cobre esse tema diretamente (o Anexo A cobre devolução, frete especial, SLA e FAQ) — possível gap a registrar no `domain-model.md` ou no `requirements.md` (fronteira do que o assistente não cobre ainda).

> **Pendente:** Anexo C menciona um **Anexo D — Starter Repo** (repositório com a árvore acima já criada, `git init` feito, pastas `docs/novatech/` e `data/retrieval-corpus/` semeadas a partir dos Anexos A e B) — ainda não recebido.

### 2.2 Artefatos em Produção nesta Sessão (Documentos-Gerados desta fase)
*Caminho base: `Prática 2\Exercicio2.1\Documentos-Gerados\`*

| # | Documento | Arquivo | Status | Resumo |
|---|-----------|---------|--------|--------|
| A1 | Dicionário de Termos | `dicionario-de-termos.md` | ✅ Criado | Glossário de termos de processo/metodologia e tecnologia/IA (spec, SDD, bounded context, RAG, MCP, skill, etc.), em linguagem acessível a leitores não técnicos. Termos de negócio/logística ficam separados, no `domain-model.md`. |
| A2 | Modelo de Domínio | `domain-model.md` | ✅ Aprovado (v0.4, pós avaliação por critérios) | 5 bounded contexts, linguagem ubíqua com 12 termos (incl. disambiguação de "Gold"/"Standard" para agentes de IA), fronteiras do assistente (5 gaps, FAQ não usado como fonte em nenhum caso), Governança Documental com vigência só para uso interno. |
| A3 | Requisitos — query-endpoint | `requirements.md` | ✅ Aprovado (v0.5, pós conferência final de consistência) | Spec SDD completa. 14 critérios de verificação (VC-01 a VC-14), outcomes reescritos para foco em resultado (não feature/métrica técnica), constraint de latência alinhada à metodologia do VC-10, risco técnico do VC-11 sinalizado para Dev Sênior. |
| A4 | Prompt de Mockup — Resposta no Teams | `mockup.jpg` em `Documentos-Gerados\`; `mockup-prompt-claude-design.md` movido para `Prática 2\Exercicio2.1\Prompts-Histórico\` (decisão do usuário) | ✅ Aprovado pelo usuário (como está) | Prompt de interação (pergunta sobre frete especial com contradição PROC-042 v1/v2), estilo visual real do Microsoft Teams, indicador de confiança reforçado (borda colorida + alerta), botão de bloqueio de resposta com motivo. Testado no Claude Design — resultado fiel ao prompt. Um detalhe de rótulo ("PROC-042" vs "PROC-042 v1" no rodapé de fontes) foi identificado e conscientemente não corrigido pelo usuário. |
| A5 | Histórico de Iteração (entregável final) | Movido para `Prática 2\Exercicio2.1\Prompts-Histórico\historico-iteracao.md` (decisão do usuário) | ✅ Concluído | Consolida todas as rodadas de revisão dos 3 artefatos anteriores + lista de itens em aberto para as próximas subfases. Fecha os 4 entregáveis do Exercicio 2.1. |
| A6 | Registro da Interação com o Claude Design | `Prática 2\Exercicio2.1\Prompts-Histórico\claude-design.md` | ✅ Já existente (criado pelo usuário) | Registro completo da sessão real no Claude Design: prompt inicial, construção do mockup, **um ajuste de layout responsivo** (card com largura fixa cortava o callout em janelas menores — corrigido para `max-width` + `flex`), e as 2 exportações (`mockup.jpg`, este próprio arquivo). Confirma o padrão previsto: ajuste pontual, não regeneração completa. |

> **Nota de organização:** seguindo o mesmo padrão da Prática 1 (onde `Prompts-Histórico\` guardava `Claude.md` e `claude design.md`, separados de `Documentos-Gerados\`), o usuário optou por mover `mockup-prompt-claude-design.md` e `historico-iteracao.md` para `Prompts-Histórico\` — mantendo em `Documentos-Gerados\` apenas os artefatos que são produto final direto (`dicionario-de-termos.md`, `domain-model.md`, `requirements.md`, `mockup.jpg`). O `claude-design.md` (A6) já estava em `Prompts-Histórico\`, registrado diretamente pelo usuário.

### 2.1 Histórico — Prática 1 (pode ou não ser necessário nesta fase)

#### 2.1.1 Exercicio1.1 — Entendimento / Discovery
*Caminho base: `Prática 1\Exercicio1.1\Documentos-Gerados\`*

| # | Documento | Arquivo | Resumo | Tags |
|---|-----------|---------|--------|------|
| 1 | Mapa de Temas Cobertos — Discovery NovaTech | `01-mapa-temas-cobertos.md` | Consolida os temas cobertos pelos 5 documentos-fonte da NovaTech e pelo Cenário, listando sobreposições/conflitos entre fontes, temas órfãos e um glossário consolidado. | discovery, mapa de temas, inconsistências, glossário |
| 2 | Hipóteses de Gaps — Discovery NovaTech | `02-hipoteses-gaps.md` | Classifica 16 gaps/riscos de discovery por probabilidade x impacto (matriz P1-P4), com recomendações priorizadas para a próxima rodada. | gaps, riscos, priorização, discovery |
| 3 | Análise de Inconsistências PROC-042 v1 x v2 | `03-analise-inconsistencias-proc042-v1-v2.md` | Comparação linha a linha entre as duas versões do procedimento de frete especial, identificando 8 inconsistências (numéricas, de governança e contradições internas) e recomendações. | inconsistências, frete especial, versionamento, PROC-042 |
| 4 | Inconsistências entre Documentos Formais e Práticas Informais (FAQ) | `04-FAQ-Inconsistencias-praticas-informais.md` | Cruza os achados formais do discovery com 9 itens do FAQ-Atendimento, identificando 10 inconsistências entre norma e prática de campo. | FAQ, práticas informais, inconsistências, cruzamento |
| 5 | Respostas às Tarefas — Estratégia de Contexto em 3 Etapas | `05-Respostas-Tarefas` | Documenta a estratégia de engenharia de contexto em 3 etapas (visão geral, análise profunda, cruzamento), com riscos identificados e reflexão sobre progressive disclosure. | engenharia de contexto, progressive disclosure, riscos, reflexão |

#### 2.1.2 Exercicio1.2 — Fluxos da Jornada
*Caminho base: `Prática 1\Exercicio1.2\Documentos-Gerados\`*

| # | Documento | Arquivo | Resumo | Tags |
|---|-----------|---------|--------|------|
| 6 | Premissas — Jornada do Atendente com Assistente de IA | `001-Premissas.md` | Registra as premissas iniciais da jornada (canal, interação, base documental, validação, fallback, métricas) que sustentam o desenho dos fluxos seguintes. | premissas, jornada, base documental, fallback |
| 7 | Fluxo Principal — Consulta do Atendente ao Assistente de IA | `002-Fluxo Principal.md` (+ `Fluxo-002-Fluxo Principal.jpg`) | Descreve o caminho feliz da consulta do atendente ao assistente (Given-When-Then + SOP), com guardrails e premissas aplicadas. Fluxograma visual em anexo. | fluxo, caminho feliz, guardrails, SOP |
| 8 | Fluxo de Fallback — Assistente sem Confiança ou Atendente Discorda | `003-Fluxo Fallback.md` (+ `Fluxo-003-Fluxo Fallback.jpg`) | Detalha os três cenários de desvio do fluxo principal (ambiguidade, lacuna, discordância do atendente) e o escalonamento ao supervisor/área especializada. Fluxograma visual em anexo. | fluxo, fallback, escalonamento, guardrails |
| 9 | Fluxo de Feedback — Sinalização e Tratamento de Conteúdo da Base | `004-Fluxo Feedback.md` (+ `Fluxo-004-Fluxo Feedback.jpg`) | Detalha como os três gatilhos do fallback são tratados pelo Responsável pela Base (correção, criação de conteúdo, esclarecimento de fronteira) ou pelo Time Técnico. Fluxograma visual em anexo. | fluxo, feedback, manutenção da base, guardrails |
| 10 | Guardrails — Consolidação e Backlog | `005-Guardrails.md` | Consolida os 3 guardrails já confirmados nos fluxos e lista 5 candidatos de guardrail em backlog, ligados aos gaps do discovery. | guardrails, backlog, consolidação |

#### 2.1.3 Exercicio1.3 — Requisitos do Pipeline de RAG
*Caminho base: `Prática 1\Exercicio1.3\Documentos-Gerados\`*

| # | Documento | Arquivo | Resumo | Tags |
|---|-----------|---------|--------|------|
| 11 | Base de Construção — Especificação de Requisitos do Pipeline de RAG | `001-Base de construção.md` | Registra progressivamente as decisões de contexto/requisitos (5 eixos) que antecederam a redação da especificação final do pipeline de RAG. | requisitos, RAG, decisões, rascunho |
| 12 | Especificação de Requisitos — Pipeline de RAG (NovaTech) | `Especificação de requisitos-Novatech.md` | Especificação não técnica dos requisitos do pipeline de RAG cobrindo fontes de dados, tratamento de contradições, comportamento sem resposta, frescor e rastreabilidade, com critérios de aceite e riscos. | requisitos, RAG, governança de dados, rastreabilidade |

#### 2.1.4 Resumos-Discovery
*Caminho base: `Prática 1\Resumos-Discovery\`*

| # | Documento | Arquivo | Resumo | Tags |
|---|-----------|---------|--------|------|
| 13 | Contexto do Exercício 1.3 — Pipeline de RAG | `Contexto-1.3.md` | Enuncia o contexto e a explicação simplificada do funcionamento de um pipeline de RAG para orientar a especificação de requisitos. | contexto, RAG, briefing |
| 14 | Dados de Discovery (Simulados) | `Dados-Discovery.md` | Traz dados quantitativos simulados sobre fontes consultadas por chamado, temas de dúvida mais comuns e taxa de escalonamento por falta de resposta. | dados, discovery, métricas |
| 15 | Cenário-Âncora — Fase de Entendimento e Contexto | `resumo-Cenário` | Apresenta o cenário de negócio da NovaTech, o problema de atendimento, dados operacionais e os anexos de apoio (documentação simulada e chunks de RAG). | discovery, cenário, contexto de negócio |
| 16 | Resumo — FAQ-Atendimento (RES-005) | `resumo-FAQ-atendimento.md` | Resume o FAQ informal de atendimento (9 de 47 itens), destacando processos, hipóteses, riscos e lacunas frente aos documentos normativos. | FAQ, práticas informais, riscos, resumo |
| 17 | Resumo — POL-001 Política de Devolução (RES-001) | `resumo-POL-001-politica-devolucao.md` | Resume a política de devolução de mercadorias: prazos, exceções, rateio de frete reverso, riscos e lacunas. | política de devolução, frete, SLA interno, resumo |
| 18 | Resumo — PROC-042 Frete Especial v1 (RES-002) | `resumo-PROC-042-frete-especial-v1.md` | Resume a versão 1 do procedimento de cálculo de frete especial, incluindo fórmula, riscos e o achado crítico de coexistência sem vigência formal com a v2. | frete, PROC-042, versionamento, resumo |
| 19 | Resumo — PROC-042 Frete Especial v2 (RES-004) | `resumo-PROC-042-v2-frete-especial-revisado.md` | Resume a versão revisada do procedimento de frete especial, comparando parâmetros com a v1 e destacando a contradição interna sobre vigência. | frete, PROC-042, inconsistências, resumo |
| 20 | Resumo — SLA-2024 Tabela de SLA por Cliente (RES-003) | `resumo-SLA-2024-tabela-sla-clientes.md` | Resume os compromissos de SLA por tier de cliente (tempos de resposta/resolução, penalidades, incidente crítico), com riscos e lacunas de reconciliação. | SLA, tiers de cliente, penalidades, resumo |

#### 2.1.5 Product-Specialist (Prática 1)
*Caminho base: `Prática 1\Product-Specialist\`*

| # | Documento | Arquivo | Resumo | Tags |
|---|-----------|---------|--------|------|
| 21 | Contexto Geral — Fase de Entendimento (Product Specialist) | `z01-ContextoGeral` | Repete o cenário-âncora do projeto NovaTech (tópicos, ferramentas, cenário de negócio e anexos de apoio), como material de contexto geral. | discovery, cenário, contexto de negócio |

---

## 3. Decisões

- **Escopo desta sessão confirmado (pelo usuário):** não vamos escrever `requirements.md` para os 5 módulos. O trabalho é (1) recorte de domínio (bounded contexts, linguagem ubíqua, fronteiras do assistente) e (2) `requirements.md` via SDD de **um único módulo principal**.
- **Módulo principal confirmado:** `query-endpoint`. Motivo: é o módulo mais "produto" (comportamento do assistente — guardrails, fallback, tratamento de contradições), concentra o que já foi construído na Prática 1 (fluxos, guardrails, requisitos de RAG), enquanto `pipeline-ingestao` é predominantemente infraestrutura de dados.
- **Formato do recorte de domínio confirmado:** documento próprio (`domain-model.md`), não uma seção dentro do `requirements.md`. Motivo: é um artefato reutilizável pelas próximas subfases desta fase (Prática 2 deve seguir o mesmo padrão de 3 subfases da Prática 1) e por outros artefatos futuros (AGENTS.md, skills) que precisam falar a mesma linguagem ubíqua.
- **Dicionário de termos (glossário técnico/metodológico) solicitado pelo usuário:** documento à parte, separado da linguagem ubíqua de negócio (que vai no `domain-model.md`). Serve para leitores não técnicos entenderem termos de processo/IA usados nos documentos desta fase (ex: spec, SDD, RAG, MCP). Criado em `dicionario-de-termos.md`.
- **Timing da revisão como Tech Lead (Passo 4):** checkpoint por artefato aprovado, não uma revisão única no final. Motivo: o domain-model.md é a fundação de que os outros artefatos dependem (scope boundaries do requirements.md, coerência do mockup) — uma ambiguidade não detectada cedo se propaga e encarece o retrabalho. Revisão leve a cada entrega aprovada, antes de avançar para o próximo artefato.
- **Escopo da sessão (Exercicio 2.1):** cobre apenas os Passos 1 a 4 do roteiro. Passos 5-7 pertencem a subfases futuras da Prática 2.
- **Estilo/design system do prompt de mockup (Passo 3):** decisão adiada — será definida quando chegarmos ao Passo 3, não agora.
- **Métrica "resposta em até 30 segundos" (revisão pré-Passo 2):** confirmado que mede **latência técnica do `query-endpoint`** (pergunta enviada → resposta retornada pela API), não o tempo de leitura do atendente. Vira critério de verificação testável no `requirements.md`.
- **"Atualização em até 24h" tratado como dependência do `pipeline-ingestao`** (mesmo padrão já aplicado à vigência): o `query-endpoint` assume que os dados indexados estão frescos, não implementa a lógica de atualização — decisão do Product Specialist, a ser exposta no `requirements.md` para validação no checkpoint de Tech Lead.
- **Escalonamento para humano / UI de aviso é responsabilidade do `teams-bot`, fora do escopo do `requirements.md` do `query-endpoint`:** o endpoint apenas retorna dados estruturados (resposta, fonte(s), nível de confiança) — decisão do Product Specialist, a ser exposta no `requirements.md` para validação no checkpoint de Tech Lead.
- **ADR-0003 x spec de RAG resumida — resolvido:** o padrão desta sessão prevalece. Diante de qualquer contradição de fonte, o assistente **sempre mostra ambas as versões** ao atendente (nível de confiança Baixa), independente de a vigência estar definida. A "priorização da mais recente" da ADR-0003 vale só para lógica interna de recuperação, nunca para o que é exibido.
- **Uso da Prática 1 como fonte (revisão pré-Passo 1):** a base para o `domain-model.md` e o `requirements.md` são **exclusivamente os inputs desta sessão** (cenário, spec de RAG resumida, ADRs resumidas, dados de discovery). Cruzamento com os documentos completos da Prática 1 (fluxos, guardrails, spec de RAG de 36K) só acontece caso pontual, quando Claude julgar genuinamente necessário — e nesse caso deve justificar o porquê ao usuário, não usar por padrão.

---

## 4. Dúvidas em Aberto

_(nenhuma até o momento)_

---

## 5. Notas de Refinamento

- **Conteúdo duplicado:** `Resumos-Discovery\resumo-Cenário` (#15) e `Product-Specialist\z01-ContextoGeral` (#21) trazem essencialmente o mesmo cenário-âncora; o segundo é uma versão estendida com tópicos/ferramentas/anexos. Ao consultar o cenário de negócio, usar o #21 como fonte mais completa.
- **Achado crítico já mapeado na Prática 1:** PROC-042 v1 e v2 (#3, #18, #19) coexistem sem vigência formal definida — ponto sensível a considerar na spec (regras de frete especial).
- **Base para a spec de RAG — resolvido:** os documentos #11 e #12 (Exercicio1.3) NÃO serão usados como ponto de partida direto. A spec de RAG resumida fornecida nesta sessão (ver 2.0.1) é a fonte de verdade para este exercício; os documentos completos da Prática 1 só entram pontualmente se necessário (ver Decisões).
- **Decisões de modelagem que ficam a critério do Product Specialist (Claude), a validar no checkpoint de Tech Lead (Passo 4):**
  1. Se o "Assistente/Consulta" (comportamento de busca, guardrails, tratamento de contradição) é um bounded context próprio, ou uma capacidade transversal que orquestra os demais bounded contexts de negócio (Devolução, Frete, SLA).
  2. Como tratar "prazos de entrega" — categoria frequente de pergunta (dados de discovery) sem documento de suporte no Anexo A: registrar como fronteira explícita de fora de escopo (gap conhecido) no domain-model.md/requirements.md, em vez de inventar um bounded context sem fonte.

---

## 6. Roteiro de Execução — Exercicio 2.1

### Entregável final desta subfase
1. Mapa de bounded contexts com linguagem ubíqua (`domain-model.md`)
2. `requirements.md` (módulo `query-endpoint`)
3. Mockup
4. Histórico de iteração

### Passos do roteiro
| Passo | Descrição | Status |
|---|---|---|
| 1 | **Recorte de domínio:** identificar bounded contexts do assistente NovaTech (ex: Atendimento ao Cliente, Gestão Documental, SLAs e Contratos, Logística de Frete) — para cada um, definir o que está dentro, o que está fora, e como se relaciona com os outros. Extrair a linguagem ubíqua do Anexo A (termos que precisam ser usados de forma consistente por humanos e agentes, ex: "carga perigosa" = classes 1-6 ANTT, "frete especial" = acima de 500kg). | 🔵 Em andamento |
| 2 | **requirements.md do query-endpoint** seguindo a estrutura SDD (outcomes, scope boundaries, constraints, prior decisions, verification criteria). *Prior decisions* referenciam as ADRs da fase anterior (simuladas no contexto — ADR-0001 a ADR-0004, já registradas na seção 1 deste arquivo). *Scope boundaries* derivam dos bounded contexts definidos no Passo 1. | ⏳ Pendente |
| 3 | **Mockup da interface de resposta no Teams**, coerente com o requirements.md. Formato de entrega: um **prompt** para o usuário copiar e colar no Claude Design (a IA de geração gráfica). | ✅ Aprovado — `mockup.jpg` gerado e aceito pelo usuário |
| 4 | **Claude atua como Tech Lead**: aponta ambiguidades nos artefatos produzidos e discute ajustes junto com o usuário. Gera, como subproduto, o **histórico de iteração** (4º entregável desta subfase). | ✅ Concluído — `historico-iteracao.md` |
| 5-7 | Pertencem a **subfases futuras** (fora do escopo do Exercicio 2.1) — usuário confirmou que esta sessão vai até o Passo 4. | 🔮 Fora de escopo desta sessão |

> **Escopo desta subfase confirmado:** Exercicio 2.1 cobre apenas os Passos 1 a 4. Os passos 5-7 (ainda não detalhados) ficam para as próximas subfases da Prática 2.
>
> Decisão de processo: os passos serão recebidos e executados **um de cada vez** (não adiantados em lote), para manter o foco e reduzir risco de aplicar informação fora de contexto/ordem. Este roteiro é atualizado a cada novo passo recebido.

### Referência encontrada para o Passo 3 (prompts de Claude Design)
Localizada em `Prática 1\Exercicio1.2\Prompts-Histórico\`:
- **`Claude.md`** — histórico da sessão que gerou os prompts de diagrama da Prática 1 (contexto, decisões, aprendizados).
- **`claude design.md`** — os prompts efetivamente usados no Claude Design para os 3 fluxogramas (Principal, Fallback, Feedback).

Padrão identificado nesses prompts (candidato a reaproveitar no mockup do Teams, a confirmar com o usuário):
- **Design system usado:** "Modernist".
- Prompts escritos em **linguagem natural portátil**, com instrução explícita de "gerar com o conteúdo exato, sem adicionar/remover/inferir etapas" — evita que a IA de design invente conteúdo.
- Processo iterativo esperado: primeiro prompt raramente sai perfeito: no histórico anterior houve *ajustes pontuais* (reposicionamento, formatação, elementos faltando) em vez de regenerar do zero — relevante para o Passo 4 (histórico de iteração) e para o próprio uso do Claude Design no Passo 3.
- Guardrails de negócio foram representados visualmente como anotações (Text Annotations) ligadas ao ponto exato onde a regra se aplica — pode ser um padrão útil também no mockup do Teams (ex: anotar visualmente a citação de fonte ou o aviso de contradição).

---

## 7. Histórico de Iteração — `domain-model.md` (alimenta o entregável final "histórico de iteração")

| Rodada | Feedback do usuário | Ajuste aplicado |
|---|---|---|
| 1 | Diagrama ASCII da seção 2 (Mapa de Bounded Contexts) ficou desalinhado/ilegível. | Substituído por descrição textual das relações entre os 5 bounded contexts. |
| 1 | Faltava definir o comportamento do assistente quando a pergunta está fora do contexto de resposta dele. | Adicionada a subseção "Comportamento diante de pergunta fora do escopo" (seção 4), com 3 casos: gap conhecido, fora do domínio, sem chunk relevante. |
| 1 | Faltava um guardrail explícito limitando o assistente à base documental, sem inventar. | Adicionado guardrail "Restrição à base documental" (seção 4) e o termo correspondente na linguagem ubíqua (seção 3). |
| 2 (checkpoint Tech Lead) | Governança Documental cruza `pipeline-ingestao` e `query-endpoint` sem essa divisão estar declarada. | Seção 2.5 passou a explicitar a divisão de responsabilidade por módulo, com nota de dependência assumida pelo `query-endpoint`. |
| 2 (checkpoint Tech Lead) | Uso do FAQ informal como fonte para gaps conhecidos conflitava com a "armadilha" já documentada no Anexo B. | Decisão: FAQ não é usado como fonte de resposta neste momento — marcado como revisável (seção 4). |
| 2 (checkpoint Tech Lead) | "Sinalizar confiança" não tinha definição operacional. | Padronizado como Alta/Média/Baixa, com critério de atribuição, na linguagem ubíqua (seção 3). |
| 2 (checkpoint Tech Lead) | Idioma de resposta e comportamento multi-turn estavam implícitos, não escritos (princípio "o óbvio deve ser dito"). | Explicitados na seção 4: resposta sempre em português; perguntas de acompanhamento usam o histórico de 3 turnos (ADR-0002). |

**Resultado do checkpoint:** `domain-model.md` aprovado (v0.2) para avançar ao Passo 2.

---

## 8. Histórico de Iteração — `requirements.md`

| Rodada | Feedback do usuário | Ajuste aplicado |
|---|---|---|
| 1 | Pedido de lista completa das decisões autônomas antes de seguir (ver princípio "toda decisão deve passar pelo usuário", seção 1). | Claude listou 6 categorias de decisão (A-F) para aprovação item a item, em vez de seguir com o rascunho como pacote fechado. |
| 1 | B — Tabela "fora do escopo" (seção 2): aceita, mas marcar como base de trabalho a revisar no momento oportuno. | Nota adicionada logo após a tabela. |
| 1 | C — Formato de resposta da API e escalonamento via `teams-bot` (seção 3, "De integração"): aceitos, mas marcar como base de trabalho a revisar no momento oportuno. | Nota adicionada antes dos dois bullets. |
| 1 | C — Frescor de dados (seção 3, "Técnicas"): trocar o enquadramento de "assumido/dependência" por "prazo padrão da política". | Reescrito como "mantida dentro do prazo padrão da política — até 24h". |
| 1 | D — Reconciliação ADR-0003 x spec de RAG resumida: usuário pediu explicação mais detalhada (3 opções com trade-offs) antes de decidir. | Claude apresentou Opções A/B/C com efeito prático no caso PROC-042; usuário escolheu **Opção C — o padrão desta sessão prevalece sempre**. |
| 1 | E (verification criteria) e F (linha de aprovação): mantidos sem alteração. | Nenhum ajuste necessário. |

**Decisão de reconciliação registrada (seção 4 do requirements.md):** diante de qualquer contradição de fonte, o assistente sempre mostra ambas as versões — a instrução de "priorizar a mais recente" da ADR-0003 passa a valer só para recuperação interna, nunca para o que é exibido ao atendente.

| Rodada | Feedback (checkpoint Tech Lead) | Ajuste aplicado |
|---|---|---|
| 2 | `domain-model.md` §2.5 ainda tinha a lógica antiga (Opção A) de vigência decidindo exibição — inconsistente com a Opção C escolhida. | Corrigido em `domain-model.md` v0.3: vigência só para uso interno, nunca decide o que é exibido. |
| 2 | `domain-model.md` §4, gaps "carga danificada" e "seguro de carga" ainda citavam o FAQ como fonte possível — desatualizado desde a decisão de não usar FAQ (v0.2). | Corrigido em `domain-model.md` v0.3: FAQ não é citado em nenhum gap. |
| 2 | `requirements.md` §2/§3 ainda descreviam a vigência como decisora do comportamento de contradição. | Corrigido em `requirements.md` v0.2. |
| 2 | VC-03/VC-04 eram condicionais ("se a v1 também for recuperada") — não garantia de fato a promessa da seção 4. | Usuário decidiu **garantir recuperação de todas as versões conhecidas** para temas com contradição — novo requisito em §2/§3 e novo critério VC-11 em `requirements.md` v0.2. |

**Resultado do checkpoint:** `requirements.md` aprovado (v0.2) para avançar ao Passo 3. `domain-model.md` atualizado para v0.3 por coerência cruzada.

| Rodada | Revisão extra (lente de QA, a pedido do usuário) | Ajuste aplicado |
|---|---|---|
| 3 | Faltava VC para "pergunta fora do domínio" e "sem chunk relevante" (2 dos 3 comportamentos do domain-model.md §4 sem teste). | Adicionados VC-12 e VC-13 (nova seção 5.5). |
| 3 | Faltava um teste de vazamento de conhecimento geral com pergunta trivial/inofensiva (não só de negócio). | Adicionado VC-14. |
| 3 | VC-05 a VC-08 sem coluna de nível de confiança esperado (inconsistente com as demais seções). | Coluna adicionada — Alta para VC-05/06, N/A (recusa) para VC-07/08. |
| 3 | VC-10 (latência) sem metodologia de medição. | Definido p95 < 30s, mínimo 20 execuções, nota sobre volume/concorrência não definidos nesta sessão. |
| 3 | VC-11 pode ter custo de implementação não trivial (mecanismo além de similaridade simples) — risco não sinalizado. | Nota de risco técnico adicionada, para validação futura de um Dev Sênior antes do `plan.md`. |

**Resultado da revisão QA:** `requirements.md` avança para v0.3, com 14 critérios de verificação (VC-01 a VC-14).

---

## 9. Histórico de Iteração — `mockup.jpg` (Claude Design)

| Rodada | Revisão (Product Specialist) | Resultado |
|---|---|---|
| 1 | Resultado do Claude Design comparado ao `mockup-prompt-claude-design.md` e ao `requirements.md`. Fidelidade alta: borda/badge de confiança, tabela comparativa com os 3 valores corretos, fonte citada, 3 botões de ação, anotação do botão de bloqueio — todos presentes conforme especificado. | Sem correção estrutural necessária. |
| 1 | Encontrado detalhe: rodapé de fontes usa "PROC-042" (sem "v1" explícito), enquanto a tabela usa "PROC-042 v1" — pequena inconsistência de rótulo entre duas partes do mesmo card. | Usuário decidiu **manter como está** — aprovado sem esse ajuste. Registrado aqui para não se perder, caso vire relevante numa iteração visual futura. |

**Resultado:** `mockup.jpg` aprovado como está (v1, sem ajuste do rótulo de fonte).
