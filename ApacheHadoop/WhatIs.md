# 什么是Hadoop？

一个由Apache基金会所开发的分布式系统基础架构
	基于GoogleMap/Reduce来实现的Hadoop为开发者提供了map、reduce原语，
	使并行批处理程序变得非常地简单和优美。
Hadoop是一个开源的框架，可编写和运行分布式应用处理大规模数据，
	是专为离线和大规模数据分析而设计的，并不适合那种对几个记录随机读写的在线事务处理模式。
	Hadoop的数据来源可以是任何形式，
		在处理半结构化和非结构化数据上与关系型数据库相比有更好的性能，具有更灵活的处理能力，
		不管任何数据形式最终会转化为key/value，key/value是基本数据单元
Hadoop就是一个分布式计算的解决方案。
	hadoop擅长日志分析，facebook就用Hive来进行日志分析，
		2009年时facebook就有非编程人员的30%的人使用HiveQL进行数据分析
	淘宝搜索中的自定义筛选也使用的Hive
	利用Pig还可以做高级的数据处理，
		包括Twitter、LinkedIn上用于发现您可能认识的人，可以实现类似Amazon.com的协同过滤的推荐效果
		淘宝的商品推荐也是！在Yahoo！的40%的Hadoop作业是用pig运行的，
			包括垃圾邮件的识别和过滤，还有用户特征建模。
		（2012年8月25新更新，天猫的推荐系统是hive，少量尝试mahout！）

## 发展
 雏形开始于2002年的Apache的Nutch，Nutch是一个开源Java 实现的搜索引擎。
	它提供了我们运行自己的搜索引擎所需的全部工具。包括全文搜索和Web爬虫。
 随后在2003年Google发表了一篇技术学术论文谷歌文件系统（GFS）。
	GFS也就是google File System，google公司为了存储海量搜索数据而设计的专用文件系统。
2004年Nutch创始人Doug Cutting基于Google的GFS论文实现了分布式文件存储系统名为NDFS。
MapReduce是一种编程模型，用于大规模数据集（大于1TB）的并行分析运算。
	2004年Google又发表了一篇技术学术论文MapReduce。
2005年Doug Cutting又基于MapReduce，在Nutch搜索引擎实现了该功能。
2006年，Yahoo雇用了Doug Cutting，Doug Cutting将NDFS和MapReduce升级命名为Hadoop，
	Yahoo开建了一个独立的团队给Goug Cutting专门研究发展Hadoop。
不得不说Google和Yahoo对Hadoop的贡献功不可没。

## 场景

可编写和运行分布式应用处理大规模数据
	是专为离线和大规模数据分析而设计的，并不适合那种对几个记录随机读写的在线事务处理模式。
应用方向
	为银行和信用卡公司增强欺诈性检测
		银行通过使用Hadoop，建立大型集群，进行数据分析；并将分析模型应用于银行交易过程，从而提供实时的欺诈行为检测。
	社交媒体市场分析
		公司利用Hadoop监测、收集、汇聚这些信息，并提取、汇总自身的产品和服务信息，以及竞争对手的相关信息，发掘内在商业模式，或者预测未来的可能趋势，从而更加了解自身的业务。
	零售行业购物模式分析
	城市发展的交通模式识别
		通过监控在一天内不同时间的交通状况，发掘交通模型，城市规划人员就可以确定交通瓶颈。
	内容优化和内容参与
		企业越来越专注于优化内容，将其呈现在不同的设备上，并支持不同格式。
	网络分析和调解
		针对交易数据、网络性能数据、基站数据、设备数据以及其他形式的后台数据等，进行大数据实时分析，能够降低公司运营成本，增强用户体验。
	大数据转换
		纽约时报要将1100万篇文章（1851至1980年）转换成PDF文件，这些文章都是从报纸上扫描得到的图片。
			利用Hadoop技术，这家报社能够在24小时内，将4TB的扫描文章转换为1.5TB的PDF文档。

## 优点
Hadoop是一个能够对大量数据进行分布式处理的软件框架。
Hadoop以一种可靠、高效、可伸缩的方式进行数据处理。
Hadoop 是可靠的，因为它假设计算元素和存储会失败，因此它维护多个工作数据副本，确保能够针对失败的节点重新分布处理。
Hadoop 是高效的，因为它以并行的方式工作，通过并行处理加快处理速度。
Hadoop 还是可伸缩的，能够处理 PB 级数据。
	此外，Hadoop依赖于社区服务，因此它的成本比较低，任何人都可以使用。
Hadoop是一个能够让用户轻松架构和使用的分布式计算平台。
	用户可以轻松地在Hadoop上开发和运行处理海量数据的应用程序。
高可靠性
	Hadoop按位存储和处理数据的能力值得人们信赖。
高扩展性
	Hadoop是在可用的计算机集簇间分配数据并完成计算任务的，这些集簇可以方便地扩展到数以千计的节点中。
高效性
	Hadoop能够在节点之间动态地移动数据，并保证各个节点的动态平衡，因此处理速度非常快。
高容错性
	Hadoop能够自动保存数据的多个副本，并且能够自动将失败的任务重新分配。
低成本
	与一体机、商用数据仓库以及QlikView、YonghongZ-Suite等数据集市相比，hadoop是开源的，项目的软件成本因此会大大降低。
Hadoop带有用Java语言编写的框架，因此运行在 Linux生产平台上是非常理想的。
	Hadoop 上的应用程序也可以使用其他语言编写，比如 C++、python。

## Hadoop商业发行版
Cloudera CDH
Hortonworks HDP
华为 Fusioninsight2
星环 Transwarp
MapR
	组件包括HDFS, HBase, MapReduce, Hive, Mahout, Oozie, Pig, ZooKeeper, Hue以及其他开源工具。
	它还包括直接NFS访问、快照、“高实用性”镜像、专有的HBase实现，与Apache完全兼容的API和一个MapR管理控制台。
	Name Node实现了真正意义上的集群。
IBM InfoSphere BigInsights
GreenPlum的Pivotal HD
亚马逊弹性MapReduce（EMR）
	亚马逊EMR是一个web服务，能够使用户方便且经济高效地处理海量的数据。
	它采用Hadoop框架，运行在亚马逊弹性计算云EC2和简单存储服务S3之上。
	包括HDFS（S3支持），HBase（专有的备份恢复），MapReduce，, Hive (Dynamo的补充支持), Pig, and Zookeeper
Windows Azure的HDlnsight
	HDlnsight基于Hortonworks数据平台（Hadoop1），运行在Azure云。
	它集成了微软管理控制台，易于部署，易于System Center的集成。
	通过使用Excel插件，可以整合Excel数据。
	通过Hive开放式数据库连接（ODBC）驱动程序，可以集成Microsoft SQL Server分析服务（SSAS）、PowerPivot和Power View。Azure Marketplace授权客户连接数据、智能挖掘算法以及防火墙之外的人。
	。Windows Azure Marketplace从受信任的第三方供应商中，提供了数百个数据集。
