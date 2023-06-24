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