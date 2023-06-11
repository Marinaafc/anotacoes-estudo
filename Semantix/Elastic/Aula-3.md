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
