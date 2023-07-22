## Kafka

### 1. Preparação do ambiente no Kafka

a) Criar o tópico “topic-kvspark” com 2 partições e o fator de replicação = 1
```
kafka-topics.sh --bootstrap-server kafka:9092 --topic topic-kvspark --create --partitions 2 --replication-factor 1
```

b) Inserir as seguintes mensagens no tópico (Chave, Valor):

o 1, Msg1

o 2, Msg2

o 3, Msg3

```
kafka-console-producer.sh --broker-list kafka:9092 --topic topic-kvspark --property parse.key=true --property key.separator=,
```

c) Criar um consumidor no Kafka para ler o “topic-kvspark”

```
kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic topic-kvspark
```

## Spark

### 1. Criar um consumidor em Scala usando Spark Streaming para ler o “topic-kvspark” no cluster Kafka ”kafka:9092”
```scala
spark-shell --packages org.apache.spark:spark-streaming-kafka-0-10_2.11:2.4.1
scala> import org.apache.kafka.clients.consumer.ConsumerRecord
scala> import org.apache.kafka.common.serialization.StringDeserializer
scala> import org.apache.spark.streaming.{Seconds, StreamingContext}
scala> import org.apache.spark.streaming.kafka010._
scala> import org.apache.spark.streaming.kafka010.LocationStrategies.PreferConsistent
scala> import org.apache.spark.streaming.kafka010.ConsumerStrategies.Subscribe
scala> val kafkaParams = Map[String, Object](
  "bootstrap.servers" -> "kafka:9092",
  "key.deserializer" -> classOf[StringDeserializer],
  "value.deserializer" -> classOf[StringDeserializer],
  "group.id" -> "aplicacao2",
  "auto.offset.reset" -> "earliest",
  "enable.auto.commit" -> (false: java.lang.Boolean)
)
scala> val ssc = new StreamingContext(sc,Seconds(5))
scala> val topic = Array("topic-kvspark")
scala> val stream = KafkaUtils.createDirectStream[String, String](
  ssc,
  LocationStrategies.PreferConsistent,
  ConsumerStrategies.Subscribe[String, String](topic, kafkaParams)
)
```

### 2. Visualizar o tópico com as seguintes informações

Nome do tópico
Partição
Chave
Valor
```scala
scala> val info_dstream = stream.map(record => (
        record.topic,
        record.partition,
        record.key,
        record.value
      ))
scala> info_dstream.print()
```
### 3. Salvar o tópico no diretório hdfs://namenode:8020/user/<nome>/kafka/dstreamkv
```scala
info_dstream.saveAsTextFiles("/user/marina/kafka/dstreamkv")
ssc.start()
hdfs dfs -ls /user/marina/kafka
hdfs dfs -ls /user/marina/kafka/dstreamkv-1690043665000
hdfs dfs -cat /user/marina/kafka/dstreamkv-1690043665000/part-00001
hdfs dfs -cat /user/marina/kafka/dstreamkv-1690043665000/part-00000
```
- As mensagens não são mostradas por ordem de envio, porque está dividido em partições e é feita por ordem da partição que estiver mais próxima para leitura;
- Cada chave vai ficar sempre em uma partição específica, por exemplo, mensagem da chave 1 fica sempre na partição 1
