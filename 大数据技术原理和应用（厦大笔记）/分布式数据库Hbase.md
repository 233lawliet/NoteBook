## 4.1Hbase简介 ##

### 背景 ###

- BigTable（gogole存储爬虫网页）的开源实现

    ![](https://i.imgur.com/8WyMfL0.png)



### 架构基础GFS ###

  ![](https://i.imgur.com/VvS9kvI.png)


### 优势 ###

- 扩展性好
- 良好的性能
- 高可靠
- 可伸缩

    ![](https://i.imgur.com/UfsSwtf.png)
    ![](https://i.imgur.com/3luOjm8.png)


### HBase和BigTable对应关系 ###

![](https://i.imgur.com/w4Zb5GT.png)

### Hbase设计原因 ###


![](https://i.imgur.com/djx2zeS.png)
    
- 传统关系数据库优化方案（仍满足不了）
    - 主从复制
    - 分库
    - 分表


### Hbase和传统数据库的关系数据 ###

  ![](https://i.imgur.com/leYEqE4.png)

- Hbase存储都是字符串
- Hbase不需要连表
- Hbase不要复杂索引
     ![](https://i.imgur.com/ToKeI1W.png)
- Hbase保留旧的版本
- 水平可扩展


### 访问方式 ###

![](https://i.imgur.com/99Q2tkB.png)




## 4.2HBase数据模型 ##




**HBase是一个系数的多维度排序的映射表**

### 定位数据（四维定位） ###

- 列族
    - 支持动态扩展
- 列限定符（列）
- 行键（rowkey）
    - 一个行键可以对应任意多个列
- 时间戳
    - 不同版本的数据会保留在数据库中
    
![](https://i.imgur.com/qktFOSf.png)


举例子：
![](https://i.imgur.com/zSL74t4.png)

![](https://i.imgur.com/F9INZef.png)
### 列族 ###

![](https://i.imgur.com/uW2PRSI.png)




### HBase的视图存储 ###

- 查看视图

    ![](https://i.imgur.com/LBlOs3X.png)

- 物理实际视图

    ![](https://i.imgur.com/FVvxtRK.png)

    - 没有存储那么多的空格。


### 行式存储与列式存储 ###

![](https://i.imgur.com/KIUnD0P.png)

![](https://i.imgur.com/Gdd20K9.png)

- 行式存储的优缺点
    - 偏向事务性操作
        ![](https://i.imgur.com/PjFL8ZG.png)

- 列式存储的优缺点
    - 偏向分析型操作

![](https://i.imgur.com/7XjUxkH.png)



## 4.3HBase实现原理 ##

### HBase功能组件 ###

![](https://i.imgur.com/khUrJbw.png)


- 库函数
    - 一般用于链接客户端

- Master服务器
    - 充当管家
        ![](https://i.imgur.com/WGi9TrF.png)

- Region服务器
    - 负责存储不同分Regin
        ![](https://i.imgur.com/RrpvIWL.png)



**HBase有许多小的Region做成**

### Region ###

![](https://i.imgur.com/Rzk85V9.png)


![](https://i.imgur.com/f8Ffx4k.png)


- Region的定位问题
    - 三层定位
    
        ![](https://i.imgur.com/BB2uhcR.png)
        - root表是写死的     
        - 三级寻址，查找数据
    - 三层作用
        ![](https://i.imgur.com/Dfv87bq.png)
    - 可访问region个数
        ![](https://i.imgur.com/7tgdvee.png)
    - 缓存问题
        - 为了为加速客户端快速访问，增加了客户端缓存设置
        - 使用了惰性缓存，访问不到时，才重新逐次访问




## 4.4HBase应用方案 ##


### 性能优化 ###


- 使用时间戳排序
    - 优先命中时间戳早的数据
        ![](https://i.imgur.com/P97Ii9F.png)
- 设置缓存
    - 开启region服务器的缓存
    
       ![](https://i.imgur.com/OTxFGK8.png)
- 设置清理过期的数据
    ![](https://i.imgur.com/ECFMIkf.png)

### 查询HBase性能的工具 ###

- 自带工具Master-status
![](https://i.imgur.com/D08DiY0.png)
- Ganglia
![](https://i.imgur.com/2i1Zvi0.png)
- OpenTSDB
![](https://i.imgur.com/ySqGsnM.png)
- Ambari!
![](https://i.imgur.com/cBZIcMQ.png)


### 使用SQL查询HBase ###

- Hive整合
- Phoenix
![](https://i.imgur.com/opBL9Cp.png)
![](https://i.imgur.com/WkqMjTU.png)



### 构建HBase的二级索引（辅助索引） ###



- 原生的HBase不支持构建索引
- 仅支持行键索引
![](https://i.imgur.com/ChBihmZ.png)

- HBase0.93之后支持了Coprocessor
- 使得可以添设索引

![](https://i.imgur.com/Wicc3CP.png)

#### 构建二级索引的过程 ####

- Coprocessor机制包括了endpoint和observer
    ![](https://i.imgur.com/bQdbM30.png)

- 从而使得可以通过非行键进行索引
![](https://i.imgur.com/VpA1zSC.png)


#### 构建二级索引的优缺点 ####

![](https://i.imgur.com/MagCtI2.png)

![](https://i.imgur.com/w1gqODZ.png)


### 二级索引方案 ###

- 华为的Hindex
- HBase+Redis

![](https://i.imgur.com/VzY1TyB.png)

- Solr+HBase
![](https://i.imgur.com/DyELyGp.png)

## 4.5HBase安装和配置 ##


### 安装 ###
1. HBase的安装 
    - 使用psftp上传到服务器
    - 解压到usr/local目录下+添加path路径
        - java的path+hbase的path
          
                vim /etc/profile
                export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
                export JRE_HOME=${JAVA_HOME}/jre
                export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
                export PATH={JAVA_HOME}/bin:$PATH
            
                export HBASE_HOME=/usr/local/hbase
                export PATH=${HBASE_HOME}/bin:$PATH

                source /etc/profile


        ![](https://i.imgur.com/fmeH4tc.png)
    


2.

- 注意事项
    ![](https://i.imgur.com/RKkQnT1.png) 


### 常用的shell命令 ###


null







- 