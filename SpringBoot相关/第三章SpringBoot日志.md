
## 日志介绍 ##

### 市场上日志框架 ###


#### 日志门面（日志抽象层） ####

- JCL(jakarta Commons Logging)   (2014更新，很久了
- jboss-logging （ hibernate使用）
- SLF4J(simple logging facade for java)

#### 日志实现 ####

- JUL(java.unitl.logging)  （应用少，java本身的工具包）
- log4j    (旧版本)
- log4j2
- logback  (log4j的升级版本)


#### 日志实现选择 ####

- 日志门面：SLF4J
- 日志实现：log4j或者logback



### SpringBoot的使用 ###

- springBoot默认使用的时JCL
- 可以选用SLF4j和logback



## SLF4J的使用 ##



### 使用流程 ###

- 添加slf4j的jar和logback的jar
- 使用

        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        
        公共类HelloWorld {
          public static void main（String [] args）{
            Logger logger = LoggerFactory.getLogger（HelloWorld.class）;
            logger.info（“Hello World”）;
          }
        }




### SLF4J与实现日志的配合 ###

![](https://www.slf4j.org/images/concrete-bindings.png)






### 遗留问题：不同日志包的适配问题 ###

#### 问题 ####

- 不同的日志，使用同一的slf4j进行交互
- 引用特定的包进行替换

![](https://www.slf4j.org/images/legacy.png)



#### 解决过程 ####


- 将系统其他的日志包去除
- 使用中间包进行替换，实现接口和原来包的引用
- 导入slf4j的替换



## springBoot日志关系 ##



### 查看springboot使用包的依赖 ###

- Pom.xml文件夹下 使用右键diaggrams->show dependencies



### springboot的log所在的包 ###

- 有依赖关系可以看到


        <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter</artifactId>
        </dependency>

         <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-logging</artifactId>
          <version>2.1.2.RELEASE</version>
          <scope>compile</scope>
        </dependency>



![](https://i.imgur.com/zZJmZgg.png)




### 总结 ###

- springboot默认使用了slf4j.jar和logback.jar
- jul-to-slf4j，log4j-over=slf4j，jcl-over-slf4j是过程包。是其他的包进行转化，适用于slf4j的包，适配于所有的包
- 如要要引入新的框架，一定把这个框架的日志包消除
- SpringBoot能够自动适配所有的日志，



## 日志的使用 ##


- 引入logger对象
- logger级别打印

    
        Logger logger = LoggerFactory.getLogger(getClass());
    
    
        @Test
        public void contextLoads() {
            System.out.println(person.toString());
    
            logger.trace("这是tace信息");
            logger.debug("这是debug信息");
            logger.info("这是info信息");
            logger.warn("这是warn信息");
            logger.error("这是error信息");
    
    
        }
### 级别介绍 ###
- 包括五个级别，由低到高

        logger.trace("这是trace信息");
        logger.debug("这是debug信息");
        logger.info("这是info信息");
        logger.warn("这是warn信息");
        logger.error("这是error信息");


- 可调整logger的级别，调整后只会输出该级别之后的日志信息
- springboot默认使用的是info级别（root级别）



### 配置介绍 ###

在springboot的配置文件添加配置

- 为某个包指定日志级别：logging.level.包名=级别
- 将日志输出到指定的日志文件：
    - logging.path=路径  （常用
    - logging.file=路径+文件 (级别比path高
    - file和path二选一

- 控制台日志输出的格式：logging.pattern.console
- 日志文件输出的格式：logging.pattern.file



### 指定配置文件 ###

- 在resources下添加自己的配置文件
    - logback配置添加logback.xml(日志框架加载)或者logback-spring.xml（spring加载（推荐，有功能扩展））
    - log4j2配置添加log4j2.xml或者log4j2-spring.xml（推荐（log4j2暂时未应用））
    - JDK(java util logging)配置添加logging.properties