# Requests

Requests 继承了urllib的所有特性。Requests支持HTTP连接保持和连接池，支持使用cookie保持会话，支持文

上传，支持自动确定响应内容的编码，支持国际化的 URL 和 POST 数据自动编码。

requests 的底层实现其实就是 urllib3

Requests的文档非常完备，中文文档也相当不错。Requests能完全满足当前网络的需求

[开源地址](https://github.com/kennethreitz/requests)

[中文文档 API](http://docs.python-requests.org/zh_CN/latest/index.html)



## 安装

因为是第三方库,因此需要进行提前安装

~~~python
pip install requests
~~~

安装完成之后,直接使用

~~~python
import requests
~~~

若没有报错,则表示安装成功



## 基本用法 - get请求

~~~python
import requests
import random

url = 'https://www.liepin.com/suzhou/zhaopin/?dqs=060080&' \
      'salary=&isAnalysis=true&init=1&searchType=1&fromSearchBtn=1&' \
      'jobTitles=&industries=&industryType=&d_headId=cf381555d0d32f6f84e096ca2d76c59c&' \
      'd_ckId=cf381555d0d32f6f84e096ca2d76c59c&d_sfrom=search_city&d_pageSize=40&siTag=&'

#拼接查询参数
qs={
    "key":"python",
    "d_curPage":0
}
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

headers={
    "user-agent":user_agent
}

with requests.get(url=url,params=qs,headers=headers) as f:
     #查看响应内容，f.text 返回的是Unicode格式的数据
     data=f.text
     with open('python.html','w+',encoding='utf-8') as fp:
         fp.write(data)
         fp.close()

    #print(f.headers['date'])
~~~



## 基本用法 - post请求

response = requests.post(url, data = data)
​	
如果是json文件可以直接显示
​	
print(response.json())



>  如果 JSON 解码失败， r.json() 就会抛出一个异常。例如，响应内容是 401 (Unauthorized)，尝试访问 r.json() 将会抛出 ValueError: No JSON object could be decoded 异常。
>
> 需要注意的是，成功调用 r.json() 并**不**意味着响应的成功。有的服务器会在失败的响应中包含一个 JSON 对象（比如 HTTP 500 的错误细节）。这种 JSON 会被解码返回。要检查请求是否成功，请使用 r.raise_for_status() 或者检查 r.status_code 是否和你的期望相同。​	



### 案例 -爬取豆瓣网的数据并且写入到本地json文件中

~~~python
from urllib import error
import requests
import random
import json
url='https://movie.douban.com/j/search_subjects?type=tv&tag=%E7%83%AD%E9%97%A8&sort=recommend&page_limit=20&'

qs = {
    'page_start':60
}

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

try:
    with requests.post(url,qs,user_agent) as f:
        data= f.json()
        print(data)
        with open('dtv.json','w+',encoding='utf-8') as fp:
            #fp.write(str(data))
            json.dump(data,fp,ensure_ascii=False) #区别是文件中的json必须是双引号才能够被解析
            #fp.close()
except error.HTTPError as err:
    print(err)
except error.URLError as err:
    print(err)
except Exception as err:
    print(err)

if __name__ == '__main__':
    with open('dtv.json','r+',encoding='utf-8') as fp:
        douban=json.load(fp)
        print(douban['subjects'])
~~~



### 案例 - 51job爬取python岗位需求信息

~~~python
import requests
import random

url='https://search.51job.com/list/070300,000000,0000,00,9,99,python,2,1.html?lang=c&stype=&postchannel=0000&workyear=99&cotype=99&degreefrom=99&jobterm=99&companysize=99&providesalary=99&lonlat=0%2C0&radius=-1&ord_field=0&confirmdate=9&fromType=&dibiaoid=0&address=&line=&specialarea=00&from=&welfare='

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

headers={
    "user-agent":user_agent
}

with requests.get(url, headers=headers) as f:
    f.encoding='gbk'
    data = f.text
    with open('51job.html', 'w+', encoding='utf-8') as fp:
        print(data)
        fp.write(data)
        fp.close()
~~~

## 代理

如果需要使用代理，你可以通过为任意请求方法提供 proxies 参数来配置单个请求
​	
```python
import requests

# 根据协议类型，选择不同的代理
proxies = {
	"http": "http://12.34.56.79:9527",
	"https": "http://12.34.56.79:9527",
}

response = requests.get("http://www.baidu.com", proxies = proxies)
print response.text
```
## 处理HTTPS请求 SSL证书验证

要想检查某个主机的SSL证书，你可以使用 verify 参数（也可以不写）

如果SSL证书验证不通过，或者不信任服务器的安全证书，则会报出SSLError，据说 12306 证书是自己做的.

如果我们想跳过 12306 的证书验证，把 verify 设置为 False 就可以正常请求了

``` python
import requests
response = requests.get("https://www.baidu.com/", verify=True)

# 也可以省略不写
# response = requests.get("https://www.baidu.com/")
print(r.text)
```
