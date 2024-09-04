# Análise de Dados de Turbinas Eólicas

Este projeto realiza uma análise de dados de turbinas eólicas, com foco na relação entre a velocidade do vento e a potência gerada, comparando os valores reais com os teóricos para determinar se estão dentro de limites aceitáveis.

## Requisitos:
As seguintes bibliotecas Python são necessárias para rodar este projeto:

pandas

seaborn

matplotlib

Você pode instalá-las usando o comando:

pip install pandas seaborn matplotlib


## Descrição do Projeto

### 1. Importação de Bibliotecas

O código começa importando as bibliotecas necessárias para a manipulação de dados e visualização:

import pandas as pd

import seaborn as sns

import matplotlib.pyplot as plt

from matplotlib.pyplot import figure

### 2. Leitura e Preparação dos Dados
   
Os dados são carregados a partir de um arquivo CSV (T1.csv) e as colunas são renomeadas para facilitar a manipulação:

turbina = pd.read_csv('T1.csv')

turbina.columns = ['Data/hora', 'Potencia', 'Velocidade vento(m/s)', 'CurvaTeórica', 'Direção Vento']

del turbina['Direção Vento']  # Removendo a coluna de direção do vento

turbina['Data/hora'] = pd.to_datetime(turbina['Data/hora'], format="%d %m %Y %H:%M")

### 3. Visualização Inicial dos Dados
   
São criados gráficos de dispersão para visualizar a relação entre a velocidade do vento e a potência, e entre a velocidade do vento e a curva teórica:

sns.scatterplot(data=turbina, x='Velocidade vento(m/s)', y='Potencia')

sns.scatterplot(data=turbina, x='Velocidade vento(m/s)', y='CurvaTeórica')

### 4. Cálculo dos Limites Aceitáveis
   
Define-se um intervalo de 5% acima e abaixo do valor teórico para determinar se a potência gerada está dentro dos limites aceitáveis. Essa informação é armazenada em uma nova coluna chamada DentroLimite:

pot_real = turbina['Potencia'].tolist()

pot_teorica = turbina['CurvaTeórica'].tolist()

pot_max = [p * 1.05 for p in pot_teorica]

pot_min = [p * 0.95 for p in pot_teorica]

dentro_limite = ['Dentro' if pot_min[i] <= pot_real[i] <= pot_max[i] else 'Fora' for i in range(len(pot_real))]

turbina['DentroLimite'] = dentro_limite

### 5. Visualização Final

Um gráfico final é gerado para visualizar quais pontos estão dentro ou fora dos limites, com cores diferentes para cada categoria:

cores = {'Dentro': 'blue', 'Fora':'red', 'Zero':'orange'}

sns.scatterplot(data=turbina, x='Velocidade vento(m/s)', y='Potencia', hue='DentroLimite', s=10, palette=cores)

## Resultados

O código calcula a porcentagem de registros que estão dentro dos limites aceitáveis e exibe um gráfico colorido que ilustra essa análise.
