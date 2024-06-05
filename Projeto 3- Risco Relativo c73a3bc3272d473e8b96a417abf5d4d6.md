# Projeto 3- Risco Relativo

## Caso

No contexto recente, a diminuição das taxas de juros no mercado desencadeou um aumento significativo na demanda por solicitações de crédito. Os clientes veem uma oportunidade favorável para financiar compras importantes ou consolidar dívidas existentes, o que levou a uma afluência de solicitações de empréstimo no banco "Super Caja". A equipe de análise de crédito do banco está enfrentando uma carga de trabalho avassaladora devido à análise manual necessária para cada solicitação de empréstimo de clientes individuais. Essa metodologia manual resultou em um processo ineficiente e demorado, afetando negativamente a eficácia e a rapidez com que as solicitações de empréstimo são processadas. A situação se torna mais crítica devido à crescente preocupação com a taxa de inadimplência, um problema que está afetando cada vez mais a indústria financeira, aumentando a pressão sobre os bancos para identificar e mitigar os riscos associados ao crédito.

Com o objetivo de enfrentar esse desafio, a proposta é a automação do processo de análise utilizando técnicas avançadas de análise de dados, visando melhorar a eficiência, precisão e rapidez na avaliação das solicitações de crédito. Além disso, o banco já possui uma métrica para identificar clientes com pagamentos em atraso, o que poderia ser uma ferramenta valiosa para integrar na classificação de risco dentro do novo sistema automatizado.

O objetivo da análise é desenvolver um score de crédito a partir de uma análise de dados e avaliação do risco relativo que possa classificar os solicitantes em diferentes categorias de risco com base em sua probabilidade de inadimplência. Essa classificação permitirá ao banco tomar decisões informadas sobre quem conceder crédito, reduzindo assim o risco de empréstimos não reembolsáveis. Além disso, a integração da métrica existente de pagamentos em atraso fortalecerá a capacidade do modelo de identificar riscos, contribuindo assim para a solidez financeira e eficiência operacional do banco.

Alcance do projeto

Este projeto está dividido em 3 alcances, cada um marcado com um marco que determina o que você aprenderá e o que deve alcançar para considerá-lo concluído.

O marco 1 é sempre considerado indispensável para avaliar o projeto. Os demais são opcionais e destinam-se a permitir que você aprofunde alguma habilidade ou adiante um pouco do que será abordado em projetos futuros. Portanto, você pode completar apenas o marco 1 ou os marcos 1 e 2 ou os marcos 1 e 3 ou os marcos 1, 2 e 3.

COLUNAS:

| Arquivo | Variável | Descrição |
| --- | --- | --- |
| user_info | user id | Número de identificação do cliente (único para cada cliente) |
|  | age | Idade do cliente |
|  | sex | Gênero do cliente |
|  | last month salary | Último salário mensal que o cliente informou ao banco |
|  | number dependents | Número de dependentes |
| loans_outstanding | loan id | Número de identificação do empréstimo (único para cada empréstimo) |
|  | user id | Número de identificação do cliente |
|  | loan type | Tipo de empréstimo (real state = imóveis, others= outros) |
| loans_detail | user id | Número de identificação do cliente |
|  | more 90 days overdue | Número de vezes que o cliente apresentou atraso superior a 90 dias |
|  | using lines not secured personal assets | Quanto o cliente está utilizando em relação ao seu limite de crédito, em linhas que não são garantidas por bens pessoais, como imóveis e automóveis |
|  | number times delayed payment loan 30 59 days | Número de vezes que o cliente atrasou o pagamento de um empréstimo (entre 30 e 59 dias) |
|  | debt ratio | Relação entre dívidas e ativos do cliente. Taxa de endividamento = Dívidas / Patrimonio |
|  | number times delayed payment loan 60 89 days | Número de vezes que o cliente atrasou o pagamento de um empréstimo (entre 60 e 89 dias) |
| default | user id | Número de identificação do cliente |
|  | default flag | Classificação dos clientes inadimplentes (1 para clientes já registrados alguma vez como inadimplentes, 0 para clientes sem histórico de inadimplência) |
- 

**Etapa 1: Big Query: Estruturação e Modelagem**

Na consulta que executei, criei uma nova tabela chamada **projeto3-421317.Proj_3.dataset_risco_relativo.**

Passo a passo:

1- **Correção de informações do usuário** (user_info_corrected): Corrigi as informações dos usuários, preenchendo os valores ausentes na coluna last_month_salary com a média dos salários do último mês. Isso foi feito usando uma subconsulta para calcular a média dos salários e um CROSS JOIN para preencher os valores ausentes.

2- Correção de padrão de pagamento (default_corrected): Corrigi o padrão de pagamento, atribuindo 1 para casos em que o default_flag é igual a 1 e 0 para os outros casos. Isso foi feito usando uma estrutura CASE.

3- Correção de detalhes dos empréstimos (loans_detail_corrected): Corrigi os detalhes dos empréstimos, substituindo os valores nulos por 0 nas colunas relevantes.

Correção de empréstimos pendentes (loans_outstanding_corrected): Corrigi os empréstimos pendentes, substituindo os valores nulos por 0 nas colunas relevantes.

Correção de outliers (outliers_corrected): Identifiquei outliers nos dados de empréstimos usando medidas estatísticas como média e desvio padrão. Os outliers foram marcados com 1 nas colunas relevantes, caso a diferença entre o valor observado e a média fosse maior que 3 vezes o desvio padrão.

4- Seleção e renomeação das colunas na nova tabela: Por fim, selecionei as colunas relevantes da tabela corrigida e renomeei-as de acordo com o idioma em português. Isso foi feito usando a estrutura SELECT ... AS ....

Resultado: Uma série de correções nos dados relacionados a informações de usuários, padrões de pagamento e detalhes de empréstimos, identificação de outliers nos dados de empréstimos e, por fim, criação de uma nova tabela com as informações corrigidas e renomeadas em português.

1. Obtenção dos Dados no BigQuery:
    - Iniciei o projeto realizando consultas no BigQuery para obter informações sobre os empréstimos e os usuários.
2. Limpeza dos Dados:
    - Criei uma tabela limpa chamada loans_outstanding_cleaned para armazenar os tipos de empréstimos padronizados.
3. Sumarização dos Dados de Empréstimos por Usuário:
    - Criei a tabela user_loans_summary para resumir o número total de empréstimos por usuário.
4. Análise de Correlação entre Dados:
    - Calculei a correlação entre o número total de empréstimos por usuário e várias características dos usuários, como idade, salário, etc.
5. Correção e Preparação dos Dados:
    - Criei tabelas para corrigir e padronizar os dados de usuários, empréstimos e padrões, garantindo consistência e completude dos dados.
6. Criação do Conjunto de Dados Estruturado:
    - Combinei todas as tabelas corrigidas em uma tabela estruturada chamada dataset_risco_relativo_estruturado, que inclui informações detalhadas sobre os usuários e seus empréstimos, bem como a classificação de inadimplência, inclui o **Score de Crédito** que multiplica os dias em atrasos:
7. Análise Exploratória dos Dados:
    - Realizei uma análise exploratória dos dados, incluindo estatísticas descritivas e visualizações, para entender melhor a distribuição e as relações entre as variáveis.

Para criar o score de crédito:

- Atraso superior a 90 dias: atribuído peso: 3
- Atraso entre 60 a 89 dias atribuído peso: 2
- Atraso entre 30 a 59 dias atribuído peso: 1

O resultado da multiplicação desses pesos é somada à taxa de endividamento e o resultado é o Score de Crédito.

1. Exportação do Conjunto de Dados:
    - Exportei o conjunto de dados estruturado para um arquivo CSV chamado dataset_risco_relativo.csv, pronto para ser utilizado em análises e modelagem de machine learning.

[Consulta no BigQuery- Projeto 3- Risco Relativo](https://console.cloud.google.com/bigquery?project=projeto3-421317&ws=!1m4!1m3!8m2!1s6616295630!2s956ebfdefd7d4460a980e8d69a3e1f19)

(Tempo de dedicação: 4 dias)

**Etapa 2- Cálculo de Risco Relativo- Google Colab (Python)**

Com o aumento da demanda por crédito devido à queda das taxas de juros, a equipe de análise de crédito do banco "Super Caja" enfrenta desafios significativos. Nosso objetivo é automatizar o processo de análise de crédito para melhorar a eficiência e precisão, utilizando técnicas avançadas de análise de dados.

Descrição do Conjunto de Dados Utilizamos um conjunto de dados com 36.000 amostras e 14 variáveis, incluindo variáveis numéricas como idade, último salário informado, e variáveis categóricas como gênero e estado civil.

Análise exploratória detalhada, incluindo análise descritiva, visualização de dados e análise de correlações. Identificamos padrões, tendências e correlações entre as variáveis.

3. Cálculo do Risco Relativo

Após o cálculo do risco relativo, encontramos os seguintes resultados:

Atraso superior a 90 dias (5.2160):

Clientes com atraso superior a 90 dias possuem um risco de inadimplência 5,22 vezes maior que a média. Essa variável também demonstra um impacto significativo no risco.

Atraso entre 60 e 89 dias (4.7570):

Clientes com atraso entre 60 e 89 dias apresentam um risco de inadimplência 4,76 vezes maior que a média.

Atraso entre 30 e 59 dias (7.1770):

Clientes com histórico de atraso entre 30 e 59 dias apresentam um risco de inadimplência 7,18 vezes maior que a média da população. Esse é o fator de maior impacto no risco de inadimplência.

Taxa de endividamento (3.1630):

Clientes com uma alta taxa de endividamento têm um risco de inadimplência 3,16 vezes maior que a média.

Total de empréstimo por ID (2.0840):

Clientes com um valor total de empréstimos mais elevado apresentam um risco de inadimplência 2,08 vezes maior que a média.

Score segmentado (1.0900):

Essa variável, possivelmente proveniente de um modelo de score de crédito pré-existente, contribui para o risco de inadimplência com um impacto de 1,09 vezes a média.

Demais variáveis que não são tão importantes:

Idade (0.9841), número de dependentes (0.8274) e último salário informado (0.7459) apresentaram impacto relativamente baixo no risco de inadimplência.

4. Treinamento e Avaliação do Modelo

Foram utilizados diferentes algoritmos de aprendizado de máquina, incluindo Regressão Logística, Random Forest, XGBoost e LightGBM. O modelo com melhor desempenho foi o XGBoost, com acurácia de 84,05% no conjunto de treinamento. No entanto, após ajustes, o modelo Random Forest apresentou uma acurácia impressionante de 99,99%.

O desempenho do modelo foi avaliado em diferentes conjuntos de dados, utilizando técnicas como validação cruzada e holdout. Os resultados comprovam a robustez do modelo e sua capacidade de generalização para novos dados.

Análise Detalhada das Métricas Acurácia: A acurácia do modelo Random Forest ajustado foi de 99,99%, indicando sua capacidade de classificar corretamente as amostras do conjunto de dados.

Matriz de Confusão Ajustada (Random Forest).

Já quando olhamos para curva ROC AUC, identificamos que nos modelos ajustados, as previsões para bons pagadores são feitas corretamente para 79%  dos casos, enquanto maus pagadores são identificados corretamente 89% dos casos.

Conclusão

O modelo de predição de inadimplência desenvolvido para o banco "Super Caja" é robusto e eficaz, fornecendo informações valiosas para a tomada de decisão. Sua implementação permitirá ao banco melhorar a eficiência e rapidez na análise de crédito, reduzindo o risco de empréstimos não reembolsáveis e fortalecendo sua posição no mercado financeiro.

Os gráficos demonstram que em diversas modelagens de previsão as variáveis que contabilizam os números de dia em atraso e a taxa de endividamento são variáveis importantes para definir o perfil de risco dos clientes.

[Google Colab- Projeto 3- Risco Relativo](https://colab.research.google.com/drive/1Q--zEXfLQFZKjjaH-mmcpXYvqf1pumgE#scrollTo=xZ84GCUeA48o)[Trilha de Aprendizagem em Python- Projeto 3](https://docs.google.com/document/u/0/d/1ieL6AbhanGT9ylwEWjI4ChgM8vu0cR1W3C45GZVZXSA/edit)

**Etapa 3- Criação de Dashboard no Looker**

Para criar o dashboard no Looker, dois arquivos foram importados: dataset_risco_relativo e dataset_risco_relativo_rr, um com os dados manipulados anteriormente via BigQuery e outro com dados manipulados via Python no Google Colab.

A apresentação é dividida por tópicos:

1- Análise exploratória

2- Score de Crédito

3- Buscador Super Caja

5- Hipóteses

6- Modelos de Previsão

7- Pontos de Atenção

Na análise exploratória aspectos gerais foram levantados para ilustrar o aspecto do bando de dados antes e após intervenções e melhorias, ilustrando em gráficos a amostra da população de acordo com os aspectos encontrados, incluindo um gráfico que avalia as variáveis com maior risco relativo.

Em Score de Crédito é explicado a maneira de criação do mesmo e as regras utilizadas, além de ilustrar em gráfico de barras e linha a proporção da população de acordo com a faixa etária e o Score desses clientes.

O Buscador Super Caja entretanto, vem como a ferramenta de solução encontrada após a análise, testes e avaliação de hipóteses, uma ferramenta simples com um buscador de id e visualizações dos empréstimos ativos, dias que este cliente atrasou, taxa de endividamento e um apontador de score de crédito.

Em hipóteses nos aprofundamos nos gráficos de dispersão para compreender a distribuição desses clientes e avaliar seus comportamentos de acordo com cada variável, mostrando quais delas são fundamentais e quais não têm impacto.

Já na página do modelo de previsão, a ideia é mostrar a curva ROC AUC, que define a acurácia geral do modelo de acordo com alguns aspectos, dando enfase então no recall ou seja, a taxa de previsões corretas para bons ou maus pagadores, finaliza explicando a aplicabilidade e a eficiência do modelo de classificação de inadimplência.

Em pontos de atenção é levantado então o apontamento a descoberta feita na análise exploratória, indicando a diferença salarial informada por homens e mulheres e a taxa de endividamento maior para mulheres, indicando alguns insights como a criação de um modelo de crédito só para mulheres, diferentes taxas de juros, facilitação no pagamento e até a prática de menores parcelas.

Ainda em pontos de atenção, finalizamos indicando alguns aspectos que não foram encontrados no banco de dados como data de efetivação e finalização dos empréstimos, valor dos empréstimos e valor das dívidas; também é apontado um insight referente a classificação dos clientes, ao invés de maus ou bons pagadores, a ideia seria atribuir níveis de inadimplência.

[Dashboard- Super Caja](https://lookerstudio.google.com/u/0/reporting/88e9a323-6cc6-410b-b19f-715310512c30/page/p_tztbukukhd)

[Apresentação](https://www.youtube.com/watch?v=pJruKkJTj9I)