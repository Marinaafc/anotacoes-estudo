### 1. Configurar o jar do spark para aceitar o formato parquet em tabelas Hive

- curl -O https://repo1.maven.org/maven2/com/twitter/parquet-hadoop-bundle/1.6.0/parquet-hadoop-bundle-1.6.0.jar
- docker cp parquet-hadoop-bundle-1.6.0.jar jupyter-spark:/opt/spark/jars
### 2. Baixar os dados dos exercícios do treinamento no diretório spark/input (volume no Namenode)

- cd input
- sudo git clone https://github.com/rodrigo-reboucas/exercises-data.git .
### 3. Verificar o envio dos dados para o namenode

### 4. Criar no hdfs a seguinte estrutura: /user/rodrigo/data

### 5. Enviar todos os dados do diretório input para o hdfs em /user/rodrigo/data
