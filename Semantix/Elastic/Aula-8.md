# Menu Kibana

### Guia Discover

- Acessar, pesquisar e filtrar dados do índice selecionado;
- Detalhes de campos da pesquisa;
- Salvar pesquisas para usar no discover, visualizações e dashboards.

### Guia Visualize

- Criar, editar e salvar visualizações dos dados (gráficos)
  - Consultas
  - Filtros
  - Agregações
 
- Faz uso de Keywords, pois usa a palavra inteira
- Lens - Dá várias opções de gráficos, depois que informa o eixo x e y, quando não se sabe qual escolher

### Guia Dashboard

- Comibinar várias visualizações de dados em um único lugar

### Guia Canvas

- Visualização e apresentação de dados
  - Páginas
  - Combinação
    - Cores
    - Imagens
    - Texto
   
### Guia Maps

- Analisar dados geográfico em tempo real
  - Mapas com várias camadas e índices
  - Carregar arquivos GeoJSON

### Guia Machine Learning
- Gerenciaomento de jobs
  - Detector de anomalias (não tem pra versão basic que está sendo usada)
- Carregamento de dados

# Menu Enterprise Search

- Criar um campo de pesquisa em sites

### Guia App Search

- Pacotes
  - App Search
    - Fornecer ferramentas para projetar e implantar uma pesquisa poderosa em seus sites e aplicativos móveis
   
  - Workplace Search
    - Unificar suas plataformas de conteúdo (Google Drive, Salesforce) em uma experiência de pesquisa **personalizada**

# Menu Observability

- Associado aos Beats (vai mostrar os logs que foram enviados)

### Guia Logs
- Logs em tempo real
  - Filebeat (só vai aparecer quando inicializar o filebeat)
  - Visualizar campos dos logs
  - Visualizar no Uptime

### Guia Metrics
- Métricas em tempo real
  - Metricbeat (só vai aparecer quando inicializar o metricbeat, assim como todos os beats)
 
### Guia APM
- Application Performance Monitoring
- Permite monitorar o desempenho de milhares de aplicativos em tempo real

### Guia Uptime
- Monitoramento em tempo real
  - Heartbeat (pode ver o filebeat nessa guia também)
 
# Menu Security

### Guia Overview - Security

- SIEM (era chamada assim antes)
  - Security Information & Event Management
- Análise de eventos de segurança relacionados ao host e à rede
  - Investigação de alertas
  - Procura de Ameaças
 
# Menu Management

### Guia Dev Tools

- Digitar request e enviá-las ao Elasticsearch

### Guia Stack Monitoring

- Monitorar
  - Armazenamento
    - Disco
    - Memória
    - Heap
  - Saúde
    - Shards Primários
    - Shards Réplicas
-  Pode monitorar o Kibana pelo Metricbeat também, alterando o arquivo de configuração. Mas é mais fácil monitorar localmente, habilitando o Expec.monitoring quando o Kibana solicitar

### Guia Management
