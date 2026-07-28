# Mapa de Temas Cobertos — Discovery NovaTech

> **Versão:** 1.0
> **Data da versão:** 26/07/2026
> **Responsável:** DB1-Arlindo
> **Status:** Rascunho

## 1. Visão geral

Este mapa consolida os temas cobertos pelos 5 documentos-chave da NovaTech (POL-001, PROC-042 v1, PROC-042 v2, SLA-2024, FAQ-Atendimento) e pelo resumo de Cenário do projeto, a partir dos resumos já produzidos em `Resumos-Discovery/`.

- **Universo documental declarado no Cenário:** ~800 documentos no SharePoint corporativo, ~400 páginas na wiki Confluence e uma pasta de rede com planilhas de referência atualizadas mensalmente por 3 áreas (Operações, Compliance, Comercial).
- **Cobertura real desta análise:** 5 documentos-fonte, representando uma fração mínima (bem menos de 1%) do universo de ~1.200 documentos/páginas conhecido — sem contar as planilhas de rede, cujo volume não é quantificado no Cenário.
- **Amostragem dentro da própria fonte:** do FAQ-Atendimento (47 perguntas no total), apenas 9 itens (19%) foram fornecidos para esta análise (RES-005).
- **Conclusão da visão geral:** este mapa deve ser lido como um ponto de partida do discovery, não como inventário. A ausência de um tema aqui não significa que ele não exista na documentação da NovaTech — significa apenas que ainda não foi analisado.

## 2. Temas cobertos por domínio de negócio

| Domínio | Temas cobertos | Documento(s) fonte | ID do resumo |
|---|---|---|---|
| Devolução de mercadorias (logística reversa) | Prazo de 7 dias úteis, exceções (carga perigosa, cadeia de frio, lacre violado), rateio de custo do frete reverso, coleta reversa, reembolso/crédito | POL-001 | RES-001 |
| Frete especial (precificação) | Fórmula de cálculo, multiplicadores regionais, fator de peso, aprovação de cargas acima de 5.000kg, desconto de volume, disposições transitórias | PROC-042 v1, PROC-042 v2 | RES-002, RES-004 |
| Nível de serviço / atendimento contratual | Tiers Gold/Silver/Standard, SLA de resposta e resolução, definição de incidente crítico, penalidades escalonadas, disponibilidade do portal | SLA-2024 | RES-003 |
| Conhecimento operacional prático / exceções informais | Exceção informal em carga perigosa, tier inexistente ("Platinum"), seguro de carga, tracking prolongado, frete expresso, carga danificada/sinistro, desconto de frete praticado em campo | FAQ-Atendimento (amostra de 9 de 47 perguntas) | RES-005 |
| Contexto de negócio / projeto | Volume de chamados, fontes de documentação existentes, objetivo do assistente de IA, orçamento e prazo do projeto | Resumo-Cenário | — (cenário-âncora, sem ID de resumo) |

## 3. Sobreposições e conflitos entre fontes

Estes são achados do próprio mapa — pontos em que duas ou mais fontes tratam do mesmo tema de forma divergente ou não reconciliada. (Análise de risco detalhada em `02-hipoteses-gaps.md`.)

- **PROC-042 v1 x v2:** duas versões do mesmo procedimento coexistindo sem indicação formal de qual prevalece, mesmo a v2 trazendo disposições transitórias que pressupõem vigência a partir de 01/12/2023 (data já expirada na data desta análise). Confirmado de forma independente pelo FAQ (item 8).
- **Desconto de volume no PROC-042:** o FAQ (item 45) relata prática de campo alinhada à regra da v1 (>10 fretes/mês, negociação caso a caso), não aos percentuais objetivos da v2 (5% a partir de 8 fretes/mês, 10% acima de 15) — evidência concreta de uso de versão desatualizada em produção.
- **Carga danificada/sinistro (FAQ item 38) x avaria em trânsito (POL-001, seção 3.5):** possível sobreposição de dois fluxos (Jurídico via e-mail de sinistros x Portal do Cliente) para o mesmo tipo de evento, sem fronteira documentada entre os dois.
- **Exceção de devolução de carga perigosa:** a POL-001 encaminha o caso à Gestão de Riscos como processo formal; o FAQ (item 3) relata concessão informal de exceção ao processo padrão pelo mesmo setor — a prática relatada extrapola o que está normatizado.
- **"Gerente de operações regional":** mesmo cargo citado no PROC-042 (aprovação de cargas acima de 5.000kg) e no SLA-2024 (reunião obrigatória de penalidade para Silver/Standard) — não confirmado se é a mesma função organizacional nos dois contextos.
- **Prazos internos x SLA formal:** a POL-001 define prazos operacionais próprios (4h de triagem, 2 dias de coleta, 5 dias de reembolso) que não são explicitamente enquadrados nas categorias "chamado geral" ou "incidente crítico" do SLA-2024 — dois padrões de prazo potencialmente concorrentes para o mesmo tipo de chamado.
- **Definição de "carga perigosa":** usada tanto na POL-001 (seção 3.2, exceção de devolução) quanto no SLA-2024 (gatilho de incidente crítico) — consistência terminológica entre os dois documentos ainda não confirmada.

## 4. Temas mencionados mas não analisados (temas órfãos)

Itens citados como dependência por algum dos 5 documentos, mas que não fazem parte do conjunto analisado até o momento:

- **PROC-043 (Frete de Cargas Perigosas):** referenciado por PROC-042 v1 e v2 como dependência direta para cálculo de frete de carga perigosa pesada; a v2 informa que está em revisão pelo Compliance.
- **PROC-088 (Procedimento de Interceptação de Carga):** referenciado pela POL-001 (seção 2 — Escopo) para casos de mercadoria ainda em trânsito.
- **Seguro de carga:** mencionado no FAQ (item 22) com impacto financeiro direto ao cliente (0,3% do valor declarado para carga padrão, 0,8% para carga perigosa), sem nenhum documento normativo correspondente entre os 5 analisados.
- **Tabela mensal de fretes (valor base):** citada como fonte externa tanto no PROC-042 v1 quanto na v2 para o cálculo de frete especial; não incluída no conjunto analisado.
- **Calendário oficial de feriados nacionais:** citado pela POL-001 como base de contagem de dias úteis, sem documento de referência.
- **Processo/SLA da Gestão de Riscos:** setor citado pela POL-001 e pelo FAQ como responsável por tratar exceções (carga perigosa, cadeia de frio, lacre violado), sem SLA ou processo próprio documentado.
- **Processo de aplicação de crédito por penalidade (SLA-2024):** mencionado na tabela de penalidades, mas o mecanismo operacional não é detalhado.
- **Restante do FAQ-Atendimento:** 38 das 47 perguntas originais não foram fornecidas nesta amostra.

## 5. Glossário consolidado

| Termo | Definição (conforme fonte) | Fonte |
|---|---|---|
| CT-e | Conhecimento de Transporte Eletrônico, documento fiscal que acompanha a carga | POL-001 |
| ANTT | Agência Nacional de Transportes Terrestres; referência regulatória para classificação de cargas perigosas (classes 1–6) | POL-001 |
| Cadeia de frio | Manutenção da temperatura da carga na faixa da nota fiscal; ruptura = saída da faixa por mais de 30 min contínuos | POL-001 |
| Lacre de segurança | Selo físico de inviolabilidade da carga; violação sem documentação bloqueia a devolução padrão | POL-001 |
| Coleta reversa / Frete reverso | Processo logístico de retirada da mercadoria devolvida / custo de transporte da devolução | POL-001 |
| Multiplicador regional / Fator de peso | Fatores aplicados ao valor base do frete conforme região e faixa de peso da carga | PROC-042 v1/v2 — **atenção:** mesmos nomes de termo, valores numéricos diferentes entre v1 e v2 (ver seção 3) |
| Disposições transitórias | Regras que definem qual versão de um procedimento se aplica a casos abertos antes/depois de uma data de corte | PROC-042 v2 |
| Tier (Gold/Silver/Standard) | Classificação contratual do cliente que determina o nível de serviço aplicável | SLA-2024 |
| Incidente crítico | Ocorrência que atende critérios objetivos de gravidade (valor, carga perigosa, recorrência, risco a pessoas), com SLA mais agressivo | SLA-2024 |
| Relógio de SLA | Contador de tempo usado para medir cumprimento do SLA, sujeito a pausas por horário comercial | SLA-2024 |
| Gestão de Riscos (ramal 4500) | Setor responsável por tratativas de exceção em devoluções de carga perigosa, cadeia de frio rompida e lacre violado | POL-001, FAQ |
| Seguro de carga | Produto adicional cobrado como percentual do valor declarado da mercadoria | FAQ |
| Sinistro | Ocorrência de carga danificada tratada pelo Jurídico via e-mail dedicado, distinta do fluxo padrão de devolução | FAQ |

**Termos com uso potencialmente inconsistente ou sobreposto** (ver seção 3 para detalhe): "gerente de operações regional"; "carga danificada" (FAQ) vs. "avaria em trânsito" (POL-001).

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 1.0 | 26/07/2026 | DB1-Arlindo | Criação do documento |
