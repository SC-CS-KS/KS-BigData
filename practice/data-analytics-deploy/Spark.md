# Deploy Spark

* Version 2.4.0
* [Download](http://spark.apache.org/downloads.html)

## Config
***spark-env.sh***
```sh
$ cp spark-env.sh.template spark-env.sh
```
```md
export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk.x86_64/
export HADOOP_CONF_DIR=/home/hadoop/hadoop/etc/hadoop
export YARN_CONF_DIR=/home/hadoop/hadoop/etc/hadoop
export SPARK_MASTER_PORT=7077
```
```sh
$ cp slaves.template slaves
```
```md
bigdata-01
bigdata-02
```
***HistoryJobServer***
```md

```
***查看配置***
```md
可以通过web界面http://hostname:4040中 的Environment查看Spark配置信息，
仅显示spark-defaults.conf、SparkConf和命令行参数，可以根据web页面确定配置属性是否生效。
```
## Start
* Spark on YARN
```md
$ spark-shell --master yarn-client
```
* Local[N] 
```md
Spark单机运行，常用于本地开发测试，使用N个线程。

$ bin/spark-shell
```
* Local Cluster
```md
伪分布模式，可配置所需的work，core，memory。

$ spark-shell --master local 
```
* Standalone
```md
即独立模式，自带完整的服务，可单独部署到一个集群中，无需依赖任何其他资源管理系统。

$ spark-shell --master spark://bigdata-02:7077
```
* Spark on Mesos

## Web Interfaces
* http://10.96.114.47:4040

## Test
```sh
$	./bin/run-example SparkPi 2>&1 | grep "Pi is roughly"
Pi is roughly 3.1406557032785165
```
```sh
$ bin/spark-submit examples/src/main/python/pi.py 2>&1 | grep "Pi is roughly"
Pi is roughly 3.141040
```
***bin/spark-shell***
```sh
$ bin/spark-shell
```
```scala
scala> var textFile = sc.textFile("file:/home/hadoop/spark/README.md")
var textFile = sc.textFile("file:/home/hadoop/spark/README.md")
textFile: org.apache.spark.rdd.RDD[String] = file:/home/hadoop/spark/README.md MapPartitionsRDD[5] at textFile at <console>:24

scala> textFile.count()
textFile.count()
res3: Long = 105

scala> textFile.first()
textFile.first()
res4: String = # Apache Spark
```
***Spark on YARN***
```sh
$ bin/spark-shell --master yarn-client
```
```scala
scala> sc.textFile("hdfs://10.96.111.130:9000/wordcountdemo/input/wc.input").flatMap(_.split(" ")).filter(!_.isEmpty).map((_,1)).reduceByKey(_+_).collect().foreach(println)
(hive,2)
(mapreduce,1)
(sqoop,1)
(spark,2)
(hadoop,3)
(storm,1)
(hbase,1)
```
