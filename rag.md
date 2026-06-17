# RAG e CAG: o que são e como se diferenciam

## Visão geral

Modelos de linguagem (LLMs) podem gerar respostas excelentes, mas têm um limite importante: o conhecimento interno pode estar desatualizado ou incompleto. Para resolver isso, surgiram estratégias de **aumentar o contexto** com informações externas.

Duas abordagens comuns são:

- **RAG (Retrieval-Augmented Generation)**
- **CAG (Cache-Augmented Generation)**

Ambas buscam melhorar qualidade, factualidade e utilidade das respostas, mas funcionam de formas diferentes.

---

## O que é RAG (Retrieval-Augmented Generation)

No **RAG**, a cada pergunta do usuário o sistema:

1. Recebe a consulta.
2. Busca documentos relevantes em uma base de conhecimento (vetorial, híbrida, etc.).
3. Envia trechos recuperados como contexto para o LLM.
4. O LLM gera a resposta com base nesse contexto + na pergunta.

### Vantagens do RAG

- Atualiza conhecimento sem re-treinar o modelo.
- Funciona bem para bases grandes e dinâmicas.
- Permite citar fontes e melhorar rastreabilidade.

### Limitações do RAG

- Custo e latência de busca em tempo de inferência.
- Qualidade depende muito da etapa de recuperação (retrieval).
- Pode trazer trechos irrelevantes se a busca estiver mal calibrada.

---

## RAG estático vs RAG dinâmico

Dentro do universo de RAG, também é comum separar duas formas de operação: **RAG estático** e **RAG dinâmico** (muitas vezes chamado de **RAG agêntico**).

### O que é RAG estático

No **RAG estático**, o fluxo é fixo e geralmente ocorre uma única vez por pergunta:

1. Recupera documentos.
2. Injeta contexto no prompt.
3. Gera a resposta.

Esse padrão é simples, previsível e fácil de operar. É muito usado como baseline em aplicações corporativas.

### O que é RAG dinâmico

No **RAG dinâmico**, o sistema decide de forma adaptativa:

- se precisa recuperar mais dados,
- quais fontes consultar,
- e quantas iterações executar até ter evidência suficiente.

Em vez de um fluxo único, há um **loop de decisão**: recuperar -> avaliar -> recuperar novamente (se necessário) -> responder.

### Diferença prática

| Aspecto | RAG estático | RAG dinâmico |
|---|---|---|
| Número de recuperações | Normalmente 1 | Variável (1..N) |
| Estratégia | Pipeline fixo | Estratégia adaptativa |
| Custo/latência | Mais previsível | Pode variar por consulta |
| Qualidade em casos complexos | Pode ser limitada | Tende a melhorar com iteração |
| Implementação | Mais simples | Mais complexa (orquestração/agent loop) |

### Quando usar

Use **RAG estático** quando:

- o domínio é bem delimitado,
- a consulta tende a ser direta,
- e previsibilidade de custo/tempo é prioridade.

Use **RAG dinâmico** quando:

- as perguntas exigem investigação em múltiplas fontes,
- é necessário validar hipóteses ao longo do processo,
- e a tarefa é mais analítica (ex.: triagem de alertas, threat hunting, pesquisa técnica).

---

## O que é CAG (Cache-Augmented Generation)

No **CAG**, parte do conhecimento é **pré-carregada em cache** antes das consultas (offline). Durante a inferência, o modelo consulta esse cache de contexto já preparado, reduzindo ou evitando busca ativa em tempo real.

Fluxo típico:

1. Selecionar conhecimento importante previamente.
2. Pré-processar e armazenar em cache (context cache).
3. Em tempo de pergunta, usar esse cache diretamente no LLM.

### Vantagens do CAG

- Menor latência em consultas repetidas ou previsíveis.
- Menor custo de retrieval online.
- Pode aumentar estabilidade em cenários com perguntas recorrentes.

### Limitações do CAG

- Risco de cache desatualizado.
- Menor flexibilidade para perguntas fora do escopo pré-carregado.
- Exige estratégia de atualização/invalidação de cache.

---

## Comparação rápida: RAG vs CAG

| Critério | RAG | CAG |
|---|---|---|
| Recuperação de conhecimento | Em tempo real (online) | Pré-carregado (offline) |
| Latência | Maior (depende da busca) | Menor em consultas frequentes |
| Atualização de conteúdo | Alta (base pode mudar continuamente) | Depende da renovação de cache |
| Flexibilidade | Alta para perguntas novas | Melhor para domínio mais estável |
| Complexidade operacional | Pipeline de retrieval robusto | Gestão de cache robusta |

---

## Quando usar cada um

Use **RAG** quando:

- O conhecimento muda com frequência.
- Você precisa cobrir perguntas abertas e variadas.
- A prioridade é flexibilidade e atualização contínua.

Use **CAG** quando:

- Há muitas consultas repetidas.
- O domínio é relativamente estável.
- A prioridade é baixa latência e previsibilidade.

---

## Estratégia híbrida (prática comum)

Na prática, muitos sistemas combinam os dois:

- **Cache para o “hot set”** (conteúdo mais acessado).
- **RAG online para cauda longa** (perguntas menos frequentes ou novas).

Essa abordagem híbrida tende a equilibrar desempenho, custo e qualidade.

---

## Conclusão

**RAG** e **CAG** não são concorrentes diretos em todos os casos; eles resolvem necessidades diferentes.

- **RAG** prioriza atualização e cobertura ampla.
- **CAG** prioriza velocidade e eficiência para contextos previsíveis.

Escolher entre um, outro, ou combinar ambos depende do seu cenário de produto, volume de consultas e necessidade de atualização do conhecimento.
