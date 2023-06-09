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

