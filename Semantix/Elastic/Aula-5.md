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

- Analyzer Standard ou Simples

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

- Analyzer whitespace
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

- Analyzer em português
  - Como tem menos tokens, a busca é mais rápida, porque tem menos coisa para buscar.
```json
POST _analyze
{
  "analyzer":"brazillian",
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

- Analyzer English

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