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
```
input{
}
filter{
}
output{
}
```

### Instalação e Configuração

- Estrutura das pastas
- docker-compose.yml
- pipeline/logstash.conf
- settings
  - elasticsearch.yml
  - kibana.yml
  - logstash.yml
 
- Os beats vão enviar os dados para o Logstash através da porta 5044:5044;

### Configuração

- pipeline/logstash.conf
```
input{
    beats{
    port => 5044
    }
}
output{
    stdout{
    codec => "json"
    }
    elasticsearch{
          hosts=>["elasticsearch:9200"]
    }
}
```

- settings/logstash.yml

```
http.host:"0.0.0.0"
xpack.monitoring.elasticsearch.hosts:[
"http://elasticsearch:9200"]
```
# Plugins de Entrada

- É o que vai permitir que uma fonte específica de eventos seja lida pelo Logstash;
- Vai apontar qual entrada de dados o Logstash está lendo;
- Plugins de entrada:
  - ![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/eca40ed3-8c8b-4618-baf2-b22ebd4e5faf)

### Plugin Entrada - Exemplo
- pipeline/logstash.conf

```
input {
  file {
    id => "test_log_sem_gz"
    path => "/var/log/*.log"
    exclude => "*.gz"
  }
}
```
> - "id" nome que vai dar ao pipeline;
> - "path" vai ler todo mundo que estiver no diretório especificado;
> - "exclude" não vai fazer a leitura de quem for "*.gz".

# Plugins de Saída

- Permite o envio de dados de evento para um destino específico;
- Plugins de saída:
  - ![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/fc8526bf-df99-4bb1-a179-d418d7d75d6d)
> - **stdout** - vai exibir informações pelo próprio terminal

### Plugin Saída - Exemplo
- pipeline/logstash.conf
```
output {
  stdout {
    codec => json
  }
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "testes-%{+YYYY.MM.dd}"
  }
}
```
> - saída do tipo json pelo próprio elasticsearch
