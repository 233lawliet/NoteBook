# Web开发 #



### 使用SpringBoot流程 ###

1. 创建SpringBoot项目，添加所需要的配置
2. springBoot已经加载了相应的场景，只需写相应的配置
3. 自己的业务代码


### 自动配置原理 ###

- xxxautoconfigration：给容器自动配置了相应的组件
- xxxproperties:配置类封装了相应的配置内容







## 静态资源访问 ##

### 第一种：公共静态资源配置 ###

- 例如：外部的前端框架类。


#### 静态资源映射规则的配置 ####


- WebMvcAutoConfigration.class下


        public void addResourceHandlers(ResourceHandlerRegistry registry) {
                    if (!this.resourceProperties.isAddMappings()) {
                        logger.debug("Default resource handling disabled");
                    } else {
                        Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
                        CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
                        if (!registry.hasMappingForPattern("/webjars/**")) {
                            this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{"/webjars/**"}).addResourceLocations(new String[]{"classpath:/META-INF/resources/webjars/"}).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
                        }
        
                        String staticPathPattern = this.mvcProperties.getStaticPathPattern();
                        if (!registry.hasMappingForPattern(staticPathPattern)) {
                            this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{staticPathPattern}).addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations())).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
                        }
        
                    }
                }



#### 公共资源访问 ####

- 所有的静态资源都是使用webjar格式来引入资源


    - 加入静态资源依赖
    
         <!--引入静态资源:webjar-jquery-->
                <dependency>
                    <groupId>org.webjars</groupId>
                    <artifactId>jquery</artifactId>
                    <version>3.3.1-1</version>
                </dependency>


- 资源访问的方式：/webjars/**
    - 目录文件结构组成url：
    
     ![](https://i.imgur.com/9cUgtLI.png)
    - 例如：
    
            http://localhost:8080/webjars/jquery/3.3.1-1/jquery.js



### 第二种：自己的静态资源访问 ###

- 例如：image,自己的html,js等

#### 自己静态资源配置地方 ####


resourcesproperties.class下的
        
   ![](https://i.imgur.com/3bKUOCC.png)



#### *静态资源的文件夹： ####

- classpath:/META-INF/resources
- classpath:/resources/
- classpath:/static/    (默认已经创建)
- classpath:/public/
- / 项目路径


**注释：classpath指的是指定类型resources**


#### 静态资源的访问 ####


- 遍历静态资源文件夹
- 访问方式
    - 不需要加文件夹
    - /**
    - 举例：

            resources/static/jquery.js的访问
            ->   http://localhost:8080/jquery.js

#### 欢迎页的访问 ####

- 遍历所有静态资源下的index.html
- 访问方式
    - 访问http://localhost:8080/默认跳转到欢迎页
    - 也可以通过/**进行访问  即http://localhost:8080/index


#### 页面图标的访问 ####

- 静态资源文件下：设置名字为：favicon.ico
- 页面访问自动设置



#### 修改静态资源路径文件夹 ####

- 修改application.properties

        spring.resources.static-location=classpath:/hello/,classpath:/lutong/
- 会使以前的五个文件夹失效




## 模板引擎 ##


- JSP,Velocity,Freemarker,Thylemeaf
- Thymeleaf使用网站：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html


![](https://i.imgur.com/Mpxedbx.png)


- springBoot支持的thymeleaf模板引擎


### Thymeleaf模板引擎的页面使用 ###

- 默认以下是导入的，可以更改相应的版本


            <!--thymeleaf模板引入-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-thymeleaf</artifactId>
            </dependency>

        <!--控制版本（一般不用）-->
        <properties>
            <thymeleaf.version>3.0.2.RELEASE</thymeleaf.version>
            <thymeleaf-layout-dialect.version>2.1.1</thymeleaf-layout-dialect.version>
        </properties>


- 添加相关的反馈页面
    - 在classpath:/templates下声明相应的html页面
    - 注：配置路径来源

            public class ThymeleafProperties {
                private static final Charset DEFAULT_ENCODING;
                public static final String DEFAULT_PREFIX = "classpath:/templates/";
                public static final String DEFAULT_SUFFIX = ".html";
                private boolean checkTemplate = true;
                private boolean checkTemplateLocation = true;
                private String prefix = "classpath:/templates/";
                private String suffix = ".html";
                private String mode = "HTML";
        

- 相关的controller控制

    - 格式：

            @RequestMapping("/success")
                public String success(){
                    return "success";
                }
            

### Thymeleaf模板引擎的语法使用 ###


- 在html添加thymeleaf的名称空间（目的：代码提示）

        <html xmlns:th="http://www.thymeleaf.org"> 


- controller传入参数


          @RequestMapping("/success")
            public String success(Map<String,Object> map){
                map.put("name","lilutong");
                return "success";
            }
- 使用thymeleaf模板语法

        <div th:text="${name}">用户</div>



### Thymeleaf模板引擎的语法规则 ###


- 可以修改html的相关页面属性

        <div th:id="${name}"  th:class="${name}"   >用户</div>


- 常见语法规则


    ![](https://i.imgur.com/Yp0ZXXm.png)


- 常见表达式  
详情：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#variables
    - ${}
        - 对象的属性
        - map,数组的数值
        - 对象的方法
        - 使用的内置的基本对象  结构${#ctx}
        
                #ctx: the context object.
                #vars: the context variables.
                #locale: the context locale.
                #request: (only in Web Contexts) the HttpServletRequest object.
                #response: (only in Web Contexts) the HttpServletResponse object.
                #session: (only in Web Contexts) the HttpSession object.
                #servletContext: (only in Web Contexts) the ServletContext object.
        - 内置的工具对象

                #execInfo: information about the template being processed.
                #messages: methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
                #uris: methods for escaping parts of URLs/URIs
                #conversions: methods for executing the configured conversion service (if any).
                #dates: methods for java.util.Date objects: formatting, component extraction, etc.
                #calendars: analogous to #dates, but for java.util.Calendar objects.
                #numbers: methods for formatting numeric objects.
                #strings: methods for String objects: contains, startsWith, prepending/appending, etc.
                #objects: methods for objects in general.
                #bools: methods for boolean evaluation.
                #arrays: methods for arrays.
                #lists: methods for lists.
                #sets: methods for sets.
                #maps: methods for maps.
                #aggregates: methods for creating aggregates on arrays or collections.
                #ids: methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).

    - *{}
        - 功能和${}一样
        - 此外：可直接获取对象的属性
           
                ${session.user.firstname}
                *{firstname}

    - \#{}取国际化内容的
    - @{}定义url的
    - ~{}片段引用

- 常见函数运算：

    - Literals（字面量）
    
              Text literals: 'one text' , 'Another one!' 
              Number literals: 0 , 34 , 3.0 , 12.3 
              Boolean literals: true , false
              Null literal: null
              Literal tokens: one , sometext , main 
    - Text operations:（文本操作）
    
                String concatenation: +
                Literal substitutions: |The name is ${name}|
    - Arithmetic operations:（数学运算）
    
            Binary operators: + , - , * , / , %
            Minus sign (unary operator): -
    - Boolean operations:（布尔运算）
    
            Binary operators: and , or
            Boolean negation (unary operator): ! , not
    - Comparisons and equality:（比较运算）
    
            Comparators: > , < , >= , <= ( gt , lt , ge , le )
            Equality operators: == , != ( eq , ne )
    - Conditional operators:条件运算（三元运算符）
    
            If-then: (if) ? (then)
            If-then-else: (if) ? (then) : (else)
            Default: (value) ?: (defaultvalue)
    

### thymeleaf模板引擎使用举例 ###

- 控制+数据传入

          @RequestMapping("/success")
            public String success(Map<String,Object> map){
                map.put("name","lilutong");
                map.put("test","<h2>这是h2</h2>");
                map.put("users", Arrays.asList("lilutong","李路通"));
                return "success";
            } 

- templates下建立页面
- 书写前端显示

        <div th:text="${test}"></div>
        <div th:utext="${test}"></div>
        <h4  th:text="user"   th:each="user:${users}"></h4>
        

## SpringMVC ##



### springBoot进行的默认配置 ###

- ContentNegotiatingViewResolver 和BeanNameViewResolver的默认配置
    - 自动配置了ViewResolver(视图解析器)
    - ContentNogotiatingViewResolver ：组合视图解析器
- Support for serving static resources, including support for WebJars (see below).静态资源文件夹路径,webjars

- Static `index.html` support. 静态首页访问

- Custom `Favicon` support (see below).  favicon.ico

- 自动注册了 of `Converter`, `GenericConverter`, `Formatter` beans.

  - Converter：转换器；  public String hello(User user)：类型转换使用Converter
  - `Formatter`  格式化器；  2017.12.17===Date；


​	==自己添加的格式化器转换器，我们只需要放在容器中即可==

- Support for `HttpMessageConverters` (see below).

  - HttpMessageConverter：SpringMVC用来转换Http请求和响应的；User---Json；

  - `HttpMessageConverters` 是从容器中确定；获取所有的HttpMessageConverter；

    ==自己给容器中添加HttpMessageConverter，只需要将自己的组件注册容器中（@Bean,@Component）==

    ​

- Automatic registration of `MessageCodesResolver` (see below).定义错误代码生成规则

- Automatic use of a `ConfigurableWebBindingInitializer` bean (see below).


### SpringBoot的springMVC配置 ###

- springboot在配置组件的时候，先查看是否有用户配置的，有则用用户的，没有则new一个。如果可以配置多个viewResolver则可以配置多个视图解析器。
- 编写配置类 
    - 编写配置类，设置配置注入
    - 继承webmvcconfiger,增加相应方法
    - 编写控制器


            @Configuration
            public class MyMvcConfig implements WebMvcConfigurer {
            
                @Override
                public void addViewControllers(ViewControllerRegistry registry) {
                    registry.addViewController("ssuu").setViewName("success");
                }
            }




### springMVC原理 ###

- WebMvcAutoConfiguration是SpringMVC的自动配置类  
- 在做其他自动配置时会导入；@Import
​

(**EnableWebMvcConfiguration**.class)

    @Configuration
	public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration {
      private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();

	 //从容器中获取所有的WebMvcConfigurer
      @Autowired(required = false)
      public void setConfigurers(List<WebMvcConfigurer> configurers) {
          if (!CollectionUtils.isEmpty(configurers)) {
              this.configurers.addWebMvcConfigurers(configurers);
            	//一个参考实现；将所有的WebMvcConfigurer相关配置都来一起调用；  
            	@Override
             // public void addViewControllers(ViewControllerRegistry registry) {
              //    for (WebMvcConfigurer delegate : this.delegates) {
               //       delegate.addViewControllers(registry);
               //   }
              }
          }
	}


​	3）、容器中所有的WebMvcConfigurer都会一起起作用；

​	4）、我们的配置类也会被调用；



### 全面接管SpringMVC；

- SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己配置；所有的SpringMVC的自动配置都失效了
-  @EnableWebMvc实现全面接管
- 实际中使用springboot的mvc

**我们需要在配置类中添加@EnableWebMvc即可；**

    //使用WebMvcConfigurerAdapter可以来扩展SpringMVC的功能
    @EnableWebMvc
    @Configuration
    public class MyMvcConfig extends WebMvcConfigurerAdapter {
    
        @Override
        public void addViewControllers(ViewControllerRegistry registry) {
           // super.addViewControllers(registry);
            //浏览器发送 /atguigu 请求来到 success
            registry.addViewController("/atguigu").setViewName("success");
        }
    }
