# Fluxo de Fallback — Assistente sem Confiança ou Atendente Discorda

> **Versão:** 1.1
> **Data da versão:** 30/07/2026
> **Responsável:** DB1-Arlindo
> **Status:** Aprovado
> **Fase do projeto:** Intent + Discovery — Passo 2 (Definição dos Fluxos)

## 1. Cenário

Fluxo de fallback — cobre os desvios do Fluxo Principal quando o assistente não tem confiança suficiente na resposta (ambiguidade entre documentos válidos ou lacuna real de documentação), ou quando o atendente discorda da resposta apresentada, mesmo com fonte citada.

## 2. Atores envolvidos neste fluxo

- **Atendente** — colaborador da NovaTech que consulta o assistente e, neste fluxo, precisa escalar o caso quando o assistente não resolve.
- **Assistente IA** — identifica e sinaliza os casos de ambiguidade ou ausência de fonte, sem decidir sozinho o que prevalece.
- **Cliente** — é comunicado de que seu chamado foi escalado, enquanto aguarda a resolução da dúvida.
- **Supervisor / Área especializada** — resolve o caso escalado com base em julgamento humano, apoiado por documentação auxiliar e dentro da sua alçada de resolução (ex.: Gestão de Riscos, Comercial, conforme o tema do chamado).
- **Responsável pela Base** — acionado quando o caso revela uma lacuna real de documentação, ou quando uma resposta é suspensa por discordância do atendente.

## 3. Cenário-resumo (Given-When-Then)

**Cenário 1 — Ambiguidade entre documentos válidos**
Dado que o atendente perguntou ao assistente sobre um tema,
Quando o assistente identifica que dois documentos válidos tratam do tema de forma ambígua ou sobreposta,
Então o assistente sinaliza a ambiguidade ao atendente, o cliente é comunicado de que seu chamado foi escalado, e o caso é encaminhado ao Supervisor/Área especializada para decisão.

**Cenário 2 — Lacuna real de documentação**
Dado que o atendente perguntou ao assistente sobre um tema,
Quando nenhuma fonte da base documental cobre esse tema,
Então o assistente informa a ausência de fonte, o cliente é comunicado de que seu chamado foi escalado, o caso é encaminhado ao Supervisor/Área especializada, e uma solicitação formal de criação de política é enviada ao Responsável pela Base.

**Cenário 3 — Atendente discorda da resposta**
Dado que o assistente respondeu com uma fonte citada e um indicador de confiança,
Quando o atendente, mesmo assim, não confia na resposta *(por exemplo, por suspeita de alucinação, uso de uma política não vigente, ou outro motivo)*,
Então a resposta é suspensa e não é utilizada, o cliente é comunicado de que seu chamado foi escalado, e o caso é encaminhado ao Supervisor/Área especializada para decisão.

## 4. Detalhamento (SOP textual)

**Como começa:** o atendente recebe uma dúvida do cliente durante o chamado, formula a pergunta ao assistente em linguagem natural, e o assistente busca a resposta na base documental restrita. A partir daqui, um dos três desvios abaixo pode ocorrer.

**Passo a passo:**

1. O atendente formula a pergunta ao assistente, em linguagem natural.
2. O assistente busca a resposta na base documental restrita.
3. Ocorre um dos três gatilhos de desvio:
   - o assistente identifica ambiguidade entre dois documentos válidos sobre o mesmo tema; ou
   - o assistente não encontra nenhuma fonte que cubra o tema (lacuna real); ou
   - o assistente responde com fonte e indicador de confiança, mas o atendente, ao ler, não confia na resposta (por suspeita de alucinação, política não vigente, ou outro motivo).
4. O assistente comunica ao atendente o motivo do desvio. Se o motivo foi discordância do atendente, a resposta é suspensa e não é reutilizada até revisão do Responsável pela Base.
5. O atendente comunica ao cliente que o chamado foi escalado para análise.
6. O atendente escala o chamado ao Supervisor/Área especializada correspondente ao tema.
7. O Supervisor/Área especializada resolve o caso com base em julgamento humano, apoiado por documentação auxiliar e dentro de sua alçada de resolução, e responde ao atendente com a orientação para aquele caso específico.
8. O atendente usa essa orientação para concluir o atendimento ao cliente. A interação pode **continuar** (surge uma nova dúvida, retomando o Fluxo Principal) ou **se encerrar** (não há mais dúvidas, o chamado é concluído).
9. **Tratamento de conteúdo por gatilho** *(detalhado no Fluxo de Feedback)*:
   - Ambiguidade entre documentos válidos → o caso é sinalizado para esclarecimento futuro da fronteira entre os documentos envolvidos.
   - Lacuna real de documentação → o caso é registrado como "gap de cobertura documental", e uma solicitação formal de criação/atualização de política é enviada ao Responsável pela Base.
   - Discordância do atendente → a resposta já suspensa no passo 4 permanece indisponível até revisão do Responsável pela Base.

**Guardrails aplicados aqui:**

- O assistente nunca decide sozinho qual fonte prevalece diante de qualquer sinal de conflito ou ambiguidade entre fontes — seja por sobreposição de escopo entre documentos distintos e válidos, seja por eventual falha de governança que deixe mais de uma versão ativa — sempre sinaliza o conflito e aciona decisão humana.
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
| 1.0 | 30/07/2026 | DB1-Arlindo | Criação do documento — versão aprovada do Fluxo de Fallback |
| 1.1 | 30/07/2026 | DB1-Arlindo | Guardrail de decisão entre fontes confirmado e ampliado (Opção B) — deixa de ser restrito à ambiguidade entre documentos distintos e passa a cobrir qualquer sinal de conflito, incluindo eventual falha de governança de versão |
