# Integração do Spark Stream com Kafka
## Spark Streaming com Kafka
- Kafka atualmente é o mais utilizado na parte de ingestão de dados
- Formas de integrar Kafka no Spark
  - Spark Streaming (DStream)
    - Versão >= Spark 0.7.0 (disponível a partir dessa versão)
   
  - Structured Streaming
    - Versão >= Spark 2.0.0 (2.3) (disponível desde a 2.0, mas era uma versão beta. o ideal é a partir da 2.3)

   
 . | spark-streaming-kafka-0-8 | spark-streaming-kafka-0-10
---- | ----------------------------| ---------
Broker Version | 0.8.2.1 or higher | 0.10.0 or higher
API Maturity | Deprecated Spark 2.3 | Stable
Language Support | Scala, Java, Python | Scala, Java
Receiver DStream | Yes | No
Direct DStream | Yes | Yes
SSL / TLS Support | No | Yes
Offset Commit API | No | Yes
Dynamic Topic Subscription | No | Yes 

- A biblioteca spark-streaming-kafka-0-8 aceita versão do broker 0.10.0 or higher, mas a biblioteca spark-streaming-kafka-0-10 não aceita versão do broker menor que a 0.10.0;
- No Spark 2.3 a API do Kafka 0-8 foi depreciada, porque no 2.3 surgiu o Structed Stream para fazer essa substituição;
- Então tendo um Kafka mais antigo e o Spark mais antigo, deve-se utilizar o kafka 0-8, mas no curso a versão do spark é 2.4, então será utilizado o 0-10;
- No curso, só vai ser falado da biblioteca 0-10.

- Spark Streaming (DStreams)
  - Scala e Java
    - Versão >= Spark 0.7.0
  - Python
    - Versão >= Spark 1.2
    - Versão <= Spark 2.3
   
- Para receber dados de um ou mais tópicos do Kafka (leitura no Spark), precisa:
  - Configurar os parâmetros do StreamContext (segundos para processar os dados, nome da aplicação, modo, etc)
  - Configurar os parâmetros do Kafka
  - Configurar o DStreams para leitura dos tópicos

# Revisão de comandos do Kafka
## Exemplos da comandos no Kafka
- Acessar o container do Kafka
  - docker exec --it kafka bash
 
- Criação de tópico
  - **kafka-topics --bootstrap-server kafka:9092 --topic topicTeste --create --partitions 1 --replication-factor 1**
    - Dependendo de como está se utilizando pode ter um .sh do executável após "kafka-topics"
    - --help mostra todas as operações disponíveis
    - --replication-factor tem que ser 1 porque no cluster do curso só tem 1 nó
   
- Criação do produtor
  - **kafka-console-producer --broker-list kafka:9092 --topic topicTeste**
 
- Criação do consumidor
  - **kafka-console-consumer --bootstrap-server kafka:9092 --topic topicTeste**
    - Pode consumir pelo kafka-console-consumer ou pelo próprio Spark
    - É bom utilizar o kafka-console-consumer para testar se a comunicação está funcionando, porque, se não estiver, também não vai funcionar no Spark
    - Com o teste, por mais que já tenha consumido pelo kafka-console-consumer, tem a opção de consumir tudo de novo no Spark
   
- Criação do produtor com Chave/Valor
  - **kafka-console-producer --broker-list kafka:9092 --topic topicTeste --property parse.key=true --property key.separator=,**
    - parse.key - habilita para ser chave/valor;
    - vai aceitar uma chave e quando estiver consumindo pelo kafka-console-consumer vai retornar apenas o valor e não a chave;
    - key.separator=, - vai separar a chave do valor pela vírgula;
    - o padrão é ler apenas o valor, mas pode ser configurado (na leitura) para ler só a chave ou a chave e o valor ou até a chave, o valor e a partição
   
- Criação do produtor enviando um arquivo
  - **kafka-console-producer --broker-list kafka:9092 --topic topicTeste < file.log**
   
- Criação do produtor enviando um arquivo com Chave/Valor
  - **kafka-console-producer --broker-list kafka:9092 --topic topicTeste --property parse.key=true --property key.separator=: < file.log**

# Dependências do Spark Streaming com Kafka 0.10
- Para o Spark Streaming fazer a comunicação com o Kafka, é preciso importar as dependências do kafka;
- Adicionar pacote do Kafka (no spark-shell)
  - ```--packages org.apache.spark:spark-streaming-kafka-0-10_<versãoScala>:<versãoSpark>```
  - Dependendo da versão do Scala e Spark pode ter funções a mais ou a menos
- Exemplo:
  - $ kafka-topics -version (para ver a versão do kafka, é para ser executado no container de kafka)
  - 2.3.0 (saída)
    - Link (para ver as versões de outras ferramentas compatíveis com a versão do kafka): https://docs.confluent.io/current/installation/versions-interoperability.html
    - Confluent Platform: 5.3x (Ex: 5.3.1-css) - quando aparece isso é porque está utilizando no cluster o kafka pela confluent e não o kafka puro
    - Apache Kafka: 2.3x
  - $ spark-shell (é para ser executado no container de spark)
    - Scala: 2.11.12
    - Spark: 2.4.1
- $ spark-shell --packages org.apache.spark:spark-streaming-kafka-0-10_2.11:2.4.1
- IntelliJ com SBT:
- libraryDependencies += "org.apache.spark" % "spark-streaming-kafka-0-10" % "2.4.1"
  - também precisa adicionar o pacote usando -- spark-submit

# Scala - Estrutura do código e importações
## Spark Streaming com Kafka
- Processos básicos para leitura de dados do Kafka
```scala
scala> import...
scala> val ssc = new StreamingContext(...)
scala> val kafkaParams = Map(String, Object)(...)//chave, valor (String, Object)
scala> val dstream = KafkaUtils.createDirectStream[String, String](...)//chave, valor (String, String)
scala> dstream.map(...) //dstream é um dataframe, então é possível utilizar as operações do RDD nele
ssc.start()
```
- Como Stream nunca para, não é interessante usar muito desempenho do cluster
## Importar packages
- Packages do Kafka 010 e Streaming

```scala
scala> import org.apache.kafka.clients.consumer.ConsumerRecord
//Para poder gravar os dados do consumidor
scala> import org.apache.kafka.common.serialization.StringDeserializer
scala> import org.apache.spark.streaming.{Seconds, StreamingContext}
scala> import org.apache.spark.streaming.kafka010._
scala> import org.apache.spark.streaming.kafka010.LocationStrategies.PreferConsistent
scala> import org.apache.spark.streaming.kafka010.ConsumerStrategies.Subscribe
```
# Scala - Parâmetros do Kafka

# Scala - Criação e Visualização do DStream
