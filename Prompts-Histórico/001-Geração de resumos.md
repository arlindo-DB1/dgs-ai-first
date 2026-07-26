# Histórico de Sessão 001 — Geração de Resumos (Fase Intent + Discovery)

**Projeto:** dgs-ai-first — Prática 1
**Papel assumido por Claude:** Product Specialist, fase Intent + Discovery
**Data da sessão:** 26/07/2026
**Participante:** arlindo.junior@db1.com.br

---

## Objetivo da sessão

Definir um prompt reutilizável para solicitar, a cada arquivo de documentação de origem, um resumo estruturado que sirva de insumo para o levantamento de hipóteses, riscos e processos na fase de Discovery de um projeto simulando a empresa fictícia NovaTech. Em seguida, aplicar esse prompt aos 5 documentos de origem definidos pelo usuário.

---

## 1. Definição de como registrar o histórico da sessão

O usuário perguntou como salvar o histórico completo da sessão em um arquivo `.md` para subir no GitHub, documentando o processo de trabalho com Claude.

Claude explicou uma limitação importante: só tem acesso ao histórico dentro do próprio contexto de conversa (não existe um "export" separado da interface), portanto o registro só pode ser feito com o que estiver disponível até o momento do pedido. Foram oferecidas três opções de cadência de registro:

1. Só no final da sessão.
2. Atualizações incrementais ao longo da sessão.
3. Criar já com o que houvesse até aquele ponto.

**Decisão do usuário:** registrar apenas no final da sessão, sob demanda explícita.

---

## 2. Construção do prompt reutilizável para resumos de Discovery

O usuário solicitou:

> "Claude, seremos o Product Specialist em um projeto, começando pela fase fase de Intent + Discovery, onde iremos analisar uma documentação extensa dividida em 5 arquivos. Teremos que gerar um resumo de cada um desses arquivos, para nos pautar nos levantamento de hipóteses, riscos e processos envolvidos. Preciso que sugira um prompt, para eu te pedir o resumo de cada um desses arquivos, indicando o arquivo de origem e onde você deve salvar o arquivo de resumo. [...] Não é para analisar os arquivos do diretório. Pois eu vou direcionar quais arquivos vamos trabalhar por processo."

Antes de propor o template, Claude fez perguntas de esclarecimento (via pergunta de múltipla escolha), e o usuário respondeu:

- **Local de salvamento dos resumos:** nova subpasta dedicada (não junto ao arquivo original, nem caminho informado a cada pedido).
- **Framework/metodologia de Discovery:** nenhum framework formal pré-definido — Claude ficou livre para sugerir a estrutura.
- **Escopo do prompt:** um arquivo por vez (não uma versão em lote para os 5 de uma vez).

Com base nisso, Claude propôs o template de prompt com 9 seções fixas (Metadados, Resumo executivo, Processos e regras de negócio, Hipóteses, Riscos, Lacunas, Termos/glossário, Referências cruzadas, Recomendações), destino de salvamento em `Prática 1\Resumos-Discovery\resumo-[NOME_BASE_DO_ARQUIVO].md`, e um conjunto inicial de regras (não inventar informação, linguagem objetiva em português/Markdown, seções padronizadas entre os 5 resumos).

### 2.1. Ajuste de regras do prompt

O usuário propôs duas regras adicionais:

> "Vou incluir duas linhas na parte de regras:
> - Manter a integridade do modelo usado para todos arquivos de resumo que são gerados
> - Manter um indice base em todos os arquivos de resumo, pois serão utilizados como ponto de pesquisa e formação de documentos."

Claude avaliou as duas linhas como compreensíveis, mas identificou ambiguidade em ambas:

- Na primeira, o termo "modelo" poderia ser confundido com "modelo de IA" em vez de "template/estrutura do documento" — Claude propôs uma redação mais explícita, reforçando estrutura, ordem e formatação fixas.
- Na segunda, "índice base" era ambíguo entre: sumário/TOC interno, bloco de metadados/tags para busca, um índice mestre centralizado, ou uma combinação. Claude perguntou diretamente ao usuário.

**Decisão do usuário:** "índice base" = bloco de metadados/tags no topo de cada resumo, para permitir busca e cruzamento entre os resumos.

### 2.2. Prompt final consolidado

```
Atue como Product Specialist na fase de Intent + Discovery.

Analise o arquivo de origem: [CAMINHO_COMPLETO_DO_ARQUIVO]

Gere um resumo estruturado desse documento e salve em:
"Prática 1\Resumos-Discovery\resumo-[NOME_BASE_DO_ARQUIVO].md"

O resumo deve conter as seguintes seções:

1. Metadados / índice base — ID único do resumo (sequencial), nome do arquivo de origem, tipo de documento (política, FAQ, procedimento, SLA etc.), categoria/tema, palavras-chave, processos relacionados e data desta análise. Este bloco deve permitir localizar e cruzar este resumo com os demais posteriormente.
2. Resumo executivo — visão geral do conteúdo em até 5 linhas.
3. Processos e regras de negócio identificados — fluxos, exceções, condições e responsáveis mencionados.
4. Hipóteses levantadas — suposições sobre comportamento de usuário, do negócio ou do sistema que este documento sugere, e que precisam ser validadas nas próximas fases.
5. Riscos identificados — de negócio, operacionais, técnicos, de compliance/legal ou de ambiguidade textual.
6. Lacunas e perguntas em aberto — informações faltantes, contradições internas ou pontos que exigem esclarecimento com stakeholders.
7. Termos e definições específicas do domínio — glossário do que aparece no documento (importante para consistência entre os 5 arquivos).
8. Referências cruzadas e dependências — menções a outros processos/documentos, versões anteriores ou conflitos potenciais com outros arquivos da mesma trilha.
9. Recomendações para a fase de Discovery — o que este documento sugere que devemos investigar, testar ou confirmar a seguir.

Regras:
- Não invente informação que não esteja no documento original; se algo for ambíguo, registre isso explicitamente na seção 6.
- Use linguagem objetiva, em português, formatada em Markdown.
- Mantenha os nomes de seção padronizados acima em todos os resumos, para permitir comparação entre os 5 arquivos depois.
- Manter rigorosamente a mesma estrutura, ordem e formatação de seções (o modelo definido acima) em todos os resumos gerados, sem omitir, renomear ou reordenar seções entre um arquivo e outro.
- Manter o bloco de metadados/índice (seção 1) preenchido de forma padronizada em todos os arquivos, pois servirá como ponto de pesquisa e cruzamento na formação de novos documentos.
```

O usuário aprovou o prompt final sem mais ajustes: "Não, vou atualizar os campos e vamos começar os trabalhos."

---

## 3. Aplicação do prompt aos 5 arquivos de origem

O prompt final foi aplicado, um arquivo por vez, exatamente como definido. Para cada um, Claude leu o arquivo de origem indicado e gerou o resumo estruturado correspondente em `Prática 1\Resumos-Discovery\`. O conteúdo completo de cada resumo **não é duplicado aqui** — apenas referenciado — para não inflar este histórico com conteúdo já versionado separadamente.

### 3.1. RES-001 — POL-001 (Política de Devolução)
- **Origem:** `Prática 1\POL-001-politica-devolucao.md`
- **Resumo:** [Prática 1/Resumos-Discovery/resumo-POL-001-politica-devolucao.md](../Prática%201/Resumos-Discovery/resumo-POL-001-politica-devolucao.md)
- **Principais achados:** prazo geral de devolução de 7 dias úteis, exceções (cargas perigosas, cadeia de frio, lacre violado), fluxo com prazos internos (4h triagem, 2 dias coleta, 5 dias reembolso), regras de custo do frete reverso. Referência explícita ao PROC-088 (fora do conjunto de 5 arquivos). Sinalizada possível sobreposição de prazos com o SLA-2024 e com os multiplicadores do PROC-042, a confirmar.

### 3.2. RES-002 — PROC-042 v1 (Frete Especial)
- **Origem:** `Prática 1\PROC-042-frete-especial-v1.md`
- **Resumo:** [Prática 1/Resumos-Discovery/resumo-PROC-042-frete-especial-v1.md](../Prática%201/Resumos-Discovery/resumo-PROC-042-frete-especial-v1.md)
- **Principais achados:** fórmula de frete especial (valor base × multiplicador regional × fator de peso), tabela de multiplicadores e faixas de peso, prazo +2 dias úteis, aprovação para cargas >5.000kg, desconto de volume negociado caso a caso (>10 fretes/mês). **Achado crítico:** o próprio documento declara coexistir com a v2 sem indicação formal de qual prevalece.

### 3.3. RES-003 — SLA-2024 (Tabela de SLA por Cliente)
- **Origem:** `Prática 1\SLA-2024-tabela-sla-clientes.md`
- **Resumo:** [Prática 1/Resumos-Discovery/resumo-SLA-2024-tabela-sla-clientes.md](../Prática%201/Resumos-Discovery/resumo-SLA-2024-tabela-sla-clientes.md)
- **Principais achados:** 3 tiers de cliente (Gold, Silver, Standard), SLAs de resposta/resolução para chamados gerais e incidentes críticos, definição objetiva de incidente crítico, penalidades escalonadas por violação mensal. Ambiguidade sobre a regra de "relógio não pausa fora do horário comercial" (fica clara só para Gold). Sem relação explícita entre estes SLAs e os prazos internos de POL-001.

### 3.4. RES-004 — PROC-042 v2 (Frete Especial Revisado)
- **Origem:** `Prática 1\PROC-042-v2-frete-especial-revisado.md`
- **Resumo:** [Prática 1/Resumos-Discovery/resumo-PROC-042-v2-frete-especial-revisado.md](../Prática%201/Resumos-Discovery/resumo-PROC-042-v2-frete-especial-revisado.md)
- **Principais achados:** multiplicadores regionais revisados (mais altos que v1), fator de peso reduzido nas faixas superiores, prazo +3 dias úteis, desconto de volume com percentuais objetivos (5%/10%), disposições transitórias com corte em 01/12/2023. **Achado crítico:** a cláusula de transição pressupõe a v2 como vigente, mas o próprio cabeçalho nega substituição formal da v1 — contradição interna, e a data de corte já está expirada em relação à data desta análise. Resumo inclui tabela comparativa v1 x v2.

### 3.5. RES-005 — FAQ-Atendimento
- **Origem:** `Prática 1\FAQ-atendimento.md`
- **Resumo:** [Prática 1/Resumos-Discovery/resumo-FAQ-atendimento.md](../Prática%201/Resumos-Discovery/resumo-FAQ-atendimento.md)
- **Principais achados:** documento informal, não validado por Compliance/Operações, com 9 de 47 perguntas analisadas. Confirma o conflito de versões do PROC-042 e revela que a prática de desconto de volume em uso no atendimento segue a regra **antiga (v1)**. Revela um processo não documentado (seguro de carga) e um possível conflito de fluxo entre "carga danificada" (via Jurídico/sinistro) e "avaria em trânsito" (POL-001, tratada como devolução padrão).

---

## 4. Achados consolidados transversais (cross-cutting), identificados ao longo da sessão

1. **Conflito de governança documental crítico:** PROC-042 v1 e v2 coexistem sem hierarquia formal declarada; a cláusula de transição da v2 está tecnicamente expirada. Prioridade máxima para resolução antes de qualquer automação/IA se apoiar neste conteúdo.
2. **Prática operacional desatualizada:** o FAQ de atendimento orienta desconto de volume conforme a regra da v1, não da v2 — evidência concreta de dessincronia entre norma vigente (mesmo que ambígua) e prática de campo.
3. **Possível conflito de fluxo:** "carga danificada" (FAQ, tratada pelo Jurídico) vs. "avaria em trânsito" (POL-001, tratada como devolução padrão) — não fica claro se são o mesmo evento.
4. **Lacuna documental:** processo de seguro de carga (citado apenas no FAQ) não tem nenhum documento normativo de referência entre os 5 arquivos.
5. **Relação não explicitada:** os SLAs gerais (SLA-2024) não são formalmente conectados aos prazos internos de processos específicos (POL-001, PROC-042).

---

## 5. Encerramento da sessão

O usuário solicitou o encerramento desta sessão com dois pedidos:

1. Renomear a sessão para "001-Geração de resumos".
2. Salvar o histórico da sessão neste arquivo.

Sobre o primeiro pedido, Claude não possui ferramenta para renomear o título da sessão diretamente — essa ação precisa ser feita manualmente pelo usuário na interface da extensão VSCode.

O trabalho de consolidação de hipóteses e riscos a partir dos 5 resumos ficou definido para uma **nova sessão**.

---

## 6. Arquivos gerados nesta sessão

| Arquivo | Caminho |
|---|---|
| Resumo RES-001 | `Prática 1\Resumos-Discovery\resumo-POL-001-politica-devolucao.md` |
| Resumo RES-002 | `Prática 1\Resumos-Discovery\resumo-PROC-042-frete-especial-v1.md` |
| Resumo RES-003 | `Prática 1\Resumos-Discovery\resumo-SLA-2024-tabela-sla-clientes.md` |
| Resumo RES-004 | `Prática 1\Resumos-Discovery\resumo-PROC-042-v2-frete-especial-revisado.md` |
| Resumo RES-005 | `Prática 1\Resumos-Discovery\resumo-FAQ-atendimento.md` |
| Este histórico | `Prompts-Histórico\001-Geração de resumos.md` |

## 7. Pendências para a próxima sessão

- Consolidar hipóteses, riscos e processos a partir dos 5 resumos (RES-001 a RES-005).
- Resolver junto a stakeholders qual versão do PROC-042 (v1 ou v2) está de fato vigente.
- Esclarecer a fronteira entre "carga danificada" (sinistro) e "avaria em trânsito" (devolução).
- Levantar documentação normativa do processo de seguro de carga.
- Avaliar se os SLAs gerais (SLA-2024) cobrem ou não os prazos internos de processos específicos.