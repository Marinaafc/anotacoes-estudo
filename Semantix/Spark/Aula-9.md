# Struct Streaming - Conceitos
- Engine de processamento de stream construído na engine do Spark SQL
- Consultas do Spark Streaming são processadas usando uma engine de processamento de micro lote
  - Processar stream de dados como uma série de pequenos Jobs em batch
  - Latências de ponta a ponta de até 100 milissegundos
  - Tolerância de uma falha
 
- Novo modelo de stream - **Continuous processing**
  - Spark >= 2.3
  - Latências de ponta a ponta tão baixas quanto 1 milissegundo
  - Tolerância de uma falha
  - Sem alterar as operações Dataset / DataFrame em suas consultas

## Struct Streaming
- Trata um stream de dados como uma tabela que está sendo continuamente anexada
- Modelo de processamento de stream muito semelhante a um modelo de processamento em lote
![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/ae37da63-818b-466e-b3be-835784bf2ad7)
- https://spark.apache.org/docs/2.4.1/structured-streaming-programming-guide.html

# Leitura e Escrita de Stream
## Struct Streaming – Exemplo Socket
- Exemplo de leitura na porta 9999 no localhost
```python
read_str = spark.readStream.format("socket").
option("host", "localhost").option("port", 9999).load()
...
write_str = readStr.writeStream.format("console").start()
```
## Struct Streaming – Exemplo CSV
- Leitura de um arquivo csv
  - Obrigatoriedade a definição do Schema
  - Leitura de diretório, não arquivo
```python
user_schema = StructType().add(“nome", "string").add("idade", "integer")
read_csv_df = spark.readStream
  .schema(user_schema)
  .csv("/user/nomes/") 
read_csv_df.printSchema()
```
- Salvar dados
  - Orc
  - Json
  - Csv
  - Parquet
```python
read_csv_df.writeStream
  .format("csv")
  .option("checkpointLocation", "/tmp/checkpoint")
  .option("path", "/home/data")
  .start()
```
# Struct Streaming com Kafka - Conceitos
-  Structured Streaming
  - Versão >= Spark 2.0.0 (2.3)
  - https://spark.apache.org/docs/2.4.1/structured-streaming-kafka-integration.html
- Spark Streaming
  - Configurar os parâmetros do StreamContext
  - Configurar os parâmetros do Kafka
  - Configurar o Dstreams para leitura dos tópicos
- Structured Streaming
  - Configurar o Dataframe para leitura dos tópicos

# Struct Streaming com Kafka - Leitura
## Leitura de dados do Kafka em Batch
- Criação do Kafka Souce para consultas batch
```python
kafka_df = spark\
        .read\
        .format("kafka")\
        .option("kafka.bootstrap.servers", "host1:port1,host2:port2")\
        .option("subscribe", "topic1")\
        .load()
```
- Criação do Kafka Souce para consultas streaming
```python
kafka_df = spark\
      .readStream\
      .format("kafka")\
      .option("kafka.bootstrap.servers", "host1:port1,host2:port2")\
      .option("subscribe", "topic1")\
      .option("startingOffsets", "earliest")\
      .load()
```
# Struct Streaming com Kafka - Visualização e Escrita
## Visualizar dados do Kafka em Batch
## Visualizar dados do Kafka em Stream
## Enviar dados Stream para o Kafka
## Enviar dados Batch para o Kafka
