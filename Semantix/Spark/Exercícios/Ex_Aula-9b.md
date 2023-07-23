### 1. Ler o tópico do kafka “topic-kvspark” em modo batch
```python
kafka_df = spark.read.format("kafka").option("kafka.bootstrap.servers", "kafka":9092).option("subscribe", "topic-kvspark").load()
```
### 2. Visualizar o schema do tópico
```python
kafka_df.printSchema
```
### 3. Visualizar o tópico com o campo key e value convertidos em string
```python
from pyspark.sql.types import *
kafka_df.select(col("key").cast(StringType), col("value").cast(StringType)).show()
```
### 4. Ler o tópico do kafka “topic-kvspark” em modo streaming
```python
kafka_stream = spark.readStream.format("kafka").option("kafka.bootstrap.servers", "kafka":9092).option("subscribe", "topic-kvspark").option("startingOffsets", "earliest").load()
```
### 5. Visualizar o schema do tópico em streaming
```python
kafka_stream.printSchema
```
### 6. Alterar o tópico em streaming com o campo key e value convertidos para string
```python
kafka_stream.select(col("key").cast(StringType), col("value").cast(StringType))
kafka_stream.start()
```
### 7. Salvar o tópico em streaming no tópico topic-kvspark-output a cada 5 segundos
```python
kafka_output = kafka_stream.writeStream.format("kafka").option("kafka.bootstrap.servers", "kafka":9092).option("topic", "topic-kvspark-output").trigger(Trigger.Continuous("5 seconds")).start()
```
### 8. Salvar o tópico na pasta hdfs://namenode:8020/user/<nome>/Kafka/topic-kvspark-output
```python
kafka_output.writeStream.format("kafka").option("path", "/user/marina/Kafka/topic-kvspark-output").start()
```
