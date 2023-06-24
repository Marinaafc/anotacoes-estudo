### 1. Criar os Analyzer simple, standard, brazilian e portuguese para a seguinte frase:

- O elasticsearch surgiu em 2010
```json
POST _analyze
{
  "analyzer":"simple",
  "text":"O elasticsearch surgiu em 2010"
}
```
```json
POST _analyze
{
  "analyzer":"standard",
  "text":"O elasticsearch surgiu em 2010"
}
```
```json
POST _analyze
{
  "analyzer":"brazilian",
  "text":"O elasticsearch surgiu em 2010"
}
```
```json
POST _analyze
{
  "analyzer":"portuguese",
  "text":"O elasticsearch surgiu em 2010"
}
```
### 2. Realizar os passos no índice produto

a) Criar um analyzer brazilian para o atributo descricao
```json
PUT produto1
{
  "settings":{
    "index":{
      "number_of_shards":1,
      "number_of_replicas":0
    }
  },
  "mappings":{
    "properties":{
      "descricao":{
        "type":"text",
        "analyzer":"brazilian"
      }
    }
  }
}
```
- O resto do mapping vai pegar do index anterior (produto)
```json
POST _reindex
{
  "source":{
  "index":"produto"
},
  "dest":{
  "index":"produto1"
  }
}
```

b) Para o atributo descricao aplicar o analzyer brazilian para o tipo de campo text e criar o atributo descricao.original com o dado do tipo keyword
***Precisa alterar o "PUT" que da questão anterior***
```json
PUT produto1
{
  "settings":{
    "index":{
      "number_of_shards":1,
      "number_of_replicas":0
    }
  },
  "mappings":{
    "properties":{
      "descricao":{
        "type":"text",
        "analyzer":"brazilian"
        "fields":{"original":{"type":"keyword"}}
      }
    }
  }
}
```
```
DELETE produto
```
```json
PUT produto
{
  "settings":{
    "index":{
      "number_of_shards":1,
      "number_of_replicas":0
    }
  },
  "mappings":{
    "properties":{
      "descricao":{
        "type":"text",
        "analyzer":"brazilian"
        "fields":{"original":{"type":"keyword"}}
      }
    }
  }
}
```
```json
POST _reindex
{
  "source":{
  "index":"produto1"
},
  "dest":{
  "index":"produto"
  }
}
```

c) Buscar a palavra “compativel” no campo descricao.original (hits = 0)

```
GET produto1/_search?q=decricao.original:compativel
```
```json
GET produto1/_search
{
  "query":{
    "match":{
      "descricao.original":"compativel"
    }
  }
}
```

d) Buscar a palavra “compativel” no campo descricao

```
GET produto1/_search?q=decricao:compativel
```
```json
GET produto1/_search
{
  "query":{
    "match":{
      "descricao":"compativel"
    }
  }
}
```
