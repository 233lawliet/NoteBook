
## post请求 ##


### 查找ajax请求url ###
    chrome->network
    过滤：xhr       //xhr是ajax请求



### 请求流程 ###

1. 设置url
2. 设置headers
3. 设置from_data.并且对表单进行处理
4. 编写request对象
5. 发送请求，获取response对象


        import urllib.request
        import urllib.parse
        
        
        #url
        postUrl="https://fanyi.baidu.com/v2transapi"
        
        #header
        headers={
        'Accept': '*/*',
        #'Accept-Encoding': 'gzip, deflate, br',
        'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8,ja;q=0.7',
        'Connection': 'keep-alive',
        'Content-Length': '102',
        'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
        'Cookie': 'BAIDUID=F15AF162D7755629C1E3A379D7489740:FG=1; BIDUPSID=F15AF162D7755629C1E3A379D7489740; PSTM=1546511596; REALTIME_TRANS_SWITCH=1; FANYI_WORD_SWITCH=1; HISTORY_SWITCH=1; SOUND_SPD_SWITCH=1; SOUND_PREFER_SWITCH=1; BDUSS=FDbGFRYm14M2cwNU9JRWplcmtyV0dSWERvU2s3WUpJSEJTam1oU35VWHlnN1JjQVFBQUFBJCQAAAAAAAAAAAEAAABaW5OEsNfEq2lpaQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAPL2jFzy9oxcM1; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; delPer=0; PSINO=1; H_PS_PSSID=1450_28938_21097_28775_28720_28964_28835_28584_26350_28604_20718; locale=zh; Hm_lvt_64ecd82404c51e03dc91cb9e8c025574=1554218700,1556790357; from_lang_often=%5B%7B%22value%22%3A%22it%22%2C%22text%22%3A%22%u610F%u5927%u5229%u8BED%22%7D%2C%7B%22value%22%3A%22zh%22%2C%22text%22%3A%22%u4E2D%u6587%22%7D%2C%7B%22value%22%3A%22en%22%2C%22text%22%3A%22%u82F1%u8BED%22%7D%5D; to_lang_often=%5B%7B%22value%22%3A%22en%22%2C%22text%22%3A%22%u82F1%u8BED%22%7D%2C%7B%22value%22%3A%22zh%22%2C%22text%22%3A%22%u4E2D%u6587%22%7D%5D; Hm_lpvt_64ecd82404c51e03dc91cb9e8c025574=1556800050',
        'Host': 'fanyi.baidu.com',
        'Origin': 'https://fanyi.baidu.com',
        'Referer': 'https://fanyi.baidu.com/',
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36'
        }

        
        #data
        word='time'
        fromData={
        'from': 'en',
        'to': 'zh',
        'query': word,
        'simple_means_flag': 3,
        'sign': 328897.124912,
        'token': 'c16c175ea99f80f6b13f9af5875924f0',
        }
        
        fromData=urllib.parse.urlencode(fromData).encode()
        
        
        #request
        reuqest=urllib.request.Request(url=postUrl,data=fromData,headers=headers)
        
        #respomce
        response=urllib.request.urlopen(reuqest)
        
        print(response.read().decode())
        
   

**注：发送的数据需要unicode化。fromData=urllib.parse.urlencode(fromData).encode()**     
      
**注：服务器返回的数据是gzip压缩的，解码麻烦，可以使用，去掉头部的gzip。不对返回的进行gzip压缩**  



## ajax的get和post ##

### get ###
    通过url拼凑data
    //不举例子了
###post###
   
    import urllib.request
    import urllib.parse
    
    
    #url
    postUrl="http://www.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=keyword"
    
    headers={
    "User-Agent":'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36',
    
    }
    
    #data
    address=input("请输入城市：")
    startPage=input("请输入开始的页面：")
    limit=input("请输入显示的个数：")
    fromData={
    'cname':'',
    'pid': '',
    'keyword':address,
    'pageIndex':startPage,
    'pageSize': limit
    }
    
    
    fromData=urllib.parse.urlencode(fromData).encode()    
    
    #request
    reuqest=urllib.request.Request(url=postUrl,data=fromData,headers=headers)
    
    #respomce
    response=urllib.request.urlopen(reuqest)
    
    print(response.read().decode())


### 百度贴吧 ###

    
    import urllib.request
    import urllib.parse
    import os
    
    
    #url
    url="http://tieba.baidu.com/f?kw=python&ie=utf-8"
    
    
    #header
    headers={
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36'
    }
    
    #data
    name=input("请输入你要去的吧名：")
    start_page=int(input("请输入你要去的n页："))
    end_page=int(input("请输入你要结束的n页："))
    
    for page in range(start_page,end_page+1):
        data = {
            'kw': name,
            'ie': 'utf-8',
            'pn': (page-1)*50
        }
        #str类型   encode之后是二进制的unicode
        data=urllib.parse.urlencode(data)
    
        #新的url
        nurl=url+data
    
    
        #request
        request=urllib.request.Request(url=nurl,headers=headers)
    
        #response
        response=urllib.request.urlopen(request)
    
    
        #创建文件夹
        if not os.path.exists(name):
            os.mkdir(name)
        filename=name+"_"+str(page)+".html"
        filepath=name+"/"+filename
    
        #写入数据
        with open(filepath,"wb") as f:
            f.write(response.read())





## URLError\HTTPError ##

### URLError产生原因 ###

- 没有网
- 服务器连接失败
- 找不到服务器



import urllib.request
import urllib.error

    #url
    url="http://wwww.maodan.com"
    
    try:
        response=urllib.request.urlopen(url=url)
        print(response)
    except urllib.error.URLError as e:
        print(e)


HTTPError是URLError的子类
    
    import urllib.request
    import urllib.error
    
    #url
    url="https://blog.csdn.net/qq_36278071/article/details/7966019"
    
    try:
        response=urllib.request.urlopen(url=url)
        print(response)
    
    except urllib.error.HTTPError as e:
        print(e)
    except urllib.error.URLError as e:
        print(e)

