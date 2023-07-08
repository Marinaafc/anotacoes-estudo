# Jupyter Notebook - Conceitos
### Projeto Jupyter
- Desenvolver softwares
  - Open-Source
  - Open-Standards
  - Serviços para computação interativa em diversas linguagens
- Jupyter Notebook (Produto do Projeto Jupyter)
  - Ambiente computacional na web
  - Cria documentos, dentro destes pode ter:
    - Código ativo
    - Equações
    - Gráficos
    - Texto (Markdown)
  - localhost:8889
 
### Jupyter Notebook

- Projeto PySpark, executa:
  - Comandos em Python: ```print("seja bem vindo")```
  - Comandos em Spark: ```spark```
  - Comandos em Linux: ```!pwd``` *(tem que colocar a "!" antes)*
  - Extensão do arquivo: ```.ipynb``` *(IPython Notebook)*
 
- Atalhos
  - Executar a célula: ```ctrl Enter```
  - Executar a célula e criar uma nova célula: ```shift Enter```
  - Alterar a célula para o tipo código: ```y```
  - Alterar a célula para o tipo markdown: ```m```
  - Deletar uma célula: ```dd```
  - Desfazer o deletar a célula: ```z```
  - Ajuda: ```h```
  - Criar uma célula acima: ```a```
  - Criar uma célula abaixo: ```b```

### Google Colab - Alternativa para Jupyter Notebook
- Não tem a mesma performance do Jupyter Notebook, mas tem suas vantagens
  - Pode ser utilizado para testes ou para não precisar baixar o Jupyter em algum computador diferente

# Sessão no Spark

- Criar sessão no Spark
  - Configurar nós (o que vai precisar de alocação de RAM e de Core, min, max)
  - Configurar o projeto (nome do projeto, etc)
 
- Classe SparkSession fornece o acesso às funcionalidades do spark
  - sql - Executa as consultas Spark SQL
  - catalog - Gerenciar tabelas
  - read - função para ler dados de um arquivo ou outra fonte de dados
  - conf - objeto para gerenciar configurações de Spark
  - sparkContext - Core Spark API
 
```
spark = SparkSession \
  .builder \
  .appName("Python") \
  .getOrCreate()
```
