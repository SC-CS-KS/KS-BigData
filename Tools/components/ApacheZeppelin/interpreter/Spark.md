# [Spark Interpreter](https://zeppelin.apache.org/docs/0.8.0/interpreter/spark.html)
```md
Spark Interpreter实现包括4个不同的解释器：
Spark，SparkSQL，Pyspark和SparkR。
```
## SparkContext, SQLContext, SparkSession, ZeppelinContext
```md
在Scala，Python和R环境中，SparkContext，SQLContext和ZeppelinContext分别自动创建并作为变量名sc，sqlContext和z。
从0.6.1开始SparkSession在使用Spark 2.x时可用作变量spark。

注意，Scala / Python / R环境共享相同的SparkContext，SQLContext和ZeppelinContext实例
```
***如何将属性传递给SparkConf***
```md
有两种属性可以传递给SparkConf
Standard spark property (prefix with spark.). e.g. spark.executor.memory will be passed to SparkConf
Non-standard spark property (prefix with zeppelin.spark.). e.g. zeppelin.spark.property_1, property_1 will be passed to SparkConf
```
## Dependency Management
* 1. Setting Dependencies via Interpreter Setting

* 2. Loading Spark Properties

* 3. Dynamic Dependency Loading via %spark.dep interpreter

## ZeppelinContext
```md
Zeppelin在Scala / Python环境中自动将ZeppelinContext注入变量z。
ZeppelinContext 提供了一些额外的功能和实用程序。
```

## Matplotlib Integration (pyspark)

## Reference
* [Spark Interpreter for Apache Zeppelin](https://zeppelin.apache.org/docs/0.8.1/interpreter/spark.html)