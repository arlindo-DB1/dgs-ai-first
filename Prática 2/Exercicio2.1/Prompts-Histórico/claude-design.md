# claude-design — Registro da interação

Projeto: mockup de tela de chat do Microsoft Teams com o bot "NovaTech Assistant"
Data: 11/08/2026
Arquivos gerados: `Teams - NovaTech Assistant.dc.html`, `mockup.jpg`

---

## 1. Prompt inicial do usuário

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
- **Rodapé de ações** (3 botões pequenos, lado a lado, estilo Teams): "👍 Útil", "👎 Não útil", "🚫 Bloquear resposta".

**Anotação de texto** (callout ligado por linha pontilhada ao botão "🚫 Bloquear resposta"): *"Ao clicar, abre um campo de texto para o atendente informar o motivo do bloqueio antes de confirmar."*

### Notas registradas no prompt

- Os valores numéricos (R$ 1.360,00 / R$ 1.530,00) são **ilustrativos**, criados para esta demonstração — não representam um cálculo real de um valor base específico.
- O badge/borda verde (Alta) e âmbar (Média) **não precisam ser desenhados nesta tela** — só a variação vermelha (Baixa). Regra de cor para telas futuras: ✅ verde = Alta, ⚠️ âmbar = Média, 🔴 vermelho = Baixa.
- Os botões de feedback (👍/👎) e o de bloqueio são elementos de UI do `teams-bot`; o comportamento por trás deles não é definido neste mockup nem no `requirements.md` do `query-endpoint`.
- Testar o resultado real no Claude Design antes de considerar definitivo; ajustes costumam ser pontuais.

### Decisões registradas nesta versão do mockup

| Decisão | Origem |
|---|---|
| Interação escolhida: pergunta de frete especial com contradição de fonte (não um caminho feliz simples) | Sugestão do Product Specialist (Claude), aceita pelo usuário |
| Valores numéricos ilustrativos | Sugestão do Product Specialist (Claude), aceita |
| Padrão visual = Microsoft Teams real (não o design system "Modernist" da Prática 1) | Pedido explícito do usuário |
| Botões de feedback (👍/👎) inclusos, comportamento fora de escopo | Sugestão do Product Specialist (Claude), aceita |
| Botão "🚫 Bloquear resposta" com anotação de comportamento (abre campo de motivo) | Pedido explícito do usuário |
| Indicador de confiança reforçado: borda lateral colorida no card + linha de alerta em destaque para Baixa | Sugestão do Product Specialist (Claude), aceita |

---

## 2. Construção (Claude)

Criado o arquivo `Teams - NovaTech Assistant.dc.html` com:

- Cabeçalho de conversa: avatar roxo (#6264A7) com ícone de bot, nome "NovaTech Assistant" e rótulo "Bot".
- Bolha do atendente à direita em lilás (#e8ebfa), avatar circular cinza com iniciais "AT".
- Adaptive Card à esquerda: faixa vertical vermelha (#c4314b) de 6px, cabeçalho com ícone + nome, badge "🔴 Confiança: Baixa" em fundo vermelho claro, linha de alerta em vermelho/negrito, texto explicativo, tabela comparativa v1/v2, rodapé de fontes e três botões de ação no padrão Teams.
- Callout tracejado ligado por linha pontilhada ao botão "🚫 Bloquear resposta".
- Fonte Segoe UI em toda a tela; nenhum elemento extra (sem sidebar, sem caixa de composição).
- Prop de tweak: `showAnnotation` (liga/desliga o callout).

Resposta ao usuário: "Mockup pronto: cabeçalho do bot, bolha do atendente à direita, Adaptive Card com faixa vermelha, badge de confiança baixa, alerta, tabela comparativa v1/v2, fontes citadas, os três botões e o callout pontilhado ligado ao 'Bloquear resposta'. Só o conteúdo especificado — sem caixa de composição nem sidebar."

---

## 3. Ajuste de layout

**Problema detectado na verificação:** o callout da anotação era cortado em larguras de janela menores — o card tinha largura fixa de 640px e não encolhia, empurrando parte do texto para fora do container com `overflow:hidden`.

**Correção aplicada:**
- Card: de `width:640px; flex:0 0 auto` para `max-width:640px; flex:1 1 auto; min-width:0`.
- Coluna do callout: de `flex:1; min-width:220px` para `flex:0 1 auto; min-width:180px`.

Resultado: o card encolhe junto com a janela e o callout pontilhado permanece inteiramente visível.

---

## 4. Exportações

| Pedido do usuário | Entrega |
|---|---|
| "preciso exportar esta tela em formato jpg com o nome mockup" | `mockup.jpg` — captura da tela em 2048×1290 px (2x), fundo branco, qualidade 95% |
| "exportar nossa interação num arquivo chamado claude-design no formato .md" | este arquivo, `claude-design.md` |

---

## 5. Estado final do projeto

- `Teams - NovaTech Assistant.dc.html` — mockup (arquivo de trabalho, editável)
- `mockup.jpg` — imagem exportada da tela
- `claude-design.md` — este registro
