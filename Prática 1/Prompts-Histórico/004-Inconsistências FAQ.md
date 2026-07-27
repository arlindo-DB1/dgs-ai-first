# Histórico de Sessão 004 — Inconsistências entre Documentos Formais e FAQ-Atendimento (Fase Discovery)

**Projeto:** dgs-ai-first — Prática 1
**Papel assumido por Claude:** Product Specialist, fase Intent + Discovery
**Data da sessão:** 26/07/2026
**Participante:** arlindo.junior@db1.com.br

---

## Objetivo da sessão

Cruzar as inconsistências já registradas nos documentos formais do projeto com as práticas informais descritas no FAQ do atendimento, montar o prompt dessa análise em conjunto com o usuário, executá-la após aprovação, e gerar um quarto documento formal do projeto em `Prática 1/Documentos-Gerados/`.

---

## 1. Leitura de base

Claude leu na íntegra os documentos disponíveis em `Prática 1/Documentos-Gerados/` (`01-mapa-temas-cobertos.md`, `02-hipoteses-gaps.md`, `03-analise-inconsistencias-proc042-v1-v2.md`) e `Prática 1/Arquivos-de-Trabalho/FAQ-atendimento.md`, além do prompt histórico da sessão anterior (`003-Inconsistências proc042.md`), para usar como base do padrão de estrutura já estabelecido no projeto.

---

## 2. Construção do prompt para análise

### 2.1. Proposta inicial e perguntas de escopo

O usuário pediu ajuda para montar o prompt que cruzaria `01-mapa-temas-cobertos.md` (lado formal) com o FAQ completo (lado informal), pedindo também sugestão de esquema de classificação das inconsistências. Reforçou que não era para executar nada ainda, apenas propor o prompt para revisão.

Antes de apresentar o rascunho, Claude identificou 3 decisões de escopo que mudariam o resultado e perguntou ao usuário:

1. **Escopo do "lado formal":** apenas `01-mapa-temas-cobertos.md`, ou também `02-hipoteses-gaps.md` (que já registra GAP-02, GAP-06 e GAP-07 citando itens do FAQ)? **Resposta:** usar os dois (01 + 02).
2. **Tratamento de itens do FAQ sem nenhum documento normativo correspondente** (lacunas puras, ex.: item 22 — seguro de carga, já registrado como GAP-03): incluir como categoria própria de classificação, ou excluir do escopo por já estarem tratados em 02? **Resposta:** incluir como categoria própria.
3. **Nome do arquivo de saída**, seguindo a numeração sequencial 01/02/03. **Resposta:** `04-FAQ-Inconsistencias-praticas-informais.md` (nome fornecido pelo usuário).

### 2.2. Primeiro rascunho do prompt

Com base nas respostas, Claude apresentou um rascunho de prompt com:
- Fontes: `01-mapa-temas-cobertos.md`, `02-hipoteses-gaps.md` e `FAQ-atendimento.md` completo (9 itens).
- Esquema de classificação de 4 eixos por inconsistência: **Natureza da divergência** (Contradição direta, Extrapolação/exceção, Lacuna documental, Ambiguidade herdada não resolvida, Terminológica/conceitual), **Criticidade** (Baixa/Média/Alta/Crítica), **Resolubilidade** (resolvível só com os documentos analisados x requer validação externa/negócio) e **Status de rastreamento** (achado já registrado em 01/02 x achado novo).
- Estrutura de documento com 8 seções: visão geral, tabela cruzada, inconsistências identificadas, práticas sem contradição candidatas a formalização, pontos confirmados, lacunas em aberto, recomendações (separadas em ações imediatas x ações que dependem do negócio) e histórico de revisões.

### 2.3. Ajuste: inclusão do documento 03 como quarta fonte

O usuário pediu dois ajustes ao prompt:
- Incluir `03-analise-inconsistencias-proc042-v1-v2.md` como fonte adicional (documento que já detalha INC-01/INC-02/INC-07, diretamente relacionados aos itens 8 e 45 do FAQ).
- Atualizar a instrução de leitura de "os três arquivos" para "os quatro arquivos".

Claude revisou o prompt incorporando `03` como terceira fonte do lado formal (total de 4 fontes: 01, 02, 03 e FAQ), ajustando a instrução de leitura completa para os quatro arquivos e as referências de rastreabilidade (GAP/INC) ao longo do prompt.

O usuário aprovou com "Pode rodar".

---

## 3. Execução do prompt

Claude releu os quatro arquivos por completo e gerou o documento em `Prática 1/Documentos-Gerados/04-FAQ-Inconsistencias-praticas-informais.md`, seguindo a estrutura e o esquema de classificação de 4 eixos acordados.

**Principais achados — 10 inconsistências (INC-FAQ-01 a INC-FAQ-10):**

- **Crítica:**
  - INC-FAQ-02 — Convivência não resolvida entre PROC-042 v1 e v2, confirmada pela prática do atendimento (aprofunda GAP-01; INC-01/INC-02 do doc 03).
  - INC-FAQ-05 — Seguro de carga sem norma, com variação adicional não confirmada para contratos anteriores a 2023 (aprofunda GAP-03).
  - INC-FAQ-09 — Prática de desconto de volume alinhada à regra da v1, não aos percentuais objetivos da v2 (aprofunda GAP-02; INC-07 do doc 03).
- **Alta:**
  - INC-FAQ-01 — Exceção informal da Gestão de Riscos em devolução de carga perigosa, sem critério documentado (aprofunda GAP-07).
  - INC-FAQ-03 — Indício de clientes com contrato vinculado à tabela de frete antiga (v1), sem regra de migração (achado novo).
  - INC-FAQ-07 — "Frete expresso" de carga perigosa sem norma própria; autorização real (~2 dias) contradiz o nome do serviço (parcialmente relacionado a GAP-09).
  - INC-FAQ-08 — Fronteira não documentada entre "carga danificada" (sinistro, 48h) e "avaria em trânsito" (aprofunda GAP-06).
  - INC-FAQ-10 — Contradição interna no próprio FAQ: item 8 orienta usar a v2 "na dúvida", mas item 45 descreve prática seguindo a v1 (achado novo).
- **Média:**
  - INC-FAQ-06 — Prazos de trânsito por região e critério de priorização (R$ 50.000) sem documento normativo (achado novo).
- **Baixa:**
  - INC-FAQ-04 — Tier "Platinum" e programa de fidelidade descontinuado (2022) sem documento correspondente (achado novo).

O documento também registra explicitamente os **pontos confirmados** (estrutura de SLA de resposta/resolução do item 41; governança da autonomia de desconto do item 45), as **práticas candidatas a formalização** sem contradição (critérios de priorização de rastreamento, processo de autorização do frete expresso, esclarecimento sobre tiers), e as **lacunas específicas** que não puderam ser confirmadas dentro do escopo desta análise (valores exatos do SLA-2024 citados no item 41; limiar de R$ 50.000 do item 27), por não ter sido relido o documento-fonte original. As recomendações foram separadas em **ações imediatas** e **ações que dependem do negócio**, com cross-referência a GAP-01, GAP-02, GAP-03, GAP-06, GAP-07 e GAP-09 (`02-hipoteses-gaps.md`) e a INC-07 (`03-analise-inconsistencias-proc042-v1-v2.md`).

---

## 4. Encerramento da sessão

Após a geração do documento, Claude notou um trecho com uma palavra em inglês ("already") misturada ao texto em português na seção de INC-FAQ-09 e tentou corrigi-lo. O usuário rejeitou a correção, explicando que a palavra já estava incorreta no original e que deveria ser mantida como está — o documento `04-FAQ-Inconsistencias-praticas-informais.md` permanece sem essa alteração.

O usuário indicou que o próximo passo do projeto será tratado em uma nova sessão, e pediu o salvamento deste histórico em `Prática 1/Prompts-Histórico/`, com o nome `004-Inconsistências FAQ.md`.

---

## 5. Arquivos gerados nesta sessão

| Arquivo | Caminho |
|---|---|
| Inconsistências entre Documentos Formais e FAQ-Atendimento | `Prática 1\Documentos-Gerados\04-FAQ-Inconsistencias-praticas-informais.md` |
| Este histórico | `Prática 1\Prompts-Histórico\004-Inconsistências FAQ.md` |

## 6. Pendências para a próxima sessão

- Confirmar junto à Diretoria Comercial/Operações qual versão do PROC-042 (v1 ou v2) está vigente hoje, incluindo o indício de contratos vinculados à tabela antiga (INC-FAQ-02, INC-FAQ-03/GAP-01, GAP-10).
- Definir e comunicar o tratamento da prática de desconto de volume hoje alinhada à v1 diante da regra objetiva da v2 (INC-FAQ-09/GAP-02).
- Levantar junto ao Comercial/Jurídico o documento normativo do seguro de carga, incluindo a variação para contratos anteriores a 2023 (INC-FAQ-05/GAP-03).
- Confirmar com a Gestão de Riscos os critérios objetivos de exceção na devolução de carga perigosa (INC-FAQ-01/GAP-07).
- Confirmar com Compliance o prazo real de autorização do frete expresso e avaliar ajuste do nome comercial do serviço (INC-FAQ-07/GAP-09).
- Esclarecer com Operações/Jurídico a fronteira entre "carga danificada" e "avaria em trânsito", incluindo o prazo de 48h relatado no FAQ (INC-FAQ-08/GAP-06).
- Confirmar contra o documento SLA-2024 original os valores exatos citados no item 41 e o limiar de R$ 50.000 citado no item 27, antes de qualquer uso normativo (pendência introduzida por esta sessão, fora do escopo dos quatro documentos analisados).
- Demais pendências já registradas em `02-hipoteses-gaps.md` (GAP-04, GAP-08, GAP-11 a GAP-16) e nas recomendações de `03-analise-inconsistencias-proc042-v1-v2.md` seguem em aberto e não foram tratadas nesta sessão.
- Próximo passo do projeto (a definir) fica para uma **nova sessão**.
