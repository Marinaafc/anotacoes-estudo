## Kafka

### 1. Preparação do ambiente no Kafka

a) Criar o tópico “topic-spark” com 1 partição e o fator de replicação = 1
```
kafka-topics.sh --bootstrap-server kafka:9092 --topic topic-spark --create --partitions 1 --replication-factor 1
kafka-topics.sh --bootstrap-server kafka:9092 --list
```
b) Inserir as seguintes mensagens no tópico:

o Msg1  

o Msg2  

o Msg3
```
kafka-console-producer.sh --broker-list kafka:9092 --topic topic-spark
```
c) Criar um consumidor no Kafka para ler o “topic-spark”
```
kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic topic-spark
```
## Spark

### 1. Criar um consumidor em Scala usando Spark Streaming para ler o “topic-spark” no cluster Kafka ”kafka:9092”
```scala
spark-shell --packages org.apache.spark:spark-streaming-kafka-0-10_2.11:2.4.1 //em jupyter-spark
scala> import org.apache.kafka.clients.consumer.ConsumerRecord
scala> import org.apache.kafka.common.serialization.StringDeserializer
scala> import org.apache.spark.streaming.{Seconds, StreamingContext}
scala> import org.apache.spark.streaming.kafka010._
scala> import org.apache.spark.streaming.kafka010.LocationStrategies.PreferConsistent
scala> import org.apache.spark.streaming.kafka010.ConsumerStrategies.Subscribe
scala> val kafkaParams = Map[String, Object](
  "bootstrap.servers" -> "localhost:9092",
  "key.deserializer" -> classOf[StringDeserializer],
  "value.deserializer" -> classOf[StringDeserializer],
  "group.id" -> "app-teste",
  "auto.offset.reset" -> "earliest",
  "enable.auto.commit" -> (false: java.lang.Boolean)
)
scala> val ssc = new StreamingContext(sc,Seconds(10))
scala> val topics = Array("topic-spark")
scala> val dstream = KafkaUtils.createDirectStream[String, String](
  ssc,
  LocationStrategies.PreferConsistent,
  ConsumerStrategies.Subscribe[String, String](topics, kafkaParams)
)
```
### 2. Visualizar o tópico com as seguintes informações

Nome do tópico  
Partição  
Valor
```scala
scala> val info_dstream = dstream.map(record => (
        record.topic,
        record.partition,
        record.value
      ))
scala> info_dstream.print()
```
### 3. Salvar o tópico no diretório hdfs://namenode:8020/user/<nome>/kafka/dstream
```scala
info_dstream.saveAsTextFiles("hdfs://namenode:8020/user/marina/kafka/dstream")
```
