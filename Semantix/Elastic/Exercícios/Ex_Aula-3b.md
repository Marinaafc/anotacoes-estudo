### 1. Visualizar as configurações do índice produto
```
GET produto/_settings
```
### 2. Visualizar o mapeamento do índice produto
```
GET produto/_mapping
```
### 3. Visualizar o mapeamento do atributo nome do índice produto
```
GET produto/_mapping/field/nome
```
### 4. Inserir o campo data do tipo date no índice produto
```
PUT produto/_mapping/
{
  "properties":{
    "data":{"type":"date"}
  }
}
```
### 5. Adicionar o documento:

_id: 6, "nome": "teclado", "qtd": 100, "descricao": "USB", "data":"2020-09-18"
```
POST produto/_doc/6
{
  "doc": {
    "nome": "teclado",
    "qtd": 100,
    "descricao": "USB",
    "data":"2020-09-18"
  }
}
```
### 6. Reindexar o índice produto para produto2, com o campo quantidade para o tipo short
```
POST _reindex
{
  "source":{
  "index":"produto"
},
  "dest":{
  "index":"produto2"
  },
{
  "properties":{
    "qtd":{"type":"short"},
  }
}
```
### 7. Visualizar o mapeamento do índice produto2
```
GET produto2/_mapping
```
### 8. Fechar o índice produto
```
POST produto/_close
```
### 9. Pesquisar todos os documentos no índice produto
```
GET produto/_search
```
### 10. Abrir o índice produto
```
POST produto/_open
```
