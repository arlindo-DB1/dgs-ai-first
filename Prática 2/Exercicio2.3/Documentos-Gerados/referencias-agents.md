# Referências — Seção "Product Rules & Guardrails" do AGENTS.md (Novatech)

> Arquivo de apoio para consulta durante a construção do `agents-md-product-specialist.md`. Equivalente, para esta subfase, ao `referencias-spec.md` (Exercicio 2.1) e ao `referencias-guardrails.md` (Exercicio 2.2) — nome próprio para não confundir os três.
> Atualizado à medida que o material for recebido e o contexto for refinado com o Product Specialist Senior.

## Como usar este arquivo
- Cada material recebido é registrado na seção **Índice de Materiais**, com caminho, resumo curto e tags.
- Decisões tomadas durante o refinamento vão em **Decisões**.
- Perguntas ainda sem resposta ficam em **Dúvidas em Aberto** até serem resolvidas (aí migram para Decisões).
- Observações e aprendizados que não se encaixam nas seções acima vão em **Notas de Refinamento**.
- Rascunhos ainda não aprovados (candidatos a virar conteúdo do `agents-md-product-specialist.md`) ficam em **Rascunho em Avaliação**, separados do que já foi decidido.

---

## 1. Contexto Geral do Projeto (trazido do `referencias-guardrails.md`, Exercicio 2.2)

- **Projeto:** Novatech
- **Fase anterior (Cenário 1 / Prática 1):** Discovery + Entendimento (concluída).
- **Fase atual (Cenário 2 / Prática 2):** Estruturação do Trabalho.
- **Subfase Exercicio 2.1 (concluída):** recorte de domínio (`domain-model.md`, v0.4, aprovado) + `requirements.md` do `query-endpoint` (v0.5, aprovado) + mockup + histórico de iteração.
- **Subfase Exercicio 2.2 (concluída):** `guardrails.md` v0.7, aprovado — 21 guardrails (9 DEVE, 7 NÃO DEVE, 5 QUANDO EM DÚVIDA), com enforcement, risco de negócio, prioridade, bounded context, exemplo concreto, rastreabilidade a incidente/VC.
- **Subfase atual (Exercicio 2.3):** contribuição do Product Specialist para o `AGENTS.md` do projeto — seção "Product Rules & Guardrails".
- **Ferramentas disponíveis:** Claude (chat, todos os papéis), GitHub Copilot (devs e Tech Lead), Claude Cowork (Delivery Manager, Product Specialist, QA), Claude Design (Product Specialist).
- **Time:** 1 Tech Lead, 2 Desenvolvedores (1 pleno, 1 sênior), 1 QA, 1 Product Specialist, 1 Delivery Manager.
- **Repositório:** `novatech-assistant` (prefixo `db1/` é narrativo) — Git **local** nesta fase, sem remoto/GitHub. Existe uma cópia de trabalho real em `Prática 2\Arquivos-de-Trabalho\novatech-assistant\`, com `AGENTS.md` presente mas todas as seções ainda `TODO`.

### Decisões já fechadas em fases/subfases anteriores (não rediscutir, usar como premissa)
- Mesma lista consolidada no `referencias-guardrails.md` §1 (modelo LLM, pipeline RAG, context budget, tratamento de documentos contraditórios — vigência para uso interno de recuperação, exibição sempre mostra ambas as versões —, 5 bounded contexts, linguagem ubíqua, FAQ não é fonte, restrição à base documental).
- **`guardrails.md` v0.7** (21 guardrails) e **`domain-model.md` v0.4** são as fontes primárias e aprovadas para esta subfase.

### Princípios de trabalho (válidos para todos os artefatos desta fase)
> **"O óbvio deve ser escrito e falado."**
>
> **"Toda decisão deve passar pelo usuário."** Reforçado nesta subfase pelo usuário logo no início: nada de tomar decisão por conta própria; sempre perguntar em caso de dúvida ou necessidade de informação adicional.

---

## 2. Contexto Específico desta Subfase (Exercicio 2.3)

### Missão recebida do usuário
> "O Tech Lead está montando o AGENTS.md e pediu que cada papel contribua com sua seção."

### Inputs fornecidos pelo usuário
- O cenário completo (ver seção 1 acima).
- Anexo A — Documentação Simulada da NovaTech. Caminho: `Prática 2\Especificações-Exercicio\anexo-a-documentacao-simulada-novatech.md`.
- Anexo C — Estrutura do Repositório. Caminho: `Prática 2\Especificações-Exercicio\anexo-c-estrutura-repositorio.md`.
- Estrutura do AGENTS.md proposta pelo Tech Lead (7 seções; a nossa é "Product Rules & Guardrails").
- **Guardrails formalizados simulados** (fornecidos no enunciado "para autossuficiência" — 9 itens: 3 DEVE, 3 NÃO DEVE, 3 QUANDO EM DÚVIDA).

### Tarefa
Escrever a seção **"Product Rules & Guardrails"** do AGENTS.md, contendo:
1. Regras de comportamento do assistente (derivadas dos guardrails).
2. Glossário de linguagem ubíqua do domínio.
3. Restrições que impactam geração de código (ex: campo `source_document` no JSON).
4. Referências a documentos de spec no repositório.

### Entregável desta subfase
A seção do AGENTS.md pronta para ser adicionada ao repositório — **só o documento em `Documentos-Gerados`** (decisão do usuário; não editar ainda o `AGENTS.md` real do repositório de trabalho, cujas outras seções pertencem a outros papéis e seguem `TODO`).

---

## 3. Índice de Materiais

| # | Documento | Caminho | Resumo | Tags |
|---|-----------|---------|--------|------|
| A1 | Anexo A — Documentação Simulada da NovaTech | `Prática 2\Especificações-Exercicio\anexo-a-documentacao-simulada-novatech.md` | Fonte de verdade: POL-001, PROC-042 v1/v2, SLA-2024, FAQ-Atendimento. Usado para validar termos de glossário e confirmar grounding de novos guardrails. | documentação-fonte, glossário, guardrails |
| A2 | Anexo C — Estrutura do Repositório | `Prática 2\Especificações-Exercicio\anexo-c-estrutura-repositorio.md` | Estrutura de diretórios do `novatech-assistant`; convenção de nomenclatura de specs (`/specs/<módulo>/requirements.md` etc.). Usado para as referências a specs na seção 4 do entregável. | estrutura-repo, specs |
| A3 | `guardrails.md` (Exercicio 2.2) | `Prática 2\Exercicio2.2\Documentos-Gerados\guardrails.md` | v0.7, aprovado, 21 guardrails. Fonte primária das regras de comportamento — usado no lugar da versão simulada simplificada do enunciado, por decisão do usuário. | guardrails, fonte primária |
| A4 | `domain-model.md` (Exercicio 2.1) | `Prática 2\Exercicio2.1\Documentos-Gerados\domain-model.md` | v0.4, aprovado. Fonte primária do glossário de linguagem ubíqua. | domain model, glossário, fonte primária |
| A5 | `AGENTS.md` (repositório de trabalho) | `Prática 2\Arquivos-de-Trabalho\novatech-assistant\AGENTS.md` | Constitution do projeto, todas as seções ainda `TODO`. Confirma que nenhum outro papel foi simulado ainda. Não editado nesta subfase (decisão do usuário — só o documento em Documentos-Gerados). | agents-md, repositório |

---

## 4. Decisões

1. **Nome do arquivo de apoio desta subfase:** `referencias-agents.md` (este arquivo).
2. **Nome do entregável desta subfase:** `agents-md-product-specialist.md`.
3. **Fonte das regras de comportamento:** usar o `guardrails.md` real e completo (v0.7, 21 itens) como base, não a versão simulada simplificada do enunciado (9 itens) — mesmo padrão já usado nas transições 2.1→2.2. A versão simulada foi usada como checklist de verificação de cobertura (gap analysis), não como substituto.
4. **Fonte do glossário:** usar o `domain-model.md` real e completo (v0.4) como base, não só os 5 termos citados como exemplo no enunciado — mesma lógica do item 3.
5. **Escopo do entregável:** só o documento em `Documentos-Gerados`, sem editar o `AGENTS.md` real do repositório de trabalho nesta subfase.
6. **Gap analysis guardrails (enunciado simulado × `guardrails.md` v0.7):**
   - Cobertos sem alteração: DEVE 1 (GR-D-01), DEVE 3 (GR-D-06), NÃO DEVE 4 (GR-N-01), NÃO DEVE 5 (GR-D-04, sem espelho por decisão já validada no Ex. 2.2), QUANDO EM DÚVIDA 7 (coberto como DEVE — GR-D-05/GR-D-08).
   - Gaps reais, aprovados para inclusão em nova versão do `guardrails.md` (v0.8): **GR-D-10** (campo `source_document` no JSON, mesmo com confiança Baixa/sem fonte), **GR-N-08** (nunca inventar tier de cliente inexistente), **GR-Q-06** (sugerir escalação ao supervisor se ambiguidade persistir após GR-Q-01 a Q-05).
   - **Conflito identificado e resolvido (item 9 do enunciado):** "priorizar a versão mais recente" contradiz o GR-Q-02/`domain-model.md` §2.5 (sempre mostrar ambas as versões, nunca escolher uma) — e reproduz o conselho informal do FAQ item 8 do Anexo A, que é a origem do próprio Incidente 2. **Decisão do usuário:** incorporar a priorização por vigência apenas como regra de **retrieval interno** (consistente com ADR-0003), nunca como regra de exibição — sem alterar o comportamento já validado do GR-Q-02. `guardrails.md` v0.8 ganha uma nota explícita esclarecendo essa distinção (onde ainda não estava clara o suficiente).
7. **Gap analysis glossário (termos do enunciado × `domain-model.md` v0.4 §3):**
   - Cobertos sem alteração: "cliente Gold" (via "Tier de cliente"), "carga perigosa", "frete especial".
   - Gaps reais, aprovados para inclusão em nova versão do `domain-model.md` (v0.5): **"Tempo de resolução (SLA)"** e **"Multiplicador regional"** como entradas próprias na tabela de linguagem ubíqua (§3).

---

## 5. Dúvidas em Aberto

_(nenhuma até o momento)_

---

## 6. Estado Final dos Artefatos (✅ Aprovados em 2026-08-13)

| Documento | Versão | Conteúdo |
|---|---|---|
| `guardrails.md` | v0.8 ✅ | 24 guardrails (21 herdados de v0.7 + GR-D-10, GR-N-08, GR-Q-06). Passou por 3 rodadas de autorrevisão (ver Histórico de Iteração). |
| `domain-model.md` | v0.5 ✅ | Linguagem ubíqua v0.4 + 2 entradas ("Multiplicador regional", "Tempo de resolução (SLA)") + nota de reafirmação §2.5. Nenhuma correção necessária além das entradas adicionadas. |
| `agents-md-product-specialist.md` | v0.9 ✅ | Seção "Product Rules & Guardrails" do AGENTS.md — regras resumidas com Prioridade (Seção 1), glossário (Seção 2), restrições de código/schema Zod (Seção 3), referências a specs (Seção 4), Avaliação Final contra os 4 critérios de aceite (Seção 5). Passou por revisão Tech Lead + QA + avaliação final + P.O. Senior + referência leve de Dev Sênior + 2 rodadas de revisão geral final (gramática/consistência). |

**Pendência registrada, não resolvida (fora do escopo autorizado nesta subfase, sobrevive à aprovação):** o `GR-D-05` (`guardrails.md`) declara que toda resposta traz confiança `Alta`/`Média`/`Baixa`, mas o `requirements.md` v0.5 (já aprovado, Ex. 2.1) usa um 4º valor — `"N/A"` — para os casos de "não encontrei" (VC-07, VC-08), nunca incorporado à definição de confiança do `domain-model.md` nem ao GR-D-05. Achado ao construir o exemplo do GR-D-10 (Ex. 2.3); registrado para decisão futura do usuário — **não bloqueou a aprovação**, por decisão explícita do usuário de mantê-lo como está.

---

## 7. Histórico de Iteração

| Rodada | Feedback do usuário | Ajuste aplicado |
|---|---|---|
| 1 | Usuário pediu para não investigar arquivos nem fazer perguntas antes de terminar de passar o material ("Quem te mandou sair fazendo as coisas? Ainda estou te passando o material"). | Interrompida a investigação/pergunta prematura; aguardado o fechamento completo do handover antes de qualquer pergunta de refinamento. |
| 2 | Fonte de guardrails/glossário: verificar cobertura do enunciado simulado contra os documentos reais (v0.7/v0.4) e completar gaps em nova versão — "vamos fazer um trabalho completo". | Gap analysis completa realizada (seção 4, itens 6-7). |
| 2 | Conflito do item 9 (vigência): incorporar como regra de retrieval interno, nunca de exibição. | Registrado como decisão (seção 4, item 6); aplicado no `guardrails.md` v0.8 (nota em GR-Q-02). |
| 2 | Aprovados os 4 itens novos (GR-D-10, GR-N-08, GR-Q-06, 2 entradas de glossário) em bloco. | Aplicados nas novas versões do `guardrails.md` (v0.8) e `domain-model.md` (v0.5). |
| 3 | Usuário interrompeu duas vezes ("Ow, perai" / "Para ai, o que está fazendo?") ao perceber que o Claude ia atualizar os arquivos originais (Exercicio2.1/2.2) em vez de criar novas versões em `Exercicio2.3\Documentos-Gerados`. | Confirmado que os originais (v0.7/v0.4) permanecem intactos; `guardrails.md` e `domain-model.md` recriados como arquivos novos e independentes na pasta correta. |
| 4 | Usuário pediu autorrevisão das inclusões ("verifique se sua inclusão está precisando de algum complemento"). | Achados reais: referência falsa do GR-D-10 a "VC-02" (na verdade sobre outro cenário); GR-N-08 na verdade já tinha VC direto (VC-05), elevando sua prioridade para Crítico; faltava nota de dependência de módulo no GR-Q-06 (escalação é UI do `teams-bot`); faltava referência cruzada à "Reconciliação decidida" do `requirements.md` §4 no GR-Q-02. Todos corrigidos no `guardrails.md` v0.8. `domain-model.md` v0.5 conferido sem erros. |
| 5 | Usuário aprovou o `agents-md-product-specialist.md` v0.1, mas questionou 2 pontos específicos: (a) impacto do formato resumido da Seção 1; (b) se 5 restrições na Seção 3 estava correto. | Seção 1: reincorporada a lista de gírias/abreviações do GR-D-06 (único critério testável) + reforço de instrução para abrir `guardrails.md` antes de escrever prompt/testes. Seção 3: auditados os 18 guardrails DEVE/NÃO DEVE com enforcement Código/Híbrido contra o critério "afeta schema/contrato de API" — achados 2 campos faltantes (`last_updated` do GR-D-07, `partial_response` do GR-Q-05), adicionados. `agents-md-product-specialist.md` → v0.2. |
| 6 | Usuário pediu revisão adicional dos itens já autorizados, para checar algo que pudesse ter passado despercebido. | 2 achados: (A) exemplo do GR-D-10 combinava incorretamente `source_document: null` com `confidence: "Baixa"` (Baixa = contradição, não ausência) — corrigido; ao corrigir, descoberta a pendência maior do "N/A" (ver Seção 6 acima), registrada mas não resolvida por estar fora do escopo autorizado nesta subfase. (B) "Supervisor" do GR-Q-06 não tem grounding no Anexo A (só existe o ramal 4500/Gestão de Riscos, já coberto pelo GR-N-05) — esclarecido como fallback genérico, não canal formal. Ambos corrigidos no `guardrails.md` v0.8 e refletidos no `agents-md-product-specialist.md` v0.2. |
| 7 | Usuário pediu revisão multi-persona antes da avaliação final, e perguntou qual persona chamar além do P.O. Senior. Escolha: Tech Lead **e** QA, em sequência (mesmo padrão do `guardrails.md`, Ex. 2.2, sem Delivery Manager/Dev Sênior desta vez). | Revisão Tech Lead (4 achados: schema Zod real em vez de tipo TS puro; `requires_human_validation`/`partial_response` tornados obrigatórios; proposta de path `docs/product/` para os documentos transversais; pendência "N/A" elevada a bloqueio conhecido) — todos aplicados. `agents-md-product-specialist.md` → v0.3. |
| 7 | Revisão QA em seguida: 4 achados (lista de gírias do GR-D-06 sem fonte única — referenciado fixture; guardrails QUANDO EM DÚVIDA sem cobertura de teste referenciada — apontado para `prompts/eval/golden-queries.json`; formatação quebrada do bloco "Bloqueio conhecido" no meio de uma lista; invariante `confidence===Baixa ⟹ requires_human_validation===true` implícito, nunca afirmado). Usuário pediu explicação mais detalhada do achado de formatação antes de aprovar — explicado com trecho do markdown bruto e o risco concreto para parsers estruturais; usuário pediu recomendação de posição (antes/depois do bloco) — recomendado "depois" (preserva a referência "abaixo" já existente, sem reescrita) e aplicado. | Todos os 4 achados aplicados. `agents-md-product-specialist.md` → v0.4. |
| 8 | Avaliação formal contra os 4 critérios de aceite do Exercicio 2.3. 3 ressalvas honestas identificadas (nenhum critério falhou). Usuário perguntou se as 3 eram resolvíveis sem comprometer o trabalho, pediu análise antes de decidir. | Análise apresentada: Ressalva 1 (Seção 1 não autossuficiente) — não recomendado resolver, custaria a própria decisão de design já aprovada; Ressalva 2 (glossário) — resolvível sem custo; Ressalva 3 (schema com N/A incompleto) — não resolvível sem reabrir a decisão de escopo já fechada na rodada 6. Usuário decidiu: manter Ressalva 1, resolver Ressalva 2 (ambas as partes), manter Ressalva 3. `agents-md-product-specialist.md` → v0.5, com nova Seção 5 (Avaliação Final). |
| 9 | Usuário perguntou, antes de fechar, se a ótica de **P.O. Senior** já tinha sido aplicada — não tinha (só Tech Lead e QA foram escolhidos quando perguntado "além do P.O."). Reconhecido como lacuna real, não só dúvida do usuário. Usuário pediu para rodar a revisão, "pois ele impacta". | Revisão P.O. Senior: 2 achados (Prioridade ausente na Seção 1; campo Risco de negócio perdido no resumo). Ambos aplicados — Prioridade adicionada como tag por item; Risco de negócio tratado por referência reforçada ao `guardrails.md`, não embutido (evita reabrir a Ressalva 1 já aceita). `agents-md-product-specialist.md` → v0.6. |
| 10 | Usuário perguntou se faltava olhar sob outra ótica (Dev Sênior/Delivery Manager) antes dos 2 pontos finais. | Recomendado Dev Sênior (leve) sim, Delivery Manager dispensado (mesmo raciocínio do Ex. 2.2 — sequenciamento é papel do `plan.md`). Usuário aprovou a sugestão. Achado Dev Sênior aplicado: nota de cuidado de implementação em `partial_response` (Seção 3), alertando que a lógica por trás (GR-Q-05) não é trivial, já sinalizado no `guardrails.md` mas não carregado para o AGENTS.md. `agents-md-product-specialist.md` → v0.7. |
| 11 | Segundo ponto final: revisão geral (óticas + critérios de avaliação + gramática/ortografia), mesmo padrão da rodada 7 do `guardrails.md` (Ex. 2.2). Usuário perguntou se as correções propostas teriam risco de efeito cascata antes de aplicar. | 3 achados: (1) inconsistência real — Seção 5 dizia que P.O. Senior revisou antes da avaliação de critérios, contradizendo a própria Nota de processo da seção, que registra o contrário; (2) erro de gramática ("não só tipo" → "não só o tipo"); (3) estilo no glossário ("raro/edge" → "exceção rara"). Confirmado sem risco de cascata (edições localizadas). Conteúdo novo do `guardrails.md`/`domain-model.md` também relido — sem erros adicionais. Todos os 3 aplicados. `agents-md-product-specialist.md` → v0.8. |
| 12 | Usuário pediu para rodar a revisão final **mais uma vez**, incluir mais revisão se necessário, e aprovar se tudo estivesse ok. | Mais 2 inconsistências reais encontradas: (1) Seção 5 do `agents-md-product-specialist.md` ainda citava "não é edge case", desatualizada em relação ao texto já corrigido do glossário ("não é uma exceção rara") — corrigido; (2) linha "Origem" do cabeçalho do `guardrails.md`, mesmo tipo de problema já visto no Ex. 2.2 (rodada 7) — dizia "1 nota de esclarecimento", desatualizada após as 2 rodadas de autorrevisão que adicionaram várias outras notas — corrigida. Varredura adicional por contagens/quantidades desatualizadas nos 2 documentos não achou mais nada. Concluído que os 3 documentos estavam consistentes. **Os 3 documentos foram aprovados** (autorização explícita do usuário, condicional a "se tudo estiver ok"): `guardrails.md` v0.8, `domain-model.md` v0.5, `agents-md-product-specialist.md` v0.9. |

---

## 8. Notas de Refinamento

- Esta subfase reafirmou, logo no início, o princípio já vigente desde o Exercicio 2.1: nada de decisão por conta própria, mesmo quando o Claude já investigou os arquivos por conta própria e "acha" que sabe a resposta. A lição já registrada no `referencias-guardrails.md` §8 se repetiu aqui e foi corrigida no mesmo turno.
- O arquivo de apoio foi criado assim que o material completo foi recebido e as primeiras decisões estruturais (nomes, fontes, escopo) foram fechadas — aplicando a lição já registrada no `referencias-guardrails.md` §8 sobre criar este arquivo cedo.
- **Lição nova desta subfase:** "criar nova versão" e "atualizar o arquivo existente" não são a mesma instrução, mesmo quando o conteúdo final é equivalente — o usuário interrompeu duas vezes (rodada 3) até isso ficar claro. Vale generalizar: quando o usuário pedir para versionar algo, checar se o destino é o mesmo arquivo (nova versão in-place) ou um arquivo novo em outro lugar (cópia versionada), em vez de assumir.
- **Lição nova desta subfase:** pedidos de autorrevisão ("verifique se está ok") renderam achados reais e não triviais em 2 rodadas seguidas (rodadas 4 e 6) — incluindo uma referência cruzada fabricada (VC-02) e uma pendência real em documentos já aprovados de fases anteriores (Alta/Média/Baixa vs N/A). Vale sempre re-conferir referências cruzadas contra o documento-fonte real antes de apresentar, em vez de confiar na memória do que "provavelmente" está lá — esse tipo de erro não aparece por displicência óbvia, só por checagem ativa.
