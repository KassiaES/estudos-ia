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

## Conclusão

Esses padrões não são excludentes. Em cenários reais, é comum combinar técnicas, por exemplo:
- **Decomposition + Few-shot** para tarefas longas com formato rígido.
- **Thought Generation + Self-criticism** para melhorar precisão em problemas complexos.
- **Ensembling** como camada final de validação em respostas críticas.

Uma estratégia prática é começar com **zero-shot**, medir qualidade e evoluir para padrões mais sofisticados conforme a complexidade da tarefa.
