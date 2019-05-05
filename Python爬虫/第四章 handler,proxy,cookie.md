## handler处理器，定制请求头 ##

- 请求头对象构建
    - url获取
    - headers获取
    - 封装为reqeust对象

- 自定义handler对象构建
- open对象构建  //用于发送信息
- 发送信息，获取response对象


        import urllib.request
        import urllib.error
        
        #url
        url="http://www.baidu.com/"
        
        #headers
        headers={
            'User-Agent':' Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36'
        }
        
        
        #请求对象
        request=urllib.request.Request(url=url,headers=headers)
        
        #创建头部自定义handler
        handler=urllib.request.HTTPHandler()
        
        #创建发送对象opener
        opener=urllib.request.build_opener(handler)
        
        #发送请求，获取返回对象
        response=opener.open(request)
        
        print(response.read().decode())



## 代理 ##


### 分类 ###

- 正向代理
    - 提客户端发送请求的代理。
- 反向代理
    - 代理服务端提供服务。



### 代理IP分类 ###

- 透明代理
    - 服务器知道你使用了代理，知道你的真实IP

- 匿名代理
    - 服务器知道你使用了代理，但是不知道你的真实IP

- 高匿代理
    - 服务器不知道你使用了代理，不知道你的真实IP


### 代理使用 ###

- 创建request对象
    - 填写url
    - 填写headers
    - 封装成request
- *创建代理handler
- 创建发送对象open
- open发送信息，获取返回对象

    
        import urllib.request
        import urllib.error
        
        #url
        url="http://www.baidu.com/"
        
        #headers
        headers={
            'User-Agent':' Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36'
        }
        
        
        #请求对象
        request=urllib.request.Request(url=url,headers=headers)
        
        #创建头部自定义handler
        handler=urllib.request.ProxyHandler({"http":"114.215.95.188：3128"})
        #创建发送对象opener
        opener=urllib.request.build_opener(handler)
        
        #发送请求，获取返回对象
        response=opener.open(request)
        
        print(response.read().decode())
    
    


## cookie ##

    
- 用户身份的认证。
- 用于登录后，对网站其他信息的访问。
- 放在headers中，一起发送。


### 实现方法1 ###
- 拦截的方法，捕获cookie

- 流程
    1. 构建请求头对象request
        - url
        - headers
        - data  //编码处理
    2. handler的构建
    3. opener的构建
    4. 发送请求，捕获response

        
            import urllib.request
            import urllib.error
            
            #url
            url="http://www.renren.com/970648536/profile"
            
            #headers
            headers={
                'User-Agent':' Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36',
                'cookie':'anonymid=jv7nvww8ty9hrg; depovince=GW; _r01_=1; JSESSIONID=abclLtVL1-PSi9lSfq8Pw; ick_login=a01b335f-d241-4389-9a03-99cdf0a4b22e; t=901333b3b52575122e082975bc91c2d36; societyguester=901333b3b52575122e082975bc91c2d36; id=970648536; xnsid=83bf854c; jebecookies=6135bd0e-2432-4be0-bd08-87c7c966bd27|||||; ver=7.0; loginfrom=null; jebe_key=85140b92-ef07-481d-8a43-1db8bd5a103c%7Caf6966bbcd63631a04b1e1602a140637%7C1556862770762%7C1%7C1556862767115; wp_fold=0'
            }
            
            
            #请求对象
            request=urllib.request.Request(url=url,headers=headers)
            
            #创建头部自定义handler
            handler=urllib.request.HTTPHandler()
            #创建发送对象opener
            opener=urllib.request.build_opener(handler)
            
            #发送请求，获取返回对象
            response=opener.open(request)
            
            #write
            with open("renren.html","wb") as f:
                f.write(response.read())



### 实现方法2： ###

- 方法
    1. 先发送账号，密码实现登录界面。
    2. 然后捕获cookie，访问其他页面


- 流程
    1. 构建请求头对象request
        - url
        - headers
        - data  //编码处理
    2. cookiesjar的构建 
    3. 通过cookjar对象构建handler
    4. opener的构建
    5. 发送请求1，捕获response1，捕获cookies
    6. 发送请求2，捕获response



                import urllib.request
                import urllib.parse
                import urllib.error
                import http.cookiejar
                
                
                
                
                
                #创建cookjar对象
                cj=http.cookiejar.CookieJar()
                
                #通过cookieJar创建handler
                handler=urllib.request.HTTPCookieProcessor(cj)
                
                #创建发送对象opener
                opener=urllib.request.build_opener(handler)
                
                #url
                url="https://www.renren.com/ajaxLogin/login?1=1&uniqueTimestamp=2019451641716"
                
                #headers
                headers={
                    'User-Agent':' Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36',
                    'cookie':'anonymid=jv7nvww8ty9hrg; depovince=GW; _r01_=1; JSESSIONID=abclLtVL1-PSi9lSfq8Pw; ick_login=a01b335f-d241-4389-9a03-99cdf0a4b22e; t=901333b3b52575122e082975bc91c2d36; societyguester=901333b3b52575122e082975bc91c2d36; id=970648536; xnsid=83bf854c; jebecookies=6135bd0e-2432-4be0-bd08-87c7c966bd27|||||; ver=7.0; loginfrom=null; jebe_key=85140b92-ef07-481d-8a43-1db8bd5a103c%7Caf6966bbcd63631a04b1e1602a140637%7C1556862770762%7C1%7C1556862767115; wp_fold=0'
                }
                
                #data
                data={
                    "email":"17854174136",
                    "icode":"",
                    "origURL":"http://www.renren.com/home",
                    "domain":"renren.com",
                    "key_id":"1",
                    "captcha_type":"web_login",
                    "password":"0e2a15ab03b4f490bca010271bdb03a3d4036ee5a2e00b613c404bc326aece8e",
                    "rkey":"e1caf15d859d1ba14bc74b79f0ef8545"
                }
                data=urllib.parse.urlencode(data).encode()
                
                #请求对象
                request=urllib.request.Request(url=url,headers=headers,data=data)
                
                
                
                #发送请求，获取返回对象
                response=opener.open(request)
                
                
                print(response.read())
                print("**************************")
                
                #重新获取登录内容的信息
                
                #url
                profileurl="https://www.renren.com/970650217/profile"
                
                #request对象
                
                newRequest=urllib.request.Request(url=profileurl,headers=headers)
                
                newResponse=opener.open(newRequest)
                
                print(newResponse.read().decode())
        











- 