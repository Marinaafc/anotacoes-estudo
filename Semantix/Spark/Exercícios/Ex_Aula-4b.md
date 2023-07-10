### 1. Criar o DataFrame names_us para ler os arquivos no HDFS “/user/<nome>/data/names”
```scala
val names_us = spark.read.csv("/user/marina/data/names")
```
### 2. Visualizar o Schema do names_us
```scala
names_us.printSchema()
```
### 3. Mostras os 5 primeiros registros do names_us
```scala
names_us.show(5)
```
### 4. Criar um case class Nascimento para os dados do names_us
```scala
case class Nascimento(nome: String, sexo: String, quantidade: Int)
```
### 5. Criar o Dataset names_ds para ler os dados do HDFS “/user/<nome>/data/names”, com uso do case class Nascimento
```scala
import org.apache.spark.sql.Encoders
val schema_names = Encoders.product[Nascimento].schema
val names_ds = spark.read.schema(schema_names).csv("/user/marina/data/names").as[Nascimento]
//tem que deixar os dados na mesma estrutura do case class, por isso que se usa o ".schema" antes do ".csv"
```
### 6. Visualizar o Schema do names_ds
```scala
names_ds.printSchema()
```
### 7. Mostras os 5 primeiros registros do names_ds
```scala
names_ds.show(5)
```
### 8. Salvar o Dataset names_ds no hdfs “/user/<nome>/ names_us_parquet” no formato parquet com compressão snappy
```scala
names_ds.write.parquet("/user/marina/names_us_parquet")
//ou
names_ds.write.save("/user/marina/names_us_parquet")
spark.read.parquet("/user/marina/names_us_parquet").printSchema
```
