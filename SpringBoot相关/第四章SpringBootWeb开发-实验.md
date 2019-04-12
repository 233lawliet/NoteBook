

## 静态资源引用 ##


- 方式1：静态文件夹下资源引用

    - 资源声明
        
            static-|-asserts-|-css-|……
                             |-img-|……
                             |-js=|……      
 

 

    - 资源引用           
            
            <head>
        		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        		<meta name="description" content="">
        		<meta name="author" content="">
        		<title>Signin Template for Bootstrap</title>
        		<!-- Bootstrap core CSS -->
        		<link href="asserts/css/bootstrap.min.css" rel="stylesheet">
        		<!-- Custom styles for this template -->
        		<link href="asserts/css/signin.css" rel="stylesheet">
        	</head>
   
- 方式2：pom文件引用


    - 资源声明

    
            <!--引入静态资源:webjar-jquery-->
            <dependency>
                <groupId>org.webjars</groupId>
                <artifactId>jquery</artifactId>
                <version>3.3.1-1</version>
            </dependency>
    
            <dependency>
                <groupId>org.webjars</groupId>
                <artifactId>bootstrap</artifactId>
                <version>4.0.0</version>
            </dependency>
    
            <!--thymeleaf模板引入-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-thymeleaf</artifactId>
            </dependency>


    - 资源引入  
        - thymeleaf模板引用
        - 静态资源herf调用



                <html lang="en" xmlns:th="http://www.thymeleaf.org">
                
                <link  th:href="@{webjars/bootstrap/4.0.0/css/bootstrap.css}" rel="stylesheet">
                		<!-- Custom styles for this template -->
                <link th:href="@{asserts/css/signin.css}" rel="stylesheet">
                <！--静态资源-->
                <img class="mb-4" th:src="@{asserts/img/bootstrap-solid.svg}"  alt="" width="72" height="72">
		


## 国际化 ##

### springmvc配置国际化 ###

1. 编写国际化配置文件
2. 使用ResourceBundleMessageSource管理国际化配置文件
3. 在页面使用fmt:message取出国际化内容


### springBoot设置国际化文件 ###

1. 设置配置文件
    - 在resources下创建i18n的目录
    - 在i18n下创建：login.properties,login_zh_CN.properties,login_en_US.properties三个配置文件

 ![](https://i.imgur.com/IoS5g2Y.png)

2. 添加信息
    - 打开 resources bundle
    - 添加字段：默认字段，英文字段，中文字段 

![](https://i.imgur.com/nrAhEQT.png)

3. 配置下绑定访问名称
   
        spring.messages.basename=i18n.login

4. 页面获取国际化的值
    - th:text="#{配置名称}" 
    - th:placeholder="#{配置名称}"
    - [[#{配置名称}]]


    			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}"></h1>
    			<input type="text" class="form-control" th:placeholder="#{login.user}" required="" autofocus="">
    			<label class="sr-only" th:text="#{login.password}"></label>
    			<input type="password" class="form-control"th:placeholder="#{login.password}"  required="">
    			<div class="checkbox mb-3">
    				<label>
                  <input type="checkbox" value="remember-me" /> [[#{login.remember}]]
            </label>
    			</div>
    			<button class="btn btn-lg btn-primary btn-block" type="submit" th:text="#{login.sign}"></button>
    			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
    			<a class="btn btn-sm">中文</a>
    			<a class="btn btn-sm">English</a>
    		


### 实现点击链接改变国际化 ###

1. 配置连接发送的请求
    - 模板格式：页面(名称="数据")
        <a class="btn btn-sm" th:href="@{/index.html(l='zh_CN'})">中文</a>
        			<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>


2. 编写自己的地域解析器
    - 配置文件中创建解析器类
    - 编写解析文件
    
            package com.lutong.springboot.config;
            
            import org.springframework.util.StringUtils;
            import org.springframework.web.servlet.LocaleResolver;
            
            import javax.servlet.http.HttpServletRequest;
            import javax.servlet.http.HttpServletResponse;
            import java.util.Locale;
            
            /**
             * @author lutong
             * @date 2019/1/25 - 14:16
             */
            public class MyLocaleResolver implements LocaleResolver {
            
                @Override
                public Locale resolveLocale(HttpServletRequest httpServletRequest) {
                    String l=httpServletRequest.getParameter("l");
                    Locale locale=Locale.getDefault();
                    if(!StringUtils.isEmpty(l)){
                        String split[]=l.split("_");
                        locale=new Locale(split[0],split[1]);
                    }
            
            
                    return locale;
                }
            
                @Override
                public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {
            
                }
            }

3. 在mvc配置中，接管自己的地域解析器配置

        
        
        
        @Configuration
        public class MyMvcConfig implements WebMvcConfigurer {
        
            //自己的视图解析器  未加入
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("ssuu").setViewName("success");
            }
        
            //自己的地域解析器
            @Bean
            public LocaleResolver  getMyLocaleResolver(){
                return new MyLocaleResolver();
            }
        }


## 登录 ##


### html页面实时更新 ###
1. 禁掉缓存

        #不使用模板引擎的缓存，从而直接修改html就可以使用相应修改
        spring.thymeleaf.cache=false
2. ctrl+F9重新编译

### 配置登录视图解析器 ###

        @Configuration
        public class MyMvcConfig implements WebMvcConfigurer {
        
            @Override
            public void addViewControllers(ViewControllerRegistry registry) {
                registry.addViewController("ssuu").setViewName("success");
            }
        
            //增加视图解析器
        
            @Bean
            public WebMvcConfigurerAdapter webMvcConfigurerAdapter(){
                WebMvcConfigurerAdapter webMvcConfigurerAdapter=new WebMvcConfigurerAdapter() {
                    @Override
                    public void addViewControllers(ViewControllerRegistry registry) {
                        registry.addViewController("").setViewName("login");
                        registry.addViewController("user/login").setViewName("login");
                        registry.addViewController("index.html").setViewName("login");
                        registry.addViewController("main.html").setViewName("dashboard");
                    }
                };
                return webMvcConfigurerAdapter;
            }
        
            //自己的地域解析器
            @Bean
            public LocaleResolver  getMyLocaleResolver(){
                return new MyLocaleResolver();
            }
        }

### 控制器配置 ###

    @Controller
    public class LoginController {
    
    
        @RequestMapping(value = "user/login",method = RequestMethod.POST)
        public String login(@RequestParam String username,
                            @RequestParam String password,
                            Map<String,String> map){
            if(!StringUtils.isEmpty(username)&&"123456".equals(password)){
    
                //防止表单重复提交，进行重定向
                return "redirect:/main.html";
    
                //return "dashboard";
            }else{
                map.put("message","登陆错误");
                return  "redirect:/index.html";
            }
    
        }
    }


### 制作登录拦截器 ###

1. 在loginController获取httpSession
2. 创建拦截器的java配置

        
        public class LoginHandleInterceptor implements HandlerInterceptor {
        
            @Override
            public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
                Object user =request.getSession().getAttribute("username");
                Object password =request.getSession().getAttribute("password");
                if(StringUtils.isEmpty(user)){
                    //未登录
                    request.getRequestDispatcher("/index.html");
                    //已经登陆
                    request.setAttribute("message","没有权限，请登录");
                    return false;
                }else{
                    return  true;
                }
            }
        
            @Override
            public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        
            }
        
            @Override
            public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        
            }
        }



3. 在之前的mvc中的webMvcConfigurerAdapter适配器配置拦截器
    - 注：ctrl+O添加继承方法

            //增加视图解析器
        
            @Bean
            public WebMvcConfigurerAdapter webMvcConfigurerAdapter(){
                WebMvcConfigurerAdapter webMvcConfigurerAdapter=new WebMvcConfigurerAdapter() {
                    @Override
                    public void addInterceptors(InterceptorRegistry registry) {
                        //添加所有请求，排除登录等
                        //springboot已经做好了静态资源影射了，不需要处理静态资源了
                        registry.addInterceptor(new LoginHandleInterceptor()).addPathPatterns("/**")
                        .excludePathPatterns("index.html","/","login","login.html");
                    }
        
                    @Override
                    public void addViewControllers(ViewControllerRegistry registry) {
                        registry.addViewController("").setViewName("login");
                        registry.addViewController("index.html").setViewName("login");
                        registry.addViewController("main.html").setViewName("dashboard");
                    }
        
        
                };
        
                return webMvcConfigurerAdapter;
            }




## 增删改查 ##



### 普通CRUD和RestfulCRUD风格的比较 ###

URL:/资源名称/资源标识  HTTP请求方式区分对资源的CRUD操作

- 查询
    - 普通crud：getEmp
    - restfulcrud:emp-----GET
    

- 添加
    - 普通crud：addEmp?xxx
    - restfulcrud:emp-----POST
    
- 修改
    - 普通crud：updateEmp?id=xxx&xxx=xx
    - restfulcrud:emp/{id}-----PUT
    

- 删除
    - 普通crud：deleteEmp?xx=xxx
    - restfulcrud:emp/{id}-----DELETE
    


### thymeleaf使用模板模板 ###

#### 模板的插入 ####

1. 引用页面和被引用页面都配置thymeleaf引用
2. 抽取公共片段

    <div th:framgment='name1'>
        
    </div>

3. 引入公共片段

    - insert：将公共的片段插入到整个元素中


            <div th:insert="~{文件名 :: name1}">
            
            </div>

            或者
            <div th:insert="文件名 :: name1">
            
            </div>



    - replace：将公共的片段进行替换

            <div th:replace="~{文件名 :: name1}">
            
            </div>

    - include ：将声明引入的片段包含进这个标签

            <div th:include="~{文件名 :: name1}">
            
            </div>


- 举例
    - 引入原来模板thymeleaf依赖

            <html lang="en" xmlns:th="http://www.thymeleaf.org">

    - 配置模板

             <div th:replace="~{common/bar:: topbar}"></div>

   - 引入后来模板thymeleaf依赖

            <html lang="en" xmlns:th="http://www.thymeleaf.org">

    - 配置引用

			<div class="sidebar-sticky" th:insert="~{common/bar::leftbar}">




#### thymeleaf模板的参数传递 ####


- 数值传递

        <div class="sidebar-sticky" th:insert="~{common/bar::leftbar(activeUrl='emps.html')}">


- 数值引用

        <a class="nav-link active" th:class="${activeUrl=='main.html'}?'nav-link active':'nav-link'" th:href="@{main.html}" >
             

#### thymeleaf的each使用 ####


     <tbody>
    	<tr th:each="emp:${emps}">
    		<td th:text="${emp.getId()}"></td>
    		<td th:text="${emp.getLastName()}"></td>
    		<td th:text="${emp.getGender()==0?'女':'男'}"></td>
    		<td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm')}"></td>
    		<td th:text="${emp.getDepartment().getDepartmentName()}"></td>
            <td>
    			<button class="btn btn-sm btn-primary">
    				编辑
    			</button>
    			<button class="btn btn-sm btn-danger">
    				删除
    			</button>
    		</td>
    	</tr>
    </tbody>

    //emp的数据是controller的model传输过来的



### 发送put请求过程 ###

1. SpringMVC中配置HiddenHttpMethodFilter;（SpringBoot自动配置好的）
2. 页面创建一个post表单

	    <input type="hidden" name="_method" value="put" th:if="${emp!=null}"/>
        //只有修改的时候才会有put标签

3. 创建一个input项，name="_method";值就是我们指定的请求方式
   
         <input type="hidden" name="_method" value="put" th:if="${emp!=null}"/>
         <input type="hidden" name="id" th:if="${emp!=null}" th:value="${emp.id}">

4. controller的书写

        @PutMapping("emp")
            public String updateEmp(Employee employee){
                employeeDao.save(employee);
                //调用函数
                return "redirect:/emps";
            }



### 关于整个项目的代码 ###



- 前端

        <!DOCTYPE html>
        <!-- saved from url=(0052)http://getbootstrap.com/docs/4.0/examples/dashboard/ -->
        <html lang="en" xmlns:th="http://www.thymeleaf.org">
        
        	<head>
        		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        		<meta name="description" content="">
        		<meta name="author" content="">
        
        		<title>Dashboard Template for Bootstrap</title>
        		<!-- Bootstrap core CSS -->
        		<link href="asserts/css/bootstrap.min.css" rel="stylesheet">
        
        		<!-- Custom styles for this template -->
        		<link href="asserts/css/dashboard.css" rel="stylesheet">
        		<style type="text/css">
        			/* Chart.js */
        			
        			@-webkit-keyframes chartjs-render-animation {
        				from {
        					opacity: 0.99
        				}
        				to {
        					opacity: 1
        				}
        			}
        			
        			@keyframes chartjs-render-animation {
        				from {
        					opacity: 0.99
        				}
        				to {
        					opacity: 1
        				}
        			}
        			
        			.chartjs-render-monitor {
        				-webkit-animation: chartjs-render-animation 0.001s;
        				animation: chartjs-render-animation 0.001s;
        			}
        		</style>
        	</head>
        
        	<body>
        		<!--引入抽取的的片段topbar-->
        		<!---->
                 <div th:replace="~{common/bar:: topbar(activeUrl='main.html')}"></div>
        
        		<div class="container-fluid">
        			<div class="row">
        				<nav class="col-md-2 d-none d-md-block bg-light sidebar">
        					<!--左侧边栏-->
        					<div class="sidebar-sticky" th:insert="~{common/bar::leftbar(activeUrl='emps.html')}">
        
        
        						<h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
                      <span>Saved reports</span>
                      <a class="d-flex align-items-center text-muted" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-plus-circle"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
                      </a>
                    </h6>
        						<ul class="nav flex-column mb-2">
        							<li class="nav-item">
        								<a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
        									<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
        										<path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
        										<polyline points="14 2 14 8 20 8"></polyline>
        										<line x1="16" y1="13" x2="8" y2="13"></line>
        										<line x1="16" y1="17" x2="8" y2="17"></line>
        										<polyline points="10 9 9 9 8 9"></polyline>
        									</svg>
        									Current month
        								</a>
        							</li>
        							<li class="nav-item">
        								<a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
        									<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
        										<path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
        										<polyline points="14 2 14 8 20 8"></polyline>
        										<line x1="16" y1="13" x2="8" y2="13"></line>
        										<line x1="16" y1="17" x2="8" y2="17"></line>
        										<polyline points="10 9 9 9 8 9"></polyline>
        									</svg>
        									Last quarter
        								</a>
        							</li>
        							<li class="nav-item">
        								<a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
        									<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
        										<path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
        										<polyline points="14 2 14 8 20 8"></polyline>
        										<line x1="16" y1="13" x2="8" y2="13"></line>
        										<line x1="16" y1="17" x2="8" y2="17"></line>
        										<polyline points="10 9 9 9 8 9"></polyline>
        									</svg>
        									Social engagement
        								</a>
        							</li>
        							<li class="nav-item">
        								<a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
        									<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
        										<path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
        										<polyline points="14 2 14 8 20 8"></polyline>
        										<line x1="16" y1="13" x2="8" y2="13"></line>
        										<line x1="16" y1="17" x2="8" y2="17"></line>
        										<polyline points="10 9 9 9 8 9"></polyline>
        									</svg>
        									Year-end sale
        								</a>
        							</li>
        						</ul>
        					</div>
        				</nav>
        
        				<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
        					<h2>
        						<a class="btn btn-sm btn-success" th:href="@{/emp}">
        						  员工添加
        						</a>
        					</h2>
        					<div class="table-responsive">
        						<table class="table table-striped table-sm">
        							<thead>
        								<tr>
        									<th>#</th>
        									<th>Header</th>
        									<th>Header</th>
        									<th>Header</th>
        									<th>Header</th>
        								</tr>
        							</thead>
        							<tbody>
        								<tr th:each="emp:${emps}">
        									<td th:text="${emp.getId()}"></td>
        									<td th:text="${emp.getLastName()}"></td>
        									<td th:text="${emp.getGender()==0?'女':'男'}"></td>
        									<td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm')}"></td>
        									<td th:text="${emp.getDepartment().getDepartmentName()}"></td>
                                            <td>
        										<a class="btn btn-sm btn-primary"  th:href="@{/emp/}+${emp.getId()}">
        											编辑
        										</a>
        										<button id="deleteBtn" th:attr="del_url=@{/emp/}+${emp.getId()}"  class="btn btn-sm btn-danger">
        												删除
        										</button>
        
        
        									</td>
        								</tr>
        							</tbody>
        						</table>
        					</div>
        				</main>
        			</div>
        		</div>
        		<form id="deleteForm"  method="post">
        			<!--设置好delete方式-->
        			<input  type="hidden" name="_method" value="delete">
        		</form>
        
        
        		<!-- Bootstrap core JavaScript
            ================================================== -->
        		<!-- Placed at the end of the document so the pages load faster -->
        		<script type="text/javascript" src="asserts/js/jquery-3.2.1.slim.min.js"></script>
        		<script type="text/javascript" src="asserts/js/popper.min.js"></script>
        		<script type="text/javascript" src="asserts/js/bootstrap.min.js"></script>
        
        		<!-- Icons -->
        		<script type="text/javascript" src="asserts/js/feather.min.js"></script>
        		<script>
        			feather.replace()
        		</script>
        
        
        		<!--删除表单的提交-->
        		<script>
        			$(".btn").click(function(){
        				console.log($(this).attr("del_url"));
        				$("#deleteForm").attr("action",$(this).attr("del_url")).submit();
        
        				return false;
        			});
        		</script>
        
        
        		<!-- Graphs -->
        		<script type="text/javascript" src="asserts/js/Chart.min.js"></script>
        		<script>
        			var ctx = document.getElementById("myChart");
        			var myChart = new Chart(ctx, {
        				type: 'line',
        				data: {
        					labels: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
        					datasets: [{
        						data: [15339, 21345, 18483, 24003, 23489, 24092, 12034],
        						lineTension: 0,
        						backgroundColor: 'transparent',
        						borderColor: '#007bff',
        						borderWidth: 4,
        						pointBackgroundColor: '#007bff'
        					}]
        				},
        				options: {
        					scales: {
        						yAxes: [{
        							ticks: {
        								beginAtZero: false
        							}
        						}]
        					},
        					legend: {
        						display: false,
        					}
        				}
        			});
        		</script>
        
        	</body>
        
        </html>




- 后端


        package com.lutong.springboot.controller;
        
        import com.lutong.springboot.bean.Department;
        import com.lutong.springboot.bean.Employee;
        import com.lutong.springboot.dao.DepartmentDao;
        import com.lutong.springboot.dao.EmployeeDao;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.stereotype.Controller;
        import org.springframework.ui.Model;
        import org.springframework.web.bind.annotation.*;
        
        import java.util.Collection;
        
        /**
         * @author lutong
         * @date 1/31/2019 - 9:19 PM
         */
        
        @Controller
        public class EmpController {
        
            @Autowired
            EmployeeDao employeeDao;
        
            @Autowired
            DepartmentDao  departmentDao;
        
            //list页面
            @GetMapping("emps")
            public  String getAllEmps(Model model){
        
               Collection<Employee> employees=employeeDao.getAll();
               model.addAttribute("emps",employees);
                //thymeleaf引擎转接到temp下的list页面
                return "emp/list";
            }
        
        
            //添加页面
            @GetMapping("emp")
            public String toInsertEmp(Model model){
               Collection<Department>  departments=departmentDao.getDepartments();
               model.addAttribute("depts",departments);
               return "emp/add";
            }
        
            //请求参数会自动和入参对象自动绑定，绑定：要求了请求参数的名字和javabean自动绑定
            @PostMapping("emp")
            public String insertEmp(Employee employee){
        
                employeeDao.save(employee);
                System.out.println("員工的信息"+employee.toString());
                //返回方式：重定向和转发
                //rediect重定向
                //forward转发
                return "redirect:/emps";
            }
        
            //参数捕捉url的id
            @GetMapping("/emp/{id}")
            public String toEditPage(@PathVariable("id") Integer id, Model  model){
                Employee employee=employeeDao.get(id);
                //部门信息
                Collection<Department>  departments=departmentDao.getDepartments();
                model.addAttribute("depts",departments);
                model.addAttribute("emp",employee);
                //修改和添加页面2合1
                return "emp/add";
            }
        
            //员工信息修改
            @PutMapping("emp")
            public String updateEmp(Employee employee){
                employeeDao.save(employee);
                //调用函数
                return "redirect:/emps";
            }
        
        
            @DeleteMapping("/emp/{id}")
            public String deleteEmp(@PathVariable("id") Integer id){
        
                System.out.println("获取的id"+id);
                employeeDao.delete(id);
                return "redirect:/emps";
            }
        
        }
        

