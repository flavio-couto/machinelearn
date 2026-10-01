# Detecção de Fraudes em Transações de Cartão de Crédito

Projeto de Machine Learning desenvolvido a partir do desafio da DIO para estudar detecção de fraude em transações de cartão de crédito.

## Problema

A base possui uma quantidade muito maior de transações normais do que fraudulentas. Nesse cenário, a acurácia pode passar uma impressão enganosa de desempenho.

Por exemplo, um modelo que classificasse quase todas as transações como normais poderia acertar a maioria dos registros simplesmente por causa do desbalanceamento, mas deixaria de identificar as fraudes.

Por isso, este projeto prioriza:

- **Precisão:** proporção das transações classificadas como fraude que realmente são fraude;
- **Recall:** proporção das fraudes reais que foram identificadas;
- **F1-score:** equilíbrio entre precisão e recall;
- **ROC AUC e PR AUC:** métricas adicionais para avaliar a capacidade de separação dos modelos.

## Dataset

A base utilizada é o `creditcard.csv`, disponibilizado publicamente pelo TensorFlow:

https://storage.googleapis.com/download.tensorflow.org/data/creditcard.csv

O arquivo não é armazenado neste repositório. O notebook faz o carregamento diretamente pelo endereço acima.

As variáveis `V1` a `V28` são componentes transformados por PCA para preservar informações sensíveis das transações. A base também possui `Time`, `Amount` e `Class`, sendo `Class` o alvo:

- `0` = transação normal
- `1` = fraude

## Preparação dos dados

Foram realizadas as seguintes etapas:

1. Carregamento da base com Pandas;
2. Exploração das dimensões, tipos e distribuição das classes;
3. Criação da variável `LogAmount` com `log1p`;
4. Separação entre variáveis explicativas e variável alvo;
5. Divisão entre treino e teste utilizando `stratify`;
6. Padronização das variáveis nos modelos que utilizam `StandardScaler`;
7. Tratamento do desbalanceamento por meio de pesos de classe.

## Modelos

Foram comparados:

- Regressão Logística;
- Random Forest;
- XGBoost.

A Regressão Logística foi utilizada como baseline, enquanto Random Forest e XGBoost permitem comparar modelos baseados em árvores.

## Avaliação

O notebook apresenta:

- Classification Report;
- Matriz de confusão;
- Curva ROC;
- Curva Precision-Recall;
- ROC AUC;
- PR AUC;
- Precisão, recall e F1 da classe fraude.

Os resultados finais são gerados durante a execução do notebook, pois dependem do treinamento dos modelos.

## Ajuste do threshold

Além da utilização do threshold padrão de 0,50, o projeto testa diferentes limiares para o XGBoost.

O threshold escolhido no notebook é aquele que apresenta o maior F1 entre os valores testados.

Essa escolha é uma decisão de demonstração. Em um sistema real, o threshold deveria considerar também o custo de:

- deixar uma fraude passar;
- bloquear ou sinalizar uma transação legítima.

## SHAP

O SHAP é utilizado para explicar as decisões do XGBoost.

A análise permite observar quais variáveis contribuíram positivamente ou negativamente para uma previsão específica e quais variáveis possuem maior impacto geral nas previsões.

Como `V1` a `V28` são componentes transformados por PCA, suas contribuições não devem ser interpretadas como características de negócio diretamente observáveis.
