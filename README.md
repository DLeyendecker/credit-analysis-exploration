Exploração e análise de dados de crédito com SQL

Descrição dos Dados:
Os dados apresentam informações sobre clientes de um banco e incluem as seguintes colunas:

idade: idade do cliente
sexo: gênero do cliente (F ou M)
dependentes: número de dependentes do cliente
escolaridade: nível de escolaridade do cliente
salario_anual: faixa salarial do cliente
tipo_cartao: tipo de cartão do cliente
qtd_produtos: quantidade de produtos comprados nos últimos 12 meses
iteracoes_12m: quantidade de interações/transações nos últimos 12 meses
meses_inativo_12m: quantidade de meses que o cliente ficou inativo
limite_credito: limite de crédito do cliente
valor_transacoes_12m: valor das transações dos últimos 12 meses
qtd_transacoes_12m: quantidade de transações dos últimos 12 meses

A tabela foi criada no AWS Athena e os dados estão disponíveis em uma versão específica do S3 Bucket, acessível em: Dataset no GitHub

Exploração de Dados:
A fase inicial da análise visa compreender a natureza dos dados disponíveis. Iniciaremos a exploração com a seguinte pergunta:

#Quantidade de Informações na Base de Dados?
SELECT COUNT(*) FROM credito;

Resposta: 2564 linhas

#Como são os dados?
Query: SELECT * FROM credito LIMIT 10;
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/fd1eb22c-f918-457c-85a9-652d29c6f79a)

Observa-se a presença de dados nulos na tabela (indicados como 'na'). Vamos agora examinar mais detalhadamente os valores em cada coluna.

##Vamos identificar os tipos de dados presentes em cada coluna por meio da seguinte consulta.

Query: DESCRIBE credito

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/dd5915a8-b93a-47e3-8ba1-869d3fb03136)

Agora que ja entendemos quais são os tipos de dados, vamos olhar mais atentamente para as varíaveis que não são numéricas.

##Quais são os tipos de escolaridade disponíveis no dataset?

Query: SELECT DISTINCT escolaridade FROM credito

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/fc23f1ab-02d8-44ae-9b26-fc84d7c33c4d)

Os dados incluem diversos níveis de escolaridade, sendo perceptível a presença de valores nulos (na) no conjunto de dados. Abordaremos essa questão posteriormente durante o tratamento dos dados.

##Quais são os tipos de estado_civil disponíveis no dataset?

Query: SELECT DISTINCT estado_civil FROM credito

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/2bce9498-8064-4e30-af18-e270b2217594)

Observamos novamente a presença de valores nulos nos dados referentes ao estado civil.

##Quais são os tipos de salario_anual disponíveis no dataset?

Query: SELECT DISTINCT salario_anual FROM credito

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/93642ad4-10c0-4ae6-811c-794afedd5235)

Os salários neste conjunto de dados não são apresentados com o valor exato do ganho do cliente. Em vez disso, as informações estão categorizadas em faixas salariais. Além disso, há registros contendo valores nulos.

##Quais são os tipos de cartão disponíveis no dataset?

Query: SELECT DISTINCT tipo_cartao FROM credito

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/4042beee-3882-47b4-bb88-21f04becab62)

Aqui, observamos que não é necessário lidar com valores nulos.







