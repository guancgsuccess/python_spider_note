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



## 案例分析 - 获取豆瓣网的json数据

**再次提醒,只要获取数据,一定要注意分页的问题.**

https://movie.douban.com/tv/#!type=tv&tag=%E7%83%AD%E9%97%A8&sort=recommend&page_limit=20&page_start=0



![alt text](imgs/douban.png)



~~~python
from urllib import request,parse,error
import random

# querystring: ?key=value&key2=value2
url='https://movie.douban.com/j/search_subjects?type=tv&tag=%E7%83%AD%E9%97%A8&sort=recommend&page_limit=20&'

qs = {
    'page_start':60
}

url = url + parse.urlencode(qs)

req=request.Request(url)

ua_list = [
    "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.71 Safari/537.36",
    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11",
    "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.133 Safari/534.16",
    "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2",
    "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:34.0) Gecko/20100101 Firefox/34.0",
    "Mozilla/5.0 (X11; U; Linux x86_64; zh-CN; rv:1.9.2.10) Gecko/20100922 Ubuntu/10.10 (maverick) Firefox/3.6.10"
]

# 随机读取列表
user_agent=random.choice(ua_list)
req.add_header('User-Agent',user_agent)

try:
    with request.urlopen(req) as f:
        data= f.read()
        print(data)
        with open('dtv.json','w',encoding='utf-8') as fp:
            fp.write(data.decode())
except error.HTTPError as err:
    pass
except error.URLError as err:
    pass
except Exception as err:
    pass
~~~





