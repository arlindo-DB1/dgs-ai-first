## Avaliação do Exercício 2.1 — Recorte de domínio e spec SDD do query endpoint

### Resumo
Entregável de altíssima qualidade: o recorte de domínio é coerente com o negócio de logística, a linguagem ubíqua antecipa ambiguidades reais para um LLM (ex.: disambiguação de "Gold"/"Standard"), e o `requirements.md` segue rigorosamente a estrutura SDD com 14 critérios de verificação testáveis. A iteração é documentada de ponta a ponta (v0.1→v0.5 e v0.1→v0.4), com mudanças de substância — não cosméticas — e autocrítica formal contra os próprios critérios de avaliação antes da entrega.

### Scores por Dimensão

| Dimensão | Score | Justificativa |
|----------|-------|----------------|
| D1 — Domínio Conceitual | 3 | Bounded contexts corretos e específicos ao NovaTech (Devolução, Frete Especial, SLA e Clientes como contextos de conhecimento; Consulta e Resposta como orquestração; Governança Documental como transversal, com divisão de responsabilidade explícita entre `pipeline-ingestao` e `query-endpoint`). SDD aplicado com nuance (outcomes vs. features, reconciliação formal entre ADR-0003 e a spec de RAG resumida). |
| D2 — Uso de Ferramentas | 3 | Iteração real e documentada: `domain-model.md` passou por 5 rodadas com mudanças de conteúdo (não wording), `requirements.md` por 6 rodadas incluindo revisão sob lente de QA (VC-12 a VC-14 adicionados) e revisão holística de consistência. Claude Design foi de fato testado — `claude-design.md` registra um bug real de layout responsivo encontrado e corrigido (CSS específico: `width:640px` → `max-width:640px; flex:1 1 auto`), evidência concreta de geração → avaliação → ajuste, não aceitação acrítica do primeiro resultado. |
| D3 — Qualidade do Entregável | 3 | Completo e acionável: `requirements.md` tem outcomes orientados a resultado, scope boundaries derivados dos bounded contexts, constraints técnicas e de negócio explícitas, e 14 verification criteria binários (passa/não passa) cobrindo casos corretos, contradição, armadilhas, multi-domínio e fronteiras de escopo. Um Tech Lead conseguiria escrever um `plan.md` a partir disso sem pedir esclarecimentos. |
| D4 — Pensamento Crítico | 3 | Autoavaliação formal contra 5 critérios antes do fechamento, com gaps reais identificados e corrigidos (ambiguidade "Gold"/"Standard", outcome misturando resultado com métrica técnica). Risco técnico do VC-11 sinalizado explicitamente como não validado por um Dev Sênior real — reconhecimento honesto de limitação, sem inflar confiança da própria spec. |
| D5 — Aplicabilidade ao Projeto | 3 | Profundamente conectado: referencia ADR-0001 a ADR-0004 nominalmente, resolve um conflito real entre ADR-0003 e a spec de RAG com opções e trade-offs explícitos, usa o budget de contexto do cenário 1 (~4K+8K tokens, 3 turnos) como constraint, e respeita a estrutura de repositório do Anexo C (`specs/query-endpoint/requirements.md`). |

**Score do exercício: 3.0**

### Verificação de Artefatos Machine-Readable
Este exercício não exige um artefato prescritivo no sentido de AGENTS.md/skill (isso é o foco dos exercícios 2.2/2.3), mas o `requirements.md` ainda precisa ser processável por um agente Tech Lead na etapa seguinte (geração do `plan.md`). Nesse sentido, o documento está bem posicionado: os verification criteria (VC-01 a VC-14) são binários e específicos ("mostra ambas as versões, nunca combina valores" / "responde que não há informação sobre frete padrão") — um agente consegue transformá-los diretamente em casos de teste, sem interpretação. O único ponto ainda aberto (não um defeito, mas uma dependência declarada) é o VC-11, cuja viabilidade de implementação (mecanismo de recuperação garantida além de similaridade semântica) está sinalizada como pendente de validação por um Dev Sênior antes de virar plano técnico — postura correta em vez de assumir viabilidade sem verificar.

### Pontos Fortes
- Disambiguação explícita de linguagem ubíqua para agentes de IA ("Gold" ≠ metal/qualidade genérica; "Standard" ≠ adjetivo solto) — exatamente o tipo de cuidado que a rubrica do papel busca em nível 3.
- Reconciliação documentada de um conflito real entre decisão da fase anterior (ADR-0003: "priorizar versão mais recente") e a spec de RAG desta fase ("mostrar ambas as versões"), com opções, trade-offs e decisão explícita do usuário — não uma escolha silenciosa do agente.
- Evidência concreta de teste real no Claude Design (bug de layout responsivo encontrado e corrigido com detalhes técnicos de CSS), não apenas a alegação de que "foi testado".

### Pontos de Melhoria
- VC-11 permanece com viabilidade técnica não confirmada por um Dev Sênior real — ação: levar para validação antes de iniciar o `plan.md`, como o próprio histórico já recomenda.
- Mockup testou apenas o estado de confiança "Baixa" no Claude Design; os estados "Alta" e "Média" (verde/âmbar) ficaram apenas descritos em texto, sem gerar/validar visualmente — ação: gerar ao menos um segundo estado antes de considerar o padrão visual encerrado.
- Inconsistência de rótulo mantida conscientemente no mockup ("PROC-042" vs "PROC-042 v1" no rodapé) — decisão do usuário, mas ainda um risco de confusão real numa UI de produção; ação: resolver antes de qualquer reuso do mockup como referência final de implementação.

### Classificação
**Aprovado com distinção (2.5–3.0)**

### Tópicos da Trilha para Reforço
Não aplicável — score acima do limiar de reforço (≥ 2.5).
