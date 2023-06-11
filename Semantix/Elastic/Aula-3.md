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
