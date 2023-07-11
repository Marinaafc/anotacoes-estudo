# Opções de Leitura e Escrita em CSV 
- Pode ser usado para Dataset e DataFrame
```python
data = spark.read.option("sep","|").option("header","true").option("quote","\"").option("mode","DROPMALFORMED").csv("hdfs:///user/teste/")
#sep ou delimiter = separador
#quote = aspas - caracter que está separando os textos (\" padrão)
```
- Por padrão, todo arquivo CSV vem com aspas, mas às vezes essas aspas vêm com um caractere diferente (ex: aspas simples: ')
- Em Python não é recomendado usar tantos "option" e sim colocar as especificações dentro do CSV

