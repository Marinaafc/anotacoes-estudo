# Apresentação do módulo de Data Application
- Módulo falará sobre o que fazer no dia a dia de construção de aplicações Big Data;
- Essa fase abordará:
  - O mínimo de conhecimento que o desenvolvedir deve ter para trabalhar com Spark;
  - As entregas que devem ser feitas ao cliente;
  - Os templates de projetos da semantix.
 
# Mínimo de Conhecimento
- 100% dos engenheiros vão precisar lidar com código;
- Código deve ser feito para ser lido por pessoas;
  - Colocar "a", "b", "c" ou "x", "y" não fica legível para manutenção de código;
  - Importante ter documentação do que o método faz para que as pessoas não fiquem muito tempo tentando entender o código.
 
### 1. Entender a história da linguagem e suas convenções
- ex:
  - java, scala: variavelEmJava (camel case)/ métodos com primeira letra maiúscula;
  - python variavel_em_python (snake case)/ métodos em snake case;
  - formatação (ferramenta para verificar a formatação): Java: checkstyle; Python:pep8.
   
### 2. Entender as características da linguagem
- ex:
  - Java, Scala: orientação a objeto para aproveitar o máximo do java;
  - Python, Scala: linguagem funcional (método, função anônima, etc).
 
### 3. Estudar e aprender com bons profissionais em texto, blog, etc
- ex:
  - S.O.L.I.D. - método para melhorar a legibilidade do código.

# Deploy
### Entregas
- Os artefatos que são gerados para os clientes normalmente são divididos em 4 tipos de arquivos:
  - Scripts
  - JAR
  - Zip
  - .egg
 
- Agendamento
  - crontab, control-M, ferramentas na nuvem, no cloudera (apache oozie), apache airflow
 
- Deploy
  - de arquivos: de uma máquina pra outra;
  - abrir chamados: GMUD;
  - devops bem estruturado: pipeline, validando o que fez.
  - ![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/1df6685f-8399-40b5-a797-a6318581c667)

# Templates
- Camada Raw - dados sem tratamento (do jeito que o arquivo veio);
- Transformações;
- Modelo baseado em serviços (carregar e gravar o dado no data lake):
- ![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/f9971755-08aa-417e-9a1e-9a8cead87768)
- modelo baseado no ciclo de vida do dado (fluxo das zonas mostrado acima):
- ![image](https://github.com/Marinaafc/anotacoes-estudo/assets/107056644/f864c5d3-be53-4747-ae82-9a603d3ebdb1)
