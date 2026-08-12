# Dicionário de Termos — Projeto NovaTech Assistant

> Este documento explica, em linguagem simples, os termos de processo, metodologia e tecnologia usados nos documentos desta fase do projeto. O objetivo é que qualquer pessoa — mesmo sem background técnico — consiga ler as specs e entender do que estamos falando.
>
> **Nota:** este dicionário cobre termos de *processo e tecnologia*. O vocabulário específico do negócio de logística da NovaTech (ex: frete especial, tiers de cliente, CT-e) fica no `domain-model.md`, junto com o recorte de domínio.

---

## Papéis do time

| Termo | O que significa |
|---|---|
| **Product Specialist** | Pessoa responsável por entender o problema de negócio e traduzir isso em requisitos claros para o time técnico. Escreve o "o quê" e o "por quê" — não o "como". |
| **Tech Lead** | Responsável técnico do time. Transforma os requisitos em um plano técnico de implementação e aprova decisões de arquitetura. |
| **Delivery Manager** | Responsável por acompanhar prazos, prioridades e a entrega do projeto como um todo. |
| **QA** | Responsável por testar o produto e mapear cenários de falha antes que cheguem ao usuário final. |

---

## Metodologia e processo

| Termo | O que significa |
|---|---|
| **Discovery** | Fase inicial do projeto, focada em entender o problema, os processos existentes e os dados disponíveis, antes de decidir o que construir. |
| **Spec** (especificação) | Documento que descreve, de forma estruturada, o que precisa ser construído — o problema a resolver, as regras de negócio e os critérios para saber se ficou pronto. |
| **SDD — Spec Driven Development** ("Desenvolvimento Guiado por Especificação") | Forma de trabalhar em que primeiro se escreve uma especificação clara e revisável, e só depois se parte para o código — inclusive quando quem "traduz" a especificação em código é um agente de IA. Reduz o risco de o time (humano ou IA) construir algo diferente do que foi pedido. |
| **Requirements.md** | Arquivo que contém os requisitos de um módulo: o que ele precisa fazer, para quem, e com quais regras. Escrito pelo Product Specialist. |
| **Plan.md** | Arquivo que contém o plano técnico de como um módulo será construído, a partir dos requisitos. Escrito pelo Tech Lead. |
| **Tasks.md** | Lista de tarefas técnicas concretas, quebradas a partir do plano, prontas para serem executadas (por uma pessoa ou por um agente de IA). |
| **ADR — Architecture Decision Record** ("Registro de Decisão Arquitetural") | Documento curto que registra uma decisão técnica importante: qual era o contexto, o que foi decidido, quais as consequências e quais alternativas foram consideradas. Serve para não perder o "porquê" de uma decisão com o tempo. |
| **Bounded Context** ("Contexto Delimitado") | Uma "área" do negócio com fronteiras claras, dentro da qual um mesmo termo tem sempre o mesmo significado. Por exemplo, "cliente" pode significar coisas diferentes na área de Cobrança e na área de Atendimento — cada uma dessas áreas é um bounded context. |
| **Linguagem Ubíqua** (*Ubiquitous Language*) | Vocabulário comum e sem ambiguidade que todo o time — pessoas e agentes de IA — deve usar ao falar sobre o negócio, para que todos entendam a mesma coisa pelos mesmos termos. |
| **Domain Model** ("Modelo de Domínio") | Documento que registra os bounded contexts, a linguagem ubíqua e as fronteiras entre o que o sistema faz e não faz. É a base de entendimento do negócio que outros documentos (specs, regras para agentes) vão usar. |

---

## Tecnologia e IA

| Termo | O que significa |
|---|---|
| **LLM — Large Language Model** ("Modelo de Linguagem") | O modelo de inteligência artificial que entende perguntas em linguagem natural e gera respostas em texto (ex: GPT-4o). |
| **RAG — Retrieval-Augmented Generation** ("Geração Aumentada por Busca") | Técnica em que, antes de responder, o sistema busca trechos relevantes em uma base de documentos e entrega esses trechos ao modelo de IA junto com a pergunta — para que a resposta seja baseada em informação real da empresa, e não "inventada" pelo modelo. |
| **Chunk** ("pedaço"/trecho) | Um trecho pequeno de um documento, extraído durante o processamento, que é usado como unidade de busca no RAG. Um documento grande é dividido em vários chunks. |
| **Embedding** | Uma representação numérica de um texto, usada pelo sistema para comparar o significado de textos entre si (por exemplo, para saber se um chunk é relevante para a pergunta feita). |
| **Pipeline de ingestão** | O processo automatizado que pega os documentos originais, extrai o texto, divide em chunks, gera embeddings e indexa tudo para que possam ser buscados depois. |
| **Prompt** | O texto de instrução que é enviado ao modelo de IA, combinando instruções fixas (system prompt) com a pergunta do usuário e os chunks recuperados. |
| **Guardrail** ("proteção"/trilho de segurança) | Uma regra ou limite colocado no sistema para evitar comportamentos indesejados — por exemplo, impedir que o assistente responda com informação que não está nos documentos oficiais. |
| **Fallback** ("desvio"/plano B) | O que o sistema faz quando não consegue responder com confiança pelo caminho normal — por exemplo, escalar a dúvida para uma pessoa em vez de arriscar uma resposta errada. |
| **Alucinação** | Quando um modelo de IA gera uma resposta que parece correta mas não é baseada em nenhuma fonte real — na prática, o modelo "inventa" uma informação. |
| **MCP — Model Context Protocol** | Um padrão que permite que agentes de IA se conectem a fontes de dados e ferramentas (arquivos, repositório Git, documentação) de forma organizada, para que possam consultar ou agir sobre essas fontes durante o trabalho. |
| **MCP server** | Um "conector" que implementa o protocolo MCP para uma fonte específica — por exemplo, um server que dá acesso a arquivos do projeto, ou outro que dá acesso ao histórico do Git. |
| **Skill** | Um arquivo que documenta um padrão reutilizável do projeto (uma convenção de código, uma forma de criar um teste, etc.), para que agentes de IA gerem código ou artefatos de forma consistente com o resto do projeto. |
| **AGENTS.md** | O documento "constitucional" do projeto: reúne as regras, limites e padrões que qualquer agente de IA (Copilot, Claude Code, etc.) deve seguir ao trabalhar no repositório. |
| **Agente de IA** | Uma ferramenta de IA que não só responde perguntas, mas também executa ações (ler arquivos, escrever código, rodar comandos) para ajudar a realizar uma tarefa. |

---

## Como manter este documento

- Sempre que um termo novo e não óbvio aparecer em algum artefato desta fase (spec, domain model, AGENTS.md, skills), adicionar aqui.
- Este dicionário é sobre termos de **processo e tecnologia**. Termos de **negócio/logística** (ex: frete especial, CT-e, tiers de cliente) pertencem ao `domain-model.md`.
