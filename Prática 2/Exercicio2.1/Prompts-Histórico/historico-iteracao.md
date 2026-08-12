# Histórico de Iteração — Exercicio 2.1 (Recorte de Domínio + Spec do query-endpoint)

> Entregável 4 de 4 desta subfase (Cenário 2 — Fase de Estruturação, Prática 2). Consolida todas as rodadas de revisão pelas quais os outros 3 entregáveis passaram: `domain-model.md`, `requirements.md` e o mockup da interface no Teams (`mockup-prompt-claude-design.md` + `mockup.jpg`).

---

## 1. `domain-model.md` — de v0.1 a v0.4

| Rodada | Origem do feedback | O que foi encontrado | Ajuste aplicado |
|---|---|---|---|
| 1 | Usuário (revisão direta) | Diagrama ASCII do mapa de bounded contexts ficou desalinhado/ilegível. | Substituído por descrição textual das relações entre os 5 bounded contexts. |
| 1 | Usuário (revisão direta) | Faltava definir o comportamento do assistente quando a pergunta está fora do contexto de resposta dele. | Adicionada a subseção "Comportamento diante de pergunta fora do escopo", com 3 casos: gap conhecido, fora do domínio, sem chunk relevante. |
| 1 | Usuário (revisão direta) | Faltava um guardrail explícito limitando o assistente à base documental, sem inventar. | Adicionado guardrail "Restrição à base documental" e o termo correspondente na linguagem ubíqua. |
| 2 | Checkpoint Tech Lead | Governança Documental cruza `pipeline-ingestao` e `query-endpoint` sem essa divisão estar declarada. | Seção 2.5 passou a explicitar a divisão de responsabilidade por módulo, com nota de dependência assumida pelo `query-endpoint`. |
| 2 | Checkpoint Tech Lead | Uso do FAQ informal como fonte para gaps conhecidos conflitava com a "armadilha" já documentada no Anexo B. | Decisão: FAQ não é usado como fonte de resposta neste momento — marcado como revisável. |
| 2 | Checkpoint Tech Lead | "Sinalizar confiança" não tinha definição operacional. | Padronizado como Alta/Média/Baixa, com critério de atribuição, na linguagem ubíqua. |
| 2 | Checkpoint Tech Lead | Idioma de resposta e comportamento multi-turn estavam implícitos, não escritos (princípio "o óbvio deve ser dito"). | Explicitados: resposta sempre em português; perguntas de acompanhamento usam o histórico de 3 turnos (ADR-0002). |
| 3 | Checkpoint Tech Lead do `requirements.md` (consistência cruzada) | §2.5 ainda tinha a lógica antiga (vigência decidindo o que é exibido) — inconsistente com a decisão de sempre mostrar ambas as versões. | Corrigido: vigência passa a valer só para uso interno (ranking), nunca decide o que é exibido. |
| 3 | Checkpoint Tech Lead do `requirements.md` (consistência cruzada) | Gaps "carga danificada" e "seguro de carga" ainda citavam o FAQ como fonte possível — desatualizado desde a rodada 2. | Corrigido: nenhum gap cita o FAQ. |
| 4 | Revisão holística (checkpoint pré-fechamento) | Metadados de status/versão no cabeçalho ainda diziam "rascunho aguardando checkpoint". | Atualizado para "✅ Aprovado". |
| 5 | Avaliação formal contra critérios (ver seção 4) | Termo "Tier de cliente" sem disambiguação de "Gold"/"Standard" para agentes de IA. | Nota de disambiguação adicionada — versão avança para v0.4. |

**Estado final:** `domain-model.md` v0.4, aprovado.

---

## 2. `requirements.md` — de v0.1 a v0.5

| Rodada | Origem do feedback | O que foi encontrado | Ajuste aplicado |
|---|---|---|---|
| 1 | Usuário (pedido de transparência de decisões) | Usuário pediu lista completa de toda decisão autônoma antes de prosseguir — não só as "bloqueantes". | Claude passou a listar decisões por categoria (A-F) para aprovação item a item; virou princípio de trabalho registrado para o resto da fase. |
| 1 | Usuário | Tabela "fora do escopo" e formato de resposta da API: aceitos como base de trabalho, a revisar quando os outros módulos tiverem suas specs. | Notas adicionadas nas seções correspondentes. |
| 1 | Usuário | "Frescor de dados" deveria ser enquadrado como "prazo padrão da política", não como suposição. | Reescrito. |
| 1 | Usuário (decisão de reconciliação) | Conflito entre ADR-0003 ("priorizar versão mais recente") e a spec de RAG resumida ("mostrar ambas as versões"). Três opções apresentadas com trade-offs. | Usuário escolheu a Opção C: o padrão desta sessão prevalece sempre — mostra ambas as versões diante de qualquer contradição, independente da vigência. |
| 2 | Checkpoint Tech Lead | VC-03/VC-04 eram condicionais ("se a v1 também for recuperada") — não garantiam de fato a promessa da seção 4. | Novo requisito: a busca deve **garantir** a recuperação de todas as versões conhecidas para temas com contradição registrada (novo VC-11). |
| 2 | Checkpoint Tech Lead | §2/§3 ainda descreviam a vigência como decisora do comportamento de contradição. | Corrigido para refletir que vigência só orienta recuperação interna. |
| 3 | Revisão QA (a pedido do usuário) | Faltava VC para "pergunta fora do domínio" e para "sem chunk relevante" — 2 dos 3 comportamentos de fronteira do domain-model sem teste. | Adicionados VC-12 e VC-13. |
| 3 | Revisão QA | Faltava teste de vazamento de conhecimento geral com pergunta trivial/inofensiva. | Adicionado VC-14. |
| 3 | Revisão QA | VC-05 a VC-08 sem coluna de nível de confiança esperado. | Coluna adicionada. |
| 3 | Revisão QA | VC-10 (latência) sem metodologia de medição. | Definido p95 < 30s, mínimo 20 execuções, com nota sobre volume/concorrência não definidos nesta sessão. |
| 3 | Revisão QA | VC-11 pode ter custo de implementação não trivial. | Nota de risco técnico adicionada, para validação futura por um Dev Sênior. |
| 4 | Revisão holística (checkpoint pré-fechamento) | Metadados de cabeçalho referenciavam `domain-model.md` na versão errada (v0.2 em vez de v0.3) em 3 lugares; linha de aprovação não refletia que o checkpoint Tech Lead já havia ocorrido. | Corrigidos todos os pontos; linha de aprovação passou a listar explicitamente a pendência real (Dev Sênior, só para o VC-11). |
| 5 | Avaliação formal contra critérios (ver seção 4) | Outcome 1 misturava resultado com métrica técnica; Outcome 4 era uma feature disfarçada de outcome; VC-13 sem exemplo concreto; constraint de latência (§3) desalinhada da metodologia p95 já definida em VC-10. | Outcomes 1 e 4 reescritos; VC-13 ganhou exemplo concreto; constraint de latência passou a referenciar a mesma metodologia (p95) do VC-10 — versão avança para v0.4. |

| 6 | Conferência final de consistência (a pedido do usuário, pós-avaliação por critérios) | Constraint de latência (§3) ainda dizia só "< 30 segundos", sem a metodologia p95 já definida no VC-10 — duas formulações do mesmo critério em lugares diferentes do documento. Também corrigidos, neste próprio `historico-iteracao.md`: cabeçalhos de seção e "Estado final" que ainda diziam v0.3 (rodada 4 havia sido erroneamente rotulada como "final"). | Constraint de §3 passou a referenciar a metodologia do VC-10 — versão avança para v0.5. |

**Estado final:** `requirements.md` v0.5, aprovado — com 14 critérios de verificação (VC-01 a VC-14).

---

## 3. Mockup — `mockup-prompt-claude-design.md` + `mockup.jpg`

| Rodada | Origem do feedback | O que foi encontrado | Ajuste aplicado |
|---|---|---|---|
| 1 | Usuário (enunciado do Passo 3) | Pediu mockup no padrão visual do Microsoft Teams (não o design system "Modernist" da Prática 1), coerente com o requirements.md, demonstrando uma interação de pergunta e resposta. | Claude propôs o cenário de frete especial com contradição PROC-042 v1/v2 (mais ilustrativo dos guardrails do que um caminho feliz simples) — aceito. |
| 1 | Usuário | Faltava um botão para o atendente bloquear a resposta por um motivo específico. | Adicionado botão "🚫 Bloquear resposta", com anotação (callout) explicando que abre um campo de motivo — mesmo padrão de anotação usado nos diagramas BPMN da Prática 1. |
| 1 | Usuário | Pedido de reforço visual para os níveis de confiança Média/Baixa. | Proposto: borda lateral colorida no card (verde/âmbar/vermelho) + linha de alerta em destaque para o caso Baixa — aceito. |
| 2 | Product Specialist (revisão do resultado gerado) | Rodapé de fontes citava "PROC-042" sem o "v1" explícito, enquanto a tabela do card usava "PROC-042 v1" — pequena inconsistência de rótulo entre duas partes do mesmo card. | Usuário decidiu **manter como está** — aprovado sem esse ajuste; registrado para referência futura. |
| 1 (registrado em `claude-design.md`) | Verificação direta no Claude Design | Card com largura fixa (640px) cortava o callout da anotação em janelas menores (`overflow:hidden`). | Ajuste pontual de CSS: card para `max-width:640px; flex:1 1 auto`, coluna do callout para `flex:0 1 auto` — confirma o padrão já esperado (ajustes pontuais, não regeneração completa, igual à Prática 1). |

**Estado final:** `mockup.jpg` aprovado como está.

---

## 4. Avaliação formal contra critérios (rodada final, pré-fechamento)

Antes do fechamento definitivo, o usuário pediu uma auto-avaliação honesta contra 5 critérios formais, seguida de correção dos gaps encontrados e nova revisão sob a ótica das personas (Product Specialist, Tech Lead, QA).

| Critério | Veredito inicial | Gap encontrado | Correção aplicada | Veredito final |
|---|---|---|---|---|
| Bounded contexts coerentes com domínio de logística (não divisões técnicas) | ⚠️ Parcial | 2 dos 5 contextos (Consulta e Resposta, Governança Documental) são "domínio de produto de IA", não logística pura — nenhum é divisão técnica tipo frontend/backend. | Nenhuma — avaliado como trade-off de design aceitável, não defeito; registrado explicitamente aqui para transparência. | ✅ Atende (com nuance documentada) |
| Linguagem ubíqua sem ambiguidade para um LLM | ⚠️ Parcial | "Gold" e "Standard" (tiers) sem disambiguação explícita — risco real de confusão com o metal / com adjetivo genérico. | `domain-model.md` §3, termo "Tier de cliente": adicionada nota explícita de disambiguação para agentes de IA. | ✅ Atende |
| Outcomes orientados a resultado do usuário, não features técnicas | ⚠️ Parcial | Outcome 1 misturava resultado com métrica técnica ("latência técnica < 30s"); Outcome 4 estava redigido como nome de feature ("Suporte a perguntas multi-domínio"). | `requirements.md` §1: Outcome 1 reescrito focado em experiência (não travar o atendimento ao vivo), métrica só na constraint/VC-10; Outcome 4 reescrito como resultado (resposta completa sem perguntas separadas). | ✅ Atende |
| Scope boundaries derivam dos bounded contexts | ✅ Atende | — | — | ✅ Atende (sem alteração) |
| Verification criteria testáveis pelo QA | ✅ Atende (quase) | VC-13 não tinha pergunta de exemplo concreta, ao contrário dos demais VCs. | `requirements.md` §5.5: VC-13 ganhou pergunta de exemplo concreta e testável (multiplicador de frete especial por região de origem — não documentado). | ✅ Atende |

**Revisão cruzada de consistência (lente Tech Lead), pós-correção:** todas as referências a `domain-model.md` no `requirements.md` foram atualizadas de v0.3 para v0.4 (3 pontos: cabeçalho, §2, §4) — sem isso, a correção do glossário ficaria referenciada com número de versão desatualizado.

**Estado final pós-avaliação:** `domain-model.md` v0.4, `requirements.md` v0.4 — ambos aprovados.

---

## 5. Itens em aberto — a carregar para as próximas subfases da Prática 2

Nenhum destes bloqueia o fechamento do Exercicio 2.1, mas não devem ser esquecidos:

1. **Validação técnica do VC-11** (garantia de recuperação de todas as versões para temas com contradição conhecida) por um Dev Sênior real, antes de virar `plan.md`.
2. **Decisão sobre o FAQ informal é revisável** — hoje o assistente nunca usa o FAQ como fonte; se isso mudar no futuro, deve vir sempre com confiança Baixa e aviso explícito (ver `domain-model.md` §4).
3. **Comportamento do botão "Bloquear resposta"** (o que acontece após o atendente informar o motivo, quem recebe a notificação) não foi definido — pertence à spec do `feedback-api`, ainda não escrita.
4. **Anexo D — Starter Repo** citado no Anexo C, ainda não recebido — os artefatos desta subfase estão em `Documentos-Gerados/` até o repositório real existir.
5. **Estados visuais de confiança Alta e Média** não foram testados no Claude Design — só o caso Baixa foi gerado. A regra de cor já está documentada no prompt de mockup para reaproveitamento futuro.
6. **Gaps conhecidos** (prazos de entrega/rastreamento, carga danificada, seguro de carga, frete padrão <500kg, desfecho da escalação à Gestão de Riscos) permanecem fora do escopo do assistente — candidatos a bounded contexts futuros, se e quando houver fonte documental.
7. **Rótulo "PROC-042" vs "PROC-042 v1"** no rodapé de fontes do mockup — inconsistência conhecida, aceita conscientemente pelo usuário.

---

## 6. Fechamento do Exercicio 2.1

| Entregável | Arquivo | Status |
|---|---|---|
| 1. Mapa de bounded contexts com linguagem ubíqua | `domain-model.md` | ✅ Aprovado (v0.4) |
| 2. Requirements.md do módulo principal | `requirements.md` | ✅ Aprovado (v0.5) |
| 3. Mockup da interface de resposta no Teams | `mockup-prompt-claude-design.md` + `mockup.jpg` | ✅ Aprovado |
| 4. Histórico de iteração | `historico-iteracao.md` (este documento) | ✅ Concluído |

**Os 4 entregáveis do Exercicio 2.1 estão completos.**
