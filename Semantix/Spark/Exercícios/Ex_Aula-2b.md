docker-compose -f docker-compose-parcial.yml up -d
http://localhost:8889/

### 1. Criar o arquivo do notebook com o nome teste_spark.ipynb
- clicar em "new"
- clicar no título e renomear para "teste_spark"

### 2. Obter as informações da sessão de spark (spark)
- clicar na célula e digitar "spark"
- shift enter

### 3. Obter as informações do contexto do spark (sc)
- "sc"
- ctrl enter
- b

### 4. Setar o log como INFO.
- spark.sparkContext.setLogLevel("INFO")
- shift tab - mostra descrição

### 5. Visualizar todos os banco de dados com o catalog
- spark.catalog.listDatabases()
- quando tem [*] quer dizer que está carregando

### 6. Ler os dados "hdfs://namenode:8020/user/rodrigo/data/juros_selic/juros_selic.json“ com uso de Dataframe
```scala
leitura_juros = spark.read.json("hdfs://namenode:8020/user/marina/data/juros_selic/juros_selic.json")

leitura_juros.show(5)
```
- "hdfs:///user/marina/data/juros_selic/juros_selic.json" também vai funcionar, porque já se sabe que o hdfs fica no namenode e a porta deste
- "/user/marina/data/juros_selic/juros_selic.json" também vai funcionar, porque a leitura do hdfs está como default
- em alguns projetos, é preciso especificar o caminho completo

### 7. Salvar o Dataframe como juros no formato de tabela Hive

### 8. Visualizar todas as tabelas com o catalog

### 9. Visualizar no hdfs o formato e compressão que está a tabela juros do Hive

### 10. Ler e visualizar os dados da tabela juros, com uso de Dataframe no formato de Tabela Hive

### 11. Ler e visualizar os dados da tabela juros , com uso de Dataframe no formato Parquet
