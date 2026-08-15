# Exercício 3.1 — Revisão Crítica de Outputs de IA — Documento Final

> **Status: ✅ Aprovado pelo usuário em 2026-08-15 — Documento Final do Exercício 3.1.**
> Consolida quatro documentos: `01-AvaliaçãoHumana.md` (Tarefa 1, aprovado), `02-AvaliaçãoClaude.md` (Tarefa 2, aprovado) e `03-ComparacaoAvaliacoes.md` (Tarefa 2, aprovado) reorganizados sem conteúdo novo; e `04-AnaliseEPropostaAjustes.md` (Tarefa 3, ainda rascunho), cuja Seção 4 aqui é um resumo derivado. A Seção 5 (checklist de aderência aos critérios) foi escrita diretamente nesta rodada de revisão por personas, não vem de nenhum documento-fonte. Ordem segue o entregável do enunciado: *"Sua avaliação, a avaliação do Claude, a comparação, e as propostas de ajuste."*

**Contexto:** antes do go-live do assistente da NovaTech, o Product Specialist valida uma amostra de 6 respostas do assistente em staging contra a documentação oficial (Anexo A), como segunda camada de revisão crítica de output de IA — complementar aos guardrails e structured outputs já formalizados nas fases anteriores.

---

## Amostra avaliada

| # | Pergunta | Resposta do assistente | Fonte Citada | Confiança |
|---|----------|---------|--------------|-----------|
| 1 | "Qual o prazo de devolução para produtos standard?" | "O prazo é de 7 dias úteis após o recebimento. O cliente deve abrir chamado no portal e anexar fotos." | POL-001, seção 3.2 | Alta |
| 2 | "Meu cliente é Silver. Qual o prazo de resolução?" | "O cliente Silver tem resolução em até 48h." | SLA-2024 | Alta |
| 3 | "Posso devolver carga perigosa classe 3?" | "Não. Cargas perigosas (classes 1 a 6 da ANTT) não podem ser devolvidas pelo processo padrão. Recomendo escalar para o supervisor." | POL-001, seção 3.2 | Alta |
| 4 | "Qual a política para carga danificada durante transporte?" | "A política de danos prevê reembolso integral quando comprovada negligência da transportadora, mediante laudo técnico e fotos." | Nenhuma | Alta |
| 5 | "Qual o SLA do cliente Enterprise?" | "O tier Enterprise não foi encontrado na documentação. Os tiers documentados são Gold, Silver e Standard. Sugiro confirmar a classificação ou escalar ao supervisor." | — | Baixa |
| 6 | "Posso enviar carga perigosa com frete expresso?" | "Sim, cargas perigosas podem ser enviadas via frete expresso mediante autorização prévia do compliance e documentação ANTT atualizada." | FAQ-Atendimento, item 32 | Alta |

---

## Seção 1 — Avaliação Humana (Tarefa 1)

*(fonte: `01-AvaliaçãoHumana.md`, avaliação feita com apoio de pesquisa direta no Anexo A)*

| # | Veredito | Justificativa |
|---|---|---|
| 1 | Correta | Conforme a resposta com pesquisa direta no Anexo A. Ponto de atenção: verificar se o procedimento informado (portal + fotos) faz realmente parte da seção citada. |
| 2 | Parcialmente correta | Existem distinções e casos específicos (chamado geral vs. incidente crítico) não tratados. Ponto de atenção: como se conta as 48 horas. |
| 3 | Correta | Conforme a resposta com pesquisa direta no Anexo A. |
| 4 | **Incorreta** | Não existe política formal para este caso no Anexo A. Ponto de atenção: o agente colocou confiança alta, não citou nenhuma fonte, e ainda assim informou um procedimento. |
| 5 | Correta | Conforme a resposta com pesquisa direta no Anexo A. |
| 6 | **Incorreta** | Não existe formalização para este procedimento no Anexo A. Ponto de atenção: o agente citou um item do FAQ que existe, mas é informal, ainda assim com confiança alta. |

---

## Seção 2 — Avaliação do Claude (Tarefa 2)

*(fonte: `02-AvaliaçãoClaude.md`, avaliação independente contra o Anexo A, sem consulta prévia à avaliação humana)*

| # | Veredito | Justificativa |
|---|---|---|
| 1 | Correta (ressalva de citação) | Prazo de 7 dias úteis (POL-001 §3.1) e procedimento de portal+fotos (§3.3) estão corretos como fato, mas a fonte citada (§3.2) é a seção errada — §3.2 trata de exceções, não de prazo/procedimento. |
| 2 | Parcialmente correta (informação incompleta) | SLA-2024 tem dois valores para Silver (48h úteis para chamado geral, 8h para incidente crítico); a resposta trata "48h" como único valor, sem esclarecer o tipo de chamado nem o qualificador "úteis". |
| 3 | Correta (observação adicional) | Recusa de devolução está correta (classe 3 está nas classes 1-6 da POL-001 §3.2). Achado adicional: o canal correto é "Gestão de Riscos (ramal 4500)", não "supervisor" — termo sem grounding no Anexo A. |
| 4 | **Incorreta — alucinação** | O Anexo A confirma explicitamente (Gaps identificados #1) que não existe documento formal sobre carga danificada em trânsito. O assistente apresenta uma "política" inexistente com confiança Alta e fonte "Nenhuma" — alucinação com overconfidence. |
| 5 | Correta (observação adicional) | Reconhecimento correto de que o tier "Enterprise" não existe (SLA-2024 §1). Mesmo achado da resposta 3: o canal correto é "Comercial", não "supervisor". |
| 6 | **Incorreta — fonte não confiável** | Conteúdo fiel ao FAQ item 32, mas o FAQ é explicitamente marcado como informal e não validado por Compliance/Operações. Confiança Alta é indevida para uma afirmação de segurança/compliance baseada só nessa fonte. |

---

## Seção 3 — Comparação entre as Avaliações

*(fonte: `03-ComparacaoAvaliacoes.md`)*

| # | Veredito Humano | Veredito Claude | Concordância no veredito final? | Divergência de detalhe |
|---|---|---|---|---|
| 1 | Correta | Correta | ✅ Sim | Nenhuma — mesmo achado sobre a seção citada, dito com outras palavras |
| 2 | Parcialmente correta | Parcialmente correta | ✅ Sim | Nenhuma relevante — convergência total |
| 3 | Correta (sem ressalva) | Correta (achado adicional: "supervisor" sem grounding) | ✅ Sim | ⚠️ Divergência real: avaliação humana não capturou a imprecisão do canal de escalação |
| 4 | Incorreta — alucinação | Incorreta — alucinação | ✅ Sim | Nenhuma — convergência total |
| 5 | Correta (sem ressalva) | Correta (mesmo achado adicional da resposta 3) | ✅ Sim | ⚠️ Mesma divergência da resposta 3 |
| 6 | Incorreta — fonte não confiável | Incorreta — mesma razão, mais observação sobre o prazo real (~2 dias) omitido | ✅ Sim | Divergência menor — ponto de completude adicional |

**Resumo honesto:** concordância total nos vereditos finais das 6 respostas — nenhuma diverge quanto a correta/parcialmente correta/incorreta, e isso bate com os critérios de avaliação oficiais do exercício. A única divergência real de conteúdo é o achado do Claude sobre o termo genérico **"supervisor"** (respostas 3 e 5), usado onde o Anexo A especifica canais concretos e diferentes (Gestão de Riscos ramal 4500; Comercial) — não capturado pela avaliação humana. Não muda nenhum veredito, mas sinaliza um possível problema sistemático de grounding em detalhes secundários de escalação.

---

## Seção 4 — Classificação de Erros e Propostas de Ajuste de Produto (Tarefa 3)

*(fonte: `04-AnaliseEPropostaAjustes.md` — documento dedicado, cobrindo as 3 respostas com problema real: 2, 4 e 6, cada uma mapeada a um dos três tipos de erro nomeados no enunciado. `04` é a fonte de verdade desta seção — o conteúdo abaixo é um resumo derivado; qualquer mudança de classificação ou proposta deve ser feita em `04` primeiro.)*

### Sumário

| # | Tipo de erro | Severidade | Causa raiz |
|---|---|---|---|
| **4** | Alucinação | 🔴 Crítica | Modelo gera política formal inexistente, com confiança Alta e sem fonte |
| **6** | Fonte não confiável | 🟠 Alta | Modelo usa fonte informal/não validada para afirmação de segurança, com confiança Alta |
| **2** | Informação incompleta | 🟡 Média | Modelo responde com um único valor quando o documento-fonte tem múltiplos valores aplicáveis a subcondições distintas |

### Resposta 4 — Alucinação

**Propostas de ajuste (por canal):**
- **Prompt:** na ausência de documento correspondente, a resposta certa é admitir a lacuna ("não encontrei documentação oficial"), nunca gerar texto no formato de regra normativa para preencher o gap.
- **Interface:** quando a resposta é bloqueada por falta de fonte, exibir ao atendente um estado visual distinto ("⚠️ Sem fonte oficial — escalar ou pesquisar manualmente"), nunca com a mesma aparência de uma resposta normal.
- **Pipeline:** `source_document` vazio/nulo → resposta rejeitada programaticamente e substituída por mensagem padrão; e `confidence_score = "Alta"` nunca pode coexistir com `source_document` vazio/nulo — regra de validação cross-field (Zod `.refine()`) em `response-validator.ts`, não uma impossibilidade de tipo do schema.

### Resposta 6 — Fonte não confiável

**Propostas de ajuste (por canal):**
- **Prompt:** nunca apresentar informação exclusiva de fonte informal como afirmação categórica sem alertar a ausência de respaldo formal.
- **Interface:** badge visível ("⚠️ Fonte não validada — confirme antes de repassar ao cliente") sempre que a fonte for informal, com confirmação obrigatória do atendente antes do envio ao cliente.
- **Pipeline:** metadado de "tipo de documento" (formal vs. informal) no índice do RAG — baixo esforço, só 5 documentos-fonte conhecidos; fonte exclusivamente informal + tema de carga perigosa/segurança → `confidence_score` rebaixado automaticamente (nunca "Alta") e roteamento para fila de revisão humana (HITL). *Nota de viabilidade: a fila de HITL é capacidade nova, não existe hoje — depende de coordenação com o desenho de Guardrails do harness.*

### Resposta 2 — Informação incompleta

**Propostas de ajuste (por canal):**
- **Prompt:** quando o chunk recuperado tiver múltiplos valores para subcondições distintas (ex.: chamado geral vs. incidente crítico), apresentar todos os valores rotulados ou perguntar qual se aplica — nunca escolher um cenário silenciosamente. Reforçar reprodução de qualificadores da fonte (ex.: "úteis").
- **Interface:** ao detectar pergunta ambígua quanto à subcondição, oferecer seletor rápido ("Chamado geral" / "Incidente crítico") antes de finalizar a resposta.
- **Pipeline:** não é puramente determinística contra texto livre — requer estender o structured output com um campo `covered_conditions: string[]`, ou virar checagem probabilística secundária; comparando cobertura vs. total de subcondições do chunk, sinalizar como potencialmente incompleta (soft-flag, sem risco de segurança envolvido).

### Achado complementar (fora do escopo formal da Tarefa 3) — Respostas 3 e 5 (termo "supervisor")

"Supervisor" não existe em nenhum lugar do Anexo A; os canais reais são Gestão de Riscos (ramal 4500) e Comercial, conforme o caso. Não altera nenhum veredito. Nota de rastreabilidade: o padrão pode ter origem no guardrail `GR-Q-06` (Cenário 2), já registrado à época como fallback genérico sem canal formal correspondente — vale confirmar antes de decidir onde aplicar a correção (prompt de resposta vs. redação do guardrail).

### Priorização Recomendada (janela de 2 semanas até a demo)

1. **Resposta 4 (crítica)** — primeiro: bloqueio de pipeline por ausência de fonte é mudança determinística, testável e de alto impacto imediato.
2. **Resposta 6 (alta)** — segundo: metadado de tipo de documento no RAG beneficia outras respostas futuras, não só este caso.
3. **Resposta 2 (média)** — pode seguir em paralelo: menor risco ao cliente, ajuste de prompt de implementação rápida.

Achados complementares (respostas 3/5) tratados como acompanhamento pós-demo.

---

## Seção 5 — Aderência aos Critérios de Avaliação (Revisão P.O.)

*(revisão de persona: P.O., contra os 4 critérios de avaliação oficiais do enunciado)*

| Critério oficial | Status | Onde está evidenciado |
|---|---|---|
| Resposta 4 identificada como alucinação | ✅ Atende | Seções 1, 2 e 4 — unânime nas 3 avaliações (humana, Claude, classificação de erro) |
| Resposta 6 identificada como problemática (fonte informal) | ✅ Atende | Seções 1, 2 e 4 — unânime |
| Respostas 1, 2, 3 e 5 corretamente avaliadas como adequadas | ✅ Atende (ver nota) | Seções 1 e 2 — 1, 3 e 5 corretas; 2 parcialmente correta. Nenhuma das 4 é reprovada ou incorreta. |
| Comparação com o Claude honesta sobre concordâncias e divergências | ✅ Atende | Seção 3 — 100% de concordância nos vereditos finais + uma divergência real registrada (termo "supervisor"), sem forçar nem esconder nada |

**Nota sobre o critério 3:** o critério oficial trata as respostas 1, 2, 3 e 5 como um bloco "adequado". Nossa avaliação usa uma régua mais granular (correta / parcialmente correta / incorreta) e, adicionalmente, dá à resposta 2 um tratamento formal de classificação de erro + proposta de ajuste (Seção 4) — o mesmo formato usado para as respostas 4 e 6. Isso **não contradiz** o critério oficial: "adequada" não significa "sem nada a melhorar". A resposta 2 não é uma falha de correção ou confiabilidade como 4 e 6 (não é bloqueadora de go-live) — é uma oportunidade de melhoria de severidade menor (informação incompleta), tratada com o mesmo rigor analítico por completude, não porque tenha sido reclassificada como um problema equivalente aos outros dois.

---

## Parecer Final

Das 6 respostas avaliadas: **3 são corretas sem ressalva ao veredito** (1, 3, 5 — com pequenas notas de precisão que não comprometem a conclusão), **1 é parcialmente correta com erro classificado** (2 — informação incompleta, não bloqueadora de go-live) e **2 são incorretas** (4 — alucinação; 6 — fonte não confiável), confirmando os critérios de avaliação oficiais do exercício (checklist completo na Seção 5). A avaliação humana e a avaliação independente do Claude convergem em 100% dos vereditos finais, com uma divergência de detalhe (termo "supervisor") que não altera nenhum veredito, mas vale ser tratada como item de acompanhamento de produto. As propostas de ajuste (Seção 4) endereçam as três causas raiz identificadas — ausência de fonte com confiança alta, fonte informal tratada como confiável, e escolha silenciosa entre valores aplicáveis a subcondições distintas — via uma combinação de regras determinísticas de pipeline, pontos de interface/HITL e reforço de prompt, priorizadas pelo risco de cada causa.
