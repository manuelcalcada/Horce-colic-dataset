# Horce colic dataset

![image](https://user-images.githubusercontent.com/42875146/122683224-3953d780-d1d4-11eb-9a3b-e639f13532f9.png)

- This repo presents a project presented as a final project for Data Mining discipline from a Data Science post gratuated degree at [PUC-Rio](https://www.puc-rio.br/english/).

- Este repositório apresenta o projeto final da disciplina de Data Mining da pós graduação de ciência de dados da [PUC-Rio](https://www.puc-rio.br/).

## Objetivo

Construir um modelo de machine learning que busque prever se um cavalo pode sobreviver ou não com base em condições médicas anteriores.

## Dados

Foram utilizados dados abertos do seguinte link: http://archive.ics.uci.edu/ml/datasets/Horse+Colic

## Principais resultados da análise exploratória de dados

![image](https://user-images.githubusercontent.com/42875146/122683338-eaf30880-d1d4-11eb-82d4-d6be396f2958.png)

Os gráficos anteriores nos permitem observar o comportamento dos dois grupos em relação às variáveis numéricas. É possível observar que, relativamente, a temperatura retal dos cavalos que morreram está melhor distribuída entre os limites inferiores e superiores, enquanto que, os cavalos que viveram, geralmente possuíam temperaturas medianas (por volta de 38 C). 

Já para a pulsação, é notório que os cavalos que viveram possuiam valores de pulso mais baixos do que cavalos que morreram. No entanto, há a presença de cavalos que viveram mesmo com pulsação elevada. 

Para packed_cell_volume, é possível verificar que cavalos que evoluíram a óbito geralmente possuem valores maiores para esta variável em comparação com cavalos que viveram, ao contrário do que acontece na taxa de respiração, onde cavalos que viveram geralmente possuem taxas menores do que os que morreram.

![image](https://user-images.githubusercontent.com/42875146/122683343-efb7bc80-d1d4-11eb-95ef-16b144dac2cd.png)

Analisando agora as diferenças entre os dois grupos (cavalos que sobreviveram e que morreram) nas variáveis categórias, é possível verificar que:

1. Mais cavalos morrem quando passam por procedimentos cirúrgicos
1. Mais cavalos morrem quando a temperatura de suas extremidades está baixa
1. Mais cavalos morrem quando peripheral_pulse está reduzida ou inexistente
1. Dores extremas e severas estão mais correlacionadas com a evolução à óbito pelos cavalos
1. A presença de lesões cirúrgicas está relacionada à uma alta diferença entre cavalos que morreram e que sobreviveram

Essas análises podem ser feitas para todos as variáveis categóricas, pois todas apresentaram diferença significativa entre os grupos analisados. Para não ser exaustivo neste trabalho, apenas elenquei 5 insights retirados desta análise. 

## Modelo de classifificação para predição do estado de cavalos

Esta etapa compreende a modelagem estatística do problema, visando a criação de um modelo para predizer se um cavalo irá viver ou morrer dado suas condições de saúde. 

A metodologia de treinamento segue os seguintes passos:

1. Treinamento de 11 tipos diversos de modelos de machine learning utilizando os hiperparâmetros padrões na base pré-processada (Cenário 1)
1. Treinamento de 11 tipos diversos de modelos de machine learning utilizando os hiperparâmetros padrões na base pré-processada com remoção de alguns atributos (Cenário 2)
1. Treinamento de 11 tipos diversos de modelos de machine learning utilizando os hiperparâmetros padrões na base pré-processada com redução de dimensionalidade por PCA (Cenário 3)
1. Treinamento de 11 tipos diversos de modelos de machine learning utilizando os hiperparâmetros padrões na base pré-processada com balanceamento via SMOTE (Cenário 4)
1. Escolha dos 3 melhores modelos no melhor cenário anterior
1. Otimização dos 3 melhores modelos utilizando grid search e validação cruzada
1. Escolha do campeão

Os modelos serão avaliados com base em suas métricas de treino e teste, mais precisamente: (i) acurácia, (ii) recall, (iii) precision e (iv) f1-score, e também pela estabilidade dos resultados utilizando estas duas bases.

Os 11 modelos a serem avaliados são:
1. Logistic Regression
1. Extra Trees
1. Gradient Boosting
1. Suport Vector Machines
1. K-Nearest Neighbours
1. SGD
1. Decision Tree
1. Random Forest
1. XGBoost
1. MLP Neural Network

### Melhor cenário

De forma geral, o melhor desempenho utilizando as métricas analisadas foi obtido no cenário 3 com a redução de dimensionalidade utilizando PCA. Entretanto, o resultado do cenário 1, com a base original, não ficou muito atrás. Sendo assim, por mais que o PCA consiga aumentar um pouco a performance dos nossos modelos, em geral a performance sem aplicação também é satisfatória. Aplicar o PCA significaria abrir mão de explicabilidade, pelo menos de forma direta.

### Otimização de hiperparâmetros

Sendo assim, vou seguir a otimização com o cenário 1 utilizando os três melhores algoritmos nele, a saber:

1. Extra Trees
1. Random Forest
1. MLP Neural Network

Após essa otimização, o melhor modelo encontrado foi o baseado em Extra Trees.

### Matriz de confusão do melhor modelo

![image](https://user-images.githubusercontent.com/42875146/122683467-c21f4300-d1d5-11eb-9b53-b6391cc2b8b8.png)

### Explicabilidade do modelo

![image](https://user-images.githubusercontent.com/42875146/122683485-d5321300-d1d5-11eb-96e8-b1670b94310b.png)

Como pode ser visto, para a predição do modelo estatístico, a variável mais importante é a **packed_cell_volume**, seguida de **total_protein** e **pulse**.

Estas variáveis exibidas no gráfico anterior são as 15 mais importantes para que o modelo chege a conclusão se o cavalo irá sobreviver ou morrer.

É importante ressaltar que, apesar de ser possível ver a importância atribuida a cada variável pelo modelo, o modelo estatístico usa todas elas para a predição, o que significa que, ao remover alguma variável destas, a performance pode mudar, pois o mesmo identifica padrões ocultos entre as variáveis para fornecer maior robustez a decisão. 

## Conclusões

Foi aplicado todo ciclo de ciência de dados na base fornecida sobre registro médico de cavalos. Começamos na exploração dos dados, onde foram realizadas análises uni e bivariadas, assim como gerados alguns insights. Na sequência, estudamos como tratar os dados ausentes e outliers, assim como efetuamos outros processamentos como balanceamento, normalização e encoding. 
Na etapa de modelagem estatísticas, foram testados onze modelos em 4 cenários bases (original, reduzido, PCA e balanceado por SMOTE). O melhor resultado encontrado foi no cenário utilizando PCA, mas optamos por seguir com o cenário original pela proximidade dos resultados e possibilidade de interpetação posterior do modelo.
Assim, após o processo de otimização de hiperparâmetros e validação cruzada, chegamos ao modelo campeão, Extra Trees, com uma performance de quase 77% de acurácia na validação cruzada e 100% de acurácia nas partições de treino e de teste.
Para finalizar, exploramos as variáveis preditoras mais importantes para a predição do modelo, onde chegamos num gráfico com as 15 mais importantes para isto.

Esta análise de explicabilidade é importante para analisarmos o peso atribuído a cada atributo, o que pode ajudar em discussões com a área de negócio quando necessário, além de tornar palpável nossos resultados para este público. Também é possível selecionar algumas variáveis e treinar o modelo novamente somente com as mais importantes, o que ajudaria a reduzir a complexibilidade do modelo (levando a tempos menores de aplicação e de geração de bases de propensão). 

Para este caso, não foi necessário seguir nenhuma dessas alternativas pois os modelos conseguiram se ajustar bem aos dados fornecidos. 

  --- 
  Visit my [LinkedIn](https://www.linkedin.com/in/manuelcalcada/ "Stay in touch!")!
