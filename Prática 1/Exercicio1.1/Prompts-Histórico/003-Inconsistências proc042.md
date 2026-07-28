# Histórico de Sessão 003 — Análise de Inconsistências PROC-042 v1 x v2 (Fase Discovery)

**Projeto:** dgs-ai-first — Prática 1
**Papel assumido por Claude:** Product Specialist, fase Discovery
**Data da sessão:** 26/07/2026
**Participante:** arlindo.junior@db1.com.br

---

## Objetivo da sessão

Partindo dos documentos já gerados em `Prática 1/Documentos-Gerados/` (`01-mapa-temas-cobertos.md` e `02-hipoteses-gaps.md`), identificar quais dos 5 documentos-fonte da NovaTech mais precisam de uma análise de inconsistência completa, montar o prompt para essa análise e executá-la, gerando um terceiro documento formal do projeto.

---

## 1. Identificação dos documentos-fonte prioritários

O usuário pediu que Claude indicasse, com base nos documentos já gerados, os 2 documentos-fonte que mais precisam de análise de inconsistência.

Claude leu `01-mapa-temas-cobertos.md` e `02-hipoteses-gaps.md` na íntegra e identificou que, entre os 5 documentos-fonte (POL-001, PROC-042 v1, PROC-042 v2, SLA-2024, FAQ-Atendimento), o par **PROC-042 v1 x v2** é o único classificado como **P1-Crítico** com evidência concreta de conflito já materializado em produção (GAP-01: nenhuma versão se declara formalmente sucessora; GAP-02: uso comprovado da regra de desconto de volume da v1 em campo, confirmado pelo FAQ item 45).

Claude localizou os arquivos completos em `Prática 1/Arquivos-de-Trabalho/`:
- `PROC-042-frete-especial-v1.md`
- `PROC-042-v2-frete-especial-revisado.md`

Antes de prosseguir, Claude perguntou ao usuário: (1) se confirmava esse par como alvo, e (2) qual formato de saída esperava para a análise. **Respostas do usuário:** não fazer a análise ainda, apenas identificar os documentos; formato = documento markdown estruturado.

---

## 2. Construção do prompt para análise de inconsistências

### 2.1. Leitura de base e proposta inicial

O usuário pediu ajuda para montar o prompt de análise de inconsistência entre os dois documentos completos, com escopo restrito a eles, mantendo a estrutura padrão já usada no projeto, e pediu sugestão de esquema de classificação e de pontos a incluir.

Claude leu os dois arquivos-fonte na íntegra e os resumos RES-002/RES-004 (`Resumos-Discovery/`) como apoio de contexto, confirmou a convenção já existente de armazenar prompts em `Prompts-Histórico/`, e propôs:
- Um esquema de classificação de 3 eixos por inconsistência: **Tipo** (numérica/valor, regra de negócio/processo, governança documental/vigência, contradição interna, textual/terminológica, dependência externa), **Criticidade** (Baixa/Média/Alta/Crítica) e **Resolubilidade** (resolvível só com os 2 documentos x requer validação externa/negócio).
- Uma estrutura de documento com: visão geral, tabela comparativa parâmetro a parâmetro, lista de inconsistências com ID (INC-XX), contradições internas, pontos sem divergência, lacunas em aberto e recomendações.

Um primeiro rascunho de prompt foi apresentado. O usuário pediu para não avançar com a execução ainda, e sim revisar o prompt primeiro.

### 2.2. Refinamento da classificação

Ao ser perguntado se o prompt estava completo, o usuário citou a classificação de risco das inconsistências como exemplo de ponto a verificar. Claude identificou 3 decisões ainda em aberto e perguntou:

1. **Critérios explícitos de Criticidade** (escrever os 4 níveis no próprio prompt, em vez de referenciar genericamente o doc 02). **Resposta:** escrever critérios explícitos.
2. **Agrupamento das recomendações por Resolubilidade** (separar "ações imediatas" de "ações que dependem do negócio", cada grupo ordenado por criticidade, em vez de uma lista única ordenada só por criticidade). **Resposta:** sim, separar em dois grupos.
3. **Referência cruzada a GAP-01/GAP-02** (`02-hipoteses-gaps.md`) como achados que a nova análise aprofunda, sem editar o arquivo original. **Resposta:** sim, citar como origem.

### 2.3. Prompt final consolidado

```
Preciso que você faça uma análise de inconsistências entre os dois documentos-fonte
completos abaixo (ler a íntegra de cada um, não os resumos):

- "Prática 1/Arquivos-de-Trabalho/PROC-042-frete-especial-v1.md"
- "Prática 1/Arquivos-de-Trabalho/PROC-042-v2-frete-especial-revisado.md"

Escopo: analisar SOMENTE estes dois documentos, de forma completa (todas as seções).
Não usar os resumos (RES-002/RES-004) nem qualquer outro documento como fonte de
verdade — eles podem ser citados apenas como referência de contexto já levantado,
nunca como substituto da leitura integral dos dois arquivos acima.

Ler os dois arquivos por completo antes de escrever qualquer coisa.

Salvar o documento em: "Prática 1/Documentos-Gerados/03-analise-inconsistencias-proc042-v1-v2.md"

Seguir o guia de padronização já em uso no projeto (mesmo aplicado em
01-mapa-temas-cobertos.md e 02-hipoteses-gaps.md):
- Bloco de metadados logo abaixo do título: Versão, Data da versão, Responsável
  (DB1-Arlindo), Status (Rascunho | Em revisão | Aprovado).
- Histórico de revisões: tabela ao final (Versão | Data | Responsável | Alteração),
  iniciando com a linha da v1.0.
- Formatação: títulos em Markdown com hierarquia consistente; tabelas para dados
  comparáveis; português formal, direto, termos técnicos do domínio mantidos.
- Rastreabilidade: toda inconsistência apontada deve citar a seção exata de origem
  em cada versão (ex.: "v1 §4" / "v2 §4"). Citar GAP-01 e GAP-02 de
  02-hipoteses-gaps.md como achados já registrados que esta análise aprofunda,
  sem editar o arquivo 02.

## Esquema de classificação das inconsistências

Cada inconsistência recebe um ID (INC-01, INC-02...) e é classificada em 3 eixos:

1. **Tipo:** Numérica/valor | Regra de negócio/processo | Governança documental/vigência
   | Contradição interna | Textual/terminológica | Dependência externa.

2. **Criticidade:**
   - **Crítica:** a divergência gera cálculo de frete diferente para o cliente E não
     há regra clara de qual versão aplicar hoje (risco financeiro/contratual direto,
     sem resolução no próprio texto).
   - **Alta:** divergência de valor/regra que impacta resultado financeiro ou prazo
     ao cliente, mas há alguma indicação (ainda que insuficiente) de qual versão
     aplicar.
   - **Média:** divergência que afeta processo interno ou dependência externa
     (ex.: PROC-043), sem exposição financeira direta ao cliente.
   - **Baixa:** divergência textual/redacional que não altera resultado prático
     (ex.: nota explicativa a mais em uma versão).

3. **Resolubilidade:** Resolvível apenas com os 2 documentos (resposta está no texto)
   | Requer validação externa/negócio (ex.: qual versão vigora hoje).

## Estrutura do documento

Arquivo: "03-analise-inconsistencias-proc042-v1-v2.md"

1. Visão geral — escopo (apenas os 2 documentos completos), objetivo da análise,
   limitação declarada (não considera o restante do universo documental).
2. Tabela comparativa parâmetro a parâmetro — fórmula, fator de peso, multiplicadores
   regionais, prazo de entrega, aprovação de cargas >5.000kg, desconto de volume,
   dependência de PROC-043, disposições transitórias — v1 x v2 x divergência.
3. Inconsistências identificadas — lista com ID, descrição, seção de origem em cada
   versão, e classificação nos 3 eixos acima.
4. Contradições internas — especialmente a contradição entre o cabeçalho da v2
   (nega substituir formalmente a v1) e a seção 5 (disposições transitórias que
   pressupõem a v2 vigente a partir de 01/12/2023).
5. Pontos sem divergência — o que é idêntico entre as duas versões (para deixar
   explícito o que foi checado e não gerou achado).
6. Lacunas e perguntas em aberto específicas desta comparação.
7. Recomendações — separadas em dois grupos, cada um ordenado por Criticidade:
   - **Ações imediatas** (Resolubilidade = resolvível só com os 2 documentos)
   - **Ações que dependem do negócio** (Resolubilidade = requer validação externa)
8. Histórico de revisões.
```

O usuário aprovou com "Pode rodar".

---

## 3. Execução do prompt

Claude leu os dois arquivos de `Prática 1/Arquivos-de-Trabalho/` na íntegra e gerou o documento em `Prática 1/Documentos-Gerados/03-analise-inconsistencias-proc042-v1-v2.md`, seguindo a estrutura e o esquema de classificação acordados.

**Principais achados — 8 inconsistências (INC-01 a INC-08):**

- **Crítica:**
  - INC-01 — Governança de vigência: nenhuma versão se declara formalmente sucessora (aprofunda GAP-01).
  - INC-02 — Contradição interna na v2: cabeçalho nega substituição formal, mas §5 (disposições transitórias) já trata a v2 como vigente a partir de 01/12/2023 — data expirada há mais de 2 anos, sem regra definida para o período seguinte.
- **Alta:**
  - INC-04 — Divergência no fator de peso (faixas 1.001–3.000kg e acima de 3.000kg).
  - INC-05 — Divergência nos multiplicadores regionais (todas as 5 regiões, todos maiores na v2).
  - INC-06 — Divergência no prazo de entrega adicional (+2 dias na v1 x +3 dias na v2).
  - INC-07 — Divergência na regra de desconto de volume (negociação caso a caso na v1 x percentuais objetivos 5%/10% na v2).
- **Média:**
  - INC-03 — Contradição interna adicional na v2: §1 (Objetivo) usa linguagem de atualização/revisão que pressupõe suceder a v1.
  - INC-08 — Divergência na nota sobre dependência do PROC-043 (só a v2 sinaliza revisão pelo Compliance).

O documento também registra explicitamente os **pontos sem divergência** entre as versões (fórmula geral, valor base, fator de peso na faixa 500–1.000kg, aprovação de cargas >5.000kg, responsável, e a ambiguidade do limite de 500kg — que persiste idêntica nas duas versões, não resolvida pela v2), e separa as recomendações em **ações imediatas** (resolvíveis só com os 2 documentos) e **ações que dependem do negócio** (requerem validação externa, com cross-referência a GAP-01, GAP-09 e GAP-10 de `02-hipoteses-gaps.md`).

---

## 4. Encerramento da sessão

O usuário indicou que o próximo passo do projeto será tratado em uma nova sessão, e pediu o salvamento deste histórico em `Prática 1/Prompts-Histórico/`.

Sobre o nome do arquivo, o usuário enviou `003-Inconsistências proc042 .md` (com um espaço antes da extensão `.md`). Claude identificou a inconsistência de nomenclatura em relação aos arquivos `001` e `002` da mesma pasta (sem espaço antes da extensão) e perguntou antes de salvar. O usuário confirmou a remoção do espaço.

---

## 5. Arquivos gerados nesta sessão

| Arquivo | Caminho |
|---|---|
| Análise de Inconsistências PROC-042 v1 x v2 | `Prática 1\Documentos-Gerados\03-analise-inconsistencias-proc042-v1-v2.md` |
| Este histórico | `Prática 1\Prompts-Histórico\003-Inconsistências proc042.md` |

## 6. Pendências para a próxima sessão

- Confirmar junto à Diretoria Comercial/Operações qual versão do PROC-042 (v1 ou v2) está oficialmente vigente hoje (INC-01/GAP-01).
- Definir e comunicar o tratamento de aditivos contratuais de desconto de volume negociados sob a lógica da v1 diante da regra objetiva da v2 (INC-07/GAP-10).
- Confirmar junto ao Compliance o status atual do PROC-043 (INC-08/GAP-09).
- Corrigir a contradição interna da v2 entre cabeçalho, §1 e §5 (INC-02, INC-03), independentemente da decisão de qual versão prevalece.
- Demais pendências já registradas em `02-hipoteses-gaps.md` (GAP-03 a GAP-16) seguem em aberto e não foram tratadas nesta sessão.
- Próximo passo do projeto (a definir) fica para uma **nova sessão**.
