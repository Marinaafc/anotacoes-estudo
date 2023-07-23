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
user_schema = StructType().add("sepal_length", "float").add("sepal_width", "float").add("petal_length","float").add("petal_width","float").add("class","string")
read_csv_df = spark.readStream.schema(user_schema).csv("/user/marina/data/iris/*.data")
```

3. Visualizar o schema das informações
```python
read_csv_df.printSchema()
```
4. Salvar os dados no diretório “hdfs://namenode:8020/user/<nome>/stream_iris/path” e o checkpoint em “hdfs://namenode:8020/user/<nome>/stream_iris/check”
```python
read_csv_df.writeStream.format("csv").option("checkpointLocation", "/user/marina/stream_iris/check").option("path", "/user/marina/stream_iris/path").start()
```
5. Verificar a saida no hdfs e entender como os dados foram salvos
```scala
hdfs dfs -ls /user/marina/stream_iris/path
```
6. Bônus: Contar as palavras do exercício 1.
```python
val read_str = spark.readStream.format("socket").option("host", "localhost").option("port", 9999).load()

nc...
val write_str = readStr.writeStream.format("console").start()
```
