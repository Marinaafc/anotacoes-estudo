### 1. Configurar o jar do spark para aceitar o formato parquet em tabelas Hive

- curl -O https://repo1.maven.org/maven2/com/twitter/parquet-hadoop-bundle/1.6.0/parquet-hadoop-bundle-1.6.0.jar (deu erro, fiz manualmente)
- docker cp parquet-hadoop-bundle-1.6.0.jar jupyter-spark:/opt/spark/jars
### 2. Baixar os dados dos exercícios do treinamento no diretório spark/input (volume no Namenode)

- cd input
- sudo git clone https://github.com/rodrigo-reboucas/exercises-data.git . (deu erro, fiz manualmente)
### 3. Verificar o envio dos dados para o namenode
```ls```  
```docker exec -it namenode ls /input```

### 4. Criar no hdfs a seguinte estrutura: /user/rodrigo/data
```docker exec -it namenode bash```  
```hdfs dfs -mkdir -p /user/marina/data``` (-p é usado para criar mais de uma pasta)

### 5. Enviar todos os dados do diretório input para o hdfs em /user/rodrigo/data
```hdfs dfs -put /input/* /user/marina/data```  
```hdfs dfs -ls /user/marina/data```
