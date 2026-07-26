# Resumo — PROC-042 Procedimento de Cálculo de Frete Especial (v1)

## 1. Metadados / índice base

- **ID do resumo:** RES-002
- **Arquivo de origem:** PROC-042-frete-especial-v1.md
- **Tipo de documento:** Procedimento
- **Categoria/tema:** Cálculo de frete especial para cargas pesadas
- **Palavras-chave:** frete especial, peso acima de 500kg, multiplicador regional, fator de peso, prazo de entrega, aprovação gerencial, desconto de volume, aditivo contratual
- **Processos relacionados:** Cálculo de frete especial, aprovação de cargas acima de 5.000kg, frete de cargas perigosas (PROC-043), negociação comercial de descontos de volume
- **Versão do documento de origem:** 1.0 (emitido em 03/03/2023) — **status: sem indicação formal de vigência ou obsolescência; documento afirma coexistir com PROC-042-v2**
- **Data desta análise:** 26/07/2026

## 2. Resumo executivo

O PROC-042 (v1) define a fórmula de cálculo de frete especial para cargas acima de 500kg: valor base (tabela mensal) × multiplicador regional × fator de peso, com tabela de multiplicadores por região (Sul, Sudeste, Centro-Oeste, Nordeste, Norte) e fator de peso escalonado em três faixas. O prazo de entrega é o prazo padrão da rota acrescido de 2 dias úteis. Prevê condições especiais: aprovação prévia para cargas acima de 5.000kg, encaminhamento a tabela específica para cargas perigosas pesadas (PROC-043) e negociação comercial formal para descontos de volume. **Ponto crítico:** o próprio documento declara não ter indicação formal de vigência e coexistir com a versão PROC-042-v2, o que é o achado mais relevante desta análise.

## 3. Processos e regras de negócio identificados

- **Fórmula de cálculo:** Valor do frete = Valor base × Multiplicador regional × Fator de peso.
- **Valor base:** definido pela tabela mensal de fretes (fonte externa a este documento).
- **Multiplicadores regionais:** Sul 1,2 | Sudeste 1,0 | Centro-Oeste 1,3 | Nordeste 1,4 | Norte 1,6.
- **Fator de peso:** 1,0 (500kg–1.000kg) | 1,2 (1.001kg–3.000kg) | 1,5 (acima de 3.000kg).
- **Prazo de entrega:** prazo padrão da rota + 2 dias úteis adicionais para manuseio de carga pesada.
- **Aprovação obrigatória:** cargas acima de 5.000kg requerem aprovação prévia do gerente de operações regional.
- **Cargas perigosas pesadas:** seguem tabela específica do PROC-043 (Frete de Cargas Perigosas), fora do escopo deste procedimento.
- **Desconto de volume:** clientes com mais de 10 fretes especiais/mês negociam desconto com o Comercial, formalizado em aditivo contratual.

## 4. Hipóteses levantadas

- Assume-se que a "tabela mensal de fretes" (valor base) está sempre atualizada e acessível a quem realiza o cálculo no momento da cotação.
- Assume-se que a pesagem da carga é feita de forma padronizada e confiável, evitando disputas sobre em qual faixa de fator de peso a carga se enquadra.
- Assume-se que o gerente de operações regional está disponível para aprovar cargas acima de 5.000kg dentro de um prazo compatível com a expectativa de entrega do cliente (prazo não definido no documento).
- Assume-se que o cliente é informado previamente sobre o acréscimo de 2 dias úteis no prazo antes da contratação do frete especial.
- Assume-se que o processo de aditivo contratual para desconto de volume é executado em tempo hábil para não gerar cobrança incorreta nos fretes seguintes ao 10º do mês.

## 5. Riscos identificados

- **Risco crítico de governança documental:** duas versões do mesmo procedimento (v1 e v2) coexistindo sem indicação de qual prevalece — risco direto de aplicação de fórmula, multiplicador ou condição desatualizada/incorreta no cálculo do frete.
- **Risco de compliance/auditoria:** ausência de controle formal de vigência entre versões pode ser apontada em auditoria como falha de governança documental.
- **Risco comercial/financeiro:** cobrança inconsistente ao cliente caso operadores diferentes usem versões diferentes do procedimento para o mesmo tipo de carga.
- **Risco operacional:** dependência de aprovação manual do gerente de operações regional para cargas acima de 5.000kg pode gerar gargalo sem SLA definido.
- **Risco de ambiguidade textual:** o objetivo do documento define escopo como cargas "acima de 500kg", mas a tabela do fator de peso inicia a primeira faixa exatamente em "500kg", criando incerteza sobre se 500kg exatos estão dentro ou fora do escopo do frete especial.
- **Risco comercial:** negociação de desconto de volume descrita de forma genérica ("negociados pelo Comercial"), sem critérios objetivos, pode gerar tratamento desigual entre clientes.

## 6. Lacunas e perguntas em aberto

- Não há definição de qual versão (v1 ou v2) deve ser usada operacionalmente — o próprio documento afirma a coexistência sem resolver o conflito.
- Não é definido o SLA de resposta do gerente de operações regional para aprovação de cargas acima de 5.000kg.
- Não é especificado o que ocorre se a tabela mensal de fretes (valor base) não estiver disponível ou atualizada no momento do cálculo.
- Não há critérios objetivos (percentual, tabela) para o desconto de volume negociado pelo Comercial — apenas o gatilho de "mais de 10 fretes/mês".
- Não é detalhado o processo ou prazo de formalização do aditivo contratual mencionado na seção 4.
- Ambiguidade sobre o limite exato de 500kg (inclusivo ou exclusivo) entre o objetivo do documento e a tabela de fator de peso.

## 7. Termos e definições específicas do domínio

- **Frete especial:** modalidade de frete aplicável a cargas com peso acima de 500kg, com cálculo e prazo diferenciados.
- **Multiplicador regional:** fator aplicado ao valor base do frete conforme a região de destino.
- **Fator de peso:** multiplicador aplicado conforme a faixa de peso da carga.
- **Aditivo contratual:** instrumento formal usado para registrar descontos de volume negociados com clientes recorrentes.

## 8. Referências cruzadas e dependências

- **PROC-042-v2-frete-especial-revisado:** citado explicitamente no cabeçalho como versão coexistente, sem indicação de qual é a vigente. Este é o achado de maior prioridade para a próxima análise — recomenda-se comparação direta linha a linha entre v1 e v2 assim que o v2 for processado.
- **PROC-043 — Frete de Cargas Perigosas:** referenciado na seção 4 para cargas perigosas com peso acima de 500kg; não faz parte do conjunto de 5 arquivos em análise até o momento — sinalizar como possível dependência externa.
- **Possível relação com POL-001 (Política de Devolução):** a seção 3.5 do POL-001 menciona que o custo do frete reverso em caso de desistência do cliente é "calculado com os mesmos multiplicadores do frete original" — a confirmar se esses multiplicadores são os definidos neste PROC-042 quando ambos os documentos forem cruzados.

## 9. Recomendações para a fase de Discovery

- Priorizar, na próxima etapa, a análise comparativa entre PROC-042-v1 e PROC-042-v2 para identificar divergências em fórmula, multiplicadores, prazos e condições especiais.
- Levantar junto à Diretoria Comercial/Operações qual das duas versões está oficialmente em vigor, e desde quando.
- Buscar o PROC-043 para mapear a fronteira entre frete especial padrão e frete de cargas perigosas.
- Esclarecer com stakeholders o limite exato de 500kg (inclusivo ou exclusivo) para elegibilidade ao frete especial.
- Confirmar SLA de aprovação para cargas acima de 5.000kg e critérios objetivos para desconto de volume negociado pelo Comercial.
- Ao processar POL-001 novamente em conjunto com este documento, validar se os "multiplicadores do frete original" citados na política de devolução correspondem exatamente aos multiplicadores regionais aqui definidos.