# handler处理器和自定义opener

opener是 urllib.OpenerDirector 的实例，我们之前一直都在使用的urlopen，它是一个特殊的opener（也就是模块帮我们构建好的）。但是基本的urlopen()方法不支持代理、cookie等其他的HTTP/HTTPS高级功能。所以要支

持这些功能：

* 使用相关的 Handler处理器 来创建特定功能的处理器对象

* 然后通过 urllib.build_opener()方法使用这些处理器对象，创建自定义opener对象；
* 使用自定义的opener对象，调用open()方法发送请求。

**如果程序里所有的请求都使用自定义的opener，可以使用urllib.install_opener() 将自定义的 opener 对象 定**

**义为 全局opener，表示如果之后凡是调用urlopen，都将使用这个opener（根据自己的需求来选择）**



## 定义简单的opener

~~~python
from urllib import request,parse,error
import random
import ssl
ssl._create_default_https_context = ssl._create_unverified_context

#构建一个HTTPHandler处理器对象,支持处理HTTPS请求
#如果在 HTTPHandler()增加 debuglevel=1参数，还会将 Debug Log 打开，这样程序在执行的时候，会把收包#和发包的报头在屏幕上自动打印出来，方便调试，有时可以省去抓包的工作。
http_handler = request.HTTPSHandler(debuglevel=1)
#创建支持处理HTTP请求的opener对象
opener = request.build_opener(http_handler)

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
    with opener.open(req) as f:
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

## ProxyHandler处理器（代理设置）

使用代理IP，这是爬虫/反爬虫的第二大招，通常也是最好用的。

很多网站会检测某一段时间某个IP的访问次数(通过流量统计，系统日志等)，如果访问次数多的不像正常人，它会

禁止这个IP的访问。所以我们可以设置一些代理服务器，每隔一段时间换一个代理，就算IP被禁止，依然可以换个

IP继续爬取。免费的开放代理获取基本没有成本，我们可以在一些代理网站上收集这些免费代理，测试后如果可以

用，就把它收集起来用在爬虫上面。

**免费短期代理网站举例：**

1. [西刺免费代理IP](http://www.xicidaili.com/)

2. [快代理免费代理](http://www.kuaidaili.com/free/inha/)

3. [Proxy360代理](http://www.proxy360.cn/default.aspx)

4. [全网代理IP](http://www.goubanjia.com/free/index.shtml)

~~~python
from urllib import request,parse,error
import random
import ssl
ssl._create_default_https_context = ssl._create_unverified_context

proxy_list = [
    {
        "https":"119.179.60.117:8118"
    },
    {
        "https":"39.81.145.104:20183"
    }
]
# 随机选择一个代理
proxy = random.choice(proxy_list)

## 使用选择的代理构建代理处理器对象
http_handler = request.ProxyHandler(proxy)#默认是使用自己的ip
#创建支持处理HTTP请求的oepner对象
opener = request.build_opener(http_handler)

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
    with opener.open(req) as f:
        data= f.read()
        print(data)
        with open('dtv.json','w',encoding='utf-8') as fp:
            fp.write(data.decode())
except error.HTTPError as err:
    print(err)
except error.URLError as err:
    print(err)
except Exception as err:
    print(err)
~~~









