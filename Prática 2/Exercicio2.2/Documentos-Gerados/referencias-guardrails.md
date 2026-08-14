# Referências — Guardrails Formais (Novatech)

> Arquivo de apoio para consulta durante a construção do `guardrails.md`. Equivalente, para esta subfase, ao `referencias-spec.md` da subfase anterior (Exercicio 2.1) — nome próprio para não confundir os dois.
> Atualizado à medida que o material for recebido e o contexto for refinado com o Product Specialist Senior.

## Como usar este arquivo
- Cada material recebido é registrado na seção **Índice de Materiais**, com caminho, resumo curto e tags.
- Decisões tomadas durante o refinamento vão em **Decisões**.
- Perguntas ainda sem resposta ficam em **Dúvidas em Aberto** até serem resolvidas (aí migram para Decisões).
- Observações e aprendizados que não se encaixam nas seções acima vão em **Notas de Refinamento**.
- Rascunhos ainda não aprovados (candidatos a virar conteúdo do `guardrails.md`) ficam em **Rascunho em Avaliação**, separados do que já foi decidido.

---

## 1. Contexto Geral do Projeto (trazido do `referencias-spec.md`, Exercicio 2.1)

- **Projeto:** Novatech
- **Fase anterior (Cenário 1 / Prática 1):** Discovery + Entendimento (concluída) — ADRs de arquitetura, spec de requisitos do pipeline de RAG, protótipo funcional, cenários de falha (QA), plano de testes inicial.
- **Fase atual (Cenário 2 / Prática 2):** Estruturação do Trabalho — preparar ambiente, padrões e artefatos que vão governar o desenvolvimento, antes da primeira linha de código de produção.
- **Subfase anterior (Exercicio 2.1 — concluída):** recorte de domínio (`domain-model.md`, v0.4, aprovado) + `requirements.md` do módulo `query-endpoint` (v0.5, aprovado) + mockup + histórico de iteração.
- **Subfase atual (Exercicio 2.2):** formalização de guardrails informais como artefato estruturado, consumível por humanos e agentes.
- **Ferramentas disponíveis:** Claude (chat, todos os papéis), GitHub Copilot (devs e Tech Lead), Claude Cowork (Delivery Manager, Product Specialist, QA), Claude Design (Product Specialist).
- **Time:** 1 Tech Lead, 2 Desenvolvedores (1 pleno, 1 sênior), 1 QA, 1 Product Specialist, 1 Delivery Manager.
- **Repositório:** `novatech-assistant` (prefixo `db1/` é narrativo) — Git **local** nesta fase, sem remoto/GitHub.

### Decisões já fechadas em fases/subfases anteriores (não rediscutir, usar como premissa)
- **Modelo LLM:** Azure OpenAI GPT-4o, janela 128K (ADR-0001).
- **Pipeline de RAG:** Azure AI Search + Azure OpenAI (ADR-0004).
- **Estratégia de contexto:** budget ~4K tokens system prompt + ~8K chunks + pergunta + histórico limitado a 3 turnos (ADR-0002).
- **Documentos contraditórios:** metadado de vigência no pipeline; **porém**, diante de contradição, o assistente sempre mostra ambas as versões ao atendente — a priorização da mais recente (ADR-0003) vale só para lógica interna de recuperação, nunca para o que é exibido (decisão do Exercicio 2.1, registrada no `requirements.md` §4).
- **5 bounded contexts do domínio** (`domain-model.md` v0.4): Consulta e Resposta do Assistente (orquestração), Devolução de Mercadorias, Frete Especial, SLA e Clientes, Governança Documental (transversal).
- **Linguagem ubíqua e nível de confiança (Alta/Média/Baixa)** já definidos no `domain-model.md` v0.4 — usar essas definições, não recriar.
- **FAQ informal não é usado como fonte de resposta** — decisão do Exercicio 2.1, revisável, mas vigente.
- **Restrição à base documental (guardrail central já identificado):** o assistente responde exclusivamente com base nos documentos indexados, nunca com conhecimento geral do modelo, nunca infere regra não escrita explicitamente numa fonte.

### Princípios de trabalho (válidos para todos os artefatos desta fase)
> **"O óbvio deve ser escrito e falado."** Não omitir uma regra, comportamento ou definição só porque parece implícita ou evidente.
>
> **"Toda decisão deve passar pelo usuário."** Toda escolha editorial/de design feita durante a redação de um rascunho deve ser listada explicitamente para aprovação, separada por categoria, antes ou junto da apresentação do rascunho. Reforçado nesta subfase pelo usuário: nada de decisão unilateral, sempre validar antes de agir.

---

## 2. Contexto Específico desta Subfase (Exercicio 2.2)

### Missão recebida do usuário
> "Na fase anterior, você identificou guardrails informais. Agora você precisa formalizá-los como um artefato estruturado consumível por humanos e agentes."

1. Elaborar um documento de guardrails organizado em: **DEVE** (comportamentos obrigatórios), **NÃO DEVE** (comportamentos proibidos), **QUANDO EM DÚVIDA** (comportamentos de fallback).
2. Para cada guardrail, classificar como enforcement via **prompt** (probabilístico) ou via **código** (determinístico), com justificativa.
3. Conectar cada guardrail a pelo menos um dos 3 incidentes (qual incidente esse guardrail previne).

### Inputs fornecidos pelo usuário
- O cenário completo (ver seção 1 acima).
- Anexo A — Documentação Simulada da NovaTech, como **fonte de verdade** para os guardrails. Caminho: `Prática 2\Especificações-Exercicio\anexo-a-documentacao-simulada-novatech.md`.
- **4 guardrails informais do Cenário 1:**
  1. Sempre citar fonte.
  2. Nunca inventar prazos ou valores.
  3. Quando não encontrar resposta, dizer explicitamente.
  4. Responder em português formal.
- **3 incidentes simulados (falhas em testes internos):**
  1. Assistente respondeu que o prazo de devolução para carga perigosa é 7 dias — quando, na verdade, cargas perigosas **não podem ser devolvidas** (POL-001 §3.2).
  2. Assistente citou "PROC-042, seção 2" mas os multiplicadores informados eram da **versão 1** (desatualizada), não da v2 (vigente).
  3. Assistente disse "Não encontrei informação sobre isso" para uma pergunta sobre SLA Gold, mas o documento SLA-2024 estava indexado e continha a resposta.

### Entregável desta subfase
Documento de guardrails completo (`guardrails.md`), com classificação de enforcement e rastreabilidade aos incidentes. **Único entregável desta subfase** (confirmado pelo usuário — sem mockup, sem histórico de iteração formal como entregável separado, embora o histórico de rodadas de revisão continue registrado neste arquivo de apoio, seção 7, por prática já estabelecida).

---

## 3. Índice de Materiais

| # | Documento | Caminho | Resumo | Tags |
|---|-----------|---------|--------|------|
| G1 | Anexo A — Documentação Simulada da NovaTech | `Prática 2\Especificações-Exercicio\anexo-a-documentacao-simulada-novatech.md` | Fonte de verdade: POL-001 (devolução), PROC-042 v1/v2 (frete especial), SLA-2024, FAQ-Atendimento + meta-notas de contradições/gaps. Usado para embasar cada guardrail em uma regra de negócio real. | documentação-fonte, guardrails, fonte de verdade |
| G2 | Modelo de Domínio (Exercicio 2.1) | `Prática 2\Exercicio2.1\Documentos-Gerados\domain-model.md` | 5 bounded contexts, linguagem ubíqua, fronteiras do assistente, nível de confiança (Alta/Média/Baixa). Fonte de vocabulário e de guardrails já identificados (restrição à base documental, FAQ não usado). | domain model, bounded contexts, linguagem ubíqua |
| G3 | Requisitos — query-endpoint (Exercicio 2.1) | `Prática 2\Exercicio2.1\Documentos-Gerados\requirements.md` | Spec SDD aprovada (v0.5). Contém constraints e verification criteria que já operacionalizam parte dos guardrails (ex: VC-11 sobre garantia de recuperação de todas as versões conhecidas). | requirements, SDD, verification criteria |
| G4 | Referências — Fase de Spec (Exercicio 2.1) | `Prática 2\Exercicio2.1\Documentos-Gerados\referencias-spec.md` | Documento de apoio da subfase anterior — histórico completo de decisões, materiais e iterações do `domain-model.md`/`requirements.md`. Consultado como fonte do contexto geral trazido para a seção 1 deste arquivo. | histórico, apoio, contexto |
| G5 | Anexo B — Chunks de Referência do Pipeline de RAG | `Prática 2\Especificações-Exercicio\anexo-b-chunks-referencia-rag.md` | Chunks simulados + 5 "armadilhas" propositais (contradição de versão, FAQ como fonte crítica, tier inexistente, inversão de regra, pergunta sem cobertura). Usado para preencher o campo "Exemplo concreto" de cada guardrail com precisão de chunk_id. | chunks, RAG, armadilhas, exemplos |
| G6 | Guardrails — Consolidação e Backlog (Prática 1, Exercicio1.2) | `Prática 1\Exercicio1.2\Documentos-Gerados\005-Guardrails.md` | 3 guardrails já confirmados (consistentes com a lista atual, sem contradição) + 5 candidatos em backlog. 3 destes candidatos foram promovidos para o `guardrails.md` desta subfase (GR-D-07, GR-N-05, GR-N-06). Consultado pontualmente, com justificativa explícita ao usuário (documento de guardrails anterior sobre o mesmo assistente). | guardrails, backlog, Prática 1, histórico |

---

## 4. Decisões

1. **Nome do documento de apoio desta subfase:** `referencias-guardrails.md` (este arquivo) — nome próprio, distinto de `referencias-spec.md` (que pertence à subfase anterior).
2. **Artefato formal alvo:** documento próprio `guardrails.md`, **não** integrado ao `AGENTS.md` nesta subfase. A integração ao AGENTS.md (constitution do projeto, ainda vazio conforme Anexo C) fica para subfase futura.
3. **Escopo desta subfase:** único entregável é o `guardrails.md` aprovado. Sem mockup ou outros artefatos adicionais.
4. **Classificação de enforcement:** permitido usar uma 3ª categoria **Híbrido** (prompt + código), quando genuinamente justificado — não como saída fácil para evitar decidir, mas quando as duas camadas são indispensáveis (ex: garantir recuperação de todas as versões é código na busca + instrução de exibição no prompt).
5. **Guardrails além dos 4 informais originais:** aprovado incluir guardrails novos, inferidos diretamente dos 3 incidentes (necessário porque os 4 guardrails originais não cobrem sozinhos, por exemplo, uma falha de busca incompleta como o Incidente 3). Cada guardrail novo deve ser identificado claramente como inferido de qual incidente.
6. **Guardrails sem incidente correspondente:** incluídos no documento mesmo sem rastreabilidade a um dos 3 incidentes (ex: idioma formal, não usar o FAQ), marcados explicitamente como "sem incidente correspondente" — não removidos por falta de teste, e sem forçar uma ligação artificial.
7. **Estrutura DEVE / NÃO DEVE:** listas independentes, sem duplicar a mesma regra como par espelhado (DEVE X / NÃO DEVE oposto-de-X). Cada guardrail aparece uma única vez, na seção mais natural à sua formulação.
8. **Lista candidata de 14 guardrails (seção 6, versão anterior deste arquivo):** aprovada como estava, sem ajustes — inclusive a convivência de D2/N3 sobre PROC-042 v1/v2 sem duplicação (confirmado pelo usuário).
9. **Formato final da tabela do `guardrails.md`:** um "cartão" por guardrail (chave-valor), não uma tabela larga única — mesmo padrão visual já usado no `domain-model.md` (seções 2.1-2.5) — com os campos: ID (`GR-D-##`/`GR-N-##`/`GR-Q-##`), Enforcement, Justificativa, Incidente(s), **Bounded Context** (novo), **Exemplo concreto** (novo), **Status** (novo, Vigente/Revisável).
10. **Estrutura aprovada:** padrão SDD dos documentos anteriores + **Seção 5 nova — Matriz de Rastreabilidade (Incidente → Guardrails)**, tabela invertida para uso de QA/auditoria.
11. **Consulta ao Anexo B** (armadilhas do pipeline RAG) aprovada — já é fonte primária da Prática 2, sem necessidade de justificativa especial. Usado para preencher o campo "Exemplo concreto".
12. **Consulta ao `005-Guardrails.md` (Prática 1, Exercicio1.2)** aprovada, com justificativa: é um documento de guardrails anterior sobre o mesmo assistente, risco de duplicar/contradizer algo já validado.
13. **GR-D-07 (transparência de vigência — exibir data de atualização da fonte) incluído**, promovido do backlog do `005-Guardrails.md`, por estar diretamente ligado ao Incidente 2.
14. **Guardrail "nunca reapresenta resposta/fonte já sinalizada como incorreta" (confirmado no `005-Guardrails.md`) NÃO incluído nesta versão** — depende de estado do `feedback-api`, fora do escopo desta subfase; revisitar quando o `feedback-api` tiver spec própria.
15. **GR-N-05 (nunca minimizar tema sensível/crítico) e GR-N-06 (não generalizar regra restrita a segmento) incluídos**, promovidos do backlog do `005-Guardrails.md`, mesmo sem vínculo direto com os 3 incidentes — marcados "sem incidente correspondente".

---

## 5. Dúvidas em Aberto

_(nenhuma até o momento — todas as dúvidas da rodada anterior foram resolvidas, ver seção 4)_

---

## 6. Rascunho em Avaliação (histórico — versão inicial de 14 guardrails, superada)

> Proposta apresentada ao usuário em 2026-08-13, antes da redação do `guardrails.md`. **Aprovada sem ajustes** e depois **expandida para 17 guardrails** (GR-D-07, GR-N-05, GR-N-06 adicionados via consulta ao Anexo B e ao `005-Guardrails.md` da Prática 1 — ver Decisões, itens 11-15). O conteúdo abaixo é mantido como registro histórico da primeira rodada; a versão vigente está em `guardrails.md` (v0.1). Legenda: Incidentes 1/2/3 conforme seção 2. Enforcement: Código / Prompt / Híbrido.

### DEVE
| # | Guardrail | Enforcement | Justificativa (resumo) | Incidente(s) |
|---|---|---|---|---|
| D1 | Citar a fonte de cada informação, incluindo identificação de versão quando o documento tiver mais de uma | Código | Checável mecanicamente: resposta deve conter um `doc_id`/versão presente nos chunks realmente recuperados | 2 |
| D2 | Garantir recuperação e exibição de todas as versões conhecidas de um documento com contradição registrada | Híbrido | Código na busca (não depender só do ranking por similaridade — risco já sinalizado no VC-11 do requirements.md); prompt para instruir exibição separada de cada versão | 2 |
| D3 | Esgotar a busca em todos os contextos de conhecimento relevantes à pergunta antes de declarar "não encontrei" | Código | Cobertura de busca é parametrizável/testável (threshold, recall) | 3 |
| D4 | Verificar se a situação se enquadra numa exceção documentada antes de aplicar a regra geral | Híbrido | Detecção da categoria (ex: classes ANTT 1-6) é determinística; redigir a distinção regra/exceção depende de compreensão semântica | 1 |
| D5 | Sinalizar nível de confiança (Alta/Média/Baixa) em toda resposta | Código | Cálculo de confiança já é regra determinística definida no domain-model.md | 1, 2 |
| D6 | Responder sempre em português formal | Prompt | Qualidade de geração/tom — sem verificação determinística prática | sem incidente |

### NÃO DEVE
| # | Guardrail | Enforcement | Justificativa (resumo) | Incidente(s) |
|---|---|---|---|---|
| N1 | Nunca responder com informação que não esteja explicitamente escrita em fonte indexada — nunca usar conhecimento geral do modelo, mesmo plausível | Híbrido | Prompt como instrução central; código como grounding check pós-geração | 1 |
| N2 | Nunca citar uma versão sem confirmar que é a efetivamente usada para extrair o valor citado | Código | Verificação determinística: identificador de versão citado deve bater com o chunk de origem do valor | 2 |
| N3 | Nunca misturar valores/parâmetros de duas versões diferentes de um mesmo documento numa única resposta | Código | Checagem determinística de que todos os números da resposta pertencem à mesma versão citada | 2 |
| N4 | Nunca usar o FAQ informal como fonte de resposta | Código | Filtro na camada de recuperação (exclusão do documento do conjunto citável) — já decidido no domain-model.md v0.4 | sem incidente |

### QUANDO EM DÚVIDA
| # | Guardrail | Incidente(s) |
|---|---|---|
| Q1 | Se o tipo de carga/situação for ambíguo quanto a exceção, perguntar ao atendente antes de assumir a regra geral | 1 |
| Q2 | Se houver contradição de fonte e vigência indefinida, mostrar ambas as versões com confiança Baixa — nunca escolher uma unilateralmente | 2 |
| Q3 | Se a similaridade estiver abaixo do limiar mas dentro do domínio, declarar "não encontrei com segurança" somente após confirmar cobertura de busca (D3) | 3 |
| Q4 | Se a pergunta cruzar mais de um contexto e só parte tiver fonte suficiente, responder a parte com fonte e declarar explicitamente a ausência na outra | sem incidente |

**Status:** aprovada em 2026-08-13, e expandida antes da redação final (ver seção 7).

---

## 7. Histórico de Iteração — `guardrails.md`

| Rodada | Feedback do usuário | Ajuste aplicado |
|---|---|---|
| 1 | Lista candidata de 14 guardrails (classificações, vínculos a incidentes): aprovada sem ajustes. | Nenhum ajuste necessário. |
| 1 | D2/N3 (ambos sobre PROC-042 v1/v2): confirmado que são guardrails distintos o suficiente para conviver sem duplicação. | Nenhum ajuste necessário. |
| 1 | Formato final da tabela: incluir campos adicionais sempre que possível. | Adicionados os campos Bounded Context, Exemplo concreto e Status; formato de "cartão" por guardrail (não tabela larga única). |
| 2 | Estrutura proposta (padrão SDD + Matriz de Rastreabilidade + campo Status): aprovada como proposta. | `guardrails.md` estruturado com Seção 5 (Matriz de Rastreabilidade Incidente→Guardrails) e Seção 6 (Guardrails Revisáveis). |
| 2 | Autorização para consultar Anexo B (fonte primária) e `005-Guardrails.md` (Prática 1, com justificativa). | Anexo B usado para exemplos concretos por chunk_id; `005-Guardrails.md` consultado e comparado — sem contradição com a lista atual. |
| 2 | GR-D-07 (transparência de vigência) incluído; guardrail de "reapresentação de fonte incorreta" excluído (depende de feedback-api); GR-N-05 e GR-N-06 incluídos mesmo sem incidente correspondente. | Lista expandida de 14 para 17 guardrails antes da redação final do `guardrails.md` v0.1. |

**Resultado desta rodada:** `guardrails.md` v0.1 criado — 17 guardrails (7 DEVE, 6 NÃO DEVE, 4 QUANDO EM DÚVIDA). Pendente de checkpoint Tech Lead.

### Revisão multi-persona (ordem definida pelo usuário: P.O. Senior → Tech Lead → QA → Delivery Manager → avaliação final contra 3 critérios)

| Rodada | Persona | Feedback (formato Achado + Ajuste, a pedido do usuário) | Ajuste aplicado |
|---|---|---|---|
| 2 | P.O. Senior | 5 achados: (1) justificativas só técnicas, sem risco de negócio; (2) GR-D-03 tensiona com constraint de latência sem reconhecer; (3) falta guardrail ligando confiança Baixa a validação humana; (4) GR-D-06 sem risco de negócio amarrado; (5) GR-N-05/N-06 sem prioridade marcada. | Campo "Risco de negócio" adicionado a todos; nota de conflito em GR-D-03; novo GR-D-08; observação melhorada em GR-D-06 (mantido); campo "Prioridade" adicionado a todos (com 1 ajuste do usuário: GR-Q-04 e GR-N-06 elevados após consulta). `guardrails.md` → v0.2 (18 guardrails). |
| 3 | Tech Lead | 3 achados: (1) nenhum guardrail cobre "pergunta fora do domínio" (gap vs VC-12/VC-14 já aprovados); (2) nota de conflito do GR-D-03 não define o que fazer se o tempo se esgotar; (3) multi-turn (ADR-0002) sem guardrail, por consistência com o idioma (GR-D-06). | Adicionado GR-N-07 (fora do domínio); adicionado GR-Q-05 (desempate latência x cobertura — extensão pequena e limitada, depois parcial sinalizado, com nota de KPI a monitorar, escolhida pelo usuário entre 3 opções); adicionado GR-D-09 (multi-turn). `guardrails.md` → v0.3 (21 guardrails). |

**Nota de processo:** a partir da rodada 2, o usuário pediu mudança de formato — sempre "Achado + Ajuste proposto" (com "por que importa" como complemento opcional), para decidir item a item, em vez do formato anterior (achado + só justificativa). Aplicado a partir da revisão P.O. Senior e mantido nas revisões seguintes.

| 4 | QA | 4 achados: (1) GR-D-04 só exemplifica carga perigosa, mas POL-001 §3.2 tem 3 categorias de exceção; (2) faltava matriz Guardrail → Verification Criteria (só existia Guardrail → Incidente); (3) GR-D-06 (idioma formal) não tinha critério objetivo para teste automatizado; (4) GR-N-06 só exemplifica "gerente dedicado", mas SLA-2024 tem múltiplos campos tier-específicos. | Notas de cobertura de teste em GR-D-04 e GR-N-06; GR-D-06 ganhou critério operacional objetivo (lista de emojis/gírias/abreviações) e enforcement atualizado para Híbrido; nova Seção 7 (Matriz Guardrail → VC) adicionada. `guardrails.md` → v0.4 (21 guardrails, mesma contagem — só enriquecimento, sem novos itens). |

**Mudança de escopo antes da rodada 5:** o usuário questionou por que Delivery Manager entrou como revisor formal (não tinha sido usado no Exercicio 2.1) — esclarecido que foi uma das 4 opções oferecidas junto com Tech Lead/QA/Dev Sênior, baseada nos papéis do time do cenário geral, e escolhida pelo usuário. Após reconsiderar, o usuário rebaixou Delivery Manager ao mesmo status de "referência" que já valia para Dev Sênior (sinalização de risco real, sem revisão formal completa) — ambos passam a apontar só pontos genuinamente impactantes, não uma rodada Achado+Ajuste inteira.

| 5 | Referência Dev Sênior + Delivery Manager | Dev Sênior sinalizou: (1) GR-D-02/N-03 (garantia de recuperação de versões) — risco já conhecido desde VC-11, ainda não confirmado; (2) GR-D-03/Q-05 (extensão adaptativa de busca) — engenharia não trivial; (3) GR-N-07 (classificador fora do domínio) — risco de falsos positivos/negativos, precisa de avaliação empírica. Delivery Manager sinalizou: (1) volume de 21 guardrails — sugestão de sequenciar por prioridade; (2) GR-D-08/GR-Q-05 dependem de `teams-bot`/`painel-web`, sem spec própria ainda. | Usuário decidiu, item a item: notas de viabilidade técnica adicionadas em GR-D-03, GR-Q-05 e GR-N-07; nota de dependência de módulo adicionada em GR-Q-05 (mesmo padrão do GR-D-08); observação de volume/sequenciamento avaliada e **conscientemente deixada de fora** do `guardrails.md` — já coberta pela Matriz de Prioridade (Seção 6), sequenciamento é papel do `plan.md`, outro artefato/dono. `guardrails.md` → v0.5. |
| 6 | Avaliação final — 3 critérios (domínio específico, prompt vs código, rastreabilidade a incidente) | Critério 1: 20/21 passam, GR-D-06 é exceção conhecida. Critério 2: 21/21 passam, nenhuma classificação equivocada. Critério 3: só 14/21 ligados diretamente a um dos 3 incidentes; achado — GR-N-04 e GR-N-07 na verdade têm grounding concreto (Armadilha 2 do Anexo B; VC-12/VC-14), só não rotulado como tal; os outros 5 (D-06, D-09, N-05, N-06, Q-04) dependem só de inferência. Usuário pediu minha recomendação para os 5 restantes, dado a fase do projeto (estruturação, antes do código de produção — favorece completude, "o óbvio deve ser dito" já aplicado antes na Prática 2.1). | GR-N-04/N-07 ganharam referência explícita ao grounding concreto; os 5 restantes ganharam nota de "tipo de grounding" (constraint aprovada / backlog validado / dado quantitativo / boa prática sem dado) em vez de remoção. Nova Seção 9 (Avaliação Final) registrada no `guardrails.md`. `guardrails.md` → v0.6. |
| 7 | Revisão geral final (todas as óticas + gramática/ortografia), a pedido do usuário — que também corrigiu Claude por ter marcado o documento como "Aprovado" (v0.6) sem essa ser uma decisão dele a tomar. | Reler o documento inteiro achou: (1) **erro real de contagem** na Seção 9 — dizia "13 guardrails ligados a incidente" e incluía por engano GR-Q-01 na lista de "sem incidente" (na verdade são 14 diretos; Q-01 já está ligado ao Incidente 1 — a menção vazou da lista da Seção 7, que trata de um critério diferente); percentual corrigido de 71% para 76%; (2) GR-Q-01 a Q-05 sem campo Status — adicionado "Vigente" aos 5; (3) linha "Origem" do cabeçalho desatualizada desde a v0.2; (4) prefixo "GR-" abreviado de forma inconsistente na Matriz de Prioridade. `guardrails.md` → **v0.7**, status revertido para rascunho — aprovação final é do usuário. |

**Resultado final:** `guardrails.md` v0.7, **Aprovado** — 21 guardrails (9 DEVE, 7 NÃO DEVE, 5 QUANDO EM DÚVIDA), com ID de rastreio, enforcement classificado e justificado, risco de negócio, prioridade, bounded context, exemplo concreto, e rastreabilidade a incidente ou a outro tipo de grounding concreto explicitado. Fechamento confirmado explicitamente pelo usuário em 2026-08-13. Histórico de interação da sessão salvo em `Prompts-Histórico\histórico-interação.md`.

---

## 8. Notas de Refinamento

- Nesta subfase, o usuário reforçou explicitamente (antes de eu terminar de coletar o material) o princípio já vigente desde o Exercicio 2.1: sempre perguntar em caso de dúvida, nunca decidir sozinho. Aplicado de forma mais estrita aqui — inclusive perguntas de nomenclatura/roteiro foram explicitamente adiadas até o recebimento completo do material.
- Lição registrada: assim que o material completo foi recebido e as primeiras decisões estruturais fechadas (nome do doc, artefato alvo, escopo), este arquivo de apoio deveria ter sido criado imediatamente — antes de avançar para o rascunho de conteúdo. Corrigido nesta sessão a pedido do usuário.
- **Lição registrada (rodada 6→7):** Claude marcou o `guardrails.md` como "✅ Aprovado" após a avaliação final contra os 3 critérios, sem que o usuário tivesse dado esse sinal — o usuário corrigiu, deixando claro que declarar um documento "final"/"aprovado" é decisão exclusivamente dele, mesmo depois de um ciclo de revisão completo. Também pediu, antes de fechar, uma última revisão geral de consistência entre todas as óticas já usadas + gramática/ortografia — que encontrou um erro real de contagem (não só estilo), confirmando que vale a pena manter esse passo mesmo quando o conteúdo já passou por várias rodadas de revisão.
