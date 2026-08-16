# Referências — Exercício 3.2 (Product Specialist) — Harness de Produto para Melhoria Contínua

> Arquivo de apoio para consulta durante a construção do harness de produto. Equivalente, para esta subfase, ao `referencias-revisao-critica.md` (Prática 3, Exercício 3.1) e ao `referencias-agents.md` (Prática 2, Exercicio 2.3) — nome próprio para não confundir.
> Atualizado à medida que o material for recebido e o contexto for refinado com o Product Specialist Senior.

## Como usar este arquivo
- Cada material recebido é registrado em **Índice de Materiais**, com caminho, resumo curto e tags.
- Decisões tomadas durante o refinamento vão em **Decisões**.
- Perguntas ainda sem resposta ficam em **Dúvidas em Aberto** até serem resolvidas.
- Observações e aprendizados que não se encaixam nas seções acima vão em **Notas de Refinamento**.

---

## 1. Contexto Geral do Projeto

- **Projeto:** Novatech.
- **Fase anterior (Cenário 2 / Prática 2):** Estruturação do Trabalho — concluída. Produziu `domain-model.md` v0.5, `guardrails.md` v0.8 (24 guardrails: 10 DEVE, 8 NÃO DEVE, 6 QUANDO EM DÚVIDA) e `agents-md-product-specialist.md` v0.9, todos aprovados (ver `Prática 2\Exercicio2.3\Documentos-Gerados\referencias-agents.md`).
- **Subfase anterior (Cenário 3 / Exercício 3.1, Product Specialist):** Revisão crítica das respostas do assistente — concluída e aprovada em 2026-08-15 (ver `Prática 3\Exercício3.1\Documentos-Gerados\referencias-revisao-critica.md`).
- **Fase atual (Cenário 3 / Prática 3):** Governança e Validação. Tópicos: Harness Engineering (HITL + Structured Outputs) e Revisão Crítica de Outputs de IA.
- **Subfase atual:** Exercício 3.2 do Product Specialist — "Harness de produto para melhoria contínua".
- **O que já foi construído no cenário:** pipeline de ingestão (847 documentos, Azure AI Search), query endpoint com citação de fonte, bot do Teams em staging (5 atendentes-piloto), AGENTS.md/specs/skills/guardrails do cenário 2 em uso, testes de integração ~75%.
- **O que foi descoberto durante o desenvolvimento:** 12% das respostas em testes internos estavam incorretas (alucinação, documento desatualizado, chunk incorreto); respostas em texto livre sem garantia estrutural de fonte/confiança; um módulo de feedback gerado por Copilot ignorou regras do AGENTS.md; demo para a diretoria em 2 semanas.
- **Ferramentas disponíveis:** Claude (chat, todos os papéis), GitHub Copilot (devs e Tech Lead), Claude Cowork (Delivery Manager, Product Specialist, QA), Claude Design (Product Specialist).
- **Ferramenta a usar nesta subfase:** apenas Claude (chat) — conforme enunciado oficial.

### Princípios de trabalho (herdados das fases anteriores, reforçados nesta)
> **"O óbvio deve ser escrito e falado."**
> **"Toda decisão deve passar pelo usuário."** Reforçado pelo usuário logo no início desta subfase: nada de tomar decisão por conta própria; sempre perguntar em caso de dúvida ou necessidade de informação adicional.

---

## 2. Contexto Específico desta Subfase (Exercício 3.2)

### Missão recebida do usuário
> "O assistente vai evoluir após o go-live. Você define, do ponto de vista de produto, como garantir que ele melhore sem degradar." — Harness Engineering.

### Inputs fornecidos pelo usuário
- O cenário completo (ver seção 1).
- O conceito de harness de produto: *"Define quais métricas de qualidade são monitoradas, como o feedback de usuários é processado, e como mudanças no assistente são validadas antes de ir a produção (regression testing de produto)."*
- Os guardrails formalizados no cenário 2 (DEVE / NÃO DEVE / QUANDO EM DÚVIDA), que o harness deve preservar ao longo da evolução — `guardrails.md` v0.8, `Prática 2\Exercicio2.3\Documentos-Gerados\guardrails.md`, 24 guardrails.

### Tarefa
Projetar um harness de produto que cubra:
1. **Processo de feedback:** como o feedback do atendente vira melhoria (novo documento? ajuste de prompt? reindexação?).
2. **Regression testing de produto:** antes de mudar o prompt ou adicionar documentos, como verificar que as respostas existentes não pioraram E que os guardrails do cenário 2 continuam sendo respeitados.
3. **Ponto de human-in-the-loop:** quais mudanças no assistente exigem aprovação humana antes de ir a produção, e quem aprova.

### Entregável desta subfase
O documento do harness de produto.

### Critérios de avaliação (do enunciado oficial, `cenario-3-exercicios-fase-governanca.md`)
- O processo de feedback é completo (do atendente até a melhoria efetiva).
- O regression testing reconhece que mudanças em IA podem ter efeitos colaterais e verifica que os guardrails não regridem.
- O ponto de HITL é concreto (define o que precisa de aprovação humana e quem aprova).

---

## 3. Índice de Materiais

| # | Documento | Caminho | Resumo | Tags |
|---|-----------|---------|--------|------|
| A1 | Enunciado oficial do exercício | `Prática 3\Especificações-Exercicio\cenario-3-exercicios-fase-governanca.md` (seção Product Specialist / Exercício 3.2) | Tarefa e critérios de avaliação oficiais do harness de produto. | enunciado, critérios-de-avaliação |
| A2 | `guardrails.md` v0.8 (Exercicio 2.3, fase anterior) | `Prática 2\Exercicio2.3\Documentos-Gerados\guardrails.md` | 24 guardrails (10 DEVE, 8 NÃO DEVE, 6 QUANDO EM DÚVIDA), cada um com Enforcement (Código/Prompt/Híbrido), Prioridade (Crítico/Importante/Desejável) e Status (Vigente/Revisável). Fonte de verdade do que o harness de regression testing precisa preservar. | guardrails, fase-anterior, fonte-de-verdade |
| A3 | `05-ConsolidaçãoAvaliações.md` (Exercício 3.1, fase anterior imediata) | `Prática 3\Exercício3.1\Documentos-Gerados\05-ConsolidaçãoAvaliações.md` | Avaliação das 6 respostas do assistente em staging; 3 respostas com problema classificado (alucinação, fonte não confiável, informação incompleta) e propostas de ajuste por canal (Prompt/Interface/Pipeline). Relevante como exemplo concreto do tipo de regressão que o harness precisa pegar antes de produção. | histórico, avaliação, fase-anterior |
| A4 | `z00-ProductSpecialist.md` | `Prática 3\Product-Specialist\z00-ProductSpecialist.md` | Enunciado dos dois exercícios do Product Specialist nesta fase (3.1 e 3.2), cópia local do enunciado oficial. | enunciado |

---

## 4. Decisões

1. **Nome do arquivo de apoio desta subfase:** `referencias-harness-produto.md` (este arquivo).
2. **Padrão de mercado adotado como base:** harness "eval-driven deployment" (padrão LLMOps) — golden dataset + guardrails como assertions + aprovação humana por nível de risco — reaproveitando os insumos já existentes do projeto (guardrails.md v0.8 com Enforcement/Prioridade já classificados, e as respostas avaliadas no Exercício 3.1) em vez de propor uma plataforma de eval nova. Decisão do usuário: "não é hora de reinventarmos a roda".
3. **Estrutura do documento do harness (aprovada pelo usuário):** 5 seções — Objetivo, Processo de Feedback, Regression Testing de Produto, Ponto de HITL, Fluxo consolidado.
4. **Aprovador de HITL para mudanças que tocam guardrail Crítico:** Product Specialist + Tech Lead, em **revisão paralela** (não sequencial), com SLA curto (24h) para não virar gargalo — escopo restrito a guardrails de Prioridade Crítico (a maioria das mudanças do dia a dia toca guardrail Importante/Desejável ou nenhum, e usa aprovador único/fast-path). Usuário levantou preocupação com lentidão do par; decisão foi manter o par mas mitigar com escopo estreito + paralelismo + SLA, em vez de reduzir para aprovador único.
5. **Rollback:** o documento do harness fica **autocontido** — não referencia o plano de rollback do Delivery Manager (Exercício 3.1 do DM), por ser artefato de outro papel, fora do escopo direto desta subfase.

---

## 5. Dúvidas em Aberto

_(nenhuma até o momento)_

---

## 6. Estado Final dos Artefatos

| Documento | Status | Conteúdo |
|---|---|---|
| `HarnessProduto.md` | ✅ Aprovado (v0.6) — 2026-08-15 | Entregável único da subfase — Objetivo, Processo de Feedback, Regression Testing de Produto, Ponto de HITL, Faseamento, Fluxo Consolidado. |

---

## 7. Histórico de Iteração

| Rodada | Feedback do usuário | Ajuste aplicado |
|---|---|---|
| 1 | Usuário perguntou qual o padrão mais usual de mercado e o ideal para o contexto, antes de estruturar o documento. Resposta: padrão "eval-driven deployment" (golden dataset + guardrails como assertions + HITL tiered por risco), reaproveitando `guardrails.md` v0.8 e as avaliações do Exercício 3.1 em vez de propor plataforma nova. Usuário concordou ("não é hora de reinventarmos a roda"). | Estrutura de 5 seções proposta e aprovada. Duas decisões fechadas via pergunta direta: (a) aprovador de guardrail Crítico = Product Specialist + Tech Lead em revisão paralela com SLA de 24h (usuário levantou preocupação com lentidão do par; mitigado com escopo estreito + paralelismo + SLA, mantendo o par); (b) documento fica autocontido, sem referenciar o plano de rollback do Delivery Manager. `HarnessProduto.md` v0.1 escrito com essas decisões. |
| 2 | Usuário pediu para iniciar revisão por personas (além do P.O., sugeri QA e Tech Lead, dispensando DM e Dev Sênior — aceito). Primeira tentativa de revisão P.O. misturou incorretamente a lente de persona com os 3 critérios de avaliação oficiais do enunciado — usuário apontou a confusão, esclarecendo que persona e critérios são atividades separadas (diferente do que eu havia assumido do histórico do Ex. 3.1). Decisão: personas revisam primeiro pela lente profissional própria; checagem dos 3 critérios oficiais roda depois, à parte. | Revisão P.O. refeita como lente de produto/negócio pura (sem critérios). 3 achados, todos aplicados em `HarnessProduto.md` v0.2: (1) Seção 1.5 nova — fechamento do loop de feedback com o atendente; (2) Seção 2.4 nova — sinais de melhoria ao longo do tempo (o harness antes só provava não-degradação, não melhoria — metade da missão da subfase); (3) Seção 4 nova — fasagem MVP-para-a-demo (2 semanas) vs. estado maduro. Próximas personas: QA, depois Tech Lead. Checagem dos 3 critérios oficiais ainda pendente, a rodar após as personas. |
| 3 | Usuário confirmou seguir com QA. | Revisão QA (lente de rigor metodológico) executada sobre a Seção 2 (Regression Testing): 3 achados, todos aplicados em `HarnessProduto.md` v0.3 — (1) regra de bloqueio do golden dataset agora compara por dimensão da rubrica, não só score agregado; (2) caso novo do golden dataset exige validação por um segundo revisor antes de virar gabarito de bloqueio; (3) execução do golden dataset passa a tratar não-determinismo do modelo (temperatura zero ou mediana de 3 execuções). Próxima persona: Tech Lead. Checagem dos 3 critérios oficiais ainda pendente. |
| 4 | Usuário pediu para continuar com Tech Lead. | Revisão Tech Lead (lente de viabilidade técnica) executada: 3 achados, todos aplicados em `HarnessProduto.md` v0.4 — (1) nota de instrumentação na Seção 2.2, esclarecendo que só 2 guardrails Código têm assertion isolada hoje; (2) nota de calibração do LLM-judge na Seção 2.2, exigindo prompt específico por guardrail em vez de genérico; (3) regra de apuração explícita de "guardrail mais crítico tocado" na Seção 3.1 (união entre alvo declarado e efeito observado no diff). Revisão por personas (P.O., QA, Tech Lead) concluída. Próximo passo: checagem dos 3 critérios oficiais de avaliação, à parte. |
| 5 | Usuário pediu para rodar a checagem dos 3 critérios oficiais (atividade separada das personas, conforme decisão da Rodada 2). | Os 3 critérios atendidos. Varredura completa (não só os trechos recém-editados) encontrou 1 inconsistência factual: Seção 2.2 listava `GR-N-01` como exemplo de guardrail Código, mas ele é Híbrido no `guardrails.md` v0.8 (contradizendo a própria Seção 1.3 deste documento). Corrigido em `HarnessProduto.md` v0.5, substituindo por `GR-N-02`. Documento segue como rascunho — aprovação final pendente do usuário. |
| 6 | Usuário pediu revisão final completa: personas, critérios de avaliação, ortografia e gramática, e o que mais fosse necessário. | Personas e critérios reconfirmados intactos (sem impacto da correção da Rodada 5). 4 achados de ortografia/gramática, todos aplicados em `HarnessProduto.md` v0.6: "Fasagem" (não é palavra em português) → "Faseamento"; "Tiering"/"tiered" (anglicismo solto, mesma classe do "hallucination" já corrigido no Ex. 3.1) → "por nível de risco"; frase passiva estranha na Seção 1.2 reescrita; referência ambígua a "melhoria efetiva" na Seção 1.5 apontada explicitamente ao Objetivo (Seção 0). Nenhum gap estrutural adicional encontrado. Documento segue como rascunho — aprovação final pendente do usuário. |
| 7 | Usuário pediu o fechamento da subfase. Antes de marcar como aprovado, varredura final encontrou 1 descuido: a Decisão 2 (Seção 4 deste arquivo) ainda citava "tiered", não sincronizada com a correção de ortografia da Rodada 6 — corrigida para "por nível de risco". Usuário deu o aceite final explícito ("Aceite dado"). | Badge de status de `HarnessProduto.md` atualizado para ✅ Aprovado (v0.6). Tabela "Estado Final dos Artefatos" e este histórico atualizados como fechamento da subfase — **Exercício 3.2 (Product Specialist) concluído**. |

---

## 8. Notas de Refinamento

- Reafirmado logo no início desta subfase: nada de decisão por conta própria; toda decisão de nome de arquivo, local de entregável e metodologia deve ser confirmada com o usuário antes de produzir conteúdo — mesma lição já registrada em `referencias-agents.md` §8 e `referencias-revisao-critica.md` §8.
- O arquivo de apoio foi criado assim que o material completo foi recebido e a primeira decisão estrutural (nome do arquivo) foi fechada — aplicando a lição já registrada nas subfases anteriores sobre criar este arquivo cedo.
