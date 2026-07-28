# Hipóteses de Gaps — Discovery NovaTech

> **Versão:** 1.0
> **Data da versão:** 26/07/2026
> **Responsável:** DB1-Arlindo
> **Status:** Rascunho

## 1. Escala de risco

Todos os gaps e riscos deste documento são classificados por **Probabilidade** × **Impacto**, conforme critérios abaixo.

**Probabilidade** — baseada em quantas fontes independentes apontam para o mesmo problema:
- **Baixa:** mencionado por 1 fonte, sem evidência concreta de ocorrência.
- **Média:** mencionado por 1 fonte com evidência explícita, ou por 2 fontes de forma indireta.
- **Alta:** confirmado por 2 ou mais fontes independentes, ou há evidência concreta de materialização (não apenas hipotética).

**Impacto** — baseado em critério de negócio:
- **Baixo:** efeito operacional pontual, sem exposição financeira, contratual ou de compliance relevante.
- **Médio:** afeta um subconjunto de casos/clientes ou gera retrabalho, sem exposição contratual direta.
- **Alto:** exposição financeira, comercial ou de tratamento desigual entre clientes.
- **Crítico:** exposição de compliance/auditoria, risco à credibilidade do futuro assistente de IA, ou risco já materializado em produção.

**Matriz de classificação:**

| Probabilidade \ Impacto | Baixo | Médio | Alto | Crítico |
|---|---|---|---|---|
| **Baixa** | P4 | P4 | P3 | P2 |
| **Média** | P4 | P3 | P2 | P1 |
| **Alta** | P3 | P2 | P1 | P1 |

`P1-Crítico` > `P2-Alto` > `P3-Médio` > `P4-Baixo` (ordem de prioridade para tratamento no discovery).

**Tabela-resumo de todos os gaps identificados:**

| ID | Gap / Risco | Probabilidade | Impacto | Classificação |
|---|---|---|---|---|
| GAP-01 | Governança de versões PROC-042 (v1 x v2 coexistindo, sem substituição formal) | Alta | Crítico | **P1-Crítico** |
| GAP-02 | Uso comprovado de versão desatualizada em campo (desconto de volume) | Alta | Alto | **P1-Crítico** |
| GAP-03 | Seguro de carga sem nenhum documento normativo de referência | Alta | Alto | **P1-Crítico** |
| GAP-04 | Dependência de conhecimento tácito / pessoa-chave ("perguntando para quem sabe") | Alta | Alto | **P1-Crítico** |
| GAP-05 | Risco de o assistente de IA citar fonte conflitante com falsa confiança | Alta | Crítico | **P1-Crítico** |
| GAP-06 | Sobreposição "carga danificada" (sinistro) x "avaria em trânsito" (POL-001 3.5) | Média | Alto | **P2-Alto** |
| GAP-07 | Exceções informais da Gestão de Riscos não documentadas na POL-001 | Média | Alto | **P2-Alto** |
| GAP-08 | Falta de reconciliação entre prazos internos (POL-001) e SLA formal (SLA-2024) | Média | Alto | **P2-Alto** |
| GAP-09 | Dependência do PROC-043 (não obtido, em revisão pelo Compliance) | Alta | Médio | **P2-Alto** |
| GAP-10 | Migração de aditivos contratuais de desconto de volume (v1 → v2) não confirmada | Média | Médio | **P3-Médio** |
| GAP-11 | Ambiguidade: "relógio não pausa" em incidente crítico vale só para Gold? | Média | Médio | **P3-Médio** |
| GAP-12 | Falta de SLA formal da Gestão de Riscos para tratar exceções | Média | Médio | **P3-Médio** |
| GAP-13 | Dependência de sensor IoT para cadeia de frio (falha não materializada na amostra) | Baixa | Alto | **P3-Médio** |
| GAP-14 | Ausência de calendário oficial de feriados nacionais para contagem de prazos | Baixa | Médio | **P4-Baixo** |
| GAP-15 | Ambiguidade do limite de 500kg (inclusivo/exclusivo) no PROC-042 | Baixa | Médio | **P4-Baixo** |
| GAP-16 | Critério de desempate de tier quando cliente atende a mais de um simultaneamente | Baixa | Médio | **P4-Baixo** |

## 2. Hipóteses de gaps identificadas dentro dos documentos já lidos

### Governança documental e precificação (frete especial)

- **GAP-01 — Governança de versões (P1-Crítico):** PROC-042 v1 (RES-002) e v2 (RES-004) coexistem sem que nenhuma delas se declare formalmente sucessora. A v2 define disposições transitórias com corte em 01/12/2023 — data expirada há mais de 2 anos na data desta análise — sem indicar o que rege o cálculo após o fim do período transitório. Confirmado por uma terceira fonte independente: o FAQ (item 8, RES-005) relata que o próprio atendimento já convive com essa ambiguidade na prática.
- **GAP-02 — Uso de versão desatualizada em campo (P1-Crítico):** o FAQ (item 45) descreve a prática de desconto de volume alinhada à regra da v1 (>10 fretes/mês, negociação caso a caso), não aos percentuais objetivos da v2 (5%/10%). É a evidência mais concreta encontrada nesta amostra de que uma versão superada já está em uso operacional — não é uma hipótese, é um fato relatado.
- **GAP-10 — Migração de aditivos contratuais (P3-Médio):** não há confirmação se clientes com desconto de volume negociado sob a lógica da v1 foram migrados para os percentuais objetivos da v2, ou se mantiveram a condição anterior (RES-004).
- **GAP-15 — Ambiguidade de 500kg (P4-Baixo):** a v1 define o escopo do frete especial como cargas "acima de 500kg", mas a tabela de fator de peso inicia a primeira faixa exatamente em 500kg (RES-002) — não confirmado se a v2 repete a mesma ambiguidade.

### Devolução e sinistro

- **GAP-06 — Sobreposição de fluxos (P2-Alto):** o FAQ (item 38) trata "carga danificada" como sinistro, encaminhado ao Jurídico via e-mail dedicado, fora do atendimento normal. A POL-001 (seção 3.5) trata "avaria em trânsito" como motivo de devolução sem custo, dentro do fluxo padrão do Portal do Cliente. Não está confirmado se são o mesmo evento de negócio tratado por dois canais diferentes, ou processos distintos por natureza.
- **GAP-07 — Exceções informais não documentadas (P2-Alto):** o FAQ (item 3) relata que a Gestão de Riscos já concedeu, na prática, exceções ao processo padrão de devolução de carga perigosa — prática não prevista na POL-001, que trata essas categorias como bloqueio formal ao processo padrão.
- **GAP-13 — Dependência de sensor IoT (P3-Médio):** a elegibilidade por ruptura de cadeia de frio depende de sensor IoT (POL-001, RES-001); falha ou descalibração pode gerar decisões incorretas. Não há evidência, nesta amostra, de que o risco já se materializou — por isso a probabilidade é classificada como baixa, apesar do impacto alto.

### SLA e atendimento

- **GAP-08 — Prazos concorrentes (P2-Alto):** a POL-001 define prazos internos próprios (4h de triagem, 2 dias de coleta, 5 dias de reembolso) que não são explicitamente enquadrados como "chamado geral" ou "incidente crítico" no SLA-2024 (RES-003). Não fica claro se um processo está subordinado ao outro ou se são independentes.
- **GAP-11 — Ambiguidade do relógio de SLA (P3-Médio):** o SLA-2024 define que o relógio de incidente crítico não pausa fora do horário comercial para clientes Gold, mas não deixa explícito se essa regra vale também para Silver/Standard (RES-003).
- **GAP-12 — SLA da Gestão de Riscos (P3-Médio):** nem a POL-001 nem o FAQ definem um SLA formal para a Gestão de Riscos responder aos casos de exceção encaminhados — o setor pode se tornar um gargalo sem prazo de resposta ao cliente.
- **GAP-16 — Critério de desempate de tier (P4-Baixo):** o SLA-2024 não define o que ocorre quando um cliente atende a critérios de mais de um tier simultaneamente (ex.: contrato Standard em valor, mas volume de operações Gold) (RES-003).

### Lacunas documentais totais

- **GAP-03 — Seguro de carga (P1-Crítico):** o FAQ (item 22) descreve um produto com impacto financeiro direto ao cliente (0,3%/0,8% do valor declarado), sem que nenhum dos outros 4 documentos analisados contenha uma política normativa correspondente. É a lacuna documental mais completa encontrada nesta amostra — não há apenas ambiguidade entre versões, não há documento nenhum.
- **GAP-14 — Calendário de feriados (P4-Baixo):** a POL-001 usa "dias úteis" como base do prazo de 7 dias, mas não referencia o calendário oficial de feriados nacionais usado para essa contagem (RES-001).

## 3. Riscos de processos não mapeados

Esta seção trata de um tipo de risco diferente dos anteriores: não é um risco *dentro* do conteúdo já lido, é um risco *sobre o método* — o que ainda não foi lido e pode mudar as conclusões acima.

- **Escala da amostra:** os 5 documentos analisados representam uma fração mínima do universo declarado no Cenário (~800 documentos SharePoint + ~400 páginas Confluence + planilhas de rede). Qualquer hipótese de gap aqui é válida apenas para o que foi lido — não se pode presumir que o restante da documentação está livre dos mesmos problemas (conflito de versão, ausência de norma, ambiguidade textual). Classificação: **Alta probabilidade de existirem gaps adicionais fora da amostra, impacto não quantificável até a próxima leitura.**
- **PROC-043 (Frete de Cargas Perigosas):** referenciado como dependência direta por PROC-042 v1 e v2 para cálculo de frete de carga perigosa pesada; a v2 informa que está em revisão pelo Compliance, introduzindo uma fonte de instabilidade externa ao próprio PROC-042 (GAP-09, P2-Alto).
- **PROC-088 (Interceptação de Carga):** referenciado pela POL-001 para mercadoria ainda em trânsito; fronteira entre os dois processos (devolução pós-entrega x interceptação em trânsito) não pode ser avaliada sem o documento.
- **Tabela mensal de fretes (valor base):** citada como fonte externa pelo PROC-042 (v1 e v2) para todo o cálculo de frete especial; sem ela, não é possível validar se o valor final cobrado ao cliente está correto em nenhuma das duas versões.
- **Restante do FAQ-Atendimento (38 de 47 perguntas):** a amostra de 9 perguntas já revelou 2 conflitos com documentos normativos (GAP-02, GAP-06/07) e uma lacuna documental completa (GAP-03). Não há base para presumir que as 38 perguntas restantes não contêm achados equivalentes ou mais críticos.
- **Governança de atualização da documentação:** o Cenário informa que a documentação é atualizada mensalmente por 3 áreas diferentes (Operações, Compliance, Comercial) sem processo unificado de revisão — este é provavelmente o mecanismo que produziu o conflito PROC-042 v1/v2, e pode estar produzindo conflitos equivalentes em outros documentos ainda não lidos.

## 4. Riscos específicos ao projeto de assistente de IA

Estes riscos não afetam apenas a operação da NovaTech — afetam diretamente a viabilidade do assistente de IA que a DB1 vai construir:

- **GAP-05 (P1-Crítico):** se o assistente responder com base em uma versão desatualizada ou conflitante de um documento (ex.: PROC-042 v1 quando a v2 já está em uso, ou vice-versa), ele vai citar a fonte "corretamente" do ponto de vista técnico (o documento existe, o trecho existe) e mesmo assim entregar uma resposta errada ao atendente — isso é mais perigoso do que uma resposta sem fonte, porque gera falsa confiança.
- A governança de fonte (qual documento é a "verdade" quando há conflito) precisa ser resolvida **antes** de qualquer pipeline de RAG ser construído sobre esses documentos — não é um ajuste que se faz depois, é um pré-requisito.
- Enquanto GAP-01, GAP-03 e GAP-06 não forem resolvidos com o negócio, qualquer resposta do assistente sobre frete especial, seguro de carga ou carga danificada carrega risco herdado de uma ambiguidade que já existe na documentação-fonte, independentemente da qualidade do modelo ou do pipeline de busca.

## 5. Outros pontos para manter no radar

- **GAP-04 — Dependência de pessoa-chave (P1-Crítico):** o próprio Cenário descreve que hoje a equipe resolve contradições "perguntando para quem sabe" — isso é ao mesmo tempo a motivação central do projeto e um risco de bus factor que pode continuar existindo mesmo depois do assistente entrar em produção, se a governança documental não for corrigida na origem.
- **Ambiguidades textuais/numéricas** entre documentos (ex.: limite de 500kg, faixas que se sobrepõem) podem gerar respostas tecnicamente corretas (citando a fonte certa) mas materialmente erradas, se a ambiguidade da fonte não for resolvida antes.
- **Viés de amostragem:** toda conclusão deste documento é limitada aos 5 documentos e às 9 perguntas do FAQ efetivamente lidas. Recomenda-se tratar as classificações de risco acima como piso, não como teto.

## 6. Recomendações para a próxima rodada de discovery

Priorizadas pela classificação de risco da seção 1:

1. **(P1) Resolver a governança de versões do PROC-042** junto à Diretoria Comercial/Operações — qual versão está vigente hoje, e atualizar o treinamento do atendimento de acordo (GAP-01, GAP-02).
2. **(P1) Levantar o documento normativo do seguro de carga** junto ao Comercial/Jurídico (GAP-03).
3. **(P1) Tratar a dependência de conhecimento tácito** como parte do escopo do projeto, não só como sintoma — mapear quem são as "pessoas que sabem" hoje (GAP-04).
4. **(P1) Definir, antes de qualquer construção de RAG, uma estratégia de governança de fonte** para documentos conflitantes ou sem indicação de vigência (GAP-05).
5. **(P2) Esclarecer com Operações/Jurídico** se "carga danificada" e "avaria em trânsito" são o mesmo processo tratado por dois canais (GAP-06).
6. **(P2) Validar com a Gestão de Riscos** se as exceções informais devem ser formalizadas na POL-001 (GAP-07).
7. **(P2) Mapear explicitamente a relação entre os SLAs do SLA-2024 e os prazos internos** de POL-001 e PROC-042 (GAP-08).
8. **(P2) Confirmar o status do PROC-043** junto ao Compliance (GAP-09).
9. **(P3/P4)** Tratar os demais gaps (GAP-10 a GAP-16) na próxima rodada de entrevistas, sem bloquear o avanço do discovery por eles.
10. Solicitar acesso às 38 perguntas restantes do FAQ e ao restante do universo documental por amostragem adicional, para reduzir o viés de amostragem descrito na seção 5.

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 1.0 | 26/07/2026 | DB1-Arlindo | Criação do documento |
