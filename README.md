### 在win10下安装hadoop2.7.1-搭建单机hadoop

#### 1 配置JAVA_HOME

由于JAVA_HOME 路径不要有空格

所以配置环境变量中/Program Files/ 改为 /Progra~1/ 例: 

JAVA_HOME

C:/Progra~1/Java/jdk1.8.0_181

PATH

%JAVA_HOME%/bin

#### 2 配置HADOOP_HOME，PATH

##### 2.1 解压hadoop-2.7.1.tar.gz到D:/hadoop (注：因人而异，举例而已)

##### 2.2 替换hadoop-2.7.1下的bin文件与etc文件（文件在/bin, /etc,另etc可自己配置）

##### 2.3 将hadoop.dll复制到C:/windows/System32下 (文件在/dll)

##### 2.4 配置环境变量

HADOOP_HOME

D:/hadoop/hadoop-2.7.1/

PATH

%HADOOP_HOME%/bin

%HADOOP_HOME%/sbin

#### 3 配置etc下配置文件

##### 3.1 hadoop-env.cmd

```bat
@rem The java implementation to use.  Required.
@rem set JAVA_HOME=%JAVA_HOME%
@rem 添加下一行
set JAVA_HOME=C:\PROGRA~1\Java\jdk1.8.0_181
@rem The jsvc implementation to use. Jsvc is required to run secure datanodes.
@rem set JSVC_HOME=%JSVC_HOME%
```

##### 3.2 core-site.xml

```xml
<!-- 添加如下，单机模式 -->
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

##### 3.3 hdfs-site.xml

```xml
<!-- 数据复制一份，临时目录data\namenode和data\datanode需要自行创建 -->
<configuration>
 <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>D:\hadoop\hadoop-2.7.1\data\namenode</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>D:\hadoop\hadoop-2.7.1\data\datanode</value>
    </property>
</configuration>
```

##### 3.4 mapred-site.xml

```xml
<!-- 执行框架设置为 Hadoop YARN -->
<configuration>
    <property>
       <name>mapreduce.framework.name</name>
       <value>yarn</value>
    </property>
</configuration>
```

##### 3.5 yarn-site.xml

```xml
<!-- 指定shuffle -->
<configuration>
    <property>
       <name>yarn.nodemanager.aux-services</name>
       <value>mapreduce_shuffle</value>
    </property>
    <property>
       <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
       <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
</configuration>
```

##### 3.6 slaves

```
localhost
```

#### 4 准备工作

hdfs namenode -format

hdfs datanode -format

#### 5 开启服务

在hadoop/sbin下启动start-all.cmd 

或者 直接cmd下 start-all

注：会开启四个cmd窗口 不要关闭要用 stop-all关闭



