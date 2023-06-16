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
# Bool Query

- Consultas Booleanas;
- São usadas para filtrar um dataset grande, porque normalmente com um dataset grande se tem a necessidade de fazer várias consultas distintas (pra isso é necessária uma estrutura flexível);
- Tem uma estrutura flexível;
- Atributos:
  - Must = And (Usa score)
  - Should = Or (Usa score)
  - Must_not = Not and (Usa score)
  - Filter = Filtrar mais dados antes de atender as outras cláusulas (Não usa score)
- Normalmente os dados são filtrados para depois fazer uma query calculando score nos dados necessários;
- Ex:

```
GET cliente/_search
{
  "query":{
    "bool":{
      "must":[{...}],
      "must_not":[{...}],
      "should":[{...}],
      "filter":[{...}]
    }
  }
}
```
- Ex 1 Consulta booleana com 1 atributo (não faz sentido usar bool, é só um exemplo)
```
GET cliente/_search
{
  "query":{
    "bool":{
      "should":{
        "term":{
          "idade":"30"
        }
      }
    }
  }
}
```
> Vai retornar todos os documentos com idade = 30

- Ex 1 Consulta booleana com N atributos
```
{
  "query":{
    "bool":{
      "must":[
        {"match":{"estado":"sp"}},
        {"match":{"ativo":"sim"}}
      ]
    }
  }
}
```
> "match" é muito parecido com "terms", mas faz uma análise um pouco maior, enquanto o terms faz uma busca mais específica. Por exemplo, não vai diferenciar maiúsculo com minúsculo. Não faz diferença pesquisar "sp" ou "SP", já com o terms faria diferença.

- Ex N consultas booleanas com N atributos
```
{
  "query":{
    "bool":{
      "must":{"match":{"setor":"vendas"}},
      "should":[
        {"match":{"tags":"imutabilidade"}},
        {"match":{"tags":"larga escala"}}
      ],
      "must_not":{"match":{"nome":"inativo"}}
    }
  }
}
```
> O should é usado muito quando se quer aumentar o score de uma busca, porque sempre que encontra a busca do should, dá um up no score

# Ordem de Busca

- Para calcular a ordem de busca, é considerado:
  - Quantas vezes o termo aparece no atributo (se é um atributo que aparece mais vezes, vai ter uma relevância/score maior);
  - Tamanho do atributo (quanto menor, maior a relevância/score);
  - Tamanho do termo (quanto menor, maior a relevância/score);
  - Quantas vezes o termo aparece em todos os documentos (quanto menos aparecer, maior o boost na busca);
  - Exemplo:
```
GET cliente/_search
{
  "query":{
    "match":{
      "conhecimento":"sqoop hive impala elk"
    }
  }
}
```
> Quando o documento tiver mais do atributo que eu estou buscando, ele vai ficar em 1º lugar;

### Operador Busca
- Padrão é o "or";
- "operator":"and";

```
GET cliente/_search
{
  "query":{
    "match":{
      "conhecimento":{
        "query":"sqoop hive",
        "operator":"and"
      }
    }
  }
}
```
> Tem que ter a palavra sqoop e palavra hive no atributo conhecimento.

- Definir o mínimo de % que estejam na consulta
  - "minimum_should_match":"valor em %"
```
GET cliente/_search
{
  "query":{
    "match":{
      "Hobby":{
        "query":"sqoop hive impala",
        "minimum_should_match":"50%"
      }
    }
  }
}
```

- Definir o mínimo de % que estejam na consulta
  - "minimum_should_match":Número
```
GET cliente/_search
{
  "query":{
    "match":{
      "Hobby":{
        "query":"sqoop hive impala",
        "minimum_should_match":2
      }
    }
  }
}
```

### Múltiplos Atributos
- "multi_match" permite pesquisar em múltiplos campos
```
{
  "query":{
    "multi_match":{
      "query":"pinheiros",
      "type":"most_fields",
      "fields":["endereco", "cidade", "estado"]
    }
  }
}
```
- OBS: Não pode usar junto com operador e minimum_should_match.
