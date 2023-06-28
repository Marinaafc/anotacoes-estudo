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
- *Versão do curso: 7.9.2*
- Documentação da Elastic: https://www.elastic.co/guide/en/beats/filebeat/master/filebeat-getting-started.html

- Download
```
$ curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.9.2-linux-x86_64.tar.gz
```
- Descompactar
```
$ tar xzvf filebeat-7.9.2-linux-x86_64.tar.gz
```
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

### Inicialização

- Inicializar de modo simples
```
$ chown root filebeat.yml
```
```
$ chown root modules.d/system.yml
```
> - Apenas para os módulos habilitados
```
$ ./filebeat –e
```
> - Exibir a configuração e saída do beat

- Inicializar como serviço
  - $ service filebeat start
  - $ service filebeat status
  - $ service filebeat stop
  - $ service filebeat restart

# Metricbeat

### Conceitos
- Módulos de Metricbeat
  - Fazem leitura de medidas quantitativas (quanto de cpu/ram/armazenamento/temperatura está gastando no serviço)
 
### Instalação e Configuração
- *Versão do curso: 7.9.2*
- Documentação da Elastic: https://www.elastic.co/guide/en/beats/metricbeat/master/metricbeat-getting-started.html

- Download
```
$ curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.9.2-linux-x86_64.tar.gz
```
- Descompactar
```
$ tar xzvf metricbeat-7.9.2-linux-x86_64.tar.gz
```
- Usar módulo de comunicação
```
$ ./metricbeat modules list
```
```
$ ./metricbeat modules enable <módulo>
```
- Testar Beat
```
$ ./metricbeat test config
```
```
$ ./metricbeat test output
```
### Configuração

- Configurar o arquivo: metricbeat.yml

### Inicialização

- Inicializar de modo simples
```
$ chown root metricbeat.yml
```
```
$ chown root modules.d/system.yml
```
> - Apenas para os módulos habilitados
```
$ ./metricbeat –e
```
> - Exibir a configuração e saída do beat

- Inicializar como serviço
  - $ service metricbeat start
  - $ service metricbeat status
  - $ service metricbeat stop
  - $ service metricbeat restart
 
# Heartbeat

### Conceitos
- Não tem módulo;
- Utilizado para ver monitoramento e disponibilidade de algum link;
- Pings via:
  - ICMP
  - TCP
  - HTTP,
- Configuração
  - TLS
  - Autenticação
  - Proxies

### Instalação e Configuração
- *Versão do curso: 7.9.2*
- Documentação da Elastic: https://www.elastic.co/guide/en/beats/heartbeat/master/heartbeat-installation-configuration.html

- Download
```
$ curl -L -O https://artifacts.elastic.co/downloads/beats/heartbeat/heartbeat-7.9.2-linux-x86_64.tar.gz
```
- Descompactar
```
$ tar xzvf heartbeat-7.9.2-linux-x86_64.tar.gz
```
- Testar Beat
```
$ ./heartbeat test config
```
```
$ ./heartbeat test output
```
### Configuração

- Configurar o arquivo: heartbeat.yml

### Inicialização

- Inicializar de modo simples
```
$ chown root heartbeat.yml
```
```
$ chown root modules.d/system.yml
```
> - Apenas para os módulos habilitados
```
$ ./heartbeat –e
```
> - Exibir a configuração e saída do beat

- Inicializar como serviço
  - $ service heartbeat start
  - $ service heartbeat status
  - $ service heartbeat stop
  - $ service heartbeat restart


