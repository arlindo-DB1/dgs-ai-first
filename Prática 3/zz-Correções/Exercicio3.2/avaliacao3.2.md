# Avaliação do Exercício 3.2 — Product Specialist

> **Papel:** Product Specialist
> **Cenário:** 3 — Governança e Validação
> **Exercício:** 3.2 — Harness de produto para melhoria contínua
> **Entregável avaliado:** `Prática 3\Exercicio3.2\`
> **Skills usadas:** `avaliacao-foundation.md` + `avaliacao-product-specialist.md`

---

### Resumo
Entregável excepcionalmente completo e tecnicamente rigoroso: cobre as 3 frentes pedidas (feedback, regression testing, HITL) com profundidade que vai além do mínimo exigido, e é sustentado por referência factual correta a `guardrails.md` v0.8 e ao Exercício 3.1. O processo de construção (revisão por 3 personas + checagem de critérios oficiais + revisão final de consistência) é evidenciado com granularidade rara, incluindo a autodetecção e correção de um erro factual próprio (GR-N-01 classificado incorretamente).

### Scores por Dimensão

| Dimensão | Score | Justificativa |
|----------|-------|----------------|
| D1 — Domínio Conceitual | 3 | Explica o *porquê* de cada mecanismo, não só o *o quê*: bloqueio por dimensão da rubrica (não score agregado) para não mascarar regressão pontual; temperatura zero/mediana de 3 execuções para separar variância de regressão real; guardrails Código/Híbrido/Prompt mapeados ao campo Enforcement real do documento. HITL não é genérico — é amarrado à Prioridade (Crítico/Importante/Desejável) do guardrail tocado. |
| D2 — Uso de Ferramentas | 3 | Uso iterativo e crítico, não prompt único: 3 rodadas de revisão por persona (P.O., QA, Tech Lead) com achados concretos aplicados, checagem separada dos critérios oficiais, e revisão final de gramática/consistência que ainda encontrou e corrigiu uma referência cruzada desatualizada. Histórico de interação documenta cada rodada com decisão do usuário. |
| D3 — Qualidade do Entregável | 3 | Completo e verificado contra a fonte de verdade: conferi `GR-N-01` (Híbrido, não Código — o documento corrigiu isso na v0.5), `GR-N-02`/`GR-N-04`/`GR-N-08` (Código), contagem de 24 guardrails (10/8/6) e 17 Críticos — todos batem com `guardrails.md` v0.8. Faseamento MVP-vs-maduro torna o entregável acionável para o prazo real de 2 semanas. |
| D4 — Pensamento Crítico | 3 | Identifica uma armadilha sutil não óbvia: o harness original só provava "não degradar", mas a missão (Seção 0) exige também mostrar melhoria — corrigido com a Seção 2.4. Também expõe honestamente que só 2 dos guardrails Código têm hoje assertion isolada (nota de instrumentação), evitando a ilusão de que o regression testing automático já está pronto. A regra de "guardrail mais crítico tocado" (união entre alvo declarado e efeito observado no diff) antecipa efeitos colaterais não intencionais — exatamente o tipo de sutileza que o critério oficial pede. |
| D5 — Aplicabilidade ao Projeto | 3 | Profundamente conectado: reaproveita os 24 guardrails com Enforcement/Prioridade reais (não reinventa mecanismo), usa as 6 respostas avaliadas no Exercício 3.1 como semente do golden dataset, referencia `response-validator.ts` do Dev (Ex. 3.1) para calibrar o que já está instrumentado. |

**Score do exercício: 3.0**

### Verificação de Armadilhas
Nenhuma armadilha obrigatória listada para este exercício na skill do papel (as armadilhas #4/#6 são específicas do Exercício 3.1). Verifiquei, no entanto, um risco de erro factual introduzido pelo próprio participante durante a construção — uma inconsistência entre `GR-N-01` classificado como "Código" na Seção 2.2 (v0.1–0.4) quando na verdade é "Híbrido" no `guardrails.md` v0.8 — que **foi identificada e corrigida pelo próprio participante** na checagem de critérios (v0.5), incluindo o rastro de uma referência cruzada esquecida no arquivo de apoio (v0.7 do histórico). Isso conta a favor do rigor de revisão, não como falha.

### Pontos Fortes
- Fecha o loop do feedback até o atendente (Seção 1.5) — sem isso, a "melhoria efetiva" do Objetivo não seria verificável do lado de quem reportou o problema.
- Distingue explicitamente sinais de melhoria (Seção 2.4) de sinais de não-degradação (Seção 2.1–2.2) — a maioria dos harnesses de regression testing só cobre o segundo.
- Regra de apuração de "guardrail mais crítico tocado" (Seção 3.1) usa a união entre o alvo declarado na triagem e o efeito observado no diff, capturando efeitos colaterais não intencionais — direto ao critério oficial de que "mudanças em IA podem ter efeitos colaterais".

### Pontos de Melhoria
- O documento é bastante extenso para o nível deliberadamente simplificado do Cenário 3 — uma versão resumida (1-2 páginas) poderia ser útil como anexo executivo, mantendo o detalhe técnico no corpo principal.
- A tabela de Faseamento (Seção 4) não atribui donos/prazos para a evolução MVP→maduro — o próprio histórico de interação já registra isso como item em aberto; vale formalizar antes da próxima subfase.
- A nota de calibração do LLM-judge (Seção 2.2) reconhece que os prompts de julgamento específicos por guardrail ainda não existem — seria valorosa uma priorização de quais guardrails calibrar primeiro, dado o prazo de 2 semanas.

### Classificação
**Aprovado com distinção** (2.5–3.0)

### Tópicos da Trilha para Reforço
Nenhum — score acima do limiar de 2.5.
