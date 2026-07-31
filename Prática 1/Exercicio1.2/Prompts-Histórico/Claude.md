# Claude — Histórico da Sessão de Trabalho

> **Versão:** 1.0
> **Data da versão:** 31/07/2026
> **Responsável:** DB1-Arlindo
> **Status:** Registro de sessão
> **Fase do projeto:** Intent + Discovery — Passos 1, 2 e 3

## 1. Objetivo da sessão

Atuação como Product Specialist na fase de Intent + Discovery do projeto NovaTech (assistente de IA para atendimento ao cliente). Missão: mapear a jornada do atendente usando o assistente de IA, em três passos combinados no início da sessão:

1. Refinar o contexto até termos as premissas da jornada.
2. Definir como seriam os fluxos a serem desenhados.
3. Gerar os prompts para geração visual dos fluxos.

## 2. Material de discovery utilizado

- Resumo do Cenário (NovaTech, setor de logística, 1.200 funcionários, 45 atendentes, 320 chamados/dia, 60% envolvendo consulta a documentação, meta de reduzir o tempo de busca de 12 para menos de 2 minutos por chamado).
- `Dados-Discovery.md` (4 fontes abertas em média por chamado, distribuição de dúvidas por tema, 15% dos casos escalados por falta de resposta).
- Os 4 documentos gerados na etapa anterior do trabalho (Exercício 1.1): mapa de temas cobertos, hipóteses de gaps, análise de inconsistências PROC-042 v1 x v2, e cruzamento FAQ x documentação formal.

**Nota metodológica:** durante a sessão, o conteúdo bruto dos resumos individuais de POL-001, PROC-042 v1/v2, SLA-2024 e FAQ-Atendimento foi deliberadamente desconsiderado, por não terem sido explicitamente indicados pelo usuário como material de trabalho — apenas os arquivos citados acima e os documentos gerados no Passo 1/2 desta sessão foram usados como fonte.

## 3. Passo 1 — Premissas

Processo: o usuário propôs 3 premissas iniciais (canais de contato do cliente, interação atendente-IA, base da IA); a partir daí, premissas adicionais foram sugeridas e refinadas em conjunto até fechar em 7, mantendo o escopo enxuto por se tratar de uma fase inicial ("não vamos esgotar as análises").

**Decisões de destaque:**

- Interação atendente-IA definida como uma versão do Claude/Anthropic, em linguagem natural, com **base de conhecimento restrita** à documentação da NovaTech.
- Validação da resposta pelo atendente **sem gate formal** — implícita no próprio ato de ler a resposta com a fonte citada — com uma salvaguarda: resposta sinalizada como errada/expirada é **suspensa de circulação** até revisão.
- Base documental com regra de **versionamento único ativo** (nunca duas versões vigentes do mesmo documento coexistindo), eliminando estruturalmente o tipo de conflito identificado no discovery entre PROC-042 v1 e v2.
- Fallback definido para cobrir **ambiguidade de fronteira entre documentos válidos** e **lacuna real de documentação** — não mais conflito de versão, já eliminado pela premissa de governança da base.
- Métrica de desvio do fluxo principal (taxa de saída para fallback) registrada mesmo não sendo a métrica-alvo do projeto.
- Dimensionamento/concorrência tratado como ponto de observação para a fase de desenvolvimento, não como premissa fechada.

**Arquivo de referência:** `001-Premissas.md`

## 4. Passo 2 — Fluxos

**Decisão de formato:** texto estruturado combinando **Given-When-Then (BDD)** como "cenário-resumo" (visão executiva rápida) com **SOP textual** como "detalhamento" (passo a passo operacional). O jargão técnico do BPMN (raias, gateway, evento) foi testado no texto e descartado por ficar pesado de ler — a lógica BPMN foi mantida, só não nomeada explicitamente na prosa. Um documento por fluxo, cada um contendo as 7 premissas completas (padronização adotada após o usuário notar que a primeira versão só citava as premissas diretamente aplicáveis).

### Fluxo Principal (`002-Fluxo Principal.md`)
- Cobre apenas o caminho feliz, sem menção a desvios.
- Passo explícito de "o atendente confia na resposta lida" como o próprio momento de validação.
- Encerramento: a interação com o cliente pode continuar (nova dúvida, reinicia o fluxo) ou se encerrar.

### Fluxo de Fallback (`003-Fluxo Fallback.md`)
- Três gatilhos de desvio, sempre na mesma ordem e nomenclatura: (a) ambiguidade entre documentos válidos, (b) lacuna real de documentação, (c) atendente discorda da resposta.
- Supervisor/Área Especializada resolve por **julgamento humano**, apoiado por **documentação auxiliar** e dentro da sua **alçada de resolução**.
- Resposta suspensa em caso de discordância, até revisão do Responsável pela Base.
- O tratamento de conteúdo por gatilho ocorre **em paralelo** à resolução do atendimento — essa explicitação foi perdida numa reorganização de passos e precisou ser restaurada depois de identificada durante a montagem do prompt de diagrama.
- Guardrail formalizado nesta etapa: o assistente nunca decide sozinho qual fonte prevalece diante de qualquer sinal de conflito ou ambiguidade — ampliado (Opção B) para cobrir não só ambiguidade de fronteira entre documentos, mas também eventual falha de governança de versão.

### Fluxo de Feedback (`004-Fluxo Feedback.md`)
- Mantém a mesma ordem e nomenclatura dos três gatilhos do Fluxo de Fallback, para rastreabilidade entre os documentos.
- Introduz o ator **Time Técnico do Assistente**, para os casos em que a discordância do atendente é causada por comportamento do assistente (ex.: interpretação incorreta de uma fonte correta), e não por um problema de conteúdo da base.
- Fecha o ciclo: a correção de conteúdo ou a liberação do ajuste técnico encerra a suspensão da resposta.

### Guardrails (`005-Guardrails.md`)
- 3 guardrails confirmados, cada um com origem (fluxo) e risco de discovery associado — destaque para o GAP-05 (P1-Crítico), risco do assistente citar uma fonte tecnicamente correta, porém conflitante, com falsa confiança.
- Backlog de 5 guardrails candidatos para análise futura, todos ancorados em gaps específicos do discovery: transparência de vigência da fonte, não combinar regras de fontes diferentes sem sinalizar, não generalizar regra restrita a um segmento, sinalizar dependência de conhecimento tácito, e nunca minimizar tema sensível/crítico.

## 5. Passo 3 — Prompts para geração visual

- **Notação escolhida:** BPMN — mantida a preferência do usuário, e considerada adequada ao caso (múltiplos atores, gateways de decisão, handoffs entre papéis), mesmo tendo sido abandonada como jargão no texto estruturado.
- **Ferramenta-alvo:** Claude Design (produto beta). Os prompts foram escritos de forma portátil, em linguagem natural, compatíveis também com o Claude padrão via instrução condicional de saída em Mermaid.
- **Guardrails no diagrama:** representados como *Text Annotations* — o elemento nativo do BPMN para anotar regras/restrições, ligado por linha pontilhada ao ponto exato onde a regra se aplica.
- **Erros encontrados e corrigidos durante os testes reais no Claude Design:**
  - Fluxo de Fallback: a raia do Cliente ficou sem atividade própria no primeiro prompt — corrigido com adição de atividades de recepção via fluxo de mensagem.
  - Fluxo de Fallback: a convergência dos três gatilhos (A/B/C) em uma atividade genérica fazia perder a rastreabilidade de qual gatilho originou o caso — corrigido mantendo cada caminho com identidade própria até a diferenciação deixar de ser necessária (gateway paralelo com convergência só após a ação específica de cada caminho).
  - Fluxo de Fallback: o evento de início foi colocado errado (diretamente na raia do Atendente, sem o Cliente trazendo a dúvida antes) — corrigido com um adendo pontual, sem regenerar o diagrama inteiro no Claude Design.
- Prompts finais dos 3 fluxos e os adendos de correção estão registrados em `claude design.md` (nesta mesma pasta).

## 6. Biblioteca final de arquivos da sessão

**`Documentos-Gerados/`:**
- `001-Premissas.md`
- `002-Fluxo Principal.md` + `Fluxo-002-Fluxo Principal.jpg`
- `003-Fluxo Fallback.md` + `Fluxo-003-Fluxo Fallback.jpg`
- `004-Fluxo Feedback.md` + `Fluxo-004-Fluxo Feedback.jpg`
- `005-Guardrails.md`

**`Prompts-Histórico/`:**
- `claude design.md`
- `Claude.md` (este documento)

## 7. Decisões metodológicas de destaque (aprendizados da sessão)

- BDD (Given-When-Then) como "resumo executivo" do cenário + SOP textual como detalhamento operacional — combinação sugerida pelo usuário para equilibrar clareza e profundidade sem depender de jargão técnico.
- BPMN mantido como notação de diagrama, mesmo tendo sido descartado como vocabulário no texto estruturado — a lógica (raias, gateways, eventos) sempre esteve por trás da redação, só não nomeada.
- Padronização das 7 premissas completas em todo documento de fluxo, para evitar lacunas de contexto entre documentos — ajuste feito após o usuário notar inconsistência no primeiro arquivo gerado.
- Escopo mantido deliberadamente enxuto ("não vamos esgotar as análises") em cada fase, coerente com uma fase inicial de discovery.
- Testar os prompts na ferramenta real (Claude Design) revelou problemas de modelagem (perda de rastreabilidade de caminhos, raia sem conteúdo, evento de início mal posicionado) que não eram visíveis apenas na leitura do texto estruturado — validação prática se mostrou necessária antes de considerar os prompts finais.

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 1.0 | 31/07/2026 | DB1-Arlindo | Criação do documento — registro histórico completo da sessão de trabalho (Passos 1, 2 e 3) |
