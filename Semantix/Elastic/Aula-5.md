# Analyzer

### Introdução 

Existem 2 tipos de buscas possíveis de fazer no Elastic:

- Busca exata
  - Sim e não

- Busca FullText
  - Busca o tão quanto a palavra casa (_score)
  - Quando se faz uma busca no FullText, pode-se aplicar um **Analisador**
  - Com esse analisador é possível melhorar a forma com que se vai fazer a busca
    - É possível **testar** o analisador com várias sentenças de palavras para adaptá-lo melhor para o caso de uso;
    - Depois 