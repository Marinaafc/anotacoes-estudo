# Variáveis Compartilhadas
- Sempre quando uma função é passada para o Spark, a operação é executada em um nó de cluster remoto
  - Trabalha em cópias separadas de todas as variáveis ​usadas na função
    - Vai ter uma cópia para tudo em cada nó
  - As variáveis não se comunicam, ​são copiadas para cada máquina e nenhuma atualização nas variáveis ​na máquina remota é propagada de volta ao programa do driver
  - A leitura e gravação entre tarefas é ineficiente
 
- Uma forma de melhorar isso é trabalhar com varáveis compartilhadas, não vai precisar manter as cópias em todas as tarefas
- Pode escolher o que quer que carregue em cache, porque é necessário e o resto não
- O Spark fornece dois tipos limitados de variáveis ​compartilhadas
  - Broadcast
  - Accumulators
 
## Broadcast
- Para cada máquina no cluster terá somente uma variável para leitura em cache
  - Não é necessário enviar uma cópia dela para as tarefas
  - As várias tarefas do executor utilizam a mesma variável
- Variáveis ​de broadcast é útil quando
  - Tarefas em vários estágios precisam dos mesmos dados
  - Importância de armazenar em cache os dados na forma desserializada
![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/6a5dde1d-6b41-4cee-b999-caa826671c6a)
 
## Broadcast - Métodos
- .Id - Identificador único
- .Value – Valor
- .Unpersist - Exclui assincronamente cópias em cache da variável broadcast nos executores
- .Destroy - Destrói todos os dados e metadados relacionados a variável de broadcast
- .toString - Representação de string

## Broadcast - Exemplo
- Sintaxe
- ```<variavelBroadcast> = sc.broadcast(<valor>)```
```python
broadcastVar = sc.broadcast([1, 2, 3])
type(broadcastVar)
#pyspark.broadcast.Broadcast
broadcastVar.value
#[1, 2, 3]
broadcastVar.destroy
```
## Accumulators
- Acumuladores são variáveis ​que são apenas “adicionadas” a uma operação associativa e comutativa
- Em Scala e Java dá para definir um tipo e um nome pro acumulador
- Em Python não é possível definir um nome
- Se não definir um nome, não é possível visualizar na interface gráfica (localhost:4040)
  - Paralelismo eficiente
  - Podem ser usados ​para implementar contadores
  - Suporta acumuladores de tipos numéricos, e podem adicionar outros
- O spark exibe o valor para cada acumulador modificado por uma tarefa na tabela “Tasks“
- O rastreamento de acumuladores na interface do usuário pode ser útil para entender o progresso dos estágios em execução
![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/c2978cf6-8aae-413a-aefe-11b468a537cd)

## Accumulators - Exemplo
- Criar o acumulador
  - ```sc. long/doubleAccumulator(valor, “<nomeAcumulador>”)``` – Scala e view Spark’s UI
  - ```sc. Accumulator(valor)``` - Python
- Adicionar tarefas
  - ```<aculumator.add(Long/Double)```
 
```scala
scala> val accum = sc.longAccumulator(0, "My Accumulator")
scala> sc.parallelize(Array(1, 2, 3, 4)).foreach(x => accum.add(x))
scala> accum.value //10
```
```python
python accum = sc. Accumulator(0)
python> sc.parallelize([1, 2, 3, 4]).foreach(lambda x: accum.add(x))
python> accum.value #10
```
## Cache de tabelas
- Armazenar tabela em cache na memória
  - spark.catalog.cacheTable("tableName")
  - dataFrame.cache()
- Remover tabela da memória
  - spark.catalog.uncacheTable("tableName")
```
spark.catalog.cacheTable("src")
broadcast(spark.table("src")).join(spark.table("records"), "key").show()
spark.catalog.uncacheTable("src")
```
# UDF
- User defined Function para Spark SQL
  - Registrar função como UDF
  - Comando:
    - ```spark.udf.register(“<nomeUDF>”, <UserDefinedFunction>)```
   
 
```scala
scala> val quadrado = ((s: Long) => s * s)
```
```python
python> quadrado = (lambda s: s * s)
```
```
spark.udf.register(“fQuad", quadrado)
spark.range(1, 20).registerTempTable("test")
spark.sql("select id, fQuad(id) as id_quad from test")
```
## UDF
- User defined Function para DataFrames
  - Comandos:
    - ```<nomeUDF> = udf(<UserDefinedFunction>)```
```scala
scala> 
import org.apache.spark.sql.functions.{col, udf}
scala> val fDfQuad = udf((s: Long) => s * s)
```
```python
python> 
from pyspark.sql.functions import col,udf
def quadrado (s):
return s * s
fDfQuad = udf(lambda s: quadrado(s))
```
```
spark.range(1, 20).select(col(“id”), fDfQuad(col("id")))
```
## UDF
- Realmente é necessário criar?
  - Otimização
  - Desempenho
  - Documentação
    - https://spark.apache.org/docs/latest/api/sql/ 
# Tunning
## Deploy com alocação dinâmica
- Parâmetros para utilizar os recursos do cluster
  - --master $YARN
    - Executar o log local: $YARN = local
    - Executar no cluster: $YARN = yarn
  - --deploy-mode cluster
  - --conf "spark.dynamic.Allocation.enable=true"
  - --conf "spark.shuffe.service.enable=true"
  - --conf "spark.shuffe.service.port=7337"
  - --conf "spark.dynamic.InitialExecutors=6"
  - --conf "spark.dynamic.maxExecutors=9"
  - --conf "spark.dynamic.minExecutors=3"
## Deploy com alocação calculada
- Considerar toda a capacidade da infra/fila
- Parâmetros para utilizar os recursos do cluster
  - --master $YARN
    - Executar o log local: $YARN = local
    - Executar no cluster: $YARN = yarn
  - --deploy-mode cluster
  - --dirver-memory = 8G (recomendável)
  - --executor-memory = int( (Memória total-10%) / num-executors)
  - --conf spark.yarn.driver.memoryOverhead = 10% de memoria
  - --conf spark.yarn.executor.memoryOverhead = 10% dos executors
  - --executors-core = 4 ou 5 no máximo, (mais que isso fica pesado)
  - --num-executors = int ( (Cores total-10%) / executors-core)
# Spark Connector
- Jdbc
  - https://spark.apache.org/docs/latest/sql-data-sources-jdbc.html
- MongoDB
  - https://docs.mongodb.com/spark-connector/current/
- Redis
  - https://github.com/RedisLabs/spark-redis
- Kafka
  - https://spark.apache.org/docs/latest/structured-streaming-kafka-integration.html
- Elastic
  - https://www.elastic.co/guide/en/elasticsearch/hadoop/current/spark.html
