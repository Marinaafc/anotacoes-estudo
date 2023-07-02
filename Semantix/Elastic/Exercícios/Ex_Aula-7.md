### 1. Enviar o arquivo <local>/paris-925.logs  para o Logstash, com uso do Filebeat.
```
cd filebeat-7.9.2-linux-x86_64/
```
```
vi filebeat.yml
```
Antes:
![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/bfe18d58-52d3-43cf-bf99-090f35c44ebc)
Depois:
![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/4fc4173d-d667-45df-9738-0ce6bb46435a)


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
### Resposta:
```
cd ../
```
```
vi pipeline/logstash.conf
```
- Mudar "testes" para seu-nome
```
docker-compose down
```
```
docker-compose up -d
```
- Precisa inicializar o beats para o logstash poder receber os dados
```
cd filebeat-7.9.2-linux-x86_64/
```
```
./filebeat test config
```
```
./filebeat test output
```
- Precisa remover as pastas data e logs para poder enviar o arquivo paris novamente
```
rm -rf data/
```
```
rm -rf logs/
```
```
./filebeat -e
```
### 3. Verificar a quantidade de documentos do índice criado pelo Logstash e visualizar seus 10 primeiros documentos
```
GET marina-2023.07.02/_count
```
```
GET marina-2023.07.02/_search
```
