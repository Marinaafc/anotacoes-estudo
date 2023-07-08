# RDD - Conceitos

- Primeiro tipo de formato de dados do Spark;
- A partir dele que surgiu o DataFrame e o DataSet;
- A usabilidade não é tão simples quanto a do DataFrame e do DataSet;

- **R**esilient **D**istributed **D**atasets
  - **R**esiliente - Recriar dado perdido na memória *(usa memória RAM)*
  - **D**istribuído - Processamento no cluster *(modo client: o nó executa o projeto; modo cluster: é executado em outro nó e é feito de forma distribuída)*
  - **D**atasets - Dados podem ser criados ou vir de fontes
 
- Resumidamente, o RDD seria uma coleção de objetos distribuídos entre os nós do cluster
  - Armazena os dados em partições
- Imutáveis
- Tipos de operações
  - Ação
  - Transformação
