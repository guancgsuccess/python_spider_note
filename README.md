# 前言

简单来说互联网是由一个个站点和网络设备组成的大网，我们通过浏览器访问站点，站点把HTML、JS、CSS代码返回给浏览器，这些代码经过浏览器解析、渲染，将丰富多彩的网页呈现我们眼前.

## 爬虫是什么?

如果我们把互联网比作一张大的蜘蛛网，数据便是存放于蜘蛛网的各个节点，而爬虫就是一只小蜘蛛，

沿着网络抓取自己的猎物（数据）爬虫指的是：向网站发起请求，获取资源后分析并提取有用数据的程序；

从技术层面来说就是 通过程序模拟浏览器请求站点的行为，把站点返回的HTML代码/JSON数据/二进制数据（图

片、视频） 爬到本地，进而提取自己需要的数据，存放起来使用.

![](imgs\spider.png)

## 爬虫流程

用户获取网络数据的方式：

方式1：浏览器提交请求--->下载网页代码--->解析成页面

方式2：模拟浏览器发送请求(获取网页代码)->提取有用的数据->存放于数据库或文件中

**爬虫要做的就是方式2**

![alt text](D:\用户文件夹\Desktop\python_spider_node\imgs\spider_path.png)

**1. 发起请求**

使用http库向目标站点发起请求，即发送一个Request

Request包含：请求头、请求体等 

Request模块缺陷：不能执行JS 和CSS 代码



**2. 获取响应内容**

如果服务器能正常响应，则会得到一个Response

Response包含：html，json，图片，视频等



**3. 解析内容**

解析html数据：正则表达式（RE模块），第三方解析库如Beautifulsoup，pyquery等

解析json数据：json模块

解析二进制数据:以wb的方式写入文件



**4.  保存数据**

数据库（MySQL，Mongdb、Redis）

文件



## http协议 请求与响应



![](imgs\spider_request.png)

* ``Request``：用户将自己的信息通过浏览器（socket client）发送给服务器（socket server）

* ``Response``：服务器接收请求，分析用户发来的请求信息，然后返回数据（返回的数据中可能包含其他链接， 如：图片，js，css等）

**注意：浏览器在接收Response后，会解析其内容来显示给用户，而爬虫程序在模拟浏览器发送请求然后接收Response后，是要提取其中的有用数据。**



### Request

**1. 请求方式：**

常见的请求方式：GET / POST

 

**2. 请求的URL**

url全球统一资源定位符，用来定义互联网上一个唯一的资源 例如：一张图片、一个文件、一段视频都可以用url唯一确定

 

url编码

https://www.baidu.com/s?wd=图片

图片会被编码（看示例代码）

 

网页的加载过程是：

加载一个网页，通常都是先加载document文档，

在解析document文档的时候，遇到链接，则针对超链接发起下载图片的请求

 

**3. 请求头**

User-agent：请求头中如果没有user-agent客户端配置，服务端可能将你当做一个非法用户host；

cookies：cookie用来保存登录信息

注意： 一般做爬虫都会加上请求头

**请求头需要注意的参数：**

（1）Referrer：访问源至哪里来（一些大型网站，会通过Referrer 做防盗链策略；所有爬虫也要注意模拟）

（2）User-Agent:访问的浏览器（要加上否则会被当成爬虫程序）

（3）cookie：请求头注意携带

 

**4. 请求体**

```
请求体
    如果是get方式，请求体没有内容 （get请求的请求体放在 url后面参数中，直接能看到）
    如果是post方式，请求体是format data

    ps：
    1、登录窗口，文件上传等，信息都会被附加到请求体内
    2、登录，输入错误的用户名密码，然后提交，就可以看到post，正确登录后页面通常会跳转，无法捕捉到post
```

### Response

**1. 响应状态码**

　　200：代表成功

　　301：代表跳转

　　404：文件不存在

　　403：无权限访问

　　502：服务器错误

 

**2. respone header**


**响应头需要注意的参数：**

（1）Set-Cookie:BDSVRTM=0; path=/：可能有多个，是来告诉浏览器，把cookie保存下来

（2）Content-Location：服务端响应头中包含Location返回浏览器之后，浏览器就会重新访问另一个页面



**3. preview就是网页源代码**

JSO数据

如网页html，图片

二进制数据等 

# 官网

关于urllib的详细介绍,务必阅读官网介绍:

https://docs.python.org/3.6/library/urllib.request.html#module-urllib.request



# 认识Urllib

Urllib是python内置的HTTP请求库.包括以下模块:

1. urllib.request 请求模块 
2. urllib.error 异常处理模块
3. urllib.parse url解析模块
4. urllib.robotparser robots.txt解析模块 [暂不了解]



# 版本变动

在python2.x版本中可以直接使用``import urllib``来进行操作

但是python3.x版本中使用的是**import urllib.request**来进行操作



 ## urlopen()

urllib.request.urlopen(*url*，*data = None*，[ *timeout*，] **，*cafile = None*，*capath = None*，*cadefault = False*，*context = None* ）

**urlopen一般常用的有三个参数，它的参数如下：**

**urllib.requeset.urlopen(url,data,timeout)**

response.read()可以获取到网页的内容.



### Hello_baidu案例

爬取百度的首页,并且将其写入到本地文件hello_baidu.html文件中



~~~python
from urllib import request
with request.urlopen('http://www.baidu.com') as f:
    # data = f.read()
    # print('Status:', f.status, f.reason)# Status: 200 OK
    # print(data)

    if f.status == 200:
        data = f.read()
        # print(data.decode())
        try:
            with open('hello_baidu.html','w+',encoding="utf-8") as fp:
                fp.write(data.decode())
                fp.close()
        except Exception as ex:
            print(ex)
~~~



## get请求

如果我们要想模拟浏览器发送GET请求，就需要使用Request对象，通过往Request对象添加HTTP头，我们就可以把请求伪装成浏览器。

>浏览器 就是互联网世界上公认被允许的身份，如果我们希望我们的爬虫程序更像一个真实用户，那我们第一步，就是需要伪装成一个被公认的浏览器。用不同的浏览器在发送请求的时候，会有不同的User-Agent头。 urllib2默认的User-Agent头为：Python-urllib/x.y（x和y是Python主版本和次版本号,例如 Python-urllib/3.6）

 [常见浏览器User-Agent大全](https://www.cnblogs.com/1906859953Lucas/p/9027165.html)



### 增加随机user_agent头

~~~python
from urllib import request,parse
import random
url = 'http://www.runoob.com'

##用Request类构建了一个完整的请求，增加了headers等一些信息
req = request.Request(url)

ua_list = [
    "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.71 Safari/537.36",
    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11",
    "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.133 Safari/534.16",
    "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2",
    "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:34.0) Gecko/20100101 Firefox/34.0",
    "Mozilla/5.0 (X11; U; Linux x86_64; zh-CN; rv:1.9.2.10) Gecko/20100922 Ubuntu/10.10 (maverick) Firefox/3.6.10"
    ]

#随机读取列表
user_agent = random.choice(ua_list)
req.add_header("User-Agent",user_agent)
# print(dir(req))
# # 获取请求的完整地址
# print(req.full_url)
# # 获取请求头信息
#print(req.headers['User-agent'])
# print(req.get_header('User-agent'))

with request.urlopen(req) as f:
    data=f.read()

   # print(f.status)
    with open("菜鸟教程.html","w+",encoding="utf-8") as fp:
        fp.write(data.decode())
    #print(data.decode())

~~~



### get请求提交数据

 ~~~python
from urllib import request,parse
import random
url = 'http://www.runoob.com/?'
#查询数据
query_string = {'s':'js'}

#转换成key-value
query_string = parse.urlencode(query_string)
#print(query_string) #s=js
#拼接url
url+=query_string

##用Request类构建了一个完整的请求，增加了headers等一些信息
req = request.Request(url)
#req.add_header('User-Agent', 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.81 Safari/537.36')

ua_list = [
    "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.71 Safari/537.36",
    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11",
    "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.133 Safari/534.16",
    "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2",
    "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:34.0) Gecko/20100101 Firefox/34.0",
    "Mozilla/5.0 (X11; U; Linux x86_64; zh-CN; rv:1.9.2.10) Gecko/20100922 Ubuntu/10.10 (maverick) Firefox/3.6.10"
    ]

#随机读取列表
user_agent = random.choice(ua_list)
req.add_header("User-Agent",user_agent)
# print(dir(req))
# # 获取请求的完整地址
# print(req.full_url)
# # 获取请求头信息
#print(req.headers['User-agent'])
# print(req.get_header('User-agent'))

with request.urlopen(req) as f:
    data=f.read()

   # print(f.status)
    with open("菜鸟教程.html","w+",encoding="utf-8") as fp:
        fp.write(data.decode())
    #print(data.decode())

 ~~~



##   **解决https无法爬取的问题**

~~~python
import ssl
ssl._create_default_https_context = ssl._create_unverified_context
~~~



## 解决url中有中文的问题

~~~python
  	from urllib import parse
  	import string
  	url='包含中文的url'
  	url=parse.quote(url,safe=string.printable)
  	print(url)
~~~

## post请求数据

案例:有道翻译

![alt text](D:\用户文件夹\Desktop\python_spider_node\imgs\youdao.png)



~~~python
from urllib import request,parse
import random

# querystring: ?key=value&key2=value2

url = "http://fanyi.youdao.com/translate_o?smartresult=dict&smartresult=rule"

ua_list = [
    "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.71 Safari/537.36",
    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11",
    "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.133 Safari/534.16",
    "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2",
    "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:34.0) Gecko/20100101 Firefox/34.0",
    "Mozilla/5.0 (X11; U; Linux x86_64; zh-CN; rv:1.9.2.10) Gecko/20100922 Ubuntu/10.10 (maverick) Firefox/3.6.10"
]

formdata={
"type":"AUTO",
"i": "管成功",
"from": "AUTO",
"to": "AUTO",
"smartresult": "dict",
"client": "fanyideskweb",
"salt": "15453587166188",
"sign": "2e70dc0949ac47720a75a3bc8df9f998",
"doctype": "json",
"version": "2.1",
"keyfrom": "fanyi.web",
"action": "FY_BY_REALTIME",
"typoResult": "false"
}


data=parse.urlencode(formdata).encode()

# 随机读取列表
user_agent=random.choice(ua_list)
header={
    "User-Agent":user_agent,
    "cookie":"DICT_UGC=be3af0da19b5c5e6aa4e17bd8d90b28a|; OUTFOX_SEARCH_USER_ID=1211146131@58.211.151.194; JSESSIONID=abcZM1N967mT665MBJoFw; OUTFOX_SEARCH_USER_ID_NCOO=2039613537.2260184; _ntes_nnid=b4a5a472cd85ea60f8eb2bc8358fa89c,1545358624181; ___rl__test__cookies=1545358716615",
    "Host": "fanyi.youdao.com",
    "Origin": "http://fanyi.youdao.com",
    "Referer": "http://fanyi.youdao.com"
}
req=request.Request(url,data,header)
# req.add_header('User-Agent',user_agent)
with request.urlopen(req) as f:
    data=f.read()

    print(f.status)

    print(data.decode())
~~~

## 案例分析 - 批量爬取聚美优品网站数据



**分析点:页面中的数据涉及到分页的问题,因此我们需要提前手动点击下一页,观察url地址发生的变化.**



~~~python
from urllib import request,parse,error
import random
import string

def load_data(page):
    for i in range(1,page+1):
        jumei_spider(i)

# 加载网站数据
def jumei_spider(num):
    # 定义url
    url = 'http://search.jumei.com/?search=手机&bid=4&site=sh&'

    # 定义查询数据
    query_String = {
        "filter": "0-11-{0}".format(num)
    }

    # 拼接url
    url = url + parse.urlencode(query_String)

    # 中文乱码处理
    url = parse.quote(url, safe=string.printable)

    # 用Request类构建了一个完整的请求，增加了headers等一些信息
    req = request.Request(url)
    # 定义user_aent
    ua_list = [
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.71 Safari/537.36",
        "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11",
        "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.133 Safari/534.16",
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2",
        "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:34.0) Gecko/20100101 Firefox/34.0",
        "Mozilla/5.0 (X11; U; Linux x86_64; zh-CN; rv:1.9.2.10) Gecko/20100922 Ubuntu/10.10 (maverick) Firefox/3.6.10"
       ]
    # 随机生成一个
    user_agent = random.choice(ua_list)
    req.add_header('User-Agent', user_agent)

    with request.urlopen(url) as fp:
        if fp.status == 200:
            data = fp.read().decode()
            path = "聚美_{0}.html".format(num)
            saveFiles(path,data)

def saveFiles(path,data):
    with open(path,'w',encoding='utf-8') as f:
        f.write(data)
        f.close()

load_data(3)
~~~
