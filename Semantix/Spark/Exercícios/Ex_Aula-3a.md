### 1. Ler com RDD os arquivos localmente do diretório “/opt/spark/logs/” ("file:///opt/spark/logs/")
```python
!ls /opt/spark/logs
#spark--org.apache.spark.deploy.master.Master-1-jupyter-notebook.out
!cat /opt/spark/logs/spark--org.apache.spark.deploy.master.Master-1-jupyter-notebook.out
```
- é um arquivo de texto com logs do spark
```python
log = sc.textFile("file:///opt/spark/logs/")
```
### 2. Com uso de RDD faça as seguintes operações
a) Contar a quantidade de linhas
```python
log.count()
```
b) Visualizar a primeira linha
```python
log.first()
```
c) Visualizar todas as linhas
```python
log.collect()
```
d) Contar a quantidade de palavras
```python
palavras = log.flatMap(lambda linha: linha.split(" "))
palavras.count()
```
e) Converter todas as palavras em minúsculas
```python
minuscula = palavras.map(lambda palavra: palavra.lower())
minuscula.take(5)
```
f) Remover as palavras de tamanho menor que 2
```python
minuscula_maior2 = minuscula.filter(lambda palavra: len(palavra) > 2)
minuscula_maior2.count()
```
g) Atribuir o valor de 1 para cada palavra
```python
palavra_1 = minuscula_maior2.map(lambda palavra: (palavra,1))
palavra_1.take(3)
```
h) Contar as palavras com o mesmo nome
```python
palavra_reduce = palavra_1.reduceByKey(lambda key1, key2: key1 + key2)
palavra_reduce.take(5)
```
i) Visualizar em ordem alfabética
palavra_ordena = palavra_reduce.sortBy(lambda palavra: palavra[0], True)
palavra_ordena.take(7)
j) Visualizar em ordem decrescente a quantidade de palavras
palavra_ordena = palavra_reduce.sortBy(lambda palavra: -palavra[1])
palavra_ordena.take(7)
k) Remover as palavras, com a quantidade de palavras > 1
filtro_palavras2 = minuscula.filter(lambda palavra: len(palavra) <= 1)
filtro_palavras2.take(10)
l) Salvar o RDD no diretorio do HDFS /user/<seu-nome>/logs_count_word
palavra_ordena.saveAsTextFile("/user/marina/logs_count_word")

#### OBS: Se colocar 2 comandos como log.count() e log.first() numa mesma célula, só vai executar o último, exceto se for colocado um println() no primeiro [ex: println(log.count())]
