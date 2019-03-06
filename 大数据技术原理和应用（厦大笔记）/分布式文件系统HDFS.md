## 3.1分布式文件系统HDFS简介 ##

### 全称 ###

- Hadoop  Distributed File System


### 目的 ###

- 解决海量数据的存储

### 计算机集群的基本架构 ###

  ![](https://i.imgur.com/OtGJ3Pz.png)

  ![](https://i.imgur.com/IoGJxcX.png)


### 文件系统的架构 ###

  ![](https://i.imgur.com/30v2MA6.png)

### HDFS目标 ###

- 廉价的硬件设备
- 海量的读写
- 海量的存储
- 支持简单的文件模型
- 强大的跨平台性

### HDFS的局限性 ###

- 不支持低延迟性的访问
- 无法高效存储大量的小文件
- 不支持多用户的写入和任意的修改文件


### 两大组件 ###

- 名称节点NameNode
    - 整个HDFS集群的管家
    - 数据目录（用于访问查询）
- 数据节点DataNode
    - 存储实际的数据


## 3.2HDFS相关的概念 ##

### 块的概念 ###

  ![](https://i.imgur.com/55jnU5i.png)


### 块设计的原因 ###

- 支持面向大规模数据的存储
- 降低分布式节点的寻址开销
- 方便数据的备份



### 名称节点 ###

#### 存储元数据 ####
   
- 文件是什么
- 文件分为了多少块
- 每个文件和块是怎么映射的
- 每个块被存储在了那个服务器上

#### 组成 ####

![](https://i.imgur.com/uEXxfqt.png)

- FsImage
  
    ![](https://i.imgur.com/z7WNEYg.png)


  - 当数据节点加入HDFS，会通知NameNode,不断丰富记录,FsImage和EditLog保存在内存中
    
### FSImage和Editlog工作流程 ###

![](https://i.imgur.com/Rl4bQXo.png)



### EditLog不断增大解决方式 ###

- secondNode通知 停止使用EditLog

  ![](https://i.imgur.com/N5HYSSw.png)
 
- secondNode生成edits.new,用具记录新的修改

  ![](https://i.imgur.com/VaeXXZ0.png)

- 在secondNode的本地拉取与合并

  ![](https://i.imgur.com/tt9XWCX.png)

- 上传FsIamge,把edits.new改成editLog

  ![](https://i.imgur.com/igeABeL.png)



## 3.3HDFS体系结构 ##

### 体系结构 ###

- 先访问NameNode,客户机获得信息
- 再客户机获得与DataNode交流

    ![](https://i.imgur.com/x3Cfy1n.png)

### 命名空间 ###

![](https://i.imgur.com/sf64oyo.png)


### 协议使用 ###

- 客户端-NameNode:TCP
- NameNode-DataNode:数据协议
- 客户端-DataNode:远程调用

    ![](https://i.imgur.com/XuvmZhe.png)



### 局限性（1.0） ###

- NameNode存储信息在内存，有容量限制
- 通过NameNode访问，受限于NameNode吞吐量限制
- 只有一个命名空间，无法对不同应用进行隔离
- 唯一节点故障，集群就会不可用

![](https://i.imgur.com/9jbxJq4.png)

### 解决（2.0） ###


- （增加SecondNode）由冷备份变成热备份



## 3.4HDFS存储原理 ##

  ![](https://i.imgur.com/vUVkwoT.png)

### 冗余存储 ###

  ![](https://i.imgur.com/dFzsdRR.png)

#### 优点 ####


- 加快数据传输的速度（并行的存储数据）
- 容易检查数据错误
- 保证数据的可靠性

### 块的存放 ###
- 第一个存放目的机架
- 第二个其他机架
- 第三为第一机架的其他节点
- 其他随机

    ![](https://i.imgur.com/CqLAMqC.png)

### 数据读取 ###
- 就近原则
    ![](https://i.imgur.com/UJ2Je5w.png)




### 数据的错误和恢复 ###

#### 名称节点出错 ####

![](https://i.imgur.com/ciI0WPw.png)

- hadoop2.x已经是热备份了

#### 数据节点恢复 ####

- 数据迁移到其他服务器

![](https://i.imgur.com/7iPW8li.png)

#### 数据出错 ####

- 校验码校验

![](https://i.imgur.com/ZLR5xTz.png)







-1