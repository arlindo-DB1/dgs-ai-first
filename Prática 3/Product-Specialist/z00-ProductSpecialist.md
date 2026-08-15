### PRODUCT SPECIALIST

#### Exercício 3.1 — Revisão crítica das respostas do assistente

**Tópico:** Revisão Crítica de Outputs de IA

**Contexto:** Antes do go-live, você valida uma amostra de respostas do assistente para garantir que atendem aos requisitos de produto.

**Ferramentas a utilizar:** Claude (chat)

**Inputs fornecidos:**
- O cenário completo.
- A documentação da NovaTech (ver **Anexo A**) como fonte de verdade.
- 6 pares de pergunta/resposta do assistente em staging (simulados):

| # | Pergunta | Resposta | Fonte Citada | Confiança |
|---|----------|---------|--------------|-----------|
| 1 | "Qual o prazo de devolução para produtos standard?" | "O prazo é de 7 dias úteis após o recebimento. O cliente deve abrir chamado no portal e anexar fotos." | POL-001, seção 3.2 | Alta |
| 2 | "Meu cliente é Silver. Qual o prazo de resolução?" | "O cliente Silver tem resolução em até 48h." | SLA-2024 | Alta |
| 3 | "Posso devolver carga perigosa classe 3?" | "Não. Cargas perigosas (classes 1 a 6 da ANTT) não podem ser devolvidas pelo processo padrão. Recomendo escalar para o supervisor." | POL-001, seção 3.2 | Alta |
| 4 | "Qual a política para carga danificada durante transporte?" | "A política de danos prevê reembolso integral quando comprovada negligência da transportadora, mediante laudo técnico e fotos." | Nenhuma | Alta |
| 5 | "Qual o SLA do cliente Enterprise?" | "O tier Enterprise não foi encontrado na documentação. Os tiers documentados são Gold, Silver e Standard. Sugiro confirmar a classificação ou escalar ao supervisor." | — | Baixa |
| 6 | "Posso enviar carga perigosa com frete expresso?" | "Sim, cargas perigosas podem ser enviadas via frete expresso mediante autorização prévia do compliance e documentação ANTT atualizada." | FAQ-Atendimento, item 32 | Alta |

**Tarefa:**
1. Avalie cada resposta por conta própria: correta, parcialmente correta, ou incorreta? Justifique com base no Anexo A.

2. Depois, use o **Claude** como segundo avaliador e compare com a sua avaliação.

3. Para as respostas com problema, classifique o tipo de erro (alucinação, fonte não confiável, informação incompleta) e proponha um ajuste de produto (prompt, interface, ou pipeline) para preveni-lo.

**Entregável:** Sua avaliação, a avaliação do Claude, a comparação, e as propostas de ajuste.

**Critérios de avaliação:**
- A resposta 4 é identificada como alucinação (não há documento sobre política de danos na base — o assistente inventou com confiança alta).
- A resposta 6 é identificada como problemática (fonte é o FAQ informal sem validação — informação sobre carga perigosa deveria vir de documento formal).
- As respostas 1, 2, 3 e 5 são corretamente avaliadas como adequadas.
- A comparação com o Claude é honesta sobre concordâncias e divergências.

---

#### Exercício 3.2 — Harness de produto para melhoria contínua

**Tópico:** Harness Engineering

**Contexto:** O assistente vai evoluir após o go-live. Você define, do ponto de vista de produto, como garantir que ele melhore sem degradar.

**Ferramentas a utilizar:** Claude (chat)

**Inputs fornecidos:**
- O cenário completo.
- O conceito de harness de produto: *"Define quais métricas de qualidade são monitoradas, como o feedback de usuários é processado, e como mudanças no assistente são validadas antes de ir a produção (regression testing de produto)."*
- Os guardrails formalizados no cenário 2 (DEVE / NÃO DEVE / QUANDO EM DÚVIDA), que o harness deve preservar ao longo da evolução.

**Tarefa:**
Usando o **Claude**, projete um harness de produto que cubra:
1. **Processo de feedback:** como o feedback do atendente vira melhoria (novo documento? ajuste de prompt? reindexação?).
2. **Regression testing de produto:** antes de mudar o prompt ou adicionar documentos, como verificar que as respostas existentes não pioraram E que os guardrails do cenário 2 continuam sendo respeitados.
3. **Ponto de human-in-the-loop:** quais mudanças no assistente exigem aprovação humana antes de ir a produção, e quem aprova.

**Entregável:** O documento do harness de produto.

**Critérios de avaliação:**
- O processo de feedback é completo (do atendente até a melhoria efetiva).
- O regression testing reconhece que mudanças em IA podem ter efeitos colaterais e verifica que os guardrails não regridem.
- O ponto de HITL é concreto (define o que precisa de aprovação humana e quem aprova).

---