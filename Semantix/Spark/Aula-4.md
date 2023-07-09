# DataFrame - Criação de Schemas
## Tipagem de Dados

- Spark SQL e DataFrame
  - ByteType
  - ShortType
  - IntegerType
  - LongType
  - FloatType
  - DoubleType
  - DecimalType
  - BigDecimal
  - StringType
  - BinaryType
  - BooleanType
  - TimestampType
  - DateType
  - ArrayType
  - MapType
  - StructType
 
- Se der algum erro com o Type, pode ser porque teve alguma mudança na versão.

## Esquemas - Definição
### Scala
- Inferir esquema manualmente em dados com cabeçalho;
- Automaticamente, sempre vai colocar o tipo de dado para cima (Ex: nº: no lugar de colocar integer vai colocar long; float coloca como double);
- Arquivo:

| setor.csv |
| ------ |
| id, setor |
| 1, vendas |
| 2, TI |
| 3, RH |
```scala
scala> import org.apache.spark.sql.types._
//O "_" pega todos os tipos de dados

scala> val columnsList = List(StructField("id", IntegerType), StructField("setor", StringType))
//É uma lista de campos e não uma estrutura

scala> val setorSchema = StructType(columnsList)
//Aqui é uma estrutura "StrucyType"

scala> val setorDF = spark.read.option("header","true").schema(setorSchema).csv("setor.csv")
//No schema, precisa passar um tipo de estrutura
//"setorDF" é um dataFrame
```
- Lista, fila, etc são subclasses do seq;
- Pode criar o cabeçalho no StructField também, se ele não existia;
- O Schema é um StructType e dentro desse StructType tem uma lista de StructField

### Python
- Inferir esquema manualmente em dados com cabeçalho
```python
from pyspark.sql.types import *

columns_list = [StructField("id", IntegerType()), StructField("setor", StringType())]

setor_schema = StructType(columns_list)

setor_df = spark.read.option("header","true").schema(setor_schema).csv("setor.csv")
```
- Em Python é recomendado colocar os options dentro do arquivo CSV. Na aula prática, é explicado com detalhes.

# DataFrame - Testar Schemas
