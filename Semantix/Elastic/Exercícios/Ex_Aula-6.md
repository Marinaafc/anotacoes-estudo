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

### 3. Monitorar as métricas do docker

Referência:
https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-module-docker.html (Links para um site externo.)

Encontrar o socket do Docker
$ sudo find / -name docker.sock

### 4. Verificar a quantidade de documentos do índice criado pelo Metricbeat e visualizar seus 10 primeiros documentos

