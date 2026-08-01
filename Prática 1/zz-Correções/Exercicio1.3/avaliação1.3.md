# Avaliação do Exercício 1.3

> **Papel:** Product Specialist
> **Cenário:** 1 — Fase de Entendimento e Contexto
> **Exercício:** 1.3 — Especificação de requisitos de RAG do ponto de vista do produto
> **Data da avaliação:** 01/08/2026

### Resumo
Entregável de nível muito elevado: a especificação (`Especificação de requisitos-Novatech.md`, v1.3) cobre os 5 eixos exigidos com requisitos testáveis, tratamento maduro de contradições e lacunas, e forte enraizamento no discovery da NovaTech (GAPs, documentos, números do cenário). O histórico de prompts evidencia iteração genuína e multi-round (v1.0 → v1.3), incluindo um ciclo de revisão estruturado (9 achados, painel de 5 perspectivas), simulação de stakeholder e uma checagem de integridade que encontrou e corrigiu regressões reais.

### Scores por Dimensão

| Dimensão | Score | Justificativa |
|----------|-------|---------------|
| D1 — Domínio Conceitual | 3 | O documento articula corretamente a limitação central do RAG ("o pipeline apenas recupera o que já está na base... o assistente herda exatamente esses problemas, por melhor que seja o modelo"), distingue com precisão "confiabilidade da fonte" vs. "confiança da resposta" (Seção 4/Glossário), e deriva requisitos tecnicamente coerentes (detecção de conflito na ingestão, regra de precedência pelo trecho de menor confiabilidade em respostas mistas). Não é reprodução de checklist — é aplicação do conceito a casos concretos do cenário. |
| D2 — Uso de Ferramentas | 3 | Evidência rica em `001-criação-documento.md` e `002-revisão-documento.md`: acordo explícito de colaboração, uso de prós/contras em vez de escolha direta, arquivo de referência versionado (`001-Base de construção.md`) para evitar deriva de contexto, prompt de revisão autoral com 5 personas profissionais e processo interativo achado-a-achado, seguido de simulação de stakeholder e checagem de integridade. Muito além de uma única rodada de iteração. |
| D3 — Qualidade do Entregável | 3 | Documento com 12 seções bem estruturadas (sumário executivo, escopo, glossário, requisitos por eixo com racional/critério de aceite/riscos, governança transversal, métricas, riscos, próximos passos), linguagem não técnica mantida, critérios de aceite objetivamente verificáveis, rastreabilidade completa a GAPs e exercícios anteriores. |
| D4 — Pensamento Crítico | 3 | O usuário corrigiu ativamente sugestões da IA em pontos de risco real — recusou a proposta de Claude de eliminar a janela de revisão humana no Eixo 4 ("aqui acho que podemos manter a janela humana, mas com prazo urgente"), o que teria reintroduzido exatamente o risco do caso PROC-042 para tarifas. Também exigiu prós/contras antes de decidir (Eixo 1) e identificou/corrigiu inconsistência terminológica na checagem de integridade. Julgamento próprio evidente, não delegação passiva. |
| D5 — Aplicabilidade ao Projeto | 3 | Cada requisito referencia GAP específico do discovery (GAP-01 a GAP-10), documentos reais (PROC-042 v1/v2, FAQ-Atendimento) e métricas do cenário (12→2 min, 320 chamados/dia, 15% de escalonamento). Nenhuma genericidade — tudo ancorado no caso NovaTech. |

**Score do exercício: 3.0**

### Verificação de Armadilhas
Este exercício não apresenta armadilhas do tipo "resposta errada plantada" ou "tier inexistente". Os critérios de red flag do checklist do papel funcionam como armadilhas implícitas — todas evitadas:
- **5 áreas cobertas** (faltar mais de 1) → Todas as 5 cobertas. ✅ Evitada.
- **Requisitos vagos** ("deve ser bom") → Critérios de aceite objetivos em todos os eixos. ✅ Evitada.
- **Contradições ignoradas/delegadas ao LLM** → Solução concreta de governança (gate de go-live, versionamento único, detecção contínua). ✅ Evitada.
- **V1 = V2** → Evolução real e documentada v1.0→v1.3, com diffs verificáveis no histórico de revisões. ✅ Evitada.

### Pontos Fortes
- Tratamento do Eixo 2 (contradições) e Eixo 3 (ausência de resposta) com maturidade rara: gate de go-live, detecção contínua pós-produção, e a observação explícita de que "o fallback é rede de segurança para exceções, não substituto da governança na origem".
- Ciclo de revisão com verificação cruzada por múltiplas lentes (5 perspectivas + persona de stakeholder), que efetivamente capturou 2 regressões reais antes da entrega — demonstra prática de QA sobre o próprio artefato.
- Rastreabilidade exemplar entre requisito, racional de negócio e risco de origem (tabela de GAPs na Seção 8).

### Pontos de Melhoria
- O documento reconhece que "Responsável pela Base" e "Comitê do Projeto" ainda não são papéis formalmente atribuídos, mas segue usando-os como mecanismo central de escalonamento em quase todos os eixos — poderia ter incluído um requisito explícito condicionando o go-live à existência desses papéis, e não apenas registrado como pendência na Seção 10.
- Não há meta numérica agregada para as métricas de acompanhamento (Seção 9) — o documento é transparente sobre essa lacuna, mas uma sugestão de baseline inicial (mesmo que provisória) fortaleceria a seção de métricas de sucesso.

### Classificação
**Aprovado com distinção (2.5–3.0)**

### Tópicos da Trilha para Reforço
Não aplicável — score acima de 2,5. Nenhum gap conceitual ou de processo foi identificado que justifique revisão de tópicos da trilha.

---

## Documentos avaliados

- **Foundation/skill do papel:** `Avaliacao-foundation`, `avaliacao-[papel].md`
- **Entregável do participante:**
  - `Prática 1\Exercicio1.3\Documentos-Gerados\001-Base de construção.md`
  - `Prática 1\Exercicio1.3\Documentos-Gerados\Especificação de requisitos-Novatech.md` (v1.3)
  - `Prática 1\Exercicio1.3\Prompts-Histórico\001-criação-documento.md`
  - `Prática 1\Exercicio1.3\Prompts-Histórico\002-revisão-documento.md`
  - `Prática 1\Exercicio1.3\Prompts-Histórico\prompt-revisão.md`
