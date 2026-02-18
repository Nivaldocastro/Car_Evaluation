# Projeto de Análise Estrutural do Car Evaluation Dataset

Este trabalho tem como objetivo realizar uma análise exploratória do Car Evaluation Dataset, um conjunto de dados composto por variáveis categóricas que descrevem características de veículos e sua respectiva classificação.

Inicialmente, foi realizado o pré-processamento das variáveis, respeitando sua natureza ordinal. Em seguida, aplicaram-se técnicas de estatística descritiva e visualizações gráficas para compreender a distribuição dos dados e suas relações com a variável alvo.

Por fim, utilizou-se a clusterização hierárquica com distância de Gower para identificar agrupamentos naturais no conjunto de dados, sem considerar previamente a classe, permitindo analisar a estrutura interna dos padrões presentes.


---

## 📁 Estrutura do Projeto
```
├── preprocessamento.py          # Pré-processamento
├── analise_estatistica.py       # análise estatistica
├── visualizaçao_dados.py        # Visualização e análise dos dados
├── clusterizaçao_gower.py       # Clusterização hierárquica 
├── Carr_dataset_ajustado.csv    # Dataset ajustado
├── imagens_Car_Evaluation       # Imagens de resultados
│   ├── boxplot_.png               # Boxplot buying
│   ├── boxplot_2.png              # Boxplot doors
│   ├── boxplot_3.png              # Boxplot safety
│   ├── matriz_correlacao.png      # Matriz de correlação
│   └── dendograma.png             # Dendograma
└── README.md
```

---

## 📂 Dataset

Fonte: UCI Machine Learning Repository  
Link: https://archive.ics.uci.edu/dataset/19/car+evaluation  
Nome: Car Evaluation Dataset  

O dataset contém avaliações de automóveis com base nos seguintes atributos categóricos:

- buying (preço de compra)
- maint (custo de manutenção)
- doors (número de portas)
- persons (capacidade de pessoas)
- lug_boot (tamanho do porta-malas)
- safety (nível de segurança)
- class (classificação final do veículo)

A variável alvo **class** possui quatro categorias:

- unacc (unacceptable)
- acc (acceptable)
- good
- vgood (very good)

O dataset é totalmente categórico, tornando necessária a utilização de métricas apropriadas para esse tipo de dado.

---

## 🛠 Bibliotecas Utilizadas

### Pandas
Utilizada para manipulação e análise de dados tabulares.

```python
import pandas as pd
```

### NumPy
Utilizada para operações numéricas e manipulação de arrays.

```python
import numpy as np
```

### Matplotlib
Utilizada para visualizações gráficas.

```python
import matplotlib.pyplot as plt
```

### SciPy
Utilizada para clusterização hierárquica e construção do dendrograma.

```python
from scipy.cluster.hierarchy import linkage, dendrogram, fcluster
```

### Gower
Utilizada para cálculo da matriz de distância para dados categóricos.

```python
import gower
```

### Scikit-learn
Utilizada para métricas de avaliação de clusterização.

```python
from sklearn.metrics import silhouette_score
```

---

## 🔎 Pré-processamento

**Arquivo: preprocessamento.py**

Para esta etapa, foi feita primeiramente a importação do dataset bruto usando a biblioteca pandas, com o objetivo de conhecer o dataset e fazer ajustes para poder realizar a análise estatistica, visualização de dados e clusterização.
Observações importantes sobre o dataset:
* Os nomes das colunas estavam ausentes, sendo preciso colocar manualmente tendo como referência as informações dentro da fonte obtida.
* Sua estrutura é 100% categórica, ou seja, cada coluna possui rótulos como por exemplo: alto, medio ou baixo; ruim, moderado, bom ou muito bom, etc
* As variáveis possuem uma natureza ordinal, ou seja, a codificação deve preservar a ordem lógica dessas categorias.

Contudo, após verificar valores nulos, dimensões do dataset e tipos das variáveis, o problema principal se resume em codificar os dados categóricos.

Para isso, é utilizado o `replace`, uma função simples que o objetivo é substituir os dados categóricos por números inteiros de forma manual que garante a ordem semântica das variáveis ordinais.

Essa estratégia foi escolhida porque:

* Preserva a ordem natural das categorias.
* Evita distorções que poderiam ocorrer com codificação arbitrária.
* Mantém coerência semântica entre os níveis.

Concluindo, os dados pré-processados foram armazenados em um arquivo `Carr_dataset_ajustado.csv`.

## 📊 Análise Estatística

**Arquivo: analise_estatistica.py**

Esta etapa do projeto teve como objetivo realizar uma análise estatística descritiva do Car Evaluation Dataset, após o pré-processamento e a codificação das variáveis categóricas em valores numéricos ordinais.
A análise buscou:
* Compreender o comportamento das variáveis após o pré-processamento
* Avaliar medidas de tendência central 

**Medidas de Tendência Central e Dispersão**
Foram calculadas média, mediana, moda, variância, desvio-padrão e amplitude para cada variável.
```
    Buying e Maint
Métrica	          Valor
Média	            2.50
Mediana           2.5
Moda             	1
Variância	        1.25
Desvio-padrão	    1.12
Amplitude	        3
```



Essas variáveis apresentam distribuição aproximadamente simétrica, com média centralizada no intervalo possível (1 a 4).

A variância de 1.25 e o desvio-padrão de 1.12 indicam boa dispersão ao longo das categorias.

Isso confirma que o dataset apresenta estrutura equilibrada nas variáveis explicativas, já que foi construído combinando sistematicamente todas as possibilidades de atributos.

```
         Doors
Métrica	          Valor
Média	            3.50
Mediana	          3.5
Moda	            2
Variância	        1.25
Desvio-padrão    	1.12
Amplitude	        3
```
A média elevada (3.5) é consequência da escala adotada (2, 3, 4, 5).

Apesar disso, a dispersão permanece uniforme, semelhante às variáveis buying e maint.

```
       Persons
Métrica	          Valor
Média	            3.67
Mediana	          4.0
Moda             	2
Variância	        1.56
Desvio-padrão	    1.25
Amplitude	        3
```
A variável persons apresentou a maior variância (1.56) e o maior desvio-padrão entre todas as variáveis explicativas.

Isso indica maior dispersão dos dados e potencialmente maior influência na diferenciação entre observações durante a clusterização.

```
    Lug_boot e Safety
Métrica         	Valor
Média	            2.00
Mediana	          2.0
Variância	        0.67
Desvio-padrão	    0.82
Amplitude       	2
```
Essas variáveis possuem apenas três níveis possíveis (1 a 3), o que naturalmente reduz sua variabilidade.

Apesar da menor dispersão, a variável safety é conhecida por exercer forte influência na classificação final dos veículos.

```
  Class (Variável Alvo)
Métrica	          Valor
Média	            1.41
Mediana	          1.0
Moda            	1
Variância	        0.55
Desvio-padrão   	0.74
Amplitude	        3
```
A variável class apresentou média próxima de 1, mediana igual a 1 e moda igual a 1, indicando forte concentração na categoria "unacc".

Isso demonstra que o dataset é estruturalmente desbalanceado, com predominância de veículos classificados como inaceitáveis.

O desvio-padrão reduzido confirma essa concentração nas classes mais baixas.

A análise estatística revelou três aspectos fundamentais:
* Equilíbrio estrutural nas variáveis explicativas.
As variáveis buying, maint, doors e persons apresentam distribuição relativamente uniforme.
* Desbalanceamento da variável alvo.
A classe "unacc" predomina significativamente no conjunto de dados.
* Influência potencial da variável safety.
Mesmo com menor variância, apresenta maior relação com a variável class.

Além disso, a variável persons apresentou maior variabilidade, podendo contribuir significativamente para a diferenciação entre grupos na etapa de clusterização.

---

## 📈 Visualização dos Dados

**Arquivo: visualizacao_dados.py**

Nesta etapa foi feito uma visualização dos dados através de uma matriz de correlação e boxplots com o objetivo de entender as correlações presentes entre as variáveis.

![Matriz de correlação ](imagens_Car_Evaluation/matriz_correlacao.png)

Ao analisar a Matriz de correlação, percebe-se visualmente que:
* A Variável alvo `class` é a única visualmente correlacionada
* As variáveis buying, maint, doors, persons, lug_boot e safety possuem uma correlação extremamente pequena, ou seja, seus valores são muito próximos de 0.
```
                buying         maint  ...        safety     class
buying    1.000000e+00 -2.072211e-15  ... -1.554300e-15 -0.282750
maint    -2.072211e-15  1.000000e+00  ... -2.588623e-16 -0.232422
doors     4.242286e-15  7.975102e-16  ...  9.909683e-17  0.066057
persons   7.983938e-16  1.883561e-16  ...  1.362772e-17  0.373459
lug_boot -1.525866e-16 -1.216188e-16  ...  7.131641e-18  0.157932
safety   -1.554300e-15 -2.588623e-16  ...  1.000000e+00  0.439337
class    -2.827504e-01 -2.324215e-01  ...  4.393373e-01  1.000000

```
De forma mais precisa, conseguimos analisar que essas variáveis realmente possuem uma correlação quase 0 entre elas. 

**Sobre a variável alvo**
Ao analisar as correlações com a variável alvo, percebe-se que a variável safety apresenta a maior correlação positiva, enquanto a variável buying apresenta a maior correlação negativa.


![Boxplot buying x class ](imagens_Car_Evaluation/boxplot_.png)

Ao analisarmos o boxplot do buying x class, percebe-se que que:
* A classe 1 possui maior variância, enquanto sua mediana encontra-se em `3`.
* Na classe 2 percebe-se uma concentração por valores mais medianos.
* Na classe 3 e 4 percebe-se que seus valores variam entre `1 a 2`.
Portando, concluímos visualmente que quanto maior o preço do carro, menor vai ser sua avaliação

![Boxplot doors x class ](imagens_Car_Evaluation/boxplot_2.png)

Ao analisarmos o boxplot do doors x class, Percebe-se que não tem muita diferença entre o número de portas para que o carro seja avaliado como aceitavel ou não.

![Boxplot safety x class ](imagens_Car_Evaluation/boxplot_3.png)

Ao analisarmos o boxplot do safety x class, Percebe-se que:
* A classe 1 possui uma concentração maior de safety entre 1 a 2
* A classe 2 e 3 possui uma variáncia mais concentrada entre 2 e 3
* A classe 4 possui seus valores de safety em 3
Portanto, conclui-se que há uma correlação positiva, ou seja, quanto maior a segurança do carro maior será a avaliação.


---

## 🤖 Clusterização Hierárquica

**Arquivo: clusterizacao_gower.py**

Para esta etapa, foi feita uma `Clusterização Hierárquica` utilizando a `Distância de Gower`, que é uma medida de dissimilaridade para conjuntos de dados que misturam tipos de variáveis (numéricas, categóricas, binárias), normalizando as diferenças entre pares de observações e calculando uma média ponderada.

Ou seja, esta distância é ideal para estes dados, que possui uma origem categórica

![Dendograma](imagens_Car_Evaluation/dendograma.png)

Sobre este Dendograma, para encontrar os clusters, baseia-se em "cortar" a árvore em uma determinada altura para separar os grupos com base na similaridade. Quanto maior a distância vertical onde a linha horizontal (corte) é feita, mais distintos e diferentes são os clusters resultantes.

A análise visual do dendrograma indicou uma divisão mais natural em 2 grandes grupos, antes da fusão em um único cluster.

Tabela absoluta
```
| Cluster | acc | good | unacc | vgood |
| ------- | --- | ---- | ----- | ----- |
| 1       | 0   | 0    | 576   | 0     |
| 2       | 384 | 69   | 634   | 65    |
```
Tabela percentual

```
| Cluster | acc    | good  | unacc  | vgood |
| ------- | ------ | ----- | ------ | ----- |
| 1       | 0%     | 0%    | 100%   | 0%    |
| 2       | 33.33% | 5.99% | 55.03% | 5.64% |

```

**Interpretação dos clusters**

Cluster 1:
* 100% composto por veículos `unacc`
* Grupo completamente homogêneo.

O algoritmo conseguiu identificar um grupo estruturalmente negativo, caracterizado por:
* Baixa segurança
* Alto custo
* Combinações desfavoráveis de atributos

Esse cluster representa o perfil mais claramente inaceitável do dataset.

Cluster 2:
* Grupo misto
* 55% ainda são "unacc"
* 33% são "acc"
* Pequena presença de "good" e "vgood"

Interpretação:
* Este cluster representa um conjunto mais heterogêneo.

Ele agrupa:
* Veículos intermediários
* Parte dos veículos inaceitáveis
* Praticamente todos os veículos de melhor qualidade

Isso indica que:
* A separação perfeita entre todas as classes não é linear.
* O dataset apresenta uma grande massa estrutural de "unacc", o que dificulta segmentação mais fina.

**Interpretação Estrutural Mais Profunda**
A clusterização com k = 2 revelou uma divisão principal:

Grupo 1 → Perfil claramente inaceitável

Grupo 2 → Perfil misto/intermediário

Isso sugere que o dataset apresenta uma estrutura predominantemente binária:
* Um grande bloco negativo bem definido
* Um segundo bloco contendo os demais padrões

---

## 🧠 Conclusão

Este projeto teve como objetivo realizar uma análise estrutural do Car Evaluation Dataset, utilizando técnicas de análise exploratória e clusterização hierárquica não supervisionada.

A etapa de pré-processamento foi fundamental para garantir a correta representação das variáveis categóricas ordinais, preservando sua ordem semântica por meio de codificação manual. Essa escolha foi essencial para manter coerência na análise estatística e na aplicação da distância de Gower.

A análise estatística revelou que:

* As variáveis explicativas apresentam estrutura equilibrada, resultado da construção sistemática do dataset.

* A variável alvo class é significativamente desbalanceada, com predominância da categoria unacc.

* A variável safety apresenta a maior correlação positiva com a classe final.

* A variável buying apresenta correlação negativa relevante com a avaliação do veículo.

* A variável persons demonstrou maior variabilidade, podendo contribuir na diferenciação estrutural dos dados.

A visualização por meio de boxplots confirmou padrões importantes:

* Quanto maior o preço de compra (buying), menor tende a ser a avaliação.

* O número de portas (doors) não apresenta influência significativa.

* A variável safety possui relação direta e clara com a qualidade da avaliação.

Na etapa de clusterização hierárquica, utilizando distância de Gower e método average linkage, o dendrograma indicou uma divisão estrutural mais natural em 2 grandes grupos.

Os resultados mostraram que:

* Um cluster é completamente composto por veículos classificados como unacc, evidenciando um padrão estrutural fortemente negativo.

* O segundo cluster é heterogêneo, contendo todas as demais categorias e parte dos veículos inaceitáveis.

Isso demonstra que o dataset possui uma estrutura predominantemente binária:

* Um grande bloco de veículos claramente inaceitáveis.

* Um bloco misto contendo veículos intermediários e superiores.

A clusterização não supervisionada foi capaz de identificar essa divisão estrutural principal, mesmo sem utilizar a variável alvo no processo de agrupamento. Esse resultado reforça que existem padrões naturais nos dados que explicam a classificação final.

De forma geral, o projeto evidencia como técnicas de análise estatística e clusterização podem revelar estruturas internas relevantes em conjuntos de dados categóricos, mesmo em cenários com desbalanceamento de classes.

