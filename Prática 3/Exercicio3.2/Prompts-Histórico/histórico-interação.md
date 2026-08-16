# Histórico de Interação — Exercício 3.2 (Harness de Produto para Melhoria Contínua)

> Entregável desta subfase (Cenário 3 — Fase de Governança e Validação, Prática 3). Consolida a definição do padrão de mercado adotado, a construção do `HarnessProduto.md` em 5 seções, três rodadas de revisão por persona (P.O., QA, Tech Lead), a checagem dos 3 critérios oficiais como atividade separada, a revisão final de gramática/ortografia, e o fechamento com aceite do usuário.

---

## 1. Decisões de processo (antes do rascunho)

| # | Decisão | Origem |
|---|---|---|
| 1 | Usuário reforçou, logo no início, para não fazer nada nem investigar arquivos até terminar de passar todo o contexto e o histórico — mesma lição já registrada nas subfases anteriores. | Usuário |
| 2 | Criação de arquivo de apoio (`referencias-harness-produto.md`) para não sobrecarregar a memória da conversa — mesmo padrão dos `referencias-*.md` anteriores (Prática 2 e Exercício 3.1). Usuário confirmou ser o último passo antes de mudar de contexto. | Usuário (pedido explícito) |
| 3 | Padrão de mercado adotado como base do harness: **eval-driven deployment** (golden dataset + guardrails como assertions + aprovação humana por nível de risco), reaproveitando os insumos já existentes do projeto (`guardrails.md` v0.8 com Enforcement/Prioridade já classificados, e as respostas avaliadas no Exercício 3.1) em vez de propor uma plataforma de eval nova. | Claude (recomendação), aceita pelo usuário — "não é hora de reinventarmos a roda" |
| 4 | Estrutura do documento aprovada: 5 seções — Objetivo, Processo de Feedback, Regression Testing de Produto, Ponto de HITL, Fluxo Consolidado (a Seção 4 de Faseamento foi acrescentada depois, na revisão P.O.). | Usuário |
| 5 | Aprovador de HITL para mudanças que tocam guardrail Crítico: Product Specialist + Tech Lead, em revisão **paralela** (não sequencial), com SLA de 24h — usuário levantou preocupação com lentidão do par, mitigada com escopo estreito (só guardrail Crítico) + paralelismo + SLA, mantendo o par em vez de reduzir a aprovador único. | Usuário (decisão fechada via pergunta direta) |
| 6 | Documento fica **autocontido** — não referencia o plano de rollback do Delivery Manager (artefato de outro papel/exercício). | Usuário |

---

## 2. Construção do `HarnessProduto.md` (v0.1)

O documento foi escrito em 5 seções, cada uma amarrada a um insumo já existente do projeto — sem inventar mecanismo novo:

| Seção | Conteúdo | Insumo reaproveitado |
|---|---|---|
| 0. Objetivo | Por que o harness existe: evoluir sem reabrir os 3 incidentes já corrigidos. | `guardrails.md` v0.8 — 3 incidentes de referência |
| 1. Processo de Feedback | Fontes (explícito/implícito) → triagem por causa-raiz → roteamento por canal → backlog priorizado. | Categorias de erro já validadas no Exercício 3.1 |
| 2. Regression Testing de Produto | Golden dataset + guardrails como assertions, com regra de bloqueio. | As 6 respostas avaliadas em 3.1 como semente; campo Enforcement do `guardrails.md` |
| 3. Ponto de HITL | Tiering por Prioridade do guardrail tocado; quem aprova cada nível. | Campo Prioridade do `guardrails.md` |
| 4. Fluxo Consolidado | Diagrama end-to-end amarrando as 3 seções anteriores. | — |

---

## 3. Revisão por personas — achados e correções

O usuário pediu revisão por personas além do P.O.; sugeridas **QA** (rigor metodológico do regression testing) e **Tech Lead** (viabilidade técnica), dispensando Delivery Manager e Dev Sênior — mesmo raciocínio já usado no Exercício 3.1 (rollback é território do DM, implementação de validators é artefato próprio do Dev).

**Correção de processo no meio do caminho:** a primeira tentativa de revisão P.O. misturou incorretamente a lente de persona com os 3 critérios de avaliação oficiais do enunciado. O usuário apontou a confusão — persona (lente profissional própria) e critérios (checklist do enunciado) são atividades separadas, rodando em sequência (personas primeiro, critérios depois), não fundidas numa só. Revisão P.O. refeita sob esse modelo corrigido.

| Persona | Achados | Ajuste aplicado |
|---|---|---|
| **P.O.** | (1) O documento só provava a metade "não degradar" da missão — nenhuma métrica mostrava o assistente de fato **melhorando**. (2) Faltava distinguir o que é viável para a demo em 2 semanas do estado maduro do harness (LLM-judge, SLA automatizado). (3) O processo de feedback não fechava o loop de volta ao atendente que reportou — sem isso, o incentivo a continuar reportando cai. | Adicionadas Seção 2.4 (sinais de melhoria ao longo do tempo), Seção 4 (Faseamento MVP vs. maduro) e Seção 1.5 (fechamento do loop com o atendente). |
| **QA** | (1) A regra de bloqueio comparava só o score agregado da rubrica, podendo mascarar queda numa dimensão específica (ex: "aderência a guardrails"). (2) Nenhuma validação de um caso novo do golden dataset antes de virar gabarito de bloqueio permanente — risco de um gabarito errado bloquear mudanças corretas indefinidamente. (3) Regression testing não tratava a não-determinismo do modelo — uma única execução podia confundir variância natural com regressão real. | Regra de bloqueio passou a comparar por dimensão; novo caso exige validação por segundo revisor; execução do golden dataset passa a usar temperatura zero ou mediana de 3 execuções. |
| **Tech Lead** | (1) Nem todo guardrail classificado como Código no `guardrails.md` tem hoje uma assertion isolada e chamável — só 2 confirmados (`response-validator.ts`, Ex. 3.1 do Dev). (2) LLM-judge com prompt genérico não é confiável para guardrails com regra sutil (ex: GR-Q-02, retrieval interno × exibição). (3) A regra de "guardrail mais crítico tocado" (gate de HITL) não definia se vinha do alvo declarado na triagem ou do efeito observado no diff da regression — podendo divergir em efeitos colaterais não intencionais. | Nota de instrumentação e nota de calibração do LLM-judge adicionadas à Seção 2.2; regra de apuração explícita adicionada à Seção 3.1 (união dos dois conjuntos). |

Em cada rodada, o usuário confirmou explicitamente a persona seguinte antes de Claude prosseguir, e autorizou a aplicação dos achados antes de qualquer edição.

---

## 4. Checagem dos 3 critérios oficiais (atividade separada)

Rodada dedicada, após as 3 personas, comparando o documento contra os critérios literais do enunciado:

| Critério oficial | Resultado |
|---|---|
| Processo de feedback completo (do atendente até a melhoria efetiva) | Atende — Seções 1.1 a 1.5 cobrem o ciclo completo, incluindo o fechamento do loop adicionado na revisão P.O. |
| Regression testing reconhece efeitos colaterais e verifica que guardrails não regridem | Atende — golden dataset inteiro reexecutado a cada mudança (não só os casos relacionados); guardrails Código rodam em toda execução. |
| Ponto de HITL concreto (o que exige aprovação e quem aprova) | Atende — tiering por Prioridade + regra de apuração + papéis nomeados com SLA. |

A varredura completa (não só os trechos recém-editados pelas personas) encontrou **1 inconsistência factual**: a Seção 2.2 listava `GR-N-01` como exemplo de guardrail Enforcement Código, mas ele é **Híbrido** no `guardrails.md` v0.8 — contradizendo a própria Seção 1.3 do mesmo documento, que já tratava GR-N-01 corretamente. Corrigido, substituindo o exemplo por `GR-N-02`.

---

## 5. Revisão final (personas + critérios + gramática/ortografia)

Personas e critérios reconfirmados intactos, sem regressão causada pela correção anterior. 4 achados de ortografia/gramática, todos corrigidos:

1. **"Fasagem"** não é palavra da língua portuguesa — corrigido para **"Faseamento"** (título da Seção 4 e referência na Seção 2.2).
2. **"Tiering"/"tiered"** — anglicismo solto num documento todo em português, mesma classe de deslize já corrigida no Exercício 3.1 ("hallucination" → "alucinação") — substituído por **"por nível de risco"** no cabeçalho, no título da Seção 3.1 e no diagrama da Seção 5.
3. Frase passiva estranha na Seção 1.2 ("mas 'não encontrei' foi respondido") reescrita para "mas o assistente respondeu 'não encontrei'".
4. Referência ambígua na Seção 1.5 ("a 'melhoria efetiva' desta seção") corrigida para apontar explicitamente ao Objetivo (Seção 0), de onde o termo vem.

Antes do fechamento, uma última varredura encontrou **1 referência cruzada desatualizada**: a Decisão 2 do `referencias-harness-produto.md` ainda citava "tiered", não sincronizada com a correção acima — corrigida para "por nível de risco".

---

## 6. Itens em aberto — a carregar para as próximas subfases

Nenhum destes bloqueou a aprovação do Exercício 3.2, mas não devem ser esquecidos:

1. **Instrumentação real dos guardrails Código.** Hoje só 2 têm assertion isolada e chamável (`response-validator.ts`). Os demais precisam ser auditados e, quando necessário, instrumentados como função reutilizável antes de o regression testing rodar de fato automatizado sobre eles (nota de instrumentação, Seção 2.2 do harness).
2. **Calibração do LLM-judge.** Prompts de julgamento específicos por guardrail (não genéricos) e calibração inicial contra avaliação humana ainda precisam ser construídos — hoje é recomendação de produto, não algo implementado.
3. **Faseamento (Seção 4 do harness) é plano, não execução.** A tabela MVP-vs-maduro define o que é viável para a demo em 2 semanas, mas a migração do MVP para o estado maduro (dashboard, ferramenta de aprovação com SLA automatizado) ainda não tem dono nem prazo definidos.
4. **Regra de apuração de "guardrail mais crítico tocado" (Seção 3.1) depende de tooling que ainda não existe** — hoje o relatório de diff da regression testing é conceitual; precisa de implementação real para a união dos dois conjuntos (alvo declarado + efeito observado) ser aplicada automaticamente.

---

## 7. Fechamento do Exercício 3.2

| Entregável | Arquivo | Status |
|---|---|---|
| Documento do harness de produto | `HarnessProduto.md` | ✅ Aprovado (v0.6) |
| Documento de apoio (contexto, decisões, histórico de rodadas) | `referencias-harness-produto.md` | ✅ Fechado |
| Histórico de interação (este documento) | `histórico-interação.md` | ✅ Concluído |

**O entregável do Exercício 3.2 está completo, com fechamento e aceite final confirmados explicitamente pelo usuário em 2026-08-15.**
