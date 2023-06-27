# Família de Beats (Ingestão de Dados)

- Existem vários tipos de Beats que podem ser utilizados;
- Os Beats enviam dados de centenas ou milhares de máquinas e sistemas para o:
  - Logstash
  - ou o Elasticsearch
- Dentro do Logstash é possível fazer algumas transformações a mais que o Beat não consegue;

- Tipos de Famílias de Beats:
  - **Filebeat**
    - Arquivos de log
  - **Metricbeat**
    - Métricas
    - Ex: pega medidas quantitativas - quanto de cpu, ram está sendo usada em uma aplicação
  - **Packetbeat**
    - Dados de rede
  - **Winlogbeat**
    - Logs de evento do Windows
  - **Auditbeat**
    - Dados de auditoria
  - **Heartbeat**
    - Monitoramento de disponibilidade
  - **Functionbeat**
    - Agente de envio sem servidor
    - Ex: Vai pegar algum serviço de cloud e fazer o monitoramento

# Filebeat

### Conceitos
- O filebeat serve para fazer leitura de logs;
  - Tanto de 0, 1 ou vários arquivos de log
- Quando se faz a leitura de log, é possível escolher um módulo;
  - Existem vários módulos de Filebeats
#### O que são esses módulos?
- São os arquivos de configuração para fazer leitura de uma aplicação específica

### Instalação e Configuração
- É preciso sempre verificar a versão do Elasticsearch que está sendo utilizada para poder instalar o beat com a versão correspondente;

- Usar módulo de comunicação
```
$ ./filebeat modules list
```
> - Mostra todos os módulos que o fillebeat consegue se comunicar
```
$ ./filebeat modules enable <módulo>
```
> - Escolhe qual módulo deseja utilizar
- Testar Beat
```
$ ./filebeat test config
```
> - Mostra se tem erros na configuração do arquivo de configuração
> - Ex: Espaço a mais, tab sem necessidade, ou apóstrofo errado, etc
```
$ ./filebeat test output
```
> - Testa a conexão. Vai ver se está fazendo a comunicação com o beat, com o elasticsearch, etc

### Configuração

- Configurar o arquivo: filebeat.yml
