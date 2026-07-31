# Premissas — Jornada do Atendente com Assistente de IA

> **Versão:** 1.0
> **Data da versão:** 30/07/2026
> **Responsável:** DB1-Arlindo
> **Status:** Rascunho
> **Fase do projeto:** Intent + Discovery — Passo 1 (Refinamento de contexto → Premissas)

## 1. Contexto

Este documento consolida as premissas iniciais da jornada do atendente utilizando o assistente de IA, construídas a partir do material de discovery fornecido (Resumo do Cenário, Dados-Discovery.md, e os documentos gerados na etapa anterior: `01-mapa-temas-cobertos.md`, `02-hipoteses-gaps.md`, `03-analise-inconsistencias-proc042-v1-v2.md`, `04-FAQ-Inconsistencias-praticas-informais.md`).

**Natureza deste documento:** trata-se de uma fase inicial de trabalho. As premissas aqui registradas cobrem os pontos principais necessários para avançar ao desenho dos fluxos — não esgotam todas as análises e explorações possíveis. Premissas podem ser revisadas conforme o trabalho avançar.

**Uso deste documento:** esta é a fonte de referência para resgate das premissas em qualquer etapa futura do trabalho.

## 2. Premissas

### 2.1 Canal de contato do cliente

Para efeito desta jornada, o atendimento é tratado a partir do momento em que já existe um "chamado" recebido pelo atendente, independentemente de qual canal o cliente usou para chegar até ele. O foco da jornada é o atendente e sua interação com a IA, não o canal de entrada do cliente.

### 2.2 Interação atendente-IA

O assistente opera com interação em **linguagem natural** (conversa livre, sem formulários rígidos), como uma versão da Claude/Anthropic com **base de conhecimento restrita** — responde exclusivamente a partir da documentação da NovaTech ingerida, sem recorrer a conhecimento geral do modelo para preencher lacunas. O assistente é acessível dentro do ambiente de trabalho do atendente (Teams/SharePoint, conforme o Cenário); a interface exata será detalhada no Passo 2 (desenho dos fluxos), não é uma decisão de premissa.

### 2.3 Base documental

A base que sustenta o assistente é **unificada, padronizada e revisada**, com as seguintes regras de governança:
- **Versionamento único ativo:** para subir uma nova versão de um documento, a versão anterior é obrigatoriamente baixada/desativada — nunca duas versões vigentes coexistindo (elimina cenários como o conflito PROC-042 v1 x v2 identificado no discovery).
- **Responsável pela manutenção:** existe um dono/responsável pela base, acionável quando necessário (correção, criação de conteúdo ausente).

### 2.4 Validação da resposta pelo atendente

Não há um gate formal de aprovação antes do uso da resposta — isso travaria o processo. A validação é **implícita na leitura da resposta com a fonte citada**, no mesmo ato em que o atendente já precisaria ler a informação para repassar ao cliente.

**Salvaguarda:** uma resposta sinalizada pelo atendente como totalmente equivocada ou expirada é **suspensa de circulação** até revisão do responsável pela base — nenhum outro atendente deve receber a mesma resposta incorreta enquanto ela não for corrigida ou validada.

### 2.5 Fallback

O fallback cobre dois cenários distintos:
- **(a) Ambiguidade de fronteira entre documentos válidos** — dois documentos distintos e vigentes tratam de temas adjacentes sem fronteira clara entre eles (ex.: "carga danificada" x "avaria em trânsito"). Não é conflito de versão (eliminado pela premissa 2.3), é sobreposição de escopo.
- **(b) Lacuna real de documentação** — nenhuma fonte cobre o tema perguntado. Esse caso é logado como "gap de cobertura" e aciona uma solicitação formal de criação/formalização de política junto à área responsável.

### 2.6 Métrica de desvio do fluxo principal

Mesmo não sendo a métrica-alvo do projeto (que é a redução do tempo de busca, de 12 para menos de 2 minutos por chamado, conforme o Cenário), mede-se a **taxa de chamados que saem do fluxo principal para o fallback** — sinaliza quanto do atendimento ainda depende de escalonamento humano.

### 2.7 Dimensionamento/concorrência

Volume de chamados (320/dia) e uso concorrente por 45 atendentes **não são tratados como premissa fechada nesta fase** — ficam registrados como ponto de observação para quando a jornada for efetivamente desenvolvida.

## 3. Sinalizações para o Passo 2 (Desenho dos Fluxos)

Pontos que já emergiram durante a construção das premissas e devem ser considerados no desenho dos fluxos:

- **Fluxo de manutenção da base:** precisa ser desenhado como decorrência direta do fluxo de feedback (2.4) e do fallback por lacuna real (2.5-b) — ambos acionam o responsável pela base, com naturezas diferentes (corrigir vs. criar conteúdo).
- **Candidatos a guardrail já identificados:**
  1. O assistente nunca reapresenta uma resposta/fonte já sinalizada como incorreta ou expirada sem que antes tenha passado por revisão (decorre da premissa 2.4).
  2. O assistente nunca responde com base em conhecimento geral do modelo quando a base restrita não cobre o tema — aciona o fallback de lacuna em vez de complementar com informação não documentada (decorre da premissa 2.2).

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 1.0 | 30/07/2026 | DB1-Arlindo | Criação do documento |
