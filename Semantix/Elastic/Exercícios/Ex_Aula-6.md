# Filebeat

### Instalação e Configuração - Passo a Passo:

1. curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.9.2-linux-x86_64.tar.gz
2. tar xzvf filebeat-7.9.2-linux-x86_64.tar.gz
3. cd filebeat-7.9.2-linux-x86_64/
4. ls
5. ./filebeat modules list
6. cd ../

### 1. Enviar o arquivo <local>/paris-925.logs com uso do Filebeat

7. cd dataset (repositorio "dataset" foi criado por mim em root para colocar o arquivo "paris-925.logs")
8. ls
9. pwd (para ver o caminho até o arquivo "paris-925.logs")
10. vi ../filebeat-7.9.2-linux-x86_64/filebeat.yml paris-925.logs
11. Editar (clicando em "i") para ficar da seguinte forma:
12. ![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/be697487-37fe-45b8-b887-13d77a7ccbe8)
(Em "path" vai ter a localização copiada no passo anterior)
13. Após, clicar em "ESC" depois em ":", depois escrever "wq" e pressionar enter
14. Usar o docker-compose up -d
15. cd ../filebeat-7.9.2-linux-x86_64/
16. ./filebeat test config
17. ./filebeat test output
18. ./filebeat -e


### 2. Verificar a quantidade de documentos do índice criado pelo Filebeat e visualizar seus 10 primeiros documentos
No Kibana:
```
GET filebeat-7.9.2-2023.06.28-000001/_count
```
```
GET filebeat-7.9.2-2023.06.28-000001/_search
```

# Metricbeat

- Download
```
$ curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.9.2-linux-x86_64.tar.gz
```
- Descompactar
```
$ tar xzvf metricbeat-7.9.2-linux-x86_64.tar.gz
```
### 3. Monitorar as métricas do docker

Referência:
https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-module-docker.html (Links para um site externo.)
```
cd metricbeat-7.9.2-linux-x86_64/
```
```
./metricbeat modules list
```
```
./metricbeat modules enable docker
```
```
./metricbeat test config
```
```
./metricbeat test output
```
> - OBS: Pro "metricbeat test output" funcionar, é preciso usar o docker-compose start ou up -d antes e verificar se todos os containers estão rodando.

Encontrar o socket do Docker
$ sudo find / -name docker.sock

```
cd modules.d
```
```
cat docker.yml
```
```
find / -name docker.sock
```
```
copiar o caminho que aparecer, no caso: "/mnt/wsl/docker-desktop/shared-sockets/guest-services/docker.sock"
```
> - É um caminho diferente do que normalmente vem configurado, porque está se usando wsl e não só o linux. Portanto, deve-se alterar essa informação no arquivo de configuração
```
vi docker.yml
```
> - Deve-se na edição, além de mudar o caminho pro docker.sock, habilitar as métricas que deseja monitorar, tirando o "#" da frente;
> - Precisa tirar a "#" da frente do "metricsets", colocar "enabled:true" embaixo de "hosts" e colocar "unix://" na frente do caminho do lado de "hosts";
> - Sempre que fizer uma alteração no arquivo de configuração, é bom sempre utilizar o "./metricbeat test config" e o "./metricbeat test output".

```
cat metricbeat.yml
```
> - Só para dar uma olhada nas configurações.
```
./metricbeat -e
```

### 4. Verificar a quantidade de documentos do índice criado pelo Metricbeat e visualizar seus 10 primeiros documentos
```
GET metricbeat-7.9.2-2023.06.28-000001/_count
```
```
GET metricbeat-7.9.2-2023.06.28-000001/_search
```

# Heartbeat

- Download
```
$ curl -L -O https://artifacts.elastic.co/downloads/beats/heartbeat/heartbeat-7.9.2-linux-x86_64.tar.gz
```
- Descompactar
```
$ tar xzvf heartbeat-7.9.2-linux-x86_64.tar.gz
```

### 5. Monitorar o site https://www.elastic.co/pt/ (Links para um site externo.) com uso do Heartbeat
```
cd heartbeat-7.9.2-linux-x86_64/
```
```
vi heartbeat.yml
```
- Mudar a "urls" para https://www.elastic.co/pt/
```
./heartbeat test config
```
```
./heartbeat test output
```
```
chown root heartbeat.yml
```
```
./heartbeat -e
```
### 6. Verificar a quantidade de documentos do índice criado pelo Heartbeat e visualizar seus 10 primeiros documentos
```
GET heartbeat-7.9.2-2023.06.29-000001/_count
```
```
GET heartbeat-7.9.2-2023.06.29-000001/_search
```
