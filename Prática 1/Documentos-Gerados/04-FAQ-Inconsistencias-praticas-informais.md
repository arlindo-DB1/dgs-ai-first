# Inconsistências entre Documentos Formais e Práticas Informais — FAQ-Atendimento

> **Versão:** 1.0
> **Data da versão:** 26/07/2026
> **Responsável:** DB1-Arlindo
> **Status:** Rascunho

## 1. Visão geral

**Escopo desta análise:** cruzamento entre o lado formal do discovery — `01-mapa-temas-cobertos.md`, `02-hipoteses-gaps.md` e `03-analise-inconsistencias-proc042-v1-v2.md` (todos em `Prática 1/Documentos-Gerados/`) — e o lado informal, representado pelos 9 itens completos do `FAQ-atendimento.md` (`Prática 1/Arquivos-de-Trabalho/`). Os quatro documentos foram lidos na íntegra.

Os documentos-fonte primários (POL-001, PROC-042 v1/v2, SLA-2024) **não foram relidos diretamente** nesta análise — eles já estão condensados em 01, 02 e 03, que são usados aqui como proxy do lado formal. Quando um achado depende de um dado que só existe no documento-fonte original (ex.: valores exatos da tabela SLA-2024), isso é sinalizado explicitamente como não confirmável dentro deste escopo, e não como divergência.

**Objetivo:** para cada um dos 9 itens do FAQ, verificar se a prática relatada (a) já está referenciada como conflito ou gap em 01, 02 ou 03 — caso em que esta análise aprofunda o achado —, ou (b) ainda não havia sido cruzada com nada — caso em que é um achado novo.

**Limitação declarada:** o FAQ-Atendimento tem 47 perguntas no documento original; apenas 9 (19%) foram fornecidas e são usadas aqui, mesma amostra já registrada em `01-mapa-temas-cobertos.md` (RES-005). Conclusões desta análise valem apenas para os 9 itens lidos — a ausência de achado em um tema não significa ausência de inconsistência no restante do FAQ.

## 2. Tabela cruzada — FAQ x lado formal

| Item FAQ | Tema/documento formal relacionado | Situação | Referência formal |
|---|---|---|---|
| 3 — Devolução de carga perigosa | Devolução de mercadorias, exceção de carga perigosa (POL-001, via 01) | Extrapolação/exceção | GAP-07 (02) |
| 8 — Frete especial (duas versões) | Governança PROC-042 v1 x v2 (01 §3; 03 INC-01/INC-02) | Ambiguidade herdada + lacuna contratual nova | GAP-01 (02); INC-01/INC-02 (03) |
| 15 — Tier "Platinum" | Nível de serviço, tiers Gold/Silver/Standard (SLA-2024, via 01) | Terminológica/conceitual | Nenhuma (achado novo) |
| 22 — Seguro de carga | Tema órfão sem documento normativo (01 §4) | Lacuna documental | GAP-03 (02) |
| 27 — Tracking prolongado | Sem tema mapeado em 01; tangencia critério de "incidente crítico" (SLA-2024, via 01) | Lacuna documental | Nenhuma (achado novo) |
| 32 — Frete expresso, carga perigosa | Dependência PROC-043 (tema órfão, 01 §4) | Lacuna documental + instabilidade de dependência | GAP-09 (02) |
| 38 — Carga danificada/sinistro | Sobreposição "carga danificada" x "avaria em trânsito" (01 §3) | Ambiguidade/sobreposição de fluxos | GAP-06 (02) |
| 41 — SLA resposta x resolução | Nível de serviço, SLA de resposta/resolução (SLA-2024, via 01) | Consistente (não confirmável integralmente no escopo) | — |
| 45 — Desconto no frete | Frete especial, desconto de volume (01 §3; 03 INC-07) | Contradição direta | GAP-02 (02); INC-07 (03) |

## 3. Inconsistências identificadas

**Tabela-resumo:**

| ID | Inconsistência | Natureza | Criticidade | Resolubilidade | Status |
|---|---|---|---|---|---|
| INC-FAQ-01 | Exceção informal da Gestão de Riscos na devolução de carga perigosa, sem critério documentado | Extrapolação/exceção | Alta | Requer validação externa | Já registrado (GAP-07) |
| INC-FAQ-02 | Convivência não resolvida entre PROC-042 v1 e v2, confirmada pela prática do atendimento | Ambiguidade herdada não resolvida | Crítica | Requer validação externa | Já registrado (GAP-01; INC-01/INC-02 doc 03) |
| INC-FAQ-03 | Indício de clientes com contrato vinculado à tabela de frete antiga (v1), sem regra de migração | Lacuna documental | Alta | Requer validação externa | Novo |
| INC-FAQ-04 | Tier "Platinum" e programa de fidelidade descontinuado (2022) sem documento correspondente | Terminológica/conceitual | Baixa | Resolvível apenas com os documentos analisados | Novo |
| INC-FAQ-05 | Seguro de carga sem norma, com variação adicional não confirmada para contratos anteriores a 2023 | Lacuna documental | Crítica | Requer validação externa | Já registrado (GAP-03) |
| INC-FAQ-06 | Prazos de trânsito por região e critério de priorização de rastreamento (R$ 50.000) sem documento normativo | Lacuna documental | Média | Requer validação externa | Novo |
| INC-FAQ-07 | "Frete expresso" para carga perigosa sem norma própria; autorização real (~2 dias) contradiz a expectativa criada pelo nome do serviço | Lacuna documental | Alta | Requer validação externa | Parcialmente registrado (GAP-09) |
| INC-FAQ-08 | Fronteira não documentada entre "carga danificada" (sinistro, Jurídico, 48h) e "avaria em trânsito" (POL-001 §3.5) | Ambiguidade herdada não resolvida | Alta | Requer validação externa | Já registrado (GAP-06) |
| INC-FAQ-09 | Prática de desconto de volume alinhada à regra da v1, não aos percentuais objetivos da v2 | Contradição direta | Crítica | Requer validação externa | Já registrado (GAP-02; INC-07 doc 03) |
| INC-FAQ-10 | Contradição interna no próprio FAQ: item 8 orienta usar a v2 "na dúvida", mas item 45 descreve prática de desconto seguindo a v1 | Ambiguidade herdada não resolvida | Alta | Resolvível apenas com os documentos analisados | Novo |

### INC-FAQ-01 — Exceção informal em devolução de carga perigosa

**Origem:** FAQ item 3

O FAQ orienta o atendente a não afirmar que a devolução de carga perigosa é impossível, pois "já tiveram casos em que o pessoal de Riscos autorizou exceção", mesmo o processo padrão não permitindo. Nenhum critério objetivo para quando a exceção é concedida está documentado em nenhuma das fontes formais.

**Cross-referência:** aprofunda GAP-07 (`02-hipoteses-gaps.md`).

### INC-FAQ-02 — Convivência PROC-042 v1/v2 confirmada pela prática

**Origem:** FAQ item 8

O FAQ instrui o atendente a "usar a mais recente (v2)" em caso de dúvida — uma regra de governança criada informalmente pelo próprio atendimento, na ausência de uma definição formal de qual versão vigora. Isso confirma, por uma terceira fonte independente, a mesma lacuna já identificada entre os dois documentos.

**Cross-referência:** aprofunda GAP-01 (`02`) e INC-01/INC-02 (`03-analise-inconsistencias-proc042-v1-v2.md`).

### INC-FAQ-03 — Indício de contratos vinculados à tabela antiga

**Origem:** FAQ item 8

O FAQ acrescenta um detalhe ausente em 01, 02 e 03: "se o cliente reclamar do valor, pode ser que o contrato dele ainda esteja na tabela antiga." Isso sugere a existência de contratos comerciais explicitamente atrelados aos parâmetros da v1, um cenário distinto de "confusão do atendente" — é uma possível decisão comercial deliberada, não documentada em nenhuma das fontes formais analisadas, e sem regra de migração ou convivência definida.

**Cross-referência:** acrescenta uma dimensão nova a GAP-01/GAP-10 (`02`).

### INC-FAQ-04 — Tier "Platinum" e programa de fidelidade descontinuado

**Origem:** FAQ item 15

O FAQ esclarece corretamente que não existe tier "Platinum", mas revela a existência de um "programa de fidelidade antigo... descontinuado em 2022" — tema que não consta em nenhum dos cinco documentos-fonte mapeados em `01-mapa-temas-cobertos.md` (nem na lista de temas órfãos, seção 4). Não há indício de conflito prático (o próprio FAQ já orienta a resposta correta), mas é uma lacuna documental nova.

**Cross-referência:** nenhuma — achado novo, candidato a registro como tema órfão adicional em `01`.

### INC-FAQ-05 — Seguro de carga: lacuna documental com variação adicional

**Origem:** FAQ item 22

Confirma a lacuna documental completa já registrada (GAP-03): não há política normativa para o seguro de carga (0,3%/0,8% do valor declarado). O FAQ acrescenta uma complicação não capturada em 02: essa tarifa "vale para contratos a partir de 2023", com contratos mais antigos podendo ter "percentuais diferentes", a confirmar caso a caso com o Comercial — ou seja, mesmo a fonte informal reconhece incerteza sobre parte dos casos.

**Cross-referência:** aprofunda GAP-03 (`02`).

### INC-FAQ-06 — Prazos de trânsito regional e critério de priorização não documentados

**Origem:** FAQ item 27

O FAQ estabelece expectativas operacionais concretas — rotas para o Norte podem levar "até 10 dias úteis"; para Sul/Sudeste, "mais de 3 dias parado é estranho" — e um critério de priorização (chamado de prioridade alta se o cliente for Gold ou a carga valer acima de R$ 50.000). Nenhuma dessas referências numéricas tem documento normativo correspondente entre os 3 documentos formais revisados. O critério de valor tangencia a definição de "incidente crítico" do SLA-2024 citada em `01` (que lista "valor" como um dos critérios objetivos de gravidade), mas o limiar exato de R$ 50.000 não pôde ser confirmado contra o SLA-2024 dentro do escopo desta análise (ver seção 6).

**Cross-referência:** nenhuma — achado novo.

### INC-FAQ-07 — "Frete expresso" de carga perigosa: nome comercial x prazo real

**Origem:** FAQ item 32

O FAQ descreve um produto ("frete expresso" para carga perigosa) sem documento normativo correspondente entre as fontes revisadas, dependente de autorização do Compliance e documentação ANTT atualizada. O próprio FAQ reconhece a contradição: a autorização "na prática... demora uns 2 dias", de forma que "o expresso acaba não sendo tão expresso" — o nome comercial do serviço contradiz o tempo real de disponibilização. A instabilidade da dependência (PROC-043 em revisão pelo Compliance) já era conhecida.

**Cross-referência:** relacionado a GAP-09 (`02`); o produto "frete expresso" em si é lacuna documental nova.

### INC-FAQ-08 — Fronteira não documentada entre "carga danificada" e "avaria em trânsito"

**Origem:** FAQ item 38

O FAQ detalha um fluxo específico para "carga danificada": prazo de 48h para o cliente registrar a ocorrência (com fotos/laudo), reembolso integral se comprovada responsabilidade, encaminhado ao Jurídico via e-mail dedicado — fora do atendimento normal. Esse prazo de 48h e a competência do Jurídico não aparecem reconciliados com os prazos internos de devolução da POL-001 (4h triagem, 2 dias coleta, 5 dias reembolso) nem com o conceito de "avaria em trânsito" (POL-001 §3.5), levantando a mesma dúvida já registrada em GAP-06: são o mesmo evento tratado por dois canais, ou processos distintos?

**Cross-referência:** aprofunda GAP-06 (`02`).

### INC-FAQ-09 — Desconto de volume: prática segue a v1, não a v2

**Origem:** FAQ item 45

O FAQ descreve desconto automático para "clientes com mais de 10 fretes especiais por mês" — a regra e o gatilho exatos da v1 (>10 fretes/mês), não os percentuais objetivos da v2 (5% a partir de 8 fretes/mês, 10% acima de 15). Esta é a evidência mais concreta already conhecida de uso de uma versão superada em produção.

**Cross-referência:** aprofunda GAP-02 (`02`) e INC-07 (`03`).

### INC-FAQ-10 — Contradição interna do próprio FAQ (item 8 x item 45)

**Origem:** FAQ item 8 / FAQ item 45

O item 8 orienta o atendente a, "na dúvida", usar a versão mais recente (v2) do PROC-042. O item 45, porém, descreve a prática de desconto de volume seguindo integralmente a regra da v1. O FAQ — a própria fonte que tenta orientar o atendimento diante da ambiguidade formal — não é internamente consistente: prescreve a v2 como padrão geral, mas relata (sem sinalizar contradição) que a exceção mais financeiramente relevante do procedimento segue a v1. Isso significa que a resolução informal de GAP-01 ("na dúvida, use a v2") não reflete a prática real já documentada no mesmo FAQ.

**Cross-referência:** conecta INC-FAQ-02 e INC-FAQ-09; não constava, como contradição explícita, em 01, 02 ou 03.

## 4. Práticas informais sem contradição, mas candidatas a formalização

- **Critérios de priorização e prazos de trânsito regional (item 27):** não contradizem nenhuma norma existente — porque nenhuma norma existe —, mas são usados operacionalmente hoje e afetam diretamente a expectativa do cliente. Candidatos a formalização em um SLA de rastreamento.
- **Processo de autorização do Compliance para frete expresso (item 32):** o prazo real de ~2 dias não contradiz o PROC-043 nem qualquer regra de compliance formal (nenhuma existe sobre este produto especificamente), mas o descompasso entre a promessa comercial e o prazo operacional é candidato a alinhamento formal.
- **Esclarecimento sobre tiers e programa de fidelidade descontinuado (item 15):** a resposta do FAQ já está correta; falta apenas um registro oficial equivalente, para não depender só do conhecimento do atendimento.

## 5. Pontos confirmados

Itens do FAQ que reforçam a norma formal, sem divergência identificada:

- **Item 41 (SLA de resposta/resolução):** os prazos citados (Gold 2h/24h, Silver 4h/48h, Standard 8h/72h) são apresentados como derivados diretamente da tabela SLA-2024, e a estrutura de dois tipos de prazo (resposta x resolução) é consistente com o que `01-mapa-temas-cobertos.md` descreve para o SLA-2024. Os valores exatos não foram reconfirmados contra o documento-fonte original nesta análise (ver seção 6).
- **Item 45 (governança da autonomia de desconto):** a orientação de que o atendente não tem autonomia para conceder desconto fora da regra automática, encaminhando casos excepcionais ao Comercial com justificativa, é consistente com a atribuição de decisão ao Comercial/Diretoria prevista tanto na v1 quanto na v2 do PROC-042.

## 6. Lacunas e perguntas em aberto específicas desta comparação

- Não foi possível confirmar, dentro do escopo desta análise (01, 02, 03 e FAQ, sem reler o SLA-2024 original), se os prazos citados no item 41 correspondem exatamente à tabela SLA-2024 vigente.
- Não foi possível confirmar se o limiar de R$ 50.000 citado no item 27 corresponde ao critério de valor usado pela definição formal de "incidente crítico" do SLA-2024 — `01` cita o critério em termos gerais, mas não reproduz o valor exato.
- Não há confirmação, em nenhuma das quatro fontes, de que a data de corte de 2023 para as tarifas de seguro de carga (item 22) tenha origem em algum marco documental formal, ou se é apenas uma convenção do atendimento.
- Não é possível, com as fontes analisadas, dimensionar quantos clientes possuem contrato vinculado à tabela de frete antiga (INC-FAQ-03) — o FAQ apenas sinaliza a possibilidade.
- O restante das 38 perguntas do FAQ (fora da amostra de 9 usada aqui e em `01`/`02`) não foi revisado nesta análise; a mesma ressalva de viés de amostragem já registrada em `02-hipoteses-gaps.md` (seção 3) se aplica integralmente a este documento.

## 7. Recomendações

### Ações imediatas (resolvível apenas com os documentos analisados)

1. **(Crítica)** Eliminar a contradição interna do próprio material de apoio ao atendimento: definir uma única orientação sobre qual regra do PROC-042 seguir "na dúvida" — hoje o FAQ recomenda a v2 como padrão (item 8) e, ao mesmo tempo, relata prática de desconto seguindo a v1 (item 45), sem reconciliar as duas (INC-FAQ-10).
2. **(Alta)** Documentar formalmente os critérios de priorização e os prazos esperados de trânsito por região citados no item 27, hoje conhecidos apenas pelo atendimento (INC-FAQ-06).
3. **(Baixa)** Registrar oficialmente o esclarecimento sobre a inexistência do tier "Platinum" e a descontinuação do programa de fidelidade em 2022, incluindo-o como tema órfão em `01-mapa-temas-cobertos.md` (INC-FAQ-04).

### Ações que dependem do negócio (requer validação externa)

1. **(Crítica)** Confirmar junto à Diretoria Comercial/Operações qual versão do PROC-042 está vigente hoje, incluindo tratamento explícito para o indício de contratos vinculados à tabela antiga (INC-FAQ-02, INC-FAQ-03) — relacionado a GAP-01/GAP-10 (`02`) e às recomendações de `03`.
2. **(Crítica)** Definir e comunicar o tratamento da prática de desconto de volume hoje alinhada à v1, diante da regra objetiva da v2 (INC-FAQ-09) — relacionado a GAP-02 (`02`) e INC-07 (`03`).
3. **(Crítica)** Levantar junto ao Comercial/Jurídico o documento normativo do seguro de carga, incluindo a variação para contratos anteriores a 2023 (INC-FAQ-05) — relacionado a GAP-03 (`02`).
4. **(Alta)** Confirmar com a Gestão de Riscos os critérios objetivos hoje usados para autorizar exceções informais de devolução de carga perigosa, avaliando formalização na POL-001 (INC-FAQ-01) — relacionado a GAP-07 (`02`).
5. **(Alta)** Confirmar com Compliance o prazo real de autorização do frete expresso para carga perigosa e avaliar ajuste do nome comercial do serviço ou do SLA interno de autorização (INC-FAQ-07) — relacionado a GAP-09 (`02`).
6. **(Alta)** Esclarecer com Operações/Jurídico a fronteira entre "carga danificada" (sinistro) e "avaria em trânsito", incluindo os prazos de 48h relatados no item 38 (INC-FAQ-08) — relacionado a GAP-06 (`02`).
7. **(Média)** Confirmar contra o documento SLA-2024 original os valores exatos citados no item 41 e o limiar de valor usado para priorização de chamados críticos citado no item 27, antes de qualquer uso normativo desses números.

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 1.0 | 26/07/2026 | DB1-Arlindo | Criação do documento |
