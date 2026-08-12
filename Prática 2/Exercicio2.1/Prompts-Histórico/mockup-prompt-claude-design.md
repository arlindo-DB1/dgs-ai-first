# Prompt — Mockup de Interface de Resposta no Microsoft Teams

> Prompt pronto para copiar e colar no Claude Design. Segue o padrão da Prática 1: conteúdo exato, sem adicionar/remover/inferir elementos além do especificado. Baseado no `requirements.md` (v0.5) e `domain-model.md` (v0.4) do módulo `query-endpoint` — o conteúdo do mockup em si (cenário PROC-042, valores, layout) não foi afetado pelas revisões posteriores desses dois documentos.

---

## Prompt

Gerar um mockup de tela de chat do **Microsoft Teams**, mostrando uma interação entre um atendente e o bot "NovaTech Assistant", com o conteúdo exato abaixo — sem adicionar, remover ou inferir elementos além dos especificados.

**Estilo visual:** seguir os padrões visuais reais do Microsoft Teams (tema claro) — não usar um design system genérico. Cabeçalho de conversa com nome e ícone do bot; bolhas de mensagem alinhadas à direita (atendente) e à esquerda (bot); fonte Segoe UI; cor de destaque roxa do Teams (#6264A7) em elementos interativos; avatar do atendente como iniciais genéricas em círculo (não inventar foto).

### Sequência da conversa (2 mensagens)

**1. Mensagem do Atendente** (bolha à direita, texto simples):
> "Qual o frete especial para uma carga de 600kg indo para Manaus?"

**2. Resposta do NovaTech Assistant** (bolha à esquerda, em formato de Adaptive Card do Teams):

- **Borda lateral esquerda do card:** faixa colorida vertical de destaque na cor **vermelha** — indicador visual de confiança Baixa (regra: verde = Alta, âmbar = Média, vermelho = Baixa; nesta tela só a versão vermelha aparece).
- **Cabeçalho do card:** ícone do assistente + "NovaTech Assistant".
- **Badge de confiança** (canto superior direito do card, fundo vermelho claro): "🔴 Confiança: Baixa".
- **Linha de alerta em destaque**, logo abaixo do cabeçalho, em texto vermelho/negrito: "⚠️ Fontes com valores divergentes — confirme antes de repassar ao cliente."
- **Corpo do texto explicativo:** "Encontrei duas versões do procedimento de frete especial (PROC-042) com valores diferentes para essa rota, e nenhuma está marcada como vigente no sistema. Veja as duas abaixo:"
- **Tabela/FactSet comparativo**, 2 colunas lado a lado (PROC-042 v1 | PROC-042 v2):
  | | PROC-042 v1 | PROC-042 v2 |
  |---|---|---|
  | Multiplicador regional (Norte) | 1.6 | 1.8 |
  | Valor do frete calculado | R$ 1.360,00 | R$ 1.530,00 |
  | Prazo adicional | +2 dias úteis | +3 dias úteis |
- **Rodapé de fontes citadas:** "📄 Fontes: PROC-042 (03/03/2023) · PROC-042 v2 (10/11/2023)".
- **Rodapé de ações** (3 botões pequenos, lado a lado, estilo Teams):
  - "👍 Útil"
  - "👎 Não útil"
  - "🚫 Bloquear resposta"

**Anotação de texto** (elemento de callout, ligado por linha pontilhada ao botão "🚫 Bloquear resposta", mesmo padrão usado nos diagramas BPMN da fase anterior): *"Ao clicar, abre um campo de texto para o atendente informar o motivo do bloqueio antes de confirmar."*

---

## Notas para quem for rodar este prompt no Claude Design

- Os valores numéricos (R$ 1.360,00 / R$ 1.530,00) são **ilustrativos**, criados para esta demonstração — não representam um cálculo real de um valor base específico.
- O badge/borda verde (Alta) e âmbar (Média) **não precisam ser desenhados nesta tela** — só a variação vermelha (Baixa), que é o caso escolhido para este mockup. Ficam registrados aqui como regra de cor para telas futuras: ✅ verde = Alta, ⚠️ âmbar = Média, 🔴 vermelho = Baixa.
- Os botões de feedback (👍/👎) e o botão de bloqueio são elementos de UI do `teams-bot` — o comportamento por trás deles (registro do feedback, motivo do bloqueio) não é definido neste mockup nem no `requirements.md` do `query-endpoint`; aparecem aqui só para compor uma tela realista.
- Testar o resultado real no Claude Design antes de considerar definitivo — historicamente (Prática 1) o primeiro prompt raramente sai perfeito; ajustes costumam ser pontuais (reposicionamento, espaçamento), não uma regeneração completa.

---

## Decisões registradas nesta versão do mockup

| Decisão | Origem |
|---|---|
| Interação escolhida: pergunta de frete especial com contradição de fonte (não um caminho feliz simples) | Sugestão do Product Specialist (Claude), aceita pelo usuário |
| Valores numéricos ilustrativos | Sugestão do Product Specialist (Claude), aceita |
| Padrão visual = Microsoft Teams real (não o design system "Modernist" da Prática 1) | Pedido explícito do usuário |
| Botões de feedback (👍/👎) inclusos, comportamento fora de escopo | Sugestão do Product Specialist (Claude), aceita — "vamos utilizar estes por enquanto" |
| Botão "🚫 Bloquear resposta" com anotação de comportamento (abre campo de motivo) | Pedido explícito do usuário |
| Indicador de confiança reforçado: borda lateral colorida no card + linha de alerta em destaque para Baixa | Sugestão do Product Specialist (Claude), aceita pelo usuário |
