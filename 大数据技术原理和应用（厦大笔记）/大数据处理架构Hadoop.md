## 2.1Hadoop简介 ##


**由java语言开发，可以使用java,c,c++语言开发Hadoop应用。**
**Apache的开源软件**

### 两大核心 ###


- HDFS
- MapReduce



### Hadoop相应的技术来源 ###

- google的GFS(Google File System)的开源->HDFS
- google的MapReduce




### 特点 ###

- 高可靠性
    - 机器发生了故障，依旧可以对外提供服务

- 高效性
    - 使用集群来做一个运算

- 可扩展性
    - 可以很好的增减机器

- 成本低
    - 之前使用（HPC(high performance computing)高性能计算）的高档机器。
    - 可以使用低端机。
- 支持多种语言开发


### 企业应用 ###

![](https://i.imgur.com/lGkW10o.png)



### hadoop版本介绍 ###

![](https://i.imgur.com/VTRCfJC.png)

### MapReduce变更 ###
- 1.0时代
    - Mapreduce负责：集群资源调度+数据分析
- 2.0时代
    - MapReduce负责：数据分析
    - 将资源调度分配给了YARN
    - YARN上支持很多的框架，MapReduce,Spark,等



### HDFS变更 ###

- 2.0版本
    - 提出增加了NN Federration(名称节点)
    - 引入了HA(NameNode热备份)


 ![](https://i.imgur.com/i8d1FC3.png)



### Hadoop版本 ###


- 开源版

![](https://i.imgur.com/BQNDKXp.png)
- 企业版

![](https://i.imgur.com/fVaHACQ.png)

![](https://i.imgur.com/8iIbfy4.png)

![](https://i.imgur.com/SNGa1Pn.png)



### 不同版本评价 ###


- 标准

![](https://i.imgur.com/qOO10rX.png)

- 评分
![](https://i.imgur.com/7yyuhsp.png)

![](https://i.imgur.com/CwhNdWf.png)

![](https://i.imgur.com/eXx1BU5.png)

- 总评价

![](https://i.imgur.com/42MLr0A.png)



## 2.2hadoop项目结构 ##


### 基本架构 ###

  ![](https://i.imgur.com/0zZ0LE6.png)

### 相关介绍 ###

- MapResuce:离线运算。基于磁盘的运算，开始结束都是磁盘。
- Tez:对MapReduce进行优化，构建有向无环图，获得更好的处理效率
- Spark：做相关的数据处理，基于内存的计算。
- Hive:建立在Hadoop架构之上的数据仓库。它能够提供数据的精炼查询和分析。支持SQL语句，将sql语句转化成MapReduce作业进行执行。
- Pig:轻量级脚本语言，用于轻量的流分析。提供了Pig Latin（类似sql）。支持镶嵌在其他语言执行。
- Oozie：工作流管理工具。
- Zookeeper:分布式协调服务。分布式锁，集群管理。
- Hbase:分布式数据库。
- Flume:实时日志的收集。
- Sqoop:用于关系数据库和非关系数据库的交流。
- Ambari:快速部署工具。


## 2.3Hadoop的安装和使用 ##


### 安装流程 ###

1. 创建hadoop用户
2. SSH登陆权限设置
3. 安装java环境
4. 单机安装配置
5. 伪分布式安装配置




### 添加用户 ###

- 添加用户

        sudo useradd -m hadoop -s /bin/bash

- 设置密码

sudo passwd hadoop xxxx

- 增加权限

sudo  adduser hadoop sudo

### SSH配置 ###

- 更新apt-get
    
        sudo apt-get update
- 安装ssh-server

        sudo apt-get install openssh-server

- 测试连接

        ssh  localhost
        exit   #退出

- 配置密匙

        cd ~/.ssh/                     # 若没有该目录，请先执行一次ssh localhost
        ssh-keygen -t rsa              # 会有提示，都按回车就可以
        cat ./id_rsa.pub >> ./authorized_keys   #保存了自动的密匙

![](https://i.imgur.com/UmA2Amx.png)

 
### java环境配置 ###

- 安装default-jre default-jdk


        sudo apt-get install default-jre default-jdk



- 打开账户主目录下的bash.bashrc文件，配置环境变量


        vim ~/.bashrc


        文本前添加export JAVA_HOME=/usr/lib/jvm/default-java   ，然后保存
        

        source ~/.bashrc   #资源起效


###hadoop安装###

- 使用psftp上传到服务器

- 解压到usr/local下

        sudo tar -zxf 你的hadoop压缩包所在路径 -C /usr/local    # 解压到/usr/local中
- 重命名

        cd /usr/local
        mv  你的hadoop名字  hadoop

- 修改文件权限

chown  -R hadoop  ./hadoop


- 查看版本

        cd  hadoop/bin
        ./hadoop version


### 伪分布安装配置 ###

![](https://i.imgur.com/h9VfP4N.png)


- 修改core-site.xml

![](https://i.imgur.com/NVhDqw9.png)

- 修改hdfs-site.xml

![](https://i.imgur.com/cmZWvkD.png)

![](https://i.imgur.com/sUYaeXy.png)

### cmd命令启动的区别 ###

![](https://i.imgur.com/rRR9ugb.png)

## 2.4 hadoop的部署和使用 ##

### 两大核心组件 ###

- HDFS
- MapReduce


### HDFS ###

#### 组成 ####

- 一个NameNode
    - 首先访问
    - 安排了访问文件的访问组成
    - SecondNameNode，2.0作为NameNode热备份

        ![](https://i.imgur.com/zRUVHKm.png)
        ![](https://i.imgur.com/8PMSG9i.png)

- 无数个DataNode
    - 有n个小的文件块组成


### MapReduce ###

#### 核心组件 ####

- JobTracker
    - 协调作业
- TaskTraker
    - 对作业进行分配


### 集群电脑配置 ###

![](https://i.imgur.com/SVOYyq7.png)


### 集群的测试 ###

![](https://i.imgur.com/9s0pD0B.png)
- 