# Mac 本地模式卸载 Hadoop

如果你在 Mac 上安装了 Hadoop 本地模式，但现在想要卸载，可以通过以下步骤完全移除 Hadoop。

## 1. 停止 Hadoop 服务

如果 Hadoop 正在运行，首先停止服务：

```bash
stop-dfs.sh
stop-yarn.sh
```
```bash
(base) fly@FlydeMacBook-Pro mac % stop-dfs.sh
Stopping namenodes on [localhost]
Stopping datanodes
Stopping secondary namenodes [FlydeMacBook-Pro.local]
(base) fly@FlydeMacBook-Pro mac % stop-yarn.sh
Stopping nodemanagers
Stopping resourcemanager
(base) fly@FlydeMacBook-Pro mac % 
```

## 2. 删除 Hadoop 数据目录

如果你在 Hadoop 配置文件中设置了 `dfs.name.dir` 和 `dfs.data.dir`，也需要删除这些文件目录：

```bash
cat hdfs-site.xml
```
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
删除对应的目录
```bash
sudo rm -rf /Users/fly/Documents/developer/hdfs/namenode
sudo rm -rf /Users/fly/Documents/developer/hdfs/datanode
```

## 3. 删除 Hadoop 目录

找到 Hadoop 安装目录（默认在 `/usr/local/hadoop`），并删除：
```bash
(base) fly@FlydeMacBook-Pro ~ % echo $HADOOP_HOME
/Users/fly/Documents/developer/hadoop-3.2.4
```
```bash
sudo rm -rf /Users/fly/Documents/developer/hadoop-3.2.4
```

## 4. 清理环境变量
修改 `~/.bash_profile` 或 `~/.zshrc`（根据你使用的 shell），删除 Hadoop 相关的环境变量：
```bash
nano ~/.zshrc  # 或 nano ~/.bash_profile
```

找到下面内容，并删除：

```bash
#HADOOP_HOME
export HADOOP_HOME=/Users/fly/Documents/developer/hadoop-3.2.4
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH
```

保存并退出，然后执行：

```bash
source ~/.zshrc  # 或 source ~/.bash_profile
```

## 5. 检查卸载是否成功

执行以下命令，确认 Hadoop 命令是否已失效：

```bash
hadoop version
```
如果结果显示 `command not found`，则表示 Hadoop 已被成功卸载。
```bash
(base) fly@FlydeMacBook-Pro namenode % hadoop version
zsh: command not found: hadoop
```

## 6. 选填：删除 Java

如果你也想清除 Java，可以使用以下命令：

```bash
sudo rm -rf /Library/Java/JavaVirtualMachines
```

或者使用 Homebrew 卸载（如果你通过 Homebrew 安装的）：

```bash
brew uninstall openjdk
```

至此，你已成功卸载了 Hadoop 及其相关文件。

