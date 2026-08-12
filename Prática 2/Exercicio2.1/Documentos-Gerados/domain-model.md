# Modelo de Domínio — NovaTech Assistant

> **Status:** ✅ Aprovado (v0.4) — Product Specialist e Tech Lead, revisado contra critérios formais de avaliação.
> **Fontes usadas:** inputs desta sessão — Cenário-Âncora 2, Anexo A (Documentação Simulada da NovaTech), Anexo B (Chunks de Referência RAG), spec de requisitos de RAG resumida, dados de discovery. Não foram usados os documentos completos da Prática 1 (por decisão do usuário) — nenhum cruzamento foi julgado necessário até o momento.
> **Módulo de referência:** `query-endpoint` — este documento é a base para o `requirements.md` desse módulo.

---

## 1. Por que este documento existe

Antes de especificar o que o `query-endpoint` deve fazer, precisamos de um vocabulário e de fronteiras claras — para o time humano e para os agentes de IA que vão ajudar a construir e operar o assistente. Um agente sem recorte de domínio claro tende a gerar respostas e código genéricos; este documento existe para evitar isso.

Termos de processo/metodologia (o que é "bounded context", "linguagem ubíqua" etc.) estão explicados em `dicionario-de-termos.md`. Aqui tratamos apenas do vocabulário de **negócio**.

---

## 2. Mapa de Bounded Contexts

O domínio do assistente NovaTech se divide em **5 bounded contexts**: um contexto de orquestração (o comportamento do próprio assistente) e quatro contextos de conhecimento de negócio, que existem independentemente do assistente e são as fontes que ele consulta.

- **Consulta e Resposta do Assistente** é o contexto central de orquestração: recebe a pergunta, decide o que buscar, e consulta os três contextos de conhecimento de negócio (Devolução, Frete Especial, SLA e Clientes) como fontes de resposta.
- **Devolução de Mercadorias**, **Frete Especial** e **SLA e Clientes** são contextos de conhecimento independentes entre si — cada um tem suas próprias regras de negócio — mas podem ser consultados juntos numa mesma resposta quando a pergunta é multi-domínio (seção 4).
- **Governança Documental** é transversal aos três contextos de conhecimento: não responde pergunta de negócio sozinha, mas define a regra que a Consulta e Resposta do Assistente deve seguir sempre que uma fonte de Devolução, Frete Especial ou SLA tiver mais de uma versão, vigência incerta, ou conteúdo contraditório.

### 2.1 Consulta e Resposta do Assistente *(bounded context de orquestração)*

| | |
|---|---|
| **O que está dentro** | Receber a pergunta do atendente; decidir quais fontes buscar; montar a resposta citando a fonte; sinalizar confiança; detectar quando uma pergunta cruza mais de um contexto de conhecimento (multi-domínio); aplicar as regras de comportamento diante de contradição, ausência de fonte, ou baixa confiança. |
| **O que está fora** | O conteúdo de negócio em si (regras de frete, devolução, SLA) — isso pertence aos contextos de conhecimento. A geração/edição desse conteúdo (isso é Governança Documental). |
| **Relação com outros** | Consome os 3 contextos de conhecimento como fontes; obedece às regras de vigência/contradição definidas pela Governança Documental. |
| **Por quê é um contexto próprio** | Tem vocabulário e regras próprias (confiança, citação, fallback) que não mudam dependendo do assunto perguntado — o comportamento do assistente diante de uma contradição é o mesmo, seja em Frete ou em SLA. |

### 2.2 Devolução de Mercadorias

| | |
|---|---|
| **O que está dentro** | Regras de elegibilidade, prazos, exceções por tipo de carga, procedimento de abertura de chamado, custos do frete reverso. Fonte: POL-001. |
| **O que está fora** | Carga ainda em trânsito (PROC-088, não incluído no material desta fase). Carga danificada em trânsito (mencionada apenas no FAQ informal — sem documento normativo; ver seção 4, gaps). Interceptação de carga. |
| **Relação com outros** | Compartilha o conceito de "carga perigosa" com Frete Especial (mesma classificação ANTT). Pode ser citada em resposta conjunta com SLA (ex: prazo de resposta a um chamado de devolução). |

### 2.3 Frete Especial

| | |
|---|---|
| **O que está dentro** | Cálculo de frete para cargas acima de 500kg: fórmula, multiplicadores regionais, fator de peso, prazo adicional de entrega, descontos de volume. Fontes: PROC-042 v1 e v2 — **ambas coexistem sem vigência formal definida** (ver Governança Documental). |
| **O que está fora** | Frete padrão (abaixo de 500kg) — não há documento na base para essa faixa (gap, ver seção 4). Frete de cargas perigosas (PROC-043, não incluído no material desta fase). |
| **Relação com outros** | Compartilha "carga perigosa" com Devolução. É o contexto com maior risco de erro do assistente, por ter duas versões de fonte válidas simultaneamente (ver Governança Documental e Anexo B, "armadilhas"). |

### 2.4 SLA e Clientes

| | |
|---|---|
| **O que está dentro** | Classificação de clientes em tiers (Gold, Silver, Standard); tempos de resposta e resolução por tier; definição de incidente crítico; penalidades por descumprimento; regras de medição do relógio de SLA. Fonte: SLA-2024. |
| **O que está fora** | Qualquer tier além dos 3 definidos (ex: "Platinum" não existe — é uma confusão recorrente de cliente, documentada no FAQ). Negociação de SLA diferenciado (vai para o Comercial, fora do assistente). |
| **Relação com outros** | Cruza com Devolução e Frete Especial quando a dúvida do atendente envolve prazo de atendimento de um chamado desses tipos (caso multi-domínio). |

### 2.5 Governança Documental *(bounded context transversal — responsabilidade dividida entre módulos)*

| | |
|---|---|
| **O que está dentro** | Regras sobre vigência de documento, versionamento, e o que fazer diante de fontes contraditórias; a distinção entre **fonte formal** (POL, PROC, SLA — normativa) e **fonte informal** (FAQ — prática de campo, não validada por Compliance/Operações). |
| **O que está fora** | O processo de quem edita/aprova um documento novo (não coberto pelo material desta fase). |
| **Relação com outros** | Toda vez que o Assistente (2.1) recupera chunks de mais de uma versão de um mesmo documento (ex: PROC-042 v1 e v2), é a Governança Documental que dita a regra: mostrar ambas as versões, nunca misturar valores das duas. |
| **Divisão de responsabilidade entre módulos** | **Criação e manutenção do metadado de vigência** (marcar qual versão está ativa/obsoleta) é responsabilidade do `pipeline-ingestao` (ADR-0003) — **fora do escopo desta subfase**. **Consumo do metadado de vigência** é responsabilidade do `query-endpoint` — **dentro do escopo do `requirements.md` desta subfase** — mas apenas para uso interno (ex: ranking de recuperação); **não decide o que é exibido**: diante de qualquer contradição de fonte detectada, o assistente sempre mostra ambas as versões, independente da vigência estar definida ou não (decisão registrada no `requirements.md` §4). |

> **Dependência assumida pelo `query-endpoint`:** o `requirements.md` deste módulo assume que o metadado de vigência já existe nos documentos indexados (produzido pelo `pipeline-ingestao`, conforme ADR-0003). O `query-endpoint` **consome** esse metadado — ele não é responsável por criá-lo ou mantê-lo. Isso evita que o `query-endpoint` duplique lógica que já pertence ao pipeline.

---

## 3. Linguagem Ubíqua

Vocabulário que deve ser usado de forma consistente por qualquer pessoa ou agente que trabalhe neste projeto. Onde há ambiguidade conhecida na documentação-fonte, isso é sinalizado explicitamente — o assistente deve seguir a definição canônica, não a informal.

| Termo | Definição canônica | Bounded Context | Fonte |
|---|---|---|---|
| **Carga perigosa** | Carga classificada nas classes 1 a 6 da ANTT (Resolução nº 5.947/2021): explosivos, gases, líquidos inflamáveis, sólidos inflamáveis, oxidantes/peróxidos, substâncias tóxicas/infectantes. | Devolução, Frete Especial | POL-001 §3.2 |
| **Frete especial** | Frete aplicável a cargas com peso **acima de 500kg**. Calculado como Valor base × Multiplicador regional × Fator de peso. | Frete Especial | PROC-042 |
| **Tier de cliente** | Classificação do cliente em **Gold, Silver ou Standard** — não existem outros tiers (ex: "Platinum" é inexistente; confusão comum do cliente, não deve ser validada pelo assistente). **Atenção para agentes de IA:** "Gold" aqui não é o metal nem uma qualidade genérica de produto — é exclusivamente o nome de um tier de SLA. "Standard" aqui não é um adjetivo genérico ("padrão", "comum", "regular") — é o nome literal do terceiro tier, sempre um dos 3 valores fechados desta lista, nunca usado como qualificador solto. | SLA e Clientes | SLA-2024 §1 |
| **Incidente crítico** | Situação que atende a pelo menos um critério formal: carga >R$100.000 com status desconhecido >6h; carga perigosa com irregularidade documental/rastreamento; >5 chamados do mesmo cliente em 24h sobre o mesmo problema; qualquer risco à segurança de pessoas. | SLA e Clientes | SLA-2024 §3 |
| **Devolução elegível** | Solicitação de devolução feita em até 7 dias úteis da confirmação de recebimento, para carga que não seja perigosa, refrigerada com cadeia de frio rompida, ou com lacre violado sem documentação de entrega. | Devolução de Mercadorias | POL-001 §3.1-3.2 |
| **Fonte formal** | Documento normativo (POL, PROC, SLA), com responsável institucional definido, cujas regras são de uso obrigatório. | Governança Documental | Anexo A |
| **Fonte informal** | Documento de conhecimento prático (ex: FAQ-Atendimento), sem validação de Compliance/Operações — pode conter informação desatualizada ou não respaldada. **Decisão atual: o assistente não usa fonte informal como fonte de resposta** (ver seção 4) — decisão revisável, não definitiva. | Governança Documental | Anexo A |
| **Vigência** | Metadado que indica se um documento (ou uma de suas versões) está formalmente ativo. **Situação atual conhecida:** PROC-042 v1 e v2 coexistem sem vigência formal definida — nenhuma delas está marcada como obsoleta. | Governança Documental | Anexo A, ADR-0003 |
| **Contradição de fonte** | Situação em que duas fontes válidas (ou duas versões do mesmo documento) apresentam valores ou regras diferentes para o mesmo tema. Regra: o assistente **mostra ambas as versões**, nunca combina valores de fontes diferentes numa mesma resposta. | Governança Documental / Consulta e Resposta | Spec de RAG resumida desta sessão |
| **Pergunta multi-domínio** | Pergunta do atendente que cruza mais de um bounded context de conhecimento (ex: devolução + frete especial). Ocorre em ~15% dos chamados, segundo dados de discovery. | Consulta e Resposta do Assistente | Dados de discovery desta sessão |
| **Fonte citada** | Toda resposta do assistente deve indicar explicitamente de qual documento (e, se aplicável, qual versão) a informação veio. Requisito não negociável (spec de RAG resumida). | Consulta e Resposta do Assistente | Spec de RAG resumida desta sessão |
| **Base documental restrita** | O conjunto de documentos indexados (Anexo A) é a **única** fonte de verdade permitida para o assistente. Ele nunca deve responder com conhecimento geral do modelo de IA, mesmo quando plausível — apenas com o que está explicitamente escrito em uma fonte indexada. | Consulta e Resposta do Assistente | Spec de RAG resumida desta sessão |
| **Nível de confiança** | Rótulo que o assistente atribui a cada resposta — **Alta**, **Média** ou **Baixa** (padrão adotado nesta fase; escala numérica fica para decisão técnica futura, se necessário). **Alta:** resposta vem de uma única fonte formal, sem contradição, que cobre diretamente a pergunta. **Média:** resposta combina mais de uma fonte formal (pergunta multi-domínio) sem contradição entre elas. **Baixa:** há contradição de fonte identificada (ambas as versões são mostradas) — sinaliza ao atendente que existe divergência a considerar antes de repassar ao cliente. | Consulta e Resposta do Assistente | Decisão desta sessão (Tech Lead + Product Specialist) |

---

## 4. Fronteiras do Assistente — o que faz e o que não faz

### O que o assistente FAZ (dentro do escopo)
- Responde perguntas sobre **SLAs, frete especial e devolução** — os 3 contextos de conhecimento de negócio cobertos pela documentação-fonte.
- Ao encontrar fontes contraditórias (ex: PROC-042 v1 x v2), **mostra ambas as versões** em vez de escolher uma.
- **Restrição à base documental (guardrail central):** o assistente responde **exclusivamente** com base nos documentos indexados (Anexo A / base documental da NovaTech). Ele nunca complementa a resposta com conhecimento geral do modelo de IA — mesmo que a resposta pareça óbvia, plausível, ou de conhecimento comum — e nunca infere uma regra de negócio que não esteja explicitamente escrita em uma fonte indexada.
- **Nunca inventa informação** (consequência direta da restrição acima) — se não há fonte, o assistente diz que não encontrou, não gera uma resposta plausível sem lastro.
- **Sempre cita a fonte** de cada resposta, junto com o **nível de confiança** (Alta, Média ou Baixa — ver linguagem ubíqua).
- Opera com dados **atualizados em até 24h** em relação à documentação-fonte.
- **Idioma:** o assistente sempre responde em **português** — mesmo idioma da documentação-fonte e do atendente. (Explicitado por decisão de princípio: "o óbvio deve ser dito" — não presumir isso como implícito.)
- **Perguntas de acompanhamento (multi-turn):** o assistente considera o histórico da conversa (limitado a 3 turnos, ADR-0002) para resolver referências implícitas — ex: atendente pergunta "e para o Nordeste?" logo após perguntar sobre frete para o Sul. A pergunta de acompanhamento permanece no mesmo bounded context da pergunta anterior, a menos que o novo turno mude claramente de assunto (nesse caso, é tratada como pergunta nova, podendo inclusive virar caso multi-domínio).

### Comportamento diante de pergunta fora do escopo
A pergunta do atendente pode "não encontrar contexto de resposta" de três formas diferentes — o assistente deve tratá-las de forma consistente, sempre sem inventar:

| Situação | Exemplo | Comportamento esperado |
|---|---|---|
| **Gap conhecido** — tema mencionado no discovery ou no FAQ informal, mas sem documento normativo (ver tabela de gaps abaixo) | "Qual o prazo de entrega para minha carga?" | Assistente informa que não há fonte formal para esse tema específico. **O FAQ informal não é usado como fonte de resposta** (decisão desta sessão — ver nota de revisão abaixo), mesmo com aviso de baixa confiabilidade. Se souber de um canal de encaminhamento formal (ex: ramal 4500 para Gestão de Riscos), pode informá-lo. |
| **Fora do domínio do assistente** — pergunta sem relação com logística/atendimento da NovaTech | "Qual a previsão do tempo amanhã?" | Assistente declina e informa o escopo real: responde apenas sobre SLAs, frete especial e devolução. Não tenta responder por educação ou tentar ser útil fora do escopo. |
| **Dentro do domínio, mas sem chunk relevante recuperado** — tema coberto em tese pelos 3 contextos de conhecimento, mas a busca não retornou nada aplicável | Pergunta muito específica que nenhum chunk cobre | Assistente informa que não encontrou informação suficiente na base para responder com segurança — mesma regra de "nunca inventar", tratada como ausência de fonte, não como gap declarado. |

Nos três casos, a resposta explicita ao atendente **por que** não foi possível responder normalmente — isso é o que distingue "não sei" de uma falha silenciosa.

> **Nota de revisão:** a decisão de não usar o FAQ informal como fonte de resposta (nem com aviso) foi tomada de forma conservadora nesta sessão, para evitar o risco descrito no Anexo B ("armadilha" de responder pergunta crítica com fonte informal e confiança alta). Isso é **explicitamente revisável** — se no futuro se optar por reintroduzir o FAQ como fonte para gaps conhecidos, ele deve vir sempre com nível de confiança Baixa e aviso explícito de fonte não validada.

### O que o assistente NÃO faz (fora do escopo — gaps conhecidos, não é ausência acidental)
Estes pontos aparecem nos dados de discovery ou no FAQ informal, mas **não têm documento normativo de suporte** no material desta fase — não devem ser tratados como bounded context funcional, e sim como limitação explícita a ser comunicada:

| Gap | Por quê está fora | Nota |
|---|---|---|
| **Prazos de entrega / rastreamento** | Uma das 4 categorias mais frequentes de pergunta (dados de discovery), mas nenhum documento do Anexo A cobre esse tema. | Candidato a bounded context futuro, quando houver fonte documental. |
| **Carga danificada em trânsito** | Só existe no FAQ informal (item 38); sem POL/PROC formal. | O FAQ não é usado como fonte de resposta (decisão desta sessão) — assistente informa que não há fonte formal para o tema. |
| **Seguro de carga** | Só existe no FAQ informal (item 22, com percentuais). | Mesma observação acima — não citar o FAQ. |
| **Frete padrão (abaixo de 500kg)** | PROC-042 só cobre frete especial (>500kg). | O assistente deve declarar que não tem essa informação, não extrapolar a fórmula de frete especial. |
| **Escalação para Gestão de Riscos (ramal 4500)** | POL-001 menciona o contato, mas não documenta o que a área faz com a carga. | Assistente pode informar o encaminhamento, não o desfecho. |

### Nota sobre perguntas multi-domínio (15% dos casos)
Quando uma pergunta cruza dois contextos (ex: "prazo de devolução de uma carga com frete especial"), o assistente deve recuperar e citar fontes de **cada** contexto envolvido — não deve fundir os dois num resumo sem atribuição clara de qual parte vem de qual fonte. Isso é consistente com a regra de "nunca combinar regras de fontes diferentes sem sinalizar" e evita o tipo de erro descrito no Anexo B ("armadilhas").

---

## 5. Decisões de modelagem — status pós checkpoint de Tech Lead (Passo 4)

1. ✅ **Consulta e Resposta como bounded context próprio** — validado, sem objeções no checkpoint.
2. ✅ **Governança Documental como bounded context transversal separado** — validado, com refinamento: responsabilidade explicitamente dividida entre `pipeline-ingestao` (criação do metadado de vigência) e `query-endpoint` (consumo do metadado) — ver seção 2.5.
3. ✅ **Gaps tratados como fronteira explícita, não como bounded context vazio** — validado, sem objeções.
4. ✅ **FAQ informal não é usado como fonte de resposta** para os gaps conhecidos — decisão conservadora, explicitamente revisável (ver seção 4).
5. ✅ **Nível de confiança padronizado em Alta/Média/Baixa** — definição operacional adicionada à linguagem ubíqua (seção 3).
6. ✅ **Idioma (português) e comportamento multi-turn** explicitados na seção 4, por aplicação do princípio "o óbvio deve ser dito".

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 0.1 | 2026-08-11 | Product Specialist (Claude) + Arlindo | Rascunho inicial — recorte de domínio (Passo 1), pendente de checkpoint Tech Lead |
| 0.2 | 2026-08-11 | Tech Lead (Claude) + Arlindo | Ajustes de revisão do usuário: diagrama trocado por texto; comportamento fora de escopo detalhado; guardrail de restrição à base documental. Checkpoint Tech Lead: divisão de responsabilidade da Governança Documental entre módulos; FAQ removido como fonte de resposta (revisável); nível de confiança padronizado (Alta/Média/Baixa); idioma e comportamento multi-turn explicitados. **Aprovado para avançar ao Passo 2 (requirements.md).** |
| 0.3 | 2026-08-11 | Tech Lead (Claude) + Arlindo | Correções de coerência encontradas no checkpoint de Tech Lead do `requirements.md`: §2.5 atualizada para refletir que vigência não decide mais o que é exibido (sempre mostra ambas as versões, decisão do requirements.md); gaps "carga danificada" e "seguro de carga" (§4) corrigidos para não mais citar o FAQ como fonte (estavam desatualizados desde a v0.2). |
| 0.4 | 2026-08-11 | Product Specialist (Claude) + Arlindo | Avaliação formal contra 5 critérios (coerência dos bounded contexts, ambiguidade de linguagem para LLM, outcomes, scope boundaries, testabilidade). Achado corrigido: termo "Tier de cliente" (§3) ganhou disambiguação explícita para "Gold" e "Standard" — risco real de um agente de IA confundir com metal/adjetivo genérico. |
