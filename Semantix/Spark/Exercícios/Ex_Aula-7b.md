### 1. Criar o diretório no hdfs “/user/rodrigo/stream”

!hdfs dfs -mkdir "/user/marina/stream"

### 2. Criar uma aplicação para contar palavras a cada 10 segundos da porta 9998 e exibir no console durante 50 segundos

```python
from pyspark.streaming import StreamingContext
from time import sleep

```

### 3. Criar uma aplicação para contar palavras a cada 10 segundos da porta 9998 e salvar os dados no namenode no diretório “hdfs://namenode/user/rodrigo/stream/word_count” durante 50 segundos
