# Fluxo Principal — Consulta do Atendente ao Assistente de IA

> **Versão:** 1.1
> **Data da versão:** 30/07/2026
> **Responsável:** DB1-Arlindo
> **Status:** Aprovado
> **Fase do projeto:** Intent + Discovery — Passo 2 (Definição dos Fluxos)

## 1. Cenário

Fluxo principal — caminho feliz: tudo ocorre conforme o esperado, sem desvios para fallback.

## 2. Atores envolvidos neste fluxo

- **Atendente** — colaborador da NovaTech responsável por atender o cliente durante o chamado, consultando o assistente de IA para embasar suas respostas.
- **Assistente IA** — assistente conversacional em linguagem natural, com acesso restrito à base documental unificada da NovaTech, responsável por buscar informação e responder sempre com a fonte citada.
- **Cliente** — pessoa que contata a NovaTech com uma dúvida relacionada ao atendimento (prazos, frete, devolução, entre outros).

## 3. Cenário-resumo (Given-When-Then)

Dado que o atendente recebeu uma dúvida do cliente durante o chamado,

Quando ele pergunta ao assistente e existe uma fonte confiável e sem ambiguidade na base documental,

Então o assistente responde com a informação, o trecho-fonte citado e o indicador de confiança — e o atendente confirma que confia na resposta e a usa no atendimento ao cliente.

## 4. Detalhamento (SOP textual)

**Como começa:** o atendente recebe uma dúvida do cliente durante o chamado.

**Passo a passo:**

1. O atendente formula a pergunta ao assistente, em linguagem natural.
2. O assistente busca a resposta exclusivamente na base documental restrita.
3. O assistente encontra uma fonte confiável e sem ambiguidade.
4. O assistente retorna ao atendente: a resposta, o trecho-fonte citado e o indicador de confiança.
5. O atendente lê a resposta e a fonte citada.
6. **O atendente confia na resposta lida** — esse é o momento de validação. Não existe uma etapa formal separada de aprovação; a confiança é o que autoriza o atendente a seguir para o próximo passo.
7. O atendente usa a resposta para responder ao cliente no atendimento.
8. A interação com o cliente pode **continuar** (surge uma nova dúvida, e o fluxo reinicia a partir do passo 1) ou **se encerrar** (não há mais dúvidas, o chamado é concluído).

**Guardrails aplicados aqui:**

- O assistente nunca completa uma resposta com conhecimento geral do modelo quando a base restrita não cobre o tema.
- O assistente nunca reapresenta uma resposta/fonte já sinalizada como incorreta ou expirada sem revisão prévia.

## 5. Premissas do projeto

Lista completa das premissas definidas em `001-Premissas.md`, mantida por padrão em todos os documentos de fluxo:

- **2.1 Canal de contato do cliente** — o atendimento é tratado a partir do chamado já recebido pelo atendente, independentemente do canal de entrada do cliente.
- **2.2 Interação atendente-IA** — interação em linguagem natural, com base de conhecimento restrita à documentação da NovaTech.
- **2.3 Base documental** — base unificada, padronizada e revisada, com versionamento único ativo (nunca duas versões vigentes de um mesmo documento).
- **2.4 Validação da resposta pelo atendente** — validação implícita na leitura da resposta com fonte citada, sem gate formal de aprovação; resposta sinalizada como errada/expirada é suspensa até revisão.
- **2.5 Fallback** — cobre ambiguidade de fronteira entre documentos válidos e lacuna real de documentação.
- **2.6 Métrica de desvio do fluxo principal** — mede-se a taxa de chamados que saem do fluxo principal para o fallback.
- **2.7 Dimensionamento/concorrência** — não tratado como premissa fechada nesta fase; ponto de observação para a fase de desenvolvimento.

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 1.0 | 30/07/2026 | DB1-Arlindo | Criação do documento — versão aprovada do Fluxo Principal |
| 1.1 | 30/07/2026 | DB1-Arlindo | Padronização da seção 5 — passa a listar as 7 premissas completas, em vez de apenas as diretamente aplicáveis |
