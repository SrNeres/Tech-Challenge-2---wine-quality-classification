# Tech-Challenge-2---wine-quality-classification

Projeto desenvolvido para o **Tech Challenge — Fase 2**, com o objetivo de aplicar técnicas de análise exploratória de dados e Machine Learning para classificar vinhos com base em suas características físico-químicas.

## 1. Objetivo do projeto

O objetivo deste projeto é desenvolver modelos de classificação capazes de prever se um vinho pertence à classe de **alta qualidade** ou à classe de **baixa/média qualidade**, utilizando variáveis físico-químicas disponíveis no dataset.

A variável original `quality` foi transformada em uma variável binária:

* `0`: vinho de baixa ou média qualidade, com nota inferior a 7;
* `1`: vinho de alta qualidade, com nota igual ou superior a 7.

## 2. Sobre o dataset

Foi utilizado o **Wine Quality Dataset**, disponível publicamente no Kaggle.

O conjunto de dados contém informações físico-químicas de amostras de vinho, incluindo:

* acidez fixa (`fixed acidity`);
* acidez volátil (`volatile acidity`);
* ácido cítrico (`citric acid`);
* açúcar residual (`residual sugar`);
* cloretos (`chlorides`);
* dióxido de enxofre livre (`free sulfur dioxide`);
* dióxido de enxofre total (`total sulfur dioxide`);
* densidade (`density`);
* pH (`pH`);
* sulfatos (`sulphates`);
* teor alcoólico (`alcohol`);
* qualidade do vinho (`quality`).

## 3. Estrutura do repositório

```text
wine-quality-classification/
│
├── data/
│   └── wine_quality.csv
│
├── notebooks/
│   └── wine_quality_classification.ipynb
│
├── results/
│   ├── matriz_confusao_arvore.png
│   ├── matriz_confusao_svm.png
│   ├── importancia_variaveis_arvore.png
│   └── tabela_resultados_modelos.csv
│
├── src/
│   └── utils.py
│
├── presentation/
│   └── apresentacao_executiva.pdf
│
├── README.md
├── requirements.txt
└── .gitignore
```

## 4. Etapas desenvolvidas

O projeto foi desenvolvido seguindo as principais etapas de uma pipeline de Machine Learning:

1. Compreensão do problema;
2. Carregamento e análise inicial dos dados;
3. Criação da variável-alvo binária;
4. Análise exploratória dos dados;
5. Verificação de valores nulos;
6. Análise da distribuição das variáveis;
7. Análise de outliers;
8. Análise de correlação;
9. Separação entre treino e teste;
10. Balanceamento da base de treinamento;
11. Desenvolvimento dos modelos;
12. Avaliação e comparação dos modelos;
13. Interpretação dos resultados;
14. Conclusão e limitações.

## 5. Análise exploratória dos dados

Durante a análise exploratória, foi identificado que a variável-alvo apresentava desbalanceamento entre as classes.

Distribuição da variável-alvo:

| Classe | Descrição             | Percentual |
| ------ | --------------------- | ---------: |
| 0      | Baixa/Média Qualidade |     86,09% |
| 1      | Alta Qualidade        |     13,91% |

Esse desbalanceamento foi considerado durante a etapa de modelagem, evitando que a avaliação dos modelos fosse baseada apenas na acurácia.

Também foram analisadas distribuições, possíveis outliers e correlações entre as variáveis. A análise de correlação indicou que o teor alcoólico (`alcohol`) apresentou a maior associação positiva com a classificação de alta qualidade, enquanto a acidez volátil (`volatile acidity`) apresentou a maior associação negativa.

## 6. Pré-processamento

A base foi dividida em conjuntos de treino e teste, mantendo a proporção original das classes por meio de divisão estratificada.

O balanceamento foi aplicado apenas no conjunto de treinamento, utilizando oversampling aleatório da classe minoritária por meio da função `resample`.

O conjunto de teste foi mantido com a distribuição original dos dados, evitando vazamento de dados e permitindo uma avaliação mais realista dos modelos.

Para o modelo SVM, foi aplicada padronização das variáveis com `StandardScaler`, pois esse algoritmo é sensível às diferenças de escala entre as variáveis.

## 7. Modelos utilizados

Foram desenvolvidos e comparados dois modelos de classificação:

### Árvore de Decisão

A Árvore de Decisão foi utilizada por sua capacidade de criar regras de classificação e capturar relações não lineares entre as variáveis.

Foi utilizada uma versão regularizada do modelo para reduzir o risco de overfitting.

### Support Vector Machine — SVM

O SVM foi utilizado como segundo modelo de classificação, com kernel RBF, buscando uma fronteira não linear para separação entre as classes.

Como o SVM é sensível à escala das variáveis, os dados foram padronizados antes do treinamento.

## 8. Resultados dos modelos

Os modelos foram avaliados utilizando o conjunto de teste original.

| Modelo            | Acurácia | Acurácia Balanceada | Precisão Classe 1 | Recall Classe 1 | F1-Score Classe 1 |
| ----------------- | -------: | ------------------: | ----------------: | --------------: | ----------------: |
| Árvore de Decisão |    0,799 |               0,805 |             0,394 |           0,813 |             0,531 |
| SVM               |    0,817 |               0,763 |             0,407 |           0,688 |             0,512 |

O SVM apresentou maior acurácia geral, com aproximadamente 81,7%. Entretanto, a Árvore de Decisão apresentou melhor desempenho na identificação dos vinhos de alta qualidade, com recall de 81,3% para a classe 1.

Considerando o desbalanceamento da base e a importância de identificar corretamente a classe minoritária, a Árvore de Decisão regularizada foi selecionada como o modelo principal.

## 9. Interpretação dos resultados

A análise de importância das variáveis da Árvore de Decisão indicou que o teor alcoólico (`alcohol`) foi a variável mais relevante para a classificação dos vinhos, concentrando aproximadamente 41,19% da importância atribuída pelo modelo.

As principais variáveis identificadas foram:

| Variável             | Importância |
| -------------------- | ----------: |
| alcohol              |      0,4119 |
| sulphates            |      0,1447 |
| fixed acidity        |      0,1337 |
| total sulfur dioxide |      0,0953 |
| pH                   |      0,0644 |
| chlorides            |      0,0614 |

As quatro principais variáveis concentraram aproximadamente 78,56% da importância total do modelo.

Apesar da relevância do teor alcoólico, esse resultado não deve ser interpretado como indicação de que vinhos com maior teor alcoólico são necessariamente melhores. A qualidade do vinho depende do equilíbrio entre diferentes características físico-químicas, como álcool, acidez, sulfatos, taninos, compostos aromáticos e estilo pretendido.

## 10. Conclusão

O projeto demonstrou que características físico-químicas dos vinhos podem ser utilizadas para apoiar a classificação entre vinhos de baixa/média qualidade e vinhos de alta qualidade.

Entre os modelos avaliados, a Árvore de Decisão regularizada apresentou melhor desempenho na identificação da classe de alta qualidade, com maior recall, maior acurácia balanceada e maior F1-score para a classe minoritária.

Entretanto, os resultados também indicam limitações. A precisão da classe de alta qualidade ainda foi baixa, o que significa que parte dos vinhos previstos como de alta qualidade pertencia, na realidade, à classe de baixa/média qualidade.

Dessa forma, o modelo deve ser interpretado como uma ferramenta de apoio à decisão, e não como substituto da avaliação sensorial e do conhecimento técnico de especialistas.

## 11. Limitações e melhorias futuras

Entre as principais limitações do projeto estão:

* desbalanceamento da variável-alvo;
* baixa quantidade de vinhos de alta qualidade no conjunto de teste;
* uso de oversampling aleatório, que duplica registros da classe minoritária;
* ausência de variáveis sensoriais ou informações adicionais sobre o processo produtivo;
* possibilidade de ajuste mais refinado dos hiperparâmetros.

Como melhorias futuras, poderiam ser testadas:

* validação cruzada;
* GridSearchCV para otimização de hiperparâmetros;
* técnicas alternativas de balanceamento, como SMOTE;
* outros algoritmos de classificação;
* comparação entre modelos com e sem tratamento de outliers;
* inclusão de novas variáveis relacionadas à produção e avaliação sensorial.

## 12. Como executar o projeto

Clone o repositório:

```bash
git clone https://github.com/seu-usuario/wine-quality-classification.git
```

Acesse a pasta do projeto:

```bash
cd wine-quality-classification
```

Instale as dependências:

```bash
pip install -r requirements.txt
```

Abra o Jupyter Notebook:

```bash
jupyter notebook
```

Execute o notebook localizado em:

```text
notebooks/wine_quality_classification.ipynb
```

## 13. Tecnologias utilizadas

* Python
* Pandas
* Matplotlib
* Seaborn
* Scikit-learn
* Jupyter Notebook

## 14. Referências

* Wine Quality Dataset — Kaggle
* Cortez et al. — Modeling wine preferences by data mining from physicochemical properties
* Evino — Teor alcoólico no vinho: como ele define corpo, equilíbrio e sabor
* Famiglia Valduga — Como a graduação alcoólica interfere no sabor de um vinho?
