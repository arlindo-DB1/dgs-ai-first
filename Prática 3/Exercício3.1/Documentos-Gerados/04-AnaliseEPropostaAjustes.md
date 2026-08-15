# Exercício 3.1 — Classificação de Erros e Propostas de Ajuste de Produto (Tarefa 3)

> **Status: ✅ Aprovado pelo usuário em 2026-08-15.**
> Substitui a Seção 2 ("Classificação de Erros e Propostas de Ajuste") de `02-AvaliaçãoClaude.md`, que cobria apenas as respostas 4 e 6. Este documento cobre as **3 respostas com problema** identificadas na amostra (2, 4 e 6), mapeando cada uma a um dos três tipos de erro nomeados no enunciado — **alucinação, fonte não confiável, informação incompleta** — com proposta de ajuste de produto organizada explicitamente pelos três canais possíveis: **Prompt, Interface, Pipeline**.
> **Fonte de verdade:** este documento é a fonte primária da Tarefa 3. A Seção 4 de `05-ConsolidaçãoAvaliações.md` reproduz um resumo derivado deste conteúdo — qualquer mudança em classificação de erro ou proposta de ajuste deve ser feita aqui primeiro e depois replicada lá.

---

## Sumário Executivo

Das 6 respostas avaliadas (`01-AvaliaçãoHumana.md` + `02-AvaliaçãoClaude.md`), 3 apresentam problema real, cada uma correspondendo a um tipo de erro distinto:

| # | Tipo de erro | Severidade | Causa raiz |
|---|---|---|---|
| **4** | Alucinação | 🔴 Crítica | Modelo gera política formal inexistente, com confiança Alta e sem fonte |
| **6** | Fonte não confiável | 🟠 Alta | Modelo usa fonte informal/não validada para afirmação de segurança, com confiança Alta |
| **2** | Informação incompleta | 🟡 Média | Modelo responde com um único valor quando o documento-fonte tem múltiplos valores aplicáveis a subcondições distintas |

Nenhuma das três é causada pelo mesmo mecanismo — por isso as propostas de ajuste, abaixo, são específicas por resposta, embora todas combinem os três canais de produto (prompt, interface, pipeline) em proporções diferentes, de acordo com o risco.

### Para contexto — respostas sem problema (não analisadas neste documento)

| # | Pergunta | Veredito |
|---|---|---|
| 1 | "Qual o prazo de devolução para produtos standard?" | Correta |
| 3 | "Posso devolver carga perigosa classe 3?" | Correta |
| 5 | "Qual o SLA do cliente Enterprise?" | Correta |

Análise completa dessas 3 respostas em `01-AvaliaçãoHumana.md` e `02-AvaliaçãoClaude.md` — listadas aqui apenas para que a amostra inteira (6 respostas) fique visível neste documento, sem repetir a análise.

---

## 1. Resposta 4 — "Qual a política para carga danificada durante transporte?"

**Resposta do assistente:** *"A política de danos prevê reembolso integral quando comprovada negligência da transportadora, mediante laudo técnico e fotos."* — Fonte citada: Nenhuma — Confiança: Alta.

**Recap do veredito:** Incorreta. O Anexo A confirma explicitamente (seção "Gaps identificados #1") que não existe documento formal (POL ou PROC) sobre carga danificada em trânsito — a única informação existente é do FAQ-Atendimento (item 38), informal e não validado. O assistente apresentou uma "política" formal inexistente, com confiança Alta e fonte "Nenhuma".

### Tipo de erro: **Alucinação** (com overconfidence)

O modelo não recuperou nenhum documento correspondente, mas ainda assim gerou um texto no formato de regra normativa ("a política de danos prevê...") em vez de admitir a lacuna. A combinação de zero fonte + confiança máxima é o sintoma mais claro de alucinação: o modelo "preencheu o vazio" com um texto plausível.

### Proposta de ajuste de produto

| Canal | Proposta |
|---|---|
| **Prompt** | Instrução explícita: se nenhum documento correspondente for recuperado, a resposta correta é declarar a lacuna ("não encontrei documentação oficial sobre isso") — nunca gerar um texto no formato de regra normativa para preencher a ausência de fonte. |
| **Interface** | Quando a resposta é bloqueada por falta de fonte (ver regra de pipeline abaixo), a interface do atendente deve exibir um estado visualmente distinto de "resposta padrão de segurança" (ex.: banner "⚠️ Sem fonte oficial — escalar ou pesquisar manualmente"), nunca uma resposta com a mesma aparência de uma resposta normal com fonte. |
| **Pipeline** | (a) Regra determinística: `source_document` vazio/nulo → resposta rejeitada programaticamente e substituída pela mensagem padrão, antes de chegar ao atendente. (b) Regra de validação cross-field (ex.: `.refine()`/`.superRefine()` no Zod, aplicada em `response-validator.ts`, o mesmo módulo que já implementa os 2 guardrails do Dev): `confidence_score = "Alta"` não pode coexistir com `source_document` vazio/nulo — combinação rejeitada em tempo de validação, não apenas evitada por instrução de prompt. Não é uma impossibilidade de tipo do schema; é uma regra de validação aplicada no momento de checagem. |

---

## 2. Resposta 6 — "Posso enviar carga perigosa com frete expresso?"

**Resposta do assistente:** *"Sim, cargas perigosas podem ser enviadas via frete expresso mediante autorização prévia do compliance e documentação ANTT atualizada."* — Fonte citada: FAQ-Atendimento, item 32 — Confiança: Alta.

**Recap do veredito:** Incorreta/problemática. O conteúdo é fiel ao FAQ item 32 — não é invenção. O problema é a fonte: o FAQ-Atendimento é explicitamente marcado como "documento informal — NÃO validado por Compliance ou Operações", e a seção de Contradições do Anexo A (#4) reforça que não existe documento formal (PROC ou POL) definindo esse processo.

### Tipo de erro: **Fonte não confiável** (confiança mal calibrada)

Diferente da resposta 4, aqui há uma fonte real e o conteúdo é fiel a ela — o erro não é inventar, é tratar uma fonte informal e explicitamente não validada como se tivesse o mesmo peso de um documento normativo, numa pergunta sobre carga perigosa (tema de segurança/compliance).

### Proposta de ajuste de produto

| Canal | Proposta |
|---|---|
| **Prompt** | Nunca apresentar informação vinda exclusivamente de um documento informal como afirmação categórica ("Sim, pode") sem alertar explicitamente a ausência de respaldo formal — mesmo quando o conteúdo do FAQ é repetido corretamente. |
| **Interface** | Badge visível na resposta sempre que a fonte for do tipo informal (ex.: "⚠️ Fonte não validada por Compliance/Operações — confirme antes de repassar ao cliente"), com exigência de confirmação/check do atendente antes de a resposta poder ser enviada ao cliente. |
| **Pipeline** | (a) Metadado de "tipo de documento" (formal/normativo vs. informal/não validado) no índice do RAG — viável com baixo esforço, já que são apenas 5 documentos-fonte conhecidos (mapeamento estático, não classificador). (b) Regra determinística: se a única fonte recuperada for do tipo informal **e** o tema envolver carga perigosa/segurança, `confidence_score` é automaticamente rebaixado (nunca "Alta") **e** a resposta é roteada para uma fila de revisão humana (HITL) antes de chegar ao atendente. **Nota de viabilidade:** a fila de HITL é uma capacidade nova — hoje o bot do Teams responde direto ao atendente-piloto, sem retenção nem dashboard de revisão. Depende de coordenação com o desenho da camada de Guardrails do harness e com os critérios de go-live que exigem um ponto de HITL — não é um ajuste pontual de código. |

---

## 3. Resposta 2 — "Meu cliente é Silver. Qual o prazo de resolução?"

**Resposta do assistente:** *"O cliente Silver tem resolução em até 48h."* — Fonte citada: SLA-2024 — Confiança: Alta.

**Recap do veredito:** Parcialmente correta. A tabela SLA-2024 §2 tem **dois** valores de resolução para Silver — 48h úteis (chamados gerais) e 8h (incidentes críticos) — mas a pergunta original não especifica qual dos dois cenários se aplica. A resposta trata "48h" como se fosse o único valor possível, sem esclarecer a que tipo de chamado se refere, e ainda omite o qualificador "úteis".

### Tipo de erro: **Informação incompleta**

Não há invenção (o valor 48h existe e é real) nem problema de fonte (SLA-2024 é documento contratual, formal). O erro é de completude: o documento-fonte tem mais de um valor aplicável a subcondições distintas dentro do mesmo tier, e a resposta escolheu implicitamente um deles sem declarar a escolha nem oferecer o outro.

### Proposta de ajuste de produto

| Canal | Proposta |
|---|---|
| **Prompt** | Quando o chunk recuperado contiver múltiplos valores aplicáveis a subcondições distintas da mesma pergunta (ex.: chamado geral vs. incidente crítico, dentro da mesma linha de tier), o modelo deve apresentar todos os valores aplicáveis rotulados, ou fazer uma pergunta de esclarecimento antes de responder com um único valor — nunca escolher silenciosamente um cenário. Reforçar também a obrigatoriedade de reproduzir qualificadores do documento-fonte (ex.: "úteis") sem omissão. |
| **Interface** | Ao detectar que a pergunta do atendente não especifica a subcondição necessária (ex.: não diz se é chamado geral ou incidente crítico), a interface pode oferecer um seletor rápido antes de finalizar a resposta ("Chamado geral" / "Incidente crítico"), em vez de deixar o modelo assumir um cenário por conta própria. |
| **Pipeline** | Esta checagem **não é puramente determinística contra texto livre** — extrair de um texto gerado quais subcondições foram cobertas não é trivial. Duas abordagens viáveis: (a) estender o structured output além do trio `{answer, source_document, confidence_score}` com um campo `covered_conditions: string[]` que o próprio modelo preenche, permitindo comparar a contagem contra o número de subcondições do chunk recuperado; ou (b) tratar como checagem probabilística secundária (outro passo de LLM), não uma regra de pipeline pura. Em qualquer uma das duas: se o chunk contém N valores aplicáveis e a cobertura for menor que N, marcar a resposta como potencialmente incompleta (soft-flag para revisão de qualidade, não bloqueio automático — diferente das respostas 4 e 6, aqui não há risco de segurança/compliance, só de expectativa mal calibrada do cliente). |

---

## Achados Complementares (fora do escopo formal da Tarefa 3)

**Respostas 3 e 5 — termo "supervisor" não fundamentado.** Já registrado em `02-AvaliaçãoClaude.md`: ambas as respostas recomendam "escalar para o supervisor", termo que não existe em nenhum lugar do Anexo A (os canais reais são Gestão de Riscos, ramal 4500, para carga perigosa; Comercial para questões de tier). Não altera o veredito de nenhuma das duas respostas (ambas continuam corretas), mas é o mesmo tipo de padrão de "informação incompleta" em menor escala.

**Critério de corte usado (explícito):** só recebe tratamento formal de Tarefa 3 (tipo de erro + proposta de ajuste dedicada) a resposta cujo problema altera o veredito de "correta" para "parcialmente correta" ou "incorreta" — é o caso das respostas 2, 4 e 6. As respostas 3 e 5 mantêm veredito "correta" apesar do achado do "supervisor", por isso ficam como achado complementar, não como problema formal. O objetivo é manter o escopo desta tarefa auditável (3 tipos de erro nomeados no enunciado, 3 respostas), sem tratar como "problema" algo que não mudou nenhuma conclusão de correção/confiabilidade.

---

## Priorização Recomendada

Considerando a janela de 2 semanas até a demonstração para a diretoria (contexto do cenário):

1. **Resposta 4 (alucinação, crítica) — primeiro.** É o único caso onde o assistente inventa conteúdo normativo do zero; a regra de pipeline (bloqueio por ausência de `source_document`) é uma mudança determinística, testável e de alto impacto imediato — deveria ser o primeiro item implementado.
2. **Resposta 6 (fonte não confiável, alta) — segundo.** Envolve tema de segurança (carga perigosa); o metadado de tipo de documento no índice do RAG é uma mudança de infraestrutura que também beneficia outras respostas futuras com fontes informais, não só este caso pontual.
3. **Resposta 2 (informação incompleta, média) — pode seguir em paralelo.** Menor risco imediato ao cliente (não é erro de segurança nem viola compliance) — o ajuste de prompt é de implementação rápida e pode ser feito sem depender das mudanças de pipeline das duas primeiras.

Os achados complementares (respostas 3 e 5) podem ser tratados como item de acompanhamento pós-demo, já que não bloqueiam nenhum veredito atual.
