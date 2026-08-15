**Usuário:**

```markdown
Qual o prazo de devolução para produtos standard?
```

**Assistente:**

De acordo com a **POL-001 — Política de Devolução de Mercadorias** (documento oficial da NovaTech), o prazo geral de devolução é de **7 (sete) dias úteis** após a data de recebimento confirmada no sistema de tracking (a contagem exclui sábados, domingos e feriados nacionais).

Um ponto de atenção: esse prazo é único e se aplica a **todos os tipos de carga**, sem diferenciação por tier de cliente (Gold, Silver ou Standard). O "Standard" que aparece nos documentos da NovaTech é uma classificação de cliente usada para SLAs de atendimento (tabela SLA-2024), não uma categoria de produto — não existe um prazo de devolução específico para "produtos standard". Se a pergunta era sobre o tier Standard, ele não altera o prazo de devolução, apenas os SLAs de resposta/resolução de chamados.

As únicas exceções ao prazo de 7 dias úteis (não elegíveis para devolução pelo processo padrão, independente do tier do cliente) são: cargas perigosas (classes 1–6 ANTT), cargas refrigeradas com rompimento da cadeia de frio, e cargas com lacre de segurança violado sem documentação na entrega. Essas exigem contato com Gestão de Riscos (ramal 4500).

Sources:
- [POL-001-politica-devolucao.md](computer://C:\Users\arlindo.junior\dgs-ai-first\Prática 2\Arquivos-de-Trabalho\novatech-assistant\docs\novatech\POL-001-politica-devolucao.md)
- [SLA-2024-tabela-sla-clientes.md](computer://C:\Users\arlindo.junior\dgs-ai-first\Prática 2\Arquivos-de-Trabalho\novatech-assistant\docs\novatech\SLA-2024-tabela-sla-clientes.md)

---

**Usuário:**

```markdown
Meu cliente é Silver. Qual o prazo de resolução?
```

**Assistente:**

Segundo a **SLA-2024 — Tabela de SLA por Tipo de Cliente**, para clientes do tier **Silver** o prazo de resolução é:

- Chamados gerais: até **48 horas úteis**
- Incidentes críticos: até **8 horas**

Vale lembrar que o relógio de SLA pausa fora do horário comercial (08h–18h, dias úteis) para chamados gerais, mas essa pausa não se aplica a incidentes críticos — e a pausa por horário comercial só é citada explicitamente como exceção para clientes Gold em incidentes críticos, ou seja, para Silver o relógio de incidente crítico segue contando mesmo fora do expediente (o texto só isenta explicitamente o Gold da pausa, o que sugere que Silver e Standard têm o relógio pausado mesmo em críticos — vale confirmar esse ponto com Operações, pois a redação do documento é ambígua nesse detalhe).

Se a dúvida for sobre qual dos dois prazos se aplica ao seu caso, me diga se é um chamado geral ou um incidente crítico (ex.: carga de alto valor sem rastreamento, ou risco à segurança) que eu confirmo o prazo certo.

Sources:
- [SLA-2024-tabela-sla-clientes.md](computer://C:\Users\arlindo.junior\dgs-ai-first\Prática 2\Arquivos-de-Trabalho\novatech-assistant\docs\novatech\SLA-2024-tabela-sla-clientes.md)

---

**Usuário:**

```markdown
Posso devolver carga perigosa classe 3?
```

**Assistente:**

Não, pela política oficial. Carga perigosa classe 3 (líquidos inflamáveis, conforme classificação ANTT) está entre as categorias explicitamente excluídas do processo padrão de devolução na **POL-001** (seção 3.2), junto com as demais classes 1 a 6 (explosivos, gases, sólidos inflamáveis, oxidantes/peróxidos, tóxicos e infectantes).

Para esse tipo de carga, o cliente não usa o fluxo padrão do Portal do Cliente — precisa entrar em contato com o setor de **Gestão de Riscos (ramal 4500)** para tratamento individual do caso.

Um detalhe complementar (do FAQ interno do time de atendimento, que é um documento não oficial e não validado por Compliance/Operações — usar com cautela): na prática, já houve casos em que Gestão de Riscos autorizou uma exceção. Ou seja, a orientação certa ao cliente não é dizer que "é impossível", e sim que carga perigosa exige tratamento especial e avaliação caso a caso pelo ramal 4500 — a decisão final não é do atendimento padrão.

Sources:
- [POL-001-politica-devolucao.md](computer://C:\Users\arlindo.junior\dgs-ai-first\Prática 2\Arquivos-de-Trabalho\novatech-assistant\docs\novatech\POL-001-politica-devolucao.md)
- [FAQ-atendimento.md](computer://C:\Users\arlindo.junior\dgs-ai-first\Prática 2\Arquivos-de-Trabalho\novatech-assistant\docs\novatech\FAQ-atendimento.md)

---

**Usuário:**

```markdown
Qual a política para carga danificada durante transporte?
```

**Assistente:**

Não há um documento formal (POL/PROC) na base que trate especificamente de carga danificada em trânsito — isso é uma lacuna real na documentação oficial da NovaTech. A única referência que encontrei é do **FAQ-Atendimento**, que é explicitamente um documento não oficial e não validado por Compliance ou Operações.

Segundo esse FAQ (item 38), a prática informal do time é:

- Processo diferente do de devolução padrão.
- Cliente deve registrar a ocorrência em até 48h após o recebimento, com fotos e laudo se possível.
- A NovaTech investiga e, se comprovada responsabilidade da empresa, reembolsa integralmente.
- O caso não passa pelo atendimento normal — é encaminhado ao Jurídico, pelo e-mail sinistros@novatech.com.br.

Como não existe respaldo formal para esse fluxo, eu não trataria essas informações como política oficial ao repassar para o cliente. Recomendo confirmar o procedimento com o Jurídico/Operações antes de comunicar prazos e garantias como se fossem regra formal — principalmente o prazo de 48h e a promessa de reembolso integral, que podem estar desatualizados ou incompletos.

Sources:
- [FAQ-atendimento.md](computer://C:\Users\arlindo.junior\dgs-ai-first\Prática 2\Arquivos-de-Trabalho\novatech-assistant\docs\novatech\FAQ-atendimento.md)

---

**Usuário:**

```markdown
Qual o SLA do cliente Enterprise?
```

**Assistente:**

Não existe tier "Enterprise" na classificação de clientes da NovaTech. Segundo a **SLA-2024 — Tabela de SLA por Tipo de Cliente**, a empresa trabalha com apenas **3 tiers**: Gold, Silver e Standard — e o próprio documento afirma explicitamente que não existem outros tiers além desses três.

Solicitações de SLA diferenciado fora desses tiers devem ser encaminhadas ao Comercial para análise de viabilidade.

É possível que o cliente esteja confundindo com outra transportadora, com um programa antigo, ou usando "Enterprise" informalmente para se referir ao tier Gold (voltado a contratos maiores). Antes de informar qualquer SLA, vale pedir o número do contrato para confirmar o tier real do cliente na base da NovaTech — não invento um SLA para um tier que não existe.

Sources:
- [SLA-2024-tabela-sla-clientes.md](computer://C:\Users\arlindo.junior\dgs-ai-first\Prática 2\Arquivos-de-Trabalho\novatech-assistant\docs\novatech\SLA-2024-tabela-sla-clientes.md)

---

**Usuário:**

```markdown
Posso enviar carga perigosa com frete expresso?
```

**Assistente:**

Sim, mas com uma condição importante — e vale destacar que essa informação não tem respaldo em nenhum documento formal (POL/PROC) da NovaTech, vem apenas do FAQ interno do time de atendimento, que é explicitamente não validado por Compliance ou Operações.

Segundo esse FAQ (item 32): é possível enviar carga perigosa com frete expresso, mas exige autorização do Compliance e a documentação ANTT precisa estar atualizada. Na prática, essa autorização costuma levar cerca de 2 dias para sair — ou seja, o "expresso" acaba não sendo tão rápido assim, e vale avisar o cliente sobre essa expectativa antes de confirmar o envio.

Como não há um processo formal documentado para isso, recomendo confirmar com Compliance os critérios exatos de autorização antes de tratar esse prazo de ~2 dias como garantido.

Sources:
- [FAQ-atendimento.md](computer://C:\Users\arlindo.junior\dgs-ai-first\Prática 2\Arquivos-de-Trabalho\novatech-assistant\docs\novatech\FAQ-atendimento.md)

---

**Usuário:**

```
Exporte o histórico desse chat como arquivo "BaseNovaTechAssistent.md" no caminho "C:\Users\arlindo.junior\dgs-ai-first\Prática 3\Exercício3.1\Documentos-Gerados"
Observação: A exportação deverá ser fiel ao chat, não deve fazer nenhum resumo ou alteração.
```

**Assistente:**

O arquivo **BaseNovaTechAssistent.md** foi exportado com o histórico completo e fiel do chat (todas as perguntas e respostas, sem resumos ou alterações) e salvo em:

`C:\Users\arlindo.junior\dgs-ai-first\Prática 3\Exercício3.1\Documentos-Gerados\BaseNovaTechAssistent.md`

---

**Usuário:**

```
Exporte novamente o histórico desse chat como arquivo "BaseNovaTechAssistent.md" no caminho "C:\Users\arlindo.junior\dgs-ai-first\Prática 3\Exercício3.1\Documentos-Gerados" Observações: 

* A exportação deverá ser fiel ao chat, não deve fazer nenhum resumo ou alteração.
* Não inclua os prompts de pedido de exportação e suas respostas
* Este novo pedido se deve ao arquivo exportado original ter sido deletado
```

**Assistente:**

O arquivo já tinha sido gerado sem os prompts de exportação (a exportação anterior já parava antes deles). Vou reenviar o mesmo conteúdo.

O arquivo **BaseNovaTechAssistent.md** foi exportado novamente (fiel ao chat, sem os prompts de exportação e sem resumos) e salvo em:

`C:\Users\arlindo.junior\dgs-ai-first\Prática 3\Exercício3.1\Documentos-Gerados\BaseNovaTechAssistent.md`
