# Resumo — POL-001 Política de Devolução de Mercadorias

## 1. Metadados / índice base

- **ID do resumo:** RES-001
- **Arquivo de origem:** POL-001-politica-devolucao.md
- **Tipo de documento:** Política (documento normativo, uso obrigatório pelo time de atendimento)
- **Categoria/tema:** Logística reversa — Devolução de mercadorias
- **Palavras-chave:** devolução, prazo de 7 dias úteis, cargas perigosas (ANTT), cadeia de frio, lacre de segurança, CT-e, coleta reversa, reembolso, frete reverso
- **Processos relacionados:** Procedimento de devolução (portal do cliente), triagem de chamado, coleta reversa, reembolso/crédito, encaminhamento à Gestão de Riscos, encaminhamento ao Comercial
- **Versão do documento de origem:** 3.1 (última atualização 15/01/2024)
- **Data desta análise:** 26/07/2026

## 2. Resumo executivo

O POL-001 normatiza as devoluções de mercadorias após entrega pela NovaTech, com prazo geral de 7 dias úteis contados do recebimento confirmado em sistema de tracking. Define três categorias de exceção que bloqueiam o processo padrão (cargas perigosas ANTT classes 1-6, ruptura de cadeia de frio e lacre violado sem documentação), encaminhando-as à Gestão de Riscos. Detalha um fluxo operacional com prazos internos (4h para triagem, 2 dias úteis para coleta reversa, 5 dias úteis para reembolso) e regras de rateio de custo do frete reverso conforme a responsabilidade pela devolução (erro da NovaTech vs. desistência do cliente).

## 3. Processos e regras de negócio identificados

- **Prazo geral:** até 7 dias úteis após confirmação de recebimento no tracking (exclui sábados, domingos e feriados nacionais).
- **Abertura do chamado:** via Portal do Cliente, categoria "Devolução de Mercadoria", com número do CT-e, mínimo de 3 fotos (embalagem externa, etiqueta, conteúdo) e motivo.
- **Triagem:** time de atendimento tem 4 horas úteis para verificar elegibilidade, documentação e prazo.
- **Coleta reversa:** agendada em até 2 dias úteis após aprovação, se elegível.
- **Reembolso/crédito:** processado em até 5 dias úteis após recebimento da mercadoria no centro de distribuição.
- **Devoluções parciais:** permitidas por volume individual em entregas multi-volume; reembolso proporcional ao peso/valor do volume, conforme CT-e.
- **Rateio de custo do frete reverso:**
  - Erro/defeito da NovaTech → devolução sem custo ao cliente.
  - Desistência do cliente (carga correta, sem defeito) → custo do frete reverso é do cliente.
  - Solicitação fora do prazo de 7 dias → não elegível pelo processo padrão; encaminhado ao Comercial para negociação caso a caso.
- **Exceções ao prazo geral:** cargas perigosas (ANTT classes 1-6), cadeia de frio rompida (>30 min fora da faixa, via sensor IoT) e lacre violado sem documentação no ato da entrega — todas encaminhadas ao setor de Gestão de Riscos (ramal 4500) para tratamento individual.

## 4. Hipóteses levantadas

- Assume-se que o sistema de tracking é a fonte única e confiável da data de recebimento para contagem do prazo de 7 dias.
- Assume-se que os sensores IoT de cadeia de frio funcionam corretamente e de forma contínua, sem falhas que possam gerar disputas sobre elegibilidade.
- Assume-se que o cliente tem acesso e domínio do Portal do Cliente para abrir o chamado com todos os anexos exigidos.
- Assume-se que o time de atendimento consegue cumprir o SLA interno de 4 horas úteis de triagem em qualquer volume de chamados.
- Assume-se que "feriados nacionais" é um calendário único e já parametrizado no sistema de tracking, sem necessidade de tratamento de feriados estaduais/municipais.
- Assume-se que o setor de Gestão de Riscos tem capacidade e SLA próprio para tratar os casos de exceção encaminhados (não definido neste documento).

## 5. Riscos identificados

- **Risco técnico:** dependência do sensor IoT para comprovar ruptura de cadeia de frio; falha ou descalibração do sensor pode gerar decisões de elegibilidade incorretas e contestações do cliente.
- **Risco de ambiguidade textual:** a expressão "documentada no ato de entrega com assinatura do motorista e do recebedor" (seção 3.2) não define um formato/canal padrão de documentação, abrindo espaço para interpretações divergentes entre operadores.
- **Risco de compliance:** classificação de cargas perigosas depende de aderência correta à Resolução ANTT nº 5.947/2021; erro de classificação pode gerar devoluções indevidas ou bloqueios incorretos.
- **Risco operacional:** cumprimento dos prazos internos (4h triagem, 2 dias coleta, 5 dias reembolso) pode não escalar em picos de volume, gerando descumprimento do próprio processo normativo.
- **Risco financeiro:** cálculo proporcional de reembolso em devoluções parciais (por peso/valor do volume no CT-e) pode ser contestado pelo cliente se a base de cálculo não for transparente.
- **Risco de canal único:** o ramal 4500 (Gestão de Riscos) como único ponto de contato para todas as exceções pode se tornar gargalo operacional.

## 6. Lacunas e perguntas em aberto

- Não há definição de calendário oficial de "feriados nacionais" usado para contagem de dias úteis.
- Não é especificado o SLA do setor de Gestão de Riscos para tratar os casos excepcionais encaminhados (prazo de resposta ao cliente).
- Não é detalhado o processo/critério do Comercial para negociação de devoluções fora do prazo (seção 3.5) — decisão fica integralmente "caso a caso", sem critérios objetivos.
- Não há changelog ou indicação do que mudou entre a versão atual (3.1) e versões anteriores da política.
- Não é especificado quem audita/valida as fotos enviadas pelo cliente, nem o que ocorre se as fotos forem insuficientes ou inconclusivas.
- Não é dito o que ocorre se o cliente não possuir o número do CT-e no momento da abertura do chamado.
- Não fica claro se o prazo de 7 dias dias úteis pode ser estendido em algum cenário (ex.: falha comprovada do próprio sistema de tracking).

## 7. Termos e definições específicas do domínio

- **CT-e:** Conhecimento de Transporte Eletrônico, documento fiscal que acompanha a carga.
- **ANTT:** Agência Nacional de Transportes Terrestres; referência regulatória para classificação de cargas perigosas (classes 1 a 6).
- **Cadeia de frio:** manutenção da temperatura da carga dentro da faixa especificada na nota fiscal; ruptura = saída da faixa por mais de 30 minutos contínuos.
- **Lacre de segurança:** selo físico de inviolabilidade da carga; sua violação sem documentação bloqueia a devolução padrão.
- **Coleta reversa:** processo logístico de retirada da mercadoria devolvida na origem do cliente.
- **Frete reverso:** custo do transporte da devolução, cuja responsabilidade (NovaTech ou cliente) depende do motivo da devolução.

## 8. Referências cruzadas e dependências

- **PROC-088 — Procedimento de Interceptação de Carga:** referenciado explicitamente na seção 2 (Escopo) para casos de mercadoria ainda em trânsito. Este documento não faz parte do conjunto de 5 arquivos em análise até o momento — sinalizar necessidade de obtê-lo caso apareça dependência relevante nas próximas fases.
- **Setor de Gestão de Riscos** e **Comercial:** mencionados como responsáveis por tratativas de exceção, mas sem documento de referência próprio citado nesta política.
- **Possível sobreposição a confirmar:** os prazos internos desta política (triagem, coleta, reembolso) podem se relacionar com os SLAs formais descritos em SLA-2024-tabela-sla-clientes.md, e o rateio de custo de frete reverso pode se relacionar com as regras de PROC-042 (frete especial) — ambos ainda não analisados neste momento; recomenda-se checagem cruzada quando esses arquivos forem processados.

## 9. Recomendações para a fase de Discovery

- Solicitar acesso ao PROC-088 para entender o tratamento de devoluções de cargas em trânsito e mapear a fronteira entre os dois processos.
- Confirmar com stakeholders o calendário de feriados nacionais utilizado pelo sistema de tracking.
- Levantar o SLA real do setor de Gestão de Riscos para tratamento de exceções (cargas perigosas, cadeia de frio, lacre violado).
- Entender os critérios objetivos (se existirem) usados pelo Comercial para negociação de devoluções fora do prazo.
- Ao processar SLA-2024 e os arquivos PROC-042 (v1 e v2), verificar consistência de prazos e de regras de custo de frete reverso com o que está definido nesta política.
- Investigar histórico de disputas relacionadas a falhas de sensor IoT, para dimensionar o risco técnico como algo já materializado ou apenas teórico.