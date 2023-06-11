### 1. Pesquisar no índice produto os documentos com os seguintes atributos:

a) Nome = mouse
```
GET produto/_search?q=nome:mouse
{
"query":{
  "match_all":{}
  }
}
```

b) Quantidade = 30
```
GET produto/_search?q=qtd:30
{
"query":{
  "match_all":{}
  }
}
```

c) Descrição = USB
```
GET produto/_search?q=descricao:USB
{
"query":{
  "match_all":{}
  }
}
```

d) Nome = hd e descrição = windows
```
GET produto/_search?q=nome:hd&q=decricao:windows
{
"query":{
  "match_all":{}
  }
}
```

e) Nome = memória e descrição = GB
```
GET produto/_search?q=nome:memoria&q=descricao:GB
{
"query":{
  "match_all":{}
  }
}
```

### 2. Pesquisar todos os índices, limitando a pesquisa em 5 documentos em cada página e visualizar a 4 página (Documentos entre 16 á 20 )
```
GET _all/_search?&size=5&from=15
```
