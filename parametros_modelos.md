# Parâmetros de Inferência dos Modelos

Os parâmetros de inferência controlam como um modelo gera texto. Eles influenciam a amostragem de tokens, o nível de aleatoriedade, a repetição, a parada da geração e o tamanho da resposta. Em geral, ajustar esses parâmetros permite equilibrar **criatividade**, **precisão**, **consistência** e **custo**.

## Temperature x Top-k x Top-p

Os três parâmetros abaixo atuam principalmente sobre a amostragem e ajudam a controlar o risco de respostas instáveis, pouco coerentes ou excessivamente repetitivas.

| Parâmetro | Camada | O que representa | Exemplo humano |
|---|---|---|---|
| **Temperature** | Emocional / tonal | Grau de “entusiasmo” do pensamento | Pessoa calma ou pessoa efusiva |
| **Top-k** | Cognitiva / lexical | Quantidade de ideias disponíveis | Pessoa que pensa com 10 palavras ou que pensa com 100 palavras |
| **Top-p** | Instintiva / narrativa | Grau de autoconfiança antes de decidir | Pessoa que fala só quando tem certeza ou aquela que fala livremente |

**Leitura prática da tabela:**
- **Temperature** altera o quanto o modelo “arrisca” na escolha das próximas palavras.
- **Top-k** limita quantas opções ficam disponíveis para seleção em cada passo.
- **Top-p** limita o conjunto de escolhas com base na probabilidade acumulada.

Esses controles ajudam a reduzir o risco de **alucinação**, especialmente quando você precisa de respostas mais estáveis, rastreáveis e coerentes.

## 1. Temperature

A **temperature** controla o grau de aleatoriedade na escolha dos próximos tokens.

- Valores mais baixos deixam a resposta mais previsível e conservadora.
- Valores mais altos aumentam a variedade e a criatividade.

**Uso prático:**
- Baixa temperatura para tarefas técnicas, classificação e respostas objetivas.
- Alta temperatura para brainstorming, escrita criativa e exploração de ideias.

## 2. Top-k

O **top-k** limita a amostragem aos $k$ tokens mais prováveis em cada passo de geração.

- Com um $k$ menor, o modelo fica mais restrito.
- Com um $k$ maior, há mais liberdade de escolha.

**Uso prático:** ajuda a reduzir saídas muito dispersas e torna a geração mais controlada.

## 3. Top-p

O **top-p** também é chamado de *nucleus sampling*. Em vez de escolher um número fixo de tokens, ele considera o menor conjunto de tokens cuja probabilidade acumulada atinge o valor $p$.

- Valores menores deixam o modelo mais seletivo.
- Valores maiores permitem mais diversidade.

**Uso prático:** é útil quando você quer manter qualidade sem prender o modelo a um número fixo de opções.

## 4. Max Tokens

O **max tokens** define o limite máximo de tokens que o modelo pode gerar na resposta.

- Evita respostas muito longas.
- Ajuda a controlar custo e tempo de execução.

**Uso prático:** importante quando você precisa de saídas curtas, como resumos, classificações ou respostas padronizadas.

## 5. Presence Penalty

A **presence penalty** penaliza o uso de tokens ou temas que já apareceram na resposta, incentivando o modelo a introduzir conteúdo novo.

- Valores mais altos aumentam a chance de explorar assuntos diferentes.
- Valores baixos mantêm o foco no que já foi mencionado.

**Uso prático:** útil em brainstorming e geração de ideias para evitar repetição de tópicos.

## 6. Frequency Penalty

A **frequency penalty** penaliza a repetição de tokens que já apareceram muitas vezes.

- Ajuda a reduzir redundância.
- Favorece respostas mais variadas e menos repetitivas.

**Uso prático:** útil em textos longos, explicações e listas em que o modelo pode repetir palavras ou expressões.

## 7. Stop Sequences

As **stop sequences** são sequências de texto que indicam ao modelo quando parar de gerar.

- Quando uma dessas sequências aparece, a geração é interrompida.
- Permite controlar o formato final da saída.

**Uso prático:** muito útil quando você quer respostas estruturadas e precisa evitar texto extra após um marcador específico.

## 8. Seed

A **seed** define um valor inicial para o processo de geração aleatória.

- Com a mesma seed e os mesmos parâmetros, é mais fácil reproduzir resultados semelhantes.
- Diferentes seeds podem gerar variações na resposta.

**Uso prático:** importante para testes, comparação de prompts e experimentos reprodutíveis.

## 9. Context Window

A **context window** é a quantidade máxima de tokens que o modelo consegue considerar ao mesmo tempo, somando entrada e saída.

- Janelas maiores permitem trabalhar com textos mais longos.
- Janelas menores podem limitar o quanto o modelo “lembra” da conversa ou do documento.

**Uso prático:** essencial em análises longas, chat com histórico e tarefas que exigem considerar muito contexto.

## Resumo Prático

Uma forma simples de pensar nesses parâmetros é:

- **Temperature, top-k e top-p** controlam a aleatoriedade da geração.
- **Max tokens** controla o tamanho da resposta.
- **Presence penalty e frequency penalty** controlam repetição e diversidade.
- **Stop sequences** controlam onde a geração termina.
- **Seed** ajuda na reprodutibilidade.
- **Context window** limita quanto conteúdo o modelo consegue considerar.

Na prática, para tarefas de análise e segurança, costuma funcionar bem usar **temperature baixa**, **saída curta**, **pouca repetição** e **formato bem definido**.
