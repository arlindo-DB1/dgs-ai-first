# Skill de Avaliação — Product Specialist (Cenário 1)

> **Programa:** Trilha de Certificação AI First — DGS / DB1 Global Software
> **Escopo:** Cenário-Âncora 1 — Fase de Entendimento e Contexto (exercícios 1.1, 1.2, 1.3)
> **Referência:** Usar com `avaliacao-foundation.md` para dimensões e escala.

**Perfil:** Opera na camada de produto — discovery, requisitos, jornadas, guardrails. Traduz conceitos técnicos em decisões de produto testáveis. Sabe estruturar contexto para obter outputs de qualidade da IA.

**Ferramentas esperadas:** Claude (chat) em todos; Claude Design no 1.2; Claude Cowork não é usado no cenário 1.

---

## Exercício 1.2 — Design de jornada com componente de IA

**Tópicos avaliados:** Fundamentos de IA (limitações que afetam jornada), RAG (feedback loop).

| Critério | Score 3 | Red flag (≤ 1) |
|----------|---------|-----------------|
| 3 fluxos presentes | Principal + fallback + feedback, todos completos | Apenas caminho feliz |
| Guardrails específicos ao domínio | "Nunca informar prazo de devolução para carga perigosa" | "Não errar" |
| Feedback loop completo | Atendente reporta → investiga → correção chega ao assistente | Botão sem processo atrás |
| Diagrama (Claude Design) | Coerente com texto, 3 caminhos visíveis, legível para não-técnico | Genérico ou ausente |

---