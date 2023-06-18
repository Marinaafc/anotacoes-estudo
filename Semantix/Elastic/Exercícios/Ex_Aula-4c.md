
### 1. Verificar se existe o índice populacao

```json
HEAD populacao
```

### 2. Executar as consultas no índice populacao

a) Mostrar os documentos com o atributo "Total Population" menor que 100

```json
GET populacao/_search
{
  "query":{
    "range":{
      "Total Population":{
        "lt":100
      }
    }
  }
}
```

b) Mostrar os documentos com o atributo "Median Age" maior que 70
```json
GET populacao/_search
{
  "query":{
    "range":{
      "Median Age":{
        "gt":70
      }
    }
  }
}
```
c) Mostrar os documentos 50 (Zip Code: 90056) à 60 (Zip Code: 90067) do índice de populacao

```json
GET populacao/_search
{
  "query":{
    "range":{
      "Zip Code":{
        "gte":90056,
        "lte":90067
      }
    }
  }
}
```
### 3. Importar através do Kibana o arquivo weekly_MSFT.csv (Guia Arquivos/dataset/weekly_MSFT.csv) com o índice bolsa

### 4. Executar as consultas no índice bolsa

a) Visualizar os documentos do dia 2019-01-01 à 2019-03-01. (hits = 9)


```json
GET bolsa/_search
{
  "query":{
    "range":{
      "timestamp":{
        "gte":"2019-01-01",
        "lte":"2019-03-01",
        "format_date":"dd/MM/yyyy||yyyy"
      }
    }
  }
}
```
b) Visualizar os documentos do dia 2019-04-01 até agora. (hits = 3)
```json
GET bolsa/_search
{
  "query":{
    "range":{
      "timestamp":{
        "gte":"2019-04-01",
        "lte":"now",
        "format_date":"dd/MM/yyyy||yyyy"
      }
    }
  }
}
```