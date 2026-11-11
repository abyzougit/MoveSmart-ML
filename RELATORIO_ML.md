# RELATÓRIO DE MACHINE LEARNING – MOVESMART
### Análise e agrupamento de linhas de ônibus da cidade de São Paulo

## 1. Introdução e definição do problema

A mobilidade urbana representa um dos principais desafios das grandes cidades brasileiras. Em São Paulo, o transporte público por ônibus possui uma estrutura extensa, composta por diversas linhas, trajetos, horários, frequências e pontos de parada. Essa diversidade torna complexa a análise da rede e dificulta a identificação de padrões operacionais que poderiam auxiliar no planejamento do sistema.

Diante desse cenário, o projeto **MoveSmart** propõe a utilização de Ciência de Dados e Aprendizado de Máquina para transformar informações públicas do transporte coletivo em conhecimento que possa apoiar a tomada de decisões.

O problema abordado nesta etapa consiste na dificuldade de analisar e comparar grandes quantidades de linhas de ônibus considerando simultaneamente suas características operacionais. Uma análise individual pode ser trabalhosa e dificultar a identificação de linhas que apresentam comportamentos semelhantes.

Como solução, foi desenvolvido um modelo de aprendizado não supervisionado utilizando o algoritmo K-Means, com o objetivo de agrupar linhas de ônibus de acordo com suas características. Dessa maneira, pretende-se identificar padrões na organização da rede de transporte público, permitindo análises mais estruturadas.

## 2. Fonte de dados e preparação da base

Para o desenvolvimento do modelo, foram utilizados dados disponibilizados pela SPTrans (São Paulo Transporte), por meio do formato GTFS (General Transit Feed Specification).

O GTFS é um padrão utilizado para representar informações estruturadas de sistemas de transporte público, incluindo linhas, viagens, paradas, horários, frequências e trajetos.

**Identificação da fonte:**

- Fonte: SPTrans – Área do Desenvolvedor.
- Link: https://www.sptrans.com.br/desenvolvedores/
- Dataset: GTFS.
- Localização: Município de São Paulo – SP, Brasil.
- Período de referência: 01/10/2023 a 01/06/2027, conforme o calendário de serviços da versão consultada.
- Data de consulta: 11/11/2026.

Inicialmente, os arquivos disponibilizados foram carregados e processados utilizando Python e a biblioteca Pandas. As informações foram organizadas em uma base consolidada, na qual cada registro representa uma linha de ônibus e suas respectivas características operacionais.

Durante o processamento, foram consideradas variáveis como quantidade de padrões de viagem, média e máximo de paradas, quantidade de direções, serviços e trajetos, quantidade de paradas únicas, intervalos médios e medianos entre viagens, distâncias dos trajetos e quantidade de dias de operação.

Também foi realizado o tratamento dos dados para remover inconsistências, lidar com valores ausentes e preparar as variáveis para utilização no algoritmo.

É importante destacar que o GTFS representa informações programadas do transporte público. Portanto, a base não contém necessariamente o comportamento real dos ônibus em circulação, como atrasos, congestionamentos ou posições geográficas em tempo real.

## 3. Metodologia de Machine Learning

A abordagem escolhida foi o aprendizado não supervisionado, pois não existia uma classificação previamente definida para indicar a qual grupo cada linha deveria pertencer.

O algoritmo utilizado foi o **K-Means**, que busca organizar os registros em grupos com características semelhantes, minimizando a distância entre os elementos e os centroides dos respectivos agrupamentos.

Antes da aplicação do algoritmo, foi realizada a padronização das variáveis numéricas. Esse procedimento é importante porque as características utilizadas possuem escalas diferentes. Sem a padronização, variáveis com valores numericamente maiores poderiam exercer influência desproporcional no processo de agrupamento.

Também foi utilizado o PCA (Principal Component Analysis), técnica de redução de dimensionalidade que transforma as variáveis originais em componentes principais, preservando a maior quantidade possível de variabilidade dos dados.

As duas componentes principais utilizadas no modelo preservaram aproximadamente **75,50% da variância total**, sendo 43,91% explicada pela primeira componente e 31,59% pela segunda.

### 3.1 Otimização dos hiperparâmetros

Para encontrar uma configuração adequada do K-Means, foram avaliadas 12 combinações de hiperparâmetros, considerando três parâmetros:

- **n_clusters:** define a quantidade de grupos que o algoritmo deverá identificar.
- **init:** determina a estratégia utilizada para inicializar os centroides.
- **n_init:** define quantas inicializações diferentes serão executadas para encontrar uma solução mais adequada.

A avaliação foi realizada utilizando validação cruzada com cinco divisões (5-Fold Cross Validation), mantendo todas as etapas de treinamento e seleção dentro do conjunto de treinamento. O conjunto de teste foi reservado para a avaliação final.

A métrica Silhouette foi utilizada como principal critério de seleção, enquanto Davies-Bouldin e Calinski-Harabasz foram utilizadas como indicadores complementares.

A melhor configuração encontrada foi:

| Hiperparâmetro | Melhor configuração |
|---|---|
| n_clusters | 4 |
| init | k-means++ |
| n_init | 20 |

Essa combinação apresentou o melhor resultado médio de Silhouette entre as configurações avaliadas.

## 4. Resultados e avaliação do modelo

Após a seleção dos hiperparâmetros, o modelo foi ajustado utilizando o conjunto completo de treinamento e posteriormente avaliado no conjunto de teste, que não participou do processo de seleção.

Os resultados obtidos foram:

| Métrica | Treinamento (CV) | Teste |
|---|---:|---:|
| Silhouette Score | 0,4423 | 0,4537 |
| Davies-Bouldin | 0,7694 | 0,7265 |
| Calinski-Harabasz | 204,8089 | 290,3248 |

O **Silhouette Score** avalia a proximidade dos elementos em relação ao próprio grupo e sua distância em relação aos demais. Quanto mais próximo de 1, melhor a separação dos agrupamentos. O resultado de 0,4537 indica uma separação razoável entre os grupos identificados.

O índice **Davies-Bouldin** avalia a dispersão interna dos clusters em relação à distância entre eles. Valores menores indicam agrupamentos mais compactos e separados. O resultado de 0,7265 demonstra uma estrutura de agrupamento favorável dentro do espaço analisado.

Já o índice **Calinski-Harabasz** considera a relação entre a dispersão dos elementos dentro dos grupos e a dispersão entre os grupos. Valores maiores indicam uma estrutura de agrupamento mais definida, embora a métrica também dependa do tamanho da amostra.

A distribuição dos registros no conjunto de teste foi:

| Cluster | Quantidade de linhas |
|---|---:|
| Grupo 0 | 72 |
| Grupo 1 | 61 |
| Grupo 2 | 32 |
| Grupo 3 | 97 |

Essa distribuição demonstra que o algoritmo identificou quatro grupos com tamanhos diferentes. O Grupo 3 concentrou a maior quantidade de linhas, enquanto o Grupo 2 apresentou a menor quantidade.

É importante ressaltar que os números dos clusters são identificadores atribuídos pelo algoritmo. Portanto, o Grupo 0 não representa necessariamente linhas melhores ou piores que as pertencentes aos demais grupos.

## 5. Análise de overfitting e generalização

O overfitting ocorre quando um modelo se ajusta excessivamente aos dados utilizados no treinamento e apresenta dificuldade para manter seu desempenho diante de novos dados.

Neste projeto, a comparação entre os resultados médios da validação cruzada e a avaliação final do teste não apresentou sinais evidentes de perda de desempenho.

O Silhouette Score passou de 0,4423 no treinamento para 0,4537 no teste. O Davies-Bouldin apresentou uma redução de 0,7694 para 0,7265, enquanto o Calinski-Harabasz aumentou de 204,8089 para 290,3248.

Essas diferenças indicam que o modelo manteve uma estrutura de agrupamento consistente nos dados que não participaram do treinamento.

Entretanto, resultados melhores no teste não significam necessariamente que o modelo seja superior nesse conjunto, pois as métricas também podem variar conforme a composição e o tamanho das amostras. Além disso, como se trata de aprendizado não supervisionado, a avaliação deve considerar a interpretação dos grupos e não apenas os indicadores numéricos.

Dessa forma, os resultados sugerem uma boa capacidade de generalização, sem evidências claras de overfitting.

## 6. Impacto para o cidadão e aplicações práticas

A utilização de Machine Learning na análise do transporte público pode contribuir para uma compreensão mais detalhada da estrutura da rede de ônibus.

A identificação de grupos com características semelhantes permite organizar as linhas em categorias operacionais, facilitando comparações e análises que seriam mais trabalhosas quando realizadas individualmente.

Para o cidadão, os benefícios podem ocorrer de maneira indireta, por meio de decisões de planejamento mais bem fundamentadas. Informações sobre características dos trajetos, quantidade de paradas e frequências programadas podem auxiliar estudos voltados à distribuição e organização das linhas.

Por exemplo, os agrupamentos poderão ser utilizados em análises futuras para identificar conjuntos de linhas com características semelhantes e investigar possíveis oportunidades de melhoria na organização do serviço.

É importante destacar que o modelo desenvolvido ainda não prevê atrasos, não identifica congestionamentos e não acompanha a localização dos veículos em tempo real. Seu objetivo atual é identificar padrões operacionais a partir dos dados programados disponibilizados pela SPTrans.

## 7. Conclusão

O desenvolvimento desta etapa do MoveSmart demonstrou a viabilidade da utilização de técnicas de aprendizado não supervisionado para analisar características operacionais do transporte público de São Paulo.

A aplicação do K-Means, após o processamento e a redução de dimensionalidade com PCA, permitiu identificar quatro grupos de linhas de ônibus com características semelhantes. A configuração selecionada apresentou resultados consistentes na validação cruzada e no conjunto de teste, com Silhouette Score de 0,4537 na avaliação final.

A comparação das métricas não demonstrou sinais evidentes de overfitting, indicando que o modelo conseguiu manter sua capacidade de agrupamento em dados não utilizados durante o treinamento.

Conclui-se que a solução representa uma etapa inicial para transformar dados públicos de transporte em informações úteis para a análise da mobilidade urbana. Como evolução, o projeto poderá incorporar dados de localização em tempo real e históricos de operação, permitindo desenvolver análises mais avançadas sobre frequência efetiva, atrasos e condições de deslocamento.

Assim, o MoveSmart busca contribuir para um planejamento do transporte público mais orientado por dados, com potencial de apoiar melhorias que beneficiem tanto a gestão do sistema quanto a população que depende diariamente desse serviço.
