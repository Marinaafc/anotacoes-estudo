# Logstash - Conceitos

- O Logstash é muito parecido com o Beats.
  - Beats:
    - Origem dos dados: Várias fontes;
    - Dados são enviados para o Elasticsearch ou para o Logstash.
  - Logstash:
    - É um serviço em que é feito o mesmo processo do Beats;
    - Dados de várias fontes enviados ao Elasticsearch...;
    - ... para analisar, amarzenar, monitorar ou para fazer a criação de alertas.
- A grande diferença entre o Logstash e o Beats é que o primeiro tem plugins divididos em 3 camadas:
  - Plugin:
    - Input - Entrada de dados
    - Filter - Transformações nos dados
    - Output - Para qual saída os dados serão enviados
