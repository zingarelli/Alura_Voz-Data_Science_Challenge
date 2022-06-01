# Alura Challenges - Data Science - 2022

Este projeto é resultado da minha participação no [Challenge](https://www.alura.com.br/challenges/data-science) promovido pela [Alura](https://www.alura.com.br), uma plataforma online brasileira de cursos de tecnologia. 

O Alura Challenges é uma proposta de desafio para colocar em prática aquilo que se aprende nos cursos. Nele, é possível simular uma experiência de trabalho e entrega de um projeto para uma empresa fictícia. São 4 semanas de desafio, sendo que em cada uma é disponibilizado um quadro no Trello com as solicitações da empresa. A cada semana, o desafio aumenta. 

---

## Análise de Churn Rate

**Solicitante:** Alura Voz, operadora de telecomunicações.

**Objetivo:** a área de vendas da empresa gostaria de reduzir o Churn Rate, isto é, a Taxa de Evasão de Clientes. Para isso, solicitou uma análise da sua base de dados de clientes para identificar potenciais candidatos a cancelarem seus planos (telefônicos e de Internet) com a empresa.

**Material:** por meio da API da Alura Voz, é possível acessar os [dados de clientes da empresa](https://github.com/sthemonica/alura-voz/blob/main/Dados/Telco-Customer-Churn.json), que são retornados em um arquivo JSON. A empresa também disponibilizou um [dicionário de dados](https://github.com/sthemonica/alura-voz/blob/main/dicionario.md), com a explicação de cada uma das colunas.

**Entrega:** ao final de cada semana, será entregue um Notebook Python, desenvolvido no Google Colab, com  o código e análises efetuados para a resolução das atividades solicitadas pela empresa. Adicionalmente, poderão ser disponibilizados  arquivos CSV com resultados obtidos, bem como imagens de gráficos gerados. Tanto os Notebooks quanto os arquivos adicionais ficarão disponíveis no [Repositório criado para o projeto no GitHub](https://github.com/zingarelli/alura-challenges-data-science-2022).

---

## Semana 1
A primeira semana é dedicada a acessar a base de dados e ter um primeiro contato com as informações disponíveis. É necessário obter os dados da base e formatá-la, para posteriormente verificar sua estrutura, os tipos de dados e valores armazenados, e, por fim, fazer a limpeza e correções apropriadas de informações faltantes ou inconsistentes.

A empresa também fez duas solicitações adicionais que gostaria de ver sendo entregues na primeira semana: tradução das colunas, que estão em inglês, e criação de uma coluna adicional contendo os gastos diários de cada cliente da base.

### Resultados obtidos
A base de dados veio em um formato JSON, com os dados estruturados em diferentes níveis. Foi utilizada a função `json_normalize()` para tabular esses dados e ficarem mais fáceis de se visualizar e trabalhar no DataFrame.

Uma das colunas da base, `Charges.Total`, que contém o valor total gasto pelo cliente, estava com alguns registros em branco. Após análise, foi constatado se tratar de clientes novos, que provavelmente não tinham 1 mês completo de plano e que por isso ainda não havia valor nessa coluna. Para sanar esse problema, foi copiado o valor mensal desses clientes para a coluna de valor total. 

A coluna de `Churn` (que é o target desse projeto) possuía cerca de 3% dos seus registros vazios. Decidi por fazer uma cópia da base como um backup e então eliminar esses dados vazios na base a ser trabalhada.

Por fim, a base foi adaptada para o português, não apenas as colunas como também o conteúdo delas, para que ficasse tudo padronizado em um mesmo idioma. Foi também criada a coluna `gasto_diário`, calculada dividindo o valor de gasto mensal por 30.

O resultado final foi exportado para um [arquivo CSV](https://raw.githubusercontent.com/zingarelli/alura-challenges-data-science-2022/main/Semana-1/analise_semana_1.csv). O backup da base (sem a remoção dos dados de `Churn` vazios), também foi exportado para [outro arquivo CSV](https://raw.githubusercontent.com/zingarelli/alura-challenges-data-science-2022/main/Semana-1/dados_sem_churn_removido.csv).

## Semana 2
O foco nesta semana é analisar graficamente os dados, gerando diversos tipos de visualizações e cruzamento entre as colunas do banco. O foco maior é na variável Churn (que agora se chama "cancelou_plano"), por ser o target deste projeto. Graficamente, o objetivo é tentar encontrar relações e tendências entre as variáveis, bem como explorar a distribuição dos dados para tentar, visualmente, identificar perfis de clientes que optaram por sair do plano.

Foi também solicitado analisar a correlação entre as variáveis. Para isso, foi utilizada a fórmula para cálculo do Coeficiente de Correlação de Pearson e o resultado foi mostrado em um Heatmap, facilitando a visualização das correlações e sua força. 

## Semana 3 - Modelagem
Para esta semana final, foram criados dois modelos de Machine Learning para classificar os clientes entre aqueles que talvez venham a cancelar seus planos ou não. As features utilizadas para treinar os modelos foram baseadas nas conclusões obtidas na Semana 2. 

Antes da modelagem, foi feita uma análise de balanceamento do target, que de fato se encontrava desbalanceado: cerca de 75% da base era formada por clientes que não cancelaram o plano e o restante, por clientes que cancelaram. Para sanar este problema, explorou-se duas estratégias: oversampling, isto é, criação de novos dados sintéticos (com a técnica SMOTE) e undersampling, que seria o contrário, remoção de registros até se obter a mesma quantidade de valores no target (neste caso, utilizando a técnica Near Miss).

Os dois modelos criados foram testados para se obter algumas métricas: Acurácia, Precisão, Recall e F1. Foi também gerada a Matriz de Confusão (que mostra os acertos, os falsos positivos e falsos negativos), possibilitando uma análise visual dos resultados. Do  modelo que se saiu melhor nos teste, foi feita uma otimização dos parâmetros, em busca de melhoria nas métricas. 
