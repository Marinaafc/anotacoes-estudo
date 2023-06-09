# API de Pesquisa

### Pesquisa Funcionamento

- Buscar todos os documentos:
```
GET cliente/_search
```
- Pesquisar algo em todos os documentos:  
```
GET cliente/_search?q=hadoop
```  
> - Está pesquisando a palavra "hadoop";  
> - Faz uso do campo "_all" (pesquisa o valor "hadoop" em todos os campos);  
  
- Pesquisar em um atributo específico:
```
GET cliente/_search?q=nome:João
```
```
GET cliente/_search?q=nome:João&q=idade:20
```
### Query DSL

- Consultas baseadas em JSON;
- Query DSL (Domain Specific Language);
- A consulta pode ser feita em uma única linha ou em uma estrutura de JSON;
- Recomendado usar em estrutura de JSON, pois uma consulta simples pode evoluir ao longo do tempo;
- Ex:
```
GET cliente/_search?q=Hadoop
```
```
GET cliente/_search?q=Hadoop
{
"query":{
  "match_all":{}
  }
}
```
### Pesquisa Cabeçalho

- Quando se busca um documento, é criada uma estrutura de JSON, que pode ser observada na saída;
- Estrutura do json de busca
  - **Took**: Tempo em milissegundo que a consulta demorou;
  - **Timed_out**: Tempo de limite excedido;
  - **_shards**: Quantidade de shards usadas (sucesso e falha). Se aparecer falha, a busca pode estar errada, porque em algum nó foi feita uma busca e nesse nó houve uma falha;
  - **Hits**: Informação do resultado
    - Total: Quantidade de documentos encontrados;
    - Max_score: Valor de semelhança da consulta (0 a 1);
      - Ex: Buscar palavras contidas em um comentário (não é exato);
      - 0 = não encontrado, 1 = tem semelhança;
      - O número não vai de 0 a 1, pode passar de 1;
      - Tem como dar um boost para forçar o valor do score a ser maior;
      - Score é calculado com uso do algoritmo BM25.  

### Pesquisa Múltiplos Índices

- Pesquisar em todos os índices
  - Cuidado para não fazer consultas lentas!
```
GET _all/_search?q=Windows
```

- Pesquisar em índices específicos
```
GET produto,cliente/_search?q=Windows
```
```
GET produto,cliente/_count?q=Windows
```
- Caso o índice não exista
```
index_not_found_exception
```

# Limitação e Paginação
- Para pesquisas com muitos documentos
  - Difícil visualização (por padrão, mostra 10 documentos);
  - Limitar a quantidade de documentos;
    - Size = nº de documentos
  - Paginação
    - Quando você não quer ver todos os documentos de uma vez só, pode criar páginas de 100 em 100 por exemplo; 
    - From = A partir de qual documento que irá visualizar (Exemplo: Ver dos 200 documentos para frente)
  - Resposta máxima
    - from + size <= index.max_result_window(10.000);
    - O from + o size não pode ultrapassar esse atributo;
    - Scroll (para respostas que retornam mais de 10.000 documentos)
- Limitar o nº de documentos
```
GET cliente/_search?q=hadoop&size=100
```
- Paginação, visualizar x documentos por paginação
```
GET cliente/_search?q=hadoop&size=100&from=500
```
### Exemplos

- Mostrar os 10 primeiros documentos (1ª página)
```
GET _search?&size=10
{
"query":{
  "match_all":{}
  }
}
```
- Mostrar todos os documentos de 31 a 40 (4ª página)
```
GET _search?&size=10&from=30
{
"query":{
  "match_all":{}
  }
}
```
- Exemplo com paginação de 10 documentos
  - 1ª página - size=10 & from=0 (Default);
  - 2ª página - size=10 & from=10;
  - 10ª página - size=10 & from=90;

- Fórmula
  - 1º documento da busca = From + 1;
  - Último documento da busca = From + Size;
  - Página = (From/Size)+1

# Gerenciamento de Índices

- Create Index
- Get Index ("Buscar o Índice)
- Indices Exists
- Delete Index
- Open / Close Index API

### Índice Criação

- Configuração básica (Default)

```
PUT teste
{
  "settings":{
    "index":{
      "number_of_shards":1,
      "number_of_replicas":1
    }
},
  "mappings":{...},
  "aliases":{...}
}
```
- Boa prática, cada shard ter entre 20GB a 50GB (Não é uma regra, para encontrar o valor ideal, precisa testar o índice);
- Nº de shards 1 vai ficar no nó do container do Elasticsearch e Nº de réplicas não vai salvar em local nenhum, porque não tem outro nó;
- Como configurou 1 replica e só tem 1 nó, não vai salvar nenhuma réplica e vai gerar um estado de alerta para o cluster;
- Estados - verde: tudo ok, amarelo: quando uma replica não está alocada em algum nó, vermelho: quando um shard não está alocado em algum nó;

### Índice Busca

- Consultar índice
  - GET teste
  - GET teste/_search
  - GET teste/_settings
  - GET teste/_mapping
  - GET teste/_alias
  - GET teste/_stats (vai retornar estatíscas do nó: quantos documentos tem, qual o tamanho do índice, etc)

- Verificar se o índice existe
  - HEAD teste

### Índice Remoção

- Deletar um índice
```
PUT teste1
DELETE teste1
```
- Deletar múltiplos índices
```
PUT indice1
PUT indice2
PUT indice3
PUT indice4
GET ind*

DELETE ind*

HEAD ind*
```

### Índice Fechamento e Abertura

- Fechar índice
  - Diminui a sobrecarga no cluster
    - Mantém metadados (mapping, settings) quando fecha
    - Ex: Não dá para fazer get search quando está fechado, mas get mapping e settings sim
  - Bloqueia leitura e gravação (ex: PUT, POST)
  - Não é recomendado manter fechado por muito tempo
    - Pode acontecer de por algum motivo o nó deixar o cluster e se o índice estiver fechado não vai ser possível reconstruir as cópias. Então as cópias serão perdidas 
  - Solução: Frozen Index (Congela o Index)
    - Não tem perigo de perder as cópias com o Frozen Index
  - Exemplo:
    - POST teste/_close
    - POST teste/_open
    - POST test*/_close

# Mapeamento

- O Elasticsearch define automaticamente no índice os tipos dos campos;
- Mapeamento é como se fosse a mesma coisa do schema da tabela (SQL);
- Ex:
  - GET cliente/_mapping
  - Não é possível alterar o tipo de dado;
    - Reindex (Para alterar, precisa reindexar o documento)
  - É possível criar novos atributos;

### Tipo de dados que o Elastic aceita

- Core
  - Texto
    - text and keyword
  - Numérico
    - long, integer, short, byte, double, float
  - Date
  - Binary
  - Boolean
  - Ip
- Complexos
  - Object
  - Nested

### Mapeamento Pesquisa
- Mapeamento pelo índice
```
GET cliente/_mapping
```
```
GET client*/_mapping
```
- Mapeamento pelo atributo
```
GET cliente/_mapping/field/conhecimento
```
```
GET cliente/_mapping/field/conhe*
```
```
GET cliente/_mapping/field/nome,conhecimento
```
- Mapeamento de todos os índices
```
GET _mapping
```
### Mapeamento Exemplo
- OBS: recomendado criar o documento e usar o mapeamento automático do Elastic para depois copiar o mapeamento, colar, alterar o que for preciso e depois inserir os dados.
```
PUT cliente1
{}

PUT cliente1/_mapping/
{
  "properties":{
    "nome":{"type":"text"},
    "idade":{"type":"long"},
    "conhecimento":{"type":"keyword"}
  }
}
```

# Reindex

- Alterar o mapeamento
- Forma básica para reindexar
  - Configura o novo índice;
  - Indexa o índice de entrada (source) para o destino (dest);
  - Vai indexar todos os dados do índice de entrada no índice de destino, mas não vai pegar os metadados (settings, mapping,...);
- Exemplo:
```
POST _reindex
{
  "source":{
  "index":"teste1"
},
  "dest":{
  "index":"new_teste"
  }
}
```
