# Exercício 3.1 — Comparação: Avaliação Humana vs. Avaliação do Claude

> **Status: ✅ Aprovado pelo usuário em 2026-08-15.**
> Compara os vereditos e justificativas de `01-AvaliaçãoHumana.md` (avaliação humana, Tarefa 1) com `02-AvaliaçãoClaude.md` (avaliação do Claude, Tarefa 2), feita de forma independente.

---

## Tabela comparativa

| # | Veredito Humano (`01-AvaliaçãoHumana.md`) | Veredito Claude (`02-AvaliaçãoClaude.md`) | Concordância no veredito final? | Divergência de detalhe |
|---|---|---|---|---|
| 1 | Correta (ponto de atenção: verificar se o procedimento citado pertence mesmo à seção informada) | Correta (mesma ressalva: procedimento é da §3.3, não §3.2) | ✅ Sim | Nenhuma — mesmo achado, dito com outras palavras |
| 2 | Parcialmente correta (ponto de atenção: como se conta as 48h) | Parcialmente correta (informação incompleta: ambiguidade geral vs. crítico não tratada, "úteis" omitido) | ✅ Sim | Nenhuma relevante — convergência total |
| 3 | Correta (sem ressalva) | Correta, mas com achado adicional: "supervisor" não tem grounding no Anexo A (o canal correto é Gestão de Riscos, ramal 4500) | ✅ Sim no veredito | ⚠️ Divergência real: avaliação humana não capturou a imprecisão do canal de escalação |
| 4 | Incorreta — alucinação (confiança alta, sem fonte, procedimento inventado) | Incorreta — alucinação (mesmos motivos) | ✅ Sim | Nenhuma — convergência total |
| 5 | Correta (sem ressalva) | Correta, mas com o mesmo achado adicional da resposta 3: deveria ser "Comercial", não "supervisor" | ✅ Sim no veredito | ⚠️ Mesma divergência da resposta 3 |
| 6 | Incorreta — fonte não confiável (FAQ informal, sem validação, confiança alta indevida) | Incorreta — mesma razão, mais a observação de que a resposta omite o detalhe do prazo real (~2 dias) do próprio FAQ | ✅ Sim | Divergência menor — Claude acrescenta um ponto de completude que a avaliação humana não mencionou |

---

## Resumo honesto da comparação

- **Concordância total** nos vereditos finais das 6 respostas — nenhuma diverge quanto a correta/parcialmente correta/incorreta. Isso também bate com os critérios de avaliação oficiais do exercício (respostas 4 e 6 como problemáticas; 1, 2, 3 e 5 como adequadas). *(Nota: a resposta 2, embora agrupada como "adequada" pelo critério oficial, recebe depois tratamento formal de classificação de erro na Tarefa 3 — ver a ressalva completa na Seção 5 de `05-ConsolidaçãoAvaliações.md`.)*
- **Maior divergência de conteúdo:** o Claude identificou um padrão recorrente (respostas 3 e 5) de uso do termo genérico **"supervisor"** para escalação, quando o Anexo A especifica canais concretos e diferentes em cada caso (Gestão de Riscos ramal 4500; Comercial). Esse achado não estava na avaliação humana. Ele não muda o veredito de nenhuma das duas respostas (ambas continuam corretas em essência), mas é um sinal de um possível problema sistemático de grounding em detalhes secundários.
- Fora esse ponto, as duas avaliações são equivalentes em rigor e chegaram às mesmas conclusões de forma independente — a convergência é real, não forçada (a avaliação do Claude foi feita sem consultar as conclusões humanas antes de decidir cada veredito).
