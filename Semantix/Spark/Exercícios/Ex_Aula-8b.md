## Kafka

### 1. Preparação do ambiente no Kafka

a) Criar o tópico “topic-kvspark” com 2 partições e o fator de replicação = 1

b) Inserir as seguintes mensagens no tópico (Chave, Valor):

o 1, Msg1

o 2, Msg2

o 3, Msg3

c) Criar um consumidor no Kafka para ler o “topic-kvspark”

## Spark

### 1. Criar um consumidor em Scala usando Spark Streaming para ler o “topic-kvspark” no cluster Kafka ”kafka:9092”

### 2. Visualizar o tópico com as seguintes informações

Nome do tópico
Partição
Chave
Valor
### 3. Salvar o tópico no diretório hdfs://namenode:8020/user/<nome>/kafka/dstreamkv
