# Análise de Inconsistências — PROC-042 v1 x v2 (Frete Especial)

> **Versão:** 1.0
> **Data da versão:** 26/07/2026
> **Responsável:** DB1-Arlindo
> **Status:** Rascunho

## 1. Visão geral

**Escopo desta análise:** comparação direta e completa entre os dois documentos-fonte na íntegra — `PROC-042-frete-especial-v1.md` (v1, emitido em 03/03/2023) e `PROC-042-v2-frete-especial-revisado.md` (v2, emitido em 10/11/2023), ambos em `Prática 1/Arquivos-de-Trabalho/`. Os resumos RES-002 e RES-004 (`Resumos-Discovery/`) e o documento `02-hipoteses-gaps.md` foram usados apenas como referência de contexto já levantado, nunca como fonte primária desta comparação.

**Objetivo:** identificar, classificar e detalhar todas as divergências entre as duas versões do procedimento de cálculo de frete especial, aprofundando os achados GAP-01 e GAP-02 já registrados em `02-hipoteses-gaps.md`.

**Limitação declarada:** esta análise não considera nenhum documento fora deste par (POL-001, SLA-2024, FAQ-Atendimento, PROC-043, tabela mensal de fretes). Achados que dependeriam desses documentos para confirmação (ex.: uso real em campo, status do PROC-043) são sinalizados como "Requer validação externa/negócio", não como fatos confirmados por esta análise.

## 2. Tabela comparativa parâmetro a parâmetro

| Parâmetro | v1 (§) | v2 (§) | Divergência |
|---|---|---|---|
| Fórmula de cálculo | Valor base × Multiplicador regional × Fator de peso (§2) | Idêntica (§2) | Nenhuma |
| Valor base | Tabela mensal de fretes (§2) | Idêntica (§2) | Nenhuma |
| Fator de peso — 500 a 1.000kg | 1,0 (§2) | 1,0 (§2) | Nenhuma |
| Fator de peso — 1.001 a 3.000kg | 1,2 (§2) | 1,15 (§2) | **Divergente** (redução) |
| Fator de peso — acima de 3.000kg | 1,5 (§2) | 1,4 (§2) | **Divergente** (redução) |
| Multiplicador regional — Sul | 1,2 (§2.1) | 1,3 (§2.1) | **Divergente** (aumento) |
| Multiplicador regional — Sudeste | 1,0 (§2.1) | 1,1 (§2.1) | **Divergente** (aumento) |
| Multiplicador regional — Centro-Oeste | 1,3 (§2.1) | 1,4 (§2.1) | **Divergente** (aumento) |
| Multiplicador regional — Nordeste | 1,4 (§2.1) | 1,5 (§2.1) | **Divergente** (aumento) |
| Multiplicador regional — Norte | 1,6 (§2.1) | 1,8 (§2.1) | **Divergente** (aumento) |
| Prazo de entrega adicional | +2 dias úteis (§3) | +3 dias úteis (§3) | **Divergente** |
| Aprovação de cargas > 5.000kg | Gerente de operações regional (§4) | Idêntica (§4) | Nenhuma |
| Dependência PROC-043 | Referenciado sem ressalva (§4) | Referenciado + nota de revisão pelo Compliance (§4) | **Divergente** (informação adicional) |
| Desconto de volume — gatilho | > 10 fretes/mês (§4) | ≥ 8 fretes/mês (§4) | **Divergente** |
| Desconto de volume — mecanismo | Negociação caso a caso + aditivo contratual (§4) | Percentual objetivo: 5%/10% (§4) | **Divergente** |
| Disposições transitórias | Seção inexistente | §5 — corte em 01/12/2023 | **Estrutural** — presente apenas na v2 |
| Status de vigência declarado (cabeçalho) | Sem indicação formal, coexiste com v2 | Sem indicação formal de substituir v1 | Ambos ambíguos, de formas distintas (ver INC-01) |
| Responsável | Diretoria Comercial | Idêntica | Nenhuma |
| Escopo declarado (§1) | Cargas acima de 500kg | Cargas acima de 500kg, com linguagem de "parâmetros atualizados" | Mesmo escopo numérico; linguagem diverge (ver INC-03) |

## 3. Inconsistências identificadas

**Tabela-resumo:**

| ID | Inconsistência | Tipo | Criticidade | Resolubilidade |
|---|---|---|---|---|
| INC-01 | Governança de vigência: nenhuma versão se declara formalmente sucessora/vigente | Governança documental/vigência | **Crítica** | Requer validação externa/negócio |
| INC-02 | Contradição interna na v2: cabeçalho nega substituição formal, mas §5 já trata a v2 como vigente a partir de 01/12/2023 (data expirada) | Contradição interna | **Crítica** | Resolvível apenas com os 2 documentos |
| INC-03 | Contradição interna adicional na v2: §1 usa linguagem de atualização/revisão que pressupõe suceder a v1 | Contradição interna | Média | Resolvível apenas com os 2 documentos |
| INC-04 | Divergência no fator de peso (faixas 1.001–3.000kg e acima de 3.000kg) | Numérica/valor | Alta | Resolvível apenas com os 2 documentos |
| INC-05 | Divergência nos multiplicadores regionais (todas as 5 regiões) | Numérica/valor | Alta | Resolvível apenas com os 2 documentos |
| INC-06 | Divergência no prazo de entrega adicional (+2 dias x +3 dias) | Numérica/valor | Alta | Resolvível apenas com os 2 documentos |
| INC-07 | Divergência na regra de desconto de volume (negociação caso a caso x percentuais objetivos) | Regra de negócio/processo | Alta | Requer validação externa/negócio |
| INC-08 | Divergência na nota sobre dependência do PROC-043 (só a v2 sinaliza revisão pelo Compliance) | Dependência externa | Média | Requer validação externa/negócio |

### INC-01 — Governança de vigência entre v1 e v2

**Origem:** v1 (cabeçalho) / v2 (cabeçalho)

O cabeçalho da v1 declara: *"Este documento não possui indicação formal de vigência ou obsolescência no sistema da NovaTech. Coexiste com a versão PROC-042-v2."* O cabeçalho da v2 declara: *"Este documento não possui indicação formal de que substitui o PROC-042 v1. Ambos coexistem no SharePoint sem hierarquia clara."* Nenhum dos dois documentos se autodeclara vigente ou sucessor, apesar de a v2 ser posterior (10/11/2023) e reformular integralmente os parâmetros numéricos da v1.

**Cross-referência:** aprofunda GAP-01 (`02-hipoteses-gaps.md`).

### INC-02 — Contradição interna na v2 (cabeçalho x §5)

**Origem:** v2 §cabeçalho / v2 §5

O cabeçalho da v2 nega qualquer status de substituição formal da v1. Ainda assim, a §5 (Disposições transitórias) define uma regra de corte em 01/12/2023 que só faz sentido se a v2 for, de fato, a versão vigente a partir dessa data — pressuposto que contradiz a própria declaração de status do documento. Além disso, essa data de corte está expirada há mais de 2 anos na data desta análise (26/07/2026), e a v2 não define o que rege o cálculo para chamados abertos após o fim do período transitório.

**Cross-referência:** aprofunda GAP-01.

### INC-03 — Contradição interna adicional na v2 (cabeçalho x §1)

**Origem:** v2 §1

O §1 (Objetivo) da v2 afirma definir *"a fórmula e os parâmetros atualizados"* e que *"os multiplicadores foram revisados para refletir os custos operacionais atualizados de cada região"* — linguagem que só faz sentido enquadrando a v2 como sucessora da v1, reforçando a mesma contradição de status já apontada em INC-02, agora também no objetivo declarado do documento.

**Cross-referência:** reforça INC-02 e GAP-01.

### INC-04 — Divergência no fator de peso

**Origem:** v1 §2 / v2 §2

v1 define fator de peso 1,2 (1.001–3.000kg) e 1,5 (acima de 3.000kg); v2 reduz para 1,15 e 1,4, respectivamente. A faixa 500–1.000kg permanece igual (1,0) nas duas versões.

### INC-05 — Divergência nos multiplicadores regionais

**Origem:** v1 §2.1 / v2 §2.1

Todos os multiplicadores regionais da v2 são superiores aos da v1 (Sul 1,2→1,3; Sudeste 1,0→1,1; Centro-Oeste 1,3→1,4; Nordeste 1,4→1,5; Norte 1,6→1,8). Como o fator de peso caiu (INC-04) e o multiplicador regional subiu, o efeito financeiro líquido da migração v1→v2 varia por combinação de região/faixa de peso e não é trivial de prever sem simulação numérica.

### INC-06 — Divergência no prazo de entrega adicional

**Origem:** v1 §3 / v2 §3

v1 define +2 dias úteis para manuseio de carga pesada; v2 define +3 dias úteis para manuseio e roteirização, citando textualmente que *"anteriormente era +2 dias na versão anterior"* — é o único ponto em que a v2 reconhece explicitamente, no corpo do texto (fora do cabeçalho), ser uma revisão da v1.

### INC-07 — Divergência na regra de desconto de volume

**Origem:** v1 §4 / v2 §4

v1 prevê negociação caso a caso pelo Comercial para clientes com mais de 10 fretes especiais/mês, formalizada em aditivo contratual, sem percentual definido. v2 substitui por regra objetiva: 5% de desconto a partir de 8 fretes/mês, 10% acima de 15 fretes/mês, com aprovação da Diretoria Comercial para descontos maiores. Nenhum dos dois documentos define o que ocorre com aditivos contratuais já firmados sob a lógica da v1 quando a v2 entra em uso.

### INC-08 — Divergência na nota sobre dependência do PROC-043

**Origem:** v1 §4 / v2 §4

v1 apenas referencia o PROC-043 para cargas perigosas acima de 500kg, sem nenhuma ressalva. v2 adiciona a nota *"a PROC-043 está em processo de revisão pelo Compliance e pode sofrer alterações"* — informação de instabilidade inexistente na v1, sinalizando que o status dessa dependência externa mudou entre a emissão das duas versões (mar/2023 → nov/2023).

## 4. Contradições internas

Esta seção consolida as duas contradições internas identificadas (detalhadas em INC-02 e INC-03), ambas exclusivas da v2:

- A v2 nega, no cabeçalho, ter status formal de substituição da v1 — mas define regras de transição (§5) e usa linguagem de objetivo (§1) que só fazem sentido se ela própria for a versão vigente. A v1 não apresenta esse tipo de contradição interna (seu cabeçalho é consistente com o restante do texto).
- Nenhuma das duas contradições é resolvida dentro do próprio documento — ambas dependem de decisão externa (INC-01) para deixar de ser contradição e passar a ser regra clara.

## 5. Pontos sem divergência

Itens verificados nos dois documentos e confirmados como idênticos — registrados para deixar explícito o que foi checado e não gerou achado:

- Fórmula geral de cálculo (Valor base × Multiplicador regional × Fator de peso) — texto idêntico.
- Definição do valor base como tarifa da tabela mensal de fretes — texto idêntico.
- Fator de peso da faixa 500–1.000kg (1,0) — valor idêntico.
- Regra de aprovação prévia do gerente de operações regional para cargas acima de 5.000kg — texto idêntico.
- Escopo numérico declarado (cargas acima de 500kg) — idêntico; a ambiguidade sobre se 500kg exatos estão dentro ou fora do escopo (limite inclusivo/exclusivo) existe de forma **idêntica** nas duas versões — a v2 não resolveu essa ambiguidade herdada da v1 (ver seção 6).
- Responsável pelo documento (Diretoria Comercial) — idêntico.

## 6. Lacunas e perguntas em aberto específicas desta comparação

- Não há, em nenhuma das duas versões, definição de qual regra vale para o cálculo de frete especial após o fim do período transitório (pós 01/12/2023) — a data está expirada há mais de 2 anos e nenhum documento define um comportamento padrão para o período seguinte.
- A ambiguidade do limite de 500kg (inclusivo ou exclusivo) persiste idêntica entre v1 e v2 — a revisão que gerou a v2 não tratou esse ponto.
- Nenhuma das duas versões define o tratamento para clientes com aditivo contratual de desconto de volume já firmado sob a lógica da v1 no momento em que a v2 entra em uso.
- Não é possível, com base apenas nos dois documentos, calcular o efeito financeiro líquido da combinação "fator de peso reduzido + multiplicador regional aumentado" — isso exigiria simulação numérica por região e faixa de peso, fora do escopo desta análise.
- Nenhuma das duas versões define prazo ou critério para quando o PROC-043 (referenciado por ambas) deve ser atualizado, nem o que fazer enquanto isso.

## 7. Recomendações

### Ações imediatas (resolvível apenas com os 2 documentos)

1. **(Crítica)** Corrigir a contradição interna da v2 entre o cabeçalho e a §5 — inserir uma declaração explícita de vigência (mesmo que seja "não vigente") ou remover a linguagem de transição que pressupõe o contrário (INC-02).
2. **(Média)** Alinhar a linguagem do §1 (Objetivo) da v2 com o status de vigência declarado no cabeçalho, eliminando o uso de termos como "atualizado"/"revisado" caso a v2 não seja formalmente reconhecida como sucessora (INC-03).
3. **(Alta)** Documentar, em ambas as versões (ou em um adendo comum), o efeito financeiro esperado da combinação fator de peso reduzido + multiplicador regional aumentado, com exemplos de cálculo por região e faixa de peso (INC-04, INC-05).
4. **(Alta)** Definir explicitamente, no texto do procedimento, a regra de cálculo aplicável a chamados abertos após o encerramento do período transitório (INC-02, INC-06).

### Ações que dependem do negócio (requer validação externa)

1. **(Crítica)** Confirmar junto à Diretoria Comercial/Operações qual versão do PROC-042 está oficialmente vigente na data desta análise (26/07/2026) (INC-01) — relacionado a GAP-01, `02-hipoteses-gaps.md`.
2. **(Alta)** Definir e comunicar o tratamento de aditivos contratuais de desconto de volume negociados sob a lógica da v1 diante da regra objetiva da v2 (INC-07) — tema relacionado a GAP-10, `02-hipoteses-gaps.md`.
3. **(Média)** Confirmar junto ao Compliance o status atual do PROC-043 e seu impacto sobre o cálculo de frete de cargas perigosas pesadas, dependência comum às duas versões (INC-08) — relacionado a GAP-09, `02-hipoteses-gaps.md`.

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 1.0 | 26/07/2026 | DB1-Arlindo | Criação do documento |
