# What Is Apache Zeppelin
```md
Web-based notebook that enables data-driven, 
interactive data analytics and collaborative documents with SQL, Scala and more.

是一个基于 Web 的 notebook，提供交互数据分析和可视化。
支持接入多种数据处理引擎，如 spark，hive 等。
支持多种语言： Scala(Apache Spark)、Python(Apache Spark)、SparkSQL、 Hive、 Markdown、Shell等。

平台要解决的是：在服务端资源一定的情况下，如何尽可能正确和高效地执行多个用户混合了多种语言的Notes问题。
```
```md
支持多语言混合的REPL(Read-Evaluation-Print-Loop)。

是一款基于Web的笔记本，支持交互式数据探索，可视化和协作。
非常适用于与长工作流交互式工作：开发，组织和运行分析代码以及可视化结果。
```
## 功能特性
```md
基于Web的笔记本样式编辑器
内置Apache Spark支持
```
```md
 Data Ingestion
 Data Discovery
 Data Analytics
 Data Visualization & Collaboration
```
## 优势
***站在使用者的角度***
```md
可以在一个Note中混合使用多种语言，用户可以根据需要完成的任务的类型，选择最合适的语言来实现，不再受限于单个语言的特性。

数据分析和机器学习，从来都不是一个“一蹴而就”的过程。分析过程和机器学习算法一样，本质是不断迭代和试错的过程。
REPL相对于“拖拽式”的数据分析平台，虽然不能像传统IDE一样，设置断点和查看中间变量，但是相对于“纯黑盒”的拖拽式数据分析工具，能在很大程度上实现“调试”功能。
```
***站在管理者的角度***
```md
使得统一工具环境成为可能，实现一个集中式的分析工具，在服务端集中于一处，
统一配置R、Python、Spark、Hadoop、Hive等开发环境，显著降低运维成本。 

使得进行统一安全控制成为可能。
```
## Usage scenario
```md
适合单一数据处理、但后端处理语言繁多的场景，尤其适合Spark。
```
## Compare
* vs. Apache Hue
```md
Zeppelin只提供了单一的数据处理功能，包括数据提取、数据分析、数据可视化等都属于数据处理的范畴。

Hue的功能相对丰富的多，除了类似的数据处理，还有元数据管理、Oozie工作流管理、作业管理、用户管理、Sqoop集成等很多管理功能。
从这点看，Zeppelin只是一个数据处理工具，而Hue更像是一个综合管理工具。

Hue适合与Hadoop集群的多个组件交互、如Oozie工作流、Sqoop等联合处理数据的场景，尤其适合与Impala协同工作。
```