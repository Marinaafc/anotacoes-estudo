### 1. Criar o diretório no hdfs “/user/rodrigo/stream”
```python
!hdfs dfs -mkdir "/user/marina/stream"
```
### 2. Criar uma aplicação para contar palavras a cada 10 segundos da porta 9998 e exibir no console durante 50 segundos

```python
from pyspark.streaming import StreamingContext
from time import sleep
ssc = StreamingContext(sc, 10)
dstream = ssc.socketTextStream("localhost", 9998)
palavras = readStr.flatMap(lambda linha: linha.split(" "))
pMinuscula = palavras.map(lambda palavra: palavra.lower())
pChaveValor = pMinuscula.map(lambda palavra: (palavra,1))
pReduce = pChaveValor.reduceByKey(lambda key1, key2: key1 + key2)
pReduce.pprint()
#docker exec -it jupyter-spark bash
#nc -lp 9998
ssc.start()
sleep(50)
ssc.stop()
```

### 3. Criar uma aplicação para contar palavras a cada 10 segundos da porta 9998 e salvar os dados no namenode no diretório “hdfs://namenode/user/rodrigo/stream/word_count” durante 50 segundos

```python
from pyspark.streaming import StreamingContext
from time import sleep
ssc = StreamingContext(sc, 10)
dstream = ssc.socketTextStream("localhost", 9998)
palavras = dstream.flatMap(lambda linha: linha.split(" "))
pMinuscula = palavras.map(lambda palavra: palavra.lower())
pChaveValor = pMinuscula.map(lambda palavra: (palavra,1))
pReduce = pChaveValor.reduceByKey(lambda key1, key2: key1 + key2)
pReduce.saveAsTextFile("hdfs://namenode/user/marina/stream/word_count")
#docker exec -it jupyter-spark bash
#nc -lp 9998
ssc.start()
sleep(50)
ssc.stop()
```
