Realizar todas as buscas a seguir no índice produto

### 1. Buscar no termo nome o valor mouse
```
GET produto/_search
{
  "query":{
    "term":{
      "nome":"mouse"
    }
  }
}
```

### 2. Buscar no termo nome os valores mouse e teclado
```
GET produto/_search
{
  "query":{
    "terms":{
      "nome":["mouse","teclado"]
    }
  }
}
```

### 3. Realizar a mesma busca do item 1 e 2, desconsiderando o score
```
GET produto/_search
{
  "query":{
    "constant_score":{
      "filter":{
        "term":{
          "nome":"mouse"
        }
      }
    }
  }
}
```
```
GET produto/_search
{
  "query":{
    "constant_score":{
      "filter":{
        "terms":{
          "nome":["mouse","teclado"]
        }
      }
    }
  }
}
```

### 4. Buscar os documentos que contenham a palavra “USB” no atributo descrição
```
GET produto/_search
{
  "query":{
    "match":{
      "descricao":"USB"
    }
  }
}
```

### 5. Buscar os documentos que contenham a palavra “USB” e não contenham a palavra “Linux” no atributo descrição
```
GET produto/_search
{
  "query":{
    "bool":{
      "must":{"match":{"descricao":"USB"}},
      "must_not":{"match":{"descricao":"Linux"}
      }
    }
  }
}
```

### 6. Buscar os documentos que podem ter a palavra “memória” no atributo nome ou contenham a palavra “USB” e não contenham a palavra “Linux” no atributo descrição
```
GET produto/_search
{
  "query":{
    "bool":{
      "should":[
        {"match":{"nome":"memória"}},
        {"match":{"descricao":"USB"}}
        ],
      "must_not":{"match":{"descricao":"Linux"}
      }
    }
  }
}
```
