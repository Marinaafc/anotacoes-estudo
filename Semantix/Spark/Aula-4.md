# DataFrame - Criação de Schemas
## Tipagem de Dados

- Spark SQL e DataFrame
  - ByteType
  - ShortType
  - IntegerType
  - LongType
  - FloatType
  - DoubleType
  - DecimalType
  - BigDecimal
  - StringType
  - BinaryType
  - BooleanType
  - TimestampType
  - DateType
  - ArrayType
  - MapType
  - StructType
 
- Se der algum erro com o Type, pode ser porque teve alguma mudança na versão.

## Esquemas - Definição
### Scala
- Inferir esquema manualmente em dados com cabeçalho;
- Automaticamente, sempre vai colocar o tipo de dado para cima (Ex: nº: no lugar de colocar integer vai colocar long; float coloca como double);
- Arquivo:

| setor.csv |
| ------ |
| id, setor |
| 1, vendas |
| 2, TI |
| 3, RH |
```scala
scala>
```
