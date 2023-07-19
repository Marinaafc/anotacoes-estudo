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

# Spark Streaming - Leitura de Dados

# Spark Streaming - Operações

# Spark Streaming - Exemplo de Contar Palavras
