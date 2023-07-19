### 1. Instalar o NetCat no container do spark
*O netcat vai liberar uma porta para ser possível enviar dados via tcp*
- docker exec -it jupyter-spark bash
- apt update
- apt install netcat
como solucionei o erro: https://unix.stackexchange.com/questions/371890/debian-the-repository-does-not-have-a-release-file
### 2. Criar uma aplicação para ler os dados da porta 9999 e exibir no console
```python
#IDE: from pyspark import SparkContext
from pyspark.streaming import StreamingContext
from time import sleep
#IDE: conf = SparkConf().setMaster("local[*]").setAppName("Dstream Python")
#IDE: sc = SparkContext.getOrCreate(conf)
#quando coloca sem o getOrCreate, dá um erro informando que não pode ser criado múltiplos spark context
ssc = StreamingContext(sc, 5)
```
```python
dstream = ssc.socketTextStream("localhost", 9999)
dstream.pprint()
#obrigatoriamente o "nc -lp 9999" no terminal container jupyter-spark precisa ser executado antes do ssc.start()
ssc.start()
sleep(20)
ssc.stop()
```
- Dicas: "Restart & Run All Cells" no Kernel para executar o processo novamente
