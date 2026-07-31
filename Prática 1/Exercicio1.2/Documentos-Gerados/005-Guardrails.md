# Guardrails — Consolidação e Backlog

> **Versão:** 1.0
> **Data da versão:** 30/07/2026
> **Responsável:** DB1-Arlindo
> **Status:** Aprovado
> **Fase do projeto:** Intent + Discovery — Passo 2 (Definição dos Fluxos)

## 1. Contexto

Este documento consolida, em um único lugar, os guardrails de comportamento do assistente de IA já confirmados nos três fluxos documentados (`002-Fluxo Principal.md`, `003-Fluxo Fallback.md`, `004-Fluxo Feedback.md`), servindo como fonte de referência para trabalhos posteriores. Além disso, registra uma lista de guardrails candidatos, identificados a partir dos gaps de discovery (`02-hipoteses-gaps.md`), que não foram formalizados nesta fase e ficam como backlog para análise futura.

## 2. Guardrails confirmados

1. **O assistente nunca completa uma resposta com conhecimento geral do modelo quando a base restrita não cobre o tema.**
   - **Origem:** Fluxo Principal, Fluxo de Fallback.
   - **Risco endereçado:** decorre da premissa 2.2 (base de conhecimento restrita) — evita que o assistente "invente" informação fora do escopo documental da NovaTech.

2. **O assistente nunca reapresenta uma resposta/fonte já sinalizada como incorreta ou expirada sem revisão prévia.**
   - **Origem:** Fluxo Principal, Fluxo de Fallback, Fluxo de Feedback.
   - **Risco endereçado:** salvaguarda da premissa 2.4 — evita que um erro já identificado por um atendente se repita para outros atendentes antes da correção.

3. **O assistente nunca decide sozinho qual fonte prevalece diante de qualquer sinal de conflito ou ambiguidade entre fontes** (seja por sobreposição de escopo entre documentos distintos e válidos, seja por eventual falha de governança que deixe mais de uma versão ativa) **— sempre sinaliza o conflito e aciona decisão humana.**
   - **Origem:** Fluxo de Fallback.
   - **Risco endereçado:** GAP-05 (P1-Crítico, `02-hipoteses-gaps.md`) — risco do assistente citar uma fonte tecnicamente correta, porém conflitante ou desatualizada, com falsa confiança.

## 3. Candidatos para análise futura (backlog — não implementados nesta fase)

1. **Transparência de vigência da fonte** — o assistente sempre exibe a data de última atualização do documento-fonte junto com a resposta.
   - **Ligado a:** risco de resposta desatualizada; GAP-02 (uso comprovado de versão desatualizada em campo).

2. **Não combinar regras de fontes diferentes sem sinalizar** — o assistente nunca calcula ou combina valores/prazos vindos de documentos distintos sem indicar explicitamente quais fontes foram combinadas.
   - **Ligado a:** GAP-08 (prazos internos x SLA formal concorrentes, sem reconciliação explícita).

3. **Não generalizar regra restrita a um segmento** — o assistente nunca aplica automaticamente uma regra documentada para um tier/segmento específico a outros tiers sem confirmação explícita da fonte.
   - **Ligado a:** GAP-11 (ambiguidade sobre se uma regra de SLA vale só para o tier Gold ou para todos).

4. **Sinalizar dependência de conhecimento tácito** — quando a única informação disponível descreve uma exceção tratada informalmente por uma área, não documentada como regra formal, o assistente marca isso como exceção sujeita a validação humana, não como regra padrão.
   - **Ligado a:** GAP-04 (dependência de conhecimento tácito/pessoa-chave) e GAP-07 (exceções informais da Gestão de Riscos não documentadas).

5. **Nunca minimizar tema sensível/crítico** — para temas classificados como incidente crítico ou exceção de risco, o assistente sempre reforça o encaminhamento ao canal apropriado, mesmo havendo informação parcial disponível.
   - **Ligado a:** risco de tratamento inconsistente em temas de maior exposição (segurança, compliance, risco financeiro).

## 4. Premissas do projeto

Lista completa das premissas definidas em `001-Premissas.md`, mantida por padrão em todos os documentos desta biblioteca:

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
| 1.0 | 30/07/2026 | DB1-Arlindo | Criação do documento — consolidação dos guardrails confirmados e backlog de candidatos |
