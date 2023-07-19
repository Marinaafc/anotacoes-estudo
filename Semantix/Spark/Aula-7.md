# Spark Streaming - Conceitos
## Ferramentas
- Spark
  - ETL e processamento em batch
- Spark SQL
  - Consultas em dados estruturados
- Spark Streaming
  - Processamento de stream
- Spark MLib
  - Machine Learning
- Spark GraphX
  - Processamento de grafos
 
![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/29a9023b-41b8-4a2d-9da8-161deea2fbcf)
- Apache Spark - Core do Spark, onde está o RDD, processamento em batch

## Spark Streaming
- Abstração de alto nível (do Core do Spark)
  - Por baixo do Spark Streaming se está trabalhando com RDD
  - Dstreams (Discretized Streaming)
  - Representa um Stream contínuo de dados
> - O fluxo de dados em batch começa e termina (lê o dado, processa o dado e finaliza o dado)
> - A ideia do stream é ler o dado, processar o dado, salvar, ler o dado, processar o dado, salvar...
>  - Ele vai gerando uma saída, mas não tem um fim
>  - É contínuo com o tempo, a não ser que em determinado momento ele realmente precise parar, mas muitos processos stream não param
- Extensão da API core do Spark (Tem todas as características do RDD)
  - Processamento escalonável
  - Alta taxa de transferência
  - Tolerante a falhas de stream de dados (caso algum stream venha a cair)
 
![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/046c56f6-6ec0-4652-894e-c24a946efbd2)
## Spark Streaming - Engine
- Recebe fluxos de dados de entrada e divide os dados em lotes (micro batches)
  - Processados pela engine do Spark para gerar o stream final de resultados em batch
- DStream é representado como uma sequência de RDDs
- Trata os dados em stream no formato de micro batches, então vai fazer processamento em pequenas partes do dado sem parar
- Vai entrar informação sempre, essa informação vai ser coletada de tempo em tempo, que pode ser definido, consegue também definir a janela em que vai ser feito o processamento e o este vai sendo salvo
- Se definir o tempo como 1 segundo, de 1 em 1 segundo vai ter um novo arquivo
![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/e1400723-162a-401e-9363-cc3f39531d4d)

# Spark Streaming - Leitura de Dados
## Dstream - Leitura Básica
- O Spark Stream é um outro formato de dados, diferentemente do DataFrame e do Dataset;
- Por conta disso, será preciso criar um novo contexto chamado StreamingContext;
- sc = Spark Context; ssc = Spark Streaming Context

### Scala
- Criar um contexto com intervalo de 2 segundos

```scala
//IDE: import org.apache.spark._
import org.apache.spark.streaming._
//IDE: val conf = new SparkConf().setMaster("local")
//IDE: val sc = new SparkContext(conf)
val ssc = new StreamingContext(sc, Seconds(2))
//parâmentros do Streaming Context:
//sc - contexto do Spark atual
//Seconds() - de quantos em quantos segundos vai ter esse contexto (pegar os dados para fazer o processamento)
```
- Criar um Dstream para captura dos dados relativos a sessão da porta 9999 *(Dizer o que vai fazer, se vai ler os dados do Kafka ou de uma porta, por exemplo)*

```scala
val dstr = ssc.socketTextStream("localhost", 9999)
//vai ser em formato de texto um socket que precisa ser definido (no caso, é a porta 9999)
```
### Python 
- Criar um contexto com intervalo de 2 segundos

```python
#IDE: from pyspark import SparkContext
from pyspark.streaming import StreamingContext
#IDE: conf = SparkConf().setMaster("local")
#IDE: sc = SparkContext.getOrCreate(conf)
ssc = StreamingContext(sc, 2)
```
- Criar um Dstream para captura dos dados relativos a sessão da porta 9999

```python
dstr = ssc.socketTextStream("localhost", 9999)
```
## Dstream - Ex. Leitura e Exibição de uma Porta
- Exemplo de leitura na porta 9999 no localhost

```python
from pyspark.streaming import StreamingContext
ssc = StreamingContext(sc, 2)
readStr = ssc.socketTextStream("localhost", 9999)
readStr.pprint()
ssc.start()
#precisa inicializar o streaming context para mostrar os dados
#nunca vai parar de enviar
```
- Usar Netcat para enviar dados na porta 9999

```python
$ nc -lp 9999
#precisa usar esse comando antes de dar o ssc.start()
#comando precisa ser executado dentro do terminal do container de spark, porque é no localhost
#lp = listen port
#vai pedir para digitar as informações depois de usar o comando
```
# Spark Streaming - Operações
- Vai ter as mesmas operações de um RDD;
- Ação: Retorna um valor
  - Count
  - CountByValue
  - Reduce
  - Print
  - ForeachRDD
 
- Transformação: Retorna um DStream
  - Map
  - Filter
  - FlatMap
  - ReduceByKey

- FlatMap
```python
from pyspark.streaming import StreamingContext
ssc = StreamingContext(sc, 2)
readStr = ssc.socketTextStream("localhost", 9999)
palavras = readStr.flatMap(lambda linha: linha.split(" "))
palavras.saveAsTextFiles("hdfs://localhost/linha")
#não precisa especificar o hdfs porque já está configurado como padrão
#vai criar um arquivo "linha_" e o código hexadecimal da data
#a cada 2 segundos vai criando um novo diretório e esse diretório pode estar particionado
ssc.start()
#só vai salvar no arquivo quando der o start
```
- Transformações de Map

```python
pMinuscula = palavras.map(lambda palavra: palavra.lower())
pMaiuscula = palavras.map(lambda palavra: palavra.upper())
pChaveValor = pMinuscula.map(lambda palavra: (palavra,1))
```
- Filtrar dados

```python
filtro_a = palavras.filter(lambda palavra: palavra.startswith("a"))
filtro_tamanho = palavras.filter(lambda palavra: len(palavra)>5)
num_par = numeros.filter(lambda numero: numero % 2 == 0)
```
# Spark Streaming - Exemplo de Contar Palavras
