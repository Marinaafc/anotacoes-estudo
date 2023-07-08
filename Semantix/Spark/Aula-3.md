# RDD - Conceitos

- Primeiro tipo de formato de dados do Spark;
- A partir dele que surgiu o DataFrame e o DataSet;
- A usabilidade não é tão simples quanto a do DataFrame e do DataSet;

- **R**esilient **D**istributed **D**atasets
  - **R**esiliente - Recriar dado perdido na memória *(usa memória RAM)*
  - **D**istribuído - Processamento no cluster *(modo client: o nó executa o projeto; modo cluster: é executado em outro nó e é feito de forma distribuída)*
  - **D**atasets - Dados podem ser criados ou vir de fontes
 
- Resumidamente, o RDD seria uma coleção de objetos distribuídos entre os nós do cluster
  - Armazena os dados em partições
- Imutáveis
- Tipos de operações
  - Ação
  - Transformação

### RDD - Operações

- Ação: Retorna um valor
  - **Collect** - conta os dados
  - **Count** - conta os registros
  - **First** - retorna o primeiro
  - **Take** - retorna os valores escolhidos
  - **Reduce** - redução dos dados
  - **CountByKey** - contar pela chave
  - **Foreach** - escolher o que fazer para cada elemento
 
- Transformação: Retorna um RDD
  - Map
  - Filter
  - FlatMap
  - GroupByKey
  - ReduceByKey
  - AggregateByKey
 
# Leitura e Visualização de Dados (Ações)
### Exemplo 2 arquivos:

entrada1.txt | entrada2.txt
------------ | ------------
Big Data     | Curso Hadoop
2019         | Data
Semantix     | Hadoop
Hadoop       | Semantix
Semantix SP  | SP
Hadoop 2019  | Data

```scala
rdd = sc.textFile("entrada*")
```
```rdd: org.apache.spark.rdd.RDD[String]``` *(resposta do SparkShell)*  
- Quando se usa o sc (SparkContext), provavelmente está se aplicando alguma função de RDD, mas tem outras coisas no sc além do RDD;
- Explicação da resposta: ele vai no org.apache.spark.rdd e cria um objeto RDD que salva um array de String [String]
```scala
rdd.count()
```
```res2: Long = 12``` *(resposta do SparkShell)*  
```scala
rdd.first()
```
```res3: String = Big Data``` *(resposta do SparkShell)*  
```python
# Não necessariamente irá mostrar os 5 primeiros. São os 5 primeiros na posição da memória
rdd.take(5)
```
```res1: Array[String] = Array(Big Data, 2019, Semantix, Hadoop, Semantix SP)``` *(resposta do SparkShell)*  
```python
# Mostrar dados - Cuidado para não estourar memória
rdd.collect()
```
```python
# Mostrar os valores sem Array em Scala
rdd.foreach(println)
```
- No Jupyter com Python, já mostra em um visual mais agradável. Em Scala vai mostrar tudo em Array, por isso precisa usar o foreach para um visual mais agradável

# Transformações de Map e FlatMap
### Scala
```scala
rdd.take(2)
//res1: Array[String] = Array(Big Data, Semantix SP)
```
```scala
val palavras = rdd.flatMap(x => x.split(" "))
//OperadorCoringa
val palavras = rdd.flatMap(_.split(" "))
```
- Como em transformação é retornado um RDD, então este precisa ser salvo em algum lugar. Por isso é criada uma variável para armazená-lo;
- Para cada elemento vai fazer alguma alteração (flatMap). No exemplo, para cada elemento com espaço, haverá separação;
- (x => x.split(" ")) - função anônima. x é o argumento, para cada argumento que passar vai separar por espaço.
- "_" - "para cada elemento"
```scala
//Entrada
palavras.foreach(println)
```
//Saída
```
'Big',
'Data',
'Semantix',
'SP'
```
### Python
```python
rdd.take(2)
# ['Big Data', 'Semantix SP']
```
```python
palavras = rdd.flatMap(lambda x: x.split(" "))
```
- O lambda é a mesma coisa da função anônima.
```python
# Entrada
palavras.collect()
```
```python
# Saída
['Big',
'Data',
'Semantix',
'SP']
```
## Função Anônima
### Python
- Lambda é utilizado para usar uma função dentro de outra função
```python
rdd.take(2)
# ['Big Data', 'Semantix SP']
palavras = rdd.flatMap(lambda x: x.split(" "))
minuscula = palavras.map(lambda linha: linha.lower())
minuscula.collect()
# ['big',
#  'data',
#  'semantix',
#  'sp']
```
- Lambda é a mesma coisa que declarar uma função. O ":" seria o "return" como no exemplo abaixo:
```python
def Func(linha):
  return linha.lower()

minuscula = palavras.map(Func)

minuscula.collect()
```
