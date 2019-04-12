# 配置 #



## 配置文件 ##

- SpringBoot使用了全局配置文件。
配置文件是src/main/resources下：
    - application.properties
    - application.yml

- 配置文件的作用：大多数使用了xxxx.xml文件



## YAML ##


### 基本介绍 ###

- YAML Ain't Mark Language
- 以数据为中心，比json和xml更适合做配置文件  

        server:
            port: 8081
            path:/hello




### 基本语法 ###
- 结构表示k: v   **（：后空格不可少）**
- 只要是左对齐的一列数据，都是同一个层级的
- 属性和值大小写敏感

### 值的写法 ###

#### 普通的值：（数字，字符串，布尔） ####

- k: v直接写出来
- v是一般不加单引号或者双引号
    - ""的v的内容可以进行转义。  例如：name: "a \n b"-->name= a 换行 b
    - ''的v的内容不可以进行转义。 例如：name: 'a \n b' --> name=a \n b



#### 对象，Map（属性和值）（键值对） ####

- yaml写法

        lilutong:
               age= 18
               gen= 男


- 行内写法：

        lilutong {age: 18,gen: 男}


**数组，集合（List,Set）:**

- yaml写法

        animals:
               - cat
               - dog


- 行内写法：
        
       animals: [cat,dog]


## 配置的引入方式 ##

- applicaton.properties或者application.yml自动装配
- @configrationproperties引用yml或者properties文件
- @value引用数据或者特定yml或者properties文件
- @importResource引用xml文件
- @RPropertySource引用properties文件
- 配置类的方式（推荐）



### ConfigurationProperties引入数值 ###

1. 书写配置信息

        person:
          name: lilutong
          age: 18
          boss: false
          birth: 1000/10/10
        
          maps:
            k1: v1
            k2: v2

2. 填写bean的信息（和配置进行对应）

        public class Person {
            private String  name;
            private Integer age;
            private Date birth;
            private boolean boss;       
            private Map<String,Object> maps;
        }


3. 给bean添加注解


        @Component
        @ConfigurationProperties(prefix = "person")
        //config的注解需要引入依赖
        //component是使对象类添加入容器
        //可进行bean注解添加（加入lombok依赖和@注解）

        <!--配置文件处理器-->
        <dependency>
            <groupId> org.springframework.boot </groupId>
            <artifactId> spring-boot-configuration-processor </artifactId>
            <optional> true </optional>
        </dependency>


        <!--关于bean的注解引入-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>   

4. 使用springBoot单元测试类 
   
        @Autowired
        Person person;
        @Test
        public void contextLoads() {
    
            System.out.println(person.toString());
    
    
        }



### @Value()引入数值 ###

三种@value方式

- @value("值")  直接配置参数
- @value("${}") 引用配置或者环境变量
- @value{"#{SPEL}"} 使用spEL方式

        @Value("${name}")
        private String  name;
        @Value("#{1+2}")
        private Integer age;
        @Value("true")
        private boolean boss;

### @configrationProperties和@value区别 




- @configration:
    - 可以批注释
    - 支持松散绑定(大驼峰,下划线,-格式)
    - JSR303数据校验支持
    - 复杂类型封装（map,set等）
    - 不支持SPEL
- @value:
    - 一个个注释
    - 不支持松散绑定，
    - 不支持JSR303数据校验
    - 不支持复杂类型封装
    - 支持SPEL

**注释：**

- 一般常用@configrationProperties
- 单独引入某个值时，才引入@value

**注释：JS303数据校验**
   
- 对类进行校验（@Validated
- 对方法校验方式 （@Email等

        @Component
        @ConfigurationProperties(prefix = "person")
        @Validated
        public class Person {
            private String  name;
            private Integer age;
            private boolean boss;
            private Date birth;
            @Email
            private String email;



### 加载指定的properties文件 ###


#### 加载指定的properties文件 ####

- 在sources下添加指定的配置文件 （必须时properties类型的文件）
- 在bean的class设置加载指定的配置文件
    
        @PropertySource("classpath:person.properties")


### 加载指定的xml文件 ###

- 在sources下添加指定的配置文件 （必须为xml类型的文件）
- 在bean的class设置加载指定的配置文件


        @ImportResource(locations = "classpath:bean.xml")

### \*开发过程中推荐配置类添加spring添加组件 ###


- 声明该类为配置类@Configration（代替了配置文件xml,properties）
- 实力化对象@Bean进行注入容器


        @Configuration
        public class BeanConfig {
        
            @Bean
            public Person addPerson(){
                Person person=new Person();
                person.setName("lilutong");
                person.setAge(100);
                return  person;
            }
         }


**注释：get,set函数的使用需要lombok plugin的idea插件**


## 关于配置取值 ##

### 随机数 ###

- age=${random.int}
- newage=${random.int(10)}
- nnewage=${random.int[1,100]}

### 引用其他的字符 ###

- ${name}
- $其他字符没有可指定默认 ${name:lutong}


## Profile配置 ##

### 多Profile配置 ###

- 我们在主配置文件编写的时候，可以创建多个properties文件
- 格式：application-xxxx.properties
- 默认是启动application.properties文件
- 需要在里面设置：spring.profiles.active=dev来确定激活的配置文件

        例如：
        application.properties
        application-dev.properties
        application-prod.properties
 


### 激活配置文件方式 ###

- properties激活：需要在application.properties里面设置：spring.profiles.active=dev/prod来确定激活的配置文件
- yml激活：利用文档块的方式区分，激活

        server:
          port: 8081
        spring:
          profiles:
            active: dev
        ---
        
        server:
          port: 8082
        spring:
          profiles: dev
        ---
        server:
          port: 8083
        spring:
          profiles: prod
        ---
        
- 命令行参数或者虚拟机参数激活：
    - run/build configrations->program argument添加 --spring.profiles.active=dev  
    - run/build configrations->VM potions添加 --spring.profiles.active=dev  

- 运行jar时命令行的方式激活：java -jar spring.jar --spring.profiles.active=dev


## springBoot配置的加载位置 ##



### 自动加载的路径 ###

- 项目目录/config/application.properties
- 项目目录/application.properties
- 项目目录/…/resources/config/application.properties
- 项目目录/…/resources/application.properties


**项目邮箱优先级逐级递减，高级会覆盖低级，可以形成互补配置，低级也会运行**

**注释：classpath <=> resources**



### 改变默认的配置文件位置 ###

- 在高级配置使用：spring.config.location=本地url
- 应用场景：项目打包好后，引用本地配置和项目默认的配置形成互补配置