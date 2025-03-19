# Mac本地模式安装

基本环境
- terminal工具：tabby
- java8 brew 下载 （待补充）
- [hadoop-3.2.4.tar.gz](https://archive.apache.org/dist/hadoop/common/hadoop-3.2.4/hadoop-3.2.4.tar.gz): 适用于hive3.x、spark3.x

## 配置环境变量
查看自己的系统用的是什么shell
```bash
echo $SHELL
显示：/bin/zsh
```
> **💡 提示：** 如果使用 zsh，建议修改 ~/.zshrc；如果使用 bash，建议修改 ~/.bashrc 或 ~/.bash_profile。

编辑对应的文件
```bash
vim ~/.zshrc
```
```bash
#ll
alias ll='ls -lG'

#JAVA
export JAVA_HOME="/usr/local/opt/openjdk@8/libexec/openjdk.jdk/Contents/Home"
export PATH="$JAVA_HOME/bin:$PATH"

#HADOOP_HOME
export HADOOP_HOME=/Users/fly/Documents/developer/hadoop-3.2.4
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH
```
```bash
source ~/.zshrc
```
##  配置 Hadoop 配置文件
###  进入 Hadoop 的配置目录：
```bash
cd $HADOOP_HOME/etc/hadoop
```
#### core-site.xml:配置Hadoop的默认文件系统
```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
#### hdfs-site.xml：配置HDFS的存储目录。
```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
  <property>
    <name>dfs.namenode.http-address</name>
    <value>0.0.0.0:50070</value>
  </property>
    <!-- NameNode 存储目录 -->
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:///Users/fly/Documents/developer/hdfs/namenode</value>
    </property>

    <!-- DataNode 存储目录 -->
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:///Users/fly/Documents/developer/hdfs/datanode</value>
    </property>
</configuration>
```
> **💡 提示：** 
> 不要设置Mac的tmp临时目录，第二次开机会清空的。

####  mapred-site.xml：配置MapReduce的执行环境。
```xml
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
```
#### yarn-site.xml：配置YARN资源管理器
```xml
<configuration>
  <property>
    <name>yarn.resourcemanager.address</name>
    <value>127.0.0.1:8032</value>
  </property>
  <property>
     <name>yarn.nodemanager.aux-services</name>
     <value>mapreduce_shuffle</value>
  </property>
</configuration>
```
> yarn.nodemanager.aux-services 是 YARN NodeManager 的参数，用于配置辅助服务（Auxiliary Services）。这些服务允许 YARN 运行 MapReduce Shuffle 之类的附加功能。
## 格式化HDFS
```bash 
hdfs namenode -format
```
格式化成功标志
```bash
2025-03-19 17:22:17,213 INFO common.Storage: Storage directory /Users/fly/Documents/developer/hdfs/namenode has been successfully formatted.
```
出现Successfully代表成功
## 启动Hadoop
```bash
start-dfs.sh
start-yarn.sh
```
告警：
```bash
WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
```
>表示 Hadoop 无法加载本地（Native）库，因此会使用 Java 内置类 作为替代。虽然不会导致严重错误，但可能会影响 性能（如压缩、文件操作）。不过本地测试就无所谓了

bug:
> Hadoop 的 start-dfs.sh 和其他脚本会通过 SSH 连接到机器（即使是本地机器）
- 🐞 **Bug:** Mac没有开通ssh访问本机，请参考***文档。
- 🐞 **Bug:** Mac没有给予 全磁盘访问权限，请参考***文档。
- 🐞 **Bug:** 如果你没有配置公钥认证，请参考***文档。
- 
##  验证安装
```bash
jsp

(base) fly@FlydeMacBook-Pro mac % jps
9265 NameNode
9831 NodeManager
9516 SecondaryNameNode
9900 Jps
9725 ResourceManager
9375 DataNode
```
## 访问 Hadoop Web 界面
- HDFS Web界面：http://localhost:50070/
  - 🐞 **Bug:** 无法访问此网站，请参考***文档。

- YARN Web界面：http://localhost:8088/

