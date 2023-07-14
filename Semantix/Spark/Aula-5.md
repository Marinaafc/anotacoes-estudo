# Opções de Leitura e Escrita em CSV 
- Pode ser usado para Dataset e DataFrame
```python
data = spark.read.option("sep","|").option("header","true").option("quote","\"").option("mode","DROPMALFORMED").csv("hdfs:///user/teste/")
#sep ou delimiter = separador
#quote = aspas - caracter que está separando os textos (\" padrão)
#DROPMALFORMED - tudo que não estiver formatado de acordo com a leitura vai ser ignorado
```
- Por padrão, todo arquivo CSV vem com aspas, mas às vezes essas aspas vêm com um caractere diferente (ex: aspas simples: \')
- Em Python não é recomendado usar tantos "option" e sim colocar as especificações dentro do CSV
## Criação de DataFrame para ser usado como exemplo
```python
from pyspark.sql import Row
Name = Row("cod", "nome")
data = [Name(1, "Rodrigo"), Name(2, "Reboucas")]
#o que estiver sem aspas vai ser entendido como um long
data_frame = spark.createDataFrame(data)
#para salvar:
data_frame.write.csv("/user/marina/teste_csv", mode="overwrite", header=true)
#mode - "append" vai anexando arquivo e overwrite vai substituindo o arquivo ao salvar novamente
```
## Leitura de dados
- Como só é leitura, não vai salvar o DataFrame em algum lugar
```python
spark.read.csv("/user/marina/teste_csv", header="true").show()
#mesmo salvando com o "header=true", tem que colocar na hora de ler também, senão aparece sem cabeçalho
```
- Se colocar o sep na hora de salvar os dados, tem que colocar na leitura também, senão vai ficar tudo numa única coluna separado pelo sep
- ```ignoreLeadingWhiteSpace="true"``` - vai ignorar os espaços em branco da esquerda (frente da palavra);
- ```ignoreTrailingWhiteSpace="true"```- vai ignorar os espaços em branco da direita (atrás da palavra);
- ```quote="\'"```- vai remover as aspas simples se estiver escrito por exemplo: "'Marina'" (na tabela ficaria 'Marina' e usando o quote removeria);
- escape - remove tudo o que está fora do quote;
- PERMISSIVE - campos em null quer dizer que o dado não está na estrutura definida;
- DROPMALFORMED - vai ignorar os arquivos corrompidos;
- FAILFAST - aplica uma execeção para registros corrompidos

# Função WithColumn - Conceitos (Comandos com WithColumn)
- WithColumn é muito utilizado em limpeza de dados, pois com ele é possível criar colunas e com essas colunas é possível editar muita coisa;
- Serve parar Dataset e DataFrame;
- WithColumn
  - Criar Colunas
  - Sintaxe: ```<dataframe>.withColumn("<nomeColuna>", <Coluna>)```
  - Se o nome da coluna já existe, ela vai ser substituída
  - ```<Coluna>``` - Especificar uma coluna que quer que seja replicada. Também pode-se utilizar funções
  - A forma mais fácil de se editar os dados é adicionando colunas e editando colunas já existentes, por isso cai muito em certificações
```python
data = spark.read.option("sep","|").option("header","true").option("quote","\'").option("mode","DROPMALFORMED").csv("hdfs:///user/teste/")
addColumn = data.withColumn("Novo Campo", col("id"))
#"col("id")" replica a coluna id
#cria uma coluna "novo campo" replicando a coluna id
```
## Acesso a coluna de DataFrame/DataSet
### Scala
- ```col("campo")``` - função
- ```dataframe("campo")``` - outra forma de acessar a coluna, especificando o dataframe
- ```$"campo"``` - outra forma de acessar a coluna (só é válida se estiver dentro de um dataframe e não tem autocomplete), $ - caractere coringa

### Python
- ```from pyspark.sql.functions import col```
- ```col("campo")```
- ```dataframe["campo"]```
- ```dataframe.campo``` - forma mais vantajosa, porque se usar um "." + tab, vai mostrar todas as funções que podem ser utilizadas

```python
dataframe.select(*cols)
#aceita receber string de um campo
dataframe.withColumn(colName, col)
#não aceita string, só type col (tem que usar uma das formas mostradas acima)
```
# Função Timestamp
## WithColumn - Trabalhando com Timestamp
- Apesar de ser possível alterar na leitura do arquivo csv o formatDate, há algumas exceções. Por exemplo, se estiver em um formato de unix_timestamp e quiser converter para dia/mês/ano não é possível fazer isso na leitura;
- Quando se está trabalhando com data e se quer mudar o formato dela, é preciso converter primeiro para um formato universal para timestamp (unix_timestamp) com o formato que "atualmente" estaria a data;
- Converter Coluna para timestamp
  - ```unix_timestamp(col("<ColString>"), "<FormatoAtual"),```
- Alterar formato de coluna de timestamp
  - ```from_unixtime(<ColTimestamp>, <FormatoConversão>))```
 
```python
from pyspark.sql.functions import unix_timestamp, from_unixtime
formato = data.select("data").show(1) #formato = 2020/10/25
convUnix = formato.withColumn("timestamp", unix_timestamp(col("data"), "yyyy/MM/dd"))
convUnixData = convUnix.withColumn("new data", from_unixtime("timestamp", "MM-dd-yyyy"))

convDataDireto = formato.withColumn("mes-dia-ano", from_unixtime(unix_timestamp(col("data"), "yyyy/MM/dd"), "MM-dd-yyyy"))
```
# Função Substring
## WithColumn - Trabalhando com Substring
- Extrair dados de uma coluna de acordo com uma posição
- Sintaxe: ```<dataframe>.withColumn("<nomeColuna>", substring("<Coluna>", <posicaoInicial>, <tamanho>)
- Muito usado com concat para concatenar as colunas e strings

```python
formato = data.select("data").show(1) #data = 2020/10/25
mes = formato.withColumn("mes", substring(col("data"),6,2)) #mes = 10

codsDF = data.select("codigo") #codigo = AA150000CCS
resumoCod = codsDF.withColumn("pedido", concat(substring(col("codigo"), 1, 2), lit("-"), substring(col("codigo"), 9, 3)) #pedido = AA-CCS
```
- Também é possível fazer o exemplo do código com o uso de regex. Porém, regex é custoso, por isso é bom sempre dar preferência a outras formas;
- No entanto, se, por exemplo, a quantidade de dígitos do código não fosse sempre a mesma, teria que usar o Regex;
- Para concatenar um texto tem que usar o literal: lit("");
- Concat pode concatenar qualquer coisa não só literais e substrings, por exemplo: função de timestamp também

# Função Split
