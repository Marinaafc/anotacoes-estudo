### 1. Ler o tópico do kafka “topic-kvspark” em modo batch
```python
from pyspark.sql.functions import *
topic_read = spark.read.format("kafka").option("kafka.bootstrap.servers", "kafka:9092").option("subscribe", "topic-kvspark").load()
#não pode colocar localhost porque está no container do jupyter-spark e não do kafka
```
### 2. Visualizar o schema do tópico
```python
topic_read.printSchema()
```
### 3. Visualizar o tópico com o campo key e value convertidos em string
```python
topic_string = topic_read.select(col("key").cast("string"), col("value").cast("string"))
topic_string.show()
#usando "string" no lugar de StringType não precisa importar os tipos
```
- Se der erro, pode usar o comando ```kafka-topics.sh --bootstrap-server kafka:9092 --list``` no container do Kafka para saber se o tópico existe
### 4. Ler o tópico do kafka “topic-kvspark” em modo streaming
```python
topic_read_stream = spark.readStream.format("kafka").option("kafka.bootstrap.servers", "kafka:9092").option("subscribe", "topic-kvspark").option("startingOffsets", "earliest").load()
```
### 5. Visualizar o schema do tópico em streaming
```python
kafka_stream.printSchema()
```
### 6. Alterar o tópico em streaming com o campo key e value convertidos para string
```python
topic_string_stream = topic_read_stream.select(col("key").cast("string"), col("value").cast("string"))
```
### 7. Salvar o tópico em streaming no tópico topic-kvspark-output a cada 5 segundos
```python
topic_string_stream_output = topic_string_stream.writeStream.format("kafka").option("kafka.bootstrap.servers", "kafka:9092").option("topic", "topic-kvspark-output").option("checkpointLocation","/user/marina/kafka_checkpoint").trigger(continuous="5 second").start()
#o tópico não vai ser criado no kafka até que seja enviada alguma informação pelo producer
```
- Por default, esse tópico vai ser criado com apenas 1 partição
### 8. Salvar o tópico na pasta hdfs://namenode:8020/user/<nome>/Kafka/topic-kvspark-output
```python
topic_string_stream_output = topic_string_stream.writeStream.format("csv").option("path", "/user/marina/kafka/topic-kvspark-output").option("checkpointLocation","/user/marina/kafka_checkpoint1").start()
#como está salvando em csv, não precisa falar quem é o broker e o tópico
#tem que colocar um diretório do checkpoint diferente do que foiusado antes para não acusar erro
!hdfs dfs -ls /user/marina/kafka/topic-kvspark-output
!hdfs dfs -cat /user/marina/kafka/topic-kvspark-output/part-00000-f2d65bfc-c99e-4c90-ab77-8d9b5e5ad51b-c000.csv
```
