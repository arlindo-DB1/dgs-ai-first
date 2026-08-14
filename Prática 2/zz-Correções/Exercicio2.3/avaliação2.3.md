## Avaliação do Exercício 2.3 — Participação na construção do AGENTS.md (Product Specialist)

### Resumo
Entregável excepcionalmente completo e rigoroso: a seção "Product Rules & Guardrails" é genuinely machine-readable (IDs rastreáveis, schema Zod real, tabelas parseáveis), deriva de um gap analysis sério entre os guardrails simulados do enunciado e os artefatos reais já aprovados (guardrails.md v0.7, domain-model.md v0.4), e passou por múltiplas rodadas de revisão de persona (Tech Lead, QA, P.O. Senior, Dev Sênior) com achados concretos aplicados a cada rodada. O ponto mais forte é a autorrevisão: o participante encontrou e corrigiu uma referência cruzada fabricada (VC-02) e uma lacuna real herdada de fases anteriores (enum de confiança sem valor "N/A"), documentando-a como pendência consciente em vez de escondê-la.

### Scores por Dimensão

| Dimensão | Score | Justificativa |
|----------|-------|----------------|
| D1 — Domínio Conceitual | 3 | Distingue com precisão enforcement Código/Prompt/Híbrido para cada regra, entende AGENTS.md como artefato consumido por agentes antes de qualquer geração, e conecta glossário a bounded contexts com nuance real (ex: "vigência" restrita ao ranking interno de retrieval, nunca à exibição — resolvendo corretamente um conflito do enunciado que reproduziria o Incidente 2). |
| D2 — Uso de Ferramentas | 3 | Iteração real e documentada de v0.1 a v0.9, cada versão com diff concreto e origem do achado (Tech Lead, QA, P.O. Senior, Dev Sênior, autorrevisão). Nenhuma versão é cosmética — v0.3 adiciona schema Zod real, v0.6 adiciona Prioridade, v0.9 corrige inconsistência textual real. Este exercício não exige teste com Copilot (ferramenta esperada é Claude chat), então a régua de evidência de execução não se aplica aqui. |
| D3 — Qualidade do Entregável | 3 | Completo e acionável: schema Zod parseável, IDs rastreáveis, invariante testável explícito (`confidence==="Baixa" ⟹ requires_human_validation===true`), paths reais do Anexo C. Pequena ressalva (auto-reconhecida no próprio documento): a Seção 1 depende de abrir `guardrails.md` para os "Exemplos concretos" — trade-off de design declarado, não uma lacuna escondida. |
| D4 — Pensamento Crítico | 3 | Nível de autocrítica raro: identifica e corrige uma citação fabricada (VC-02), eleva uma pendência de "registrada" para "bloqueio conhecido" quando percebe o risco real para o `plan.md`/`tasks.md`, e resolve um conflito do enunciado (priorizar versão mais recente) em vez de aceitá-lo acriticamente, com justificativa ligada ao Incidente 2. |
| D5 — Aplicabilidade ao Projeto | 3 | Profundamente ancorado no NovaTech: referencia ADR-0002 (multi-turn), ADR-0003 (vigência), os 3 incidentes do cenário 1, o `requirements.md` do Exercicio 2.1 (VCs, Outcomes) e a estrutura do Anexo C. Nenhum guardrail é genérico de chatbot — todos citam carga perigosa, tiers, multiplicadores ou versões de documento específicas. |

**Score do exercício: 3.0**

### Verificação de Artefatos Machine-Readable
O artefato é prescritivo e um agente conseguiria segui-lo:
- **Bom exemplo:** Seção 3 traz um schema Zod real (`z.object`, `z.enum`) com comentários apontando cada campo ao guardrail que o exige — isso é diretamente colável em `validator.ts` e influencia geração de código pelo Copilot, não é descrição de intenção.
- **Bom exemplo:** IDs entre colchetes (`[GR-D-01]`) com tag de enforcement e prioridade tornam cada regra individualmente referenciável por outro documento ou por código.
- **Ponto de atenção (auto-reconhecido):** a Seção 1 é um resumo — para o "Exemplo concreto" completo de cada regra, o agente precisa abrir `guardrails.md`. Isso é aceitável como índice, mas reduz a autossuficiência se `guardrails.md` não estiver no contexto do agente no momento da geração.

### Pontos Fortes
- Gap analysis genuíno entre o enunciado simulado e os artefatos reais, em vez de aceitar a lista simulada como substituto — e resolução correta de um conflito real (priorização de vigência) sem reproduzir o erro do Incidente 2.
- Rastreabilidade completa: cada guardrail liga a incidente, VC do requirements.md, bounded context e prioridade — permite sequenciar implementação quando o tempo for escasso.
- Transparência sobre limitações não resolvidas (enum de confiança sem "N/A", `partial_response` não trivial) registradas como bloqueios conhecidos, não escondidas.

### Pontos de Melhoria
- O processo de revisão (9 versões, 4+ personas, 2 rodadas de revisão geral) é desproporcional ao escopo do exercício — considerar, em contextos reais com prazo, definir um número fixo de rodadas de revisão de persona antecipadamente para evitar consumo excessivo de tempo em uma seção que é só um índice.
- A pendência do valor "N/A" no enum de confiança é conhecida desde o Exercicio 2.1 (requirements.md) e reaparece sem resolução até o Exercicio 2.3 — sugestão de ação: abrir um item explícito de backlog/ADR para essa decisão em vez de deixá-la como nota recorrente em três documentos.

### Classificação
**Aprovado com distinção (2.5–3.0)**

### Tópicos da Trilha para Reforço
Não aplicável — score acima de 2.5.
