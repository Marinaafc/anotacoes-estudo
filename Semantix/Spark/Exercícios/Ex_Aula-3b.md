### 1. Ler com RDD os arquivos localmente do diretório “/opt/spark/logs/” ("file:///opt/spark/logs/") com 10 partições
```python
log_particoes = sc.textFile("file:///opt/spark/logs/", 10)
log_particoes.getNumPartitions()
```

### 2. Contar a quantidade de cada palavras do RDD em 5 partições
```python
palavras_particoes = log_particoes.flatMap(lambda linha: linha.split(" "),5)
palavras_particoes.getNumPartitions()
palavras_minuscula_particoes = palavras_particoes.map(lambda palavra: palavra.lower)
alavras_count_particoes = palavras_minuscula_particoes.map(lambda palavra: (palavra,1))
palavras_reduce_particoes = palavras_count_particoes.reduceByKey(lambda chave1, chave2: chave1 + chave2,5)
palavras_reduce_particoes.getNumPartitions()
```
### 3. Salvar o RDD no diretório do HDFS /user/<seu-nome>/logs_count_word_5
```python
palavras_reduce_particoes.saveAsTextFile("/user/marina/logs_count_word_5")
!hdfs dfs -ls /user/marina/logs_count_word_5
```
### 4. Refazer a questão 2, com todas as funções na mesma linha de um RDD
```python
palavras_particoes_linha = log_particoes.flatMap(lambda linha: linha.split(" ")).map(lambda palavra: (palavra.lower(),1)).reduceByKey(lambda chave1, chave2: chave1 + chave2,5)
palavras_particoes_linha.count()
```
ou
```python
palavras_particoes_linha = log_particoes.flatMap(lambda linha: linha.split(" ")).map(lambda palavra: palavra.lower()).map(lambda palavra: (palavra,1)).reduceByKey(lambda chave1, chave2: chave1 + chave2,5)
palavras_particoes_linha.count()
```
#### Não é possível alterar as partições no map e flatMap, porque nesses só está mapeando os dados
#### RDD é utilizado para varreção de bits, ou seja, tratar palavra por palavra ou letra por letra
