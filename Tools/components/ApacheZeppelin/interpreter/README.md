# [Interpeter](https://zeppelin.apache.org/docs/0.8.0/usage/interpreter/overview.html)

```md
Zeppelin Interpreter是一个插件，它使Zeppelin用户能够使用特定的语言/数据处理后端。

Interpreter Process是一个JVM进程，它使用thrift与Zeppelin守护进程通信。
每个解释器进程都有一个解释器组，这个解释器组可以有一个或多个解释器实例
```
## interpreter setting
```md
当属性名称由大写字母，数字和下划线（[A-Z_0-9]）组成时，属性将导出为环境变量。
否则将属性设置为JVM属性。

可以通过在值中添加＃{contextParameterName}来使用解释器上下文中的参数，
参数可以是以下类型：string，number，boolean。

如果context参数为null，则替换为空字符串。

可以使用笔记本右上角的设置图标将每个笔记本绑定到多个解释器设置。
```
## interpreter group
```md
每个解释器都属于解释器组，解释器组是一个启动/停止 解释器的单元。
默认情况下，每个解释器都属于一个组，但该组可能包含更多解释器。
例如，Spark解释器组包括Spark支持，pySpark，Spark SQL和依赖性加载器。

从技术上讲，来自同一组的Zeppelin解释器在同一个JVM中运行。

每个解释器属于一个组并在一起注册，所有属性都在解释器设置中列出。
```
## ***[Interpreter binding mode](Interpreter-binding-mode.md)***

## Connecting to the existing [Remote Interpreter](remote-interpeter.md)
```md
Zeppelin 用户可以启动嵌入其服务中的解释器线程，这将为用户提供在远程主机上启动解释器的灵活性。
```
```md
要启动解释器以及服务，您必须创建 RemoteInterpreterServer 的实例并按如下所示启动它：
```
```java
RemoteInterpreterServer interpreter = new RemoteInterpreterServer(3678); 
// Here, 3678 is the port on which interpreter will listen.    
interpreter.start();
```
```md
上面的代码将在您的进程中启动解释器线程。

启动解释器后，可以通过选中“连接到现有进程”复选框将 zeppelin 配置为连接到 RemoteInterpreter，
然后提供解释器进程正在侦听的主机和端口。
```

## Precode

## Interpreter Lifecycle Management
```md
在0.8.0之前，Zeppelin没有解释器的生命周期管理，用户必须通过UI显式关闭解释器。
从0.8.0开始，Zeppelin提供了一个新的界面LifecycleManager来控制解释器的生命周期。

目前，有2个实现：
NullLifecycleManager 和 TimeoutLifecycleManager是默认的。
```
* NullLifecycleManager
```md
不会做任何事情，用户需要像以前一样自己控制解释器的生命周期。
```
* TimeoutLifecycleManager
```md
将在解释器空闲一段时间后关闭解释器。默认情况下，空闲阈值为1小时。
用户可以通过zeppelin.interpreter.lifecyclemanager.timeout.threshold更改它。

TimeoutLifecycleManager是默认的生命周期管理器，
用户可以通过 zeppelin.interpreter.lifecyclemanager.class 进行更改。
```
## Generic ConfInterpreter
```md
Zeppelin的解释器设置由所有用户和笔记共享，如果您想要设置不同的设置，
例如 你可以创建spark_jar1来运行带有依赖jar1的spark和spark_jar2来运行带有依赖jar2的spark。

ConfInterpreter可以为解释器设置提供更细粒度的控制，并提供更大的灵活性。
```
* ConfInterpreter
```md
是一个通用解释器，可供任何解释器使用。输入格式应为属性文件格式。它可用于为任何解释器进行自定义设置。
但它需要在解释程序启动之前运行。并且当解释器进程启动时由解释器模式设置确定。

所以用户需要理解Zeppelin的解释器模式设置，并在启动解释器进程时注意。
例如，如果我们将spark解释器设置设置为每个音符隔离。在此设置下，每个音符将启动一个解释器进程。
在这种情况下，用户需要将ConfInterpreter作为第一段作为下面的示例。
否则无法应用自定义设置（实际上它会报告ERROR）
```
## Interpreter Process Recovery
```md
在0.8.0之前，关闭Zeppelin也意味着关闭所有正在运行的解释器进程。
通常，admin会关闭Zeppelin服务器以进行维护或升级，但不希望关闭正在运行的解释程序进程。
但不想关闭正在运行的解释器进程。在这种情况下，需要翻译过程恢复。

从0.8.0开始，用户可以通过将zeppelin.recovery.storage.class设置为
org.apache.zeppelin.interpreter.recovery.FileSystemRecoveryStorage或其他实现（如果可用）来启用解释器进程恢复，
默认情况下为org.apache.zeppelin.interpreter.recovery.NullRecoveryStorage表示未启用恢复。

启用恢复意味着关闭 Zeppelin 不会终止解释程序进程，并且当Zeppelin重新启动时，它将尝试重新连接到现有运行的解释程序进程。
如果要在终止 Zeppelin 后终止所有解释器进程，即使启用了恢复，也可以运行bin / stop-interpreter.sh。
```

## [Writing a New Interpreter](https://zeppelin.apache.org/docs/0.8.0/development/writing_zeppelin_interpreter.html)