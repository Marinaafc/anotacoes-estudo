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
        "field":"volume"}
    }
  }
}

```
### 2. Calcular a estatística do campo close
```json
```
### 3. Visualizar os documentos do dia 2019-04-01 até agora. (hits = 3)
```json
```
### 4. Calcular a estatística do campo open do período do dia 2019-04-01 até agora
```json
```
### 5. Calcular a mediana do campo open
```json
```
### 6. Contar a quantidade de documentos agrupados por ano
```json
```
### 7. Contar a quantidade de documentos de 2 anos atrás até hoje
```json
```
