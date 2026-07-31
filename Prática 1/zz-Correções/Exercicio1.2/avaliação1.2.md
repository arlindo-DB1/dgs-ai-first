## Avaliação do Exercício 1.2

### Resumo
Entregável de altíssima qualidade: os três fluxos (principal, fallback, feedback) estão completos, coerentes entre si e ancorados nos dados reais de discovery da NovaTech (incluindo achados do Exercício 1.1, como GAP-05 e o conflito PROC-042 v1/v2). Os diagramas BPMN produzidos no Claude Design são profissionais e rastreáveis, e há evidência clara de iteração real entre versões (v1→v2→v3 do Fallback) e de correções feitas após testar os prompts na ferramenta.

### Scores por Dimensão

| Dimensão | Score | Justificativa |
|----------|-------|----------------|
| D1 — Domínio Conceitual | 3 | Demonstra entendimento real de limitações de RAG: base restrita sem "preenchimento" por conhecimento geral do modelo, risco de citar fonte tecnicamente correta mas conflitante com falsa confiança (GAP-05), e distinção entre problema de *conteúdo* vs. problema de *comportamento do assistente* — uma nuance que vai além do exigido no enunciado. |
| D2 — Uso de Ferramentas | 3 | Uso combinado de Claude (chat) e Claude Design conforme exigido. Evidência de iteração genuína: o Fluxo de Fallback passou por v1 (perdia rastreabilidade dos 3 caminhos) → v2 (identidade própria por caminho) → v3 (nota de sincronismo + correção do evento de início) — diferenças concretas e documentadas, não cosméticas. Erros reais de modelagem foram encontrados só ao testar no Claude Design e corrigidos. |
| D3 — Qualidade do Entregável | 3 | Documentos versionados, com histórico de revisão, GWT + SOP consistentes, premissas rastreáveis em todos os arquivos. Diagramas coerentes com o texto e com os 3 caminhos sempre visíveis. Único ponto de atenção: os diagramas de Fallback/Feedback são densos (múltiplos gateways paralelos, convergências X/Y), o que pode exigir narração ao apresentar a um público não técnico. |
| D4 — Pensamento Crítico | 3 | Julgamento próprio evidente: decisão deliberada de manter escopo "enxuto" numa fase inicial, tratar volume/concorrência como "ponto de observação" (não fechar premissa prematuramente), e a captura da divergência do Passo 9 (paralelismo do Ramo 1/Ramo 2) só percebida ao montar o prompt de diagrama — mostra revisão crítica do próprio raciocínio, não aceitação cega do output da IA. |
| D5 — Aplicabilidade ao Projeto | 3 | Fortemente ancorado no projeto NovaTech: usa números reais do cenário (320 chamados/dia, meta de 12→2min), e conecta guardrails a gaps específicos do discovery (GAP-05, GAP-08, GAP-11, PROC-042 v1/v2). O backlog de guardrails futuros também está vinculado a gaps concretos, não genéricos. |

**Score do exercício: 3.0**

### Verificação de Armadilhas
Nenhuma armadilha intencional explícita está definida para este exercício na skill/enunciado (diferente de exercícios com tiers inexistentes ou respostas erradas embutidas). Vale notar, no entanto, um teste implícito de continuidade: o participante precisava usar corretamente os achados do Exercício 1.1 (GAP-05, conflito PROC-042 v1/v2) para embasar os guardrails do 1.2 — isso foi feito de forma explícita e correta, com origem e risco documentados para cada guardrail confirmado.

### Pontos Fortes
- Feedback loop completo e sofisticado: sinalização → suspensão da resposta → investigação (conteúdo vs. comportamento) → correção → reliberação — muito além do mínimo "botão sem processo atrás".
- Guardrails específicos e rastreáveis a riscos reais do discovery (GAP-05, GAP-08, GAP-11), não genéricos.
- Evidência de uso real e iterativo das ferramentas, com histórico de prompts, ajustes e erros corrigidos documentados em `claude design.md` e `Claude.md`.

### Pontos de Melhoria
- Os diagramas de Fallback e Feedback são densos (gateways paralelos, convergências nomeadas X/Y); para uma apresentação a um cliente não técnico, vale simplificar visualmente ou preparar uma versão "resumida" para abertura da apresentação, guardando a versão completa para material de apoio.
- Os guardrails "confirmados" (implementados nesta fase) são mais arquiteturais/epistêmicos (ex.: "nunca completar com conhecimento geral") do que regras de negócio específicas do domínio logístico (ex.: prazos de carga perigosa); os candidatos mais "de domínio" (SLA por tier, carga danificada x avaria) ficaram no backlog — poderia ter promovido ao menos um deles a "confirmado" para reforçar a especificidade de domínio exigida no critério.

### Classificação
**Aprovado com distinção (2.5–3.0)**

### Tópicos da Trilha para Reforço
Não aplicável — score acima de 2.5. Nenhum tópico obrigatório de revisão.
