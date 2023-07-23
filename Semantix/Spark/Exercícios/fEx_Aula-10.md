Para as seguintes máquinas:

**a) 20 nodes x 8 cores | 16Gb RAM**
driver-memory 8GB
--executor-memory ((20*16)-10%)/39 = 8GB
--conf spark.yarn.driver.memoryOverhead 20*16*0,1*0,1 = 3G
--conf spark.yarn.executor.memoryOverhead 20*8*0,1*0,1 = 2
--executors-core 4
--num-executors ((20*8)-10%)/4 = 39
**b) 9 nodes x 20 cores | 128Gb RAM**
driver-memory 8GB
--executor-memory ((9*128)-10%)/44 = 25G
--conf spark.yarn.driver.memoryOverhead 9*128*0,1*0,1 = 12G
--conf spark.yarn.executor.memoryOverhead 9*20*0,1*0,1 = 2
--executors-core 4
--num-executors ((9*20)-10%)/4 = 44
**c) 6 nodes x 10 cores | 32Gb RAM – 3 Jobs em paralelo**
driver-memory 8GB
--executor-memory (((6*32)-10%)/4)/3 = 15G
--conf spark.yarn.driver.memoryOverhead 6*32*0,1*0,1 = 2G
--conf spark.yarn.executor.memoryOverhead 6*10*0,1*0,1 = 1
--executors-core 4
--num-executors (((6*10)-10%)/4)/3 = 4
**d) 10 nodes x 16 cores | 64Gb RAM – 50 % dos recursos já utilizados
driver-memory 8GB**
--executor-memory (((10*64)-10%)/19)/2 = 16G
--conf spark.yarn.driver.memoryOverhead 10*64*0,1*0,1 = 6G
--conf spark.yarn.executor.memoryOverhead 10*16*0,1*0,1 = 2
--executors-core 4
--num-executors (((10*16)-10%)/4)/2 = 19
Configurar os atributos:

driver-memory 8GB
--executor-memory 
--conf spark.yarn.driver.memoryOverhead 
--conf spark.yarn.executor.memoryOverhead 
--executors-core 4
--num-executors 
