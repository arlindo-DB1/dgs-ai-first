# Exercício 3.1 — Avaliação Humana (Tarefa 1)

**Revisão Humana, utilizando a resposta do agente e a resposta gerada com a mesma pergunta via I.A. pesquisando o Anexo A**

1.Qual o prazo de devolução para produtos standard?
De acordo com a **POL-001 — Política de Devolução de Mercadorias** (v3.1), o prazo geral de devolução é o mesmo para todos os tipos de cliente — não há diferenciação por tier (Gold/Silver/Standard) nessa política:
O cliente pode solicitar devolução em até **7 dias úteis** após a data de recebimento confirmada no sistema de tracking (contagem exclui sábados, domingos e feriados nacionais).
Algumas exceções não são elegíveis para devolução pelo processo padrão, independente do prazo: cargas perigosas (classes 1 a 6 ANTT), cargas refrigeradas com rompimento da cadeia de frio, e cargas com lacre de segurança violado (sem documentação da violação na entrega). Para essas, o cliente deve acionar Gestão de Riscos (ramal 4500).
Vale notar: a tabela de SLA por tier (Gold/Silver/Standard) trata de tempos de resposta e resolução de chamados, não do prazo de devolução em si — então não existe um "prazo de devolução específico para Standard" diferente do prazo geral de 7 dias úteis.

| # | Pergunta | Resposta | Fonte Citada | Confiança |
|---|----------|---------|--------------|-----------|
| 1 | "Qual o prazo de devolução para produtos standard?" | "O prazo é de 7 dias úteis após o recebimento. O cliente deve abrir chamado no portal e anexar fotos." | POL-001, seção 3.2 | Alta |

Análise: correta. Conforme a resposta com pesquisa diretamente no Anexo A. Ponto de atenção é verificar o procedimento informado se faz realmente parte do procedimento.

---

2.Meu cliente é Silver. Qual o prazo de resolução?
Para cliente **Silver**, segundo a tabela de SLA-2024:
Tempo de resolução para chamados gerais: até **48h úteis**.
Tempo de resolução para incidentes críticos: até **8h**.
Um detalhe da seção 5 do mesmo documento: o relógio de SLA pausa fora do horário comercial (08h–18h, dias úteis) para chamados gerais — mas essa pausa **não** se aplica a incidentes críticos de clientes Gold (para Silver, não há essa ressalva explícita, então o padrão de pausa fora do expediente vale normalmente).
Se for um incidente crítico, vale checar a definição da seção 3 (valor declarado acima de R$ 100.000 com status desconhecido por mais de 6h, carga perigosa com irregularidade, mais de 5 chamados em 24h sobre o mesmo problema, ou risco à segurança de pessoas) para confirmar qual dos dois prazos se aplica.

| # | Pergunta | Resposta | Fonte Citada | Confiança |
|---|----------|---------|--------------|-----------|
| 2 | "Meu cliente é Silver. Qual o prazo de resolução?" | "O cliente Silver tem resolução em até 48h." | SLA-2024 | Alta |

Análise: parcialmente correta. Pois conforme a resposta com pesquisa diretamente no Anexo A, existem distinções e casos específicos. Ponto de atenção referente com se conta as 48 horas.

---

3.Posso devolver carga perigosa classe 3?
Não, pelo processo padrão não. A **POL-001** (seção 3.2) exclui explicitamente cargas perigosas classificadas nas classes 1 a 6 da ANTT — e a classe 3 (líquidos inflamáveis) está nessa lista, junto com explosivos (classe 1), gases (classe 2), sólidos inflamáveis (classe 4), oxidantes e peróxidos (classe 5) e substâncias tóxicas e infectantes (classe 6).
Para esse caso, a orientação oficial é encaminhar o cliente ao setor de **Gestão de Riscos** (ramal 4500) para tratamento individual — não é elegível para devolução pelo fluxo padrão do Portal do Cliente.
Vale um adendo do FAQ interno (item 3, documento não validado por Compliance): na prática, o time relata que já houve casos em que Gestão de Riscos autorizou exceção. Por isso a orientação recomendada ao cliente é dizer que a carga "precisa de tratamento especial", em vez de afirmar que é impossível — mas a resposta oficial e normativa continua sendo a da POL-001: não elegível pelo processo padrão.

| # | Pergunta | Resposta | Fonte Citada | Confiança |
|---|----------|---------|--------------|-----------|
| 3 | "Posso devolver carga perigosa classe 3?" | "Não. Cargas perigosas (classes 1 a 6 da ANTT) não podem ser devolvidas pelo processo padrão. Recomendo escalar para o supervisor." | POL-001, seção 3.2 | Alta |

Análise: correta. Conforme a resposta com pesquisa diretamente no Anexo A

---

4.Qual a política para carga danificada durante transporte?
Não existe um documento formal (POL ou PROC) sobre isso na documentação da NovaTech — esse é um gap identificado explicitamente nas notas da documentação. A única informação disponível vem do **FAQ-Atendimento** (item 38), que é um documento informal, não controlado e explicitamente marcado como "NÃO validado por Compliance ou Operações":
Segundo o FAQ, carga danificada em trânsito segue um processo diferente da devolução padrão: o cliente precisa registrar a ocorrência em até 48h após o recebimento, com fotos e, se possível, laudo. A NovaTech investiga e, se comprovada responsabilidade da empresa, reembolsa integralmente. O caso não passa pelo atendimento normal — deve ser encaminhado para o e-mail sinistros@novatech.com.br (ou seja, tratado pelo Jurídico).
Importante deixar claro ao cliente (ou registrar internamente) que essa é a prática relatada pelo time, não uma política oficial documentada — se precisar de algo formalmente respaldado, vale escalar para Operações/Jurídico confirmarem se existe um procedimento não capturado nesta amostra de documentos.

| # | Pergunta | Resposta | Fonte Citada | Confiança |
|---|----------|---------|--------------|-----------|
| 4 | "Qual a política para carga danificada durante transporte?" | "A política de danos prevê reembolso integral quando comprovada negligência da transportadora, mediante laudo técnico e fotos." | Nenhuma | Alta |

Análise: incorreta. Pois conforme a resposta com pesquisa diretamente no Anexo A, não existe politica formal para este caso. E ponto de atenção é que o agente além de colocar como confiança alta, ele não citou nenhuma fonte e ainda informou um procedimento.

---

5.Qual o SLA do cliente Enterprise?
Não existe tier **Enterprise** na classificação de clientes da NovaTech. Segundo a SLA-2024 (seção 1), a empresa trabalha com apenas 3 tiers: **Gold**, **Silver** e **Standard**, definidos por volume mensal de operações e valor de contrato. O próprio documento reforça: "Não existem outros tiers além dos três listados acima."
Isso é parecido com o caso do FAQ (item 15), que trata de um cliente dizendo ser "Platinum" — também inexistente. A orientação nesse tipo de situação é: o cliente provavelmente está confundindo com outra transportadora, com nomenclatura de outro contrato, ou pode estar se referindo informalmente ao tier Gold (que tem os benefícios mais altos).
Recomendo pedir o número do contrato para verificar o tier real do cliente na base, e se ele insistir em condições fora dos três tiers padrão, encaminhar ao Comercial para análise de viabilidade — conforme indicado na nota da seção 1.

| # | Pergunta | Resposta | Fonte Citada | Confiança |
|---|----------|---------|--------------|-----------|
| 5 | "Qual o SLA do cliente Enterprise?" | "O tier Enterprise não foi encontrado na documentação. Os tiers documentados são Gold, Silver e Standard. Sugiro confirmar a classificação ou escalar ao supervisor." | — | Baixa |

Análise: correta. Conforme a resposta com pesquisa diretamente no Anexo A

---

6.Posso enviar carga perigosa com frete expresso?
Não há um documento formal (POL ou PROC) que defina esse processo — isso está listado explicitamente como uma contradição/gap na documentação: a informação existe apenas no FAQ informal (item 32), que não foi validado por Compliance ou Operações.
Segundo o FAQ, a resposta prática é: sim, é possível, mas exige autorização do Compliance e a documentação ANTT precisa estar atualizada. Na prática, o time relata que essa autorização costuma levar cerca de 2 dias — o que, segundo a própria observação do FAQ, faz com que o "expresso" acabe não sendo tão expresso assim. A recomendação é avisar o cliente sobre esse prazo real antes de prometer um frete expresso genuíno.
Como não existe respaldo formal para esse processo, vale cautela ao comunicar isso ao cliente como política oficial — o ideal seria confirmar com Compliance se esse fluxo de autorização está de fato formalizado ou é apenas uma prática informal do time.

| # | Pergunta | Resposta | Fonte Citada | Confiança |
|---|----------|---------|--------------|-----------|
| 6 | "Posso enviar carga perigosa com frete expresso?" | "Sim, cargas perigosas podem ser enviadas via frete expresso mediante autorização prévia do compliance e documentação ANTT atualizada." | FAQ-Atendimento, item 32 | Alta |

Análise: incorreta. Pois conforme a resposta com pesquisa diretamente no Anexo A, não existe formalização para este procedimento. E o ponto de atenção é o agente citar um item, que existe, mas é informal e ainda com uma confiança alta.

---