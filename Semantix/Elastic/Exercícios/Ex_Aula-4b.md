Realizar todas as buscas a seguir no índice produto

### 1. Buscar os documentos que contenham as palavras “Windows” e “Linux” no atributo descrição
```
GET produto/_search
{
  "query":{
    "match":{
      "descricao":{
        "query":"Windows Linux",
        "operator":"and"
      }
    }
  }
}
```
### 2. Buscar os documentos que contenham as palavras “Windows”, “Linux” ou “USB” no atributo descrição
```
GET produto/_search
{
  "query":{
    "match":{
      "descricao":"Windows Linux USB"
    }
  }
}
```
> É a mesma coisa de utilizar o "descricao":{"query":"Windows Linux USB", "operator":"or"}
### 3. Buscar os documentos que contenham pelo menos 2 palavras da seguinte lista de palavras: “Windows”; “Linux” e “USB” no atributo descrição
```
GET produto/_search
{
  "query":{
    "match":{
      "descricao":{
        "query":"Windows Linux USB",
        "minimum_should_match":2
      }
    }
  }
}
```
### 4. Buscar os documentos que contenham pelo menos 50 % da seguinte lista de palavras: “Windows”; “Linux” e “USB” no atributo descrição
```
GET produto/_search
{
  "query":{
    "match":{
      "descricao":{
        "query":"Windows Linux USB",
        "minimum_should_match":"50%"
      }
    }
  }
}
```
> Como tem 3 nomes na pesquisa (Windows,Linux e USB) e só encontrou 1, já é considerado como 50%, pois metade de 3 é 1,5 e não é considerado esse 0,5. Logo, 1 já é considerado 50%. Se fossem 4 termos, teria que encontrar pelo menos 2. 