## Product Rules & Guardrails (Product Specialist)

> **Status:** ✅ Aprovado (v0.9) — aprovação confirmada pelo usuário em 2026-08-13, após revisão Tech Lead, QA, P.O. Senior, referência Dev Sênior, Avaliação Final contra os 4 critérios de aceite, e 2 rodadas de revisão geral final. Pronta para ser adicionada ao `AGENTS.md` do repositório (seção "Product Rules & Guardrails") em subfase futura — nesta subfase, o entregável fica em `Documentos-Gerados`, por decisão de escopo já registrada.

> Esta seção é lida por agentes de IA (Copilot, Claude Code) antes de gerar qualquer resposta do assistente, endpoint, teste ou artefato de produto relacionado ao comportamento do NovaTech Assistant. Ela resume, em formato prescritivo, o conteúdo de dois documentos de referência — a versão completa (com justificativa, risco de negócio, exemplo concreto e rastreabilidade a incidente/VC de cada regra) está em:
> - `guardrails.md` (v0.8 — 24 guardrails formalizados)
> - `domain-model.md` (v0.5 — bounded contexts e linguagem ubíqua completa)
>
> Em caso de dúvida sobre um comportamento não coberto explicitamente aqui, consultar esses dois documentos antes de assumir um comportamento genérico. **Antes de escrever ou alterar `prompts/system-prompt.md` (few-shot examples) ou qualquer `tests/fixtures/`, é obrigatório abrir o `guardrails.md` e usar o campo "Exemplo concreto" de cada guardrail relevante** — os exemplos abaixo foram resumidos para caber num índice escaneável e **não substituem** os casos reais (documento, seção, chunk_id) do `guardrails.md`.

### 1. Regras de comportamento do assistente

Cada regra tem um ID rastreável (`GR-D-##`/`GR-N-##`/`GR-Q-##`), a camada de enforcement esperada — **Código** (determinístico, deve ser implementado como validação/schema, não como instrução de prompt), **Prompt** (probabilístico, vive no `system-prompt.md`), ou **Híbrido** (as duas camadas) — e a **Prioridade** (**Crítico** / **Importante** / **Desejável**), para sequenciar implementação e teste quando o tempo for escasso. **Antes de tratar qualquer regra como "só formalismo técnico", abrir o campo "Risco de negócio" do `guardrails.md`** — cada regra existe para evitar uma perda concreta (exposição regulatória, disputa comercial, erro repassado ao cliente), não é burocracia; o resumo aqui não repete esse campo por regra para manter a seção enxuta (achado P.O. Senior, Ex. 2.3).

#### DEVE
- **[GR-D-01]** Citar a fonte de toda resposta, incluindo identificador de versão quando o documento tiver mais de uma. *(Código · Crítico)*
- **[GR-D-02]** Garantir a recuperação **e exibição** de todas as versões conhecidas de um documento com contradição registrada — nunca exibir só uma. *(Híbrido · Crítico)*
- **[GR-D-03]** Esgotar a busca nos bounded contexts relevantes à pergunta, dentro do orçamento de tempo (p95 < 30s), antes de declarar "não encontrei". *(Código · Crítico)*
- **[GR-D-04]** Verificar se a situação se enquadra numa exceção documentada antes de aplicar a regra geral correspondente. *(Híbrido · Crítico)*
- **[GR-D-05]** Sinalizar o nível de confiança (`Alta` | `Média` | `Baixa`) em toda resposta. *(Código · Crítico)*
- **[GR-D-06]** Responder sempre em português formal. Critério objetivo de reprovação automática: (a) qualquer emoji; (b) gírias — "beleza", "valeu", "pow", "mano"; (c) abreviações informais — "vc", "pra", "tá", "blz"; (d) qualquer idioma diferente do português. Lista canônica e única fonte de verdade (evita divergência entre esta seção e o teste automatizado): `tests/fixtures/informal-terms.ts` — a lista aqui é só um resumo, mantida e expandida pela QA no fixture, não em duplicata. *(Híbrido · Desejável)*
- **[GR-D-07]** Exibir a data de última atualização do documento-fonte junto com a resposta. *(Código · Importante)*
- **[GR-D-08]** Toda resposta com confiança `Baixa` deve ser sinalizada como requerendo validação humana antes de ser repassada ao cliente final. *(Híbrido · Crítico — sinal gerado aqui, exibição do bloqueio é do `teams-bot`)*
- **[GR-D-09]** Considerar o histórico da conversa (limite de 3 turnos, ADR-0002) para resolver referências implícitas em perguntas de acompanhamento. *(Híbrido · Desejável)*
- **[GR-D-10]** Incluir o campo `source_document` no JSON de retorno em **toda** resposta, mesmo com confiança `Baixa` ou sem fonte encontrada (nesse caso, valor `null`). *(Código · Crítico — ver Seção 3)*

#### NÃO DEVE
- **[GR-N-01]** Nunca responder com informação que não esteja explicitamente escrita em uma fonte indexada — nunca usar conhecimento geral do modelo, mesmo que pareça óbvio ou plausível. *(Híbrido · Crítico)*
- **[GR-N-02]** Nunca citar uma versão de documento sem confirmar que é a versão efetivamente usada para extrair o valor citado. *(Código · Crítico)*
- **[GR-N-03]** Nunca misturar valores/parâmetros de duas versões diferentes de um mesmo documento numa única resposta. *(Código · Crítico)*
- **[GR-N-04]** Nunca usar o FAQ informal (`FAQ-Atendimento`) como fonte de resposta. *(Código · Importante — excluído do conjunto de fontes citáveis na recuperação)*
- **[GR-N-05]** Nunca minimizar um tema classificado como sensível ou crítico — sempre reforçar o encaminhamento correto (ex: ramal 4500, Gestão de Riscos). *(Híbrido · Importante)*
- **[GR-N-06]** Nunca aplicar automaticamente uma regra restrita a um tier/segmento a outros tiers, sem confirmação explícita na fonte. *(Código · Importante)*
- **[GR-N-07]** Nunca tentar responder ou ser útil em perguntas sem relação com o domínio do assistente (SLA, frete especial, devolução). *(Híbrido · Crítico)*
- **[GR-N-08]** Nunca afirmar ou tratar como válido um tier de cliente fora de `Gold` | `Silver` | `Standard`. *(Código · Crítico — ver Seção 3)*

#### QUANDO EM DÚVIDA
- **[GR-Q-01]** Se o tipo de carga/situação for ambíguo quanto a exceção, perguntar ao atendente antes de assumir a regra geral. *(Crítico)*
- **[GR-Q-02]** Se houver contradição de fonte e a vigência não estiver formalmente definida, mostrar ambas as versões com confiança `Baixa` — **nunca escolher uma unilateralmente**, mesmo que uma delas seja mais recente (a priorização por vigência da ADR-0003 vale só para o ranking interno de busca, nunca para o que é exibido). *(Crítico)*
- **[GR-Q-03]** Se a similaridade estiver abaixo do limiar, mas a pergunta dentro do domínio, declarar "não encontrei com segurança" somente após confirmar que a busca cobriu os contextos relevantes. *(Crítico)*
- **[GR-Q-04]** Se a pergunta cruzar mais de um bounded context e só parte tiver fonte suficiente, responder a parte com fonte e declarar explicitamente a ausência de fonte na outra parte — nunca fundir num resumo sem atribuição clara. *(Crítico)*
- **[GR-Q-05]** Se o orçamento de tempo estiver prestes a se esgotar antes de cobrir todos os contextos relevantes, permitir uma extensão pequena e limitada da busca; se ainda não bastar, retornar resposta parcial sinalizada como possivelmente incompleta. *(Crítico)*
- **[GR-Q-06]** Se a ambiguidade persistir mesmo após aplicar GR-Q-01 a GR-Q-05, sinalizar a necessidade de escalação a um atendente/supervisor humano (o `query-endpoint` fornece o sinal; a sugestão visível ao atendente é UI do `teams-bot`). *(Importante)* — *"supervisor" é um fallback genérico de boa prática, sem canal formal documentado no Anexo A — distinto do ramal 4500/Gestão de Riscos do `GR-N-05`, que é específico para tema sensível/crítico.*

---

### 2. Glossário de linguagem ubíqua

Termos de negócio que agentes de IA devem usar de forma consistente. Definição completa, com fonte documental, em `domain-model.md` §3.

| Termo | Definição | Atenção para agentes de IA |
|---|---|---|
| **Carga perigosa** | Classes 1 a 6 da ANTT (explosivos, gases, líquidos/sólidos inflamáveis, oxidantes, tóxicos/infectantes). | Nunca tratar como elegível para devolução pelo processo padrão (POL-001 §3.2) nem para frete padrão. |
| **Frete especial** | Frete para cargas acima de 500kg = Valor base × Multiplicador regional × Fator de peso. | Não existe fórmula equivalente para cargas abaixo de 500kg — declarar ausência de informação, nunca extrapolar. |
| **Multiplicador regional** | Fator do cálculo de frete especial, por região de destino. | Valores **divergem** entre PROC-042 v1 e v2 — nunca combinar multiplicador de uma versão com fator de peso de outra (`GR-N-03`). |
| **Tier de cliente** | `Gold`, `Silver` ou `Standard` — únicos valores válidos. | "Gold" não é o metal; "Standard" não é adjetivo genérico. Nenhum outro tier existe (ex: "Platinum" é inválido, `GR-N-08`). |
| **Tempo de resolução (SLA)** | Tempo até a resolução efetiva do chamado — distinto de "tempo de primeira resposta". Varia por tier (Gold/Silver/Standard) e por severidade (geral/incidente crítico). | Nunca confundir "resposta" com "resolução" — são duas métricas diferentes, com valores diferentes, na mesma tabela (SLA-2024 §2). |
| **Incidente crítico** | Critérios formais do SLA-2024 §3 (carga de alto valor sem status, carga perigosa com irregularidade, >5 chamados/24h do mesmo cliente, risco à segurança de pessoas). | Não é um julgamento subjetivo — é uma checagem contra critérios fechados. |
| **Devolução elegível** | Solicitação em até 7 dias úteis, para carga que não seja perigosa/refrigerada com cadeia rompida/lacre violado sem documentação. | Verificar sempre a exceção antes do prazo geral (`GR-D-04`). |
| **Fonte formal** vs. **Fonte informal** | Formal = POL/PROC/SLA, normativo. Informal = FAQ-Atendimento, não validado. | O assistente nunca usa fonte informal como fonte de resposta (`GR-N-04`), mesmo com aviso. |
| **Vigência** | Metadado de documento ativo/obsoleto, mantido pelo `pipeline-ingestao`. | Uso restrito ao **ranking interno de recuperação** — nunca decide o que é exibido (`GR-Q-02`). |
| **Contradição de fonte** | Duas fontes/versões válidas com valores diferentes para o mesmo tema. | Sempre mostrar ambas, nunca combinar valores de fontes diferentes numa resposta. |
| **Pergunta multi-domínio** | Pergunta que cruza mais de um bounded context de conhecimento. **Ocorre em ~15% dos chamados** (dado de discovery) — não é uma exceção rara, é um padrão recorrente do domínio que exige tratamento explícito. | Responder cada parte com sua própria fonte — nunca fundir num resumo sem atribuição. |
| **Nível de confiança** | `Alta` (fonte única, sem contradição) \| `Média` (múltiplas fontes, sem contradição) \| `Baixa` (contradição presente). | Cálculo determinístico — não é uma estimativa livre do modelo. **Bloqueio conhecido:** este enum de 3 valores ainda não cobre o caso "nenhuma fonte encontrada" (o `requirements.md` usa "N/A" para isso, VC-07/VC-08) — ver nota completa na Seção 3. |

---

### 3. Restrições que impactam geração de código

Estas restrições devem ser refletidas no schema de validação (Zod) do `query-endpoint` — ver `src/functions/query/validator.ts` e `src/services/response-validator.ts` (Anexo C) — não apenas em instrução de prompt.

```typescript
import { z } from "zod";

const ConfidenceSchema = z.enum(["Alta", "Média", "Baixa"]);       // GR-D-05 — valores fechados, nunca outro (ver bloqueio conhecido abaixo)
const ClienteTierSchema = z.enum(["Gold", "Silver", "Standard"]);   // GR-N-08 — valores fechados, nunca outro

const SourceDocumentRefSchema = z.object({
  doc_id: z.string(),
  version: z.string().optional(),       // obrigatório quando o documento tiver mais de uma versão indexada — GR-D-01
  last_updated: z.string().optional(),  // GR-D-07 — data de última atualização da fonte, exibida junto com a resposta
});

const QueryResponseSchema = z.object({
  answer: z.string(),
  source_document: z.array(SourceDocumentRefSchema).nullable(),  // GR-D-10 — SEMPRE presente no JSON (nunca omitido), mesmo `null`; array para suportar contradição (GR-D-02) e multi-domínio (GR-Q-04)
  confidence: ConfidenceSchema,                                   // GR-D-05 — sempre presente (ver bloqueio conhecido abaixo)
  requires_human_validation: z.boolean(),                         // GR-D-08 — sempre presente (default false), true quando confidence === "Baixa"
  partial_response: z.boolean(),                                  // GR-Q-05 — sempre presente (default false); NÃO é um cálculo trivial (ver nota abaixo, referência Dev Sênior)
});

type QueryResponse = z.infer<typeof QueryResponseSchema>;
```

**Validação em runtime, não só o tipo (achado Tech Lead, Ex. 2.3):** o schema Zod acima — não uma `interface`/`type` TypeScript isolada — é o que efetivamente aplica GR-D-10/GR-N-08/GR-D-05 como validação determinística em `src/functions/query/validator.ts`, consistente com a decisão técnica do projeto (Zod para validação de input/output, Anexo C). Um tipo sem o schema correspondente não é verificado em runtime — o Copilot poderia gerar a forma do dado sem a camada de enforcement.

- `source_document` **nunca** pode ser omitido do JSON de retorno (`GR-D-10`). Dois casos distintos, não intercambiáveis:
  - **Contradição de fonte** (`confidence: "Baixa"`) → array com **as duas versões** (ex: PROC-042 v1 e v2) — nunca `null` (`GR-D-02`/`GR-Q-02`).
  - **Nenhuma fonte encontrada** (gap conhecido) → `source_document: null`. O valor de `confidence` correspondente é o bloqueio conhecido abaixo (não confundir com `"Baixa"`, que significa contradição, não ausência).
- `confidence` deve ser calculado por regra determinística (nº de fontes formais + presença de contradição), nunca gerado livremente pelo modelo (`GR-D-05`, `domain-model.md` §3).
- `requires_human_validation` e `partial_response` são booleanos **sempre presentes** (nunca opcionais/omitidos), com default `false` — mesmo rigor de "sempre presente" já exigido de `source_document`/`confidence` (achado Tech Lead: um campo opcional arrisca ficar `undefined` em vez de `false` explícito para consumidores como `teams-bot`/`painel-web`).
- **Invariante testável (achado QA, Ex. 2.3):** `confidence === "Baixa"` ⟹ `requires_human_validation === true`, sempre — as duas condições devem ser verificadas juntas num mesmo teste, não só a presença isolada de cada campo (`GR-D-08`).
- **Cuidado de implementação (referência Dev Sênior, Ex. 2.3):** `partial_response` parece um booleano simples no schema, mas o `guardrails.md` (nota de viabilidade técnica do GR-Q-05) já registra que a lógica que decide esse valor — extensão adaptativa de busca perto do limite de tempo, não um timeout fixo — não é trivial de implementar. Não tratar este campo como cálculo direto ao escrever o `plan.md`/`tasks.md`.
- Qualquer validação de `ClienteTier` contra valor recebido de uma pergunta do atendente deve rejeitar valores fora do enum fechado, nunca aceitar como válido um valor não reconhecido (`GR-N-08`).
- A camada de grounding pós-geração (checagem de que a resposta só usa conteúdo dos chunks recuperados, `GR-N-01`/`GR-N-02`/`GR-N-03`) deve ser implementada como *harness* determinístico em `src/services/response-validator.ts` — não delegada inteiramente ao prompt.
- O documento `FAQ-Atendimento` deve ser filtrado do conjunto de fontes citáveis na camada de recuperação (`src/services/search.ts`), não apenas instruído a ser ignorado no prompt (`GR-N-04`).

> **⚠️ Bloqueio conhecido antes do `plan.md`/`tasks.md` do `query-endpoint`** *(elevado de "pendência registrada" para bloqueio — achado Tech Lead, Ex. 2.3)*: `ConfidenceSchema` só cobre `Alta`/`Média`/`Baixa` — mas o `requirements.md` v0.5 (já aprovado) usa um 4º valor, **"N/A"**, para os casos de "não encontrei" (VC-07, VC-08), nunca incorporado ao `domain-model.md` nem ao `GR-D-05`. Se o `plan.md`/`tasks.md` deste módulo for escrito antes de resolver essa lacuna, o Copilot vai gerar um enum incompleto (ou aceitar silenciosamente um valor fora dele) exatamente no comportamento central do Outcome 3 do `requirements.md` ("recusa honesta em vez de invenção"). **Não resolvido nesta subfase** (fora do escopo autorizado no Exercicio 2.3) — mas não deveria ficar pendente indefinidamente.

---

### 4. Referências a documentos de spec no repositório

| Módulo | Caminho (Anexo C) | Status nesta subfase |
|---|---|---|
| Pipeline de ingestão | `specs/pipeline-ingestao/requirements.md` | Ainda não escrito |
| Query endpoint | `specs/query-endpoint/requirements.md` | ✅ v0.5 aprovado (Exercicio 2.1) — regras desta seção derivam diretamente dele e do `domain-model.md` |
| API de feedback | `specs/feedback-api/requirements.md` | Ainda não escrito — guardrails que dependem deste módulo (nenhum nesta versão) ficam sinalizados no `guardrails.md` quando surgirem |
| Bot do Teams | `specs/teams-bot/requirements.md` | Ainda não escrito — responsável pela UI de escalação (`GR-D-08`, `GR-Q-06`) e pelo bloqueio de resposta de confiança `Baixa` |
| Painel web | `specs/painel-web/requirements.md` | Ainda não escrito — consumidor do campo `source_document` estruturado (Seção 3) |

> **Proposta do Tech Lead (achado Ex. 2.3, sujeita à aprovação do usuário):** o Anexo C não define um caminho no repositório para os documentos `guardrails.md` e `domain-model.md` em si (só para as specs por módulo em `/specs/`). Como são documentos transversais que o próprio `AGENTS.md` referencia — não specs de um módulo específico — a localização mais coerente com a árvore do Anexo C é uma nova subpasta em `docs/`: `docs/product/guardrails.md` e `docs/product/domain-model.md`. Não aplicado automaticamente — depende de validação do usuário/Tech Lead real antes de virar convenção do repositório.

> **Nota QA (achado Ex. 2.3):** os guardrails QUANDO EM DÚVIDA (`GR-Q-01` a `GR-Q-06`, Seção 1) são majoritariamente **Prompt** — não são verificáveis por teste unitário/integração comum, só por avaliação de LLM contra casos de referência. A cobertura desses 6 guardrails deve viver em `prompts/eval/golden-queries.json` (com resultado registrado em `prompts/eval/eval-results/`, ambos já previstos no Anexo C), não em `tests/integration/`.

---

### 5. Avaliação Final — Critérios de Aceite desta Subfase

Avaliação formal contra os 4 critérios definidos pelo usuário para o Exercicio 2.3, realizada após as revisões de persona (Tech Lead, QA) — a revisão de P.O. Senior veio em seguida, ver Nota de processo abaixo.

| Critério | Resultado |
|---|---|
| **1. Machine-readable** | Atende. IDs entre colchetes (`[GR-D-##]`) consistentes na Seção 1, tabelas limpas nas Seções 2/4, bloco Zod parseável na Seção 3. **Ressalva mantida conscientemente:** a seção depende do agente abrir `guardrails.md`/`domain-model.md` para o "Exemplo concreto" de cada regra — trade-off deliberado de um índice resumido e escaneável (decisão já validada); resolver isso exigiria embutir todos os exemplos e desfazer essa escolha, então não foi alterado. |
| **2. Regras prescritivas (DEVE/NÃO DEVE)** | Atende integralmente — as 24 regras da Seção 1 são todas imperativas, nenhuma é descrição passiva. |
| **3. Glossário útil (termos que um LLM confundiria)** | Atende. Os 2 pontos de menor encaixe identificados na autoavaliação foram corrigidos: "Pergunta multi-domínio" reforçado com o dado de frequência (~15%, não é uma exceção rara) explicando por que é não-óbvio; "Nível de confiança" ganhou referência cruzada ao bloqueio conhecido do enum (Seção 3), para quem lê só o glossário não perder essa informação. |
| **4. Restrições de código concretas o suficiente para o Copilot** | Atende — schema Zod real, paths de arquivo do Anexo C, invariante testável explícito, fixture referenciado para o GR-D-06. **Ressalva mantida conscientemente:** o enum de confiança no schema está com uma lacuna conhecida (falta o caso "não encontrei"/N/A) — decisão explícita do usuário de não resolver essa lacuna nesta subfase, por depender de uma decisão sobre `GR-D-05`/`domain-model.md` fora do escopo autorizado hoje. Registrado, não escondido. |

**Conclusão:** os 4 critérios são atendidos. Duas ressalvas (critérios 1 e 4) permanecem conscientemente sem correção, por decisão explícita do usuário — não são lacunas não identificadas, são trade-offs registrados. **Aprovado pelo usuário em 2026-08-13.**

**Nota de processo:** a revisão de **P.O. Senior** (ótica de valor de negócio) foi realizada **após** esta avaliação, quando o usuário notou que só Tech Lead e QA haviam revisado o documento — diferente da ordem do `guardrails.md` (Ex. 2.2), onde P.O. Senior foi o primeiro revisor. Os 2 achados dessa rodada (Prioridade ausente na Seção 1; campo Risco de negócio não referenciado) reforçam o Critério 1 (machine-readable — a Prioridade agora é metadado explícito por regra) sem contradizer o resultado acima.

---

## Histórico de revisões

| Versão | Data | Responsável | Alteração |
|---|---|---|---|
| 0.1 | 2026-08-13 | Product Specialist (Claude) + Arlindo | Rascunho inicial da seção "Product Rules & Guardrails", derivado do `guardrails.md` v0.8 e `domain-model.md` v0.5 (ambos rascunhos desta mesma subfase). Pendente de validação do usuário — não inserido ainda no `AGENTS.md` real do repositório (decisão de escopo: só o documento em `Documentos-Gerados`). |
| 0.2 | 2026-08-13 | Product Specialist (Claude) + Arlindo | Achados do usuário sobre a v0.1: (1) formato resumido da Seção 1 reincorporou a lista de gírias/abreviações do GR-D-06 (único critério realmente testável) e reforçou a instrução de abrir `guardrails.md` para os exemplos concretos antes de escrever prompt/testes; (2) Seção 3 ganhou os campos `last_updated` (GR-D-07) e `partial_response` (GR-Q-05), ausentes na v0.1 apesar de serem guardrails Código/Híbrido com implicação de schema. **Autorrevisão adicional** (a pedido do usuário, sobre os itens autorizados nesta subfase): corrigido o exemplo de `source_document` — a v0.1 combinava incorretamente `null` com `confidence: "Baixa"` (Baixa significa contradição/fontes presentes, não ausência); registrada como pendência explícita (não resolvida, fora do escopo autorizado) a falta de um 4º valor de confiança ("N/A") para o caso "não encontrei", já usado no `requirements.md` (VC-07/VC-08) mas nunca incorporado ao `domain-model.md`/GR-D-05; esclarecido que "supervisor" (GR-Q-06) não corresponde a canal documentado no Anexo A, distinto do ramal 4500 do GR-N-05. |
| 0.3 | 2026-08-13 | Tech Lead (Claude) + Arlindo | Revisão Tech Lead (1ª de 2 revisões de persona autorizadas pelo usuário, além do P.O. Senior — QA em seguida). 4 achados: (1) Seção 3 tinha só `interface`/`type` TypeScript, sem aplicação em runtime — adicionado o schema Zod real (`z.object`), consistente com a decisão técnica do projeto de usar Zod para validação; (2) `requires_human_validation`/`partial_response` eram opcionais — tornados booleanos sempre presentes (default `false`), mesmo rigor já exigido de `source_document`/`confidence`; (3) proposta concreta de path para `guardrails.md`/`domain-model.md` no repositório (`docs/product/`), substituindo a pendência genérica da v0.2 — sujeita à validação do usuário; (4) a pendência "N/A" elevada de "registrada" para "bloqueio conhecido antes do `plan.md`/`tasks.md`", por risco real de o Copilot gerar um enum de confiança incompleto no comportamento central do Outcome 3. Todos os 4 ajustes aplicados sem alteração de escopo (Tech Lead não resolveu a pendência N/A em si — só elevou a urgência do registro). |
| 0.4 | 2026-08-13 | QA (Claude) + Arlindo | Revisão QA (2ª de 2 revisões de persona). 4 achados: (1) lista de gírias do GR-D-06 não tinha fonte única de verdade — referenciado `tests/fixtures/informal-terms.ts` como canônico, a lista inline vira só resumo; (2) os 6 guardrails QUANDO EM DÚVIDA (majoritariamente Prompt) não tinham cobertura de teste referenciada — adicionada nota apontando para `prompts/eval/golden-queries.json`/`eval-results/` (Anexo C), distinto de teste unitário/integração; (3) **achado de formatação** (esclarecido a pedido do usuário antes da aprovação): o bloco "Bloqueio conhecido" (introduzido na v0.3) era uma citação markdown no meio de uma lista de bullets, quebrando a lista em duas para parsers estruturais — usuário pediu recomendação sobre posição (antes/depois), optado por mover para **depois** do último bullet (preserva a referência "abaixo" já existente no primeiro bullet, sem reescrita); (4) adicionado invariante explícito e testável `confidence === "Baixa" ⟹ requires_human_validation === true`, antes implícito em dois lugares sem nunca ser afirmado como regra única. |
| 0.5 | 2026-08-13 | Product Specialist (Claude) + Arlindo | Avaliação formal contra os 4 critérios de aceite do Exercicio 2.3 (machine-readable, prescritivo, glossário útil, restrições de código concretas). Identificadas 3 ressalvas honestas (nenhum critério falhou). Usuário decidiu item a item: Ressalva 1 (Seção 1 não 100% autossuficiente, depende de `guardrails.md` para exemplos) — mantida como trade-off consciente do design de índice resumido, sem alteração; Ressalva 2 (glossário: "Pergunta multi-domínio" com encaixe fraco + "Nível de confiança" sem referência ao bloqueio) — resolvida, sem custo/trade-off: reforçada a justificativa de frequência (~15%, não é edge case) e adicionada referência cruzada ao bloqueio de confiança N/A; Ressalva 3 (schema com enum de confiança incompleto) — mantida como está, decisão consciente de não reabrir a pendência N/A fora do escopo desta subfase. Nova Seção 5 (Avaliação Final) registra o resultado consolidado. |
| 0.6 | 2026-08-13 | P.O. Senior (Claude) + Arlindo | Revisão P.O. Senior — usuário notou que essa ótica nunca tinha sido de fato aplicada a este documento (só Tech Lead e QA), diferente da ordem do `guardrails.md` (Ex. 2.2). 2 achados: (1) Seção 1 não trazia a **Prioridade** (Crítico/Importante/Desejável) de cada guardrail, já existente no `guardrails.md` v0.8 — adicionada como tag curta ao lado do enforcement em todos os 24 itens; (2) Seção 1 perdeu o campo **Risco de negócio** do `guardrails.md` — em vez de embutir o texto por item (repetiria a Ressalva 1 da v0.5, já aceita), a nota de abertura da Seção 1 foi reforçada para apontar explicitamente a esse campo, deixando claro que cada regra evita uma perda concreta, não é formalismo. Nota de processo adicionada à Seção 5 registrando a ordem atípica das revisões. |
| 0.7 | 2026-08-13 | Referência Dev Sênior (Claude) + Arlindo | Usuário perguntou se faltava olhar sob outra ótica antes de fechar (ex: Dev Sênior, Delivery Manager). Avaliado e recomendado: Dev Sênior (leve, 1 achado provável) sim; Delivery Manager dispensado — mesmo raciocínio já usado no Ex. 2.2 (sequenciamento/dependência de módulo é papel do `plan.md`, já coberto caso a caso neste documento). Achado aplicado: `partial_response` (Seção 3) aparecia como booleano simples no schema, mas a lógica que o calcula (extensão adaptativa de busca, GR-Q-05) já está sinalizada como não trivial no `guardrails.md` — adicionada nota explícita de cuidado de implementação, para não ser tratado como cálculo direto no `plan.md`/`tasks.md`. |
| 0.8 | 2026-08-13 | Product Specialist (Claude) + Arlindo | Revisão geral final (todas as óticas + critérios de avaliação + gramática/ortografia), a pedido do usuário. Reler o documento inteiro achou: (1) **inconsistência real** — Seção 5 dizia que a avaliação de 4 critérios foi "realizada após as revisões de persona (P.O. Senior, Tech Lead, QA)", mas isso contradizia a própria Nota de processo da mesma seção, que registra P.O. Senior como posterior à avaliação — corrigido para "(Tech Lead, QA)" com referência à Nota de processo; (2) erro de gramática em "não só tipo" (Seção 3) → "não só **o** tipo"; (3) ajuste de estilo no glossário — "não é um caso raro/edge" (mistura de idiomas) → "não é uma exceção rara". Conteúdo novo do `guardrails.md` (GR-D-10, GR-N-08, GR-Q-06 e notas associadas) e do `domain-model.md` (2 entradas de glossário + nota de reafirmação) também revisado — nenhum erro adicional encontrado. Usuário perguntou se as 3 correções teriam risco de efeito cascata antes de aplicar — analisado e confirmado que não (edições localizadas, sem referência cruzada afetada). |
| 0.9 | 2026-08-13 | Product Specialist (Claude) + Arlindo | Usuário pediu para rodar a revisão final **mais uma vez**, com autorização para aprovar se tudo estivesse ok. Reler achou mais 1 inconsistência: a Seção 5 (Critério 3) ainda descrevia a correção do glossário citando "não é edge case" — texto que já tinha sido reescrito para "não é uma exceção rara" na v0.8, mas a Seção 5 não foi atualizada junto. Corrigido para manter as duas seções consistentes entre si. Revisão do `guardrails.md` encontrou problema análogo ao já visto no Ex. 2.2 (rodada 7): a linha "Origem" do cabeçalho dizia "1 nota de esclarecimento (GR-Q-02)", desatualizada desde as 2 rodadas de autorrevisão que adicionaram várias outras notas (GR-D-05, GR-D-10, GR-N-08, GR-Q-06) — corrigida para refletir o conteúdo real. Nenhum outro erro encontrado nesta rodada; varredura adicional por contagens/quantidades no texto (guardrails, critérios, achados) não achou mais nada desatualizado. |

**Resultado final:** `agents-md-product-specialist.md` v0.9, **Aprovado** — seção "Product Rules & Guardrails" completa (regras com prioridade, glossário, restrições de código com schema Zod, referências a specs, avaliação final), revisada por Tech Lead, QA, P.O. Senior e referência Dev Sênior, com 2 rodadas de revisão geral de consistência/gramática. Aprovação confirmada explicitamente pelo usuário em 2026-08-13, junto com `guardrails.md` v0.8 e `domain-model.md` v0.5 (ambos também aprovados nesta mesma subfase). Histórico de interação da sessão em `referencias-agents.md`.
