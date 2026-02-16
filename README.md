# Projeto de Análise Estrutural do Car Evaluation Dataset

Este trabalho apresenta uma análise exploratória e estrutural do Car Evaluation Dataset, com foco na aplicação de técnicas de clusterização não supervisionada. O objetivo principal foi identificar agrupamentos naturais nos dados a partir de atributos categóricos, utilizando a distância de Gower para tratar adequadamente esse tipo de variável. Foram realizados pré-processamento, análise estatística, visualização dos dados e clusterização hierárquica. Os resultados demonstram que existem padrões significativos nos atributos avaliados, possibilitando a identificação de agrupamentos coerentes com as classes originais, em especial para a categoria “unacc”.


---

## 📁 Estrutura do Projeto
```
├── preprocessamento.py          # Pré-processamento
├── analise_estatistica.py       # análise estatistica
├── visualizaçao_dados.py        # Visualização e análise dos dados
├── clusterizaçao_gower.py       # Clusterização hierárquica 
├── Carr_dataset_ajustado.csv    # Dataset ajustado
├── imagem                       # Imagens de resultados
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

O dataset é totalmente categórico, tornando necessária a utilização de uma métrica apropriada para esse tipo de dado.

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

Para esta etapa, foi feito primeiramente a importação do dataset bruto usando a biblioteca pandas, com o objetivo de conhecer o dataset e fazer ajustes para poder fazer a análise estatistica, visualização de dados e clusterização.
Observações importantes sobre o dataset:
* Os nomes das colunas estavam ausentes, sendo preciso colocar manualmente tendo como referencia informações dentro da fonte obtida.
* Sua estrutura é 100% categórica, ou seja, cata coluna possui rotulos como por exemplo: alto, medio ou baixo; ruim, moderado, bom ou muito bom, etc
* Variaveis possuem uma natureza ordinal, ou seja, a codificação deve preservar a ordem lógica dessas categorias.

Contudo, após verificar valores nulos, dimenções do dataset, tipos das variaveis, o problema principal se resume em codificar os dados categóricos.

Para isso, é utilizado o `replace`, uma função simples que o objetivo é substituir as categóricas por numeros inteiros de forma manual que garante a ordem semântica das variáveis ordinais.

Essa estratégia foi escolhida porque:

* Preserva a ordem natural das categorias.
* Evita distorções que poderiam ocorrer com codificação arbitrária.
* Mantém coerência semântica entre os níveis.

Concluindo, os dados pré-processados foram armazenados em um arquivo `Carr_dataset_ajustado.csv `.

## 📊 Análise Estatística

**Arquivo: analise_estatistica.py**




