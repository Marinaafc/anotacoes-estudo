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

b) Para o atributo descricao aplicar o analzyer brazilian para o tipo de campo text e criar o atributo descricao.original com o dado do tipo keyword

c) Buscar a palavra “compativel” no campo descricao.original (hits = 0)

d) Buscar a palavra “compativel” no campo descricao
