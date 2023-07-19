*Organizou no Pyspark. Separou por exercícios
### 1. Criar o diretório no hdfs “/user/rodrigo/stream”
```python
!hdfs dfs -mkdir /user/marina/stream
```
### 2. Criar uma aplicação para contar palavras a cada 10 segundos da porta 9998 e exibir no console durante 50 segundos

```python
#IDE: from pyspark import SparkContext
from pyspark.streaming import StreamingContext
from time import sleep
#IDE: conf = SparkConf().setMaster("local[*]").setAppName("Dstream WordCount")
#IDE: sc = SparkContext.getOrCreate(conf)
ssc = StreamingContext(sc, 10)
dstream = ssc.socketTextStream("localhost", 9998)
wordcount = dstream.flatMap(lambda linha: linha.split(" ")).map(lambda palavra: palavra.lower()).map(lambda palavra: (palavra,1)).reduceByKey(lambda chave1, chave2: chave1 + chave2)
wordcount.pprint()
#docker exec -it jupyter-spark bash
#nc -lp 9998
ssc.start()
sleep(50)
ssc.stop()
#ir testando repetindo as palavras
#reparar que a cada 10s não vai juntando
```
- Para contornar o fato de que não vai juntando a contagem a cada tempo definido, é possível utilizar uma operação de janela que será mostrada mais na frente. Mas basicamente vai continuar processando no tempo definido, mas criando janelas com o tempo maior que o definido para juntar as operações
- Se faz isso principalmente em kafka lendo os tópicos
- Para copiar no Pyspark, só utilizar o "c" e para colar o "v"
### 3. Criar uma aplicação para contar palavras a cada 10 segundos da porta 9998 e salvar os dados no namenode no diretório “hdfs://namenode/user/rodrigo/stream/word_count” durante 50 segundos

```python
#IDE: from pyspark import SparkContext
from pyspark.streaming import StreamingContext
from time import sleep
ssc = StreamingContext(sc, 10)
dstream = ssc.socketTextStream("localhost", 9998)
wordcount = dstream.flatMap(lambda linha: linha.split(" ")).map(lambda palavra: palavra.lower()).map(lambda palavra: (palavra,1)).reduceByKey(lambda chave1, chave2: chave1 + chave2)
wordcount.saveAsTextFiles("hdfs://namenode/user/marina/stream/word_count")
#docker exec -it jupyter-spark bash
#nc -lp 9998
ssc.start()
sleep(50)
ssc.stop()
#não vai mostrar nada porque não é pptrint, mas ir testando
!hdfs dfs -ls /user/marina/stream
#listar também o diretório e dar o cat em alguma partição
```
- A cada 10s, um arquivo vai ser criado e vários diretórios com o tempo em hexadecimal;
- O arquivo está salvo e distribuído no cluster para se fazer leitura
