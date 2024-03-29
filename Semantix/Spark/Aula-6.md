# Apresentação do módulo de Data Application
- Módulo falará sobre o que fazer no dia a dia de construção de aplicações Big Data;
- Essa fase abordará:
  - O mínimo de conhecimento que o desenvolvedir deve ter para trabalhar com Spark;
  - As entregas que devem ser feitas ao cliente;
  - Os templates de projetos da semantix.
 
# Mínimo de Conhecimento
- 100% dos engenheiros vão precisar lidar com código;
- Código deve ser feito para ser lido por pessoas;
  - Colocar "a", "b", "c" ou "x", "y" não fica legível para manutenção de código;
  - Importante ter documentação do que o método faz para que as pessoas não fiquem muito tempo tentando entender o código.
 
### 1. Entender a história da linguagem e suas convenções
- ex:
  - java, scala: variavelEmJava (camel case)/ métodos com primeira letra maiúscula;
  - python variavel_em_python (snake case)/ métodos em snake case;
  - formatação (ferramenta para verificar a formatação): Java: checkstyle; Python:pep8.
   
### 2. Entender as características da linguagem
- ex:
  - Java, Scala: orientação a objeto para aproveitar o máximo do java;
  - Python, Scala: linguagem funcional (método, função anônima, etc).
 
### 3. Estudar e aprender com bons profissionais em texto, blog, etc
- ex:
  - S.O.L.I.D. - método para melhorar a legibilidade do código.

# Deploy
### Entregas
- Os artefatos que são gerados para os clientes normalmente são divididos em 4 tipos de arquivos:
  - Scripts
  - JAR
  - Zip
  - .egg
 
- Agendamento
  - crontab, control-M, ferramentas na nuvem, no cloudera (apache oozie), apache airflow
 
- Deploy
  - de arquivos: de uma máquina pra outra;
  - abrir chamados: GMUD;
  - devops bem estruturado: pipeline, validando o que fez.
  - ![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/1df6685f-8399-40b5-a797-a6318581c667)

# Templates
- Baixar modelo de template da aula;
- Camada Raw - dados sem tratamento (do jeito que o arquivo veio);
- Transformações;
![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/6b672354-0f3c-47e9-ba33-f1dea7931564)
![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/c6296049-4d98-4a75-8a8e-77b801c5a283)

- Modelo baseado em serviços (carregar e gravar o dado no data lake *tem regras de negócios):
- ![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/f9971755-08aa-417e-9a1e-9a8cead87768)
- modelo baseado no ciclo de vida do dado (fluxo das zonas mostrado acima *tem regras de negócios):
- ![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/f864c5d3-be53-4747-ae82-9a603d3ebdb1)

# Spark Application - Introdução
## Spark shell x Spark applications
- Spark shell
  - Exploração e manipulação dos dados (visualiza os dados)
  - Usado para estudo

- Spark applications
  - Usado para rodar programas no cluster
  - Rodar os programas de forma independente
  - Aplicações para rodar em vários nós
  - Jobs para ETL e streaming (foi visto apenas in batch, vai ser visto in stream)
  - Criação de objetos
    - SparkSession - spark. (spark SQL)
    - SparkContext - sc.
   
## Rodar uma Spark application
- ```spark-submit --class NameList MyJarFile.jar people.json namelist/```
- Colocar no nó master;
- Class só funciona para Java e Scala;
- Compactou o projeto no .jar
- Opções submit
  - master: local, yarn, mesos ou spark standalone (como vai rodar o projeto)
  - jars: adicionar arquivos jar
  - py-files: lista de arquivos em .py, .zip ou .egg
  - driver-java-options: parâmetros para o driver JVM (Java e Scala)
  - deploy-mode: client (vai executar na máquina que está enviando) ou cluster (vai enviar para vários nós do cluster e fazer o processamento e paralelo)
  - driver-memory: Memória alocada para o spark driver (padrão: 1G, recomendável que não passe de 8G)
  - executor-memory: Memória alocada para a aplicação (limite: qtd de memória do cluster)
  - num-executors: Número de executores para iniciar com a aplicação
  - driver-cores: Número de cores alocados para o spark driver
  - queue: Rodar na fila do Yarn (fila de aplicações + ou - críticas, ou que utilizam + ou - recursos)
  - help (mostrar as opções)
