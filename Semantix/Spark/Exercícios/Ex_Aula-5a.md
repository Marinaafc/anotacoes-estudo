# Exercícios - WithColumn 1/2
### 1. Criar um dataframe para ler o arquivo no HDFS /user/<nome/data/juros_selic/juros_selic
```python
!hdfs dfs -cat /user/marina/data/juros_selic/juros_selic
juros = spark.read.csv("/user/marina/data/juros_selic/juros_selic", header=True, sep=";")
juros.show()
```
### 2. Alterar o formato do campo data para “MM/dd/yyyy”
```python
from pyspark.sql.functions import unix_timestamp, from_unixtime, col, substring, split
juros_americano = juros.withColumn("data", from_unixtime(unix_timestamp(col("data"), "dd/MM/yyyy"),"MM/dd/yyyy"))
juros_americano.show(5)
```
### 3. Com uso da função from_unixtime crie o campo “ano_unix”, com a informação do ano do campo data
```python
juros_unixtime = juros_americano.withColumn("ano_unix", from_unixtime(unix_timestamp(col("data"), "dd/MM/yyyy"),"yyyy"))
juros_unixtime.show(5)
```
### 4. Com uso de substring crie o campo “ano_str”, com a informação do ano do campo data
```python
juros_substring = juros_unixtime.withColumn("ano_str", substring(col("data"), 7, 4))
juros_substring.show(5)
```
### 5. Com uso da função split crie o campo “ano_str”, com a informação do ano do campo data
```python
juros_split = juros_substring.withColumn("ano_split", split(col("data"), "/").getItem(2))
juros_split.show(5)
```
### 6. Salvar no hdfs /user/rodrigo/juros_selic_americano no formato CSV, incluindo o cabeçalho
```python
juros_split.write.csv("/user/marina/juros_selic_americano", header=True)
```
