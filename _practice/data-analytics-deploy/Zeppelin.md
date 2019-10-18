# Zeppelin Deploy
* Version 0.8.1

* Requirements
```md
OS ：	Mac OSX，Ubuntu 14.X，CentOS 6.X，Windows 7 Pro SP1
```
***注意：关于 JDK版本***
```md 
JDK ：官网标注的是 JDK 1.7
我在使用 JDK 1.7 启动时，报以下错误：
Exception in thread "main" java.lang.UnsupportedClassVersionError: 
javax/ws/rs/core/Application : Unsupported major.minor version 52.0

更改为 JDK 1.8 启动正常，个人建议使用 JDK 1.8
```
## [Download](http://zeppelin.apache.org/download.html)

## Config
***Web端口***
```md
默认端口 8080，可以修改：
zeppelin/conf/zeepelin-site.xml 
zeppelin.server.port 配置项
```
## Start
```shell
bin/zeppelin-daemon.sh start
bin/zeppelin-daemon.sh stop
```
## Web Interfaces
```md
http://10.96.111.130:8080
```

## [Spark Interpreter](https://zeppelin.apache.org/docs/0.8.1/interpreter/spark.html)

* Modifying Interpreter Settings
> * Export SPARK_HOME
***zeppelin-env.sh***
```md
export SPARK_HOME=/home/hadoop/spark
export HADOOP_CONF_DIR=/home/hadoop/hadoop/etc/hadoop
```

> * Set master in Interpreter menu
```md 
Zeppelin Web 页，点击右上角菜单的 Interpreter。
设置 Spark master 地址为 yarn-client
设置 HDFS 地址 : hdfs://10.96.111.130:8020
```
> * Yarn mode
```md
Zeppelin support both yarn client and yarn cluster mode (yarn cluster mode is supported from 0.8.0).
For yarn mode, you must specify SPARK_HOME & HADOOP_CONF_DIR. 
You can either specify them in zeppelin-env.sh, or in interpreter setting page. 
Specifying them in zeppelin-env.sh means you can use only one version of spark & hadoop. 
Specifying them in interpreter setting page means you can use multiple versions of spark & hadoop in one zeppelin instance.
```
## Test
```md
新建 Note，绑定 Spark Interpreter。
```
```scala
sc.version
 res2: String = 2.4.0
```
```scala
sc.textFile("hdfs://10.96.111.130:9000/wordcountdemo/input/wc.input").flatMap(_.split(" ")).filter(!_.isEmpty).map((_,1)).reduceByKey(_+_).collect().foreach(println)
(hive,2)
(mapreduce,1)
(sqoop,1)
(spark,2)
(hadoop,3)
(storm,1)
(hbase,1)
```
***可视化展示***
* 数据
```md
$ cat test/ipdata_code_with_maxmind.txt
3757755392,3757834239,CN,310000,310100,-1,100023,31.0456,121.4
3757834240,3757835775,AU,AU_04,Brisbane,-1,3000683,-27.4679,153.028
3757835776,3757836031,AU,AU_04,Brisbane,-1,3000683,-27.4833,153.042
```
* 上传 HDFS
```sh
$ hadoop dfs -put test/ipdata_code_with_maxmind.txt  /wordcountdemo/input/
```
* 创建临时表
```scala
import sqlContext.implicits._

val ip2region = sc.textFile("hdfs://10.96.111.130:9000/wordcountdemo/input/ipdata_code_with_maxmind.txt")

case class Ip(ipstart:Long, ipend:Long, country:String, province: String, city : String, county: String)

val ip_map = ip2region.map(s=>s.split(",")).filter(s=>(s.length==9)).map(
s=>Ip(s(0).toLong,
s(1).toLong, s(2), s(3), s(4), s(5))
).toDF()

ip_map.registerTempTable("ip2region")
```
* 查询
```sql
%sql
select distinct country, count(1) as sum from ip2region group by country order by country
```
```md
默认展示表格
```

* [官网实例](https://zeppelin.apache.org/docs/0.8.0/quickstart/tutorial.html)
