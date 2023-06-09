# Comunicação com Elasticsearch
- A comunicação com o Elasticsearch é feita através de Protocolo HTTP;
- Então se tem uma API Rest com comandos como HEAD, GET, POST, PUT, DELETE;
- São feitas requisições HTTP para a API de Elasticsearch por meio do envio de objetos JSON;
- A resposta sempre será no formato JSON;

### Estrutura de uma requisição HTTP
```
<ação> endereço_api:porta/índice/<opções>
```
Exemplo:
```
PUT localhost:9200/cliente/_create
```

# Operações Básicas

### CRUD

- **C**reate;  
- **R**ead;  
- **U**pdate;  
- **D**elete.  

### CRUD HEAD
- Retorna apenas o cabeçalho do HTTP;
- Útil para verificar se o documento existe;  

**Ex:**  

***Curl***  
- É uma ferramente de linha de comando;
- É uma biblioteca para transferir dados com URLs;
- Ex:
```
curl -XHEAD -v http://localhost:9200/cliente/_doc/1
```

***Kibana***  
- Opção: Dev Tools
- Ex:
```
HEAD cliente/_doc/1
```

### CRUD PUT
- Criar ou reindexar um documento inteiro (_version);
- Inserir documento = Indexar na Elasticsearch;
- Ex:
```
PUT cliente/_doc/1
{
    "nome":"Lucas",
    "idade": 20,
    "conhecimento": "Windows, Office, Hadoop, Elastic"
}
PUT cliente/_create/1
{
    "nome":"Lucas",
    "idade": 20,
    "conhecimento": "Windows, Office, Hadoop, Elastic"
}
```
- *PUT cliente/_doc/1 = Inserindo documento 1*
- Executando o comando **PUT cliente/_doc/1** novamente vai inserir o documento, mas vai vir com um atributo _version que vai mudar para 2 (2ª versão);
- Se não quiser inserir o documento novamente, tem que usar o comando **PUT cliente/_create/1**;
- _create: se o docummento já existir, ele não vai criar, vai dar um aviso que já existe;
- Se mudar a idade de 20 para 21 usando _doc, não vai atualizar o documento, mas vai reindexá-lo, ou seja, vai inserir o documento todo novamente e alterar de 20 para 21;

### CRUD POST
- Utilizado para criar um documento com _id ou atualizar um documento parcial;
