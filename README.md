### Exploração e análise de dados de crédito com SQL

**Descrição dos Dados:**
Os dados apresentam informações sobre clientes de um banco e incluem as seguintes colunas:

| Campo                 | Descrição                                                |
|-----------------------|----------------------------------------------------------|
| idade                 | Idade do cliente                                         |
| sexo                  | Gênero do cliente (F ou M)                               |
| dependentes           | Número de dependentes do cliente                         |
| escolaridade          | Nível de escolaridade do cliente                         |
| salario_anual         | Faixa salarial do cliente                                |
| tipo_cartao           | Tipo de cartão do cliente                                |
| qtd_produtos          | Quantidade de produtos comprados nos últimos 12 meses    |
| iteracoes_12m         | Quantidade de interações/transações nos últimos 12 meses |
| meses_inativo_12m     | Quantidade de meses que o cliente ficou inativo          |
| limite_credito        | Limite de crédito do cliente                             |
| valor_transacoes_12m  | Valor das transações dos últimos 12 meses                |
| qtd_transacoes_12m    | Quantidade de transações dos últimos 12 meses            |


A tabela foi criada no AWS Athena e os dados estão disponíveis em uma versão específica do S3 Bucket, acessível em: Dataset no GitHub

### Exploração de Dados:
A fase inicial da análise visa compreender a natureza dos dados disponíveis. Iniciaremos a exploração com a seguinte pergunta:

**Quantidade de Informações na Base de Dados?**

```SELECT COUNT(*) FROM credito;```

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

Análise de Dados

Após explorar os dados para compreender as informações disponíveis em nosso banco de dados, podemos agora realizar análises para entender melhor o panorama. Vamos iniciar com uma pergunta: 

##No conjunto de dados, quantos clientes temos em cada faixa salarial?

Query: select count(*), salario_anual from credito group by salario_anual

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/94d4e0e8-dc83-4206-811e-ada6b2a07e23)

A predominância na base de dados é de clientes com renda inferior a 40K, havendo 235 casos em que a informação sobre a faixa salarial não foi fornecida ou está ausente. De certo modo, pode ser estrategicamente interessante para a empresa direcionar seu foco a esse público de menor renda.

##Nesse banco de dados, quantos clientes são homens e quantos são mulheres?

Query: select count(*), sexo from credito group by sexo

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/78f1af32-5497-4998-a37f-0fd3f4f17c95)

A maioria dos clientes neste banco é do sexo masculino!

##Queremos focar o nosso marketing de maneira adequada para nossos clientes, qual será a idade deles?

Query: select avg(idade) as media_idade, min(idade) as min_idade, max(idade) as max_idade, sexo from credito group by

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/16c006d1-962c-4022-8b97-a7d71e0ec6bd)

Essa análise não revelou informações significativas. Ambos os sexos apresentam a mesma idade mínima, e a média é bastante semelhante. A única disparidade notável está na idade máxima, mas isso é quase irrelevante devido à diferença não ser expressiva.

Qual a maior e menor transação dos clientes?

Query: select min(valor_transacoes_12m) as transacao_minima, max(valor_transacoes_12m) as transacao_minima from credito

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/3bda6071-f540-4c65-9042-bb3883f39722)

Neste banco de dados, a soma das transações nos últimos 12 meses varia de 510.16 a 4776.58.

##Quais as características dos clientes que possuem os maiores creditos?

Query: select max(limite_credito) as limite_credito, escolaridade, tipo_cartao, sexo from credito where escolaridade != 'na' and tipo_cartao != 'na' group by escolaridade, tipo_cartao, sexo order by limite_credito desc limit 10

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/5838f508-4f93-4d94-ab1e-1d54684f80ca)

Parece não haver um impacto da escolaridade no limite, uma vez que o limite mais alto é oferecido a um homem sem educação formal. Além disso, não parece haver relação entre o tipo de cartão, a escolaridade e o limite. Entre os maiores limites, identificamos clientes com os cartões Gold, Silver, Platinum e Blue.

##Quais as características dos clientes que possuem os menores creditos?

Query: select max(limite_credito) as limite_credito, escolaridade, tipo_cartao, sexo from credito where escolaridade != 'na' and tipo_cartao != 'na' group by escolaridade, tipo_cartao, sexo order by limite_credito asc

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/2c8c2bd1-076a-48e3-a592-0c7764b0162e)

Na análise atual, notamos que não há clientes com cartão Platinum entre os menores limites. Além disso, observamos que a maioria dos limites mais baixos pertence a mulheres, enquanto nos limites mais altos, predominam homens.















