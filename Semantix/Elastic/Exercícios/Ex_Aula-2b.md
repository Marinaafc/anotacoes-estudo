### 1. Importar os dados na Guia Arquivos para os índices

Índice: concessionaria2
dataset/cars.bulk
Índice: populacao
dataset/populacaoLA.csv
### 2. Executar as consultas

Contar o número de documentos de cada um dos novos índices
```
GET concessionaria2/_count
GET populacao/_count
```
Mostrar todos os documentos de cada um dos novos índices
```
GET concessionaria2/_search
GET populacao/_search
```
