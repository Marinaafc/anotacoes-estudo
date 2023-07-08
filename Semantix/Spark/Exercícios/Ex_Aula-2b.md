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
- padrão: formato parquet com compressão snappy
- leitura_juros.write.saveAsTable("juros")

### 8. Visualizar todas as tabelas com o catalog
- spark.catalog.listTables()
- "isTemporaty=False" quer dizer que não é uma view

### 9. Visualizar no hdfs o formato e compressão que está a tabela juros do Hive
- !hdfs dfs -ls /user/hive/warehouse/juros

### 10. Ler e visualizar os dados da tabela juros, com uso de Dataframe no formato de Tabela Hive
- spark.read.table("juros").show(5)

### 11. Ler e visualizar os dados da tabela juros , com uso de Dataframe no formato Parquet
- spark.read.parquet("/user/hive/warehouse/juros").show(5)
- no caso de parquet, precisa especificar o caminho
- /user/hive/warehouse/ é diretório padrão para tabelas hives. especificar só o nome da tabela só é válido com tabelas hive, por isso que com parquet precisa especificar

### Ao finalizar, é preciso clicar em "Kernel" e depois em "Shutdown", pois deixar 2 sessões abertas resultará em erro ou conflito
