# [Notebook Storage](https://zeppelin.apache.org/docs/0.8.0/setup/storage/storage.html#notebook-storage-in-local-git-repository)
```md
Apache Zeppelin有一个可插拔的笔记本存储机制。
由zeppelin.notebook.storage配置选项控制，具有多个实现。
```
* org.apache.zeppelin.notebook.repo.GitNotebookRepo
* org.apache.zeppelin.notebook.repo.FileSystemNotebookRepo
```md
Notes可以存储在hadoop兼容的文件系统（如hdfs）中，以便多个Zeppelin实例可以共享相同的注释。
它支持hadoop 2.x的所有版本。
如果您使用FileSystemNotebookRepo，则 zeppelin.notebook.dir 是hadoop兼容文件系统上的路径。
并且您需要在zeppelin-env.sh中指定HADOOP_CONF_DIR，以便zeppelin可以找到正确的hadoop配置文件。
如果你的hadoop集群是kerberized，那么你需要指定zeppelin.server.kerberos.keytab和zeppelin.server.kerberos.principal
```

* org.apache.zeppelin.notebook.repo.MongoNotebookRepo
***Why MongoDB?***
```md
High Availability (HA) by a replica set 副本集的高可用性（HA）
Seperation of storage from server 从服务器分离存储
```
* org.apache.zeppelin.notebook.repo.GitHubNotebookRepo

