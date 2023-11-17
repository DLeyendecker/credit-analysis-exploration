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

Quantidade de Informações na Base de Dados?
SELECT COUNT(*) FROM credito;

Resposta: 2564 linhas

Como são os dados
Query: SELECT * FROM credito LIMIT 10;
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/fd1eb22c-f918-457c-85a9-652d29c6f79a)

Observa-se a presença de dados nulos na tabela (indicados como 'na'). Vamos agora examinar mais detalhadamente os valores em cada coluna.
