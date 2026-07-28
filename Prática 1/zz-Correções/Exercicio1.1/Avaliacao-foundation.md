# Skill de Avaliação — Product Specialist (Cenário 1)

> **Programa:** Trilha de Certificação AI First — DGS / DB1 Global Software
> **Escopo:** Cenário-Âncora 1 — Fase de Entendimento e Contexto (exercícios 1.1, 1.2, 1.3)
> **Referência:** Usar com `avaliacao-foundation.md` para dimensões e escala.

**Perfil:** Opera na camada de produto — discovery, requisitos, jornadas, guardrails. Traduz conceitos técnicos em decisões de produto testáveis. Sabe estruturar contexto para obter outputs de qualidade da IA.

**Ferramentas esperadas:** Claude (chat) em todos; Claude Design no 1.2; Claude Cowork não é usado no cenário 1.

---

## Exercício 1.1 — Mapeamento de intent com engenharia de contexto

**Tópicos avaliados:** Engenharia de Prompt, Engenharia de Contexto (progressive disclosure, orçamento de atenção).

**Este é o exercício mais puro de context engineering da trilha. Avaliar com rigor em D1.**

| Critério | Score 3 | Red flag (≤ 1) |
|----------|---------|-----------------|
| Estratégia de 3 etapas coerente | Etapa 1 (metadados) → Etapa 2 (deep dive nos contraditórios) → Etapa 3 (cruzamento com FAQ). Cada etapa justificada | Colou tudo de uma vez no primeiro prompt |
| Escolha da etapa 2 justificada | Selecionou PROC-042 vs v2 por serem contraditórios | Documentos aleatórios sem justificativa |
| Reflexão progressive disclosure | Compara resultado progressivo vs "tudo de uma vez". Identifica que contexto em excesso degrada qualidade | Reflexão ausente ou superficial |
| Riscos identificados | PROC-042 contradição + FAQ sem validação formal | Riscos genéricos |
| Prompts específicos | Constraints claros, formato de output definido, contexto calibrado | "Analise estes documentos" |

**Armadilha:** Se forneceu 5 documentos completos de uma vez no primeiro prompt → D1 ≤ 1 (demonstra o oposto do objetivo).

---