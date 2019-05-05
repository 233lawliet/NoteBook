# urllib库 #



## 简介 ##

### 简介 ###

- 模拟浏览器，发送请求
- python自带


### 版本介绍 ###

- pyrhon2:分为urllib和urllib2两个库
- python3:合并为urllib库



## 相关请求函数 ##
### request函数 ###

- 引用库
    import urllib.request
- url：协议+域名+端口+文件+参数+锚点
        
        http://www.baidu.com/index.html?name=123#dian

    ####请求request函数####
    responce=urllib.request.urlopen(url=url)

    urllib.request.urlretrieve(url,"chun.jpg")

### 返回对象responce函数 ###

- read()  //读取全部二进制(字节类型)
- geturl()  //获取请求的url
- getheaders()  //获取头部信息
- getcode()  //获取状态码
- readlines()  //按行读取

   
- 网页下载举例
     
        import urllib.request
        import gzip
        from io import BytesIO
        
        
        url = 'http://www.baidu.com'
        response=urllib.request.urlopen(url=url)
        
        #二进制内容
        binaryHtml = response.read()
        buff = BytesIO(binaryHtml)
        
        
        #解压缩
        f = gzip.GzipFile(fileobj=buff)
        res = f.read().decode('utf-8')
        print(res)
        
        
        print(response.geturl())
        print(dict(response.getheaders()))
        print(response.getcode())
        print(response.readlines())

- 图片下载举例


        import  urllib.request
        
        
        url="http://maoerfei.cn:8000/ershow/wxa4f08677a3b44bf1.o6zAJsyUE-Pqsr-Lrqbvt1Kv-fv4.MC1gtkWdJXIO2330784947265962f972258f43bd01d0.jpg"
        
        
        responce=urllib.request.urlopen(url)
        
        with open("lawliet.jpg","wb") as f:
            f.write(responce.read())

        F2:

        
        import  urllib.request
        
       
        url="http://maoerfei.cn:8000/ershow/wxa4f08677a3b44bf1.o6zAJsyUE-Pqsr-Lrqbvt1Kv-fv4.MC1gtkWdJXIO2330784947265962f972258f43bd01d0.jpg"
        urllib.request.urlretrieve(url,"chun.jpg")



### url编码和解码函数 ###

- urllib.parse.encode({})  #参数封装,并且实现url转码
- urllib.parse.quote("")  #url进行编码  中文->%xxx
- urllib.parse.unquote("")  #url进行解码  %xxx->中文

   - http://不进行编码，使用拼凑str

            import  urllib.parse
            
            url="https://www.baidu.com/s?wd=周杰伦"
            
            print(urllib.parse.quote(url))




 ###url参数的封装###

F1:

    通过字典字段逐个解析

        string=[]
        data={
            'name':'lutong'，
              "age":18
        }
        
        for  k,v in data.items:
            string.append(k+'='+str(v))
        
        quert_string='&'.join(string)
        url=url+'？'+query_string


F2:
    使用urlencode()
    并且可以自动url编码
    

        data={
            'name':'lutong'，
              "age":18
        }
        query_string=urllib.parse.urlencode(data)
        url=url+'？'+query_string
    


### 项目例子:百度查找（编码问题还未解决） ###
        import  urllib.parse
        import  urllib.request
        
        #word=input("请输入你要搜索的东西：")
        word="中国"
        
        #拼接
        url="http://www.baidu.com/s?"
        data={
            "ie":"utf-8",
            "wd":word
        }
        url=url+urllib.parse.urlencode(data)
        
        
        
        response=urllib.request.urlopen(url)
        
        
        filename=word+".html"
        with open(filename,"wb") as  file:
            file.write(response.read())



### 关于二进制和字符串的转化 ###

二进制--->>字符串

- decode()
    - 不写参数，默认utf8
    - 一般写参数使用gbk

字符串--->>二进制
- encode()
    - 不写参数，默认utf8
    - 一般写参数使用gbk    - 


//信息不全



## 构建头部信息 ##


### 实现流程 ###

1. 设置url
2. 设置请求头
3. 获取请求对象
4. 发送请求，获取返回对象


        import  urllib.parse
        import  urllib.request
        
        
        #url
        url="https://www.baidu.com/"
        #headers
        headers={
        'User-Agent':' Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36'
        }
        #reuqest
        request=urllib.request.Request(url=url,headers=headers)
        
        #responce
        response=urllib.request.urlopen(request)
        
        print(response.read().decode("utf-8"))




























- 