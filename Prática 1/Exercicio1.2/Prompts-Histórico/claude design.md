# Conversa — Diagramas BPMN (Assistente IA no atendimento)

Registro dos pedidos e do que foi produzido. Design system: **Modernist**.

---

## 1. Pergunta inicial

**Usuário:** "Preciso criar um diagrama visual de um fluxo, como devo proceder?"

**Resposta:** formulário de perguntas (tipo de fluxo, formato, público, nível de detalhe, uso final, tom). O usuário optou por colar prompts estruturados descrevendo cada fluxo.

---

## 2. Fluxo principal

**Prompt do usuário — Diagrama BPMN do Fluxo Principal**

Gerar diagrama BPMN com o conteúdo exato, sem adicionar/remover/inferir etapas.

Raias: Cliente, Atendente, Assistente IA.

Elementos, em ordem:

1. Evento de início (Cliente): "Cliente traz uma dúvida durante o chamado."
2. Atividade (Atendente): "Atendente formula a pergunta ao assistente, em linguagem natural."
3. Atividade (Assistente IA): "Assistente busca a resposta exclusivamente na base documental restrita."
4. Atividade (Assistente IA): "Assistente encontra uma fonte confiável e sem ambiguidade."
5. Atividade (Assistente IA): "Assistente retorna ao atendente a resposta, o trecho-fonte citado e o indicador de confiança."
6. Atividade (Atendente): "Atendente lê a resposta e a fonte citada."
7. Atividade (Atendente): "Atendente confia na resposta lida" — momento de validação; não existe etapa formal de aprovação separada.
8. Atividade (Atendente): "Atendente usa a resposta para responder ao cliente."
9. Gateway exclusivo (Atendente): "Há uma nova dúvida do cliente?" — Sim → retorna à atividade 2; Não → Evento de fim: "Chamado concluído."

Anotações de texto:

- Ligada à atividade 3: "Guardrail: o assistente nunca completa uma resposta com conhecimento geral do modelo quando a base restrita não cobre o tema."
- Ligada à atividade 5: "Guardrail: o assistente nunca reapresenta uma resposta/fonte já sinalizada como incorreta ou expirada sem revisão prévia."

**Entregue:** `Fluxo BPMN - Assistente IA.dc.html`

Ajustes pedidos depois:

- "As descrições estão ficando fora das formas, revise a formatação" → caixas ampliadas para 250×180, raias mais altas, conectores reposicionados.
- Correção de sobreposição entre o rótulo "Sim" e a legenda do evento de fim.
- Exportação em JPG → `export/fluxo-bpmn.jpg` (5980×2962).

---

## 3. Fluxo de fallback (v1)

**Prompt do usuário — Diagrama BPMN do Fluxo de Fallback**

Raias: Cliente, Atendente, Assistente IA, Supervisor/Área Especializada, Responsável pela Base.

1. Evento de início (Atendente): "Atendente formula a pergunta ao assistente, em linguagem natural."
2. Atividade (Assistente IA): "Assistente busca a resposta na base documental restrita."
3. Gateway exclusivo (Assistente IA): "Qual gatilho de desvio ocorre?" — três caminhos que convergem em seguida:
   - Caminho A: "Assistente identifica ambiguidade entre dois documentos válidos sobre o mesmo tema."
   - Caminho B: "Assistente não encontra nenhuma fonte que cubra o tema (lacuna real)."
   - Caminho C: "Assistente responde com fonte e indicador de confiança, mas o atendente, ao ler, não confia na resposta (por suspeita de alucinação, política não vigente, ou outro motivo)."
4. Gateway exclusivo de convergência → Atividade (Assistente IA): "Assistente comunica ao atendente o motivo do desvio."
5. Gateway paralelo (AND-split): dois ramos simultâneos.

Ramo 1 — Resolução do atendimento: comunicação ao cliente, escalonamento ao Supervisor/Área especializada, resolução por julgamento humano, conclusão do atendimento, gateway "Há uma nova dúvida do cliente?" (Sim → evento de ligação para o Fluxo Principal; Não → "Chamado concluído.").

Ramo 2 — Tratamento de conteúdo (Responsável pela Base), com as três variações A/B/C e evento de fim de sub-processo: "Encaminhado para tratamento detalhado no Fluxo de Feedback."

Anotações: guardrail de decisão humana (ligado ao gateway de gatilhos) e guardrail de não reapresentação (ligado à busca da resposta).

**Entregue:** `Fluxo BPMN - Fallback.dc.html`

**Adendo do usuário:** inserir na raia Cliente, ligadas por fluxo de mensagem:

- Após "Atendente comunica ao cliente que o chamado foi escalado para análise": "Cliente é informado de que seu chamado foi escalado para análise."
- Após "Atendente usa essa orientação para concluir o atendimento ao cliente": "Cliente recebe a resposta final referente ao seu chamado."

---

## 4. Fluxo de fallback v2 e v3

**Prompt corrigido do usuário:** cada caminho (A, B, C) passa a ter **atividade própria** e **AND-split próprio**, alimentando simultaneamente:

- a convergência **X** (comum aos três) → Ramo 1 — Resolução do atendimento;
- o respectivo **Ramo 2A / 2B / 2C** na raia Responsável pela Base, unidos pela convergência **Y** → evento de fim de sub-processo "Encaminhado para tratamento detalhado no Fluxo de Feedback."

Textos dos caminhos:

- A — Ambiguidade: "Assistente identifica ambiguidade entre dois documentos válidos sobre o mesmo tema e comunica o motivo ao atendente."
- B — Lacuna real: "Assistente não encontra nenhuma fonte que cubra o tema (lacuna real) e comunica o motivo ao atendente."
- C — Discordância: "Assistente responde com fonte e indicador de confiança; o atendente, ao ler, não confia na resposta (…) e sinaliza isso; a resposta é suspensa e não é reutilizada até revisão do Responsável pela Base."

Ramo 2: 2A "Caso sinalizado para esclarecimento futuro da fronteira entre os documentos envolvidos."; 2B "Caso registrado como 'gap de cobertura documental'; solicitação formal de criação/atualização de política enviada."; 2C "Resposta suspensa permanece indisponível até revisão."

**Entregue:** `Fluxo BPMN - Fallback v2.dc.html`

**v3** acrescenta:

- **Nota de sincronismo:** "O Ramo 1 e o Ramo 2 ocorrem em paralelo, independente um do outro — o tratamento de conteúdo (Ramo 2) não espera a resolução do atendimento (Ramo 1) terminar."
- **Correção do evento de início:** novo evento de início na raia Cliente — "Cliente traz uma dúvida durante o chamado." — e "Atendente formula a pergunta ao assistente, em linguagem natural" passa a ser a primeira atividade, ligada por fluxo de sequência.

**Entregue:** `Fluxo BPMN - Fallback v3.dc.html` · JPG: `export/fluxo-fallback-v3.jpg` (8120×5562).

Correções de notação aplicadas no caminho: pontas de seta ajustadas aos vértices reais dos losangos (meia-diagonal, não meia-aresta) e legendas reposicionadas para não serem cortadas por linhas de fluxo.

---

## 5. Fluxo de feedback

**Prompt do usuário — Diagrama BPMN do Fluxo de Feedback**

Raias: Atendente, Assistente IA, Responsável pela Base, Time Técnico do Assistente. Três eventos de início, um por gatilho; cada caminho mantém identidade até concluir sua ação específica.

- **Caminho A — Ambiguidade** (início na raia Assistente IA): "Ambiguidade identificada entre dois documentos válidos sobre o mesmo tema (vindo do Fluxo de Fallback)." → Atividade (Responsável pela Base): "Define e documenta explicitamente a fronteira entre os processos envolvidos, atualizando a base do assistente com essa definição." → convergência Y.
- **Caminho B — Lacuna real** (início na raia Assistente IA): "Lacuna real de documentação identificada — nenhuma fonte cobre o tema (vindo do Fluxo de Fallback)." → Atividade (Responsável pela Base): "Elabora e publica a política ausente junto à área dona do tema (Operações, Compliance ou Comercial), e atualiza a base do assistente com o novo conteúdo." → convergência Y.
- **Caminho C — Atendente não confia na resposta** (início na raia Atendente): "Atendente sinaliza que não confia na resposta lida, ou que uma resposta já utilizada estava errada, desatualizada ou incompleta."
  1. Atividade (Assistente IA): "Assistente registra a sinalização e suspende a resposta/fonte envolvida — nenhum outro atendente recebe essa mesma resposta enquanto durar a suspensão."
  2. Atividade (Responsável pela Base): "Avalia se o problema é de conteúdo ou de comportamento do assistente."
  3. Gateway exclusivo: "O problema é de conteúdo ou de comportamento do assistente?"
     - Conteúdo → (Responsável pela Base): "Atualiza o conteúdo existente na base, respeitando a regra de versionamento único ativo."
     - Comportamento → (Time Técnico do Assistente): "Revisa o comportamento relacionado ao caso, aplica o ajuste técnico necessário e libera a correção."
  4. Gateway de convergência → Atividade (Assistente IA): "Encerra a suspensão da resposta — conteúdo/comportamento revisado volta a ficar disponível." → convergência Y.

**Convergência final Y** → Evento de fim: "Na próxima vez que o tema for perguntado, o assistente responde com o conteúdo corrigido, criado, esclarecido, ou com o comportamento ajustado."

Anotações:

- Ligada à suspensão (Caminho C): "Guardrail: o assistente nunca reapresenta uma resposta/fonte já sinalizada como incorreta ou expirada sem revisão prévia."
- Ligada às atividades de atualização de conteúdo (A, B e o ramo "Conteúdo" de C): "Guardrail: a regra de versionamento único ativo é aplicada obrigatoriamente em toda correção ou atualização de conteúdo — a versão anterior é baixada ao publicar a nova."

**Entregue:** `Fluxo BPMN - Feedback.dc.html` · JPG: `export/fluxo-feedback.jpg` (7520×5922).

---

## Arquivos do projeto

| Arquivo | Conteúdo |
| --- | --- |
| `Fluxo BPMN - Assistente IA.dc.html` | Fluxo principal |
| `Fluxo BPMN - Fallback.dc.html` | Fallback v1 (histórico) |
| `Fluxo BPMN - Fallback v2.dc.html` | Fallback com identidade A/B/C e convergências X e Y |
| `Fluxo BPMN - Fallback v3.dc.html` | v2 + nota de sincronismo + início na raia Cliente |
| `Fluxo BPMN - Feedback.dc.html` | Fluxo de feedback |
| `export/fluxo-bpmn.jpg` | Fluxo principal em JPG |
| `export/fluxo-fallback-v3.jpg` | Fallback v3 em JPG |
| `export/fluxo-feedback.jpg` | Feedback em JPG |

Todos os diagramas têm três controles de exibição: título, numeração das etapas e guardrails.
