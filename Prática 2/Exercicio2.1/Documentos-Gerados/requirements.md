# Requirements — query-endpoint (NovaTech Assistant)

> **Status:** ✅ Aprovado (v0.5) — Product Specialist, Tech Lead, revisão QA, avaliação formal contra critérios e conferência final de consistência.
> **Fontes usadas:** `domain-model.md` (v0.4, aprovado), spec de requisitos de RAG resumida, dados de discovery, ADRs resumidas da fase anterior (0001-0004), Anexo B (chunks e mapa de cobertura). Nenhum cruzamento com os documentos completos da Prática 1 foi necessário.
> **Destino final:** `specs/query-endpoint/requirements.md` no repositório do projeto (ainda não recebido como Anexo D — Starter Repo; por ora mantido em `Documentos-Gerados/`).
> **Aprovado por:** Product Specialist, Tech Lead (checkpoint realizado). **Pendente:** Dev Sênior — validação de viabilidade do requisito VC-11 (ver nota na seção 5), conforme convenção do Anexo C.

---

## 1. Outcomes

O que este módulo precisa entregar, em termos de valor — não de implementação:

1. **Resposta rápida o suficiente para não travar o atendimento ao vivo:** o atendente recebe a resposta às suas perguntas sobre SLA, frete especial ou devolução sem precisar fazer o cliente esperar na linha, sempre citando a fonte e o nível de confiança. (O limite objetivo de tempo que sustenta esse resultado está definido como constraint/critério de verificação — seção 3 e VC-10 — não repetido aqui.)
2. **Proteção contra erro por contradição de fonte:** quando duas fontes válidas divergem (ex: PROC-042 v1 x v2), o atendente vê ambas as versões explicitamente — nunca uma resposta que mistura ou esconde a divergência.
3. **Recusa honesta em vez de invenção:** quando a pergunta está fora do que a base documental cobre (gap conhecido, fora do domínio, ou sem chunk relevante), o atendente recebe uma resposta clara dizendo que não há informação — nunca uma resposta plausível, porém inventada.
4. **Resposta completa mesmo quando a dúvida cruza mais de um assunto:** quando a pergunta do cliente envolve, por exemplo, devolução e frete especial ao mesmo tempo (~15% dos casos, conforme discovery), o atendente não precisa fazer duas perguntas separadas — recebe uma única resposta que já cobre as duas partes, com cada uma atribuída à sua fonte/contexto de origem.

---

## 2. Scope Boundaries

Derivado diretamente dos bounded contexts definidos em `domain-model.md` (v0.4).

### Dentro do escopo
- Responder perguntas dos 3 contextos de conhecimento de negócio: **Devolução de Mercadorias**, **Frete Especial**, **SLA e Clientes**.
- Orquestração de busca + montagem de resposta (bounded context **Consulta e Resposta do Assistente**).
- **Consumo** do metadado de vigência (Governança Documental) para uso interno (ex: ranking de recuperação) — não a criação/manutenção desse metadado, e não a decisão do que é exibido (ver §4: diante de contradição, sempre mostra ambas as versões, independente da vigência).
- **Garantia de recuperação de todas as versões conhecidas** de um documento com contradição registrada (ex: PROC-042 v1 e v2) — não depender apenas do ranking de similaridade semântica para isso (ver constraint correspondente em §3 e VC-11 em §5).
- Detecção e tratamento de perguntas multi-domínio.
- Os 3 comportamentos de "pergunta fora do escopo" definidos no domain-model (gap conhecido, fora do domínio, sem chunk relevante).

### Fora do escopo (pertence a outro módulo ou outra subfase)
| Item | Pertence a |
|---|---|
| Geração de embeddings, chunking, indexação, criação/atualização do metadado de vigência | `pipeline-ingestao` |
| UI de chat, Adaptive Cards, escalonamento visual para supervisor/humano | `teams-bot` |
| Dashboard de métricas e histórico | `painel-web` |
| Registro/tratamento de sinalização de resposta incorreta pelo atendente | `feedback-api` |
| AGENTS.md, skills, configuração de MCP | Outras subfases da Prática 2 (fora do Exercicio 2.1) |

> **Nota:** a atribuição de responsabilidade por módulo acima foi usada apenas como **base de trabalho** para delimitar o escopo deste `requirements.md` — será revisada no momento oportuno, quando os módulos citados tiverem suas próprias specs.

### Gaps conhecidos (fora do escopo por ausência de fonte documental — ver domain-model.md §4)
Prazos de entrega/rastreamento, carga danificada em trânsito, seguro de carga, frete padrão (<500kg), desfecho da escalação à Gestão de Riscos.

---

## 3. Constraints

### Técnicas (herdadas das ADRs da fase anterior)
- **Modelo LLM:** Azure OpenAI GPT-4o, janela de 128K tokens (ADR-0001).
- **Busca:** Azure AI Search (ADR-0004).
- **Budget de contexto:** ~4K tokens para system prompt + ~8K tokens para chunks (até 5 chunks de ~1.500 tokens) + pergunta + histórico limitado a **3 turnos** (ADR-0002).
- **Metadado de vigência:** assumido como já existente nos documentos indexados, mantido pelo `pipeline-ingestao` (ADR-0003) — este módulo apenas o consome, e só para uso interno (não decide o que é exibido — ver §4).
- **Busca com garantia de cobertura para temas com contradição conhecida:** para documentos com contradição já identificada (ex: PROC-042 v1/v2), a recuperação deve garantir a inclusão de todas as versões conhecidas, não confiar apenas no ranking por similaridade semântica do top-k — do contrário, a garantia de "sempre mostrar ambas as versões" (seção 4) vira best-effort em vez de garantia real.
- **Frescor de dados:** a base documental é mantida dentro do **prazo padrão da política — até 24h** (conforme spec de RAG resumida). Este módulo opera sobre o índice já atualizado dentro desse prazo.

### De negócio / produto
- Resposta sempre em **português**.
- **Restrição à base documental:** nunca responde com conhecimento geral do modelo, apenas com o que está em fonte indexada.
- **FAQ informal não é usado como fonte de resposta** (decisão revisável, ver domain-model.md §4).
- **Latência técnica do endpoint:** p95 < 30 segundos (não inclui tempo de leitura do atendente) — metodologia de medição completa em VC-10, seção 5.6.

### De integração
> **Nota:** os dois pontos abaixo são definidos aqui como **base de trabalho**, necessária para dar limites a este `requirements.md` — serão revisados no momento oportuno, junto com as specs do `teams-bot` e do `painel-web`.

- O endpoint é consumido pelo `teams-bot` e pelo `painel-web` (Anexo C) — a resposta deve ser retornada em formato estruturado suficiente para esses consumidores montarem a UI: texto da resposta, fonte(s) citada(s) com identificação de versão quando aplicável, e nível de confiança (Alta/Média/Baixa).
- Escalonamento para humano é decisão/UI do `teams-bot`, não deste módulo — este módulo apenas fornece o nível de confiança e a informação de que não encontrou resposta, quando for o caso.

---

## 4. Prior Decisions

Decisões já fechadas que este módulo herda como premissa, sem rediscutir:

- **ADR-0001** — Modelo LLM: Azure OpenAI GPT-4o.
- **ADR-0002** — Estratégia de contexto: budget de tokens e histórico de 3 turnos.
- **ADR-0003** — Documentos contraditórios: metadado de vigência no pipeline; documentos obsoletos marcados, não excluídos. **Reconciliada nesta sessão (ver abaixo) quanto à instrução de priorizar a versão mais recente.**
- **ADR-0004** — Pipeline de RAG: Azure AI Search + Azure OpenAI.
- **domain-model.md (v0.4)** — 5 bounded contexts, linguagem ubíqua (com disambiguação de "Gold"/"Standard" para agentes de IA), fronteiras do assistente, nível de confiança padronizado (Alta/Média/Baixa), FAQ não usado como fonte.

### Reconciliação decidida: ADR-0003 x spec de RAG resumida desta sessão
A ADR-0003 instruía o modelo a *"priorizar versão mais recente"* diante de documentos contraditórios. A spec de RAG resumida desta sessão instrui: *"Fontes contraditórias devem mostrar ambas as versões."*

**Decisão (padrão desta sessão prevalece):** diante de qualquer contradição de fonte detectada — independente de a vigência estar definida ou não — o assistente **sempre mostra ambas as versões** ao atendente, nunca elegendo uma como "a resposta". A instrução de "priorizar a mais recente" da ADR-0003 deixa de valer para o que é **exibido** ao atendente; ela pode, no máximo, orientar decisões internas de recuperação (ex: qual chunk ranquear primeiro ao montar a resposta), mas nunca decide sozinha o que aparece na tela.

**Efeito prático no PROC-042:** o assistente sempre apresenta v1 e v2 lado a lado quando ambas forem recuperadas para a mesma pergunta, com nível de confiança **Baixa** (ver VC-03/VC-04 na seção 5) — mesmo se um dia o Compliance definir a vigência de uma delas.

---

## 5. Verification Criteria

Baseado no mapa de cobertura e nas "armadilhas" do Anexo B — usado como gabarito de aceite.

### 5.1 Casos de resposta correta (fonte formal, sem ambiguidade)
| # | Pergunta | Comportamento esperado | Nível de confiança esperado |
|---|---|---|---|
| VC-01 | "Qual o prazo de devolução?" | Cita POL-001-A e POL-001-B (7 dias úteis + exceções). | Alta |
| VC-02 | "Qual o SLA do cliente Gold?" | Cita SLA-2024-B (2h resposta / 24h resolução). | Alta |

### 5.2 Casos de contradição de fonte
| # | Pergunta | Comportamento esperado | Nível de confiança esperado |
|---|---|---|---|
| VC-03 | "Frete para 600kg para Manaus?" | A busca **garante** a recuperação de PROC-042 v1 e v2 (tema com contradição conhecida); resposta **mostra ambas as versões** explicitamente, sem misturar multiplicadores. | Baixa |
| VC-04 | "Qual o multiplicador para o Sudeste?" | Mesmo tratamento — v1 (1.0) e v2 (1.1) sempre recuperados juntos e nunca combinados numa única resposta. | Baixa |
| VC-11 | Qualquer pergunta que caia no tema "frete especial" (independente da fórmula exata da pergunta) | A recuperação inclui obrigatoriamente PROC-042 v1 **e** v2 — não apenas a versão mais semanticamente parecida com a pergunta. | Baixa (contradição sempre presente até a vigência ser resolvida) |

> **⚠️ Risco técnico a validar (lente de Dev Sênior, antes do checkpoint final):** VC-11 provavelmente exige um mecanismo de recuperação além de busca por similaridade simples (ex: um registro de "temas com contradição conhecida" que força a inclusão de documentos específicos) — não coberto pelas ADRs da fase anterior. Viabilidade técnica ainda não confirmada por um Dev Sênior real; sinalizado aqui para não virar surpresa no `plan.md`.

### 5.3 Casos de "armadilha" — o assistente NÃO deve fazer
| # | Situação | Comportamento proibido | Comportamento correto | Nível de confiança esperado |
|---|---|---|---|---|
| VC-05 | "Cliente diz que é Platinum. Existe esse tier?" | Inventar SLA para um tier inexistente. | Responder que só existem Gold, Silver e Standard (SLA-2024-A). | Alta |
| VC-06 | "Posso devolver carga perigosa?" | Confundir a exceção com a regra e dizer que pode. | Responder que **não pode** pelo processo padrão (POL-001-B), citando o encaminhamento à Gestão de Riscos (ramal 4500) sem detalhar o desfecho (gap). | Alta |
| VC-07 | "Frete para 300kg para Salvador?" | Extrapolar a fórmula de frete especial para uma faixa não coberta. | Responder que não há informação sobre frete padrão (<500kg) na base. | N/A (recusa por gap conhecido — não é uma resposta de negócio) |
| VC-08 | "O que acontece com carga danificada?" / "Carga perigosa com frete expresso?" | Responder com base no FAQ informal (itens 38 e 32). | Responder que não há fonte formal para o tema — **sem citar o FAQ** (decisão desta sessão). | N/A (recusa por gap conhecido) |

### 5.4 Caso multi-domínio
| # | Pergunta | Comportamento esperado | Nível de confiança esperado |
|---|---|---|---|
| VC-09 | "Prazo de devolução + carga perigosa + frete especial" | Recupera e cita POL-001-A, POL-001-B, PROC-042v2-A/B, atribuindo claramente cada parte à sua fonte — não funde num resumo sem atribuição. | Média (múltiplas fontes formais, sem contradição entre si) |

### 5.5 Casos de fronteira do domínio (comportamento diante de pergunta fora do escopo — domain-model.md §4)
> Cobre os 3 comportamentos definidos no domain-model.md que ainda não tinham critério de verificação correspondente.

| # | Situação | Comportamento esperado |
|---|---|---|
| VC-12 | "Qual a previsão do tempo para amanhã em Manaus?" (pergunta sem relação com o domínio do assistente) | Assistente declina e informa o escopo real (SLAs, frete especial, devolução) — não tenta ser útil fora do escopo (caso "fora do domínio", domain-model.md §4). |
| VC-13 | "Qual o multiplicador de frete especial para uma carga que **sai** de Manaus (não que chega)?" — dentro do contexto Frete Especial, mas a base só documenta multiplicador por região de **destino**, nunca por região de origem. | Assistente informa que não encontrou informação sobre multiplicador por região de origem na base — não extrapola o multiplicador de destino nem inventa uma regra simétrica. Mesma regra de "nunca inventar" (caso "sem chunk relevante", domain-model.md §4). |
| VC-14 | "Quantos dias tem um ano?" (pergunta trivial, de conhecimento geral, sem qualquer relação com logística) | Assistente declina da mesma forma que VC-12 — reforça que a restrição à base documental (§3) vale mesmo para perguntas triviais e aparentemente inofensivas, não só para perguntas de negócio complexas. |

### 5.6 Critério de latência
| # | Critério | Medição |
|---|---|---|
| VC-10 | Latência técnica do endpoint | Critério de aceite: **p95 < 30 segundos**, medido em pelo menos 20 execuções em ambiente de teste, sem carga concorrente simulada nesta fase. Tempo contado entre requisição recebida e resposta retornada pela API — não inclui tempo de leitura do atendente. |

> **Nota:** volume de chamados e concorrência simultânea não foram definidos nos inputs desta sessão — o critério acima assume execução sequencial de teste, não sob carga real. Se isso for necessário, precisa ser levantado como constraint adicional antes da implementação.

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 0.1 | 2026-08-11 | Product Specialist (Claude) + Arlindo | Rascunho inicial (Passo 2), pendente de checkpoint Tech Lead |
| 0.2 | 2026-08-11 | Tech Lead (Claude) + Arlindo | Checkpoint Tech Lead: corrigida menção obsoleta ao papel da vigência em §2/§3; adicionado requisito de garantia de recuperação de todas as versões conhecidas para temas com contradição (§2, §3, VC-11); VC-03/VC-04 deixaram de ser condicionais. Correções propagadas para `domain-model.md` (v0.3). **Aprovado para avançar ao Passo 3 (mockup).** |
| 0.3 | 2026-08-11 | QA lens (Claude) + Arlindo | Revisão de cobertura dos verification criteria: adicionados VC-12, VC-13, VC-14 (fronteiras do domínio — antes sem teste correspondente); adicionada coluna de nível de confiança em 5.3 (VC-05 a VC-08); VC-10 (latência) ganhou metodologia de medição (p95, 20 execuções) e nota sobre volume/concorrência não definidos; sinalizado risco técnico de viabilidade do VC-11 para validação futura de um Dev Sênior. |
| 0.4 | 2026-08-11 | Product Specialist (Claude) + Arlindo | Avaliação formal contra 5 critérios. Achados corrigidos: Outcome 1 não menciona mais "latência técnica" (métrica movida só para constraints/VC-10); Outcome 4 reescrito de "feature" para resultado do usuário; VC-13 ganhou pergunta de exemplo concreta e testável (multiplicador por região de origem, não documentado). |
| 0.5 | 2026-08-11 | Product Specialist (Claude) + Arlindo | Conferência final de consistência entre documentos: constraint de latência (§3) só dizia "< 30 segundos", sem a metodologia p95 já definida no VC-10 — alinhado para referenciar a mesma regra, evitando duas formulações diferentes do mesmo critério. |
