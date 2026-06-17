# Padrões de Prompt

Com base em *The Prompt Report: A Systematic Survey of Prompt Engineering Techniques* (2025), estes são seis padrões importantes de *prompting* e como aplicá-los na prática.

## 1. Few-Shot Prompting

No **few-shot prompting**, você inclui alguns exemplos de entrada e saída no próprio prompt para ensinar o modelo pelo padrão.

**Quando usar:**
- Quando a tarefa tem formato específico.
- Quando você quer consistência no estilo da resposta.

**Vantagem principal:** melhora a aderência ao formato desejado sem precisar de ajuste fino do modelo.

**Cuidado:** exemplos ruins ou ambíguos tendem a contaminar a resposta.

**Exemplo (análise de URL para firewall):**
```text
Você é um analista de cibersegurança. Classifique a URL como LIBERAR ou BLOQUEAR.
Use o formato: Decisão | Risco | Justificativa curta.

Exemplo 1
Entrada: http://login-microsoft-security-check.com
Saída: BLOQUEAR | Alto | Domínio suspeito com tentativa de se passar por marca conhecida.

Exemplo 2
Entrada: https://www.microsoft.com/security
Saída: LIBERAR | Baixo | Domínio oficial, HTTPS válido e contexto corporativo legítimo.

Agora analise:
https://update-account-verification.net
```

## 2. Zero-Shot Prompting

No **zero-shot prompting**, você não fornece exemplos; apenas descreve claramente a tarefa, contexto e restrições.

**Quando usar:**
- Tarefas simples ou conhecidas pelo modelo.
- Situações em que você quer prompts curtos e rápidos.

**Vantagem principal:** simplicidade e baixo custo de tokens.

**Cuidado:** pode haver mais variação de qualidade quando a instrução não está precisa.

**Exemplo (análise de URL para firewall):**
```text
Atue como analista de cibersegurança.
Avalie a URL abaixo e responda apenas no formato:
Decisão: LIBERAR ou BLOQUEAR
Risco: Baixo, Médio ou Alto
Motivo: até 2 linhas

URL: http://secure-payroll-check.employee-portal.info
```

## 3. Thought Generation (ex.: Chain-of-Thought)

Em **thought generation**, o prompt incentiva o modelo a decompor o raciocínio em etapas intermediárias antes de chegar à resposta final.

**Quando usar:**
- Problemas lógicos, matemáticos ou de decisão.
- Tarefas com múltiplas restrições.

**Vantagem principal:** melhora desempenho em problemas complexos por estruturar o raciocínio.

**Cuidado:** respostas podem ficar longas; para uso prático, peça síntese final objetiva.

**Exemplo (análise de URL para firewall):**
```text
Você é analista SOC.
Analise a URL em etapas:
1) Estrutura do domínio (typosquatting, TLD incomum, subdomínios enganosos)
2) Indicadores técnicos (HTTP/HTTPS, parâmetros suspeitos, encurtadores)
3) Contexto operacional (tipo de sistema afetado)
4) Decisão final

No final, responda só com:
Decisão final: LIBERAR ou BLOQUEAR
Confiança: baixa/média/alta
Resumo: 2 linhas

URL: https://accounts-google.com.security-check.ru/login
```

## 4. Ensembling

**Ensembling** combina múltiplas respostas (ou múltiplos prompts) para chegar a uma saída mais robusta, por exemplo por votação, comparação ou fusão.

**Quando usar:**
- Quando precisão é mais importante que velocidade.
- Quando a tarefa admite diferentes soluções e você quer reduzir erro.

**Vantagem principal:** maior confiabilidade ao reduzir dependência de uma única geração.

**Cuidado:** aumenta custo e tempo de execução.

**Exemplo (análise de URL para firewall):**
```text
Execute 3 análises independentes da URL abaixo:
- Análise A: foco em reputação e domínio
- Análise B: foco em engenharia social e phishing
- Análise C: foco em risco operacional no ambiente corporativo

Depois consolide por maioria e produza:
Decisão final: LIBERAR ou BLOQUEAR
Nível de consenso: 0 a 3
Justificativa final: até 3 linhas

URL: http://vpn-company-support-helpdesk.com/reset
```

## 5. Self-Criticism

No padrão de **self-criticism**, o modelo gera uma resposta e depois a revisa criticamente (checando coerência, falhas e melhorias) antes da versão final.

**Quando usar:**
- Produção de conteúdo técnico.
- Revisão de argumentos, planos ou explicações.

**Vantagem principal:** melhora qualidade final por meio de autoavaliação iterativa.

**Cuidado:** pode criar excesso de refinamento sem ganho real se o critério de revisão for vago.

**Exemplo (análise de URL para firewall):**
```text
Você é analista de cibersegurança.
Fase 1: gere uma decisão inicial para a URL (LIBERAR/BLOQUEAR) com justificativa.
Fase 2: critique sua própria análise, apontando possíveis falhas.
Fase 3: forneça decisão revisada.

Saída final obrigatória:
Decisão revisada: LIBERAR ou BLOQUEAR
O que mudou após revisão: 1-2 linhas

URL: https://storage-googleusercontent-secure-download.com/file
```

## 6. Decomposition

Na **decomposition**, você divide uma tarefa grande em subtarefas menores, resolvendo etapa por etapa.

**Quando usar:**
- Projetos longos ou perguntas complexas.
- Fluxos com dependência entre etapas.

**Vantagem principal:** facilita controle, rastreabilidade e correção de erros por fase.

**Cuidado:** exige bom desenho das etapas para evitar redundância.

**Exemplo (análise de URL para firewall):**
```text
Resolva a tarefa em blocos:
Bloco 1: Extraia características da URL (protocolo, domínio raiz, subdomínios, caminho).
Bloco 2: Classifique indicadores de risco (baixo/médio/alto) para cada característica.
Bloco 3: Aplique política:
- Se houver 2 ou mais indicadores altos => BLOQUEAR
- Se não houver indicadores altos e domínio for confiável => LIBERAR
Bloco 4: Emita decisão final e recomendação operacional.

URL: http://microsoft-login-verification-alerts.xyz/security-check
```

## Boas Práticas Complementares

Além dos padrões de *prompting*, a documentação da Anthropic destaca práticas de escrita de prompt que melhoram bastante a qualidade da resposta, independentemente do padrão escolhido.

### 1. Seja claro e direto

O modelo responde melhor quando a instrução é explícita, específica e sem ambiguidade. Em vez de esperar que ele “entenda a intenção”, é melhor declarar com precisão:

- o objetivo da tarefa;
- o formato esperado da resposta;
- as restrições mais importantes;
- os critérios de qualidade.

Em outras palavras, se uma pessoa com pouco contexto ficaria confusa ao ler o prompt, o modelo provavelmente também ficará.

### 2. Explique o contexto e a motivação

Não basta dizer *o que* fazer; muitas vezes ajuda dizer *por que* aquilo importa. Quando o modelo entende o contexto de uso, ele tende a produzir respostas mais alinhadas com a necessidade real.

No caso da análise de URLs, por exemplo, informar que a resposta será usada por um analista para decidir se uma URL pode ser liberada no firewall ajuda o modelo a priorizar segurança, rastreabilidade e objetividade.

### 3. Use exemplos de forma estratégica

No *few-shot prompting*, a qualidade dos exemplos importa tanto quanto a quantidade. A recomendação prática é usar exemplos:

- relevantes para o caso real;
- variados o suficiente para cobrir casos limítrofes;
- consistentes no formato de entrada e saída.

Quando possível, vale incluir entre 3 e 5 exemplos bons, em vez de apenas um ou dois exemplos pouco representativos.

### 4. Estruture o prompt em blocos bem definidos

Uma das recomendações mais fortes da Anthropic é separar partes diferentes do prompt com marcações claras. Em contextos simples, isso pode ser feito com títulos em Markdown. Em cenários mais técnicos ou com automação, pode ser feito com tags como:

```text
<role>
<context>
<instructions>
<examples>
<input>
<output_format>
```

Essa separação reduz ambiguidades e ajuda o modelo a distinguir instruções, contexto, exemplos e dados de entrada.

### 5. Defina o papel do modelo

Indicar um papel melhora foco, tom e critérios de decisão. Por exemplo:

```text
Você é um analista sênior de cibersegurança especializado em triagem de URLs suspeitas.
```

Isso não substitui instruções claras, mas complementa o prompt ao orientar a postura esperada do modelo.

### 6. Peça verificação antes da resposta final

Uma prática valiosa é pedir ao modelo que revise a própria resposta antes de concluí-la. Isso é próximo de *self-criticism*, mas pode ser usado mesmo fora desse padrão.

Exemplo:

```text
Antes de finalizar, verifique se sua decisão está consistente com os indicadores de risco identificados.
```

Essa simples etapa reduz erros de coerência e melhora a confiabilidade da saída.

### 7. Em tarefas com muito contexto, organize a informação com cuidado

Quando o prompt trabalha com muitos dados, documentos ou evidências, a ordem importa. Uma prática recomendada é:

- colocar os dados longos antes;
- deixar a pergunta ou instrução principal mais ao final;
- pedir que o modelo fundamente a conclusão com trechos concretos das evidências.

Isso é especialmente útil em tarefas de análise, investigação e revisão documental.

### 8. Controle explicitamente o formato da resposta

Em vez de dizer apenas o que não quer, é melhor dizer exatamente como a saída deve ser. Por exemplo:

```text
Responda no formato:
Decisão: LIBERAR ou BLOQUEAR
Risco: Baixo, Médio ou Alto
Justificativa: até 3 linhas
```

Esse tipo de instrução tende a funcionar melhor do que pedidos vagos como “seja objetivo” ou “responda bem formatado”.

## Conclusão

Esses padrões não são excludentes. Em cenários reais, é comum combinar técnicas, por exemplo:
- **Decomposition + Few-shot** para tarefas longas com formato rígido.
- **Thought Generation + Self-criticism** para melhorar precisão em problemas complexos.
- **Ensembling** como camada final de validação em respostas críticas.

Uma estratégia prática é começar com **zero-shot**, medir qualidade e evoluir para padrões mais sofisticados conforme a complexidade da tarefa. Ao mesmo tempo, vale aplicar desde o início boas práticas como **clareza**, **contexto**, **papel**, **exemplos bem escolhidos**, **estrutura explícita** e **formato de saída controlado**.
