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
### Python
```python
from pyspark.sql.types import *

columns_list = [StructField("id", IntegerType()), StructField("setor", StringType())]

setor_schema = StructType(columns_list)

dados_teste = [Row(1, "vendas"), Row(2, "TI"), Row(3, "RH")]

setor_df = spark.createDataFrame(data = dados_teste, schema = setor_schema)
#Não é obrigado escrever o "data"
```
- Row class
```python
from pyspark.sql import Row

Schema = Row("id","setor")
dados_teste = [Schema(1, "vendas"), Schema(2, "TI"), Schema(3, "RH")]

teste_df = spark.createDataFrame(data = dados_teste)

teste_df.printSchema()
```
# Dataset - Conceitos
- Coleção distribuída de objetos de tipagem forte
  - Tipos primitivos: Int ou String
  - Tipos Complexos: Array ou listas
  - Product objects (objeto criado pelo dataset)
    - Scala - case classes (usado para criar a estrutura do objeto)
    - Java - JavaBen objects
 - Row objects
- Mapeado para um schema relacional
  - Schema é definido por um encoder
  - Schema mapea objeto de propriedades para tipos de colunas
- Otimizado pelo Catalyst (tem melhor performance do que RDD e Dataframe)
  - Otimiza melhor as operações para determinados tipos, pois com o encoder o já fica tipado na leitura dos dados
- Implementado apenas em Java e Scala
  - Não existe Dataset em Python, pois ele é utilizado em linguagens fortemente tipadas
 
## DataFrame x Dataset
- DataFrames (Conjuntos de dados de Row objects _objeto de linha_)
  - Representam dados tabulares (tabela)
  - Transformações são não tipadas
    - Linhas podem conter elementos de qualquer tipo
    - Schemas definidos não são aplicados aos tipos de coluna até o tempo de execução
    - O Schema só é definido nas ações, nas transformações não há importância com o tipo
    - Então se tivesse um erro em relação ao tipo de objeto usado, ele não seria encontrado na transformação, somente na ação
    - Como não especifica o tipo, consome mais memória
- Datasets
  - Representam dados tipados e orientados a objeto
  - Transformações são tipadas (algumas transformações podem variar entre tipada ou não tipada)
    - Propriedades do objeto são tipadas em tempo de compilação (ganha performance)
- Representação dos dados
  - RDD - 2011
  - Dataframe - 2013
  - Dataset - 2015
