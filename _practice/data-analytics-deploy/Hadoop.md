# Hadoop Deploy
## 系统准备
```md
部署 user : hadoop
路径 ：/home/hadoop
HADOOP_HOME : /home/hadoop/hadoop
```
* 配置 SSH 免密登录
```md
[hadoop@bigdata-01 ~]$ ssh-keygen -t rsa
[hadoop@bigdata-01 ~]$ ssh-copy-id bigdata-02
[hadoop@bigdata-02 ~]$ ssh-keygen -t rsa
[hadoop@bigdata-02 ~]$ ssh-copy-id bigdata-01
```
```md
[hadoop@bigdata-02 ~]$ ssh-keygen -t rsa
[hadoop@bigdata-02 ~]$ ssh-copy-id bigdata-01
[hadoop@bigdata-01 ~]$ ssh-keygen -t rsa
[hadoop@bigdata-01 ~]$ ssh-copy-id bigdata-02
```
## Hadoop 模块（服务）
* HDFS 分布式存储方式
* YARN 通用的资源协同和任务调度框架
* MapReduce 计算框架
* JobHistoryServer 历史服务-查看Yarn上执行job情况的详细信息 

## Deployment Mode
* [Local (Standalone) Mode](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/SingleCluster.html#Standalone_Operation)
```md
  默认情况下，Hadoop配置为以非分布式模式运行，作为单个Java进程。对调试很有用。
```
* [Pseudo-Distributed Operation (伪分布式模式)](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/SingleCluster.html#Pseudo-Distributed_Operation)
```md
Hadoop 也可以在伪分布式模式下在单节点上运行，其中每个 Hadoop 实例 在单独的Java进程中运行。
```
* [Fully-Distributed Mode (Hadoop Cluster)](https://hadoop.apache.org/docs/r3.2.0/hadoop-project-dist/hadoop-common/ClusterSetup.html)
```md
Hadoop集群范围从几个节点到具有数千个节点的极大集群。
```
> * HA Mode
```md
  Hadoop HA是Hadoop 2.x中新添加的特性，包括NameNode HA 和 ResourceManager HA。
  HA 组件：时间服务器，Zookeeper
  解决 单NameNode的缺陷存在单点故障的问题。
```
> * Secure Mode

```md
作为测试环境，又为了保证能验证 Hadoop 的完整特性，我们选用 两台 机器 搭建 最小的 Hadoop 分布式集群。
暂不考虑 Security 和 High Availability。
```
## Version [3.2.0](https://hadoop.apache.org/docs/r3.2.0/)

## Requirements
> * [Hadoop Java Versions](https://wiki.apache.org/hadoop/HadoopJavaVersions)
```md
2.7及更高版本需要Java 7，它在 OpenJDK 和 Oracle（HotSpot）的 JDK / JRE 上构建和测试。
早期版本（2.6及更早版本）支持Java 6。
```
> * SSH
```md
必须安装 ssh 才能使用管理远程 Hadoop 守护程序的 Hadoop 脚本。
此外，建议还安装pdsh以实现更好的ssh资源管理。
```
```shell
安装方式：
  $ sudo apt-get install ssh
  $ sudo apt-get install pdsh
```
## Deploy
* [Download](https://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-3.2.0/hadoop-3.2.0.tar.gz)

* Config
> * 执行的环境 与 参数
```md
通过 etc/hadoop/hadoop-env.sh 和 etc/hadoop/yarn-env.sh 设置特定于站点的值来控制分发的 bin/ 目录中的 Hadoop 脚本。
```
```md
至少必须指定JAVA_HOME，以便在每个远程节点上正确定义它。

etc/hadoop/hadoop-env.sh
export JAVA_HOME=/usr/share/java/
```

> * Hadoop 的 Java 配置
```md
由两种类型的重要配置文件驱动：
只读默认配置 - core-default.xml，hdfs-default.xml，yarn-default.xml和mapred-default.xml。
特定于站点的配置 - etc/hadoop/core-site.xml，etc/hadoop/hdfs-site.xml，etc/hadoop/yarn-site.xml 和 /etc/hadoop/mapred-site.xml。
```
***core-site.xml***
```xml
<property>
  <name>fs.defaultFS</name>
  <value>hdfs://10.96.111.130:9000</value>
</property>
```
```xml
<property>
  <name>hadoop.tmp.dir</name>
  <value>/home/hadoop/hadoop/data/tmp</value>
</property>
```
***hdfs-site.xml***
* dfs.namenode.name.dir
```md
NameNode持久存储命名空间和事务日志的本地文件系统上的路径。
如果这是逗号分隔的目录列表，那么名称表将在所有目录中复制，以实现冗余。
```
* dfs.datanode.data.dir
```md
逗号分隔的DataNode本地文件系统上的路径列表。
```
```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>2</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>/home/hadoop/hadoop/hdfs/name</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>/home/hadoop/hadoop/hdfs/data</value>
    </property>
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>bigdata-02:50090</value>
    </property>
</configuration>
```
```md
dfs.namenode.secondary.http-address 是指定secondaryNameNode的http访问地址和端口号。
```
***yarn-site.xml***
```xml
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
<property>
    <name>yarn.resourcemanager.hostname</name>
    <value>bigdata-02</value>
</property>
<property>
  <name>yarn.log-aggregation-enable</name>
  <value>true</value>
</property>
<property>
  <name>yarn.log-aggregation.retain-seconds</name>
  <value>106800</value>
</property>
```
```md
yarn.nodemanager.aux-services 配置了yarn的默认混洗方式，选择为mapreduce的默认混洗算法。
yarn.resourcemanager.hostname 指定了Resourcemanager运行在哪个节点上。
yarn.log-aggregation-enable是配置是否启用日志聚集功能。
yarn.log-aggregation.retain-seconds是配置聚集的日志在HDFS上最多保存多长时间。
```
***mapred-site.xml***
* 指定mapreduce运行在yarn框架上
```xml
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
</property>
<property>
  <name>mapreduce.jobhistory.address</name>
  <value>bigdata-01:10020</value>
</property>
<property>
  <name>mapreduce.jobhistory.webapp.address</name>
  <value>bigdata-01:19888</value>
</property>
<property>
  <name>yarn.app.mapreduce.am.env</name>
  <value>HADOOP_MAPRED_HOME=/home/hadoop/hadoop</value>
</property>
<property>
  <name>mapreduce.map.env</name>
  <value>HADOOP_MAPRED_HOME=/home/hadoop/hadoop</value>
</property>
<property>
  <name>mapreduce.reduce.env</name>
  <value>HADOOP_MAPRED_HOME=/home/hadoop/hadoop</value>
</property>
```
```md
mapreduce.framework.name 设置mapreduce任务运行在yarn上。
mapreduce.jobhistory.address 是设置mapreduce的历史服务器安装在bigdata-01机器上。
mapreduce.jobhistory.webapp.address 是设置历史服务器的web页面地址和端口号。
```
***Slaves File***
```md
在etc/hadoop/workers文件中 列出所有Worker主机名或IP地址，每行一个。
```
```md
bigdata-02
```
```sh
配置完成后，同步 /home/hadoop/hadoop 到 其余 机器：
$ scp -r hadoop hadoop@bigdata-02:/home/hadoop/
```
* Startup
***Start HDFS***
```md
第一次启动HDFS时，必须对其进行格式化。
格式化是对HDFS这个分布式文件系统中的DataNode进行分块，统计所有分块后的初始元数据的存储在NameNode中。
格式化后，查看core-site.xml里/home/hadoop/hdfs/name/current/指定的目录下是否有了dfs目录，如果有，说明格式化成功。

因为每次格式化，默认是创建一个集群ID，并写入NameNode和DataNode的VERSION文件中
（VERSION文件所在目录为dfs/name/current 和 dfs/data/current），
重新格式化时，默认会生成一个新的集群ID,如果不删除原来的目录，
会导致namenode中的VERSION文件中是新的集群ID，而DataNode中是旧的集群ID，不一致时会报错。

另一种方法是格式化时指定集群ID参数，指定为旧的集群ID。
```
```sh
$ $HADOOP_HOME/bin/hdfs namenode -format <cluster_name>
```
```md
$ ll /home/hadoop/hdfs/name/current/
total 16
-rw-rw-r-- 1 hadoop hadoop 401 Feb 27 12:09 fsimage_0000000000000000000
-rw-rw-r-- 1 hadoop hadoop  62 Feb 27 12:09 fsimage_0000000000000000000.md5
-rw-rw-r-- 1 hadoop hadoop   2 Feb 27 12:09 seen_txid
-rw-rw-r-- 1 hadoop hadoop 217 Feb 27 12:09 VERSION
```
```md
fsimage 是NameNode元数据在内存满了后，持久化保存到的文件。
fsimage*.md5 是校验文件，用于校验fsimage的完整性。
seen_txid 是hadoop的版本
vession文件里保存：
  namespaceID：NameNode的唯一ID。
  clusterID:集群ID，NameNode和DataNode的集群ID应该一致，表明是一个集群。
```
```md
$ cat /home/hadoop/hdfs/name/current/VERSION
#Wed Feb 27 12:09:48 CST 2019
namespaceID=1599188951
clusterID=CID-894de4e2-b6e5-4962-a1ed-fdd48d074530
cTime=1551240588388
storageType=NAME_NODE
blockpoolID=BP-541546279-10.96.111.130-1551240588388
layoutVersion=-65
```
```sh
$ $HADOOP_HOME/bin/hdfs --daemon start namenode
$ $HADOOP_HOME/bin/hdfs --daemon start datanode
$ $HADOOP_HOME/bin/hdfs --daemon start secondarynamenode
```
```sh
如果配置了 etc/hadoop/workers和ssh trusted access，则可以使用实用程序脚本启动所有HDFS进程。
$HADOOP_HOME/sbin/start-dfs.sh
```
***Start YARN***
```sh
$HADOOP_HOME/bin/yarn --daemon start resourcemanager
$HADOOP_HOME/bin/yarn --daemon start nodemanager
```
```md
同样，如果配置了etc / hadoop / workers和ssh trusted access（请参阅单节点设置），则可以使用实用程序脚本启动所有YARN进程。
$HADOOP_HOME/sbin/start-yarn.sh
```
***Start JobHistory Server***
```md
$HADOOP_HOME/bin/mapred --daemon start historyserver
```
***Start on Every Server***
```md
以上 描述了 各个模块的启动指令，对应于 我们一开始规划的部署架构，
悬着响应的 启动命令执行即可。
```
```md
[hadoop@bigdata-02 ~/hadoop]$ jps
6983 SecondaryNameNode
5863 ResourceManager
7302 NodeManager
7672 Jps
7090 DataNode
```
```md
[hadoop@bigdata-01 ~/hadoop]$ jps
6261 NodeManager
8064 DataNode
6616 JobHistoryServer
8138 Jps
7645 NameNode
2008 ZeppelinServer
```
***脚本启动***
```md
在配置了SSH免登陆的情况下，也可以使用脚本启动
```
```sh
[hadoop@bigdata-01 ~/hadoop]$ sh sbin/start-dfs.sh //会启动集群的 NameNode、DataNode、SecondNameNode服务 
[hadoop@bigdata-01 ~/hadoop]$ sh sbin/start-yarn.sh //会启动集群的 NodeManager、ResourceManager
```
## Web Interfaces

| Daemon                      | Web Interface              | Notes                       |
|-----------------------------|----------------------------|-----------------------------|
| NameNode                    | http://10.96.111.130:9870  | Default HTTP port is 9870.  |
| ResourceManager             | http://10.96.114.47:8088   | Default HTTP port is 8088.  |
| MapReduce JobHistory Server | http://10.96.111.130:19888 | Default HTTP port is 19888. |

## Test
```sh
$ cat test/wc.input
hadoop mapreduce hive
hbase spark storm
sqoop hadoop hive
spark hadoop
```
```sh
$ bin/hdfs dfs -mkdir -p /wordcountdemo/input
$ bin/hdfs dfs -put test/wc.input /wordcountdemo/input
$ bin/yarn jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.0.jar wordcount /wordcountdemo/input /wordcountdemo/output
```
```sh
$ bin/hdfs dfs -ls /wordcountdemo/output
-rw-r--r--   1 hadoop supergroup          0 2019-02-27 14:21 /wordcountdemo/output/_SUCCESS
-rw-r--r--   1 hadoop supergroup         60 2019-02-27 14:21 /wordcountdemo/output/part-r-00000
```
```md
_SUCCESS文件 是空文件，说明Job执行成功。

part-r-00000文件 是结果文件，其中-r-说明这个文件是Reduce阶段产生的结果，
mapreduce程序执行时，可以没有reduce阶段，但是肯定会有map阶段，如果没有reduce阶段是-m-。

一个reduce会产生一个part-r-开头的文件。
```
```sh
$ bin/hdfs dfs -cat /wordcountdemo/output/part-r-00000
hadoop  3
hbase   1
hive    2
mapreduce       1
spark   2
sqoop   1
storm   1
```

## 其他可选服务
* 日志聚集
### Hadoop HA安装
* 时间服务器
```md
集群的各个机器与这个时间服务器进行时间同步。
```
* Zookeeper
```md
Zookeeper集群能够保证NamaNode服务高可用。
```
* HDFS HA 
```md
单NameNode的缺陷存在单点故障的问题，如果NameNode不可用，则会导致整个HDFS文件系统不可用。
所以需要设计高可用的HDFS（Hadoop HA）来解决NameNode单点故障的问题。
解决的方法是在HDFS集群中设置多个NameNode节点。
```
* YARN HA
```md
Hadoop2.4版本之前，ResourceManager也存在单点故障的问题，也需要实现HA来保证ResourceManger的高可也用性。
ResouceManager从记录着当前集群的资源分配情况和JOB的运行状态，
YRAN HA 利用Zookeeper等共享存储介质来存储这些信息来达到高可用。
另外利用Zookeeper来实现ResourceManager自动故障转移。
```
* HDFS Federation
```md
是可以在Hadoop集群中设置多个NameNode，不同于HA中多个NameNode是完全一样的，是多个备份，
Federation中的多个NameNode是不同的，可以理解为将一个NameNode切分为了多个NameNode，每一个NameNode只负责管理一部分数据。 
HDFS Federation中的多个NameNode共用DataNode。
```
