# Analyzer

### Introdução 

Existem 2 tipos de buscas possíveis de fazer no Elastic:

- Busca exata
  - Sim e não

- Busca FullText
  - Busca o tão quanto a palavra casa (_score);
  - Quando se faz uma busca no FullText, pode-se aplicar um **Analisador**;
  - Com esse analisador é possível melhorar a forma com que se vai fazer a busca;
    - É possível **testar** o analisador com várias sentenças de palavras para adaptá-lo melhor para o caso de uso;
    - Depois que testar, consegue **aplicar em atributos específicos** em que se quer realizar a análise;
    - Ou até mesmo criar um **Analyzer personalizado**

Como funciona o Analyzer?

- Índice Invertido
  - Quebra em tokens;
  - Insere numa tabela (vai ter uma referência de onde se localiza cada token);
  - Ex: ```_search?cidade="São Paulo"```
    - Vai separar em tokens: "São" vai ser um token e "Paulo" vai ser outro token
  - É com base nesses tokens que se trabalha em cima dos Analyzers para dizer como que quer que salve;
    - Ex: Se quer que só separe por espaço, se quer que tire acentuação, que deixe tudo minúsculo, etc...
  - Analyzers estão na parte de como os tokens serão gerados

### Analyzers Principais

- Espaço em branco: whitespace
  - Separa as palavras por espaço

- Simples: simple
  - Remover números
  - Remover espaços e pontuação ('!@#$%"&*-+`~^/:;><,)
  - Somente texto
  - Texto em lowercase

- Padrão: standard
  - Remover espaços e pontuação
  - Texto em lowercase 

> OBS: Por isso que mesmo quando foi cadastrado o dado "USB" em maiúsculo, teve que buscar esse dado em minúsculo. Porque o padrão do Elastic é indexar o dado em lowercase

- Idioma: brazilian,english
  - remover acentos, gêneros e plural (deixa a palavra na forma raiz)

# Analyzer Exemplos

Formas de testar o Analyzer:

### Analyzer Standard ou Simples

```json
POST _analyze
{
  "analyzer":"standard",
  "text":"Elasticsearch e Hadoop são ferramentas de Big Data"
}
```
```json
POST _analyze
{
  "analyzer":"simple",
  "text":"Elasticsearch e Hadoop são ferramentas de Big Data"
}
```
```json
{
  "tokens":[
    {"token":"elasticsearch"},
    {"token":"e"},
    {"token":"hadoop"},
    {"token":"são"}, 
    {"token":"ferramentas"}, 
    {"token":"de"},
    {"token":"big"},
    {"token":"data"}
  ]
}
```
> A única diferença é que o simple tira os números. Como na frase não tinha números, o resultado foi o mesmo.  

*** Anotação para mim: Acho que esse "são" está errado, deveria ser "sao", testar no kibana depois.

### Analyzer whitespace
```json
POST _analyze
{
  "analyzer":"whitespace",
  "text":"Elasticsearch e Hadoop são ferramentas de Big Data"
}
```
```json
{
  "tokens":[
    {"token":"Elasticsearch"},
    {"token":"e"},
    {"token":"Hadoop"},
    {"token":"são"}, 
    {"token":"ferramentas"}, 
    {"token":"de"},
    {"token":"Big"},
    {"token":"Data"}
  ]
}
```
> Neste caso, se pesquisar "Elasticsearch", vai encontrar. Se pesquisar "elasticsearch", não vai encontrar.

### Analyzer em português
- Como tem menos tokens, a busca é mais rápida, porque tem menos coisa para buscar.
```json
POST _analyze
{
  "analyzer":"brazilian",
  "text":"Elasticsearch e Hadoop são ferramentas de Big Data"
}
```
```json
{
  "tokens":[
    {"token":"elasticsearch"},
    {"token":"hadoop"},
    {"token":"sao"}, 
    {"token":"ferrament"}, 
    {"token":"big"},
    {"token":"dat"}
  ]
}
```
> Tem coisas que dá pra configurar e especificar. Ex: retornar "data" com o "a", sem ser "data".

### Analyzer English

```json
POST _analyze
{
  "analyzer":"english",
  "text":"Elasticsearch and Hadoop are Big Data tools"
}
```
```json
{
  "tokens":[
    {"token":"elasticsearch"},
    {"token":"hadoop"},
    {"token":"big"},
    {"token":"data"},
    {"token":"tool"}
  ]
}
```
> O dicionário do inglês é melhor formatado do que o brazillian. Por exemplo, não tem necessidade do "são".

# Aplicação de Analyzer em atributos

### Adicionar em um atributo
```json
PUT cliente1
{
  "mappings":{
    "properties":{
      "conhecimento":{
        "type":"text",
        "analyzer":"standard"
      }
    }
  }
}
```
- Para alterar o analyzer de um atributo que já existe, precisa fazer o mesmo processo de mudar o tipo do dado: **reindexar tudo**;
- Por isso que é importante sempre testar o analyzer para quando ter que fazer esse processo, fazer só uma vez e não ter que ficar reindexando sempre.

### Analyzer - Boas Práticas

- Um campo pode ter diferentes atributos para diferentes fins;
- Indexar o mesmo campo de maneiras diferentes para fins diferentes
- Geralmente, quando se tem um campo que se faz busca, ele não é apenas de um tipo, normalmente ele vai ter 2 tipos (boa prática):
  - Tipo Keyword (É a palavra inteira, ex: "São Paulo")
  - Usado para:
    - Classificação e
    - Agregação dos dados
  - Tipo Text (Utilizado para busca)
    - Pesquisa Fulltext
   
- Manter 2 versões do atributo com analyzer (boa prática)
  - Tipo Keyword
    - Dado original
  - Tipo Text
    - Dado com analisador
   
### Analyzer Exemplo - Campo de 2 Tipos
```json
PUT cliente2
{
  "mappings":{
    "properties":{
      "conhecimento":{
        "type":"text",
        "analyzer":"standard",
        "fields":{"raw":{"type":"keyword"}}
      }
    }
  }
}
```
> raw = dado bruto

### Exemplo: Criação de índice com settings e mappings
```json
PUT cliente3
{
  "settings":{
    "index":{
      "number_of_shards":1,
      "number_of_replicas":0
    }
  },
  "mappings":{
    "properties":{
      "nome":{"type":"text"},
      "conhecimento":{
        "type":"text",
        "analyzer":"whitespace",
        "fields":{
          "raw":{"type":"keyword"}
        }
      }
    }
  }
}
```

# Conceitos de Agregações

- **Agregação** é uma forma de analisar os dados indexados;
- Sempre vai seguir a seguinte estrutura:
```json
GET <index>/_search
{
  "aggs":{
    "<nomeAgregação>":{
      "<TipoAgregação>":{}
    }
  }
}
```
> Pode ser usado em cima do "aggs" tudo que já foi visto, como query, size, etc


### Tipos de Agregações

- **Bucket**
  - Combinam os documentos resultantes em buckets
    - Buckets são criados

- **Metric**
  - Cálculos matemáticos feitos nos campos de documentos
    - São calculados em buckets

- **Matrix**
  - Operam em diversos campos, produzindo uma matriz de resultado (matrix_stats)

- **Pipeline**
  - Agrega a saída de outras agregações 
  - A saída de uma agregação é outra agregação

### Buckets

- Conjunto de documento formado por critérios
  - Data
  - Intervalo
  - Atributo
- Ex:
  - Range
  - Date_range
  - Ip_ranges
  - Geo_distance (latitude, longitude)
  - Significant_terms (os termos mais significantes)
  - Etc

### Métricas

- Operações matemáticas
  - Um valor de saída
- Ex:
  - Avg
  - Sum
  - Min
  - Max
  - Cardinality
  - Value_count (conta todos os documentos)
  - Etc

- Operações matemáticas
  - N valores de saída
- Ex:
  - Stats
  - Percentiles
  - Percentile_ranks
  - Etc

# Agregações de Métricas

### Exemplo - Avg
```json
GET cliente/_search
{
  "query":{...},
  "aggs":{
    "media":{
      "avg":{
        "field":"qtd"
      }
    }
  }
}
```
> - Neste exemplo, "media" é o nome da agregação (pode escolher qualquer um nome);
  - Não é obrigado colocar uma query. A query é usada para caso queira filtrar em quais documentos do índice vai ser realizada a agregação;
  - Se não for colocada uma query, a agregação será realizada em todos os documentos do índice.

### Exemplo - Sum com limitação de escopo

- Visualizar apenas o resultado da agregação ou uma parte dos resultados
  - **size**
```json
GET cliente/_search
{
  "query":{...},
  "size":0,
  "aggs":{
    "soma":{
      "sum":{
        "field":"qtd"
      }
    }
  }
}
```
- Sempre vai mostrar primeiro todo o resultado da query para depois mostrar o resultado da agregação;
- Até quando não tem a query, o Elastic mostra por padrão os 10 primeiros documentos;
- Para mostrar apenas agregação, deve colocar o size=0

### Exemplo - Stats

- Várias estatísticas com apenas uma requisição;
- Estatísticas:
  - "count"
  - "min"
  - "max"
  - "avg"
  - "sum"

```json
GET cliente/_search
{
  "query":{...},
  "aggs":{
    "estatistica":{
      "stats":{
        "field":"qtd"
      }
    }
  }
}