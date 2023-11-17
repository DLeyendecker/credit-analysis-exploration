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
```
Query: SELECT COUNT(*) FROM credito;
```
<sub>Resposta: 2564 linhas<sub>

**Como são os dados?**
```
Query: SELECT * FROM credito LIMIT 10;
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/fd1eb22c-f918-457c-85a9-652d29c6f79a)

<sub>Observa-se a presença de dados nulos na tabela (indicados como 'na'). Vamos agora examinar mais detalhadamente os valores em cada coluna.<sub>

**Vamos identificar os tipos de dados presentes em cada coluna por meio da seguinte consulta.**
```
Query: DESCRIBE credito
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/72addd94-88a1-4df6-b30c-9f995059f125)


<sub>Agora que ja entendemos quais são os tipos de dados, vamos olhar mais atentamente para as varíaveis que não são numéricas.<sub>

**Quais são os tipos de escolaridade disponíveis no dataset?**
```
Query: SELECT DISTINCT escolaridade FROM credito
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/c8ad8529-9ddf-4341-b69d-e756d9a3faa1)

<sub>Os dados incluem diversos níveis de escolaridade, sendo perceptível a presença de valores nulos (na) no conjunto de dados. Abordaremos essa questão posteriormente durante o tratamento dos dados.<sub>

**Quais são os tipos de estado_civil disponíveis no dataset?**
```
Query: SELECT DISTINCT estado_civil FROM credito
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/92d743ce-9a8c-4eb6-a164-c969d05878b6)

<sub>Observamos novamente a presença de valores nulos nos dados referentes ao estado civil.<sub>

**Quais são os tipos de salario_anual disponíveis no dataset?**
```
Query: SELECT DISTINCT salario_anual FROM credito
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/d51d3d1a-42c3-4727-86bc-1f9546504f1e)

<sub>Os salários neste conjunto de dados não são apresentados com o valor exato do ganho do cliente. Em vez disso, as informações estão categorizadas em faixas salariais. Além disso, há registros contendo valores nulos.<sub>

**Quais são os tipos de cartão disponíveis no dataset?**
```
Query: SELECT DISTINCT tipo_cartao FROM credito
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/f7523c88-2e0a-4852-b444-edb4efda5990)

<sub>Aqui, observamos que não é necessário lidar com valores nulos.<sub>

### Análise de Dados

Após explorar os dados para compreender as informações disponíveis em nosso banco de dados, podemos agora realizar análises para entender melhor o panorama. Vamos iniciar com uma pergunta: 

**No conjunto de dados, quantos clientes temos em cada faixa salarial?**
```
Query: SELECT COUNT(*), salario_anual FROM credito GROUP BY salario_anual
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/ea9130db-81a4-4512-90b2-d03d69b574d4)

<sub>A predominância na base de dados é de clientes com renda inferior a 40K, havendo 235 casos em que a informação sobre a faixa salarial não foi fornecida ou está ausente. De certo modo, pode ser estrategicamente interessante para a empresa direcionar seu foco a esse público de menor renda.<sub>

**Nesse banco de dados, quantos clientes são homens e quantos são mulheres?**
```
Query: SELECT COUNT(*) AS qtd_sexo, sexo FROM credito GROUP BY sexo
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/20aaebc5-9a0e-4ce5-b60a-acfbf924c708)

![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/50c117a3-b46f-450a-84ea-81c7bd55e294)

<sub>A maioria dos clientes neste banco é do sexo masculino!<sub>

**Queremos focar o nosso marketing de maneira adequada para nossos clientes, qual será a idade deles?**
```
Query: SELECT AVG(idade) AS media_idade, MIN(idade) AS min_idade, MAX(idade) AS max_idade, sexo FROM credito GROUP BY sexo
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/9b68c434-086f-49d2-a0e3-b5ebcbb309df)

<sub>Essa análise não revelou informações significativas. Ambos os sexos apresentam a mesma idade mínima, e a média é bastante semelhante. A única disparidade notável está na idade máxima, mas isso é quase irrelevante devido à diferença não ser expressiva.<sub>

**Qual a maior e menor transação dos clientes?**
```
Query: SELECT MIN(valor_transacoes_12m) AS transacao_minima, MAX(valor_transacoes_12m) AS transacao_maxima FROM credito
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/82b2529d-b72f-41ac-aaa0-1bcd10e07b05)

<sub>Neste banco de dados, a soma das transações nos últimos 12 meses varia de 510.16 a 4776.58.<sub>

**Quais as características dos clientes que possuem os maiores creditos?**
```
Query: SELECT MAX(limite_credito) AS limite_credito, escolaridade, tipo_cartao, sexo FROM credito WHERE escolaridade != 'na' AND tipo_cartao != 'na' GROUP BY escolaridade, tipo_cartao, sexo ORDER BY limite_credito DESC LIMIT 10
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/2c8c2bd1-076a-48e3-a592-0c7764b0162e)

<sub>Parece não haver um impacto da escolaridade no limite, uma vez que o limite mais alto é oferecido a um homem sem educação formal. Além disso, não parece haver relação entre o tipo de cartão, a escolaridade e o limite. Entre os maiores limites, identificamos clientes com os cartões Gold, Silver, Platinum e Blue.<sub>

**Quais as características dos clientes que possuem os menores creditos?**
```
Query: SELECT MAX(limite_credito) AS limite_credito, escolaridade, tipo_cartao, sexo FROM credito WHERE escolaridade != 'na' AND tipo_cartao != 'na' GROUP BY escolaridade, tipo_cartao, sexo ORDER BY limite_credito ASC
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/5838f508-4f93-4d94-ab1e-1d54684f80ca)

<sub>Na análise atual, notamos que não há clientes com cartão Platinum entre os menores limites. Além disso, observamos que a maioria dos limites mais baixos pertence a mulheres, enquanto nos limites mais altos, predominam homens.<sub>

**Será que as mulheres gastam mais?**
```
Query: SELECT MAX(valor_transacoes_12m) AS maior_valor_gasto, AVG(valor_transacoes_12m) AS media_valor_gasto, MIN(valor_transacoes_12m) AS min_valor_gasto, sexo FROM credito GROUP BY sexo
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/9981987e-89f5-4e9f-988e-0ae1632eb676)

<sub>Apesar das diferenças nos limites de crédito, os gastos entre homens e mulheres mostram-se semelhantes!<sub>

Por fim,

**O salário impacta no limite?**
```
Query: SELECT AVG(qtd_produtos) AS qts_produtos, AVG(valor_transacoes_12m) AS media_valor_transacoes, AVG(limite_credito) AS media_limite, sexo, salario_anual FROM credito WHERE salario_anual != 'na' GROUP BY sexo, salario_anual ORDER BY AVG(valor_transacoes_12m) DESC
```
![image](https://github.com/DLeyendecker/credit-analysis-exploration/assets/123911132/4774ae95-319e-478e-afd2-7d0e5c1f0d16)

<sub>Sim! Indivíduos com faixa salarial mais baixa também possuem limites de crédito mais baixos<sub>

**Conclusão:**

Com base nas análises realizadas no conjunto de dados de crédito, destacam-se alguns insights intrigantes:

- A maioria dos clientes possui uma renda de até 40K.
- A predominância de clientes é do sexo masculino.
- A escolaridade não parece ser um fator determinante para o limite ou tipo de cartão.
- Os clientes com os limites mais elevados são predominantemente homens.
- Mulheres compõem a maioria dos clientes com limites mais baixos.
- Não há presença de cartão Platinum entre os clientes com limites mais baixos.
- A faixa salarial exerce uma influência direta sobre o limite de crédito.
- Não foram identificados clientes do sexo feminino com salário anual acima de 60K.
  
Essas conclusões proporcionam uma visão abrangente do perfil dos clientes, fornecendo informações valiosas para estratégias de marketing e tomada de decisões no âmbito financeiro.










