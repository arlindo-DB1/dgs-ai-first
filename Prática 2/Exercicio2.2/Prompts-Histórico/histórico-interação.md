# Histórico de Interação — Exercicio 2.2 (Formalização de Guardrails)

> Entregável desta subfase (Cenário 2 — Fase de Estruturação, Prática 2). Consolida a construção do `guardrails.md`, da primeira lista candidata até a versão final (v0.7), incluindo as decisões de processo tomadas ao longo da sessão — não só as rodadas de revisão do conteúdo.

---

## 1. Decisões de processo (antes do rascunho)

| # | Decisão | Origem |
|---|---|---|
| 1 | Nome do documento de apoio: `referencias-guardrails.md` (equivalente ao `referencias-spec.md` da subfase anterior, mas nome próprio — o antigo pertence a outra fase). | Usuário |
| 2 | Artefato formal alvo: documento próprio `guardrails.md`, não integrado ao `AGENTS.md` nesta subfase. | Usuário |
| 3 | Escopo da subfase: único entregável é o `guardrails.md` aprovado — sem mockup ou outros artefatos adicionais. | Usuário |
| 4 | Usuário pediu para aguardar o recebimento completo do material antes de qualquer pergunta de refinamento (nome, formato, escopo) — inclusive uma primeira tentativa de pergunta prematura foi corrigida. | Usuário |
| 5 | Reforço explícito do princípio "toda decisão deve passar pelo usuário", já vigente desde o Exercicio 2.1: nada de decisão unilateral, sempre validar antes de agir. | Usuário |
| 6 | Lição de processo: assim que o material completo foi recebido e as primeiras decisões estruturais fechadas, o `referencias-guardrails.md` deveria ter sido criado imediatamente — Claude só criou depois de o usuário apontar a omissão. | Usuário (correção) |

---

## 2. `guardrails.md` — de v0.1 a v0.7

### Rascunho inicial (lista candidata → v0.1)

| Rodada | Origem do feedback | O que foi encontrado / decidido | Ajuste aplicado |
|---|---|---|---|
| 1 | Usuário | Lista candidata de 14 guardrails (DEVE/NÃO DEVE/QUANDO EM DÚVIDA, com enforcement e vínculo a incidente) apresentada para aprovação antes da redação. | Aprovada sem ajustes. |
| 1 | Usuário | GR-D-02 e GR-N-03 (ambos sobre PROC-042 v1/v2) pareciam sobrepostos. | Confirmado que são distintos o suficiente para conviver sem duplicação (um trata de garantir recuperação/exibição, o outro de nunca misturar valores). |
| 1 | Usuário | Formato final da tabela: incluir campos adicionais sempre que possível. | Adicionados os campos ID de rastreio (`GR-D/N/Q-##`), Bounded Context, Exemplo concreto e Status; formato de "cartão" por guardrail. |
| 1 | Usuário | Consulta ao Anexo B (armadilhas do pipeline RAG) e ao `005-Guardrails.md` da Prática 1 (com justificativa: documento de guardrails anterior sobre o mesmo assistente). | Autorizado — usado para exemplos concretos por chunk_id e para verificar consistência com guardrails já validados antes. |
| 1 | Usuário | GR-D-07 (transparência de vigência) promovido do backlog da Prática 1, ligado ao Incidente 2; GR-N-05/N-06 incluídos mesmo sem incidente correspondente; guardrail de "reapresentação de fonte incorreta" excluído (depende do `feedback-api`, fora de escopo). | Lista expandida de 14 para 17 guardrails. `guardrails.md` v0.1 criado. |

### Revisão multi-persona (ordem definida pelo usuário: P.O. Senior → Tech Lead → QA → referências leves → avaliação final)

> A partir daqui, o usuário pediu mudança de formato: sempre "Achado + Ajuste proposto" (com "por que importa" como complemento opcional), para decidir item a item.

| Rodada | Persona | Achados | Ajuste aplicado |
|---|---|---|---|
| 2 | P.O. Senior | 5 achados: (1) justificativas só técnicas, sem risco de negócio; (2) GR-D-03 tensiona com a constraint de latência sem reconhecer; (3) faltava guardrail ligando confiança Baixa a validação humana; (4) GR-D-06 (idioma) sem risco de negócio amarrado; (5) GR-N-05/N-06 sem prioridade marcada. | Campo "Risco de negócio" adicionado a todos; nota de conflito em GR-D-03; novo GR-D-08 (confiança Baixa → validação humana); observação melhorada em GR-D-06 (mantido); campo "Prioridade" adicionado a todos, com 2 ajustes do usuário (GR-Q-04 e GR-N-06 elevados após consulta). → v0.2 (18 guardrails). |
| 3 | Tech Lead | 3 achados: (1) nenhum guardrail cobria "pergunta fora do domínio" — gap contra os VC-12/VC-14 já aprovados no `requirements.md`; (2) a nota de conflito do GR-D-03 não definia o que fazer se o tempo se esgotasse antes da cobertura completa; (3) comportamento multi-turn (ADR-0002) sem guardrail, por consistência com o idioma (GR-D-06). | Adicionado GR-N-07 (fora do domínio); adicionado GR-Q-05 (desempate latência x cobertura — usuário escolheu "extensão pequena e limitada, depois parcial sinalizado", entre 3 opções, com nota de KPI a monitorar); adicionado GR-D-09 (multi-turn). → v0.3 (21 guardrails). |
| 4 | QA | 4 achados: (1) GR-D-04 só exemplificava carga perigosa, mas POL-001 §3.2 documenta 3 categorias de exceção; (2) faltava matriz Guardrail → Verification Criteria (só existia Guardrail → Incidente); (3) GR-D-06 (idioma) sem critério objetivo para teste automatizado; (4) GR-N-06 só exemplificava "gerente dedicado", mas SLA-2024 tem múltiplos campos tier-específicos. | Notas de cobertura de teste em GR-D-04 e GR-N-06; GR-D-06 ganhou critério operacional objetivo (lista de emojis/gírias/abreviações) e enforcement atualizado para Híbrido; nova Seção 7 (Matriz Guardrail → VC). → v0.4 (21 guardrails, só enriquecimento). |

**Mudança de escopo antes da rodada 5:** o usuário questionou por que Delivery Manager havia entrado como revisor formal (não usado no Exercicio 2.1) — esclarecido que foi uma das opções oferecidas junto com Tech Lead/QA/Dev Sênior, com base nos papéis do time do cenário geral, escolhida pelo usuário. Após reconsiderar, o usuário rebaixou Delivery Manager ao mesmo status de "referência" que já valia para Dev Sênior — sinalização de risco real, sem tratamento formal completo.

| Rodada | Origem | Achados / sinalizações | Ajuste aplicado |
|---|---|---|---|
| 5 | Referência Dev Sênior + Delivery Manager | Dev Sênior: (1) GR-D-02/N-03 (garantia de recuperação de versões) — risco já conhecido desde o VC-11, ainda não confirmado; (2) GR-D-03/Q-05 (extensão adaptativa de busca) — engenharia não trivial; (3) GR-N-07 (classificador fora do domínio) — risco de falsos positivos/negativos. Delivery Manager: (1) volume de 21 guardrails — sugestão de sequenciar por prioridade; (2) GR-D-08/GR-Q-05 dependem de `teams-bot`/`painel-web`, sem spec própria ainda. | Notas de viabilidade técnica em GR-D-03, GR-Q-05 e GR-N-07; nota de dependência de módulo em GR-Q-05 (mesmo padrão do GR-D-08). Observação de sequenciamento/volume avaliada e **conscientemente deixada de fora** — já coberta pela Matriz de Prioridade; sequenciamento é papel do `plan.md`, outro artefato/dono. → v0.5. |
| 6 | Avaliação final — 3 critérios (domínio específico, prompt vs código, rastreabilidade a incidente) | Critério 1: 20/21 passam, GR-D-06 é exceção conhecida. Critério 2: 21/21 passam. Critério 3: 14/21 ligados diretamente a um dos 3 incidentes; achado — GR-N-04 e GR-N-07 na verdade tinham grounding concreto (Armadilha 2 do Anexo B; VC-12/VC-14), só não rotulado como tal; os outros 5 (D-06, D-09, N-05, N-06, Q-04) dependiam só de inferência. Usuário pediu recomendação de Claude para os 5 restantes, dado o momento do projeto (estruturação, antes do código — favorece completude). | GR-N-04/N-07 ganharam referência explícita ao grounding concreto; os 5 restantes ganharam nota de "tipo de grounding" (constraint aprovada / backlog validado / dado quantitativo / boa prática sem dado), em vez de remoção. Nova Seção 9 (Avaliação Final). → v0.6. |
| 7 | Revisão geral final (todas as óticas + gramática/ortografia) | Usuário corrigiu Claude por ter marcado o documento como "✅ Aprovado" (v0.6) sem essa ser uma decisão dele a tomar. Na releitura completa pedida pelo usuário, encontrado **erro real de contagem** na Seção 9 (não só estilo): dizia "13 guardrails ligados a incidente" e incluía por engano GR-Q-01 na lista de "sem incidente" (o correto é 14 diretos; Q-01 já está ligado ao Incidente 1 — a menção vazou de uma lista diferente, da Seção 7); também faltava o campo Status em GR-Q-01 a Q-05; a linha "Origem" do cabeçalho estava desatualizada; o prefixo "GR-" estava abreviado de forma inconsistente na Matriz de Prioridade. | Contagem corrigida (14 diretos, percentual de 71% para 76%); Status "Vigente" adicionado aos 5 itens de QUANDO EM DÚVIDA; cabeçalho "Origem" reescrito; prefixos completados. Status do documento revertido para rascunho — aprovação é decisão do usuário. → v0.7. |

**Estado final:** `guardrails.md` v0.7 — 21 guardrails (9 DEVE, 7 NÃO DEVE, 5 QUANDO EM DÚVIDA), fechado com aprovação explícita do usuário.

---

## 3. Avaliação formal contra os 3 critérios de aceite (resumo)

| Critério | Resultado |
|---|---|
| Guardrails específicos ao domínio NovaTech, não genéricos | 20 de 21 — exceção conhecida: GR-D-06 (idioma formal), mantido por decisão consciente. |
| Classificação prompt (probabilístico) vs código (determinístico) com compreensão correta | 21 de 21 — nenhuma classificação equivocada, inclusive nos casos Híbridos. |
| Rastreabilidade a um risco concreto | 14 ligados diretamente a um dos 3 incidentes + 2 (GR-N-04, GR-N-07) ligados a evidência concreta documentada (Armadilha 2 do Anexo B; VC-12/VC-14) = 16/21 (76%). Os 5 restantes (GR-D-06, D-09, N-05, N-06, Q-04) têm tipo de grounding explicitado (constraint aprovada / backlog validado / dado quantitativo / boa prática), sem serem invenção sem lastro. |

---

## 4. Itens em aberto — a carregar para as próximas subfases da Prática 2

Nenhum destes bloqueia o fechamento do Exercicio 2.2, mas não devem ser esquecidos:

1. **Validação técnica real (Dev Sênior)** para os mecanismos sinalizados como não triviais: GR-D-02/N-03 (garantia de recuperação de todas as versões, mesmo risco do VC-11 do Exercicio 2.1), GR-D-03/Q-05 (extensão adaptativa de busca) e GR-N-07 (classificador de "fora do domínio" — precisa de avaliação empírica com exemplos reais).
2. **KPI a instrumentar**: frequência de acionamento da extensão limitada de busca (GR-Q-05) e de respostas parciais por limite de latência — candidato a indicador do `painel-web`, ainda sem spec própria.
3. **Dependências cross-módulo não resolvidas nesta subfase**: GR-D-08 (validação humana na UI) depende do `teams-bot`; o KPI do GR-Q-05 depende do `painel-web` — ambos sem spec própria ainda.
4. **Sequenciamento de implementação**: a Matriz de Prioridade (Seção 6 do `guardrails.md`) já dá a base (15 Crítico, 4 Importante, 2 Desejável), mas a decisão de sequenciar entrega é do `plan.md`, ainda não escrito.
5. **GR-N-04 (FAQ não é fonte) é revisável** — se reintroduzido no futuro, deve vir sempre com confiança Baixa e aviso explícito de fonte não validada (mesma ressalva já registrada no `domain-model.md` desde o Exercicio 2.1).
6. **Integração ao AGENTS.md**: os guardrails formalizados aqui ainda não foram incorporados à constitution do projeto (Anexo C), que segue vazia — decisão consciente de manter como documento próprio nesta subfase.

---

## 5. Fechamento do Exercicio 2.2

| Entregável | Arquivo | Status |
|---|---|---|
| Documento de guardrails (DEVE/NÃO DEVE/QUANDO EM DÚVIDA), com classificação de enforcement e rastreabilidade a incidentes | `guardrails.md` | ✅ Aprovado (v0.7) |
| Documento de apoio (contexto, decisões, histórico de rodadas) | `referencias-guardrails.md` | ✅ Atualizado |
| Histórico de interação (este documento) | `histórico-interação.md` | ✅ Concluído |

**O entregável do Exercicio 2.2 está completo, com fechamento confirmado pelo usuário.**
