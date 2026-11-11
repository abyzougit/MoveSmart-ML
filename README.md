## Autores
  HEITOR ANDERSON PRESTES DE OLIVEIRA FILHO RM 554823
  
  AUGUSTO FERREIRA ROGEL RM 557709

# MoveSmart – Análise Inteligente do Transporte Público

## Sobre o projeto

O **MoveSmart** é um projeto desenvolvido com o objetivo de analisar características operacionais do transporte público por ônibus da cidade de São Paulo, utilizando técnicas de Ciência de Dados e Aprendizado de Máquina.

A proposta é identificar padrões entre as linhas de ônibus, agrupando aquelas que apresentam características semelhantes. Os resultados podem contribuir para estudos e análises que auxiliem o planejamento e a organização do transporte público.

## Objetivo

Aplicar técnicas de Machine Learning não supervisionado para identificar grupos de linhas de ônibus com características operacionais semelhantes, permitindo compreender melhor a estrutura da rede de transporte público.

## Fonte de dados

Os dados utilizados foram obtidos por meio da SPTrans, empresa responsável pela gestão do transporte público por ônibus no município de São Paulo.

- **Fonte:** SPTrans – Área do Desenvolvedor.
- **Dataset:** GTFS (General Transit Feed Specification).
- **Link:** https://www.sptrans.com.br/desenvolvedores/
- **Localização:** São Paulo – SP, Brasil.
- **Período de referência:** 01/10/2023 a 01/06/2027, conforme calendário de serviços da versão consultada.
- **Data de consulta:** 11/11/2026.

O conjunto GTFS disponibiliza informações sobre linhas, viagens, paradas, horários, frequências e trajetos.

**Observação:** os dados representam informações programadas do transporte público, não registros de localização em tempo real ou histórico de atrasos.

## Tecnologias utilizadas

- Python
- Google Colab
- Pandas
- NumPy
- Scikit-learn
- Matplotlib

## Metodologia

O desenvolvimento foi dividido nas seguintes etapas:

### 1. Coleta de dados

Download dos arquivos GTFS disponibilizados pela SPTrans.

### 2. Pré-processamento

Leitura, organização e tratamento dos arquivos, reunindo informações operacionais das linhas de ônibus em uma única base de dados.

Foram consideradas características como:

- Quantidade de padrões de viagem;
- Média e máximo de paradas;
- Quantidade de direções;
- Quantidade de serviços e trajetos;
- Quantidade de paradas únicas;
- Intervalos médios e medianos;
- Distâncias médias e medianas dos trajetos;
- Quantidade de períodos de frequência;
- Média de dias de operação.

### 3. Redução de dimensionalidade

Foi utilizado o **PCA (Principal Component Analysis)** para reduzir a dimensionalidade dos dados, preservando aproximadamente 75,50% da variância original nas duas componentes principais.

### 4. Agrupamento com K-Means

Foi aplicado o algoritmo K-Means para identificar grupos de linhas com características operacionais semelhantes.

Foram avaliadas 12 combinações de hiperparâmetros, utilizando validação cruzada com 5 divisões exclusivamente no conjunto de treinamento.

A melhor configuração encontrada foi:

| Hiperparâmetro | Valor |
|---|---|
| Número de clusters | 4 |
| Inicialização | k-means++ |
| Número de inicializações | 20 |

### 5. Avaliação do modelo

Foram utilizadas três métricas para avaliar a qualidade dos agrupamentos:

- **Silhouette Score:** avalia a coesão e a separação dos grupos.
- **Davies-Bouldin:** avalia a dispersão e a separação entre os clusters.
- **Calinski-Harabasz:** avalia a relação entre a dispersão dos grupos e a dispersão interna.

### 6. Resultados

Resultados obtidos na avaliação final do conjunto de teste:

| Métrica | Resultado |
|---|---:|
| Silhouette Score | 0,4537 |
| Davies-Bouldin | 0,7265 |
| Calinski-Harabasz | 290,3248 |

O modelo identificou quatro grupos de linhas de ônibus, demonstrando capacidade de encontrar padrões operacionais na rede analisada.

## Contribuição para a sociedade

O MoveSmart busca transformar dados do transporte público em informações que possam auxiliar o planejamento da mobilidade urbana.

A identificação de padrões entre linhas pode contribuir para análises de distribuição, frequência e características dos trajetos, apoiando futuras decisões voltadas à melhoria da organização do transporte público e à experiência dos cidadãos.

O modelo desenvolvido não realiza previsões de atrasos ou localização em tempo real. Seu objetivo atual é identificar padrões por meio de agrupamentos.

## Como executar

1. Acesse o Google Colab: https://colab.research.google.com/
2. Abra o notebook do projeto.
3. Disponibilize o arquivo `dataset_sptrans_processado.csv` no ambiente do Colab.
4. Execute as células em ordem para carregar os dados, realizar o processamento, treinar o modelo e visualizar os resultados.
