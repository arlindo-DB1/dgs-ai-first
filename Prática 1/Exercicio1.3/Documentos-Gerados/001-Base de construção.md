# Base de Construção — Especificação de Requisitos do Pipeline de RAG

> **Versão:** 1.0
> **Data da versão:** 01/08/2026
> **Responsável:** DB1-Arlindo
> **Status:** Rascunho (em construção — atualizado a cada rodada de refinamento)
> **Fase do projeto:** Intent + Discovery — Passo 3 (Especificação de Requisitos do Pipeline de RAG)

## 1. Contexto

Este documento consolida, de forma progressiva, as decisões de contexto e requisitos combinadas ao longo do refinamento do Exercício 1.3, antes da redação da especificação final. Funciona como fonte de consulta viva — cada nova rodada de conversa pode adicionar, ajustar ou substituir um ponto aqui, sempre com nova versão registrada no histórico de revisões (seção 5).

**Missão desta etapa:** escrever uma especificação de requisitos do produto (não técnica, porém precisa) cobrindo os requisitos que o pipeline de RAG deve atender, ao longo de 5 eixos:
1. Fontes de dados a indexar (e excluir).
2. Tratamento de documentos contraditórios.
3. Comportamento quando não há resposta na base.
4. Requisitos de atualização/frescor da base.
5. Requisitos de rastreabilidade.

## 2. Decisões consolidadas

### 2.1 Fontes de dados a indexar (Eixo 1)

**Decisão:** abordagem híbrida — todas as fontes documentais conhecidas (SharePoint ~800 docs, Confluence ~400 páginas, planilhas de rede, e conteúdo informal como o FAQ-Atendimento) são indexadas, mas conteúdo informal/não normativo recebe uma marcação de **confiabilidade inferior**, exibida de forma visivelmente distinta do conteúdo normativo (POL/PROC/SLA) nas respostas do assistente.

**Por que não as duas opções "puras":**
- Indexar *só* o normativo deixaria temas inteiros sem nenhuma cobertura (ex.: seguro de carga, sem nenhum documento formal — GAP-03), preservando a dependência de conhecimento tácito que é a própria motivação do projeto (o atendimento hoje resolve isso "perguntando para quem sabe").
- Indexar *tudo sem diferenciação* arriscaria o assistente citar uma fonte tecnicamente presente, mas desatualizada ou nunca validada, com falsa confiança (GAP-05) — como já evidenciado pelo próprio FAQ (item 45 segue a regra antiga do PROC-042 v1, não a v2 vigente).

**Requisito complementar — tratamento e formalização obrigatória:** a via híbrida não pode ser um estado permanente. Todo tema identificado como de baixa confiabilidade (coberto apenas por fonte informal, sem documento normativo correspondente) deve ter um plano de formalização aberto junto à área de negócio responsável, com:
- **Prazo:** até 60 dias corridos (2 ciclos mensais de atualização documental) a partir da identificação do tema como baixa confiança — apoiado no próprio ritmo mensal de atualização já praticado pelas 3 áreas (Operações, Compliance, Comercial), conforme o Cenário.
- **Escalonamento e consequência (confirmado):** se o prazo vencer sem formalização, o conteúdo de baixa confiabilidade daquele tema é **indisponibilizado no sistema** — deixa de ser servido pelo assistente (o tema passa a cair em fallback de lacuna) — e o caso é escalado formalmente ao comitê do projeto. Isso torna a consequência tangível: em vez de conteúdo não confiável circular indefinidamente, a ausência de formalização tem um efeito concreto e visível para o negócio.
- **Objetivo do requisito:** evitar que o projeto se torne apenas uma automação do estado atual (informação tácita/informal permanentemente não oficializada) em vez de uma melhoria de governança documental.

### 2.2 Tratamento de documentos contraditórios (Eixo 2)

**Decisão:** resolução de conflitos de governança documental já existentes (ex.: PROC-042 v1 x v2 coexistindo sem versão vigente declarada) é **pré-requisito de go-live por tema** — a NovaTech precisa formalizar qual versão vigora antes de o assistente responder sobre aquele tema em produção.

**Complemento importante (adicionado nesta rodada):** mesmo com esse pré-requisito cumprido, o Fluxo de Fallback (Exercício 1.2) continuará cobrindo casos remanescentes (ambiguidade de fronteira entre documentos válidos, ou lacunas reais). A especificação precisa deixar explícito que **sem a formalização/regularização da fonte de dados, o índice de chamados desviados para fallback tende a ficar artificialmente alto** — o fallback é uma rede de segurança para exceções, não um substituto para a resolução de governança na origem.

### 2.3 Comportamento quando não há resposta na base (Eixo 3)

**Já coberto pelos guardrails do Exercício 1.2** (reaproveitado como requisito desta especificação): o assistente nunca completa uma resposta com conhecimento geral do modelo quando a base restrita não cobre o tema — aciona o fallback de lacuna (Cenário 2 do Fluxo de Fallback), comunica ausência de fonte e registra como "gap de cobertura documental".

### 2.4 Requisitos de atualização/frescor (Eixo 4)

**Decisão:** até **5 dias úteis** entre a publicação/atualização de um documento pela área responsável e sua disponibilidade para consulta pelo assistente.

**Justificativa:** por ser um processo novo em implantação, esse prazo reserva uma janela de revisão humana antes da indexação automática (ex.: checar conflito com conteúdo já indexado), evitando que o pipeline consuma revisões sem controle de qualidade. O prazo é declarado como sujeito a revisão futura, podendo ser reduzido conforme o processo de governança documental amadurecer.

### 2.5 Requisitos de rastreabilidade (Eixo 5)

**Decisão:** toda resposta do assistente deve exibir, no mínimo:
- A fonte (documento de origem).
- O trecho relevante citado.
- **(Promovido de candidato de backlog para requisito confirmado nesta rodada)** a data de última atualização/versão do documento-fonte citado.

**Origem:** os dois primeiros itens já eram guardrails confirmados no Exercício 1.2 (Fluxo Principal). O terceiro item constava como candidato de backlog em `005-Guardrails.md` ("Transparência de vigência da fonte") e passa a ser requisito obrigatório desta especificação.

## 3. Pontos ainda em aberto / pendentes de confirmação

- Estrutura e formato final do documento de especificação (proposta em discussão — ver troca de mensagens; ainda não incorporada aqui até confirmação).

## 4. Como usar este documento

Este arquivo é a fonte de referência para resgatar decisões já tomadas nesta etapa, evitando retrabalho ou contradição entre rodadas de refinamento. Qualquer nova decisão ou ajuste deve ser adicionado aqui, com nova entrada no histórico de revisões abaixo, antes de (ou junto com) qualquer atualização da especificação final.

---

## 5. Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 1.0 | 01/08/2026 | DB1-Arlindo | Criação do documento — consolidação das decisões dos eixos 1 (fontes, incl. requisito de tratamento/formalização), 2 (conflitos, incl. alerta de índice de fallback), 4 (atualização/frescor) e 5 (rastreabilidade) |
| 1.1 | 01/08/2026 | DB1-Arlindo | Confirmado o mecanismo de escalonamento do eixo 1: conteúdo de baixa confiabilidade não formalizado em 60 dias é indisponibilizado no sistema, não apenas escalado |
