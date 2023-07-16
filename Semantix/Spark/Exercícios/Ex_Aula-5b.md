# Exercícios - WithColumn 2/2
### 1. Criar um dataframe para ler o arquivo no HDFS /user/<nome/data/juros_selic/juros_selic
### 2. Agrupar todas as datas pelo ano em ordem decrescente e salvar a quantidade de meses ocorridos, o valor médio, mínimo e máximo do campo valor com a seguinte estrutura:

Anual | MesesValor | médioValor | MínimoValor | Máximo
---------| ----------|------------|---------|----
2019 | 2 | 00.00 | 00.00 | 00.00 
2018 | 12 | 00.00 | 00.00 | 00.00
2017 | 12 | 00.00 | 00.00 | 00.00
... | ... | 00.00 | 00.00 | 00.00 
1986 | 2 | 00.00 | 00.00 | 00.00
### 3. Salvar no hdfs:///user/<nome>/relatorioAnual com compressão zlib e formato orc
