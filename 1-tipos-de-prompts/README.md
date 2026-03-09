# Tipos de Prompts: 9 Tecnicas Essenciais de Prompt Engineering

Este modulo apresenta as principais tecnicas de prompt engineering, organizadas em ordem progressiva de complexidade. Cada tecnica possui um script Python correspondente com exemplos praticos utilizando a API da OpenAI.

---

## 0. Role-based Prompting

**Arquivo:** `0-Role-prompting.py`

Role-based prompting consiste em atribuir uma persona ou papel especifico ao modelo antes de fazer a pergunta. Ao definir quem o modelo "e", controlamos o tom, a profundidade tecnica e o estilo da resposta.

**Como funciona:** O prompt inicia com uma instrucao do tipo "Voce e um professor universitario de Ciencia da Computacao" ou "Voce e um estudante do ensino medio". O modelo adapta vocabulario, nivel de detalhe e abordagem de acordo com o papel atribuido.

**Quando usar:**
- Quando o formato ou tom da resposta importa tanto quanto o conteudo
- Para adaptar explicacoes ao nivel de conhecimento do publico-alvo
- Para obter perspectivas diferentes sobre o mesmo tema

**Exemplo do projeto:** O script pede que dois papeis diferentes (professor universitario e estudante) expliquem recursao em 50 palavras. O professor usa pseudocodigo e linguagem formal; o estudante usa analogias simples.

---

## 1. Zero-shot Prompting

**Arquivo:** `1-zero-shot.py`

Zero-shot e a forma mais direta de prompting: voce faz a pergunta sem fornecer nenhum exemplo previo. O modelo depende exclusivamente do conhecimento adquirido durante o treinamento para gerar a resposta.

**Como funciona:** O prompt contem apenas a instrucao ou pergunta, sem exemplos demonstrativos. A qualidade da resposta depende da clareza e precisao da instrucao.

**Quando usar:**
- Tarefas simples e objetivas (perguntas factuais, classificacoes claras)
- Quando nao ha exemplos disponiveis ou necessarios
- Como baseline antes de tentar tecnicas mais sofisticadas

**Exemplo do projeto:** O script demonstra perguntas diretas como "Qual e a capital do Brasil?" e deteccao de intencao em texto, mostrando como instrucoes mais especificas ("responda apenas com o nome da cidade") refinam a saida.

---

## 2. Few-shot Prompting (One-shot e Few-shot)

**Arquivo:** `2-one-few-shot.py`

Few-shot prompting fornece exemplos no prompt para que o modelo aprenda o padrao esperado antes de responder. One-shot usa um unico exemplo; few-shot usa varios.

**Como funciona:** O prompt inclui pares de entrada/saida que demonstram o comportamento desejado. O modelo identifica o padrao e o aplica a nova entrada.

**Quando usar:**
- Tarefas de classificacao ou formatacao especifica
- Quando o modelo erra consistentemente em zero-shot
- Para definir formatos de saida sem descricoes longas

**Exemplo do projeto:** O script progride de 1 exemplo (capital da Franca -> Paris, agora pergunte sobre o Brasil) ate 15 exemplos de classificacao de severidade de logs, mostrando que mais exemplos melhoram a precisao em tarefas ambiguas.

**Observacao:** Existe um ponto de retorno decrescente — apos certa quantidade, exemplos adicionais nao melhoram significativamente o resultado.

---

## 3. Chain of Thought (CoT)

**Arquivo:** `3-CoT.py`

Chain of Thought (Cadeia de Pensamento) instrui o modelo a raciocinar passo a passo antes de chegar a conclusao final. Essa tecnica melhora drasticamente a precisao em tarefas que exigem logica ou calculo.

**Como funciona:** Adiciona-se ao prompt uma instrucao como "pense passo a passo" ou "explique seu raciocinio". O modelo decompoe o problema em etapas intermediarias antes de responder.

**Quando usar:**
- Problemas matematicos ou logicos
- Tarefas que exigem raciocinio em multiplas etapas
- Quando o modelo erra em respostas diretas

**Exemplo do projeto:** O script compara respostas com e sem CoT. Na tarefa de contar a letra "r" em "strawberry", sem CoT o modelo frequentemente erra; com CoT, ele analisa letra por letra e acerta.

### 3.1 CoT com Self-Consistency

**Arquivo:** `3.1-CoT-Self-consistency.py`

Variacao do CoT onde o modelo gera multiplos caminhos de raciocinio e seleciona a resposta mais consistente entre eles. Aumenta a confiabilidade em problemas complexos.

**Exemplo do projeto:** O script pede 3 caminhos de raciocinio diferentes para identificar um problema de N+1 queries em codigo Go, selecionando a resposta que aparece com mais frequencia.

---

## 4. Tree of Thoughts (ToT)

**Arquivo:** `4-ToT.py`

Tree of Thoughts expande o Chain of Thought para explorar multiplas ramificacoes de raciocinio simultaneamente, como uma arvore de decisao. Cada ramificacao e avaliada antes de escolher o melhor caminho.

**Como funciona:** O prompt solicita que o modelo gere multiplas hipoteses ou abordagens, avalie cada uma com criterios definidos e selecione a mais promissora.

**Quando usar:**
- Problemas com multiplas solucoes possiveis
- Decisoes arquiteturais que exigem comparacao de trade-offs
- Diagnostico de problemas com diversas causas potenciais

**Exemplo do projeto:** O script apresenta dois cenarios — debug de latencia em API (gerando 3+ hipoteses, avaliando probabilidade de cada uma) e design de arquitetura para processamento de imagens (comparando opcoes por escalabilidade, custo e complexidade).

---

## 5. Skeleton of Thought (SoT)

**Arquivo:** `5-SoT.py`

Skeleton of Thought divide a geracao de resposta em duas fases: primeiro cria um esqueleto (estrutura topica), depois expande cada ponto do esqueleto com detalhes.

**Como funciona:** Na primeira fase, o modelo produz uma lista de topicos ou pontos-chave. Na segunda fase, cada ponto e expandido com conteudo detalhado. Isso permite paralelismo na expansao e respostas mais organizadas.

**Quando usar:**
- Respostas longas e estruturadas (artigos, ADRs, guias)
- Quando a organizacao da resposta e tao importante quanto o conteudo
- Para gerar documentacao tecnica completa

**Exemplo do projeto:** O script gera um Architecture Decision Record (ADR) sobre PostgreSQL vs MongoDB, primeiro criando o esqueleto com 5 secoes e depois expandindo cada uma com argumentos detalhados.

---

## 6. ReAct (Reasoning + Acting)

**Arquivo:** `6-ReAct.py`

ReAct e uma tecnica de prompt engineering que intercala raciocinio (Thought) com acoes concretas (Action) e observacoes dos resultados (Observation). Esse ciclo iterativo permite que o modelo resolva problemas de forma mais fundamentada, ancorando cada passo em evidencias.

**Como funciona:** O prompt estrutura a resposta em ciclos de:
1. **Thought** — o modelo raciocina sobre o estado atual do problema
2. **Action** — o modelo executa uma acao (consultar dados, inspecionar codigo, buscar informacao)
3. **Observation** — o modelo analisa o resultado da acao

Esse ciclo se repete ate chegar a uma conclusao.

**Quando usar:**
- Debug e investigacao de problemas em codigo
- Tarefas que exigem consulta a fontes de informacao
- Planejamento com multiplas etapas dependentes

**Exemplo do projeto:** O script apresenta dois cenarios — debug de um endpoint POST /products retornando HTTP 500 (com codigo Go como contexto) e planejamento de viagem avaliando diferentes opcoes de transporte. Em ambos, o modelo alterna entre pensar, agir e observar.

---

## 7. Prompt Chaining

**Arquivo:** `7-Prompt-channing.py`

Prompt chaining conecta multiplos prompts em sequencia, onde a saida de um prompt se torna a entrada do proximo. Cada etapa pode usar um modelo diferente, otimizando custo e capacidade.

**Como funciona:** O problema e dividido em etapas discretas. Cada etapa recebe um prompt especializado, e seu resultado alimenta a etapa seguinte. Isso permite que tarefas complexas sejam resolvidas de forma modular.

**Quando usar:**
- Pipelines de transformacao de dados ou codigo
- Quando diferentes etapas exigem diferentes capacidades
- Para manter cada prompt focado e com escopo reduzido

**Exemplo do projeto:** O script encadeia tres modelos em um pipeline:
1. **GPT-3.5-turbo** (engenheiro backend) — converte especificacao em JSON Schema
2. **GPT-5-mini** (desenvolvedor Go) — transforma o schema em rotas REST com handlers
3. **GPT-4o-mini** (desenvolvedor pragmatico) — gera uma mensagem de commit a partir do schema e das rotas

---

## 8. Least-to-Most Decomposition

**Arquivo:** `8-Least-to-most.py`

Least-to-Most (do menor ao maior) decompoe um problema complexo em subproblemas ordenados do mais simples ao mais complexo. Cada subproblema e resolvido individualmente, e as solucoes anteriores servem de contexto para os proximos.

**Como funciona:** O prompt solicita que o modelo:
1. Identifique os subproblemas necessarios
2. Ordene do mais simples ao mais complexo
3. Resolva cada um sequencialmente, acumulando contexto

**Quando usar:**
- System design e arquitetura de sistemas
- Problemas grandes que precisam ser decompostos
- Quando a solucao de uma parte depende de partes anteriores

**Exemplo do projeto:** O script projeta um encurtador de URLs, decompondo em subproblemas (geracao de URLs unicas, armazenamento em memoria, migracao para banco de dados, escalabilidade) e resolvendo cada um progressivamente com implementacao em Go.

---

## Guia de Selecao Rapida

| Tecnica | Melhor para | Complexidade |
|---------|------------|--------------|
| Role-based | Adaptar tom e perspectiva | Baixa |
| Zero-shot | Tarefas simples e diretas | Baixa |
| Few-shot | Classificacao e formatacao | Baixa |
| Chain of Thought | Raciocinio logico e calculo | Media |
| CoT Self-Consistency | Problemas complexos com alta confiabilidade | Media |
| Tree of Thoughts | Decisoes com multiplas alternativas | Alta |
| Skeleton of Thought | Respostas longas e estruturadas | Media |
| ReAct | Debug, investigacao e planejamento | Alta |
| Prompt Chaining | Pipelines multi-etapa | Alta |
| Least-to-Most | Decomposicao de problemas grandes | Alta |

## Ordem Recomendada de Estudo

```
0-Role → 1-Zero-shot → 2-Few-shot → 3-CoT → 3.1-CoT-SC → 4-ToT → 5-SoT → 6-ReAct → 7-Chaining → 8-Least-to-Most
```

Comece pelas tecnicas mais simples. Adicione complexidade apenas quando a tecnica anterior nao resolver o problema.
