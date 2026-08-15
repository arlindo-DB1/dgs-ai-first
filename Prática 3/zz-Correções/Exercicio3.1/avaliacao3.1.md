# Avaliação do Exercício 3.1 — Product Specialist

> **Papel:** Product Specialist
> **Cenário:** 3 — Governança e Validação
> **Exercício:** 3.1 — Revisão crítica das respostas do assistente
> **Entregável avaliado:** `Prática 3\Exercício3.1\`
> **Skills usadas:** `avaliacao-foundation.md` + `avaliacao-product-specialist.md`

---

### Resumo
Entregável extremamente completo e bem processado: avaliação humana, avaliação independente do Claude, comparação honesta, classificação de erro por três canais (prompt/interface/pipeline) e três rodadas de revisão por persona (P.O., QA, Tech Lead) até o fechamento. As duas armadilhas obrigatórias (#4 e #6) foram corretamente identificadas e bem justificadas. Há, no entanto, uma fragilidade metodológica relevante na Tarefa 1: o texto da "avaliação humana" é, em boa parte, cópia literal de um documento gerado por IA (Claude Cowork), o que tensiona o requisito de "avaliação por conta própria antes da IA".

### Scores por Dimensão

| Dimensão | Score | Justificativa |
|----------|-------|----------------|
| D1 — Domínio Conceitual | 3 | Distingue com precisão alucinação (confiança alta + fonte "Nenhuma" = "preencheu o vazio") de fonte não confiável (conteúdo fiel ao FAQ, mas fonte informal tratada com peso normativo indevido) e de informação incompleta (múltiplos valores aplicáveis, cobertura parcial). Conecta a proposta de correção à mecânica real de structured output (Zod `.refine()` cross-field, não apenas tipagem), mostrando entendimento específico do projeto, não genérico. |
| D2 — Uso de Ferramentas | 3 | Claude usado com metodologia deliberada (avaliação independente, sem ancorar nas conclusões humanas, para permitir comparação honesta) e com análise crítica real do output — inclusive um achado próprio do Claude (termo "supervisor" sem grounding) que foi levado a sério e rastreado até um guardrail do Cenário 2 (`GR-Q-06`). |
| D3 — Qualidade do Entregável | 3 | Completo, correto, específico ao NovaTech e acionável: propostas de ajuste organizadas pelos 3 canais pedidos no enunciado, com priorização pragmática para a janela de 2 semanas, e correções de viabilidade técnica aplicadas após revisão de Tech Lead (ex.: a regra da resposta 4 não é "impossibilidade de schema", é validação cross-field). |
| D4 — Pensamento Crítico | 2 | Identifica corretamente as duas armadilhas e vai além do pedido (classifica também a resposta 2, achado "supervisor"), mas a Tarefa 1 ("avalie por conta própria... justifique") reaproveita **verbatim** o texto de `BaseAnexoA.md` — gerado via Claude Cowork — como corpo da justificativa; a contribuição autoral fica restrita ao veredito e a uma frase de "Análise" por pergunta. Isso não invalida os vereditos (corretos e depois corroborados por uma avaliação Claude genuinamente independente), mas enfraquece a alegação de "análise humana substantiva e anterior ao uso de IA". |
| D5 — Aplicabilidade ao Projeto | 3 | Referencia explicitamente guardrails do Cenário 2 (`GR-Q-06`, `GR-D-10`), conecta a proposta de bloqueio de pipeline ao `response-validator.ts` do exercício do Dev, e reconhece dependência da fila de HITL com o desenho de Guardrails/critérios de go-live do Tech Lead e Delivery Manager. |

**Score do exercício: 2.8**

### Verificação de Armadilhas

| Armadilha | Identificada? | Onde |
|---|---|---|
| **#4 — Política de carga danificada = alucinação** | ✅ Sim | `01-AvaliaçãoHumana.md` (veredito incorreta, nota de confiança alta sem fonte) e formalmente classificada como "Alucinação (com overconfidence)" em `04-AnaliseEPropostaAjustes.md` |
| **#6 — Carga perigosa + frete expresso = fonte não confiável** | ✅ Sim | Mesma estrutura: veredito incorreta em `01`, classificação explícita "Fonte não confiável (confiança mal calibrada)" em `04` |

### Pontos Fortes
- Comparação humano × Claude genuinamente honesta: reconhece 100% de concordância nos vereditos finais e registra a única divergência real de conteúdo (termo "supervisor" sem grounding), sem forçar nem esconder nada.
- Rastreabilidade exemplar: fonte de verdade declarada entre documentos, revisão por três personas (P.O./QA/Tech Lead) com achados reais aplicados (ex.: correção de que a proposta de pipeline da resposta 2 não é puramente determinística).
- Conexão de causa raiz ao histórico do próprio projeto (guardrail `GR-Q-06` do Cenário 2 como possível origem do padrão "supervisor").

### Pontos de Melhoria
- **Reforçar a independência da Tarefa 1**: o texto de `01-AvaliaçãoHumana.md` é cópia literal de `BaseAnexoA.md` (gerado via Claude Cowork). Em exercícios "humano primeiro", escrever a própria justificativa a partir do Anexo A antes de gerar qualquer material de apoio via IA preserva a integridade do passo independente e evita a aparência de que o "veredito humano" foi decidido sobre uma narrativa já pronta.
- O processo (10 rodadas, 3 personas, múltiplos documentos) é mais pesado do que o exercício simplificado do Cenário 3 pedia — não é penalizado aqui (não contraria nenhum critério), mas vale calibrar esforço vs. escopo em exercícios futuros dessa fase.

### Classificação
**Aprovado com distinção** (2.5–3.0)

### Tópicos da Trilha para Reforço
Nenhum obrigatório — score acima de 2.5. Recomenda-se, ainda assim, reforçar a prática de "avaliação humana antes da IA" como etapa literalmente manual (sem apoio de IA na primeira passada), para blindar exercícios futuros contra a mesma tensão metodológica encontrada em D4.
