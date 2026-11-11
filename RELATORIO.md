# RELATÓRIO DE MACHINE LEARNING – MOVESMART

## 1. Problema e objetivo

A mobilidade urbana é um dos principais desafios das grandes cidades, especialmente em São Paulo, onde o transporte público por ônibus atende diariamente milhares de cidadãos. A grande quantidade de linhas e suas diferentes características operacionais tornam importante a utilização de ferramentas capazes de organizar e analisar essas informações.

O projeto MoveSmart propõe utilizar Ciência de Dados e Machine Learning para identificar padrões entre as linhas de ônibus da cidade de São Paulo. O objetivo desta etapa é agrupar linhas que apresentam características operacionais semelhantes, permitindo uma melhor compreensão da estrutura da rede de transporte público.

Para isso, foi utilizado o algoritmo K-Means, uma técnica de aprendizado não supervisionado que identifica grupos a partir das semelhanças existentes nos dados, sem a necessidade de classes previamente definidas.

## 2. Fonte e preparação dos dados

Os dados foram obtidos por meio da SPTrans, utilizando o formato GTFS (General Transit Feed Specification), disponibilizado na área de desenvolvedores.

- **Fonte:** SPTrans – Área do Desenvolvedor.
- **Link:** https://www.sptrans.com.br/desenvolvedores/
- **Dataset:** GTFS.
- **Localização:** Município de São Paulo – SP.
- **Período de referência:** 01/10/2023 a 01/06/2027, conforme o calendário de serviços da versão consultada.
- **Data de consulta:** 11/11/2026.

O GTFS disponibiliza informações sobre linhas, viagens, paradas, horários, frequências e trajetos. Esses dados foram processados utilizando Python e Pandas, resultando em uma base consolidada com características operacionais das linhas.

Entre as variáveis consideradas estão quantidade de paradas, quantidade de trajetos, número de direções, intervalos entre viagens, distâncias dos trajetos e dias de operação.

Após o tratamento dos dados, foram aplicadas técnicas de padronização e redução de dimensionalidade com PCA (Principal Component Analysis), facilitando a aplicação do algoritmo de agrupamento.

As duas componentes principais utilizadas preservaram aproximadamente **75,50% da variância dos dados originais**.

## 3. Metodologia e treinamento

O algoritmo escolhido foi o K-Means, responsável por agrupar as linhas de ônibus de acordo com suas características operacionais.

Para encontrar uma configuração adequada, foram avaliadas 12 combinações de hiperparâmetros, considerando três parâmetros:

- `n_clusters`: quantidade de grupos.
- `init`: método de inicialização dos centroides.
- `n_init`: quantidade de inicializações do algoritmo.

A seleção foi realizada utilizando validação cruzada com 5 divisões, exclusivamente nos dados de treinamento. A métrica Silhouette foi utilizada como principal critério de seleção, enquanto Davies-Bouldin e Calinski-Harabasz foram utilizadas como métricas complementares.

A melhor configuração encontrada foi:

| Hiperparâmetro | Configuração |
|---|---|
| n_clusters | 4 |
| init | k-means++ |
| n_init | 20 |

O conjunto de teste foi reservado para a avaliação final do modelo, sem participar da escolha dos hiperparâmetros.

## 4. Resultados e avaliação

A avaliação final apresentou os seguintes resultados:

| Métrica | Treinamento (CV) | Teste |
|---|---:|---:|
| Silhouette Score | 0,4423 | 0,4537 |
| Davies-Bouldin | 0,7694 | 0,7265 |
| Calinski-Harabasz | 204,8089 | 290,3248 |

Os resultados demonstram que o modelo manteve um desempenho consistente entre a validação cruzada e o conjunto de teste.

O Silhouette Score de 0,4537 indica uma separação razoável entre os grupos. O Davies-Bouldin de 0,7265 aponta uma relação favorável entre dispersão e separação dos clusters. Já o Calinski-Harabasz de 290,3248 reforça a existência de estrutura nos agrupamentos encontrados.

A distribuição dos grupos no conjunto de teste foi:

| Grupo | Quantidade de linhas |
|---|---:|
| 0 | 72 |
| 1 | 61 |
| 2 | 32 |
| 3 | 97 |

O Grupo 3 apresentou a maior quantidade de linhas, enquanto o Grupo 2 apresentou a menor.

### Análise de overfitting

Não foram identificados sinais evidentes de overfitting. As métricas do conjunto de teste não apresentaram queda em relação aos resultados médios da validação cruzada.

O Silhouette aumentou de 0,4423 para 0,4537, enquanto o Davies-Bouldin diminuiu de 0,7694 para 0,7265. Isso sugere que o modelo conseguiu manter sua capacidade de agrupamento em dados que não participaram do treinamento.

Entretanto, essas diferenças não comprovam ausência absoluta de overfitting, sendo importante considerar também a natureza não supervisionada do algoritmo e a avaliação das características dos grupos.

## 5. Conclusão e impacto para o cidadão

O desenvolvimento desta etapa do MoveSmart demonstrou que técnicas de Machine Learning podem ser utilizadas para identificar padrões operacionais no transporte público de São Paulo.

A aplicação do K-Means permitiu identificar quatro grupos de linhas de ônibus com características semelhantes, apresentando resultados consistentes na validação cruzada e no conjunto de teste.

Para o cidadão, esse tipo de análise pode contribuir indiretamente para a melhoria da mobilidade urbana. A identificação de padrões entre as linhas pode auxiliar estudos sobre distribuição, frequência e características dos trajetos, oferecendo informações úteis para o planejamento e a organização do transporte público.

Como limitação, os dados GTFS representam informações programadas, não o comportamento real dos ônibus em circulação. Portanto, o modelo atual não prevê atrasos nem fornece localização em tempo real.

Conclui-se que o MoveSmart apresenta uma abordagem inicial promissora para transformar dados públicos em informações que apoiem o planejamento do transporte coletivo, podendo ser ampliado futuramente com dados de operação em tempo real e histórico de deslocamentos.
