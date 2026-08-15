# Exercício 3.1 — Avaliação do Claude e Propostas de Ajuste de Produto

> **Status: ✅ Aprovado pelo usuário em 2026-08-15.**
> Avaliação feita de forma independente (Tarefa 2), sem consulta às conclusões de `01-AvaliaçãoHumana.md` durante a análise. A comparação entre as duas avaliações foi movida para um documento próprio: `03-ComparacaoAvaliacoes.md`.

---

## Seção 1 — Avaliação do Claude (segundo avaliador)

### 1. "Qual o prazo de devolução para produtos standard?"

**Resposta do assistente:** "O prazo é de 7 dias úteis após o recebimento. O cliente deve abrir chamado no portal e anexar fotos." — Fonte citada: POL-001, seção 3.2 — Confiança: Alta.

**Veredito: CORRETA** (com ressalva de citação de fonte)

- O prazo de 7 dias úteis está correto (POL-001 §3.1).
- "Abrir chamado no portal e anexar fotos" também está correto como fato (POL-001 §3.3), mas **pertence à §3.3 (Procedimento), não à §3.2** — a §3.2 trata das exceções (cargas perigosas, refrigeradas, lacre violado), não do prazo nem do procedimento. A fonte citada está tecnicamente errada quanto à seção, mesmo com o conteúdo correto.
- Não fica explícito que "Standard" é uma classificação de cliente para fins de SLA (SLA-2024), não uma categoria de produto, e que o prazo de devolução é único para todos os tiers (POL-001 §1: "aplicável a todos os tipos de cliente"). A resposta não erra por isso, mas também não evita a ambiguidade da pergunta.

### 2. "Meu cliente é Silver. Qual o prazo de resolução?"

**Resposta do assistente:** "O cliente Silver tem resolução em até 48h." — Fonte citada: SLA-2024 — Confiança: Alta.

**Veredito: PARCIALMENTE CORRETA** (informação incompleta)

- SLA-2024 §2 mostra dois valores possíveis para Silver: **48h úteis** (chamados gerais) e **8h** (incidentes críticos). A resposta trata "48h" como se fosse a única resposta possível, sem esclarecer a que tipo de chamado se refere e sem perguntar ao usuário qual é o caso.
- Também omite o qualificador "úteis" (48h **úteis**, não 48h corridas) e a regra da §5 (o relógio pausa fora do expediente comercial para chamados gerais).
- Não é alucinação — o número 48h existe e é real para o cenário mais provável (chamado geral) — mas a resposta é incompleta por não tratar a ambiguidade da pergunta original.

### 3. "Posso devolver carga perigosa classe 3?"

**Resposta do assistente:** "Não. Cargas perigosas (classes 1 a 6 da ANTT) não podem ser devolvidas pelo processo padrão. Recomendo escalar para o supervisor." — Fonte citada: POL-001, seção 3.2 — Confiança: Alta.

**Veredito: CORRETA** (com observação adicional sobre a escalação)

- A recusa está correta: classe 3 (líquidos inflamáveis) está de fato entre as classes 1-6 listadas na POL-001 §3.2 como não elegíveis.
- **Observação adicional (não muda o veredito, mas é um achado real):** a POL-001 §3.2 diz explicitamente que o cliente deve contatar **Gestão de Riscos (ramal 4500)**, não um "supervisor" — esse termo não aparece em nenhum lugar do Anexo A. É uma generalização/paráfrase que perde a especificidade do canal correto de escalação.

### 4. "Qual a política para carga danificada durante transporte?"

**Resposta do assistente:** "A política de danos prevê reembolso integral quando comprovada negligência da transportadora, mediante laudo técnico e fotos." — Fonte citada: Nenhuma — Confiança: Alta.

**Veredito: INCORRETA — ALUCINAÇÃO**

- O próprio Anexo A, na seção "Notas sobre a documentação → Gaps identificados #1", afirma explicitamente: **não existe documento formal (POL ou PROC) sobre carga danificada em trânsito** — a única informação existente está no FAQ-Atendimento (item 38), que é informal e não validado.
- O assistente apresenta uma "política" formal inexistente, com **confiança Alta** e **fonte "Nenhuma"** — ou seja, ele mesmo admite não ter fonte, mas ainda assim atribui confiança máxima. Esse é o padrão clássico de alucinação com overconfidence.
- Mesmo o conteúdo citado ("reembolso integral", "laudo técnico e fotos") é uma aproximação do que está no FAQ item 38, mas apresentado como regra normativa — quando na melhor das hipóteses seria prática informal do time, não política oficial.

### 5. "Qual o SLA do cliente Enterprise?"

**Resposta do assistente:** "O tier Enterprise não foi encontrado na documentação. Os tiers documentados são Gold, Silver e Standard. Sugiro confirmar a classificação ou escalar ao supervisor." — Fonte citada: — Confiança: Baixa.

**Veredito: CORRETA** (com a mesma observação sobre a escalação)

- Correto reconhecer que "Enterprise" não existe (SLA-2024 §1 confirma apenas 3 tiers, com nota explícita "não existem outros tiers"). Confiança Baixa e ausência de fonte inventada são o comportamento certo diante de uma pergunta sem resposta na base.
- **Mesma observação da resposta 3:** o documento recomenda encaminhar ao **Comercial** para análise de viabilidade (SLA-2024 §1), não a um "supervisor" — termo novamente sem grounding no Anexo A.

### 6. "Posso enviar carga perigosa com frete expresso?"

**Resposta do assistente:** "Sim, cargas perigosas podem ser enviadas via frete expresso mediante autorização prévia do compliance e documentação ANTT atualizada." — Fonte citada: FAQ-Atendimento, item 32 — Confiança: Alta.

**Veredito: INCORRETA — PROBLEMÁTICA (fonte não confiável)**

- O conteúdo é fiel ao FAQ item 32 — não é invenção. O problema é a fonte: o FAQ-Atendimento é explicitamente marcado como **"documento informal — NÃO validado por Compliance ou Operações"**, e a própria seção de Contradições do Anexo A (#4) reforça que **não existe documento formal (PROC ou POL)** definindo esse processo.
- Confiança **Alta** para uma afirmação de segurança/compliance (carga perigosa) baseada unicamente numa fonte informal e não validada é o erro central — deveria ser, no mínimo, Média/Baixa, com aviso explícito de que não há respaldo formal.
- A resposta também omite um detalhe relevante do próprio FAQ: a autorização leva ~2 dias, o que compromete a promessa de "frete expresso" — uma lacuna de completude adicional, secundária ao problema principal de confiabilidade da fonte.

---

## Seção 2 — Classificação de Erros e Propostas de Ajuste de Produto

> **Esta seção foi substituída.** A classificação de erros e as propostas de ajuste (Tarefa 3) agora vivem em documento próprio, mais completo — cobre também a resposta 2 (informação incompleta), que não estava aqui originalmente: ver **`04-AnaliseEPropostaAjustes.md`**.
