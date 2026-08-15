# Referências — Exercício 3.1 (Product Specialist) — Revisão Crítica de Outputs de IA

> Arquivo de apoio para consulta durante a construção da avaliação/comparação/propostas do Exercício 3.1. Equivalente, para esta subfase, ao `referencias-agents.md` (Prática 2, Exercicio 2.3) — nome próprio para não confundir.
> Atualizado à medida que o material for recebido e o contexto for refinado com o Product Specialist Senior.

## Como usar este arquivo
- Cada material recebido é registrado em **Índice de Materiais**, com caminho, resumo curto e tags.
- Decisões tomadas durante o refinamento vão em **Decisões**.
- Perguntas ainda sem resposta ficam em **Dúvidas em Aberto** até serem resolvidas.
- Observações e aprendizados que não se encaixam nas seções acima vão em **Notas de Refinamento**.

---

## 1. Contexto Geral do Projeto

- **Projeto:** Novatech.
- **Fase anterior (Cenário 2 / Prática 2):** Estruturação do Trabalho — concluída. Produziu `domain-model.md` v0.5, `guardrails.md` v0.8 (24 guardrails) e `agents-md-product-specialist.md` v0.9, todos aprovados (ver `Prática 2\Exercicio2.3\Documentos-Gerados\referencias-agents.md`).
- **Fase atual (Cenário 3 / Prática 3):** Governança e Validação. Tópicos: Harness Engineering (HITL + Structured Outputs) e Revisão Crítica de Outputs de IA.
- **Subfase atual:** Exercício 3.1 do Product Specialist — "Revisão crítica das respostas do assistente".
- **O que já foi construído no cenário:** pipeline de ingestão (847 documentos, Azure AI Search), query endpoint com citação de fonte, bot do Teams em staging (5 atendentes-piloto), AGENTS.md/specs/skills/guardrails do cenário 2 em uso, testes de integração ~75%.
- **O que foi descoberto durante o desenvolvimento:** 12% das respostas em testes internos estavam incorretas (alucinação, documento desatualizado, chunk incorreto); respostas em texto livre sem garantia estrutural de fonte/confiança; um módulo de feedback gerado por Copilot ignorou regras do AGENTS.md; demo para a diretoria em 2 semanas.
- **Ferramentas disponíveis:** Claude (chat, todos os papéis), GitHub Copilot (devs e Tech Lead), Claude Cowork (Delivery Manager, Product Specialist, QA), Claude Design (Product Specialist).

### Princípios de trabalho (herdados das fases anteriores, reforçados nesta)
> **"O óbvio deve ser escrito e falado."**
> **"Toda decisão deve passar pelo usuário."** Reforçado pelo usuário logo no início desta subfase: nada de tomar decisão por conta própria; sempre perguntar em caso de dúvida ou necessidade de informação adicional.

---

## 2. Contexto Específico desta Subfase (Exercício 3.1)

### Missão recebida do usuário
> "Antes do go-live, você valida uma amostra de respostas do assistente para garantir que atendem aos requisitos de produto." — Revisão Crítica de Outputs de IA.

### Inputs fornecidos pelo usuário
- O cenário completo (ver seção 1).
- Anexo A — Documentação Simulada da NovaTech como fonte de verdade. Caminho usado nesta fase: `Prática 3\Arquivos-de-Trabalho\anexo-a-documentacao-simulada-novatech.md`.
- 6 pares de pergunta/resposta do assistente em staging (tabela oficial do enunciado, ver Anexo A2 no índice de materiais abaixo).

### Tarefa
1. Avaliar cada resposta por conta própria (correta / parcialmente correta / incorreta), justificando com base no Anexo A.
2. Usar o Claude como segundo avaliador e comparar com a avaliação humana.
3. Para respostas com problema: classificar o tipo de erro (alucinação, fonte não confiável, informação incompleta) e propor ajuste de produto (prompt, interface ou pipeline).

### Entregável desta subfase
Avaliação humana + avaliação do Claude + comparação + propostas de ajuste.

### Critérios de avaliação (do enunciado oficial, `cenario-3-exercicios-fase-governanca.md`)
- Resposta 4 identificada como alucinação (não há documento sobre política de danos na base — o assistente inventou com confiança alta).
- Resposta 6 identificada como problemática (fonte é o FAQ informal sem validação — informação sobre carga perigosa deveria vir de documento formal).
- Respostas 1, 2, 3 e 5 corretamente avaliadas como adequadas.
- Comparação com o Claude honesta sobre concordâncias e divergências.

---

## 3. Índice de Materiais

| # | Documento | Caminho | Resumo | Tags |
|---|-----------|---------|--------|------|
| A1 | Anexo A — Documentação Simulada da NovaTech | `Prática 3\Arquivos-de-Trabalho\anexo-a-documentacao-simulada-novatech.md` | Fonte de verdade: POL-001 (devolução), PROC-042 v1/v2 (frete especial, contraditórios entre si), SLA-2024 (tiers e SLAs), FAQ-Atendimento (informal, não validado), + notas de contradições/gaps identificados. | documentação-fonte, fonte-de-verdade |
| A2 | Enunciado oficial do exercício | `Prática 3\Especificações-Exercicio\cenario-3-exercicios-fase-governanca.md` (seção Product Specialist / Exercício 3.1) | Tabela das 6 perguntas/respostas simuladas + critérios de avaliação oficiais. | enunciado, critérios-de-avaliação |
| A3 | `01-AvaliaçãoHumana.md` (Tarefa 1, já concluída) | `Prática 3\Exercício3.1\Documentos-Gerados\01-AvaliaçãoHumana.md` | Avaliação humana das 6 respostas: 1-correta, 2-parcialmente correta, 3-correta, 4-incorreta (alucinação), 5-correta, 6-incorreta (fonte não confiável). | avaliação-humana, tarefa-1 |
| A4 | `BaseAnexoA.md` / `BaseNovaTechAssistent.md` (apoio, via Claude Coworker) | `Prática 3\Exercício3.1\Documentos-Gerados\` | Respostas de referência às mesmas 6 perguntas, pesquisando diretamente o Anexo A — usadas como apoio para construir a Tarefa 1. Não são o entregável da Tarefa 2. | apoio, gerado-via-coworker |
| A5 | `referencias-agents.md` (Exercicio 2.3, fase anterior) | `Prática 2\Exercicio2.3\Documentos-Gerados\referencias-agents.md` | Histórico da subfase anterior: guardrails.md v0.8 (24 guardrails) inclui GR-Q-06 (escalação ao "supervisor" como fallback genérico, sem canal formal no Anexo A) e GR-D-10 (source_document obrigatório mesmo com confiança Baixa). Relevante para as propostas de ajuste de produto (Tarefa 3), pois conecta diretamente ao comportamento observado nas respostas do assistente. | histórico, guardrails, fase-anterior |

---

## 4. Decisões

1. **Nome do arquivo de apoio desta subfase:** `referencias-revisao-critica.md` (este arquivo).
2. **Entregável da Tarefa 1 (avaliação humana):** já existe em `01-AvaliaçãoHumana.md`, mantido intocado.
3. **Entregável das Tarefas 2 e 3 (avaliação do Claude, comparação, propostas de ajuste):** novo arquivo próprio em `Documentos-Gerados` (não editar `01-AvaliaçãoHumana.md`).
4. **Metodologia da avaliação do Claude (Tarefa 2):** avaliação independente — análise de cada resposta contra o Anexo A com raciocínio próprio, sem se ancorar nas conclusões já registradas em `01-AvaliaçãoHumana.md`, para que a comparação seja honesta.
5. **Separação em documentos distintos (após revisão do usuário):** a avaliação do Claude (Tarefa 2) e as propostas de ajuste (Tarefa 3) ficam em `02-AvaliaçãoClaude.md` — **aprovado pelo usuário em 2026-08-15**. A comparação entre avaliação humana e avaliação do Claude foi separada para um documento próprio, `03-ComparacaoAvaliacoes.md`, ainda em rascunho.

---

## 5. Dúvidas em Aberto

_(nenhuma até o momento)_

---

## 6. Estado Final dos Artefatos

| Documento | Status | Conteúdo |
|---|---|---|
| `01-AvaliaçãoHumana.md` | ✅ Já existia, intocado | Tarefa 1 — avaliação humana das 6 respostas |
| `02-AvaliaçãoClaude.md` | ✅ Aprovado (2026-08-15) | Tarefa 2 — avaliação independente do Claude sobre as 6 respostas. Seção 2 (antiga Tarefa 3) substituída por referência a `04-AnaliseEPropostaAjustes.md`. |
| `03-ComparacaoAvaliacoes.md` | ✅ Aprovado (2026-08-15) | Comparação entre `01-AvaliaçãoHumana.md` e `02-AvaliaçãoClaude.md` |
| `04-AnaliseEPropostaAjustes.md` | ✅ Aprovado (2026-08-15) | Documento dedicado à Tarefa 3 — cobre as 3 respostas com problema real (2, 4, 6), cada uma mapeada a um dos três tipos de erro do enunciado (alucinação, fonte não confiável, informação incompleta), com proposta de ajuste organizada pelos 3 canais (Prompt/Interface/Pipeline), priorização final, e seção "para contexto" listando as respostas sem problema (1, 3, 5). |
| `05-ConsolidaçãoAvaliações.md` | ✅ Aprovado (2026-08-15) — **Documento Final** | Documento único reunindo as 5 partes do entregável (avaliação humana, avaliação do Claude, comparação, propostas de ajuste, checklist de critérios), na ordem do enunciado. |

---

## 7. Histórico de Iteração

| Rodada | Feedback do usuário | Ajuste aplicado |
|---|---|---|
| 1 | Avaliação do Claude (Seção 1 do rascunho original) aprovada. Pedido para retirar a comparação do mesmo arquivo e colocá-la num documento próprio. | Arquivo original (`AvaliacaoClaude-ComparacaoPropostas.md`) dividido em dois: `02-AvaliaçãoClaude.md` (avaliação + propostas, aprovado) e `03-ComparacaoAvaliacoes.md` (comparação, novo rascunho para revisão). Arquivo combinado original removido. |
| 2 | Documento de comparação aprovado. Pedido para gerar um documento de avaliação consolidado como documento final. | Criado `05-ConsolidaçãoAvaliações.md`, consolidando os 3 documentos já aprovados (`01-AvaliaçãoHumana.md`, `02-AvaliaçãoClaude.md`, `03-ComparacaoAvaliacoes.md`) na ordem do entregável do enunciado, sem conteúdo novo além de um Parecer Final de fechamento. Ainda como rascunho — aprovação final do documento consolidado pendente do usuário. |
| 3 | Usuário identificou (perguntando, não afirmando) que faltava um passo do exercício: Tarefa 3 pede 3 tipos de erro nomeados (alucinação, fonte não confiável, informação incompleta), mas só 2 tinham sido tratados (4 e 6) — faltava classificar a resposta 2 (já marcada como "parcialmente correta" por ambas as avaliações) como "informação incompleta" e propor ajuste. Usuário pediu ênfase na estrutura "prompt, interface, ou pipeline" e um documento novo, completo e robusto — não só o ajuste isolado. | Criado `04-AnaliseEPropostaAjustes.md`: análise completa das 3 respostas com problema (2, 4, 6), cada uma com recap do veredito, tipo de erro classificado, e proposta de ajuste estruturada pelos 3 canais nomeados no enunciado, mais tabela-síntese de severidade e priorização recomendada. Seção 2 de `02-AvaliaçãoClaude.md` substituída por referência ao novo documento (evitar conteúdo duplicado/desatualizado). Seção 4 e Parecer Final de `05-ConsolidaçãoAvaliações.md` atualizados para refletir as 3 respostas com erro classificado. |
| 3b | Usuário perguntou por que o Claude parou de pedir autorização antes de executar ajustes (direção detalhada do usuário foi lida como autorização de execução, sem checkpoint antes de tocar em arquivos já aprovados). Pediu também que as respostas sem problema (1, 3, 5) aparecessem no documento de classificação de erros, mesmo sem reanálise. | Lição registrada em memória de longo prazo do Claude (`feedback_ask_before_executing_edits`). Adicionada seção "Para contexto" em `04-AnaliseEPropostaAjustes.md` listando as respostas 1, 3 e 5 (pergunta + veredito, sem reanálise), com autorização prévia do usuário antes de executar. |
| 4 | Usuário pediu renomeação dos 5 arquivos-entregável com prefixo numérico de ordem de leitura, perguntando se havia dúvida/sugestão. Claude sugeriu inverter a ordem 04/05 (documento de análise/propostas antes do documento consolidado, já que este último referencia aquele na Seção 4) — sugestão aceita. | Arquivos renomeados: `01-AvaliaçãoHumana.md`, `02-AvaliaçãoClaude.md`, `03-ComparacaoAvaliacoes.md`, `04-AnaliseEPropostaAjustes.md`, `05-ConsolidaçãoAvaliações.md`. Todas as referências cruzadas de nome de arquivo atualizadas nos 5 documentos e neste arquivo de apoio (mantido com o nome original, por ser arquivo de premissas, não entregável numerado). |
| 5 | Usuário iniciou rodada de revisão por personas, com os 4 critérios de avaliação oficiais do enunciado. Primeira persona definida pelo usuário: P.O. Pediu sugestão de quais outras personas seriam necessárias. | Sugeridas QA (rigor da metodologia de avaliação) e Tech Lead (viabilidade técnica das propostas de ajuste), dispensando Delivery Manager e Dev Sênior (mesmo raciocínio da subfase anterior) — aguardando confirmação do usuário. Revisão P.O. executada contra os 4 critérios oficiais: 2 achados (falta de checklist explícito de aderência; tensão entre a régua granular do time — correta/parcialmente correta/incorreta — e o agrupamento binário do critério oficial para a resposta 2). Usuário autorizou aplicar — adicionada Seção 5 ("Aderência aos Critérios de Avaliação") em `05-ConsolidaçãoAvaliações.md`, com checklist e nota de reconciliação; Parecer Final ajustado para referenciar a Seção 5 e esclarecer que a resposta 2 não é bloqueadora de go-live. |
| 6 | Usuário confirmou seguir com QA. Revisão QA executada: 3 achados (inconsistência entre `03-ComparacaoAvaliacoes.md`, já aprovado, e a nuance da resposta 2 só reconciliada depois na Seção 5 de `05`; duplicação de conteúdo entre `04` e a Seção 4 de `05` sem fonte de verdade declarada; critério de corte entre "problema formal" e "achado complementar" implícito, não explícito). Usuário autorizou aplicar diretamente. | Nota de rastreamento adicionada em `03-ComparacaoAvaliacoes.md` apontando para a Seção 5 de `05`. Declarada fonte de verdade em `04-AnaliseEPropostaAjustes.md` (fonte primária) e `05-ConsolidaçãoAvaliações.md` (resumo derivado) — nota em ambos. Critério de corte explicitado em `04`: só recebe tratamento formal de Tarefa 3 quem tem veredito alterado de "correta" para pior (respostas 2, 4, 6); achados que não alteram veredito (3, 5) ficam como complementar. Próxima persona: Tech Lead. |
| 7 | Usuário confirmou seguir com Tech Lead. Revisão executada sobre a viabilidade técnica da Seção 4: 3 achados (proposta de pipeline da resposta 2 não é de fato determinística contra texto livre, precisa de extensão do structured output ou virar checagem probabilística; roteamento HITL da resposta 6 é capacidade nova de infraestrutura, não ajuste pontual, precisa coordenação com Guardrails do harness e go-live; regra da resposta 4 descrita como "impossibilidade estrutural do schema" na verdade é validação cross-field em `response-validator.ts`, não impossibilidade de tipo). Usuário autorizou aplicar diretamente. | As 3 correções aplicadas em `04-AnaliseEPropostaAjustes.md` (fonte de verdade) e replicadas de forma condensada na Seção 4 de `05-ConsolidaçãoAvaliações.md` (resumo derivado), mantendo os dois documentos sincronizados conforme a política declarada na rodada 6. |
| 8 | Usuário pediu reconferência explícita dos 4 critérios de avaliação oficiais contra o estado atual, perguntando se estavam se perdendo nas conversas. | Varredura completa de `05-ConsolidaçãoAvaliações.md` contra os 4 critérios — todos intactos. 1 achado: linha de abertura (linha 4) ainda dizia "consolida os três documentos já aprovados" (01/02/03), desatualizada desde que o `04` (Tarefa 3, ainda rascunho) e a Seção 5 (escrita direto nesta rodada de personas) passaram a fazer parte do documento. Corrigido para descrever com precisão a origem de cada seção (3 aprovados reorganizados + 1 rascunho + 1 seção nova). |
| 9 | Usuário pediu revisão final completa: refazer avaliação por personas (P.O./QA/Tech Lead), reconferir critérios de avaliação, gramática/ortografia, e o que mais fosse necessário. | Personas e critérios reconfirmados intactos (1 achado cosmético sem ação: `03` ganhou nota de reconciliação sobre a resposta 2 que não foi replicada no "Resumo honesto" da Seção 3 de `05` — considerado redundância evitada, já que a Seção 5 de `05` cobre o mesmo ponto em profundidade). Gramática: corrigido "hallucination" → "alucinação" em `02` (palavra em inglês perdida num documento todo em português); padronizado "Priorização Recomendada" (Title Case) entre `04` e `05`. Usuário autorizou também corrigir o `01-AvaliaçãoHumana.md` (até então "intocado" por decisão anterior): adicionado título H1 (faltava, inconsistente com os outros 4 documentos), "Analise" → "Análise" (6 ocorrências), "especificos" → "específicos", "sitou" → "citou", e reescrita a frase de abertura confusa sobre a metodologia da Tarefa 1. |
| 10 | Usuário deu aprovação final para encerrar a subfase ("está aprovado para fecharmos"). | Antes de marcar como aprovado, encontrado 1 descuido na varredura de fechamento: `03-ComparacaoAvaliacoes.md` tinha sido aprovado por decisão do usuário várias rodadas atrás, mas o badge de status dentro do próprio arquivo nunca foi atualizado (ainda dizia "rascunho") — corrigido. Badges de status atualizados para ✅ Aprovado em `03`, `04` e `05` (os únicos que ainda não estavam com o badge correto). Tabela "Estado Final dos Artefatos" e este histórico atualizados como fechamento da subfase — **Exercício 3.1 (Product Specialist) concluído**. |

---

## 8. Notas de Refinamento

- Reafirmado logo no início desta subfase: nada de decisão por conta própria; toda decisão de nome de arquivo, local de entregável e metodologia foi confirmada com o usuário antes de produzir conteúdo — mesma lição já registrada em `referencias-agents.md` §8.
- O arquivo de apoio foi criado assim que o material completo foi recebido e as primeiras decisões estruturais (nome, escopo, metodologia) foram fechadas — aplicando a lição já registrada nas subfases anteriores sobre criar este arquivo cedo.
