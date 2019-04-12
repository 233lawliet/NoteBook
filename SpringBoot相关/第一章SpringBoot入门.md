# SpringBoot简介 #

### 内涵 ###

- Spring开发的框架
- Spring的技术栈的大整合
- J2EE的一站式解决方案 

### 背景 ###

- J2EE开发笨重
- 配置繁琐
- 开发效率低下
- 复杂的部署流程
- 第三方技术难度大


### 解决方案 ###

- SpringBoot全家桶
- springBoot->J2EE一站式解决方案
- springCloud->分布式整体解决方案




### 优点 ###

- 快速创建独立的spring继承的项目
- 使用嵌入式的servlet容器，应用只需要打包成jar即可
- starters（启动器）自动依赖和版本控制
- 大量的自动配置，简化配置，可以修改默认
- 无需配置xml,无代码生成
- 准生产环境的监控
- 与云计算天然集成



### 缺点 ###

- 入门容易，精通难





# 微服务简介 #


**内涵**

- microservice
- 是一种架构风格，
- 一个服务应该是一种小型服务，可以通过HTTP的方式进行互相访问
- 每个小型的服务都可独立替换和独立升级


# 环境设置 #


### maven设置 ###

profiles下添加profile

    <profile>
      <id>jdk-1.8</id>
      <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
      </activation>
      <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      </properties>
    </profile>



idea中指定maven

- idea->configration中指定idea的maven和config






# SpringBoot项目的hello World #


1. 创建简单的maven项目
2. 引入pom依赖

          <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>1.5.9.RELEASE</version>
        </parent>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
        </dependencies>

3. 创建主程序
4. 写controller
5. 测试访问
6. 添加打包的plugin


            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>


7. maven-plugin的生命周期里打包
8. java -jar 包名.jar



# HelloWorld探究 #

## 1.POM文件 ##

### 依赖的父文件 ###


    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
    </parent>

parent--版本仲裁中心

    <!--版本仲裁中心：
     管理springboot所有的依赖版本，
     所以：引用spring boot管理的依赖，不需要配置版本号-->
    <parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-dependencies</artifactId>
		<version>1.5.9.RELEASE</version>
		<relativePath>../../spring-boot-dependencies</relativePath>
	</parent>



### 依赖的文件 ###

web--场景启动器

        <!--web场景启动器starter
          引入了springboot中web模块所需要的依赖pom包
         -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>



**注释**  

springBoot把所有的场景都抽象出来，做成一个个的启动器starters,只需要导入相关的依赖就可以，用什么功能启用什么启动器




## 主程序类 ##




- @**SpringBootApplication**:    Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；



        java
        @Target(ElementType.TYPE)
        @Retention(RetentionPolicy.RUNTIME)
        @Documented
        @Inherited
        @SpringBootConfiguration
        @EnableAutoConfiguration
        @ComponentScan(excludeFilters = {
          @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
          @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
       public @interface SpringBootApplication {
    
- @**SpringBootConfiguration**:
    - Spring Boot的配置类；
    - 标注在某个类上，表示这是一个Spring Boot的配置类；
    - @**Configuration**:配置类上来标注这个注解； 
    - 配置类 -----  配置文件；配置类也是容器中的一个组件；
​

- @**EnableAutoConfiguration**：

    - 开启自动配置功能；  
    - 以前我们需要配置的东西，Spring Boot帮我们自动配置；
 
- @**EnableAutoConfiguration**告诉SpringBoot开启自动配置功能；这样自动配置才能生效；
    
        
           @AutoConfigurationPackage
           @Import(EnableAutoConfigurationImportSelector.class)
           public @interface EnableAutoConfiguration {
   

- @**AutoConfigurationPackage**：自动配置包
​- @**Import**(AutoConfigurationPackages.Registrar.class)：

    - Spring的底层注解@Import，给容器中导入一个组件；导入的组件由AutoConfigurationPackages.Registrar.class；

    - 将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器；==

​- @**Import**(EnableAutoConfigurationImportSelector.class)；  

​- @**EnableAutoConfigurationImportSelector**：  

 - 将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中；
​   - 会给容器中导入非常多的自动配置类（xxxAutoConfiguration）；就是给容器中导入这个场景需要的所有组件，并配置好这些组件；		
有了自动配置类，免去了我们手动编写配置注入功能组件等的工作；



==Spring Boot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；==以前我们需要自己配置的东西，自动配置类都帮我们；

J2EE的整体整合解决方案和自动配置都在spring-boot-autoconfigure-1.5.9.RELEASE.jar；



# **Spring Initializer快速创建SpringBoot项目** #

### 特点 ###

- 主程序已经写好，主要写逻辑。


### 文件结构 ###

- static:存放静态资源：html,js,css,image
- templates:保存模板页面，(SpringBoot默认使用嵌入式d的tomcat,不支持jsp，可以使用模板引擎)
- application.properties保存的spring配置信息


**注释：**  

- @ResponseBody直接加给类==类的所有方法都是写给浏览器的，若果是对象转化成json。
- @Controller+@ResponseBody==@RestController