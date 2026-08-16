# Harness de Produto — Melhoria Contínua do Assistente NovaTech

> **Status:** ✅ Aprovado (v0.6) — aceite dado pelo usuário em 2026-08-15.
> **Exercício:** 3.2 (Product Specialist) — Harness de produto para melhoria contínua.
> **Padrão adotado:** eval-driven deployment (LLMOps) — golden dataset + guardrails como assertions + aprovação humana por nível de risco. Reaproveita os insumos já existentes do projeto em vez de propor uma plataforma nova (ver `referencias-harness-produto.md`, Decisão 2).

---

## 0. Objetivo

O assistente vai continuar evoluindo depois do go-live: novos documentos, ajustes de prompt, reindexação. Este harness garante que essa evolução **melhore** o assistente sem **degradar** o que já funciona — ou seja, sem reabrir nenhum dos 3 incidentes já corrigidos (ver `guardrails.md` v0.8) e sem introduzir novos.

Ele cobre três frentes, amarradas entre si:
1. **Processo de feedback** — como um problema relatado por um atendente vira uma melhoria real.
2. **Regression testing de produto** — como qualquer mudança candidata é verificada antes de ir ao ar.
3. **Ponto de HITL** — quais mudanças exigem aprovação humana, e de quem.

O harness também precisa mostrar quando o assistente está de fato **melhorando** — não só impedir piora (ver Seção 2.4) — e reconhecer que ele evolui em fases, começando pelo que é viável antes da demo à diretoria em 2 semanas (ver Seção 4).

---

## 1. Processo de Feedback

### 1.1 Fontes de feedback

| Tipo | Fonte | Exemplo |
|---|---|---|
| **Explícito** | Módulo de feedback do atendente (rating + comentário livre, no bot do Teams) | Atendente marca uma resposta como "incorreta" e comenta "disse que dava pra devolver carga perigosa" |
| **Implícito — escalação** | Atendente aciona o fallback de escalação (GR-Q-06) ou o canal do GR-N-05 (ramal 4500) | Resposta de confiança Baixa é escalada em vez de repassada ao cliente |
| **Implícito — repetição** | Atendente reformula a mesma pergunta em sequência (sinal de resposta insatisfatória) | Pergunta "e para o Nordeste?" repetida com palavras diferentes |
| **Implícito — confiança recorrente** | Mesma pergunta (ou pergunta do mesmo tema) volta e sempre sai com confiança Baixa/contradição | Multiplicador do Sudeste sempre sai como contradição PROC-042 v1/v2 |

### 1.2 Triagem por causa-raiz

Todo feedback recebido é classificado numa das categorias já validadas no Exercício 3.1 (mesma régua, para manter consistência entre subfases):

| Causa-raiz | Definição | Exemplo já catalogado |
|---|---|---|
| **Alucinação** | Resposta afirma algo que não está em nenhuma fonte indexada | Resposta 4 do Ex. 3.1 — política de danos inventada, sem documento correspondente |
| **Fonte não confiável** | Resposta cita fonte informal/não validada para tema que exige fonte formal | Resposta 6 do Ex. 3.1 — FAQ-Atendimento usado para carga perigosa (viola GR-N-04) |
| **Informação incompleta** | Resposta correta mas incompleta, ou aplica generalização indevida | Resposta 2 do Ex. 3.1 — SLA sem indicar exceções/condições |
| **Documento desatualizado** | Resposta usa versão vencida de um documento com contradição | Incidente 2 (`guardrails.md`) — PROC-042 v1 citado no lugar da v2 |
| **Chunk incorreto recuperado** | Retrieval falha em recuperar o chunk certo, mesmo indexado | Incidente 3 (`guardrails.md`) — SLA-2024 indexado, mas o assistente respondeu "não encontrei" |
| **Gap de conteúdo** | Pergunta legítima do domínio sem nenhuma fonte indexada | Pergunta sobre um tema de logística nunca documentado |

### 1.3 Roteamento para tipo de correção

A causa-raiz determina o canal de correção — a mesma estrutura de 3 canais já usada no Exercício 3.1 (Prompt / Interface / Pipeline), com o acréscimo do canal de conteúdo:

| Causa-raiz | Canal | Ação típica |
|---|---|---|
| Alucinação | Prompt + Código | Reforço de instrução de grounding (GR-N-01) e/ou nova assertion determinística de bloqueio |
| Fonte não confiável | Código (retrieval) | Ajuste no filtro de fontes citáveis (GR-N-04) |
| Informação incompleta | Prompt | Ajuste de instrução para checar exceções/condições antes de responder (GR-D-04) |
| Documento desatualizado | Pipeline (reindexação) | Reindexar removendo/sinalizando versão vencida, ou corrigir metadado de vigência (GR-D-07) |
| Chunk incorreto recuperado | Pipeline (retrieval) | Ajuste de chunking, threshold de similaridade, ou cobertura de busca (GR-D-03) |
| Gap de conteúdo | Novo documento | Solicitar/produzir documentação formal para o tema; até lá, resposta deve declarar ausência de fonte (GR-N-01) |

### 1.4 Backlog e priorização

Cada item de feedback triado vira um item de backlog com:
- Causa-raiz e canal (acima).
- **Guardrail(s) relacionado(s)**, quando aplicável (ex: um caso de fonte não confiável referencia GR-N-04).
- **Prioridade herdada do guardrail** (`guardrails.md`, Seção 6): item que toca guardrail **Crítico** é priorizado antes de item que toca **Importante**/**Desejável** ou nenhum guardrail.
- Item sem guardrail associado (ex: gap de conteúdo novo, sem incidente prévio) é priorizado por volume/frequência do feedback.

### 1.5 Fechamento do loop com o atendente

Todo item de feedback triado (Seção 1.2) recebe um status visível para quem o reportou — sem isso, a "melhoria efetiva" prevista no Objetivo (Seção 0) não é verificável do lado de quem originou o feedback, e o incentivo a continuar reportando cai.

| Status | Quando | Como o atendente vê |
|---|---|---|
| Em análise | Assim que o item é triado (Seção 1.2) | Confirmação simples no módulo de feedback ("recebido, em análise") |
| Corrigido | Após a mudança passar por regression testing (Seção 2) e HITL (Seção 3) e ir a produção | Notificação vinculando o feedback original à versão que o corrigiu |
| Não será feito | Quando o item é avaliado e descartado (ex: fora de escopo, falso positivo) | Notificação com motivo breve |

Esse fechamento não exige ferramenta nova — reaproveita o mesmo módulo de feedback do atendente já existente (Seção 1.1), apenas com um campo de status.

---

## 2. Regression Testing de Produto

Antes de qualquer mudança de prompt, documento novo ou reindexação ir a produção, ela roda contra uma verificação em duas partes: **golden dataset** (as respostas não pioraram) e **guardrails como assertions** (os guardrails do cenário 2 continuam valendo).

### 2.1 Golden Dataset

- **Semente inicial:** as 6 perguntas/respostas já avaliadas no Exercício 3.1 (`05-ConsolidaçãoAvaliações.md`) — cobrindo os 2 casos negativos já confirmados (alucinação, fonte não confiável) e os 4 casos positivos.
- **Crescimento:** todo item de feedback triado na Seção 1 que resultar em correção efetiva vira um **caso candidato** ao golden dataset — mas só entra de fato depois que um segundo revisor (Product Specialist ou QA, diferente de quem propôs o caso) confirma a resposta-gabarito contra o Anexo A. Sem essa segunda checagem, um gabarito errado passaria a bloquear mudanças corretas indefinidamente, sem que ninguém desconfiasse.
- **Rubrica de pontuação:** reaproveita as 4 dimensões já validadas pela QA no cenário (precisão factual, citação de fonte, aderência a guardrails, completude — escala 1-3 cada), para manter a mesma régua entre avaliação de go-live e regression testing contínuo.
- **Regra de bloqueio:** a comparação é feita **por dimensão**, não só pelo score agregado — se qualquer dimensão de qualquer caso do golden dataset piorar (comparado à última execução aprovada) após a mudança candidata, a promoção é bloqueada até investigação. Comparar só o total mascararia uma queda pontual (ex: em "aderência a guardrails") escondida atrás de uma melhora em outra dimensão.

### 2.2 Guardrails como assertions

Os 24 guardrails do `guardrails.md` v0.8 são reaproveitados diretamente como critério de verificação, usando o campo **Enforcement** que já classifica cada um:

| Enforcement | Como roda no regression testing |
|---|---|
| **Código** (ex: GR-D-01, GR-N-02, GR-N-04, GR-N-08) | Assertion automatizada, roda em toda execução — checagem determinística (campo presente, fonte na lista válida, tier existente, etc.). |
| **Híbrido** (ex: GR-D-02, GR-D-04, GR-D-08) | Parte automatizada (a metade determinística do guardrail) + parte verificada por amostragem humana ou LLM-judge contra o golden dataset. |
| **Prompt** (nenhum guardrail DEVE/NÃO DEVE é puro Prompt na v0.8 — todos os itens de QUANDO EM DÚVIDA são predominantemente Prompt) | Verificado por amostragem humana ou LLM-judge, focando nos casos do golden dataset que exercitam aquele guardrail específico. |

**Nota de instrumentação (achado Tech Lead):** nem todo guardrail classificado como Código no `guardrails.md` tem hoje uma assertion isolada e chamável pelo harness — confirmado até agora, apenas 2 (campo `source_document` e a negação de carga perigosa + devolução, ambos em `response-validator.ts`, Ex. 3.1 do Desenvolvedor). Os demais guardrails Código precisam ser auditados e, quando necessário, instrumentados como função reutilizável antes de entrarem de fato no regression testing automático — até lá, rodam como checagem manual/amostragem, no mesmo regime dos guardrails Híbrido/Prompt (ver faseamento, Seção 4).

**Nota de calibração do LLM-judge (achado Tech Lead):** o prompt de julgamento não pode ser genérico ("checar aderência ao guardrail X") para guardrails com regra sutil — ex: GR-Q-02 distingue priorização por vigência no retrieval interno (ADR-0003) de regra de exibição (sempre mostrar as duas versões), distinção que já gerou confusão dentro do próprio time (Ex. 2.3). Cada guardrail Híbrido/Prompt usado como assertion no golden dataset precisa de um prompt de julgamento específico (não um genérico) e uma calibração inicial contra avaliação humana (mesmo padrão do Ex. 3.1 — Claude como segundo avaliador, comparado à avaliação humana) antes de o judge rodar sozinho no regression testing contínuo.

**Regra de bloqueio:** falha em qualquer guardrail de **Prioridade Crítico** (17 dos 24) bloqueia a promoção automaticamente, independente do resultado do golden dataset. Falha em guardrail **Importante**/**Desejável** gera alerta, mas não bloqueia sozinha — combinada com o tiering de HITL da Seção 3.

### 2.3 Quando e como roda

1. Mudança candidata é preparada (novo prompt, novo documento, reindexação).
2. Golden dataset inteiro é reexecutado contra a versão candidata, com temperatura zero (ou equivalente determinístico) sempre que o modelo permitir — reduzindo a variância entre execuções. Para casos onde isso não for possível, cada caso roda 3 vezes e usa-se a mediana do score, para não confundir variância natural do modelo com regressão real.
3. Guardrails de Enforcement Código rodam automaticamente sobre as respostas geradas.
4. Guardrails Híbrido/Prompt relevantes ao golden dataset são checados (amostragem humana ou LLM-judge, mesmo padrão do Exercício 3.1 — Claude como segundo avaliador).
5. Relatório de diff é gerado: score antes/depois por caso do golden dataset + status de cada guardrail Crítico.
6. Só então a mudança segue para o gate de HITL (Seção 3).

### 2.4 Sinal de Melhoria (não só de não-degradação)

O regression testing (2.1–2.3) garante que o assistente não piora — mas a missão desta subfase é garantir que ele **melhore** sem degradar (Seção 0). Para isso, o harness também acompanha, ao longo do tempo (não por execução isolada):

- **Score médio do golden dataset por versão** — deve subir ou se manter, nunca só "não piorar" pontualmente.
- **Taxa de feedback negativo por semana** — tendência de queda indica melhoria real percebida pelo atendente.
- **Tempo médio entre triagem e fechamento de um item de backlog** (Seção 1.4/1.5) — encolhendo, indica que o processo de melhoria está ficando mais eficiente.
- **Volume de mudanças que seguem automaticamente** (Seção 3.1, sem guardrail Crítico/Importante tocado) — crescendo, indica que o assistente está estabilizando e reduzindo a superfície de risco a cada ciclo.

Esses sinais não bloqueiam nenhuma mudança (diferente das assertions da Seção 2.2) — são acompanhamento de tendência, não gate.

---

## 3. Ponto de Human-in-the-Loop

### 3.1 Níveis de aprovação por risco

O nível de aprovação exigido depende da **Prioridade** do guardrail mais crítico tocado pela mudança (não do tipo de mudança em si — um simples ajuste de prompt pode tocar um guardrail Crítico, e uma reindexação grande pode não tocar nenhum):

**Regra de apuração (achado Tech Lead):** "guardrail mais crítico tocado" é o mais crítico dentre (a) o guardrail declarado como alvo da correção na triagem do backlog (Seção 1.4), e (b) qualquer guardrail cujo resultado de assertion mudou no relatório de diff da regression testing (Seção 2.3) — usa-se a união dos dois conjuntos, nunca só a intenção declarada, para capturar efeitos colaterais não intencionais que a correção pode ter gerado em guardrails não relacionados ao alvo original.

| Guardrail mais crítico tocado | Aprovação exigida | Pode ir a produção sem aprovação humana? |
|---|---|---|
| **Crítico** (ex: mudança que afeta GR-N-01, GR-D-04, GR-N-08) | Product Specialist **+** Tech Lead, em revisão paralela | Não — bloqueado até os dois aprovarem ou o SLA expirar com escalação |
| **Importante** (ex: GR-D-07, GR-N-05, GR-Q-06) | Um aprovador (Product Specialist) | Não — mas aprovação única, sem par |
| **Desejável** (GR-D-06, GR-D-09) ou nenhum guardrail tocado | Nenhuma aprovação humana obrigatória | Sim, se passar 100% no regression testing (Seção 2) |

### 3.2 Quem aprova e como

- **Guardrail Crítico tocado:** Product Specialist e Tech Lead revisam **em paralelo**, não em sequência — Product Specialist valida se o comportamento resultante ainda atende ao requisito de produto; Tech Lead valida se a implementação faz o que promete. SLA de **24h** para cada aprovador responder; se o prazo expirar sem resposta, o item escala automaticamente (mesmo espírito do GR-Q-06 — fallback explícito em vez de ficar parado sem dono).
- **Guardrail Importante tocado:** Product Specialist aprova sozinho — menor risco, não justifica par.
- **Sem guardrail Crítico/Importante tocado:** segue automaticamente se o regression testing (Seção 2) passar integralmente. Nenhum humano precisa aprovar manualmente — o próprio golden dataset + assertions de guardrail Código já são o gate.

**Nota de escopo:** este harness cobre apenas a validação de produto da mudança (feedback, regression, aprovação). Decisões de rollback em produção após um incidente detectado seguem processo próprio, fora do escopo deste documento.

---

## 4. Faseamento: MVP para a Demo vs. Estado Maduro

O desenho das Seções 1–3 descreve o estado-alvo do harness. Dado que a demo para a diretoria é em **2 semanas**, nem tudo é viável de construir antes disso — o harness evolui em fases, sem que isso adie o go-live:

| Elemento | MVP (viável para a demo) | Estado maduro (evolução posterior) |
|---|---|---|
| Guardrails Código (Seção 2.2) | Já são checagens determinísticas existentes — rodam como estão | Sem mudança necessária |
| Guardrails Híbrido/Prompt (Seção 2.2) | Checagem manual por amostragem (Product Specialist revisa o golden dataset caso a caso) | LLM-judge automatizado, reduzindo esforço manual |
| Fechamento do loop com o atendente (Seção 1.5) | Atualização manual de status pelo Product Specialist | Notificação automática integrada ao módulo de feedback |
| Sinais de melhoria (Seção 2.4) | Acompanhados manualmente (planilha/relatório semanal) | Dashboard automatizado |
| HITL — par Product Specialist + Tech Lead (Seção 3.2) | Revisão via chat/Teams, SLA informal de 24h | Ferramenta de aprovação com tracking de SLA e escalação automática |

Nenhum item do MVP exige infraestrutura nova além do que o time já tem (módulo de feedback, Teams, chat) — a versão madura é evolução, não pré-requisito para ir ao ar.

---

## 5. Fluxo Consolidado

```
Feedback do atendente (explícito ou implícito)
        │
        ▼
Triagem por causa-raiz (Seção 1.2)
        │
        ▼
Roteamento: Prompt / Código / Pipeline / Novo documento (Seção 1.3)
        │
        ▼
Backlog priorizado pela Prioridade do guardrail afetado (Seção 1.4)
        │
        ▼
Mudança candidata preparada
        │
        ▼
Regression testing (Seção 2):
  • Golden dataset reexecutado (score antes/depois)
  • Guardrails Código rodam como assertions automáticas
  • Guardrails Híbrido/Prompt checados por amostragem/LLM-judge
        │
        ├── Falha em guardrail Crítico OU regressão no golden dataset ──► BLOQUEADO, volta para investigação
        │
        ▼ (passou)
Gate de HITL (Seção 3), por nível de risco conforme o guardrail mais crítico tocado:
  • Crítico ──► Product Specialist + Tech Lead (paralelo, SLA 24h)
  • Importante ──► Product Specialist (aprovador único)
  • Desejável/nenhum ──► segue automaticamente
        │
        ▼
Produção
```

---

## Histórico de Revisões

| Versão | Data | Alteração |
|---|---|---|
| 0.1 | 2026-08-15 | Primeira versão — rascunho para revisão do usuário. |
| 0.2 | 2026-08-15 | Revisão persona P.O. (lente de produto, independente dos critérios oficiais): 3 achados aplicados — (1) adicionada Seção 1.5, fechamento do loop de feedback com o atendente (status em análise/corrigido/não será feito); (2) adicionada Seção 2.4, sinais de melhoria ao longo do tempo (o harness antes só media não-degradação); (3) adicionada Seção 4, fasagem MVP-para-a-demo vs. estado maduro, dado o prazo de 2 semanas. Fluxo Consolidado renumerado de Seção 4 para Seção 5. |
| 0.3 | 2026-08-15 | Revisão persona QA (lente de rigor metodológico de testes, independente dos critérios oficiais): 3 achados aplicados na Seção 2 — (1) regra de bloqueio (2.1) agora compara por dimensão da rubrica, não só score agregado, evitando mascarar regressão pontual; (2) crescimento do golden dataset (2.1) passa a exigir validação por um segundo revisor antes de um caso novo virar gabarito de bloqueio; (3) execução do golden dataset (2.3) passa a usar temperatura zero ou mediana de 3 execuções, para não confundir variância natural do modelo com regressão real. |
| 0.4 | 2026-08-15 | Revisão persona Tech Lead (lente de viabilidade técnica, independente dos critérios oficiais): 3 achados aplicados — (1) Seção 2.2 ganhou nota de instrumentação, deixando claro que só 2 dos guardrails Código têm assertion isolada hoje (`response-validator.ts`, Ex. 3.1 do Dev), os demais rodam manual/amostragem até serem instrumentados; (2) Seção 2.2 ganhou nota de calibração do LLM-judge, exigindo prompt de julgamento específico por guardrail (não genérico) e calibração inicial contra avaliação humana, dado o risco de guardrails com regra sutil (ex: GR-Q-02); (3) Seção 3.1 ganhou regra de apuração explícita de "guardrail mais crítico tocado" — união entre o alvo declarado na triagem (1.4) e qualquer guardrail cujo resultado mudou no diff (2.3), para capturar efeitos colaterais não intencionais. Revisão por personas concluída (P.O., QA, Tech Lead). Próximo passo: checagem dos 3 critérios oficiais de avaliação, à parte. |
| 0.5 | 2026-08-15 | Checagem dos 3 critérios oficiais de avaliação (atividade separada das personas): os 3 atendidos. Varredura completa encontrou 1 inconsistência factual não relacionada aos critérios: Seção 2.2 listava `GR-N-01` como exemplo de guardrail Enforcement Código, mas no `guardrails.md` v0.8 ele é Híbrido (a própria Seção 1.3 deste documento já tratava GR-N-01 corretamente como Prompt + Código) — corrigido, substituído por `GR-N-02` (Código de verdade) na lista de exemplos. Documento ainda em rascunho, pendente de aprovação final do usuário. |
| 0.6 | 2026-08-15 | Revisão final completa (personas + critérios oficiais + ortografia/gramática): personas e critérios reconfirmados intactos, sem impacto da v0.5. 4 achados de ortografia/gramática aplicados — (1) "Fasagem" (termo inexistente em português) corrigido para "Faseamento" no título da Seção 4 e na referência da Seção 2.2; (2) "Tiering"/"tiered" (anglicismo solto num documento todo em português, mesma classe de deslize já corrigida no Ex. 3.1 com "hallucination"→"alucinação") substituído por "por nível de risco" no cabeçalho, no título da Seção 3.1 e no diagrama da Seção 5; (3) frase passiva estranha na Seção 1.2 ("mas 'não encontrei' foi respondido") reescrita para "mas o assistente respondeu 'não encontrei'"; (4) referência ambígua na Seção 1.5 ("a 'melhoria efetiva' desta seção") corrigida para apontar explicitamente ao Objetivo (Seção 0), de onde o termo vem. Antes do fechamento, encontrada e corrigida 1 referência cruzada desatualizada no `referencias-harness-produto.md` (Decisão 2 ainda citava "tiered", não sincronizada com esta correção). **Aceite final do usuário dado em 2026-08-15 — Exercício 3.2 (Product Specialist) concluído.** |
