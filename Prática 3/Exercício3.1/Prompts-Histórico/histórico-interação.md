# Histórico de Interação — Exercicio 3.1 (Revisão Crítica de Outputs de IA)

> Entregável desta subfase (Cenário 3 — Fase de Governança e Validação, Prática 3). Consolida a validação de 6 respostas do assistente da NovaTech contra o Anexo A, a segunda avaliação independente do Claude, a comparação honesta entre as duas, a classificação de erros e propostas de ajuste de produto, e três rodadas de revisão por persona (P.O., QA, Tech Lead) até a aprovação final.

---

## 1. Decisões de processo (antes do rascunho)

| # | Decisão | Origem |
|---|---|---|
| 1 | Usuário reforçou, logo no início, para não investigar arquivos nem fazer perguntas de refinamento antes de terminar de passar todo o material — mesma lição já registrada nas subfases da Prática 2. | Usuário |
| 2 | Criação de arquivo de apoio (`referencias-revisao-critica.md`) para não sobrecarregar a memória da conversa — mesmo padrão dos `referencias-*.md` anteriores. | Usuário (pedido explícito no início) |
| 3 | A Tarefa 1 (avaliação humana) já estava pronta em `ValidaçãoHumana.md` (com apoio de `BaseAnexoA.md`/`BaseNovaTechAssistent.md`, gerados via Claude Coworker) — mantida intocada; a Tarefa 2/3 do Claude ficaria em documento(s) próprio(s), nunca editando a Tarefa 1. | Usuário |
| 4 | Metodologia da avaliação do Claude (Tarefa 2): independente, sem consultar as conclusões da avaliação humana antes de formar o próprio veredito — só comparar no final, para a comparação ser honesta. | Usuário (confirmado via pergunta direta) |
| 5 | **Lição de processo, registrada em memória de longo prazo:** depois de uma rodada em que uma direção detalhada do usuário foi lida como autorização implícita para executar múltiplas edições em arquivos já aprovados sem checkpoint final, o usuário perguntou diretamente por que Claude tinha parado de pedir autorização. Corrigido no mesmo turno — direção sobre *o que* fazer não é autorização para *executar*, o plano concreto de arquivos/edições precisa ser confirmado antes. | Usuário (correção) |

---

## 2. Da avaliação inicial ao passo faltante da Tarefa 3

| Etapa | O que aconteceu |
|---|---|
| Avaliação do Claude (Tarefa 2) | Feita de forma independente contra o Anexo A completo (POL-001, PROC-042 v1/v2, SLA-2024, FAQ-Atendimento + notas de contradições/gaps), sem olhar a avaliação humana antes de decidir cada veredito. Aprovada pelo usuário sem ressalvas. |
| Primeira versão da Tarefa 3 | Cobria só as respostas 4 (alucinação) e 6 (fonte não confiável) — os dois problemas citados explicitamente nos critérios oficiais do enunciado. |
| Comparação | Separada para documento próprio a pedido do usuário, em vez de ficar junto com a avaliação (Seção que viria a ser `03-ComparacaoAvaliacoes.md`). Achado principal: concordância total nos vereditos finais das 6 respostas; única divergência real é o termo genérico **"supervisor"** usado nas respostas 3 e 5, sem grounding no Anexo A (os canais reais são Gestão de Riscos/ramal 4500 e Comercial). |
| Documento consolidado (1ª versão) | Reunindo avaliação humana + avaliação do Claude + comparação + propostas, na ordem do entregável do enunciado. |
| **Passo faltante identificado pelo usuário** | O enunciado nomeia 3 tipos de erro (alucinação, fonte não confiável, **informação incompleta**), mas só 2 tinham tratamento formal. A resposta 2 ("parcialmente correta" nas duas avaliações, por ambiguidade não tratada entre chamado geral e incidente crítico) nunca tinha recebido classificação de erro nem proposta de ajuste. | 
| Correção | Criado documento dedicado à Tarefa 3, cobrindo as 3 respostas com problema real (2, 4, 6), cada uma com tipo de erro nomeado e proposta de ajuste estruturada explicitamente pelos 3 canais do enunciado — **Prompt, Interface, Pipeline** — mais tabela de severidade e priorização recomendada (janela de 2 semanas até a demo). Usuário também pediu que as respostas sem problema (1, 3, 5) aparecessem no documento, mesmo sem reanálise — adicionada seção "Para contexto". |

---

## 3. Reorganização dos arquivos

O usuário pediu renomeação dos 5 arquivos-entregável com prefixo numérico de ordem de leitura e perguntou se havia dúvida ou sugestão. Uma sugestão foi levantada e aceita: inverter a ordem que o usuário tinha originalmente pedido para os dois últimos documentos, já que o documento consolidado referencia o documento de análise/propostas na própria Seção 4 — a ordem numérica devia acompanhar a ordem de dependência de leitura.

**Nomenclatura final:**

| # | Arquivo | Conteúdo |
|---|---|---|
| 01 | `01-AvaliaçãoHumana.md` | Tarefa 1 — avaliação humana (pré-existente) |
| 02 | `02-AvaliaçãoClaude.md` | Tarefa 2 — avaliação independente do Claude |
| 03 | `03-ComparacaoAvaliacoes.md` | Comparação humana × Claude |
| 04 | `04-AnaliseEPropostaAjustes.md` | Tarefa 3 — classificação de erro + propostas de ajuste (fonte de verdade) |
| 05 | `05-ConsolidaçãoAvaliações.md` | Documento Final — consolidação de tudo, na ordem do entregável |

Todas as referências cruzadas de nome de arquivo foram atualizadas nos 5 documentos e no arquivo de apoio (`referencias-revisao-critica.md`, mantido com o nome original por ser arquivo de premissas, não entregável numerado).

---

## 4. Revisão por personas — achados e correções

O usuário definiu a primeira persona (P.O.) e pediu sugestão das demais. Sugeridas **QA** (rigor da metodologia de avaliação) e **Tech Lead** (viabilidade técnica das propostas de ajuste), dispensando Delivery Manager e Dev Sênior — mesmo raciocínio já usado na subfase anterior (sequenciamento é papel de outro artefato; as propostas eram de alto nível o suficiente para o Tech Lead cobrir sozinho).

| Persona | Achados | Ajuste aplicado |
|---|---|---|
| **P.O.** | (1) Faltava um checklist explícito de aderência aos 4 critérios oficiais no documento final — a cobertura só existia de forma narrativa. (2) Tensão a esclarecer: o critério oficial agrupa as respostas 1, 2, 3 e 5 como bloco "adequado", mas a resposta 2 recebia o mesmo tratamento formal de erro/proposta reservado a 4 e 6 — não é contradição ("adequada" não significa "sem nada a melhorar"), mas precisava ficar explícito. | Adicionada Seção 5 ("Aderência aos Critérios de Avaliação") em `05`, com checklist critério-a-critério e nota de reconciliação sobre a resposta 2 (não bloqueadora de go-live, diferente de 4 e 6). |
| **QA** | (1) `03` (já aprovado) afirmava sem ressalva que 1,2,3,5 batiam com o critério oficial como bloco adequado, sem a nuance que só foi reconciliada depois, na Seção 5 do `05`. (2) Duplicação de conteúdo entre `04` e a Seção 4 do `05`, sem fonte de verdade declarada — risco de desalinhamento futuro. (3) Critério de corte entre "achado complementar" (respostas 3/5, termo "supervisor") e "problema formal" (2, 4, 6) estava implícito, nunca afirmado como regra. | Nota de rastreamento adicionada em `03` apontando para a Seção 5 do `05`. Declarada fonte de verdade explícita: `04` é a fonte primária da Tarefa 3, `05` reproduz um resumo derivado. Critério de corte explicitado em `04`: só recebe tratamento formal quem tem veredito alterado de "correta" para pior. |
| **Tech Lead** | (1) A proposta de pipeline da resposta 2 ("comparar valores mencionados no texto com o total de subcondições do chunk") não é de fato determinística contra texto livre — precisa de um campo extra no structured output (`covered_conditions`) ou virar checagem probabilística secundária. (2) O roteamento HITL da resposta 6 é uma capacidade de infraestrutura nova (não existe fila/dashboard de revisão hoje), não um ajuste pontual — depende de coordenação com o desenho de Guardrails do harness. (3) A regra da resposta 4 descrita como "impossibilidade estrutural do schema" na verdade é validação cross-field (Zod `.refine()`) em `response-validator.ts`, não impossibilidade de tipo. | As 3 correções aplicadas em `04` (fonte de verdade) e replicadas de forma condensada na Seção 4 do `05`. |

Em cada rodada, o usuário confirmou explicitamente a persona seguinte antes de Claude prosseguir, e autorizou a aplicação dos achados antes de qualquer edição.

---

## 5. Revisão final (personas + critérios + gramática)

A meio caminho, o usuário pediu uma reconferência explícita dos 4 critérios oficiais contra o estado atual dos documentos, perguntando se estavam se perdendo em meio às rodadas — varredura confirmou que os 4 critérios seguiam intactos, com 1 achado: a linha de abertura do `05` ainda dizia "consolida os três documentos já aprovados" (01/02/03), desatualizada desde que o `04` (ainda rascunho na época) e a Seção 5 (escrita direto na revisão de personas) passaram a fazer parte do documento — corrigida para descrever a origem real de cada seção.

Na revisão final propriamente dita (personas + critérios + gramática/ortografia + "o que mais achar necessário"):

- **Personas e critérios:** reconfirmados intactos, sem regressão. 1 achado cosmético sem ação necessária (nota de reconciliação do `03` não replicada no "Resumo honesto" da Seção 3 do `05` — considerado redundância evitada, já que a Seção 5 cobre o mesmo ponto em profundidade).
- **Gramática/ortografia:** corrigido "hallucination" (palavra em inglês perdida num documento todo em português) → "alucinação" em `02`; padronizado "Priorização Recomendada" (Title Case) entre `04` e `05`.
- **`01-AvaliaçãoHumana.md`:** até então mantido "intocado" por decisão anterior (é o trabalho de Tarefa 1 do próprio usuário). Usuário autorizou corrigir também: título H1 adicionado (faltava, inconsistente com os outros 4 documentos), "Analise" → "Análise" (6 ocorrências), "especificos" → "específicos", "sitou" → "citou", e reescrita da frase de abertura confusa sobre a metodologia da Tarefa 1.

---

## 6. Avaliação formal contra os 4 critérios de aceite (resumo)

| Critério oficial | Resultado |
|---|---|
| Resposta 4 identificada como alucinação | Atende — unânime nas 3 avaliações (humana, Claude, classificação de erro), sem regressão em nenhuma rodada de revisão. |
| Resposta 6 identificada como problemática (fonte informal) | Atende — unânime, mesma consistência. |
| Respostas 1, 2, 3 e 5 corretamente avaliadas como adequadas | Atende, com nota registrada: a régua interna (correta/parcialmente correta/incorreta) é mais granular que o agrupamento binário do critério oficial, e a resposta 2 recebe tratamento formal de erro por completude analítica — não por ser reclassificada como falha equivalente a 4 e 6. |
| Comparação com o Claude honesta sobre concordâncias e divergências | Atende — 100% de concordância nos vereditos finais + uma divergência real de conteúdo registrada sem forçar nem esconder (termo "supervisor" nas respostas 3 e 5). |

---

## 7. Itens em aberto — a carregar para as próximas subfases

Nenhum destes bloqueou a aprovação do Exercicio 3.1, mas não devem ser esquecidos:

1. **Termo "supervisor" (respostas 3 e 5) sem canal formal no Anexo A.** Pode ter origem no guardrail `GR-Q-06` (Cenário 2, Exercicio 2.3), já registrado à época como fallback genérico. Vale confirmar antes de decidir onde aplicar a correção — no prompt de resposta ou na redação do próprio guardrail.
2. **Extensão do structured output para a proposta da resposta 2** (campo `covered_conditions: string[]`) precisa de validação técnica real antes de virar tarefa de implementação — hoje é só uma recomendação de produto, não um schema formalizado.
3. **Fila de revisão humana (HITL) para a resposta 6** é capacidade de infraestrutura nova, não existe hoje no bot do Teams — depende do desenho da camada de Guardrails do harness (território do Exercício 3.1 do Tech Lead) e dos critérios de go-live com HITL (Exercício 3.1 do Delivery Manager).
4. **Duplicação de conteúdo entre `04` e a Seção 4 do `05`**, mitigada por uma política declarada de fonte de verdade (`04` primeiro, `05` replica), mas ainda é um risco de manutenção caso um dos dois seja editado no futuro sem lembrar do outro.

---

## 8. Fechamento do Exercicio 3.1

| Entregável | Arquivo | Status |
|---|---|---|
| Avaliação humana (Tarefa 1) | `01-AvaliaçãoHumana.md` | ✅ Corrigido (gramática/ortografia) |
| Avaliação do Claude (Tarefa 2) | `02-AvaliaçãoClaude.md` | ✅ Aprovado |
| Comparação humana × Claude | `03-ComparacaoAvaliacoes.md` | ✅ Aprovado |
| Classificação de erros e propostas de ajuste (Tarefa 3) | `04-AnaliseEPropostaAjustes.md` | ✅ Aprovado |
| Documento Final consolidado | `05-ConsolidaçãoAvaliações.md` | ✅ Aprovado — **Documento Final** |
| Documento de apoio (contexto, decisões, histórico de rodadas) | `referencias-revisao-critica.md` | ✅ Fechado |
| Histórico de interação (este documento) | `histórico-interação.md` | ✅ Concluído |

**O entregável do Exercicio 3.1 está completo, com fechamento e aprovação final confirmados explicitamente pelo usuário em 2026-08-15.**
