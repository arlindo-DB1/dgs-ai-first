# Resumo — PROC-042-v2 Procedimento de Cálculo de Frete Especial (Revisado)

## 1. Metadados / índice base

- **ID do resumo:** RES-004
- **Arquivo de origem:** PROC-042-v2-frete-especial-revisado.md
- **Tipo de documento:** Procedimento
- **Categoria/tema:** Cálculo de frete especial para cargas pesadas (versão revisada)
- **Palavras-chave:** frete especial, multiplicador regional, fator de peso, prazo de entrega, desconto de volume, disposições transitórias, PROC-043, gerente de operações regional
- **Processos relacionados:** Cálculo de frete especial, aprovação de cargas acima de 5.000kg, frete de cargas perigosas (PROC-043), desconto de volume por faixa, regra de transição entre versões do procedimento
- **Versão do documento de origem:** 2.0 (emitido em 10/11/2023) — **status: sem indicação formal de que substitui a v1; documento afirma que ambas as versões coexistem no SharePoint sem hierarquia clara**
- **Data desta análise:** 26/07/2026

## 2. Resumo executivo

O PROC-042-v2 revisa a fórmula de frete especial da v1: mantém a estrutura (valor base × multiplicador regional × fator de peso), mas atualiza os multiplicadores regionais (todos maiores que na v1) e reduz o fator de peso nas faixas superiores. O prazo de entrega adicional passa de 2 para 3 dias úteis. A regra de desconto de volume deixa de ser uma negociação genérica e passa a ter percentuais objetivos por faixa (5% a partir de 8 fretes/mês, 10% acima de 15). O documento inclui uma seção de disposições transitórias que define qual versão usar conforme a data do chamado, mas — de forma contraditória — o próprio cabeçalho declara não ter indicação formal de que substitui a v1, mesmo definindo regras de transição que pressupõem ser a versão vigente a partir de 01/12/2023.

## 3. Processos e regras de negócio identificados

- **Fórmula de cálculo:** idêntica à v1 — Valor do frete = Valor base × Multiplicador regional × Fator de peso.
- **Fator de peso (revisado):** 1,0 (500kg–1.000kg) | 1,15 (1.001kg–3.000kg) | 1,4 (acima de 3.000kg) — reduzido nas duas faixas superiores em relação à v1 (1,2 e 1,5).
- **Multiplicadores regionais (atualizados em nov/2023):** Sul 1,3 | Sudeste 1,1 | Centro-Oeste 1,4 | Nordeste 1,5 | Norte 1,8 — todos superiores aos valores da v1.
- **Prazo de entrega:** prazo padrão da rota + 3 dias úteis (documento destaca explicitamente que era +2 dias na versão anterior).
- **Aprovação obrigatória:** cargas acima de 5.000kg requerem aprovação prévia do gerente de operações regional (regra idêntica à v1).
- **Cargas perigosas pesadas:** seguem tabela específica do PROC-043, com nota adicional de que este está em revisão pelo Compliance e pode sofrer alterações.
- **Desconto de volume (revisado e objetivado):** a partir de 8 fretes especiais/mês, desconto de 5% sobre o multiplicador regional; acima de 15 fretes/mês, desconto de 10%; descontos maiores exigem aprovação da Diretoria Comercial.
- **Disposições transitórias:** chamados abertos antes de 01/12/2023 e ainda em processamento usam os multiplicadores da v1; chamados novos a partir de 01/12/2023 usam os multiplicadores desta v2.

## 4. Hipóteses levantadas

- Assume-se que a regra de transição (corte em 01/12/2023) foi suficiente para migrar toda a operação para a v2, sem chamados legados pendentes usando a v1 além do período de transição.
- Assume-se que o sistema de cálculo de frete (humano ou automatizado) sabe identificar a data de abertura do chamado para aplicar a versão correta durante o período de transição.
- Assume-se que a nota sobre a revisão do PROC-043 pelo Compliance não altera, no período coberto por este documento, a forma como cargas perigosas pesadas são calculadas hoje.
- Assume-se que os novos percentuais de desconto de volume (5%/10%) substituem integralmente a negociação caso a caso prevista na v1, sem processo paralelo de exceção.

## 5. Riscos identificados

- **Risco crítico de governança documental (herdado e agravado):** o documento define regras de transição que pressupõem ser a versão vigente a partir de 01/12/2023, mas o próprio cabeçalho nega ter status formal de substituição da v1 — contradição interna que perpetua a ambiguidade já identificada no resumo da v1 (RES-002).
- **Risco de obsolescência da regra de transição:** a data-limite de transição (01/12/2023) está años no passado em relação à data desta análise (26/07/2026); não há indicação de o que rege o cálculo hoje caso ainda existam chamados antigos ou disputas sobre qual versão aplicar, já que o documento não define um comportamento "padrão" após o fim do período transitório.
- **Risco financeiro/comercial:** multiplicadores regionais mais altos combinados com fator de peso mais baixo nas faixas superiores tornam o resultado financeiro da mudança não trivial de prever sem simulação — pode haver cargas em que o frete final é mais caro ou mais barato que na v1, a depender da combinação região/peso.
- **Risco operacional:** a dependência do PROC-043 (em revisão pelo Compliance) para cargas perigosas pesadas introduz uma fonte de instabilidade externa a este procedimento.
- **Risco comercial:** a regra de desconto de volume objetiva (5%/10%) pode conflitar com aditivos contratuais já firmados sob a lógica de negociação caso a caso da v1, gerando inconsistência para clientes com contratos anteriores a novembro de 2023.

## 6. Lacunas e perguntas em aberto

- Não há indicação de qual regra vale para cálculos realizados após o encerramento do período de transição, caso surjam divergências ou chamados retroativos.
- A contradição entre o cabeçalho ("não possui indicação formal de que substitui a v1") e a seção 5 (que trata a v2 como vigente a partir de 01/12/2023) não é resolvida no próprio documento.
- Não é dito o que ocorre com clientes que já tinham aditivo contratual de desconto de volume negociado sob a lógica da v1 — se migram automaticamente para os novos percentuais ou mantêm a condição anterior.
- Não há prazo ou critério definido para quando o PROC-043 (em revisão) deve ser atualizado, nem o que fazer enquanto isso.
- Não é especificado se a aprovação da Diretoria Comercial para descontos acima de 10% tem SLA ou critérios formais.

## 7. Termos e definições específicas do domínio

- **Disposições transitórias:** regras que definem qual versão de um procedimento se aplica a casos abertos antes/depois de uma data de corte.
- **Fator de peso / multiplicador regional:** ver definições equivalentes no resumo do PROC-042-v1 (RES-002); valores numéricos revisados nesta versão.
- **Desconto de volume:** redução percentual aplicada ao multiplicador regional para clientes com alto volume mensal de fretes especiais.

## 8. Referências cruzadas e dependências

- **PROC-042-v1-frete-especial (RES-002):** este documento é a contraparte direta da v1 já analisada. Principais divergências identificadas:
  | Parâmetro | v1 | v2 |
  |---|---|---|
  | Fator de peso (1.001–3.000kg) | 1,2 | 1,15 |
  | Fator de peso (>3.000kg) | 1,5 | 1,4 |
  | Multiplicador Sul | 1,2 | 1,3 |
  | Multiplicador Sudeste | 1,0 | 1,1 |
  | Multiplicador Centro-Oeste | 1,3 | 1,4 |
  | Multiplicador Nordeste | 1,4 | 1,5 |
  | Multiplicador Norte | 1,6 | 1,8 |
  | Prazo adicional | +2 dias úteis | +3 dias úteis |
  | Desconto de volume | Negociação caso a caso (>10 fretes/mês) | 5% (≥8 fretes/mês) / 10% (>15 fretes/mês), acima disso aprovação da Diretoria Comercial |
  | Cargas >5.000kg | Aprovação do gerente de operações regional | Idêntico |
  | Cargas perigosas | Encaminha a PROC-043 | Encaminha a PROC-043, com nota de que está em revisão pelo Compliance |
  A v2 possui a seção 5 (disposições transitórias) que a v1 não tem — é o único elemento textual que estabelece alguma precedência temporal entre as versões, ainda que sem valor de "substituição formal" declarado.
- **PROC-043 — Frete de Cargas Perigosas:** referenciado novamente, agora com nota de que está em revisão pelo Compliance; ainda não faz parte do conjunto de 5 arquivos em análise.
- **Possível relação com POL-001 (seção 3.5):** os "multiplicadores do frete original" citados na política de devolução precisam ser confrontados com os valores desta v2 (mais recentes) e não apenas com os da v1, para saber qual conjunto de multiplicadores é de fato aplicado hoje.

## 9. Recomendações para a fase de Discovery

- Resolver formalmente, junto à Diretoria Comercial, qual versão do PROC-042 está vigente hoje (26/07/2026), já que a data de corte da disposição transitória (01/12/2023) está expirada e nenhuma versão se autodeclara sucessora oficial.
- Simular o impacto financeiro da migração v1→v2 combinando as faixas de peso e regiões, para dimensionar o efeito real nos custos de frete especial.
- Confirmar o status atual do PROC-043 junto ao Compliance, já que ambas as versões do PROC-042 dependem dele para cargas perigosas.
- Levantar se clientes com aditivo contratual de desconto de volume (negociado sob a v1) foram migrados para os novos percentuais objetivos da v2, e sob qual critério.
- Recomenda-se registrar formalmente, como achado consolidado desta fase de Discovery, a existência de duas versões operacionais coexistentes de um mesmo procedimento crítico de precificação, como risco prioritário a ser resolvido antes de qualquer automação ou IA se apoiar neste conteúdo.