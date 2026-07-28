## Avaliação do Exercício 1.1 — Product Specialist

### Resumo
Entregável de altíssima qualidade: o participante executou rigorosamente a estratégia de 3 etapas de engenharia de contexto, documentou decisões de contexto, outputs e variação de qualidade em cada etapa, e produziu quatro documentos formais (mapa de temas, hipóteses de gaps, análise de inconsistências PROC-042 e cruzamento com o FAQ) tecnicamente corretos e ricamente referenciados entre si. O histórico completo de prompts (4 sessões) evidencia iteração real na engenharia de prompt, não apenas execução de um pedido único.

### Scores por Dimensão

| Dimensão | Score | Justificativa |
|----------|-------|---------------|
| D1 — Domínio Conceitual | 3 | A reflexão em `05-Respostas-Tarefas` articula corretamente orçamento de atenção ("recursos finitos da ferramenta, como a janela de contexto"), degradação de contexto com excesso de informação, e o valor de "não dar mais do que precisa" ao modelo. A estratégia de 3 etapas segue exatamente a lógica de progressive disclosure pedida, com justificativa explícita em cada transição. |
| D2 — Uso de Ferramentas | 3 | Os 4 arquivos em `Prompts-Histórico/` mostram construção iterativa dos prompts (Claude faz perguntas de esclarecimento, propõe rascunho, usuário ajusta, só então executa), não aceitação acrítica do primeiro resultado. Prompts são específicos, com formato de output, regras de rastreabilidade e padronização definidos explicitamente — muito acima de "analise estes documentos". |
| D3 — Qualidade do Entregável | 3 | Os 4 documentos gerados são completos, consistentes entre si (cross-referências GAP-XX/INC-XX corretas), tecnicamente corretos frente ao Anexo A (ex.: divergências de multiplicadores e fator de peso do PROC-042 v1/v2 conferem exatamente com a fonte) e utilizáveis como artefato real de discovery. Pequenos deslizes de digitação na reflexão ("andesse", "deteriorização") não comprometem o conteúdo. |
| D4 — Pensamento Crítico | 3 | Evidências concretas de julgamento próprio: definição autoral do schema de metadados/versionamento, rejeição consciente de uma "correção" automática do Claude que alteraria uma citação textual do FAQ original (sessão 004), e checkpoints de confirmação antes de cada execução. Ponto de atenção: a escolha dos 2 documentos da Etapa 2 foi pedida ao próprio Claude (com base no mapa já gerado) em vez de decidida primeiro pelo participante e só então validada — mitigado pelo fato de os dados (GAP-01 P1-Crítico) já apontarem inequivocamente para esse par. |
| D5 — Aplicabilidade ao Projeto | 3 | Os riscos escolhidos (governança de versões do PROC-042 e dependência de conhecimento tácito) são amarrados diretamente ao risco do assistente de IA citar fonte conflitante com falsa confiança (GAP-05) — a conexão entre achado de discovery e viabilidade do RAG está explícita e correta, não é uma observação genérica de produto. |

**Score do exercício: 3.0**

### Verificação de Armadilhas
- **Colar os 5 documentos completos de uma vez no primeiro prompt:** **Não ocorreu.** A Etapa 1 (sessão 002) foi construída a partir dos resumos estruturados dos 5 documentos (`Resumos-Discovery/`), não do conteúdo integral. Os documentos completos só entraram no contexto de forma restrita: individualmente durante a geração de cada resumo (sessão 001, um arquivo por vez) e depois apenas os 2 documentos contraditórios na Etapa 2 (sessão 003). Progressive disclosure foi respeitado em todas as etapas.

### Pontos Fortes
- Rastreabilidade impecável: cada afirmação nos 4 documentos gerados cita a origem (RES-XXX, GAP-XX, INC-XX), permitindo auditar de onde veio cada achado.
- A reflexão sobre riscos vai além do pedido mínimo: distingue explicitamente "risco dentro do conteúdo lido" de "risco sobre o método" (viés de amostragem — só 5 de ~1.200 documentos, só 9 de 47 perguntas do FAQ), demonstrando maturidade de discovery.
- Uso do Claude como par de trabalho, não só gerador: perguntas de esclarecimento foram respondidas pelo usuário antes de cada execução, e uma sugestão indevida do Claude foi corrigida pelo usuário.

### Pontos de Melhoria
- Na Etapa 2, faça a seleção dos documentos prioritários você mesmo primeiro (com base no mapa da Etapa 1) e só então peça ao Claude para validar — reforça a evidência de julgamento independente nesse ponto específico do processo.
- Revisar a reflexão final (`05-Respostas-Tarefas`, item 4) quanto a pequenos erros de digitação antes de considerá-la entregável final.
- A reflexão poderia nomear explicitamente o termo "orçamento de atenção" (usado no enunciado) — hoje ele é descrito com palavras próprias corretas, mas amarrar ao vocabulário da trilha reforça a demonstração de domínio conceitual.

### Classificação
**Aprovado com distinção (2.5–3.0)**

### Tópicos da Trilha para Reforço
Não aplicável — score acima do limiar de 2.5.
