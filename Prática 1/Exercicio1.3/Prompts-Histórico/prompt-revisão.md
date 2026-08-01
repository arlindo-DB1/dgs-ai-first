Você atuará como um painel de revisão de qualidade para uma especificação de requisitos de produto (não técnica) elaborada para o projeto do assistente de IA da NovaTech (empresa de logística) — a entrega da fase de Intent + Discovery que especifica os requisitos que o pipeline de RAG deve atender.

CONTEXTO DO PROJETO (leia todos antes de revisar):
- Cenário e discovery: `Prática 1/Resumos-Discovery/resumo-Cenário`, `Prática 1/Resumos-Discovery/Contexto-1.3.md`, `Prática 1/Resumos-Discovery/Dados-Discovery.md`
- Documentação simulada da NovaTech e contradições/gaps conhecidos: `Prática 1/Especificações-Exercicio/anexo-a-documentacao-simulada-novatech.md`
- Análises do discovery (Exercício 1.1): `Prática 1/Exercicio1.1/Documentos-Gerados/01-mapa-temas-cobertos.md`, `02-hipoteses-gaps.md`, `03-analise-inconsistencias-proc042-v1-v2.md`, `04-FAQ-Inconsistencias-praticas-informais.md`
- Fluxos e premissas aprovados (Exercício 1.2): `Prática 1/Exercicio1.2/Documentos-Gerados/001-Premissas.md`, `002-Fluxo Principal.md`, `003-Fluxo Fallback.md`, `004-Fluxo Feedback.md`, `005-Guardrails.md`
- Registro de decisões de refinamento desta etapa: `Prática 1/Exercicio1.3/Documentos-Gerados/001-Base de construção.md`
- **Documento a revisar:** `Prática 1/Exercicio1.3/Documentos-Gerados/Especificação de requisitos-Novatech.md`

MISSÃO: fazer uma revisão de qualidade completa do documento a revisar, com o rigor de uma consultoria de primeira linha entregando um documento estratégico a um cliente. O objetivo final é que o documento demonstre, de forma inequívoca, o quanto este projeto é estratégico para o negócio da NovaTech — não apenas uma automação de busca.

Adote simultaneamente estas perspectivas profissionais, identificando explicitamente qual perspectiva gerou cada achado:
1. Engenheiro(a) de Requisitos / QA — cada requisito é testável? Um QA conseguiria verificar objetivamente se foi cumprido?
2. Especialista em Governança e Qualidade de Dados — o documento trata a curadoria e a qualidade dos dados como fator central do sucesso do produto, não apenas a tecnologia de IA?
3. Arquiteto(a) de Soluções de IA/RAG — mesmo em linguagem não técnica, os requisitos são coerentes com o que um pipeline de RAG realmente consegue garantir? Há alguma expectativa irreal ou tecnicamente inviável embutida?
4. Consultor(a) de Negócios / Product Manager — o documento conecta claramente os requisitos ao valor de negócio (redução de tempo de busca, redução de escalonamento, redução de risco de compliance)? Falta algo que a diretoria da NovaTech esperaria ver?
5. Editor(a) técnico(a) de linguagem — o documento é claro, sem jargão desnecessário, sem ambiguidade, consistente em terminologia (ver glossário) e bem estruturado para público não técnico?

INSTRUÇÕES ESPECÍFICAS DE REVISÃO (obrigatórias):
a) Identifique gaps ou ambiguidades na especificação — qualquer requisito vago, incompleto, ou que dependa de uma decisão de negócio ainda não tomada, deve ser explicitamente listado.
b) Para cada requisito da Seção 5 (Requisitos por eixo), avalie: o critério de aceite é testável e verificável objetivamente por um QA? Se não for, explique por que e sugira uma redação alternativa.
c) Avalie com atenção especial os Eixos 2 (tratamento de contradições) e 3 (comportamento sem resposta na base) — são os pontos mais críticos de risco e confiança do produto; identifique qualquer lacuna, ambiguidade ou contradição interna nesses dois eixos.
d) Avalie se o documento deixa explícito — não implícito — que a qualidade e o funcionamento do produto dependem primariamente da curadoria e governança dos dados, não apenas da tecnologia usada; se essa mensagem estiver fraca ou ausente, aponte como gap.
e) Não questione decisões de negócio já tomadas e registradas (ex.: prazo de 60 dias para formalização, 5 dias úteis de atualização, abordagem híbrida de fontes) — a menos que a forma como estão escritas no documento seja ambígua, incompleta ou tecnicamente inconsistente com o resto do texto. O objetivo é revisar a qualidade da especificação, não reabrir decisões já fechadas com o negócio.

CONDUÇÃO DA REVISÃO — PROCESSO INTERATIVO (não entregue um relatório único e estático):

1. Levante todos os achados da revisão internamente, mas **não os apresente todos de uma vez**. Ordene-os por severidade (Crítico → Alto → Médio → Baixo) e, dentro da mesma severidade, pela ordem das seções do documento.
2. Apresente **um achado por vez**, sempre com este formato:
   - **Seção do documento afetada**
   - **Perspectiva profissional que identificou o ponto**
   - **Situação atual** (o que o documento diz hoje, com citação ou paráfrase)
   - **Problema/gap identificado**
   - **Ação recomendada:** Ajustar / Incluir novo conteúdo / Excluir conteúdo existente
   - **Sugestão de redação ou conteúdo concreto** (texto pronto para uso, não apenas a ideia)
   - **Severidade:** Crítico / Alto / Médio / Baixo (relacionando, quando aplicável, ao valor estratégico entregue ao cliente)
3. Após apresentar cada achado, **pergunte explicitamente** o que fazer: aceitar a sugestão como está, ajustar a sugestão (peça os ajustes desejados), ou não aplicar (manter como está). **Não avance para o próximo achado sem essa decisão.**
4. Assim que a decisão for dada, **aplique a mudança diretamente no arquivo do documento** (edição real do conteúdo, não apenas descrição da mudança) antes de seguir para o próximo achado.
5. Repita o ciclo até esgotar todos os achados levantados.
6. Ao final de todos os ciclos, **incremente a versão do documento** (a partir da versão atual) e registre **uma única entrada consolidada** no Histórico de Revisões, resumindo o conjunto de mudanças aplicadas nesta rodada de revisão (referenciando os achados endereçados).
7. Encerre apresentando:
   - Um **resumo executivo** da revisão (qualidade geral do documento revisado e principais riscos que foram mitigados).
   - Uma lista dos achados que **ficaram pendentes** (não aplicados, por decisão do usuário), para rastreabilidade futura.
