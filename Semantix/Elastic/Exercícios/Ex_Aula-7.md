### 1. Enviar o arquivo <local>/paris-925.logs  para o Logstash, com uso do Filebeat.

### 2. Configurar e executar o logstash com as seguintes configurações
```
Entrada:
beats {
            port => 5044

        }

Saída:
elasticsearch {
            hosts => [ “elasticsearch:9200" ]

            index => “seu_nome-%{+YYYY.MM.dd}"

        }
```
### 3. Verificar a quantidade de documentos do índice criado pelo Logstash e visualizar seus 10 primeiros documentos
