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
  
