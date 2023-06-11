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
- _create: se o documento já existir, ele não vai criar, vai dar um aviso que já existe;
- Se mudar a idade de 20 para 21 usando _doc, não vai atualizar o documento, mas vai reindexá-lo, ou seja, vai inserir o documento todo novamente e alterar de 20 para 21;

### CRUD POST
- Utilizado para criar um documento com _id ou atualizar um documento parcial;
- No primeiro exemplo, vai atualizar o documento que foi criado em PUT sem precisar reindexar novamente;
- Em _update, tem que colocar "doc" para falar qual documento quer atualizar e quais campos quer modificar;
- Ex:
```
POST cliente/_update/1
{
    "doc":{
            "nome":"João"
    }
}
POST cliente/_doc
{
    "nome":"Marcos"
}
```

***OBS: Com o POST, quando não coloca o /1, automaticamente o Elastic vai inserir o _id.***

### CRUD DELETE

- Deletar um documento;
```
DELETE cliente/_doc/1
```
- Deletar um índice;
```
DELETE cliente
```
### CRUD GET
- Informações sobre o nó do Elasticsearch (http://localhost:9200/)
```
GET /
```
- Buscar todos os documentos em um índice
```
GET cliente/_search
```
- Buscar um documento em um índice
```
GET cliente/_doc/1
```
- Buscar a quantidade de documentos em um índice
```
GET cliente/_count
```
- Buscar os dados de um documento em um índice
```
GET cliente/_source/1
```
> É como se fosse em SQL -> select * from cliente where id=1;  
> Ele busca só os dados do documento, sem os dados que a Elastic gera.

# Múltiplas Operações Simultâneas

- Como fazer operações de CRUD de forma simultânea.

### Bulk API

- Execução de várias operações de indexação / exclusão em uma única chamada de API;
- Isso aumenta a velocidade de indexação;
- Comandos:
```
POST _bulk
{"index":{"_index":"test","_id":"1"}}
{"field1":"value1"}
{"delete":{"_index":"test","_id":"2"}}
{"create":{"_index":"test","_id":"3"}}
{"field1":"value3"}
{"update":{"_id":"1","_index":"test"}}
{"doc":{"field2":"value2"}}
```
> **"index":** Não força com o _create. Pode inserir várias informações com o mesmo id que vai substituir e criar novas versões.  
> Com o index, se tentar indexar novamente o mesmo id, ele vai permitir. Já com o create vai dar erro, pois ele só permite 1 versão.

- Ex:
```
POST concessionaria/_bulk
{"create":{"_id":"1"}}
{"nome":"carro"}
{"create":{"_id":"2"}}
{"nome":"automovel"}
...
```

# Importação de Dados com Kibana

- Home
- Menu/Kibana/Machine Learning
    - Import data
        - Upload file  
