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

---

## Semana 2
O foco nesta semana é analisar graficamente os dados, gerando diversos tipos de visualizações e cruzamento entre os valores nas colunas do banco. Grande parte dos gráficos envolveu a coluna `Churn` (que agora se chama `cancelou_plano`), por ser o target do projeto, aquilo que busca-se entender. 

Graficamente, o objetivo foi procurar relações e tendências entre `cancelou_plano` e as outras colunas, bem como explorar a distribuição dos dados, buscando identificar, visualmente, perfis de clientes que optaram por sair do plano.

A empresa também solicitou como atividade extra analisar a correlação entre as variáveis. 

Foi utilizado o arquivo CSV criado na primeira semana, contendo a base tratada e adaptada para português. Muito dos gráficos foram criados utilizando a biblioteca [Seaborn](https://seaborn.pydata.org).

### Resultados obtidos

Fiz duas suposições e depois tentei verificar a validade delas por meio de gráficos relacionando `cancelou_plano` com outras colunas:

- Suposição 1: um dos motivos para uma pessoa cancelar o plano é o valor pago.

- Suposição 2: clientes fiéis (com mais meses de plano ativo) dificilmente cancelam o plano.

**Suposição 1**

Para verificar a primeira suposição, criei um gráfico de distribuição categórica (catplot) com as colunas `cancelou_plano` e `gasto_mensal`. Para melhor visualizar a concentração dos pontos no gráfico, utilizei o chamado [violinplot](https://seaborn.pydata.org/generated/seaborn.violinplot.html), em que, quanto maior a área ao redor do eixo y de cada região, maior a concentração de dados nela. 

O gráfico abaixo mostra que, de fato, houve uma concentração de cancelamentos nos clientes cujos gastos mensais ficam por volta de 70 a 110 dólares, que representam os planos mais caros. A região em azul representa os clientes que não cancelaram o plano, enquanto a região em laranja representa os que cancelaram o plano.

![Gráfico de comparação entre variáveis cancelou_plano e gasto_mensal](https://user-images.githubusercontent.com/19349339/171524052-d9e7f0c2-cfda-4b9d-9f77-4b5167b24f03.png)

Verifiquei também como a mesma distribuição se comportava ao separar os dados por gênero e entre os maiores de 65 anos.

No gráfico abaixo, a região azul representa clientes do gênero feminino e a região laranja, do gênero masculino. Foi possível verificar que, para os clientes que não cancelaram o plano, a distribuição de gasto mensal por gênero é bem semelhante. No entanto, dentre aqueles que cancelavam o plano havia um pequeno aumento nas mulheres na faixa de gastos mensais entre 70 e 90 dólares.

![Gráfico de comparação entre variáveis cancelou_plano e gasto_mensal, separado por gênero](https://user-images.githubusercontent.com/19349339/171524723-68bd1bd7-b3a6-4d49-bd61-75d52f38b93d.png)

Por fim, no gráfico abaixo se encontra a distribuição entre os clientes maiores de 65 anos (em laranja) e os abaixo de 65 anos (azul). Foi possível notar que há um maior número de cancelamentos entre os que possuem mais de 65 anos e planos mais caros. Curiosamente, foi possível também notar que pessoas abaixo de 65 anos e que possuem o plano ativo se concentram em gastos mensais por volta de 20 dólares. Uma estratificação mais detalhada por idade poderia trazer mais detalhes desse fato.

![Gráfico de comparação entre variáveis cancelou_plano e gasto_mensal, separado por maiores de 65 anos ou não](https://user-images.githubusercontent.com/19349339/171524969-3e66a69e-c01b-4025-8df6-916fbf838791.png)

**Suposição 2**

Nesta segunda suposição, utilizei a função [displot](https://seaborn.pydata.org/generated/seaborn.displot.html) para criar um histograma relacionando as colunas `cancelou_plano` e `meses_contrato`. Por meio do parâmetro `multiple='fill'`, foi possível sobrepor os dois histogramas, criando um gráfico único que mostrava a proporção entre os clientes que cancelaram ou não o plano, de acordo com a quantidade de meses de contrato.

No gráfico abaixo, estão representados em laranja os clientes que cancelaram o plano, e em azul, os que não cancelaram. Os meses de contrato estão no eixo y. No eixo x temos a contagem proporcional de clientes, entre 0 e 1, que também pode ser interpretada como uma porcentagem. É possível notar uma saída expressiva de clientes com menos tempo de contrato (mais de 50% entre 0 e 5 meses) e que a quantidade de cancelamentos vai gradualmente diminuindo conforme a quantidade de meses de contrato aumenta.

![Histograma comparando as variáveis cancelou_plano e meses_contrato](https://user-images.githubusercontent.com/19349339/171526282-bf1dce66-24f0-401e-a5c8-f044a8987409.png)

Olhando com mais detalhes para os clientes com tempo de contrato até 5 meses, foi possível verificar que a maior parte dos cancelamentos ocorrem logo após o primeiro mês. Isso pode ser visto no gráfico abaixo, que mostra o histograma entre 0 e 5 meses de contrato, divididos entre os que cancelaram o plano (histograma azul) e os que não cancelaram (histograma laranja). 

![Histograma dos clientes que cancelaram ou não o plano, entre aqueles que possuem até 5 meses de contrato](https://user-images.githubusercontent.com/19349339/171527057-76f8b308-1d3b-4fea-8832-12daa2bc5640.png)

Esse fato levanta um questionamento: **será que a empresa oferece algum tipo de "degustação" de um mês grátis** e que, por isso, há tantos cancelamentos concentrados após um mês de contrato?

**Conclusão**

Baseado nos gráficos aqui apresentados e em outros que podem ser vistos no [Notebook desta semana](https://github.com/zingarelli/alura-challenges-data-science-2022/blob/main/Semana-2/Alura_Challenges_%7C_Data_Science_2022_Semana_02.ipynb), foi possível concluir que os clientes que **cancelaram** seus planos tinham gastos mensais na faixa de **70 a 110 dólares**, com uma concentração ligeiramente maior por volta de 80 dólares. Dentro dessa faixa, há uma quantidade maior de clientes **mulheres** e uma concentração relevante de pessoas com **idade maior ou igual a 65 anos**.

 Foi também possível observar que a maior parte dos **cancelamentos** está concentrada em clientes **novos**, com **até 5 meses de contrato**, e que muitos deles estão **cancelando o plano logo após o primeiro mês** de contrato.

 Por fim, há também uma concentração maior de **cancelamentos** entre clientes que **não possuem dependentes**. Além disso, quando se olha para o tipo de contrato feito (1 ano, mensal ou 2 anos) a **grande maioria** dos cancelamentos ocorre nos clientes com **plano mensal**. 

**Atividade Extra**

Para verificar a correlação entre as colunas da base de dados, foi utilizada a função [corr()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.corr.html), com o cálculo do coeficiente de correlação pelo método de Pearson. O resultado foi mostrado em um Heatmap, que facilita a visualização das correlações e sua força por meio de cores de intensidade e tonalidades diferentes. O heatmap foi salvo em um [arquivo PNG](https://github.com/zingarelli/alura-challenges-data-science-2022/blob/main/Semana-2/heatmap.png).

## Semana 3 - Modelagem
Para esta semana final (a quarta semana é para organizar o projeto e refinar o storytelling), o foco é preparar a base para criação de modelos de Machine Learning que consigam fazer a classificação dos clientes entre aqueles que talvez venham a cancelar seus planos ou não, que é o target do projeto. Para isso, deve-se verificar se o target está balanceado e corrigir este problema caso não esteja. Antes de utilizar os dados da base para o treinamento dos modelos, também é necessário efetuar o encoding dos valores categóricos. 

Com essas etapas superadas, é possível seguir com a criação e treinamento de dois modelos de Machine Learning e testá-los com os dados da base, comparando os resultados das métricas obtidos em cada um para escolher o melhor deles, que irá, por fim, passar por uma etapa de otimização. As features utilizadas para treinar os modelos são aquelas identificadas na Semana 2. 

### Resultados obtidos

A análise de balanceamento do target revelou que este de fato se encontrava desbalanceado: cerca de 75% da base é formada por clientes que não cancelaram o plano e o restante, por clientes que cancelaram. Uma variável balanceada precisa ter uma quantidade equivalente de registros para cada um dos valores possíveis que ela pode assumir. No caso desta base, o target possui dois valores possíveis: `Sim` (`1`) e `Nao` (`0`). Para sanar este problema, foram exploradas duas estratégias: oversampling, isto é, criação de novos dados sintéticos (utilizando a técnica [SMOTE](https://imbalanced-learn.org/stable/references/generated/imblearn.over_sampling.SMOTE.html)) e undersampling, que seria o contrário, remoção de registros até se obter a mesma quantidade de registros para os dois valores possíveis para o target (neste caso, utilizando a técnica [Near Miss](https://imbalanced-learn.org/stable/references/generated/imblearn.under_sampling.NearMiss.html)).

Como a variáveis categóricas possuíam entre 2 a 4 valores possíveis (com exceção do ID do cliente, que não foi utilizado na modelagem), primeiramente apliquei a técnica de Label Encoding em cada coluna, que é mais simples. Nessa técnica, cada valor diferente é convertido para um valor numérico. Por exemplo, `Nao` pode ser convertido para `0` e `Sim` para `1` e assim por diante para cada novo valor. Outra técnica que foi aplicada, porém em um momento posterior do projeto, foi o [One-Hot Encoding](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html). No One-Hot, cada coluna é substituída por várias outras colunas, uma para cada um dos valores possíveis, aplicando-se 1 na coluna em que o valor está presente e 0 nas restantes. Isso é feito para que não se tenha nenhum tipo de relação de hierarquia entre os valores (por exemplo, o modelo "entender" que um valor maior tem mais relevância que um valor menor).

Os dois modelos criados foram testados para se obter algumas métricas: Acurácia, Precisão, Recall e F1. Foi também gerada a Matriz de Confusão (que mostra os acertos, os falsos positivos e falsos negativos), possibilitando uma análise visual dos resultados. Do  modelo que se saiu melhor nos teste, foi feita uma otimização dos parâmetros, em busca de melhoria nas métricas. 
