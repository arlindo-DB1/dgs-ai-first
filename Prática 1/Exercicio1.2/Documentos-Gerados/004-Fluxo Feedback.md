# Fluxo de Feedback — Sinalização e Tratamento de Conteúdo da Base

> **Versão:** 1.0
> **Data da versão:** 30/07/2026
> **Responsável:** DB1-Arlindo
> **Status:** Aprovado
> **Fase do projeto:** Intent + Discovery — Passo 2 (Definição dos Fluxos)

## 1. Cenário

Fluxo de feedback — trata os três gatilhos que saem do Fluxo de Fallback (ambiguidade entre documentos, lacuna real de documentação, e discordância do atendente), encaminhando cada um para a ação correspondente: esclarecimento de fronteira, criação de conteúdo, ou correção de conteúdo/comportamento do assistente.

## 2. Atores envolvidos neste fluxo

- **Atendente** — sinaliza que não confia em uma resposta, ou que uma resposta já utilizada estava errada, desatualizada ou incompleta.
- **Assistente IA** — identifica ambiguidade ou lacuna (conforme o Fluxo de Fallback), registra sinalizações do atendente e suspende a resposta/fonte envolvida, quando aplicável.
- **Responsável pela Base** — recebe, analisa e trata cada caso: corrige conteúdo existente, cria conteúdo ausente, esclarece a fronteira entre documentos, ou identifica quando o problema é de comportamento do assistente.
- **Time Técnico do Assistente** — acionado quando o problema identificado não é de conteúdo, mas de comportamento do assistente (ex.: interpretação incorreta de uma fonte correta); responsável por revisar, ajustar e liberar a correção técnica.

## 3. Cenário-resumo (Given-When-Then)

**Cenário 1 — Ambiguidade entre dois documentos válidos**
Dado que o assistente identificou ambiguidade entre dois documentos válidos sobre o mesmo tema (conforme o Fluxo de Fallback),
Quando o caso é encaminhado ao Responsável pela Base,
Então ele avalia e formaliza a fronteira entre os processos envolvidos, atualizando a base do assistente com essa definição.

**Cenário 2 — Lacuna real de documentação (nenhuma fonte cobre o tema)**
Dado que o assistente não encontrou nenhuma fonte que cubra o tema perguntado (conforme o Fluxo de Fallback),
Quando o caso é registrado como "gap de cobertura documental" e encaminhado ao Responsável pela Base,
Então ele elabora e publica a política ausente junto à área dona do tema, e atualiza a base do assistente com o novo conteúdo.

**Cenário 3 — Atendente não confia na resposta**
Dado que o atendente, ao ler uma resposta com fonte e indicador de confiança, não confia nela *(por suspeita de alucinação, política não vigente, ou outro motivo)* — ou identifica posteriormente que uma resposta já utilizada estava errada, desatualizada ou incompleta,
Quando ele sinaliza esse problema,
Então a resposta/fonte envolvida é suspensa de circulação, e o Responsável pela Base avalia se o problema é de conteúdo (corrige a base) ou de comportamento do assistente (aciona o Time Técnico do Assistente, que revisa o comportamento relacionado ao caso, aplica o ajuste necessário e libera a correção).

## 4. Detalhamento (SOP textual)

**Como começa:** o Fluxo de Feedback é acionado por um dos três gatilhos abaixo — vindo do Fluxo de Fallback (ambiguidade ou lacuna) ou por sinalização direta do atendente (discordância, inclusive percebida após o uso da resposta).

**Passo a passo:**

1. Ocorre um dos três gatilhos, na mesma ordem definida no Fluxo de Fallback:
   - o assistente identificou ambiguidade entre dois documentos válidos sobre o mesmo tema; ou
   - o assistente não encontrou nenhuma fonte que cubra o tema (lacuna real); ou
   - o atendente não confia na resposta lida, ou sinaliza posteriormente que uma resposta já utilizada estava errada, desatualizada ou incompleta.
2. Se o gatilho for a discordância/sinalização do atendente, o assistente registra a sinalização e suspende a resposta/fonte envolvida — nenhum outro atendente recebe essa mesma resposta enquanto durar a suspensão.
3. O caso é encaminhado ao Responsável pela Base, com o tipo de tratamento necessário conforme o gatilho de origem.
4. O Responsável pela Base analisa o caso e realiza a ação correspondente:
   - **Ambiguidade** → define e documenta explicitamente a fronteira entre os processos envolvidos, atualizando a base do assistente com essa definição.
   - **Lacuna real** → elabora e publica a política ausente junto à área dona do tema (Operações, Compliance ou Comercial), e atualiza a base do assistente com o novo conteúdo.
   - **Discordância/sinalização** → avalia se o problema é de conteúdo ou de comportamento do assistente:
     - Se for de conteúdo → atualiza o conteúdo existente na base, respeitando a regra de versionamento único ativo.
     - Se for de comportamento do assistente → aciona o Time Técnico do Assistente, que revisa o comportamento relacionado ao caso, aplica o ajuste técnico necessário e libera a correção.
5. Se havia uma resposta suspensa (gatilho de discordância), a suspensão é encerrada após a correção de conteúdo ou a liberação do ajuste técnico do assistente — o conteúdo/comportamento revisado volta a ficar disponível.
6. Na próxima vez que o tema for perguntado, o assistente já responde com o conteúdo corrigido, criado, esclarecido, ou com o comportamento ajustado.

**Guardrails aplicados aqui:**

- O assistente nunca reapresenta uma resposta/fonte já sinalizada como incorreta ou expirada sem revisão prévia.
- A regra de versionamento único ativo (premissa 2.3) é aplicada obrigatoriamente em toda correção ou atualização de conteúdo.

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
| 1.0 | 30/07/2026 | DB1-Arlindo | Criação do documento — versão aprovada do Fluxo de Feedback |
