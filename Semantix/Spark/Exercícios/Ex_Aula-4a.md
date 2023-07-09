### 1. Criar o DataFrame names_us_sem_schema para ler os arquivos no HDFS “/user/<nome>/data/names”
```python
!hdfs dfs -ls /user/marina/data/names
!hdfs dfs -cat /user/marina/data/names/yob1880.txt
names_us_sem_schema = spark.read.csv("/user/marina/data/names")
#Como os dados do arquivo estão separados por vírgula, foi escolhida a leitura em CSV
#Ler o arquivo como texto irá tratar todos os dados como string
```
### 2. Visualizar o Schema e os 5 primeiros registos do names_us_sem_schema
```python
names_us_sem_schema.printSchema()
names_us_sem_schema.show(5)
```
### 3. Criar o DataFrame names_us para ler os arquivos no HDFS “/user/<nome>/data/names” com o seguinte schema:

- nome: String
- sexo: String
- quantidade: Inteiro
```python
from pyspark.sql.types import *
estrutura_lista = [StructField("nome", StringType()), StructField("sexo", StringType()), StructField("quantidade", IntegerType())]
names_schema = StructType(estrutura_lista)
names_us = spark.read.csv("/user/marina/data/names", schema=names_schema)
```
### 4. Visualizar o Schema e os 5 primeiros registos do names_us
```python
print(names_us.printSchema())
names_us.show(5)
#Se colocar os 2 na mesma célula, tem que colocar o "print()" no de cima para mostrar o output do de cima
```
### 5. Salvar o DataFrame names_us no formato orc no hdfs “/user/<nome>/names_us_orc”
```python
names_us.write.orc("/user/marina/names_us_orc")
!hdfs dfs -ls /user/marina/names_us_orc
```
