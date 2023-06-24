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

```json
PUT produto1
{
  "mappings":{
    "properties":{
      "descricao":{
        "type":"text",
        "analyzer":"brazilian",
        "fields":{"original":{"type":"keyword"}}
      }
    }
  }
}
```

c) Buscar a palavra “compativel” no campo descricao.original (hits = 0)

```
GET produto1/_search?q=decricao.original:compativel
```

d) Buscar a palavra “compativel” no campo descricao

```
GET produto1/_search?q=decricao:compativel
```
