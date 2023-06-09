# Arquitetura Elastic
### Elasticsearch 
- Também é um banco de dados, mas o foco principal do Elastic não é simplesmente armazenar o dado, mas sim bucar o dado;  
- É um banco de dados orientado a documento;
- O foco do Elastic é ser um motor de busca eficiente;
### Logstash 
Transporte entre a origem e destino. Ou seja, sempre que quiser enviar um dado para salvar no Elasticsearch, vai ser usado o Logstash para isso. Ele vai pegar o dado, enviar armazenar no Elasticsearch;  
### Beats 
- Também é possível usar o Beats para fazer isso que o Logstash faz;  
- O Beats fica do lado do cliente e não do servidor;  
- É mais leve para coletar os dados, mas o Logstash tem mais opções;  
- Tanto o Beats quanto o Logstash são para enviar informações para o Elasticsearch.  
### Kibana
- É a interface gráfica do Elastic;  
- Não é uma interface só para visualizar os dados, nele tem todo o gerenciamento do Cluster também.

# Banco Relacional x ElasticSearch

Banco de dados = Index    
~~Tabela = Type~~  
Schema = Mapping  
Registro (linha) = Documento  
Coluna = Atributo  

- Na versão 5 do Elastic, era possível criar vários tipos de dados, como a tabela;  
- Mas, a partir da versão 7 do Elastic, os documentos são todos do tipo _doc;  
- Aconteceu isso, pois para passar dados de um banco relacional para o Elasticsearch, que é um banco não relacional, as pessoas pegavam as tabelas e criavam um Type de mesmo nome, o que não era a ideia do Elastic;  
- Na versão 6, todo index só podia ter um type;  

# Índice (= Banco de Dados)

- É onde vão ficar todas as informações;  
- É dividido por **Shards** e é nos Shards que os dados ficarão armazenados;  
### Alias
- É um link virtual para um índice real (apelido);
- É possível associar um Alias a mais de um índice (grupo);
### Analyzer
- Busca por Full Text e Valores Exatos;
### Mapping (= Schema)
- Definição da estrutura do índice;
