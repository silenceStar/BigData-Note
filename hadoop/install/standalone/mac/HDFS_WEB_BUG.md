# http://localhost:50070/访问失败
如果 http://localhost:50070/ 访问失败，可能是以下原因：

## Hadoop NameNode 未启动：
请确保 NameNode 正常运行，可以通过以下命令检查：

```bash
(base) fly@FlydeMacBook-Pro logs % jps
13040 NodeManager
12500 NameNode
12740 SecondaryNameNode
12604 DataNode
13324 Jps
7517 Main
12943 ResourceManager
```
如果没有 NameNode 进程，尝试启动它：
```bash
start-dfs.sh
```

## 端口被防火墙阻止：
确保 50070 端口没有被防火墙阻挡。

查看防火墙状态 
disabled是关闭状态
```bash
(base) fly@FlydeMacBook-Pro logs % sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate
Firewall is disabled. (State = 0)
```

## 端口占用情况
```bash
(base) fly@FlydeMacBook-Pro logs % lsof -i :50070

```
显示为空，没有被占用

## Hadoop 配置问题：
检查 core-site.xml 和 hdfs-site.xml 配置文件是否正确配置。
```bash
cd $HADOOP_HOME/etc/hadoop
cat hdfs-site.xml
```
```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <!-- NameNode 存储目录 -->
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/Users/fly/Documents/developer/hdfs/namenode</value>
    </property>

    <!-- DataNode 存储目录 -->
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/Users/fly/Documents/developer/hdfs/datanode</value>
    </property>
</configuration>
```

## 发现问题
### hdfs-site.xml没有指定50070的配置
修改
```bash
vim hdfs-site.xml
```
```xml

<property>
    <name>dfs.namenode.http-address</name>
    <value>localhost:50070</value>
</property>
```

关闭
```bash
(base) fly@FlydeMacBook-Pro hadoop % stop-all.sh 
WARNING: Stopping all Apache Hadoop daemons as fly in 10 seconds.
WARNING: Use CTRL-C to abort.
Stopping namenodes on [localhost]
Stopping datanodes
Stopping secondary namenodes [FlydeMacBook-Pro.local]
2025-03-19 17:49:55,441 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Stopping nodemanagers
Stopping resourcemanager
(base) fly@FlydeMacBook-Pro hadoop % jps     
14347 Jps
7517 Main
```
重新启动
```bash
start-dfs.sh
```
http://localhost:50070/
成功！
![成功](resource/picture/hdfs/hdfs_web_success.png)
