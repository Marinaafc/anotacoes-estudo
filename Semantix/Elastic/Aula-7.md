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

- Boa prática:
  - Enviar os Beats para o Logstash, onde irá passar pelo Logstash Pipeline, em que terá a entrada dos dados, as transformações e saída que geralmente é para o Elasticsearch;
  - Os Beats não têm Filter, ou seja, não é possível fazer transformações nos dados;
  - É possível até fazer algumas modificações em alguns Beats, mas é muito limitado. Por isso até que o Beats é bem mais leve que o Logstash;
  - Sempre é uma boa prática o Logstash ser o centralizador;
  - Ex: 1.000 serviços vão enviar dados para o Elasticsearch, não vai ser usado 1.000 Logstashs e sim 1.000 Beats e esses Beats vão enviar para 1 ou mais Logstashs que vão ser os centralizadores;
  - Esse(s) Logstash(s) vão enviar os dados para o Elasticsearch ou outro lugar.

- Para fazer o pipeline (Inputs > Filters > Outputs), precisa configurar o arquivo "**logstash.conf**";
 
- Estrutura do json
```json
input{
}
filter{
}
output{
}
```
