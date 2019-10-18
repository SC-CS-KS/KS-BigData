# 大数据分析平台实践 - 如何搭建一套完成的大数据分析平台
## 系统规划
* 相关组件
```md
* Hadoop - 提供存储（HDFS）、计算（MapReduce）、资源管理（YARN）
* Hive - 数据仓库
* Spark - 计算引擎
* Presto - MPP架构的查询引擎
* Zeppelin - 数据可视化
```

* 系统布局
<table border=0 cellpadding=0 cellspacing=0 width=696 style='border-collapse: 
 collapse;table-layout:fixed;width:522pt'>
 <col width=162 style='mso-width-source:userset;width:121pt'>
 <col width=100 style='width:75pt'>
 <col width=167 style='mso-width-source:userset;width:125pt'>
 <col width=267 style='mso-width-source:userset;width:200pt'>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r0'>
<td height=21 class=x22 width=162 style='height:16pt;width:121.5pt;' >机器</td>
<td class=x22 width=100 style='width:75pt;' >组件</td>
<td class=x22 width=167 style='width:125.25pt;' ></td>
<td class=x22 width=267 style='width:200.25pt;' >运行进程</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r1'>
<td rowspan=8 height=149 class=x24 style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:112pt;' >10.96.111.130</br>bigdata-01</td>
<td rowspan=4 height=85 class=x24 style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:64pt;' >Hadoop</td>
<td class=x24>YARN</td>
<td class=x25>NodeManager</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r2'>
<td rowspan=2 height=42 class=x24 style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:32pt;' >HDFS</td>
<td class=x25>NameNode</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r3'>
<td class=x25>DataNode</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r4'>
<td class=x24></td>
<td class=x25>JobHistoryServer</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r5'>
<td rowspan=2 height=42 class=x24 style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:32pt;' >Hive</td>
<td class=x24>HiveMetaStore</td>
<td class=x25>RunJar</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r6'>
<td class=x24>HiveServer2</td>
<td class=x25>RunJar</td>
 </tr>
  </tr>
   <tr height=21 style='mso-height-source:userset;height:16pt' id='r7'>
<td class=x24>Zeppelin</td>
<td class=x24></td>
<td class=x25>ZeppelinServer</br>RemoteInterpreterServer</td>
<td class=x25></td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r13'>
<td class=x24>Spark</td>
<td class=x26></td>
<td class=x26>SparkSubmit</br>CoarseGrainedExecutorBackend</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r8'>
<td rowspan=9 height=192 class=x24 style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:144pt;' >10.96.114.47</br>bigdata-02</td>
<td rowspan=4 height=85 class=x24 style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:64pt;' >Hadoop</td>
<td rowspan=2 height=42 class=x24 style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:32pt;' >YARN</td>
<td class=x26>ResourceManager</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r9'>
<td class=x26>NodeManager</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r10'>
<td rowspan=2 height=42 class=x24 style='border-right:1px solid windowtext;border-bottom:1px solid windowtext;height:32pt;' >HDFS</td>
<td class=x26>DataNode</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r11'>
<td class=x26>SecondaryNameNode</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r12'>
<td class=x27>Hive</td>
<td class=x24>MySQL</td>
<td class=x26>mysqld</td>
 </tr>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r13'>
<td class=x24>Spark</td>
<td class=x26></td>
<td class=x26>ExecutorLauncher</br>CoarseGrainedExecutorBackend</td>
 <tr height=21 style='mso-height-source:userset;height:16pt' id='r16'>
<td class=x24>Presto</td>
<td class=x26></td>
<td class=x26>PrestoServer</td>
 </tr>
</table>

* 操作系统环境配置

*修改 /etc/hosts*
```md
10.96.111.130 bigdata-01
10.96.114.47 bigdata-02
```

## [Hadoop](Hadoop.md)

## Hive

## [Spark](Spark.md)

## Presto

## [Zeppelin](Zeppelin.md)