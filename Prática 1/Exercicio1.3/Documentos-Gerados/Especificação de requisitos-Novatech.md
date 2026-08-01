# Especificação de Requisitos — Pipeline de RAG do Assistente de IA (NovaTech)

> **Versão:** 1.3
> **Data da versão:** 01/08/2026
> **Responsável:** DB1-Arlindo
> **Status:** Rascunho — para validação com o negócio
> **Fase do projeto:** Intent + Discovery — Passo 3 (Especificação de Requisitos do Pipeline de RAG)

## 1. Sumário executivo

Este documento especifica, em linguagem não técnica, os requisitos que o pipeline de RAG (busca e resposta fundamentada em documentação) do assistente de IA da NovaTech deve atender para gerar valor real aos atendentes. Cobre cinco eixos: quais fontes de dados indexar (e como tratar as que não devem entrar sem ressalva), como lidar com documentos contraditórios, o que fazer quando não há resposta na base, com que rapidez novos documentos precisam estar disponíveis, e o que toda resposta precisa mostrar para ser rastreável. As decisões aqui refletem o refinamento de contexto feito com o negócio e se apoiam diretamente no discovery já realizado (Exercícios 1.1 e 1.2).

**Para quem serve:** stakeholders de negócio da NovaTech (Operações, Compliance, Comercial) e o time do projeto DB1 — não pressupõe conhecimento técnico de IA para ser compreendido.

**Mensagem central deste documento:** o assistente de IA em si já representa uma referência em tecnologia conversacional e em qualidade de interação — capaz de responder em linguagem natural, com fonte citada, de forma acessível dentro do ambiente de trabalho do atendente. Mas essa tecnologia só se traduz em ganho operacional excepcional para a NovaTech quando combinada a uma base documental segura, íntegra e bem governada: um pipeline de RAG apenas recupera e apresenta o que já está na base; se a base contém versões conflitantes, lacunas ou conhecimento tácito não documentado, o assistente herda exatamente esses problemas, por melhor que seja o modelo por trás dele. Por isso, os requisitos abaixo não tratam apenas de "como buscar documentos" — tratam de que **padrão de curadoria documental** a NovaTech precisa sustentar para que a combinação tecnologia + dados confiáveis entregue o retorno esperado. Este é, na prática, tanto um projeto de tecnologia de ponta quanto um projeto de governança da informação da empresa — e é a soma dos dois que gera o ganho operacional excepcional buscado pela NovaTech.

## 2. Contexto e objetivo do negócio

A NovaTech é uma empresa de logística de médio porte (1.200 funcionários) cuja documentação interna — manuais operacionais, políticas de compliance, tabelas de SLA e regras de frete — está espalhada em três fontes: SharePoint corporativo (~800 documentos), wiki Confluence (~400 páginas) e uma pasta de rede com planilhas atualizadas mensalmente.

A equipe de atendimento ao cliente (45 pessoas, ~320 chamados/dia, dos quais ~60% envolvem consulta a documentação) gasta hoje, em média, **12 minutos por chamado** buscando informação nessas fontes — em média 4 fontes diferentes por chamado. As dúvidas mais comuns são sobre prazos de entrega (35%), regras de frete (25%) e política de devolução (20%); em **15% dos casos**, o atendente não encontra resposta e escala para o supervisor.

**Objetivo do projeto:** reduzir o tempo médio de busca de 12 para **menos de 2 minutos por chamado**, por meio de um assistente conversacional em linguagem natural, integrado ao ambiente Microsoft da NovaTech (Teams/SharePoint), que responda com base exclusivamente na documentação oficial da empresa, sempre citando a fonte.

**Objetivo deste documento:** garantir que o pipeline que sustenta esse assistente — a forma como os documentos são selecionados, atualizados, e usados para gerar respostas — seja especificado com precisão suficiente para orientar o desenvolvimento, sem entrar em detalhes de implementação técnica.

## 3. Escopo

**Dentro do escopo deste documento:**
- Requisitos sobre quais fontes de dados alimentam o assistente e como são tratadas antes e depois da indexação.
- Requisitos de comportamento diante de documentos contraditórios ou ausentes.
- Requisitos de atualização/frescor da base documental.
- Requisitos de rastreabilidade das respostas.
- Requisitos de governança documental que sustentam os itens acima.

**Fora do escopo deste documento** (tratados em outras etapas do projeto):
- Desenho detalhado da interface do assistente dentro do Teams/SharePoint — sinalizado na Premissa 2.2 (`001-Premissas.md`, Exercício 1.2) como decisão a ser tomada em etapa de desenvolvimento, não de discovery.
- Dimensionamento de infraestrutura e concorrência (volume de 320 chamados/dia, 45 atendentes simultâneos) — registrado na Premissa 2.7 como ponto de observação para a fase de desenvolvimento, não uma premissa fechada.
- Arquitetura técnica do pipeline (modelo de embeddings, banco vetorial, etc.) — este documento trata do *comportamento esperado*, não da *implementação*.
- Fluxos de interação atendente-assistente-cliente — já especificados em `002` a `005` (Exercício 1.2) e referenciados aqui, não repetidos.

## 4. Glossário

| Termo | Definição em linguagem simples |
|---|---|
| Pipeline de RAG | O processo pelo qual o assistente busca informação: os documentos são divididos em pedaços (chunks), transformados em representações numéricas (embeddings), guardados numa base de busca (banco vetorial) e recuperados por similaridade quando o atendente pergunta algo. O assistente então gera a resposta usando esses trechos recuperados como base. |
| Chunk | Um pedaço de um documento, pequeno o suficiente para ser buscado e citado com precisão. |
| Fonte normativa | Documento oficial, aprovado e sob governança de uma área responsável (ex.: POL-001, PROC-042, SLA-2024). |
| Fonte informal | Conteúdo não validado formalmente por Compliance/Operações, refletindo prática de campo (ex.: FAQ-Atendimento). |
| Indicador de confiança | Sinal exibido junto à resposta indicando o quão segura é a informação retornada (já usado no Fluxo Principal, Exercício 1.2). |
| Baixa confiabilidade | Classificação atribuída a conteúdo de fonte informal ou não formalizada — ver Seção 6. |
| Fallback | Desvio do fluxo principal quando o assistente não tem confiança suficiente na resposta (ambiguidade ou lacuna) ou quando o atendente discorda dela — detalhado em `003-Fluxo Fallback.md`. |
| Guardrail | Regra de comportamento que o assistente nunca deve violar — consolidadas em `005-Guardrails.md`. |
| Gap de cobertura documental | Registro formal de que nenhuma fonte cobre um tema perguntado. |
| Tema | Agrupamento de documentos-fonte e/ou chunks que respondem ao mesmo conjunto de perguntas de negócio (ex.: "cálculo de frete especial") — unidade usada para classificação de confiabilidade (Eixo 1) e para o gate de go-live por conflito de versão (Eixo 2). Ver nota de operacionalização na abertura da Seção 5. |
| Confiabilidade × Confiança | Dois conceitos distintos, apesar do nome parecido: **confiabilidade** classifica a fonte (normativa ou informal/baixa confiabilidade — Eixo 1); **confiança** é o indicador exibido junto a cada resposta (já usado no Fluxo Principal). Uma resposta pode ter indicador de alta confiança mesmo citando uma fonte de baixa confiabilidade, desde que isso esteja marcado — a marcação de confiabilidade da fonte não substitui o indicador de confiança da resposta. |

## 5. Requisitos por eixo

**Nota de operacionalização — definição de "tema":** Para efeito dos requisitos de gate de go-live (Eixo 2) e de classificação de confiabilidade (Eixo 1), um **tema** é definido como um agrupamento de documentos-fonte e/ou chunks que respondem ao mesmo conjunto de perguntas de negócio (ex.: "cálculo de frete especial", "política de devolução"), mapeado explicitamente pela área de negócio responsável durante o processo de classificação de fontes (Eixo 1, critério de aceite 1). Cada documento ou chunk indexado deve ser associado a um ou mais temas nesse mapeamento, de forma que seja possível auditar, para qualquer tema, quais documentos o compõem e se algum deles está em conflito não resolvido.

**Nota sobre revisão de prazos e pré-requisitos:** os prazos e pré-requisitos definidos neste documento (ex.: Eixo 1 — 60 dias corridos; Eixo 2 — pré-requisito de go-live por tema) refletem o cenário e a maturidade de governança da NovaTech na data desta especificação. Antes do início do projeto, qualquer um desses prazos ou pré-requisitos pode ser revisado, desde que: (a) o risco específico que passa a ser assumido seja formalmente identificado e documentado (ex.: risco de o assistente citar fonte conflitante antes da resolução de versão, no caso do Eixo 2); e (b) haja aceite formal e registrado pelo comitê do projeto (mesma instância de autorização e escalonamento usada em todos os mecanismos de exceção deste documento). Revisões feitas nessas condições não configuram desvio da especificação, e sim uma adequação formal dela, previamente ao início do projeto.

### 5.1 Eixo 1 — Fontes de dados a indexar

**Requisito:** todas as fontes documentais conhecidas da NovaTech (SharePoint, Confluence, planilhas de rede, e conteúdo informal como o FAQ-Atendimento) devem ser indexadas pelo pipeline. Conteúdo de fonte informal ou não validada recebe uma marcação de **baixa confiabilidade**, exibida de forma visivelmente distinta do conteúdo normativo em toda resposta que a utilize.

Conteúdo de baixa confiabilidade não é um estado permanente: todo tema nessa condição deve ter um plano de formalização junto à área de negócio responsável, com prazo de **até 60 dias corridos** (2 ciclos mensais de atualização documental) a partir da identificação. Se o prazo vencer sem formalização, o conteúdo é **indisponibilizado no sistema** — o assistente deixa de responder sobre aquele tema com base nele, passando a acionar o fallback de lacuna — e o caso é escalado formalmente ao comitê do projeto.

**Racional:** indexar apenas documentos normativos deixaria temas inteiros sem nenhuma cobertura (ex.: seguro de carga — nenhum dos 5 documentos-fonte analisados o normatiza), preservando a dependência de conhecimento tácito que é a própria motivação do projeto. Indexar tudo sem diferenciação, por outro lado, arriscaria o assistente citar uma fonte tecnicamente presente, porém desatualizada ou nunca validada, com falsa confiança — como evidenciado pelo próprio FAQ, que orienta uma regra de desconto de frete já superada pela versão vigente do procedimento.

**Critério de aceite:**
- 100% das fontes conhecidas (SharePoint, Confluence, planilhas, FAQ) passam por uma etapa de classificação (normativo vs. informal) antes da indexação.
- Toda resposta apoiada, total ou parcialmente, em conteúdo de baixa confiabilidade exibe uma marcação textual explícita e inequívoca (ex.: rótulo "fonte não validada oficialmente" ou equivalente), posicionada junto à citação da fonte — nunca apenas por uma sinalização visual isolada (cor/ícone) sem texto de apoio. O padrão visual exato é definido na etapa de desenvolvimento (Premissa 2.2), mas a presença do texto explícito é requisito mínimo desta especificação.
- Existe um registro auditável da data de identificação de cada tema de baixa confiabilidade e do prazo de 60 dias em curso.
- Nenhum tema de baixa confiabilidade permanece disponível além do prazo sem escalonamento formal.
- O prazo de 60 dias pode ser estendido, para um tema específico, mediante autorização formal do comitê do projeto — com motivo, novo prazo e responsável registrados e auditáveis. Sem essa autorização, o prazo padrão de 60 dias corridos se aplica integralmente, incluindo a indisponibilização automática em caso de vencimento.

**Riscos/dependências associados:** GAP-03 (seguro de carga sem documento normativo), GAP-04 (dependência de conhecimento tácito/pessoa-chave), GAP-07 (exceções informais da Gestão de Riscos não documentadas) — `02-hipoteses-gaps.md`, Exercício 1.1.

### 5.2 Eixo 2 — Tratamento de documentos contraditórios

**Requisito:** a resolução de conflitos de governança documental já existentes (ex.: duas versões do mesmo procedimento coexistindo sem indicação de qual vigora) é **pré-requisito de go-live por tema** — a NovaTech precisa formalizar qual versão está vigente antes de o assistente responder sobre aquele tema em produção.

Mesmo com esse pré-requisito cumprido, o Fluxo de Fallback (`003-Fluxo Fallback.md`) continuará cobrindo casos remanescentes de ambiguidade de fronteira entre documentos válidos (ex.: "carga danificada" x "avaria em trânsito") ou lacunas reais. Este documento deixa explícito que **o fallback é uma rede de segurança para exceções, não um substituto da resolução de governança na origem** — sem a formalização das fontes, o índice de chamados desviados para fallback tende a ficar artificialmente alto, mascarando o ganho real do projeto.

**Racional:** o caso concreto do PROC-042 (v1 e v2) mostra que, sem essa exigência, o próprio material de apoio ao atendimento (o FAQ) já convive de forma inconsistente com o conflito — inclusive contradizendo a si mesmo (orienta usar a versão mais recente, mas descreve uma prática de campo que segue a versão antiga). Deixar essa resolução para depois do go-live significaria automatizar a mesma ambiguidade que hoje gera respostas inconsistentes.

**Requisito complementar — detecção contínua de novas contradições:** a resolução das contradições já identificadas no discovery é condição de entrada (go-live), não uma tarefa única — o discovery cobriu uma fração mínima (bem menos de 1%) do universo documental conhecido da NovaTech (`01-mapa-temas-cobertos.md`), e novos conflitos entre documentos podem surgir à medida que o restante da base é ingerido. O pipeline deve identificar, no momento da ingestão de qualquer novo documento ou nova versão, se ele entra em conflito de conteúdo com algo já indexado para o mesmo tema (ex.: dois documentos vigentes com regras numéricas diferentes para a mesma situação).

**Salvaguarda e prazo:** ao ser detectado, um conflito pós-go-live segue o mesmo mecanismo de segurança já previsto para conteúdo sinalizado (Premissa 2.4): o conteúdo em conflito é **imediatamente suspenso de circulação** — o assistente deixa de responder com base nele, passando a acionar o fallback de lacuna para aquele tema — até que a área responsável formalize qual versão prevalece. Essa formalização deve ocorrer em até **60 dias corridos** a partir da detecção, alinhado ao mesmo prazo e mecanismo de escalonamento já definido no Eixo 1 para temas de baixa confiabilidade: se o prazo vencer sem formalização, o caso é escalado formalmente ao comitê do projeto.

**Critério de aceite:**
- Para cada tema com conflito de versão identificado no discovery (ex.: PROC-042 v1 x v2), existe confirmação formal e documentada, pela área responsável, de qual versão é a vigente, antes de o tema entrar em produção.
- A regra de "versionamento único ativo" (Premissa 2.3) é aplicada: nunca duas versões vigentes do mesmo documento coexistem na base indexada.
- Todo novo documento ou nova versão ingerida passa por uma verificação de conflito com o conteúdo já indexado do mesmo tema antes de ficar disponível para consulta.
- Todo conflito detectado após o go-live suspende automaticamente o conteúdo envolvido e aciona o fallback de lacuna para o tema afetado, com prazo de até 60 dias corridos para formalização pela área responsável e escalonamento ao comitê do projeto em caso de vencimento — sem depender de nova rodada de discovery manual.
- A taxa de chamados desviados para fallback é monitorada desde o go-live (Premissa 2.6) e reportada como indicador de saúde da governança documental, não apenas como métrica operacional.

**Riscos/dependências associados:** GAP-01 (governança de versões PROC-042, P1-Crítico), GAP-02 (uso comprovado de versão desatualizada em campo, P1-Crítico), GAP-06 (sobreposição carga danificada x avaria em trânsito, P2-Alto), GAP-08 (prazos internos x SLA formal concorrentes, P2-Alto), GAP-09 (dependência do PROC-043 em revisão, P2-Alto), GAP-10 (migração de aditivos contratuais, P3-Médio) — `02-hipoteses-gaps.md` e `03-analise-inconsistencias-proc042-v1-v2.md`, Exercício 1.1.

### 5.3 Eixo 3 — Comportamento quando não há resposta na base

**Requisito:** o assistente nunca completa uma resposta com conhecimento geral do modelo quando a base documental restrita não cobre o tema perguntado. Nesse caso, ele informa explicitamente a ausência de fonte ao atendente, aciona o fallback de lacuna (Cenário 2, `003-Fluxo Fallback.md`) e o caso é registrado como "gap de cobertura documental", disparando uma solicitação formal de criação de política junto à área responsável (`004-Fluxo Feedback.md`).

**Racional:** este é o guardrail mais fundamental do projeto — se o assistente responder com base em conhecimento geral do modelo em vez de dizer "não encontrei", ele deixa de ser um assistente fundamentado na documentação da NovaTech, e passa a introduzir um risco novo (informação plausível, mas não verificável) que não existe no processo manual atual.

**Observação sobre a premissa de saneamento da base:** este eixo parte da premissa de que a lacuna de cobertura documental é uma condição **temporária, a ser sanada já na entrada do projeto**, e não um estado com o qual o assistente deve conviver indefinidamente. Se o tratamento de "lacuna real" ficar restrito a registrar e sinalizar a ausência de fonte, sem prazo e sem cobrança de resolução, o projeto corre o risco de apenas **automatizar o problema atual** — hoje o atendente já escala 15% dos chamados por falta de resposta (Seção 2); um assistente que apenas formaliza esse mesmo escalonamento, sem pressionar a correção da causa raiz (ausência de documentação), entrega eficiência de processo sem entregar a melhoria de governança que justifica o investimento no projeto.

**Critério de aceite:**
- Toda pergunta sem cobertura documental resulta em uma resposta que informa a ausência de fonte, nunca uma resposta gerada sem base documental.
- Todo caso de lacuna é registrado como "gap de cobertura documental", com data de identificação, e segue o mesmo prazo de formalização do Eixo 1 (até 60 dias corridos) para que a área responsável publique a política ausente junto à base do assistente. Vencido o prazo sem publicação, o tema permanece em fallback de lacuna (situação já vigente, sem impacto adicional de disponibilização) e o caso é escalado formalmente ao comitê do projeto, da mesma forma que os temas de baixa confiabilidade não formalizados (Eixo 1).

**Requisito complementar — cobertura parcial:** quando uma pergunta é apenas parcialmente coberta pela base documental (parte da resposta tem fonte válida, parte não tem fonte alguma), o assistente nunca apresenta uma resposta única combinando as duas partes sem distinção. A resposta deve deixar explícito, trecho a trecho, o que tem fonte documental (com a citação correspondente) e o que não tem — tratando a parte sem fonte com o mesmo comportamento deste eixo (informar ausência de fonte e registrar como gap de cobertura), em vez de omitir a lacuna dentro de uma resposta aparentemente completa.

**Regra de precedência da marcação:** quando uma mesma resposta combina trechos de diferentes níveis de confiabilidade (fonte normativa, fonte informal de baixa confiabilidade, ou ausência de fonte), a marcação de confiabilidade exibida para a resposta como um todo é sempre a do **trecho de menor confiabilidade presente** — nunca a média nem a do trecho predominante. Isso garante que o atendente não seja induzido a confiar no conjunto da resposta pelo fato de a maior parte dela ter fonte normativa.

**Critério de aceite complementar:**
- Nenhuma resposta combina conteúdo com fonte e conteúdo sem fonte sem identificar explicitamente qual trecho corresponde a qual situação.
- Toda porção de resposta sem fonte, mesmo dentro de uma resposta majoritariamente coberta, aciona o registro de "gap de cobertura documental" para aquele sub-tema.
- A marcação de confiabilidade geral de qualquer resposta composta por trechos de níveis distintos corresponde sempre ao nível mais baixo entre os trechos combinados.

**Riscos/dependências associados:** GAP-05 (risco do assistente citar fonte conflitante ou ausente com falsa confiança, P1-Crítico) — `02-hipoteses-gaps.md`, Exercício 1.1.

### 5.4 Eixo 4 — Requisitos de atualização/frescor

**Requisito:** um documento novo ou revisado, publicado por qualquer uma das áreas responsáveis (Operações, Compliance, Comercial), deve estar disponível para consulta pelo assistente em **até 5 dias úteis** após sua publicação/atualização.

**Racional:** por ser um processo novo em implantação, esse prazo reserva uma janela de revisão humana antes da indexação automática — por exemplo, checar se o novo documento entra em conflito com algo já indexado (Eixo 2) ou se deveria ser tratado como conteúdo de baixa confiabilidade (Eixo 1) — evitando que o pipeline consuma atualizações sem controle de qualidade. O prazo é declarado como sujeito a revisão futura, podendo ser reduzido conforme o processo de governança documental amadurecer.

**Categoria de prazo urgente:** o prazo de 5 dias úteis é o padrão de início do projeto para a generalidade dos documentos. Antes do início do projeto, categorias de conteúdo com necessidade de disponibilidade mais rápida (ex.: tarifas/tabelas de frete) podem ser definidas como exceção com um **prazo urgente**, mais curto que o padrão — mantendo, porém, a mesma janela de revisão humana e a verificação automática de conflito (Eixo 2) antes da disponibilização, apenas com prazo reduzido, nunca eliminado. A duração exata do prazo urgente e a lista de categorias que o utilizam são definidas e documentadas antes do início do projeto, junto ao mesmo processo de exceção formalizada (comitê do projeto).

**Critério de aceite:**
- Existe um registro auditável do tempo decorrido entre publicação/atualização de um documento-fonte e sua disponibilidade no assistente.
- Nenhum documento leva mais de 5 dias úteis para ficar disponível, salvo exceção formalmente justificada — entendida como: dependência de validação por área externa à NovaTech (ex.: órgão regulador), indisponibilidade documentada do responsável pela base, ou necessidade de tratamento como conflito/baixa confiabilidade (Eixos 1 e 2) identificada durante a janela de revisão. Toda exceção exige autorização formal do comitê do projeto (mesma instância de escalonamento definida nos Eixos 1 a 3), com motivo, responsável e nova previsão de disponibilidade registrados, ficando sujeita à mesma auditoria do prazo padrão.
- Categorias de conteúdo definidas com prazo urgente (ex.: tarifas) são disponibilizadas dentro do prazo urgente acordado, mantendo a janela de revisão humana e a verificação de conflito do Eixo 2 — a redução é apenas de tempo, nunca de controle de qualidade.
- O prazo é revisado periodicamente (ex.: a cada trimestre) à luz da maturidade do processo de governança.

**Riscos/dependências associados:** relaciona-se ao GAP-02 (uso comprovado de versão desatualizada em campo) — um prazo de atualização mal definido perpetua esse tipo de risco mesmo após a governança de versões ser corrigida.

### 5.5 Eixo 5 — Requisitos de rastreabilidade

**Requisito:** toda resposta do assistente deve exibir, no mínimo:
1. A fonte (documento de origem).
2. O trecho relevante citado.
3. A data de última atualização/versão do documento-fonte citado.

**Racional:** os dois primeiros itens já eram guardrails confirmados no Fluxo Principal (Exercício 1.2) — nenhuma resposta é útil ou auditável sem indicar de onde veio. O terceiro item constava como candidato de backlog em `005-Guardrails.md` ("Transparência de vigência da fonte") e é promovido a requisito confirmado nesta especificação: sem a data de vigência, o atendente não tem como perceber, por conta própria, que está lendo uma resposta baseada em conteúdo potencialmente desatualizado.

**Critério de aceite:**
- 100% das respostas do assistente exibem fonte, trecho citado e data de última atualização do documento-fonte.
- Nenhuma resposta é apresentada sem essas três informações, mesmo em casos de alta confiança.

**Riscos/dependências associados:** GAP-05 (falsa confiança em fonte desatualizada ou conflitante) — a exibição da data de vigência é uma mitigação direta desse risco.

## 6. Governança documental (requisitos transversais)

Os eixos acima dependem de um conjunto comum de regras de governança, já confirmadas no refinamento desta etapa e no Exercício 1.2:

- **Versionamento único ativo** (Premissa 2.3): para subir uma nova versão de um documento, a versão anterior é obrigatoriamente baixada/desativada — nunca duas versões vigentes coexistindo.
- **Responsável pela base:** existe um dono/responsável pela base documental, acionável para correção ou criação de conteúdo ausente.
- **Pré-requisito de go-live por tema** (Eixo 2): conflitos de versão já existentes devem ser resolvidos antes de o assistente responder sobre aquele tema em produção.
- **Prazo de formalização de conteúdo de baixa confiabilidade** (Eixo 1): 60 dias corridos, com indisponibilização automática e escalonamento formal em caso de descumprimento.
- **Prazo de atualização/frescor** (Eixo 4): até 5 dias úteis entre publicação e disponibilidade no assistente.
- **Suspensão por sinalização do atendente** (Premissa 2.4, `004-Fluxo Feedback.md`): uma resposta sinalizada como equivocada ou expirada é suspensa de circulação até revisão do responsável pela base — nenhum outro atendente recebe a mesma resposta incorreta enquanto ela não for corrigida.

> **Nota:** as figuras de "Responsável pela Base" e "Comitê do Projeto", referenciadas ao longo deste documento, ainda não correspondem a papéis formalmente atribuídos na estrutura da NovaTech — sua definição depende da revisão da estrutura operacional a ser conduzida durante o início do projeto, e é pré-requisito para o funcionamento efetivo dos mecanismos de prazo e escalonamento aqui descritos.

## 7. Premissas do projeto

Lista completa das premissas definidas em `001-Premissas.md` (Exercício 1.2), válidas por padrão para esta especificação:

- **2.1 Canal de contato do cliente** — o atendimento é tratado a partir do chamado já recebido pelo atendente, independentemente do canal de entrada do cliente.
- **2.2 Interação atendente-IA** — interação em linguagem natural, com base de conhecimento restrita à documentação da NovaTech.
- **2.3 Base documental** — base unificada, padronizada e revisada, com versionamento único ativo.
- **2.4 Validação da resposta pelo atendente** — validação implícita na leitura da resposta com fonte citada, sem gate formal de aprovação; resposta sinalizada como errada/expirada é suspensa até revisão.
- **2.5 Fallback** — cobre ambiguidade de fronteira entre documentos válidos e lacuna real de documentação.
- **2.6 Métrica de desvio do fluxo principal** — mede-se a taxa de chamados que saem do fluxo principal para o fallback.
- **2.7 Dimensionamento/concorrência** — não tratado como premissa fechada nesta fase; ponto de observação para a fase de desenvolvimento.

## 8. Riscos e dependências

Riscos priorizados do discovery (`02-hipoteses-gaps.md`, Exercício 1.1) diretamente relevantes ao pipeline de RAG, com sua classificação de risco:

| ID | Risco | Classificação | Eixo relacionado |
|---|---|---|---|
| GAP-01 | Governança de versões PROC-042 (v1 x v2 coexistindo) | P1-Crítico | Eixo 2 |
| GAP-02 | Uso comprovado de versão desatualizada em campo | P1-Crítico | Eixo 2, Eixo 4 |
| GAP-03 | Seguro de carga sem nenhum documento normativo | P1-Crítico | Eixo 1 |
| GAP-04 | Dependência de conhecimento tácito/pessoa-chave | P1-Crítico | Eixo 1 |
| GAP-05 | Risco do assistente citar fonte conflitante com falsa confiança | P1-Crítico | Eixo 3, Eixo 5 |
| GAP-06 | Sobreposição "carga danificada" x "avaria em trânsito" | P2-Alto | Eixo 2 |
| GAP-07 | Exceções informais da Gestão de Riscos não documentadas | P2-Alto | Eixo 1, Eixo 2 |
| GAP-08 | Falta de reconciliação entre prazos internos e SLA formal | P2-Alto | Eixo 2 |
| GAP-09 | Dependência do PROC-043 (em revisão pelo Compliance) | P2-Alto | Eixo 2 |
| GAP-10 | Migração de aditivos contratuais de desconto (v1 → v2) | P3-Médio | Eixo 2 |

Nenhum desses riscos é resolvido por este documento — eles são a razão de ser dos requisitos acima. A resolução de negócio (GAP-01, GAP-03, GAP-07, GAP-09 em especial) continua sendo pré-condição para que os eixos 1 e 2 sejam efetivamente cumpridos.

## 9. Métricas de sucesso

- **Tempo médio de busca por chamado:** meta do projeto de reduzir de 12 minutos para menos de 2 minutos (Cenário) — métrica-alvo principal, não específica deste pipeline, mas o objetivo que todos os requisitos acima servem.
- **Taxa de desvio para fallback** (Premissa 2.6): monitorada desde o go-live como indicador de saúde da governança documental; não há meta numérica fechada nesta fase — fica registrada como métrica de acompanhamento.
- **Temas de baixa confiabilidade pendentes de formalização:** acompanhado por tema individual, com prazo de 60 dias corridos cada (Eixo 1); não há meta agregada nesta fase, apenas o cumprimento do prazo por tema.
- **Exposição a risco de compliance por resposta desatualizada ou conflitante:** acompanhada por meio de duas métricas indiretas: (i) número de temas com conflito de versão ou baixa confiabilidade em aberto além do prazo formal (Eixos 1 e 2); (ii) número de respostas suspensas por sinalização do atendente (Premissa 2.4) que se confirmaram como incorretas ou desatualizadas. A tendência esperada é de queda contínua após o go-live, na medida em que a governança documental amadurece — funcionando como indicador de que o projeto está reduzindo, e não apenas automatizando, o risco de informação divergente já hoje presente no processo manual (ex.: uso comprovado da versão desatualizada do PROC-042 em campo).

## 10. Perguntas em aberto / próximos passos

- Resolução de negócio dos riscos P1-Crítico (Seção 8) junto às áreas responsáveis (Compliance, Comercial, Operações), pré-requisito para o go-live dos temas afetados.
- Desenho da interface exata do assistente dentro do Teams/SharePoint (fora do escopo deste documento — Premissa 2.2).
- Dimensionamento de infraestrutura para o volume de 320 chamados/dia e 45 atendentes concorrentes (fora do escopo deste documento — Premissa 2.7).
- Revisão do prazo de atualização/frescor (5 dias úteis) após maturidade do processo de governança documental.
- Definição formal de quem ocupa os papéis de "Responsável pela Base" e "Comitê do Projeto" na estrutura da NovaTech, como parte da revisão da estrutura operacional no início do projeto — pré-requisito para o funcionamento dos mecanismos de prazo, exceção e escalonamento descritos neste documento (Seção 6).

## 11. Anexos e referências

- Resumo do Cenário (`Resumos-Discovery/resumo-Cenário`).
- Contexto da etapa (`Resumos-Discovery/Contexto-1.3.md`).
- Dados de discovery (`Resumos-Discovery/Dados-Discovery.md`).
- Anexo A — Documentação simulada da NovaTech (`Especificações-Exercicio/anexo-a-documentacao-simulada-novatech.md`).
- Resumos individuais RES-001 a RES-005 (`Resumos-Discovery/`).
- Exercício 1.1 — `01-mapa-temas-cobertos.md`, `02-hipoteses-gaps.md`, `03-analise-inconsistencias-proc042-v1-v2.md`, `04-FAQ-Inconsistencias-praticas-informais.md`.
- Exercício 1.2 — `001-Premissas.md`, `002-Fluxo Principal.md`, `003-Fluxo Fallback.md`, `004-Fluxo Feedback.md`, `005-Guardrails.md`.
- `001-Base de construção.md` (Exercício 1.3) — registro das decisões de refinamento que originaram este documento.

---

## 12. Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 1.0 | 01/08/2026 | DB1-Arlindo | Criação do documento — especificação de requisitos do pipeline de RAG, cobrindo os 5 eixos definidos em `Contexto-1.3.md` |
| 1.1 | 01/08/2026 | DB1-Arlindo | Rodada de revisão de qualidade (painel de 5 perspectivas): (1) definida a granularidade operacional de "tema" para os Eixos 1 e 2; (2) incluído requisito de detecção contínua de novas contradições pós-go-live, com suspensão automática, prazo de 60 dias e escalonamento ao comitê; (3) alinhado o tratamento de "lacuna real" (Eixo 3) ao mesmo prazo/escalonamento de "baixa confiabilidade" (Eixo 1), com observação sobre o risco de apenas automatizar o processo atual; (4) reforçada a mensagem central do Sumário Executivo sobre governança de dados como fator primário de sucesso, combinada à tecnologia/interação do assistente; (5) definido critério objetivo para exceções ao prazo de frescor (Eixo 4), com autorização do comitê do projeto; (6) incluída métrica de exposição a risco de compliance (Seção 9); (7) definido padrão mínimo verificável para a marcação de baixa confiabilidade (Eixo 1); (8) incluído requisito de tratamento de cobertura parcial e regra de precedência da marcação pelo trecho de menor confiabilidade (Eixo 3); (9) incluídos os termos "Tema" e "Confiabilidade × Confiança" no Glossário. |
| 1.2 | 01/08/2026 | DB1-Arlindo | Ajustes de flexibilização negociados com a perspectiva do stakeholder NovaTech, mantendo as salvaguardas de risco: (1) prazos e pré-requisitos dos Eixos 1 e 2 declarados como baseline revisável antes do início do projeto, mediante identificação formal do risco assumido e aceite por autoridade competente; (2) prazo de 60 dias do Eixo 1 passa a admitir extensão por tema mediante autorização do comitê do projeto; (3) Eixo 4 passa a admitir categoria de "prazo urgente" para conteúdo crítico (ex.: tarifas), definida antes do início do projeto, mantendo obrigatoriamente a janela de revisão humana e a verificação de conflito do Eixo 2; (4) incluída nota reconhecendo que os papéis de "Responsável pela Base" e "Comitê do Projeto" ainda não estão formalmente atribuídos na estrutura da NovaTech, a serem definidos no início do projeto. |
| 1.3 | 01/08/2026 | DB1-Arlindo | Correção de integridade identificada em nova rodada de revisão pelo painel de 5 perspectivas: (1) substituído o termo indefinido "autoridade competente da NovaTech" (nota da Seção 5) por "comitê do projeto", alinhando com a única instância de autorização/escalonamento usada em todo o restante do documento; (2) incluído na Seção 10 o item, previamente prometido e não aplicado, sobre a definição formal dos papéis de "Responsável pela Base" e "Comitê do Projeto" como pré-requisito de execução. |
