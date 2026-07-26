# Resumo — SLA-2024 Tabela de SLA por Tipo de Cliente

## 1. Metadados / índice base

- **ID do resumo:** RES-003
- **Arquivo de origem:** SLA-2024-tabela-sla-clientes.md
- **Tipo de documento:** SLA (documento contratual — compromissos formais com o cliente)
- **Categoria/tema:** Níveis de serviço e atendimento por tier de cliente
- **Palavras-chave:** tier Gold/Silver/Standard, tempo de primeira resposta, tempo de resolução, incidente crítico, disponibilidade do portal, penalidade contratual, gerente de conta, Azure DevOps
- **Processos relacionados:** Classificação de clientes por tier, atendimento de chamados gerais, atendimento de incidentes críticos, aplicação de penalidades por descumprimento de SLA, medição de SLA via sistema de chamados
- **Versão do documento de origem:** 2024.1 (última atualização 02/01/2024)
- **Data desta análise:** 26/07/2026

## 2. Resumo executivo

O SLA-2024 define, com caráter contratual, os compromissos de atendimento da NovaTech por tier de cliente (Gold, Silver, Standard), classificados por volume mensal de operações ou valor de contrato. Estabelece tempos de primeira resposta e resolução distintos para chamados gerais e incidentes críticos, disponibilidade mínima do portal de tracking e benefícios adicionais (gerente de conta dedicado, relatórios). Define critérios objetivos para o que constitui um incidente crítico e uma escala de penalidades progressivas por violação de SLA no mês. A medição é feita pelo sistema de chamados (Azure DevOps), com regra específica de pausa do relógio de SLA fora do horário comercial — exceto para incidentes críticos de clientes Gold.

## 3. Processos e regras de negócio identificados

- **Classificação de clientes em 3 tiers:** Gold (contrato anual > R$500.000 OU >200 operações/mês), Silver (contrato entre R$100.000 e R$500.000 OU 50–200 operações/mês), Standard (demais clientes). Revisão semestral (Gold/Silver) ou anual (Standard). Solicitações de SLA diferenciado fora dos 3 tiers vão ao Comercial para análise de viabilidade.
- **SLAs de chamados gerais:** primeira resposta em até 2h/4h/8h úteis (Gold/Silver/Standard); resolução em até 24h/48h/72h úteis.
- **SLAs de incidentes críticos:** primeira resposta em até 30min/1h/2h; resolução em até 4h/8h/24h.
- **Disponibilidade do portal de tracking:** 99,5% (Gold), 99,0% (Silver), 98,0% (Standard).
- **Benefícios adicionais:** gerente de conta dedicado apenas para Gold; relatório mensal detalhado (Gold), resumido (Silver) ou sob demanda (Standard).
- **Definição de incidente crítico:** carga de valor declarado > R$100.000 com status desconhecido há mais de 6h; carga perigosa com irregularidade de documentação/rastreamento; mais de 5 chamados do mesmo cliente em 24h sobre o mesmo problema; qualquer risco à segurança de pessoas.
- **Penalidades escalonadas por mês:** 1ª violação — registro interno sem impacto contratual; 2ª violação — crédito de 5% sobre o valor do frete do chamado; 3ª violação ou mais — crédito de 10% + reunião obrigatória com gerente de conta (Gold) ou gerente de operações (Silver/Standard).
- **Medição:** feita pelo sistema de chamados (Azure DevOps), a partir do timestamp de abertura; relógio pausa fora do horário comercial (08h–18h, dias úteis) para chamados gerais, mas não pausa para incidentes críticos de clientes Gold.

## 4. Hipóteses levantadas

- Assume-se que o sistema Azure DevOps está corretamente integrado a todos os canais de abertura de chamado (ex.: Portal do Cliente citado em POL-001), garantindo timestamp confiável para medição de SLA.
- Assume-se que a classificação de tier do cliente é recalculada de forma automática e consistente nas janelas de revisão (semestral/anual), sem defasagem manual.
- Assume-se que o time de atendimento consegue diferenciar corretamente, no momento do chamado, se ele se enquadra como "incidente crítico" segundo os 4 critérios da seção 3, para acionar o SLA correto.
- Assume-se que o cliente tem visibilidade do seu próprio tier e dos SLAs aplicáveis, para poder cobrar cumprimento.
- Assume-se que o crédito financeiro por penalidade é aplicado automaticamente ou com processo já definido em outro documento (não detalhado aqui).

## 5. Riscos identificados

- **Risco de ambiguidade textual:** o documento não define se o relógio de SLA de incidentes críticos para clientes Silver/Standard também deixa de pausar fora do horário comercial, ou se essa exceção vale só para Gold (texto trata apenas do caso Gold, deixando os demais tiers implícitos).
- **Risco operacional:** tempos agressivos de incidente crítico Gold (resposta em até 30min, inclusive fora do horário comercial) exigem plantão ou escala 24/7 — não há confirmação no documento de que essa estrutura existe.
- **Risco financeiro/contratual:** penalidades de crédito (5%/10%) dependem de apuração correta e tempestiva das violações; falha na medição pode gerar créditos indevidos ou não aplicados, com exposição contratual.
- **Risco de negócio:** ausência de tiers além dos três definidos pode gerar atrito comercial em negociações especiais, dependendo inteiramente de análise manual e discricionária do Comercial.
- **Risco de consistência entre processos:** os prazos internos operacionais definidos em outros documentos (ex.: POL-001 — 4h de triagem, 2 dias de coleta, 5 dias de reembolso) não são explicitamente reconciliados com os SLAs de "chamados gerais" ou "incidentes críticos" definidos aqui, criando risco de dois padrões de prazo concorrentes para o mesmo tipo de chamado.
- **Risco de compliance:** por ser documento contratual, qualquer divergência entre o que é comunicado ao cliente e o que é operacionalmente cumprido pode gerar exposição jurídica.

## 6. Lacunas e perguntas em aberto

- Não fica claro se a regra de "relógio não pausa fora do horário comercial" para incidentes críticos se aplica apenas a clientes Gold ou também a Silver/Standard.
- Não é definido o processo operacional de aplicação do crédito financeiro (quem aciona, prazo, forma de compensação — fatura seguinte, estorno etc.).
- Não há detalhamento de como e quando a reunião obrigatória (3ª violação) é agendada, nem suas consequências caso não ocorra.
- Não é especificado o critério de desempate quando um cliente atende a critérios de mais de um tier simultaneamente (ex.: contrato Standard em valor, mas volume de operações Gold).
- Não há relação explícita entre os SLAs aqui definidos e os prazos internos de processos específicos (ex.: devolução em POL-001, aprovação de frete especial em PROC-042) — não fica claro se esses processos estão dentro ou fora do escopo do SLA geral de "chamados".
- Não é definido o que ocorre se o cliente for reclassificado de tier no meio de um chamado em andamento.

## 7. Termos e definições específicas do domínio

- **Tier (Gold/Silver/Standard):** classificação contratual do cliente que determina o nível de serviço aplicável.
- **Incidente crítico:** ocorrência que atende a critérios objetivos de gravidade (valor, carga perigosa, recorrência, risco a pessoas), com SLA mais agressivo que chamados gerais.
- **Tempo de primeira resposta / tempo de resolução:** métricas de SLA medidas a partir da abertura do chamado.
- **Relógio de SLA:** contador de tempo usado para medir cumprimento do SLA, sujeito a pausas conforme regras de horário comercial.

## 8. Referências cruzadas e dependências

- **POL-001 (Política de Devolução):** define prazos internos próprios (triagem em 4h úteis, coleta em 2 dias úteis, reembolso em 5 dias úteis) que não são explicitamente enquadrados como "chamado geral" ou "incidente crítico" neste SLA — recomenda-se cruzamento para verificar se os prazos de POL-001 estão subordinados a este SLA ou são independentes.
- **PROC-042 (Frete Especial, v1 e v2):** menciona aprovação do "gerente de operações regional" para cargas acima de 5.000kg, papel também citado aqui como responsável por reuniões de penalidade (Silver/Standard) — mesmo cargo em contextos diferentes; confirmar se é a mesma função organizacional.
- **Definição de carga perigosa:** este SLA cita "carga perigosa com qualquer irregularidade de documentação ou rastreamento" como gatilho de incidente crítico, o que se relaciona diretamente com as exceções de carga perigosa detalhadas em POL-001 (seção 3.2) — cruzamento recomendado para consistência de critérios.

## 9. Recomendações para a fase de Discovery

- Confirmar com stakeholders se a exceção de "relógio não pausa" em incidentes críticos vale apenas para Gold ou para todos os tiers.
- Levantar o processo operacional de aplicação de créditos por penalidade (sistema, prazo, responsável).
- Mapear explicitamente a relação entre os SLAs gerais desta tabela e os prazos internos de processos específicos (POL-001, PROC-042), para eliminar sobreposição ou lacuna de cobertura.
- Validar se existe estrutura de plantão/escala compatível com o SLA de 30 minutos para incidentes críticos Gold fora do horário comercial.
- Ao final da análise dos 5 documentos, cruzar a definição de "carga perigosa" usada aqui com a definição normativa da POL-001, garantindo consistência terminológica entre os artefatos.