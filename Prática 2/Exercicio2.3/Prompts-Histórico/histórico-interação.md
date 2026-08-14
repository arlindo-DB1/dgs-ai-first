# Histórico de Interação — Exercicio 2.3 (Participação na construção do AGENTS.md)

> Entregável desta subfase (Cenário 2 — Fase de Estruturação, Prática 2). Consolida a construção da seção "Product Rules & Guardrails" do `AGENTS.md`, incluindo o gap analysis e a evolução de duas versões auxiliares (`guardrails.md` v0.8 e `domain-model.md` v0.5, ambas novas versões criadas nesta subfase), até a aprovação final.

---

## 1. Decisões de processo (antes do rascunho)

| # | Decisão | Origem |
|---|---|---|
| 1 | Usuário reforçou, logo no início, para não investigar arquivos nem fazer perguntas de refinamento antes de terminar de passar todo o material — repetição da lição já registrada no Exercicio 2.2. | Usuário (correção) |
| 2 | Nome do documento de apoio: `referencias-agents.md`. Nome do entregável: `agents-md-product-specialist.md`. | Usuário |
| 3 | Fonte das regras de comportamento e do glossário: usar o `guardrails.md` (v0.7) e o `domain-model.md` (v0.4) reais e completos, não a versão simulada e simplificada do enunciado — a versão simulada foi usada só como checklist de cobertura (gap analysis), não como substituto. | Usuário |
| 4 | Escopo do entregável: só o documento em `Documentos-Gerados` — sem editar ainda o `AGENTS.md` real do repositório de trabalho (`Arquivos-de-Trabalho\novatech-assistant\AGENTS.md`), cujas outras seções pertencem a outros papéis e seguem `TODO`. | Usuário |
| 5 | **"Nova versão" ≠ "atualizar o arquivo original":** Claude começou a editar os arquivos originais do Exercicio 2.1/2.2 em vez de criar cópias versionadas na pasta do Exercicio 2.3 — usuário interrompeu duas vezes ("Ow, perai" / "Para ai, o que está fazendo?") até isso ficar claro. `guardrails.md` v0.7 e `domain-model.md` v0.4 permaneceram intactos; as novas versões (v0.8 e v0.5) foram criadas como arquivos próprios em `Exercicio2.3\Documentos-Gerados`. | Usuário (correção) |

---

## 2. Gap analysis — enunciado simulado × documentos reais

### Guardrails (9 itens simulados × 21 guardrails do `guardrails.md` v0.7)

| Achado | Resolução |
|---|---|
| 6 dos 9 itens simulados já cobertos pelo `guardrails.md` v0.7, sem necessidade de alteração. | Nenhuma ação. |
| **Gap real:** nenhum guardrail exigia o campo `source_document` sempre presente no JSON de retorno. | Novo **GR-D-10**. |
| **Gap real:** nenhum guardrail proibia validar um tier de cliente inexistente (ex: "Platinum"). | Novo **GR-N-08**. |
| **Gap real:** nenhum guardrail cobria escalação genérica ao supervisor como fallback de última instância. | Novo **GR-Q-06**. |
| **Conflito, não gap:** o item simulado "priorizar a versão mais recente e informar que existe versão anterior" contradiz o `GR-Q-02` (sempre mostrar ambas as versões, nunca escolher uma) — e reproduz o conselho informal do FAQ item 8 do Anexo A, origem do próprio Incidente 2. | **Decisão do usuário:** incorporado só como regra de ranking interno de recuperação (consistente com ADR-0003), nunca como regra de exibição — nota de esclarecimento adicionada ao `GR-Q-02`. |

### Glossário (5 termos citados no enunciado × linguagem ubíqua do `domain-model.md` v0.4)

| Achado | Resolução |
|---|---|
| "Cliente Gold", "carga perigosa" e "frete especial" já cobertos. | Nenhuma ação. |
| **Gap real:** "Multiplicador regional" só existia embutido na definição de "Frete especial", sem entrada própria — apesar de ser o núcleo da contradição do Incidente 2. | Nova entrada na linguagem ubíqua. |
| **Gap real:** "SLA de resolução" ("Tempo de resolução") não tinha entrada própria, apesar de já citado em `GR-N-06`. | Nova entrada na linguagem ubíqua. |

**Resultado:** `guardrails.md` → v0.8 (24 guardrails: 10 DEVE, 8 NÃO DEVE, 6 QUANDO EM DÚVIDA). `domain-model.md` → v0.5 (12 termos na linguagem ubíqua).

---

## 3. Autorrevisão do `guardrails.md` v0.8 (3 rodadas, a pedido do usuário)

| Rodada | Achados | Ajuste aplicado |
|---|---|---|
| 1 | (1) Referência cruzada **falsa**: GR-D-10 citava "VC-02" como já cobrindo o campo `source_document` — checado contra o `requirements.md` real, VC-02 trata de outro cenário (SLA do cliente Gold); o texto citado vinha de um `requirements.md` **simulado diferente**, de outro exercício. (2) GR-N-08 na verdade **já tinha** VC direto e aprovado (VC-05 — cenário "Platinum" idêntico), não detectado na v0.8 inicial. (3) Faltava nota de dependência de módulo em GR-Q-06 (escalação é UI do `teams-bot`). (4) Faltava referência à seção "Reconciliação decidida" do `requirements.md` §4 na nota do GR-Q-02. | Referência falsa removida; GR-N-08 elevado de Importante para Crítico (mesmo critério do GR-N-07); nota de dependência adicionada; referência cruzada adicionada; nota técnica sobre `source_document` precisar ser array (não string única). |
| 2 | Ao corrigir o exemplo do GR-D-10 (que combinava incorretamente `source_document: null` com `confidence: "Baixa"` — Baixa significa contradição, não ausência), descoberta uma **lacuna maior, pré-existente nos documentos já aprovados**: o `GR-D-05` declara confiança sempre `Alta`/`Média`/`Baixa`, mas o `requirements.md` v0.5 (já aprovado desde o Exercicio 2.1) usa um 4º valor, `"N/A"`, para os casos de "não encontrei" (VC-07, VC-08) — nunca incorporado à definição de confiança. | Exemplo do GR-D-10 corrigido. **Pendência registrada, não resolvida** (fora do escopo autorizado pelo usuário nesta subfase) — usuário decidiu explicitamente deixar como registro de pendência. |
| 3 | Revisão geral final: linha "Origem" do cabeçalho desatualizada (mesmo tipo de erro já visto no Ex. 2.2, rodada 7) — dizia "1 nota de esclarecimento", sem contar as várias notas adicionadas nas rodadas 1-2. | Corrigida para refletir o conteúdo real. |

Achado adicional (não factual, de transparência): **"Supervisor" (GR-Q-06) não corresponde a nenhum canal documentado no Anexo A** — o único canal real é o ramal 4500 (Gestão de Riscos, já coberto pelo GR-N-05). Mantido como fallback genérico de boa prática, com a ressalva explícita registrada.

---

## 4. `agents-md-product-specialist.md` — de v0.1 a v0.9

| Versão | Origem do achado | O que foi encontrado / decidido | Ajuste aplicado |
|---|---|---|---|
| 0.1 | Product Specialist (rascunho inicial) | Seção estruturada em 4 partes (regras, glossário, restrições de código, referências a specs), com decisões editoriais explicitamente listadas para aprovação do usuário. | Rascunho apresentado. |
| 0.2 | Usuário (2 perguntas específicas) | (1) "O formato resumido da Seção 1 vai impactar o trabalho?" — sim: perdia os exemplos concretos e o critério objetivo do GR-D-06 (lista de gírias). (2) "5 restrições na Seção 3 está correto?" — auditoria contra os 18 guardrails DEVE/NÃO DEVE de enforcement Código/Híbrido achou 2 campos de schema faltando (`last_updated` do GR-D-07, `partial_response` do GR-Q-05). | Lista de gírias reincorporada; campos adicionados ao schema. |
| 0.3 | Tech Lead (revisão completa) | 4 achados: schema com só `type`/`interface`, sem validação em runtime; `requires_human_validation`/`partial_response` opcionais quando deveriam ser obrigatórios; falta de path para `guardrails.md`/`domain-model.md` no Anexo C; pendência N/A subestimada como "não urgente". | Schema Zod real adicionado; campos tornados obrigatórios; proposta de `docs/product/`; pendência elevada a "bloqueio conhecido". |
| 0.4 | QA (revisão completa) | 4 achados: lista de gírias sem fixture único de verdade; guardrails QUANDO EM DÚVIDA sem cobertura de teste referenciada; formatação quebrada (citação markdown no meio de uma lista de bullets); invariante `confidence===Baixa ⟹ requires_human_validation===true` nunca afirmado explicitamente. Usuário pediu explicação detalhada do achado de formatação, e recomendação de posição (antes/depois) antes de aprovar. | Fixture referenciado; nota sobre `prompts/eval/golden-queries.json`; bloco de citação movido para depois da lista; invariante explicitado. |
| 0.5 | Product Specialist (Avaliação Final) | Avaliação formal contra os 4 critérios de aceite. 3 ressalvas honestas (nenhum critério falhou). Usuário pediu análise de quais eram resolvíveis sem comprometer o trabalho antes de decidir. | Ressalva 1 (Seção 1 não autossuficiente) mantida por decisão do usuário — resolver desfaria o design de índice enxuto já aprovado. Ressalva 2 (glossário) resolvida sem custo. Ressalva 3 (schema com N/A incompleto) mantida — reabri-la fugiria do escopo já fechado. |
| 0.6 | P.O. Senior | Usuário percebeu que essa ótica nunca tinha sido de fato aplicada (só Tech Lead e QA foram escolhidos quando perguntado "além do P.O."). 2 achados: Prioridade (Crítico/Importante/Desejável) ausente na Seção 1; campo Risco de negócio perdido no resumo. | Prioridade adicionada a todas as 24 regras; nota de abertura da Seção 1 reforçada para apontar ao campo Risco de negócio do `guardrails.md`, sem embutir o texto (evita desfazer a Ressalva 1). |
| 0.7 | Referência Dev Sênior (leve) | Usuário perguntou se faltava outra ótica (Dev Sênior/Delivery Manager). Recomendado Dev Sênior (achado provável) e dispensado Delivery Manager (sequenciamento é papel do `plan.md`, mesmo raciocínio do Ex. 2.2). Achado: `partial_response` parecia cálculo trivial no schema, mas a lógica por trás (GR-Q-05) já está sinalizada como não trivial no `guardrails.md`. | Nota de cuidado de implementação adicionada. |
| 0.8 | Revisão geral final (1ª rodada) | Reler o documento inteiro achou: inconsistência real (Seção 5 dizia que P.O. Senior revisou antes da Avaliação Final, contradizendo a própria Nota de processo da mesma seção); erro de gramática ("não só tipo" → "não só o tipo"); estilo no glossário ("raro/edge" → "exceção rara"). Usuário perguntou se as correções tinham risco de efeito cascata antes de aplicar. | Todas as 3 corrigidas, confirmado sem risco de cascata. |
| 0.9 | Revisão geral final (2ª rodada, a pedido explícito do usuário — "rode mais uma vez... se tudo estiver ok pode aprovar") | Mais 2 inconsistências reais: Seção 5 ainda citava "não é edge case", desatualizada em relação ao texto já corrigido do glossário; linha "Origem" do `guardrails.md` também desatualizada (ver Seção 3 acima). Varredura adicional por contagens desatualizadas não achou mais nada. | Ambas corrigidas. **Documento aprovado pelo usuário em 2026-08-13.** |

---

## 5. Avaliação formal contra os 4 critérios de aceite (resumo)

| Critério | Resultado |
|---|---|
| Machine-readable | Atende. IDs rastreáveis, tabelas limpas, schema Zod parseável. Ressalva consciente: depende de `guardrails.md`/`domain-model.md` para o exemplo concreto de cada regra — trade-off deliberado de um índice enxuto, mantido por decisão do usuário. |
| Regras prescritivas (DEVE/NÃO DEVE) | Atende integralmente — as 24 regras são todas imperativas. |
| Glossário útil (termos que um LLM confundiria) | Atende, após reforço de 2 entradas na revisão P.O. Senior/autoavaliação ("Pergunta multi-domínio", "Nível de confiança"). |
| Restrições de código concretas o suficiente para o Copilot | Atende — schema Zod real, paths de arquivo do Anexo C, invariante testável, fixture referenciado. Ressalva consciente: enum de confiança com lacuna conhecida (N/A), mantida por decisão do usuário. |

---

## 6. Itens em aberto — a carregar para as próximas subfases da Prática 2

Nenhum destes bloqueou a aprovação do Exercicio 2.3, mas não devem ser esquecidos:

1. **Pendência do valor de confiança "N/A"**: o `GR-D-05`/`domain-model.md` não cobrem o caso "nenhuma fonte encontrada" no enum de confiança, apesar de o `requirements.md` v0.5 (Ex. 2.1) já usar esse valor em dois Verification Criteria (VC-07, VC-08). **Bloqueio conhecido antes do `plan.md`/`tasks.md` do `query-endpoint`** — decisão consciente de não resolver nesta subfase.
2. **Proposta de path para documentos transversais**: `docs/product/guardrails.md` e `docs/product/domain-model.md` (achado Tech Lead) — sujeita à validação de um Tech Lead real antes de virar convenção do repositório; o Anexo C não define esse caminho hoje.
3. **`partial_response` (GR-Q-05)** não é um cálculo trivial — depende da mesma engenharia de extensão adaptativa de busca já sinalizada como não trivial desde o Exercicio 2.2 (referência Dev Sênior). Precisa de validação técnica real antes do `plan.md`.
4. **GR-Q-06 ("supervisor")** é um fallback genérico sem canal formal documentado no Anexo A — distinto do ramal 4500/Gestão de Riscos do GR-N-05. Revisável se a NovaTech formalizar um canal próprio no futuro.
5. **Integração ao `AGENTS.md` real do repositório**: a seção aprovada nesta subfase ainda não foi inserida em `Arquivos-de-Trabalho\novatech-assistant\AGENTS.md` — decisão consciente de manter o entregável só em `Documentos-Gerados` nesta subfase (mesmo padrão de escopo do Exercicio 2.2 com o AGENTS.md).
6. Itens já em aberto desde o Exercicio 2.2 (validação técnica de Dev Sênior real para GR-D-02/N-03/D-03/Q-05/N-07, KPIs a instrumentar no `painel-web`, dependências de `teams-bot`/`painel-web` sem spec própria) continuam válidos e não foram resolvidos nesta subfase.

---

## 7. Fechamento do Exercicio 2.3

| Entregável | Arquivo | Status |
|---|---|---|
| Seção "Product Rules & Guardrails" do AGENTS.md | `agents-md-product-specialist.md` | ✅ Aprovado (v0.9) |
| Nova versão do documento de guardrails (gap analysis + correções) | `guardrails.md` | ✅ Aprovado (v0.8) |
| Nova versão do modelo de domínio (gap analysis) | `domain-model.md` | ✅ Aprovado (v0.5) |
| Documento de apoio (contexto, decisões, histórico de rodadas) | `referencias-agents.md` | ✅ Atualizado |
| Histórico de interação (este documento) | `histórico-interação.md` | ✅ Concluído |

**O entregável do Exercicio 2.3 está completo, com fechamento e aprovação confirmados explicitamente pelo usuário em 2026-08-13.**
