# Guardrails — Assistente NovaTech

> **Status:** ✅ Aprovado (v0.7) — revisado por P.O. Senior, Tech Lead, QA, referências leves de Dev Sênior/Delivery Manager, avaliação final contra os 3 critérios de aceite, e uma revisão geral de consistência/gramática. Fechamento confirmado pelo usuário em 2026-08-13.
> **Fontes usadas:** Anexo A (Documentação Simulada da NovaTech), Anexo B (Chunks de Referência RAG — "armadilhas"), `domain-model.md` (v0.4, aprovado), `requirements.md` do `query-endpoint` (v0.5, aprovado), `005-Guardrails.md` (Prática 1 — Exercicio1.2, consultado pontualmente por decisão do usuário).
> **Escopo:** guardrails de comportamento do assistente NovaTech como um todo. A maioria é responsabilidade direta do `query-endpoint`; onde um guardrail depender de outro módulo (ex: `pipeline-ingestao`, `teams-bot`), isso é sinalizado explicitamente no próprio item.
> **Origem:** formalização dos guardrails informais identificados no Cenário 1 + guardrails inferidos dos 3 incidentes reportados em testes internos + 3 itens promovidos do backlog de guardrails da Prática 1 (GR-D-07, N-05, N-06) + 4 guardrails novos surgidos ao longo das revisões desta subfase (GR-D-08 e GR-D-09, GR-N-07, GR-Q-05). Detalhe completo de cada rodada no Histórico de Revisões, ao final do documento.

---

## 1. Como usar este documento

Este documento é a formalização, em artefato estruturado, dos guardrails de comportamento que o assistente NovaTech deve seguir — pensado para ser consumido tanto por humanos (Tech Lead, QA, Product Specialist, Delivery Manager) quanto por agentes de IA (Copilot, Claude Code) durante o desenvolvimento.

### Estrutura
- **DEVE** — comportamentos obrigatórios (o que o assistente sempre faz).
- **NÃO DEVE** — comportamentos proibidos (o que o assistente nunca faz).
- **QUANDO EM DÚVIDA** — comportamentos de fallback, para situações em que a regra geral não resolve sozinha a incerteza.

### Campos de cada guardrail
| Campo | Significado |
|---|---|
| **ID** | Identificador único de rastreio (`GR-D-##` para DEVE, `GR-N-##` para NÃO DEVE, `GR-Q-##` para QUANDO EM DÚVIDA), referenciável por outros documentos (AGENTS.md futuro, tasks.md, código). |
| **Enforcement** | **Código** (determinístico), **Prompt** (probabilístico), ou **Híbrido** (as duas camadas são indispensáveis; usado com moderação). Não se aplica aos itens de QUANDO EM DÚVIDA (ver nota abaixo). |
| **Justificativa** | Por que essa é a camada de enforcement correta para este guardrail (mecanismo técnico). |
| **Risco de negócio** *(novo, v0.2)* | O que a empresa/cliente perde se este guardrail falhar — framing de valor/risco, não de mecanismo. Adicionado a pedido do P.O. Senior: um guardrail se defende para stakeholders não-técnicos pelo risco que evita, não pela implementação. |
| **Incidente(s) prevenido(s)** | Qual(is) dos 3 incidentes reportados este guardrail teria evitado. Quando não há vínculo direto, marcado como "sem incidente correspondente". |
| **Bounded Context** | Em qual bounded context do `domain-model.md` (v0.4) este guardrail se aplica. |
| **Exemplo concreto** | Um exemplo correto e um incorreto, ancorados em documentos/chunks reais do Anexo A/B. |
| **Prioridade** *(novo, v0.2)* | **Crítico** (ligado a um dos 3 incidentes reais, ou risco equivalente), **Importante** (risco relevante mas sem incidente registrado, ou papel de apoio a um guardrail Crítico), **Desejável** (qualidade/consistência, risco baixo). Adicionado a pedido do P.O. Senior para orientar prioridade de implementação. |
| **Status** | **Vigente** ou **Revisável** (decisão consciente, explicitamente aberta a mudança futura). |

### Nota sobre enforcement em QUANDO EM DÚVIDA
Os guardrails desta seção são regras de decisão contextual — aplicadas *durante* a geração da resposta, quando um guardrail DEVE/NÃO DEVE não resolve sozinho a situação. Por isso, não recebem uma classificação Código/Prompt própria: são predominantemente **Prompt** por natureza, com apoio de código apenas quando dependem de um guardrail DEVE/NÃO DEVE já classificado (indicado no próprio item).

### 3 incidentes usados como referência (testes internos, Cenário 1)
| # | Incidente |
|---|---|
| **1** | Assistente respondeu que o prazo de devolução para carga perigosa é 7 dias — quando, na verdade, cargas perigosas **não podem ser devolvidas** pelo processo padrão (POL-001 §3.2). |
| **2** | Assistente citou "PROC-042, seção 2" mas os multiplicadores informados eram da **versão 1** (desatualizada), não da v2 (vigente). |
| **3** | Assistente disse "Não encontrei informação sobre isso" para uma pergunta sobre SLA Gold, mas o documento SLA-2024 estava indexado e continha a resposta. |

---

## 2. DEVE

### GR-D-01 — Sempre citar a fonte de cada resposta, incluindo identificação de versão quando o documento tiver mais de uma versão indexada
| | |
|---|---|
| **Enforcement** | Código |
| **Justificativa** | Checável mecanicamente: a resposta deve conter um identificador de documento/versão presente nos chunks efetivamente recuperados. |
| **Risco de negócio** | Cliente recebe informação (prazo, valor, regra) sem possibilidade de conferência — perda de confiança do atendente na ferramenta e risco de repasse de informação não confiável ao cliente final. |
| **Incidente(s)** | 2 |
| **Bounded Context** | Consulta e Resposta do Assistente / Governança Documental |
| **Exemplo** | ✅ "Multiplicador para o Sudeste: 1,1 (fonte: PROC-042 **v2**, §2.1)." ❌ "Multiplicador para o Sudeste: 1,1 (fonte: PROC-042)" — sem indicar qual versão. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-D-02 — Garantir a recuperação e exibição de todas as versões conhecidas de um documento com contradição registrada
| | |
|---|---|
| **Enforcement** | Híbrido |
| **Justificativa** | A garantia de recuperação não pode depender só do ranking por similaridade semântica (risco técnico já sinalizado no VC-11 do `requirements.md`); a exibição separada e clara de cada versão na resposta é responsabilidade do prompt. |
| **Risco de negócio** | Decisão comercial (ex: frete cobrado) baseada em só uma versão gera cobrança divergente do que o cliente foi informado — risco de disputa comercial. |
| **Incidente(s)** | 2 |
| **Bounded Context** | Frete Especial / Governança Documental |
| **Exemplo** | ✅ "Sudeste: 1,0 (PROC-042 v1) e 1,1 (PROC-042 v2) — as duas versões coexistem sem vigência formal definida." ❌ Resposta cita só a v2, escondendo que a v1 também está indexada e diverge. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-D-03 — Esgotar a busca em todos os contextos de conhecimento relevantes à pergunta antes de declarar "não encontrei informação"
| | |
|---|---|
| **Enforcement** | Código |
| **Justificativa** | Cobertura de busca (threshold de similaridade, recall mínimo por bounded context) é parametrizável e testável — não é uma "decisão" do modelo. |
| **Nota de conflito (v0.2)** | "Esgotar a busca" significa cobrir os bounded contexts relevantes à pergunta **dentro do orçamento de tempo já definido** pela constraint de latência (`requirements.md` §3/VC-10, p95 < 30s) — não uma busca sem limite. Achado da revisão P.O. Senior: sem essa referência explícita, este guardrail poderia ser lido como conflitante com a constraint de latência já aprovada. **O que fazer se o tempo se esgotar antes da cobertura completa está definido no GR-Q-05** (achado da revisão Tech Lead). |
| **Nota de viabilidade técnica (referência Dev Sênior)** | Garantir cobertura de busca "dentro do orçamento de tempo" pressupõe instrumentação de recall/cobertura por bounded context em tempo real — não é um ajuste trivial de configuração. Precisa de validação de um Dev Sênior real antes de ser assumido como resolvido no `plan.md`. |
| **Risco de negócio** | Atendente escala desnecessariamente um caso que já tinha resposta pronta, aumentando o tempo de atendimento — e, no caso do Incidente 3, arrisca violar o próprio SLA que a resposta não encontrada deveria descrever. |
| **Incidente(s)** | 3 |
| **Bounded Context** | Consulta e Resposta do Assistente (exemplo em SLA e Clientes) |
| **Exemplo** | ✅ Busca no SLA-2024 antes de responder sobre o SLA do cliente Gold, dentro do tempo de resposta esperado. ❌ (= Incidente 3 exato) Responde "não encontrei" mesmo com o chunk SLA-2024-B indexado e cobrindo a pergunta. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-D-04 — Verificar explicitamente se a situação se enquadra em uma exceção documentada antes de aplicar a regra geral correspondente
| | |
|---|---|
| **Enforcement** | Híbrido |
| **Justificativa** | A detecção da categoria que aciona a exceção (ex: classes ANTT 1-6 de carga perigosa) é determinística; mas redigir corretamente a distinção entre regra geral e exceção ainda depende de compreensão semântica do modelo. |
| **Risco de negócio** | Aprovação indevida de devolução de carga perigosa gera exposição regulatória (ANTT) e risco operacional/de segurança real — não apenas um erro de atendimento. |
| **Incidente(s)** | 1 |
| **Bounded Context** | Devolução de Mercadorias |
| **Exemplo** | ✅ Identifica que a carga é perigosa (POL-001-B) e informa que a devolução padrão não se aplica, antes de citar o prazo geral. ❌ (= Incidente 1 exato / Armadilha 4 do Anexo B) Aplica o prazo geral de 7 dias (POL-001-A) a uma carga perigosa, ignorando a exceção da seção 3.2. |
| **Nota de cobertura de teste (achado QA, v0.3)** | O POL-001 §3.2 documenta **3 categorias de exceção**: carga perigosa (classes 1-6 ANTT), carga refrigerada com cadeia de frio rompida, e carga com lacre de segurança violado sem documentação de entrega. Os casos de teste deste guardrail devem cobrir as 3 categorias — não só carga perigosa, única testada pelo Incidente 1. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-D-05 — Sinalizar o nível de confiança (Alta/Média/Baixa) em toda resposta
| | |
|---|---|
| **Enforcement** | Código |
| **Justificativa** | O cálculo de confiança já é uma regra determinística definida no `domain-model.md` (nº de fontes formais usadas, presença ou não de contradição). |
| **Risco de negócio** | Atendente repassa ao cliente uma informação divergente como se fosse certeza, sem saber que precisa validar — mina a credibilidade do atendimento quando o erro aparece depois. |
| **Incidente(s)** | 1, 2 |
| **Bounded Context** | Consulta e Resposta do Assistente |
| **Exemplo** | ✅ Resposta sobre o multiplicador do Sudeste marcada como confiança **Baixa** (contradição v1/v2 presente). ❌ Mesma resposta marcada como Alta, escondendo a divergência do atendente. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-D-06 — Responder sempre em português formal
| | |
|---|---|
| **Enforcement** | Híbrido *(atualizado v0.3 — antes só Prompt)* |
| **Justificativa** | A "formalidade" da resposta em si ainda depende de geração de texto (Prompt); mas um proxy operacional determinístico pode ser verificado por código: presença de emojis, gírias de uma lista fechada, ou abreviações informais. Isso viabiliza um teste automatizado parcial, mesmo sem cobrir 100% da formalidade percebida. |
| **Critério operacional objetivo (achado QA, v0.3)** | Reprovar automaticamente se a resposta contiver: (a) emojis; (b) gírias de uma lista fechada (ex: "beleza", "valeu", "pow", "mano"); (c) abreviações informais (ex: "vc", "pra", "tá", "blz"); (d) idioma diferente do português. Lista a ser mantida e expandida pela QA conforme casos reais surgirem. |
| **Observação (v0.2)** | Mantido sem incidente correspondente por decisão de produto (P.O. Senior): o guardrail existe para mitigar o risco de o assistente alternar de idioma ou usar tom fora do padrão formal esperado pelo atendente — o que geraria confusão ou uma percepção de inconsistência/pouca confiabilidade da ferramenta, mesmo sem haver, até o momento, um caso de falha factual registrado para esse comportamento específico. |
| **Tipo de grounding (avaliação final, v0.5)** | Boa prática de produto, sem dado quantitativo ou decisão técnica prévia por trás — o mais fraco dos guardrails sem incidente neste critério, mantido conscientemente. |
| **Risco de negócio** | Percepção de ferramenta inconsistente ou não confiável pelo atendente, mesmo sem erro de conteúdo — risco reputacional interno à operação, não financeiro/regulatório direto. |
| **Incidente(s)** | sem incidente correspondente |
| **Bounded Context** | Consulta e Resposta do Assistente (transversal) |
| **Exemplo** | ✅ "O prazo de devolução é de 7 dias úteis." ❌ Gírias, abreviações informais ("vc", "pra", "tá"), emojis, ou resposta em outro idioma. |
| **Prioridade** | Desejável |
| **Status** | Vigente |

### GR-D-07 — Sempre exibir a data de última atualização do documento-fonte junto com a resposta
| | |
|---|---|
| **Enforcement** | Código |
| **Justificativa** | A data de atualização é metadado do documento indexado — extração e exibição são mecânicas, não dependem de geração livre do modelo. |
| **Risco de negócio** | Sem a data visível, o atendente não tem como desconfiar de uma versão desatualizada mesmo quando ela é citada corretamente — reduz a chance de o próprio atendente pegar um erro como o do Incidente 2 antes de repassar ao cliente. |
| **Incidente(s)** | 2 |
| **Bounded Context** | Governança Documental |
| **Exemplo** | ✅ "PROC-042 v2 (última atualização: 10/11/2023)... PROC-042 v1 (última atualização: 03/03/2023)" — o atendente vê que a v1 é anterior. ❌ Cita "PROC-042" sem nenhuma data, como no Incidente 2. |
| **Prioridade** | Importante |
| **Status** | Vigente |
| **Origem** | Promovido do backlog de guardrails candidatos da Prática 1 (`005-Guardrails.md`, item "Transparência de vigência da fonte"). |

### GR-D-08 — Toda resposta com nível de confiança Baixa deve ser sinalizada como requerendo validação humana antes de ser repassada ao cliente final
| | |
|---|---|
| **Enforcement** | Híbrido |
| **Justificativa** | O rótulo de confiança Baixa já é calculado deterministicamente pelo `query-endpoint` (GR-D-05); a aplicação do bloqueio/aviso ao atendente na interface é responsabilidade do `teams-bot` (fora do escopo direto deste módulo — ver nota de dependência abaixo). |
| **Nota de dependência de módulo** | Este guardrail é transversal: o `query-endpoint` garante que o campo de confiança está sempre presente e correto (já coberto por GR-D-05); a exibição do aviso/bloqueio na UI do Teams é responsabilidade do `teams-bot`, consistente com a divisão de escopo já registrada no `requirements.md` §3 ("escalonamento para humano é decisão/UI do teams-bot"). |
| **Risco de negócio** | Uma informação internamente incerta (confiança Baixa) chega ao cliente final sem qualquer aviso, transformando uma divergência interna conhecida numa comunicação oficial errada ao cliente — o mesmo tipo de exposição que o Incidente 2 já demonstrou ser real. |
| **Incidente(s)** | 2 |
| **Bounded Context** | Consulta e Resposta do Assistente / Governança Documental |
| **Exemplo** | ✅ Resposta sobre o multiplicador do Sudeste (confiança Baixa) vem acompanhada de aviso explícito: "requer validação antes de repassar ao cliente". ❌ Atendente repassa a resposta de confiança Baixa diretamente ao cliente, sem qualquer sinalização adicional além do rótulo "Baixa". |
| **Prioridade** | Crítico |
| **Status** | Vigente |
| **Origem** | Novo — incluído a partir da revisão P.O. Senior desta subfase (Exercicio 2.2), redigindo explicitamente algo que antes só estava implícito na definição de confiança do `domain-model.md` (princípio "o óbvio deve ser dito"). |

### GR-D-09 — Considerar o histórico da conversa (limitado a 3 turnos, ADR-0002) para resolver referências implícitas em perguntas de acompanhamento
| | |
|---|---|
| **Enforcement** | Híbrido |
| **Justificativa** | Manter e passar os últimos 3 turnos é mecânico (gestão de janela de contexto, já definida na ADR-0002); decidir se a pergunta de acompanhamento muda de assunto ou permanece no mesmo bounded context é uma tarefa semântica. |
| **Risco de negócio** | Perder o contexto da pergunta anterior obriga o atendente a reformular a pergunta inteira, aumentando o tempo de atendimento — contraria diretamente o Outcome 1 do `requirements.md` (resposta rápida o suficiente para não travar o atendimento). |
| **Incidente(s)** | sem incidente correspondente |
| **Bounded Context** | Consulta e Resposta do Assistente (transversal) |
| **Exemplo** | ✅ Atendente pergunta "frete para o Sul, 600kg?" e depois "e para o Nordeste?" — assistente entende que a segunda pergunta é sobre o mesmo tema (frete especial), só mudando a região. ❌ Trata "e para o Nordeste?" como pergunta sem contexto e pede para o atendente reformular do zero. |
| **Prioridade** | Desejável |
| **Status** | Vigente |
| **Origem** | Incluído a partir da revisão Tech Lead desta subfase, por consistência com o GR-D-06 (idioma) — comportamento já "óbvio" no `domain-model.md` §4, mas sem guardrail formal correspondente. |
| **Tipo de grounding (avaliação final, v0.5)** | Constraint técnica já aprovada (ADR-0002, limite de 3 turnos) — não é uma invenção desta subfase, só a formalização explícita de um comportamento já decidido em fase anterior. |

---

## 3. NÃO DEVE

### GR-N-01 — Nunca responder com informação que não esteja explicitamente escrita em uma fonte indexada
| | |
|---|---|
| **Enforcement** | Híbrido |
| **Justificativa** | Prompt como instrução central (restrição à base documental, guardrail já central no `domain-model.md`); código como camada de verificação pós-geração (grounding check contra os chunks recuperados). |
| **Risco de negócio** | Qualquer invenção de regra pode gerar exposição contratual, regulatória ou financeira, dependendo do tema — no caso do Incidente 1, exposição regulatória por afirmar uma devolução que não pode ocorrer. |
| **Incidente(s)** | 1 |
| **Bounded Context** | Consulta e Resposta do Assistente (transversal) |
| **Exemplo** | ✅ Diz que não há informação sobre frete padrão abaixo de 500kg (Armadilha 5 do Anexo B). ❌ (= Incidente 1) Inventa um prazo de devolução de 7 dias para carga perigosa, sem checar a exceção. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-N-02 — Nunca citar uma versão de documento sem confirmar que é a versão efetivamente usada para extrair o valor citado
| | |
|---|---|
| **Enforcement** | Código |
| **Justificativa** | Verificação determinística: o identificador de versão citado deve corresponder ao chunk de onde o valor realmente veio — não a uma suposição do modelo sobre "qual é a mais recente". |
| **Risco de negócio** | Cobrança de frete com valores desatualizados gera disputa comercial com o cliente e retrabalho de estorno para a NovaTech. |
| **Incidente(s)** | 2 |
| **Bounded Context** | Frete Especial / Governança Documental |
| **Exemplo** | ✅ Cita explicitamente "PROC-042 v2, §2.1" ao usar o multiplicador 1,1. ❌ (= Incidente 2 exato) Cita "PROC-042, seção 2" usando o multiplicador 1,0 (chunk PROC-042-B, da v1), como se fosse a v2 vigente. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-N-03 — Nunca misturar valores/parâmetros de duas versões diferentes de um mesmo documento numa única resposta
| | |
|---|---|
| **Enforcement** | Código |
| **Justificativa** | Checagem determinística: todos os parâmetros numéricos usados numa resposta devem pertencer à mesma versão citada. |
| **Risco de negócio** | Gera um valor de frete que não corresponde a nenhuma versão oficial documentada — impossível de defender ou auditar depois, mesmo internamente. |
| **Incidente(s)** | 2 |
| **Bounded Context** | Frete Especial / Governança Documental |
| **Exemplo** | ✅ Usa fator de peso e multiplicador regional ambos da v2. ❌ (= Armadilha 1 do Anexo B) Combina o fator de peso da v1 (1,2) com o multiplicador regional da v2 (1,1) na mesma resposta. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-N-04 — Nunca usar o FAQ informal como fonte de resposta
| | |
|---|---|
| **Enforcement** | Código |
| **Justificativa** | Filtro simples e determinístico na camada de recuperação — exclusão do documento FAQ-Atendimento do conjunto de fontes citáveis. |
| **Risco de negócio** | Informação de campo não validada (ex: liberação informal de carga perigosa com frete expresso) tratada como regra oficial gera exposição regulatória e contradiz a política formal vigente. |
| **Incidente(s)** | sem incidente correspondente |
| **Bounded Context** | Governança Documental |
| **Exemplo** | ✅ Para "carga perigosa com frete expresso", diz que não há fonte formal para o tema. ❌ (= Armadilha 2 do Anexo B) Responde com base no FAQ-32 ou FAQ-38, com confiança Alta, como se fosse fonte normativa. |
| **Grounding concreto (avaliação final, v0.5)** | Sem ser um dos 3 incidentes, este guardrail tem risco documentado real: a **Armadilha 2 do Anexo B** ("FAQ como fonte para informação crítica") descreve exatamente este cenário como um caso de teste proposital. |
| **Prioridade** | Importante |
| **Status** | **Revisável** — decisão conservadora registrada no `domain-model.md` v0.4 §4, explicitamente aberta a mudança futura. |

### GR-N-05 — Nunca minimizar um tema classificado como sensível ou crítico
| | |
|---|---|
| **Enforcement** | Híbrido |
| **Justificativa** | A detecção do tema como sensível/crítico pode ser determinística (lookup contra a definição de incidente crítico do SLA-2024 §3 e a lista de classes ANTT de carga perigosa); o reforço redacional do encaminhamento depende de geração de texto. |
| **Risco de negócio** | Falha em reforçar o encaminhamento correto num tema de risco (carga perigosa, incidente crítico) pode gerar dano real de segurança ou descumprimento regulatório — não apenas um erro comum de atendimento. |
| **Incidente(s)** | sem incidente correspondente |
| **Bounded Context** | SLA e Clientes / Devolução de Mercadorias |
| **Exemplo** | ✅ Mesmo respondendo parcialmente sobre carga perigosa, sempre reforça o encaminhamento ao ramal 4500 (Gestão de Riscos). ❌ Informa a exceção sem mencionar o canal de tratamento individual. |
| **Tipo de grounding (avaliação final, v0.5)** | Backlog de guardrail já validado em fase anterior do projeto (`005-Guardrails.md`, Prática 1) — passou por um crivo antes de ser promovido aqui, não é uma criação sem lastro. |
| **Prioridade** | Importante |
| **Status** | Vigente |
| **Origem** | Promovido do backlog de guardrails candidatos da Prática 1 (`005-Guardrails.md`, item "Nunca minimizar tema sensível/crítico"). |

### GR-N-06 — Nunca aplicar automaticamente uma regra documentada como restrita a um tier/segmento específico a outros tiers, sem confirmação explícita na fonte
| | |
|---|---|
| **Enforcement** | Código |
| **Justificativa** | Checagem determinística de estrutura de dados: cada valor de SLA está associado a um tier específico na tabela indexada (SLA-2024 §2). |
| **Risco de negócio** | Cliente de tier inferior recebe um benefício não contratado (ex: gerente dedicado do Gold), gerando custo operacional não previsto — ou, no sentido oposto, um cliente deixa de receber um benefício ao qual tem direito. Custo operacional é sempre um risco relevante a gerenciar. |
| **Incidente(s)** | sem incidente correspondente |
| **Bounded Context** | SLA e Clientes |
| **Exemplo** | ✅ "Gerente de conta dedicado" confirmado como "Sim" só para clientes Gold; para Silver/Standard, responde "Não" — mesma lógica se aplica a qualquer campo tier-específico da tabela SLA-2024 (tempo de resposta, tempo de resolução, disponibilidade do portal de tracking, relatório mensal de performance). ❌ Generaliza um benefício ou prazo exclusivo de um tier (ex: gerente dedicado do Gold, ou o tempo de resposta de 2h do Gold) para outro tier, sem checar a tabela. |
| **Nota de cobertura de teste (achado QA, v0.3)** | Os casos de teste deste guardrail devem cobrir mais de um campo tier-específico da tabela SLA-2024 — não só "gerente de conta dedicado". |
| **Tipo de grounding (avaliação final, v0.5)** | Backlog de guardrail já validado em fase anterior do projeto (`005-Guardrails.md`, Prática 1) — mesma origem do GR-N-05. |
| **Prioridade** | Importante |
| **Status** | Vigente |
| **Origem** | Promovido do backlog de guardrails candidatos da Prática 1 (`005-Guardrails.md`, item "Não generalizar regra restrita a um segmento"). |

### GR-N-07 — Nunca tentar responder ou ser útil em perguntas sem relação com o domínio do assistente
| | |
|---|---|
| **Enforcement** | Híbrido |
| **Justificativa** | Uma triagem inicial por classificador de tema (o assunto não pertence a nenhum dos 3 bounded contexts de conhecimento: Devolução, Frete Especial, SLA) pode ser determinística; mas declinar de forma educada e informar o escopo correto é geração de texto. |
| **Risco de negócio** | Sem esse guardrail, o assistente pode reforçar o hábito de responder com conhecimento geral do modelo mesmo fora de qualquer tema de negócio — mesmo tipo de risco de confiabilidade que motiva a restrição à base documental (GR-N-01), mas para quando nem sequer há tema de negócio envolvido. |
| **Incidente(s)** | sem incidente correspondente |
| **Bounded Context** | Consulta e Resposta do Assistente (transversal) |
| **Exemplo** | ✅ "Previsão do tempo não é algo que eu cubro — posso ajudar com dúvidas sobre SLA, frete especial ou devolução." ❌ Tenta responder sobre o clima, ou qualquer tema de conhecimento geral, "para ser útil". |
| **Nota de viabilidade técnica (referência Dev Sênior)** | Um classificador determinístico de "fora do domínio" pode gerar falsos positivos/negativos em perguntas de fronteira — precisa de avaliação empírica com exemplos reais (informações complementares ainda não disponíveis nesta subfase) antes de ser considerado pronto. |
| **Grounding concreto (avaliação final, v0.5)** | Sem ser um dos 3 incidentes, este guardrail tem risco documentado real: **VC-12 e VC-14**, já aprovados no `requirements.md` v0.5, testam exatamente este comportamento de fronteira. |
| **Prioridade** | Crítico |
| **Status** | Vigente |
| **Origem** | Incluído a partir da revisão Tech Lead desta subfase — cobre o comportamento de fronteira "fora do domínio" (`domain-model.md` §4, VC-12/VC-14 do `requirements.md`), que não tinha guardrail correspondente até a v0.2. |

---

## 4. QUANDO EM DÚVIDA

### GR-Q-01 — Se o tipo de carga ou situação for ambíguo quanto a se enquadrar numa exceção documentada, perguntar ao atendente antes de assumir a regra geral
| | |
|---|---|
| **Risco de negócio** | Mesmo risco do GR-D-04 — presumir a regra geral sem confirmar a exceção gera o mesmo tipo de exposição do Incidente 1. |
| **Incidente(s)** | 1 |
| **Bounded Context** | Devolução de Mercadorias |
| **Exemplo** | Pergunta do atendente não informa se a carga é perigosa/refrigerada/lacre violado — assistente pergunta a categoria da carga antes de aplicar o prazo geral de 7 dias. Depende de GR-D-04. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-Q-02 — Se houver contradição de fonte e a vigência não estiver formalmente definida, mostrar ambas as versões com nível de confiança Baixa — nunca escolher uma unilateralmente
| | |
|---|---|
| **Risco de negócio** | Mesmo risco do GR-D-02 — decisão unilateral sobre qual versão "vale" pode contradizer o que o Compliance decidir depois, gerando retrabalho ou disputa. |
| **Incidente(s)** | 2 |
| **Bounded Context** | Governança Documental / Frete Especial |
| **Exemplo** | Pergunta sobre multiplicador do Sudeste — mostra 1,0 (v1) e 1,1 (v2), confiança Baixa, sem tentar decidir qual "vale". Depende de GR-D-02. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-Q-03 — Se a similaridade da busca estiver abaixo do limiar de confiança, mas a pergunta ainda estiver dentro do domínio do assistente, declarar "não encontrei com segurança" somente após confirmar que a busca cobriu os contextos relevantes
| | |
|---|---|
| **Risco de negócio** | Mesmo risco do GR-D-03 — escalonamento desnecessário ou, pior, resposta inventada por pressa de responder rápido. |
| **Incidente(s)** | 3 |
| **Bounded Context** | Consulta e Resposta do Assistente |
| **Exemplo** | Pergunta sobre multiplicador de frete por região de *origem* (não documentado, só existe por destino) — confirma que buscou em Frete Especial antes de declarar ausência de informação (VC-13 do `requirements.md`). Depende de GR-D-03. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-Q-04 — Se a pergunta cruzar mais de um contexto de conhecimento e apenas parte tiver fonte suficiente, responder a parte com fonte e declarar explicitamente a ausência de fonte na outra parte
| | |
|---|---|
| **Risco de negócio** | Cliente recebe resposta incompleta apresentada como completa — atendente pode repassar informação parcial sem perceber a lacuna, gerando reclamação futura. Afeta ~15% dos casos (dado de discovery) — volume relevante o suficiente para justificar prioridade máxima mesmo sem um incidente específico já registrado. |
| **Incidente(s)** | sem incidente correspondente |
| **Bounded Context** | Consulta e Resposta do Assistente (transversal) |
| **Exemplo** | Pergunta cruza devolução (com fonte) e um tema sem fonte — responde a parte de devolução citando POL-001 e declara explicitamente que não há informação para a outra parte, sem fundir num resumo genérico. |
| **Tipo de grounding (avaliação final, v0.5)** | Dado quantitativo real do discovery (~15% dos casos são multi-domínio) — não é um incidente, mas é evidência concreta de frequência/volume real. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

### GR-Q-05 — Se o orçamento de tempo (p95 < 30s, VC-10) estiver prestes a se esgotar antes de cobrir todos os bounded contexts relevantes à pergunta, permitir uma extensão pequena e limitada da busca apenas para fechar a cobertura já identificada como necessária; se mesmo essa extensão não bastar, retornar uma resposta parcial sinalizada como possivelmente incompleta
| | |
|---|---|
| **Risco de negócio** | Mesmo risco do GR-D-03/GR-Q-03 — deixar esse desempate indefinido no momento de maior pressão (tempo se esgotando) gera comportamento ad-hoc e inconsistente entre execuções, o que é pior do que qualquer uma das duas opções isoladas. |
| **Nota operacional — KPI (achado da revisão Tech Lead)** | A frequência com que essa extensão limitada é acionada — e a frequência com que, mesmo assim, é necessário retornar resposta parcial — deve ser monitorada como métrica operacional (candidato a indicador do `painel-web`, subfase futura). Isso evita que buscas perdidas pela restrição de latência se tornem um ponto cego silencioso. |
| **Nota de dependência de módulo** | A métrica de KPI acima depende do `painel-web`, módulo sem spec própria ainda nesta fase — dependência de sequenciamento entre subfases futuras (mesmo padrão de nota já usado no GR-D-08 para o `teams-bot`). |
| **Nota de viabilidade técnica (referência Dev Sênior)** | Implementar uma extensão de tempo adaptativa (pequena e limitada, só quando necessário) é mais complexo que um timeout fixo — exige instrumentação de decisão em tempo de execução. Precisa de validação de um Dev Sênior real antes de ser assumido como resolvido no `plan.md`. |
| **Incidente(s)** | 3 (indiretamente — mesma causa raiz de busca incompleta) |
| **Bounded Context** | Consulta e Resposta do Assistente |
| **Exemplo** | Pergunta cruza Frete Especial e SLA e Clientes; a busca em Frete Especial já terminou, mas a busca em SLA ainda está em andamento quando o orçamento de tempo está quase no limite — sistema permite uma pequena extensão só para fechar a busca em SLA; se ainda não bastar, responde a parte de Frete Especial normalmente e sinaliza que a parte de SLA pode estar incompleta. Depende de GR-D-03. |
| **Prioridade** | Crítico |
| **Status** | Vigente |

---

## 5. Matriz de Rastreabilidade (Incidente → Guardrails)

| Incidente | Guardrails que previnem |
|---|---|
| **1** — Prazo inventado para carga perigosa | GR-D-04, GR-D-05, GR-N-01, GR-Q-01 |
| **2** — Versão errada do PROC-042 citada | GR-D-01, GR-D-02, GR-D-05, GR-D-07, GR-D-08, GR-N-02, GR-N-03, GR-Q-02 |
| **3** — Falso "não encontrei" para SLA Gold | GR-D-03, GR-Q-03, GR-Q-05 |
| **Sem incidente correspondente** (guardrails de negócio/backlog válidos, sem caso de falha reportado) | GR-D-06, GR-D-09, GR-N-04, GR-N-05, GR-N-06, GR-N-07, GR-Q-04 |

## 6. Matriz de Prioridade

| Prioridade | Guardrails |
|---|---|
| **Crítico** (15) | GR-D-01, GR-D-02, GR-D-03, GR-D-04, GR-D-05, GR-D-08, GR-N-01, GR-N-02, GR-N-03, GR-N-07, GR-Q-01, GR-Q-02, GR-Q-03, GR-Q-04, GR-Q-05 |
| **Importante** (4) | GR-D-07, GR-N-04, GR-N-05, GR-N-06 |
| **Desejável** (2) | GR-D-06, GR-D-09 |

---

## 7. Matriz de Rastreabilidade (Guardrail → Verification Criteria do requirements.md)

Complementar à Seção 5 (Guardrail → Incidente) — conecta cada guardrail ao(s) caso(s) de teste já aprovado(s) no `requirements.md` (v0.5), para uso de QA na manutenção da suíte de testes. Achado da revisão QA desta subfase.

| ID | Guardrail (resumo) | VC(s) relacionado(s) |
|---|---|---|
| GR-D-01 | Citar fonte + versão | VC-01, VC-02 |
| GR-D-02 | Garantir recuperação/exibição de todas versões em contradição | VC-03, VC-04, VC-11 |
| GR-D-03 | Esgotar busca antes de "não encontrei" | VC-02, VC-10 |
| GR-D-04 | Verificar exceção antes da regra geral | VC-06, VC-09 |
| GR-D-05 | Sinalizar nível de confiança | Transversal — coluna "nível de confiança esperado" presente em todas as tabelas de VC (5.1 a 5.5) |
| GR-D-06 | Responder em português formal | Nenhum VC direto |
| GR-D-07 | Exibir data de vigência | VC-04 (apoio) |
| GR-D-08 | Confiança Baixa → validação humana | Nenhum VC direto (depende de UI do `teams-bot`, fora do escopo dos VCs do `query-endpoint`) |
| GR-D-09 | Multi-turn (3 turnos) | Nenhum VC direto (coberto como constraint técnica, ADR-0002, não testado via VC) |
| GR-N-01 | Nunca inventar informação sem fonte | VC-05, VC-06, VC-07 |
| GR-N-02 | Nunca citar versão sem confirmar | VC-03, VC-04 |
| GR-N-03 | Nunca misturar valores de versões diferentes | VC-03, VC-04 |
| GR-N-04 | Nunca usar FAQ como fonte | VC-08 |
| GR-N-05 | Nunca minimizar tema sensível/crítico | Nenhum VC direto |
| GR-N-06 | Nunca generalizar regra de tier | Nenhum VC direto (relacionado, mas não idêntico, ao VC-05) |
| GR-N-07 | Nunca responder fora do domínio | VC-12, VC-14 |
| GR-Q-01 | Perguntar se ambíguo quanto a exceção | VC-06 (cenário correlato) |
| GR-Q-02 | Mostrar ambas versões, confiança Baixa | VC-03, VC-04 |
| GR-Q-03 | "Não encontrei com segurança" só após confirmar busca | VC-13 |
| GR-Q-04 | Multi-domínio parcial | VC-09 |
| GR-Q-05 | Desempate latência x cobertura | VC-10 |

**Nota QA:** 6 guardrails (GR-D-06, GR-D-08, GR-D-09, GR-N-05, GR-N-06, e parcialmente GR-Q-01) não têm um VC direto e exclusivo no `requirements.md` atual — não é um erro (nem todo guardrail nasce de um caso de teste formal), mas fica registrado aqui como candidato a ganhar VC próprio numa futura revisão do `requirements.md`.

---

## 8. Guardrails Revisáveis — atenção especial

| ID | Guardrail | Motivo de revisão futura |
|---|---|---|
| GR-N-04 | Nunca usar o FAQ informal como fonte de resposta | Decisão conservadora (ver `domain-model.md` v0.4 §4) — se reintroduzida, deve vir sempre com confiança Baixa e aviso explícito de fonte não validada. |

---

## 9. Avaliação Final — Critérios de Aceite desta Subfase

Avaliação formal contra os 3 critérios definidos pelo usuário para esta subfase (Exercicio 2.2), rodada após as revisões de P.O. Senior, Tech Lead, QA e as referências leves de Dev Sênior/Delivery Manager.

| Critério | Resultado |
|---|---|
| **1. Guardrails específicos ao domínio NovaTech, não genéricos** | 20 de 21 guardrails ancorados em documentos/exemplos reais (POL-001, PROC-042, SLA-2024, Anexo B). Exceção conhecida e aceita: **GR-D-06** (idioma formal), mantido por decisão de produto mesmo sendo o mais genérico. |
| **2. Classificação prompt vs código com compreensão correta (probabilístico vs determinístico)** | 21 de 21 — todas as justificativas raciocinam corretamente sobre o que é mecanicamente checável (código) vs o que depende de geração de texto (prompt), inclusive nos casos Híbridos. Os 5 itens de QUANDO EM DÚVIDA não recebem essa classificação por decisão de design explícita (Seção 1). |
| **3. Rastreabilidade a um risco concreto** | **14 guardrails** ligados diretamente a um dos 3 incidentes (GR-D-01, D-02, D-03, D-04, D-05, D-07, D-08, N-01, N-02, N-03, Q-01, Q-02, Q-03, Q-05); **2 adicionais** (GR-N-04, GR-N-07) ligados a evidência concreta documentada (Armadilha 2 do Anexo B; VC-12/VC-14 do `requirements.md`) — total de **16/21 (76%) com grounding concreto documentado**. Os **5 restantes** (GR-D-06, GR-D-09, GR-N-05, GR-N-06, GR-Q-04) têm seu tipo de grounding explicitado individualmente (constraint técnica aprovada, backlog validado em fase anterior, dado quantitativo de discovery, ou boa prática sem dado) — nenhum é uma invenção sem lastro, mas nem todos vêm de um incidente ou caso de teste formal. |

**Conclusão:** documento pronto para fechamento desta subfase, com as exceções acima cientes e conscientemente aceitas pelo usuário — não são lacunas não identificadas, são trade-offs registrados. Aprovação final é decisão do usuário, não deste documento.

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 0.1 | 2026-08-13 | Product Specialist (Claude) + Arlindo | Rascunho inicial — 17 guardrails (7 DEVE, 6 NÃO DEVE, 4 QUANDO EM DÚVIDA). Pendente de checkpoint. |
| 0.2 | 2026-08-13 | P.O. Senior (Claude) + Arlindo | Revisão P.O. Senior: adicionado campo "Risco de negócio" em todos os guardrails; adicionada nota de conflito latência x busca em GR-D-03; adicionado novo guardrail GR-D-08 (confiança Baixa → validação humana); observação melhorada em GR-D-06; adicionado campo "Prioridade" em todos os guardrails + nova Seção 6 (Matriz de Prioridade); Matriz de Rastreabilidade (Seção 5) atualizada com GR-D-08. Total: 18 guardrails (8 DEVE, 6 NÃO DEVE, 4 QUANDO EM DÚVIDA). |
| 0.3 | 2026-08-13 | Tech Lead (Claude) + Arlindo | Revisão Tech Lead: adicionado GR-N-07 (cobre o comportamento de fronteira "fora do domínio", VC-12/VC-14, que não tinha guardrail correspondente); adicionado GR-D-09 (multi-turn, ADR-0002, por consistência com GR-D-06); adicionado GR-Q-05 (desempate latência x cobertura de busca — extensão pequena e limitada, depois parcial sinalizado — com nota de KPI operacional a monitorar); GR-D-03 atualizado para referenciar o GR-Q-05. Matriz de Rastreabilidade e Matriz de Prioridade atualizadas. Total: 21 guardrails (9 DEVE, 7 NÃO DEVE, 5 QUANDO EM DÚVIDA). |
| 0.4 | 2026-08-13 | QA (Claude) + Arlindo | Revisão QA: GR-D-04 e GR-N-06 ganharam nota de cobertura de teste (POL-001 §3.2 tem 3 categorias de exceção, não só carga perigosa; SLA-2024 tem múltiplos campos tier-específicos, não só gerente dedicado); GR-D-06 (idioma) ganhou critério operacional objetivo e enforcement atualizado para Híbrido (proxy determinístico: emojis/gírias/abreviações); nova Seção 7 — Matriz Guardrail → Verification Criteria do `requirements.md`, complementar à matriz de incidentes. |
| 0.5 | 2026-08-13 | Referência Dev Sênior + Delivery Manager (Claude) + Arlindo | Referências leves (sem revisão formal completa, por decisão do usuário — Dev Sênior e Delivery Manager rebaixados a "sinalização de risco", não revisores plenos): nota de viabilidade técnica em GR-D-03, GR-Q-05 e GR-N-07 (mecanismos não triviais, pendentes de validação de Dev Sênior real); nota de dependência de módulo em GR-Q-05 (`painel-web`, mesmo padrão do GR-D-08). Observação de sequenciamento/volume (Delivery Manager) avaliada e **conscientemente não incorporada** ao documento — já coberta pela Matriz de Prioridade (Seção 6); sequenciar entrega é papel do `plan.md`, não deste documento. |
| 0.6 | 2026-08-13 | Product Specialist (Claude) + Arlindo | Avaliação final contra os 3 critérios de aceite (domínio específico, prompt vs código, rastreabilidade a incidente). Achados: GR-N-04 e GR-N-07 ganharam referência explícita ao grounding concreto que já tinham (Armadilha 2 do Anexo B; VC-12/VC-14) além de "sem incidente correspondente"; os 5 guardrails restantes sem incidente (GR-D-06, D-09, N-05, N-06, Q-04) ganharam nota de "tipo de grounding" (constraint técnica aprovada / backlog validado / dado quantitativo / boa prática sem dado). Nova Seção 9 — Avaliação Final — registra o resultado consolidado dos 3 critérios. |
| 0.7 | 2026-08-13 | Product Specialist (Claude) + Arlindo | Revisão geral de consistência e gramática, a pedido do usuário (nada de auto-aprovação antes da validação dele). Achados corrigidos: (1) erro de contagem na Seção 9 — o texto dizia "13 guardrails ligados a incidente" e "6 restantes, e parcialmente GR-Q-01", mas a contagem correta é **14** diretos e **5** restantes (GR-Q-01 não pertence a essa lista — já está ligado ao Incidente 1; a menção a ele vazou por engano da lista da Seção 7, que trata de um critério diferente — VC, não incidente); percentual corrigido de 71% para 76%. (2) GR-Q-01 a GR-Q-05 não tinham o campo Status, embora só o Enforcement seja isento para esses itens — adicionado "Vigente" aos 5, e cross-referência "Depende de GR-D-0X" completada onde faltava. (3) Linha "Origem" do cabeçalho, desatualizada desde a v0.2, reescrita para refletir todas as adições. (4) Prefixo "GR-" completado em todos os itens da Matriz de Prioridade (Seção 6), antes abreviado de forma inconsistente. Status revertido para rascunho — aprovação final é decisão do usuário. |
