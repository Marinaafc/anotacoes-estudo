# Queries e Filtros

### Queries

- Quão bem a busca corresponde com o documento;
- Características:
  - Tem score;
  - Não são armazenados na cache.

### Filtros

- A busca corresponde com o documento;
  - Sim ou não (busca exata)
- Características:
  - Não possui score (se quiser ordenar é por order by);
  - Dados armazenados em cache automaticamente;
    - Acelera o desempenho
  - As buscas são mais rápidas porque não é preciso fazer o cálculo do score.

### Inverted Index

- Index Invertido
- Index Remissivo
  - As páginas que estão localizados os principais termos

![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/bb19e6de-a672-48e3-b04d-41332f00b055)

- Todas informações inseridas no Elastic são mapeadas, por isso que pra alterar o "schema" precisa indexar o documento de novo. Isso porque a palavra salva já está no índice invertido. Alterando o tipo, precisa alterar a forma que a palavra está salva (reindexar os dados);

### Queries Tipos

- Query DSL
- Todas as queries calculam o "_score";
  - Se não
    - Utilizar o "constant_score" (deixa o score sempre em 1 -constante)
    - É utilizado quando não se quer usar o score, para fazer uma filtragem por exemplo
- É possível  filtrar os dados primeiro para se obter algumas informações para depois filtrar com o score para os dados que quiser score
- Filtrar os dados antes da busca textual
  - Ganha desempenho
- Consultas:
  - Term
  - Terms
  - Range
  - Match
  - Exists
  - Missing
  - Prefix
  - Wildcard
  - Regexp
  - Fuzzy
  - Ids

### Exemplo Query - Term

- O termo é para pesquisar termos, mas ele faz consultas mais específicas;
- Ex: pesquisar um nº ou uma palavra, diferenciar minúsculo de maiúsculo na pesquisa (precisa saber como a palavra está salva no index invertido)
```
GET cliente/_search
{
  "query":{
  "term":{
    "nome":"joão"
    }
  }
}
```
### Exemplo Query - Term com constant_score

- Pode ser utilizado para agilizar consulta
```
GET cliente/_search
{
  "query":{
    "constant_score":{
      "filter":{
        "term":{
          "nome":"joão"
        }
      }
    }
  }
}
```
- Observar a diferença entre o "took" e o "score" de um e de outro (com o constant_score é bem mais rápido)

### Exemplo Query - Terms

- Pode colocar múltiplos termos na pesquisa;
```
GET cliente/_search
{
  "query":{
  "terms":{
    "idade":[30,20]
    }
  }
}
```
