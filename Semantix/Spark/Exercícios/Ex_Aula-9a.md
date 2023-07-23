1. Criar uma aplicação em scala usando o spark para ler os dados da porta 9999 e exibir no console

```python
porta_leitura = spark.readStream.format("socket").option("host", "localhost").option("port", 9999).load()
#o host é o jupyter-spark
#readStream é uma leitura que não para, diferentemente do read comum
nc -lp 9999
porta_saida = porta_leitura.writeStream.format("console").start()
```

2. Ler os arquivos csv “hdfs://namenode:8020/user/<nome>/data/iris/*.data” em modo streaming com o seguinte schema:

- sepal_length float
- sepal_width float
- petal_length float
- petal_width float
- class string

```python
from pyspark.sql.types import StructType
iris_schema = StructType().add("sepal_length", "float").add("sepal_width", "float").add("petal_length","float").add("petal_width","float").add("class","string")
iris = spark.readStream.schema(iris_schema).csv("/user/marina/data/iris/*.data")
#se der tudo certo em batch (usando só "read"), também vai dar certo com readStream
```


3. Visualizar o schema das informações
```python
iris.printSchema()
```
4. Salvar os dados no diretório “hdfs://namenode:8020/user/<nome>/stream_iris/path” e o checkpoint em “hdfs://namenode:8020/user/<nome>/stream_iris/check”
```python
iris_saida = iris.writeStream.format("csv").option("checkpointLocation", "/user/marina/stream_iris/check").option("path", "/user/marina/stream_iris/path").start()
#dependendo do formato que estiver trabalhando pode precisar o checkpoint ou não
```
5. Verificar a saida no hdfs e entender como os dados foram salvos
```scala
!hdfs dfs -ls /user/marina/stream_iris/path
```
6. Bônus: Contar as palavras do exercício 1. (ele não resolveu na aula, tentar depois)
```python
!hdfs dfs -ls /user/marina/data/iris/*.data
iris_batch = spark.read.csv("/user/marina/data/iris/bezdekIris.data")
iris_batch.count()
iris_batch = spark.read.csv("/user/marina/data/iris/iris.data")
iris_batch.count()
spark.read.csv("/user/marina/stream_iris/path/part-00000-5bbc76e9-d576-4443-88c1-1025439caab4-c000.csv").count()
spark.read.csv("/user/marina/stream_iris/path/part-00001-7247296c-b20e-46d6-bab6-bfb0cc47774f-c000.csv").count()
```
- Para parar o iris_saida, precisa clicar em "Kernel" e depois em "Restart & Clear Outout" ou desligar o cluster;
- Para verificar se funcionou, basta acessar o localhost:4040 e ver se tem jobs ativos ou ver pelo iris_saida.status;
- Pararia também se usasse o sleep
