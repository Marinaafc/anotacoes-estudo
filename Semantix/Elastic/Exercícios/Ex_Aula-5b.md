### Realizar os exercícios no índice bolsa
```
HEAD bolsa
```
### 1. Calcular a média do campo volume
```json
GET bolsa/_search
{
  "size":0,
  "aggs":{
    "media":{
      "avg":{
        "field":"volume"
      }
    }
  }
}
```
### 2. Calcular a estatística do campo close
```json
GET bolsa/_search
{
  "size":0,
  "aggs":{
    "estatistica":{
      "stats":{
        "field":"close"
      }
    }
  }
}
```
### 3. Visualizar os documentos do dia 2019-04-01 até agora. (hits = 3)
```json
GET bolsa/_search
{
  "size":0,
  "query":{
    "range":{
      "timestamp":{
        "gte":"2019-04-01",
        "lt":"now",
        "format":"yyyy-MM-dd"
      }
    }
  }
}
```
### 4. Calcular a estatística do campo open do período do dia 2019-04-01 até agora
```json
GET bolsa/_search
{
  "size":0,  
  "query": {
    "range": {
      "timestamp": {
        "gte": "2019-04-01",
        "lt": "now",
        "format": "yyyy-MM-dd"
      }
    }
  },
  "aggs": {
    "estatistica": {
      "stats": {
        "field": "open"
      }
    }
  }
}
```
### 5. Calcular a mediana do campo open
```json
GET bolsa/_search
{
  "size":0,
  "aggs":{
    "mediana":{
      "percentiles":{
        "field":"open"
      }
    }
  }
}
```
### 6. Contar a quantidade de documentos agrupados por ano
```json
GET bolsa/_search
{
  "size":0,
  "aggs":{
    "docs_por_ano":{
      "date_histogram":{
        "field":"@timestamp",
        "calendar_interval":"year"
      }
    }
  }
}
```
### 7. Contar a quantidade de documentos de 2 anos atrás até hoje
**FORMA IDEAL:**
```json
GET bolsa/_search
{
  "size": 0,
  "aggs": {
    "doc_anos": {
      "date_range": {
        "field": "@timestamp",
        "ranges": [
          {
            "from": "now-2y",
            "to": "now"
          }
        ]
      }
    }
  }
}
```
**OU outra forma que eu fiz:**
```json
GET bolsa/_search
{
  "size":0,  
  "query": {
    "range": {
      "timestamp": {
        "gte": "now-2y",
        "lt": "now",
        "format": "yyyy-MM-dd"
      }
    }
  },
  "aggs": {
    "qtd_documentos": {
      "value_count": {
        "field": "open"
      }
    }
  }
}
```
