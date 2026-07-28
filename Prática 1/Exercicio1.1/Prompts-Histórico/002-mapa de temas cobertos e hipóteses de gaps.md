# Histórico de Sessão 002 — Mapa de Temas Cobertos e Hipóteses de Gaps (Fase Intent + Discovery)

**Projeto:** dgs-ai-first — Prática 1
**Papel assumido por Claude:** Product Specialist, fase Intent + Discovery
**Data da sessão:** 26/07/2026
**Participante:** arlindo.junior@db1.com.br

---

## Objetivo da sessão

Partindo dos 5 resumos já gerados na sessão anterior (RES-001 a RES-005, em `Prática 1\Resumos-Discovery\`), construir os dois primeiros documentos formais do projeto: um **Mapa de Temas Cobertos** e um documento de **Hipóteses de Gaps**, com escala de risco, riscos de processos não mapeados e demais pontos de atenção — e deixá-los versionados em `.md` em uma estrutura de trabalho padronizada.

---

## 1. Construção do prompt para mapa de temas e hipóteses de gaps

### 1.1. Pedido inicial e recomendação sobre estrutura de documentos

O usuário pediu que Claude sugerisse um prompt para gerar o mapa de temas e as hipóteses de gaps, incluindo escala de riscos, riscos de processos não mapeados e outros pontos importantes para o radar, e perguntou se deveria ser um documento único ou dois documentos separados.

Claude leu os 5 resumos e o resumo de Cenário para embasar a sugestão, e recomendou **dois documentos separados**:
- O mapa de temas é um artefato descritivo/de cobertura (o que já foi lido).
- As hipóteses de gaps são um artefato analítico/de risco (o que falta, o que é incerto).
- Públicos e ritmo de atualização diferentes ao longo do discovery justificam a separação, mantendo-os cruzados por referência.

Um achado relevante identificado já nesta etapa: os 5 documentos analisados são uma fração mínima do universo total de documentação descrito no Cenário (~800 documentos SharePoint + ~400 páginas Confluence + planilhas de rede) — isso precisaria aparecer explicitamente no mapa, não só os gaps *dentro* dos 5 arquivos.

Claude then propôs um primeiro rascunho de prompt, cobrindo:
- Estrutura do Documento 1 (mapa de temas): visão geral de cobertura, temas por domínio, sobreposições/conflitos, temas órfãos, glossário.
- Estrutura do Documento 2 (hipóteses de gaps): escala de risco (Probabilidade × Impacto), hipóteses consolidadas por tema, riscos de processos não mapeados, riscos específicos ao projeto de IA, outros pontos de radar, recomendações.

### 1.2. Ajustes solicitados pelo usuário

O usuário pediu três ajustes ao prompt:
1. Caminho de salvamento alterado para `Prática 1\Documentos-Gerados`.
2. Inclusão de bloco de versão em cada documento: versão (1.0), data da versão e responsável (o próprio usuário).
3. Inclusão de regras de padronização para manter os documentos consistentes — tanto estes quanto os futuros do projeto.

Como o usuário pediu para ser consultado em caso de dúvida, Claude fez três perguntas de esclarecimento antes de reescrever o prompt:

- **Preenchimento do campo "Responsável":** opções "Arlindo Junior" (derivado do e-mail) ou "apenas o e-mail". **Resposta do usuário:** "DB1-Arlindo" (via campo de texto livre).
- **Escopo das regras de padronização:** guia completo reutilizável (metadados + nomenclatura de arquivo + formatação + histórico de revisões) vs. apenas um bloco de metadados nos 2 documentos atuais. **Resposta do usuário:** guia completo reutilizável.
- **Convenção de nome de arquivo:** numeração simples (`01-...`, `02-...`) vs. versão no nome do arquivo. **Resposta do usuário:** numeração simples.

Claude também verificou previamente, via terminal, que a pasta `Prática 1\Documentos-Gerados` já existia (vazia) antes de finalizar o prompt.

### 1.3. Prompt final consolidado

```
Preciso que você gere dois documentos de discovery em Markdown, com base nos resumos
já produzidos em "Prática 1/Resumos-Discovery/" (arquivos: resumo-Cenário,
resumo-FAQ-atendimento.md, resumo-POL-001-politica-devolucao.md,
resumo-PROC-042-frete-especial-v1.md, resumo-PROC-042-v2-frete-especial-revisado.md,
resumo-SLA-2024-tabela-sla-clientes.md).

Leia todos os arquivos antes de escrever qualquer coisa.

Salvar ambos os documentos em: "Prática 1/Documentos-Gerados/"

## Regras de padronização (aplicar a estes e a todos os documentos futuros do projeto)

1. Nomenclatura de arquivo: numeração simples + slug descritivo, sem versão no nome
   (ex.: "01-mapa-temas-cobertos.md", "02-hipoteses-gaps.md").
2. Bloco de metadados obrigatório logo abaixo do título principal, em todo documento
   gerado: Versão, Data da versão, Responsável, Status (Rascunho | Em revisão | Aprovado).
3. Histórico de revisões: tabela no final do documento com colunas
   Versão | Data | Responsável | Alteração — iniciar com a linha da v1.0.
4. Regra de versionamento: incrementos de décimo (1.0 → 1.1) para ajustes/correções;
   incremento de inteiro (1.0 → 2.0) para mudanças estruturais ou de escopo.
5. Rastreabilidade: toda afirmação originada de um resumo específico deve citar o ID
   do resumo de origem (ex.: RES-002).
6. Formatação: títulos em Markdown com hierarquia consistente; listas para itens
   discretos; tabelas para dados comparáveis.
7. Idioma e tom: português formal, direto, termos técnicos do domínio mantidos.

## Documento 1 — Mapa de Temas Cobertos
Arquivo: "01-mapa-temas-cobertos.md"
[visão geral de cobertura vs. universo documental total, temas por domínio de negócio,
sobreposições/conflitos entre fontes, temas órfãos não analisados, glossário consolidado]

## Documento 2 — Hipóteses de Gaps
Arquivo: "02-hipoteses-gaps.md"
[escala de risco Probabilidade × Impacto com matriz de classificação, hipóteses de gaps
consolidadas por tema, riscos de processos não mapeados, riscos específicos ao projeto
de IA, outros pontos de radar, recomendações priorizadas por classificação de risco]
```

O usuário aprovou com "Pode executar".

---

## 2. Execução do prompt

Claude leu os 6 arquivos de `Prática 1\Resumos-Discovery\` (resumo-Cenário, RES-001 a RES-005) na íntegra e gerou os dois documentos em `Prática 1\Documentos-Gerados\`.

### 2.1. Documento 1 — `01-mapa-temas-cobertos.md`

- Visão geral: 5 documentos analisados representam menos de 1% do universo declarado no Cenário (~1.200 documentos/páginas); do FAQ, apenas 9 de 47 perguntas (19%) foram fornecidas.
- Temas cobertos por domínio: devolução (POL-001), frete especial (PROC-042 v1/v2), SLA (SLA-2024), conhecimento operacional prático (FAQ), contexto de negócio (Cenário).
- Sobreposições e conflitos mapeados: PROC-042 v1 x v2; desconto de volume divergente entre norma e prática; "carga danificada" x "avaria em trânsito"; exceções informais da Gestão de Riscos; ambiguidade do cargo "gerente de operações regional"; prazos internos x SLA formal; definição de "carga perigosa" usada em dois documentos.
- Temas órfãos: PROC-043, PROC-088, seguro de carga, tabela mensal de fretes, calendário de feriados, processo/SLA da Gestão de Riscos, restante do FAQ.
- Glossário consolidado com 13 termos, sinalizando os de uso potencialmente inconsistente.

### 2.2. Documento 2 — `02-hipoteses-gaps.md`

- Escala de risco definida: Probabilidade (Baixa/Média/Alta) × Impacto (Baixo/Médio/Alto/Crítico), com matriz de classificação P1-Crítico a P4-Baixo.
- Tabela-resumo com 16 gaps identificados (GAP-01 a GAP-16), classificados por severidade.
- 5 gaps classificados como **P1-Crítico**:
  1. GAP-01 — Governança de versões do PROC-042 (v1 x v2 sem substituição formal).
  2. GAP-02 — Uso comprovado de versão desatualizada em campo (desconto de volume).
  3. GAP-03 — Seguro de carga sem nenhum documento normativo de referência.
  4. GAP-04 — Dependência de conhecimento tácito / pessoa-chave ("perguntando para quem sabe").
  5. GAP-05 — Risco do assistente de IA citar fonte conflitante com falsa confiança.
- Seção dedicada a riscos de processos não mapeados (PROC-043, PROC-088, tabela mensal de fretes, restante do FAQ, governança de atualização documental sem revisão unificada).
- Seção específica sobre riscos ao projeto de assistente de IA: necessidade de resolver governança de fonte antes de qualquer pipeline de RAG.
- Recomendações priorizadas pela classificação de risco (P1 primeiro).

Ambos os documentos seguem o guia de padronização acordado: bloco de metadados (Versão 1.0, Data 26/07/2026, Responsável DB1-Arlindo, Status Rascunho), histórico de revisões ao final, e citação do ID do resumo de origem (RES-00X) em toda afirmação derivada de um resumo específico.

---

## 3. Encerramento da sessão

O usuário solicitou o encerramento desta sessão com o pedido de salvar o histórico completo em arquivo `.md`, indicando inicialmente um caminho sem a pasta `Prática 1`. Claude identificou a inconsistência (o histórico da sessão anterior está em `Prática 1\Prompts-Histórico\`, não direto em `dgs-ai-first\Prompts-Histórico\`) e perguntou antes de salvar. O usuário confirmou o caminho correto: `C:\Users\arlindo.junior\dgs-ai-first\Prática 1\Prompts-Histórico`.

O próximo passo do projeto (a definir) ficou para uma **nova sessão**.

---

## 4. Arquivos gerados nesta sessão

| Arquivo | Caminho |
|---|---|
| Mapa de Temas Cobertos | `Prática 1\Documentos-Gerados\01-mapa-temas-cobertos.md` |
| Hipóteses de Gaps | `Prática 1\Documentos-Gerados\02-hipoteses-gaps.md` |
| Este histórico | `Prática 1\Prompts-Histórico\002-mapa de temas cobertos e hipóteses de gaps.md` |

## 5. Pendências para a próxima sessão

- Resolver junto a stakeholders qual versão do PROC-042 (v1 ou v2) está de fato vigente (GAP-01/GAP-02).
- Levantar documentação normativa do processo de seguro de carga (GAP-03).
- Mapear quem são as "pessoas que sabem" hoje, para tratar a dependência de conhecimento tácito (GAP-04).
- Definir estratégia de governança de fonte para documentos conflitantes antes de qualquer construção de pipeline de RAG (GAP-05).
- Esclarecer a fronteira entre "carga danificada" (sinistro) e "avaria em trânsito" (devolução) (GAP-06).
- Validar com a Gestão de Riscos se as exceções informais devem ser formalizadas na POL-001 (GAP-07).
- Tratar os demais gaps (P2 a P4) e ampliar a amostragem (restante do FAQ e do universo documental) na próxima rodada de discovery.
