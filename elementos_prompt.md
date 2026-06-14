# Elementos de um Prompt

Um prompt bem estruturado aumenta significativamente a qualidade das respostas geradas por modelos de IA Generativa. Diferentes pesquisadores e organizações propõem conjuntos distintos de elementos para compor um prompt eficaz. A seguir, são apresentadas três referências amplamente utilizadas.

---

## 1. Schulhoff, Sander et al.

Modelo proposto em pesquisa acadêmica que organiza os elementos de um prompt em cinco componentes:

| Elemento | Descrição |
|---|---|
| **Diretriz ou intenção** | O objetivo principal do prompt — o que se espera que o modelo faça. |
| **Formato de saída** | Como a resposta deve ser apresentada (lista, tabela, parágrafo, JSON, etc.). |
| **Instruções de estilo** | Tom, linguagem e estilo da resposta (formal, técnico, didático, etc.). |
| **Papel** | O papel ou persona que o modelo deve assumir (ex.: "aja como um especialista em..."). |
| **Informação adicional** | Contexto, exemplos ou dados complementares que auxiliam o modelo a gerar uma resposta mais precisa. |

---

## 2. DAIR — The Distributed Artificial Intelligence Research Institute

O DAIR propõe uma estrutura mais enxuta, focada na clareza da comunicação entre usuário e modelo:

| Elemento | Descrição |
|---|---|
| **Instrução** | A tarefa ou ação que o modelo deve executar. |
| **Contexto** | Informações de fundo que ajudam o modelo a entender a situação ou o domínio. |
| **Dados de Entrada** | Os dados concretos sobre os quais o modelo deve operar ou analisar. |
| **Indicador de Saída** | Sinalização sobre o tipo, formato ou estrutura esperados na resposta. |

---

## 3. Google

O modelo do Google é o mais completo e detalhado, distinguindo elementos obrigatórios dos opcionais:

| Elemento | Obrigatório | Descrição |
|---|:---:|---|
| **Objetivo** | ✅ | O que o modelo deve realizar — a meta central do prompt. |
| **Instruções** | ✅ | Passos ou orientações específicas sobre como executar a tarefa. |
| **Restrições** | ❌ | O que o modelo deve ou não deve fazer — limites e proibições. |
| **Contexto** | ❌ | Informações de fundo relevantes para contextualizar a tarefa. |
| **Formato de saída** | ❌ | Estrutura e apresentação esperadas para a resposta. |
| **Exemplos** | ❌ | Amostras (*few-shot*) que ilustram o comportamento desejado. |
| **Recap** | ❌ | Resumo ou reforço das instruções principais ao final do prompt, para evitar que o modelo as ignore. |

---

## Minha Estrutura Preferida

Baseada no modelo do Google, esta é a estrutura recomendada para construção de prompts eficazes:

```
## Objetivo
[Descreva claramente o que o modelo deve fazer]

## Instruções
1. [Primeiro passo ou orientação]
2. [Segundo passo ou orientação]
3. [Terceiro passo ou orientação]

## Restrições
Faça: [comportamentos desejados]
Não faça: [comportamentos a evitar]

## Contexto
[Informações de fundo relevantes para a tarefa]

## Formato de saída
[Estrutura e apresentação esperadas para a resposta]

## Exemplos
[Exemplos de entrada e saída para guiar o modelo]

## Recap
[Reforço das instruções mais importantes]
```
