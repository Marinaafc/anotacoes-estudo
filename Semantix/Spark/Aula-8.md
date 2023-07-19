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
   
- Receber dados de um ou mais tópicos do Kafka
  - Configurar os parâmetros do StreamContext
  - Configurar os parâmetros do Kafka
  - Configurar o DStreams para leitura dos tópicos


# Revisão de comandos do Kafka

# Dependências do Spark Streaming com Kafka 0.10

# Scala - Estrutura do código e importações

# Scala - Parâmetros do Kafka

# Scala - Criação e Visualização do DStream
