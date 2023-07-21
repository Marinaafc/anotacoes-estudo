## Kafka

### 1. Preparação do ambiente no Kafka

a) Criar o tópico “topic-spark” com 1 partição e o fator de replicação = 1

b) Inserir as seguintes mensagens no tópico:

o Msg1  

o Msg2  

o Msg3

c) Criar um consumidor no Kafka para ler o “topic-spark”

## Spark

### 1. Criar um consumidor em Scala usando Spark Streaming para ler o “topic-spark” no cluster Kafka ”kafka:9092”

### 2. Visualizar o tópico com as seguintes informações

Nome do tópico  
Partição  
Valor
### 3. Salvar o tópico no diretório hdfs://namenode:8020/user/<nome>/kafka/dstream
