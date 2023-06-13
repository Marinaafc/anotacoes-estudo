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
