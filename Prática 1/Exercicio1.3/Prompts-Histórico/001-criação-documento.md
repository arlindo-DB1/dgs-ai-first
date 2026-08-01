# Histórico de Sessão 001 — Criação da Especificação de Requisitos (Fase Intent + Discovery, Passo 3)

> **Versão:** 1.0
> **Data da versão:** 01/08/2026
> **Responsável:** DB1-Arlindo
> **Status:** Registro de sessão
> **Fase do projeto:** Intent + Discovery — Passo 3 (Especificação de Requisitos do Pipeline de RAG)
> **Participante:** arlindo.junior@db1.com.br

## 1. Objetivo da sessão

Atuação como Product Specialist na fase de Intent + Discovery do projeto NovaTech (assistente de IA para atendimento ao cliente). Missão desta sessão: refinar o contexto e escrever uma especificação de requisitos de produto — não técnica, porém precisa — cobrindo os requisitos que o pipeline de RAG deve atender, e, num segundo momento, preparar o prompt de revisão que será usado numa próxima sessão.

## 2. Acordo inicial de trabalho

No início da sessão, foi combinado explicitamente o modo de colaboração:

- Claude atuaria restrito aos documentos explicitamente fornecidos pelo usuário, sem ler nada além disso por conta própria.
- Antes de qualquer redação, o contexto seria refinado em conjunto até alinhamento completo.
- Claude deveria sempre perguntar quando tivesse dúvida ou precisasse de informação complementar.
- O usuário reforçou, ao longo da sessão, que as perguntas de refinamento só deveriam começar após o aviso explícito de que todos os documentos haviam sido enviados — pedido feito depois que Claude tentou perguntar cedo demais, antes do envio completo do material.

## 3. Material de discovery recebido nesta sessão

Documentos fornecidos pelo usuário, na ordem em que foram enviados:

1. Resumo do Cenário — `Resumos-Discovery/resumo-Cenário`
2. Contexto da etapa 1.3 — `Resumos-Discovery/Contexto-1.3.md`
3. Dados do discovery simulado — `Resumos-Discovery/Dados-Discovery.md`
4. Anexo A — documentação simulada completa da NovaTech, incluindo a seção de contradições e gaps identificados — `Especificações-Exercicio/anexo-a-documentacao-simulada-novatech.md`
5. Todos os documentos gerados no Exercício 1.1 (`Exercicio1.1/Documentos-Gerados/`): mapa de temas cobertos, hipóteses de gaps, análise de inconsistências PROC-042 v1 x v2, cruzamento FAQ x documentação formal, e o registro de respostas às tarefas do exercício.
6. Todos os documentos gerados no Exercício 1.2 (`Exercicio1.2/Documentos-Gerados/`): Premissas, Fluxo Principal, Fluxo de Fallback, Fluxo de Feedback e Guardrails.
7. Os resumos individuais das 5 fontes originais (`Resumos-Discovery/resumo-*.md`): POL-001, PROC-042 v1, PROC-042 v2, SLA-2024 e FAQ-Atendimento.

## 4. Missão completa da etapa

O usuário definiu a missão em 5 eixos, a serem cobertos pela especificação:

1. Quais fontes de dados devem ser indexadas (e quais não — documentos obsoletos devem ser excluídos ou marcados?).
2. Como o assistente deve lidar com documentos contraditórios (ex.: duas versões do mesmo procedimento).
3. Qual o comportamento esperado quando a pergunta do atendente não tem resposta na base (dizer "não encontrei" ou tentar responder com conhecimento geral?).
4. Requisitos de atualização: em quanto tempo um documento novo deve estar disponível no assistente?
5. Requisitos de rastreabilidade: toda resposta deve citar a fonte? Deve mostrar o trecho relevante?

## 5. Rodada de perguntas de refinamento

Após o aviso de que todos os documentos haviam sido enviados, Claude fez 4 perguntas de múltipla escolha para fechar as decisões pendentes antes da redação:

### 5.1 Fontes de dados a indexar (Eixo 1)

O usuário não escolheu diretamente uma das opções apresentadas — pediu uma análise de prós e contras:

> "Quero que apresente os prós e contras de usarmos todas as fontes, sem excessão e também os prós e contras de usarmos só os documentos normativos (POL/PROC/SLA), e também me sugira qual devemos utilizar neste documento, e o porquê. E vamos utilizar todo este bloco no documento."

Claude apresentou a análise comparativa e recomendou uma via híbrida (indexar tudo, com conteúdo informal/não normativo marcado como baixa confiabilidade). O usuário aprovou, com uma condição adicional:

> "Gostei da via hibrida, mas faltou colocarmos que é necessário que a base seja tratada, e tudo que seja de confiabilidade baixa seja substituido e formalizado. [...] precisamos incluir um prazo factivel para que isto ocorra, se não o projeto corre o risco de se apenas uma automação do cenário atual."

Claude propôs 60 dias corridos (2 ciclos mensais de atualização documental) como prazo de formalização. Numa rodada seguinte, o usuário confirmou o prazo e adicionou o mecanismo de consequência:

> "Gostei da proposta de 60 dias, e eu colocaria como sugestão que ao escalar o conteudo será indisponibilizado no sistema."

**Decisão final:** via híbrida de fontes + prazo de 60 dias para formalização + indisponibilização automática do conteúdo de baixa confiabilidade em caso de descumprimento do prazo, com escalonamento formal ao comitê do projeto.

### 5.2 Tratamento de documentos contraditórios (Eixo 2)

**Decisão do usuário:** a resolução de conflitos de governança já existentes (ex.: PROC-042 v1 x v2) é pré-requisito de go-live por tema — não apenas algo coberto pelo Fluxo de Fallback já desenhado no Exercício 1.2. O usuário acrescentou o racional a ser explicitado no documento:

> "Precisamos colocar que o pré-requisito de go-live por tema é necessário, pois mesmo com ele, teremos ainda cenários que o cenário de fallback irá cobrir. [...] sem formalizar as versões regularizar a fonte de dados, geraríamos um indice enorme de fallback (respostas fora do fluxo principal)."

### 5.3 Requisito de atualização/frescor (Eixo 4)

**Decisão do usuário:** usar a faixa de "poucos dias" como base, justificando como um processo novo que precisa de janela de revisão antes do sistema consumir automaticamente. Claude propôs um número concreto — até 5 dias úteis — e o usuário confirmou: "Gostei da sua sugestão, vamos utiliza-la".

### 5.4 Requisitos de rastreabilidade (Eixo 5)

**Decisão do usuário:** promover a exibição da data de última atualização do documento-fonte, que constava apenas como candidato de backlog em `005-Guardrails.md` (Exercício 1.2), a requisito confirmado desta especificação — em conjunto com a citação de fonte e trecho relevante, já confirmados anteriormente.

## 6. Documento de referência viva — `001-Base de construção.md`

A pedido do usuário, as decisões acima foram consolidadas num arquivo de referência versionado, no mesmo padrão dos documentos das etapas anteriores, para evitar retrabalho ou contradição entre rodadas de refinamento:

> "Como esta é uma definição que precisaremos utilizar mais vezes na nossa conversa, seria util salvarmos em um arquivo de consulta para ti? [...] Pode chamar ele de 001-Base de construção [...] Gostaria que esse documento fosse controlado por versionamento."

O arquivo foi criado na versão 1.0 com as decisões dos eixos 1, 2, 4 e 5, e revisado para a versão 1.1 quando o mecanismo de indisponibilização do eixo 1 foi confirmado.

## 7. Estrutura do documento final de especificação

Antes de escrever a especificação, o usuário pediu que Claude sugerisse uma estrutura prática, alinhada a padrões de mercado usados por consultorias de referência. Claude propôs 12 seções (sumário executivo, contexto e objetivo, escopo, glossário, requisitos por eixo com racional/critério de aceite/riscos, governança documental transversal, premissas, riscos e dependências, métricas de sucesso, perguntas em aberto, anexos e referências, histórico de revisões) — aprovada sem ajustes.

## 8. Criação da especificação — `Especificação de requisitos-Novatech.md`

Antes de escrever, Claude confirmou que tinha material suficiente, sinalizando duas derivações editoriais (escopo fora do documento e métricas sem meta numérica fechada) para o usuário poder contestar caso discordasse. O documento foi então escrito na íntegra, seguindo a estrutura de 12 seções aprovada, incorporando todas as decisões das seções 5 e 6 deste histórico, e citando explicitamente os GAPs do Exercício 1.1 relevantes a cada eixo. Feedback do usuário: "Boa, gostei das suas obervações."

## 9. Segundo passo — Prompt de revisão para nova sessão

O usuário definiu o próximo passo do projeto: preparar um prompt para rodar em uma nova sessão, revisando a especificação recém-criada. Requisitos explícitos do prompt:

- Apresentar pontos, sugestões de ajuste, pontos a excluir e pontos a incluir (revisão completa).
- Identificar gaps ou ambiguidades na especificação.
- Verificar que os requisitos são testáveis (um QA conseguiria verificar cada um).
- Tratar com atenção especial os eixos de contradição de documentos e ausência de resposta.
- Garantir que a especificação deixe claro que a qualidade do produto depende da curadoria dos dados, não só da tecnologia.
- Revisão sob a ótica de múltiplos profissionais (qualidade, técnica, linguagem, conhecimento), visando demonstrar o quanto o projeto é estratégico para a NovaTech.

Claude escreveu o prompt cobrindo todos os pontos, com 5 perspectivas profissionais (Engenharia de Requisitos/QA, Governança de Dados, Arquitetura de Soluções RAG, Consultoria de Negócios/PM, Editoria técnica de linguagem) e um formato de saída estruturado por achado.

Antes de salvar, o usuário pediu um ajuste importante no formato de condução da revisão:

> "quero que antes, inclua na seção do 'FORMATO DE SAÍDA [...]', que vá me perguntando e eu já vou decidindo o que será feito, para ele mesmo já efetuar a atualização no documento e versionar."

Claude reescreveu essa seção como um processo interativo: apresentar um achado por vez (ordenado por severidade), perguntar a decisão do usuário antes de avançar, aplicar a edição diretamente no arquivo assim que decidida, repetir até esgotar os achados, e só então incrementar a versão do documento com uma entrada consolidada no histórico de revisões — encerrando com resumo executivo e lista de achados pendentes.

O prompt final foi salvo em `Prompts-Histórico/prompt-revisão.md`.

## 10. Biblioteca final de arquivos da sessão

**`Documentos-Gerados/`:**
- `001-Base de construção.md` (v1.1)
- `Especificação de requisitos-Novatech.md` (v1.0)

**`Prompts-Histórico/`:**
- `prompt-revisão.md`
- `001-criação-documento.md` (este documento)

## 11. Decisões metodológicas de destaque (aprendizados da sessão)

- O usuário reforçou explicitamente, mais de uma vez, que perguntas de refinamento só deveriam ocorrer após o aviso de que todo o material havia sido enviado — evitando ciclos de pergunta prematura sobre contexto ainda incompleto.
- Decisões de trade-off relevantes (ex.: fontes de dados) foram tratadas com pedido explícito de análise de prós/contras + recomendação justificada, não apenas uma escolha direta em pergunta de múltipla escolha — e o próprio bloco de análise foi reaproveitado no documento final.
- Criação de um documento de referência viva e versionado (`001-Base de construção.md`) para registrar decisões incrementais de contexto antes da redação final — mesmo padrão já usado para as Premissas no Exercício 1.2, reaplicado aqui para a etapa de especificação de requisitos.
- Separação clara entre "refinar contexto" e "escrever o documento": o usuário pediu explicitamente para não avançar para a redação até confirmar que a estrutura e o entendimento do contexto estavam completos.
- O prompt de revisão para a próxima sessão foi desenhado como um processo interativo achado-a-achado com aplicação e versionamento imediatos, em vez de um relatório estático — decisão tomada explicitamente para acelerar a curadoria humana das mudanças propostas.

## 12. Encerramento da sessão

O usuário encerrou esta sessão informando que o próximo passo do projeto será conduzido em uma nova sessão — a execução do `prompt-revisão.md` sobre a `Especificação de requisitos-Novatech.md` — e solicitou o registro deste histórico como último passo antes do fechamento.

## 13. Pendências para a próxima sessão

- Executar o `prompt-revisão.md` sobre `Especificação de requisitos-Novatech.md`, em processo interativo achado-a-achado, aplicando e versionando as mudanças aprovadas.
- Resolução de negócio dos riscos P1-Crítico já mapeados (GAP-01, GAP-03, GAP-04, GAP-05), pré-requisito de go-live para os temas afetados, permanece pendente fora do escopo desta sessão.

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 1.0 | 01/08/2026 | DB1-Arlindo | Criação do documento — registro histórico completo da sessão de criação da Especificação de Requisitos (Passo 3) |
