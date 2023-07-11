# Opções de Leitura e Escrita em CSV 
- Pode ser usado para Dataset e DataFrame
```python
data = spark.read.option("sep","|").option("header","true").option("quote","\"").option("mode","DROPMALFORMED").csv("hdfs:///user/teste/")
#sep ou delimiter = separador
#quote = aspas - caracter que está separando os textos (\" padrão)
#DROPMALFORMED - tudo que não estiver formatado de acordo com a leitura vai ser ignorado
```
- Por padrão, todo arquivo CSV vem com aspas, mas às vezes essas aspas vêm com um caractere diferente (ex: aspas simples: ')
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

# Comandos com WithColumn
- WithColumn é muito utilizado em limpeza de dados, pois com ele é possível criar colunas e com essas colunas é possível editar muita coisa;
- Serve parar Dataset e DataFrame;
- WithColumn
  - Criar Colunas
  - Sintaxe: <dataframe>.withColumn("<nomeColuna>", <Coluna>)
```python
data = spark.read.option("sep","|").option("header","true").option("quote","\").option("mode","DROPMALFORMED").csv("hdfs:///user/teste/")
addColumn = data.withColumn("Novo Campo", col("id"))
```
