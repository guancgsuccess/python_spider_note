# spider-页面解析-XML	

#### 什么是XML?

* XML 指可扩展标记语言（EXtensible Markup Language）
* XML 是一种标记语言，很类似 HTML
* XML 的设计宗旨是传输数据，而非显示数据
* XML 的标签需要我们自行定义
* XML 被设计为具有自我描述性
* XML 是 W3C 的推荐标准

###### 示例文档

~~~xml
<?xml version="1.0" encoding="utf-8"?>
<bookstore> 
    <book category="cooking"> 
        <title lang="en">Everyday Italian</title>  
        <author>Giada De Laurentiis</author>  
        <year>2005</year>  
        <price>30.00</price> 
    </book>  

    <book category="children"> 
        <title lang="en">Harry Potter</title>  
        <author>J K. Rowling</author>  
        <year>2005</year>  
        <price>29.99</price> 
    </book>  

    <book category="web"> 
        <title lang="en">XQuery Kick Start</title>  
        <author>James McGovern</author>  
        <author>Per Bothner</author>  
        <author>Kurt Cagle</author>  
        <author>James Linn</author>  
        <author>Vaidyanathan Nagarajan</author>  
        <year>2003</year>  
        <price>49.99</price> 
    </book> 

    <book category="web" cover="paperback"> 
        <title lang="en">Learning XML</title>  
        <author>Erik T. Ray</author>  
        <year>2003</year>  
        <price>39.95</price> 
    </book> 
</bookstore>
~~~



#### 什么是XPath？

XPath (XML Path Language) 是一门在 XML 文档中查找信息的语言，可用来在 XML 文档中对元素和属性进行遍历。

XPath 开发工具

* 开源的XPath表达式编辑工具:XMLQuire(XML格式文件可用)
* Chrome插件 XPath Helper
* Firefox插件 XPath Checker



#### 安装lxml

~~~python
pip install lxml
~~~



**常用方法如下:**

* etree.parse()

  读取xml文件，结果为**xml对象**（不是字符串）

* etree.HTML(string_html)

  将字符串形势的html文件转换为xml对象

* etree.tostring(htmlelement, encoding="utf-8").decode("utf-8")

  按字符串序列化HTML文档


#### 选择器

|表达式|	描述|
|---|---|---|
|nodename|	选取此节点的所有子节点。|
|/	|从根节点选取。|
|//	|从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。|
|.	|选取当前节点。|
|..	|选取当前节点的父节点。|
|@	|选取属性。|



##### 示例

* **bookstore**  选取 bookstore 元素的所有子节点(如果只有一个的话)。

* **/bookstore** 选取根元素 bookstore。注释：假如路径起始于正斜杠( / )，则此路径始终代表到某元素的绝对路 径！

* **bookstore/book** 选取属于 bookstore 的子元素的所有 book 元素

* **//book** 选取所有 book 子元素，而不管它们在文档中的位置。

* **bookstore//book** 选择属于 bookstore 元素的后代的所有 book 元素，而不管它们位于 bookstore 之下的什么位置。

* **//@lang** 选取名为 lang 的所有属性。

* **/bookstore/book[1]** 选取属于 bookstore 子元素的第一个 book 元素。

* **/bookstore/book[last()]** 选取属于 bookstore 子元素的最后一个 book 元素。

* **/bookstore/book[last()-1]** 选取属于 bookstore 子元素的倒数第二个 book 元素。

* **/bookstore/book[position()<3]** 选取最前面的两个属于 bookstore 元素的子元素的 book 元素。

* **//title[@lang]** 选取所有拥有名为 lang 的属性的 title 元素。

* **//title[@lang='eng']** 选取所有 title 元素，且这些元素拥有值为 eng 的 lang 属性。

* **/bookstore/book[price>35.00]** 选取 bookstore 元素的所有 book 元素，且其中的 price 元素的值须大于 35.00。

* **/bookstore/book[price>35.00]/title** 选取 bookstore 元素中的 book 元素的所有 title 元素，且其中的 price 元素的值须大于 35.00。


## 案例解析 - 利用xpath解析xml文件

~~~python
from lxml import etree

# 读取xml文件,结果为xml对象
xml = etree.parse('data.xml')
print(xml) # <lxml.etree._ElementTree object at 0x031017D8>

#titles = xml.xpath('/bookstore/book/title')
#titles = xml.xpath('//book/title')
titles = xml.xpath('//title')
print(len(titles))

for tt in titles:
    print(tt.text)
print('*'*20)

# 选择所有title节点的lang属性值
attrs = xml.xpath('/bookstore//title/@lang')

for a in attrs:
    print(a)

print('*'*20)

#选title属性lang = en的内部值
titles = xml.xpath('/bookstore//title[@lang="en"]')
for t in titles:
    print(t.text)

print('*'*20)

books = []
titles = xml.xpath('/bookstore/book/title')
prices = xml.xpath('/bookstore/book/price')
author = xml.xpath('/bookstore/book/author')

for i in range(len(titles)):
    book = {"title":titles[i].text,"price":prices[i].text,"author":author[i].text}
    books.append(book)

print(books)
~~~



##  lxml读取html文件

自己创建html解析器，增加parser参数

~~~python
import lxml.html
etree = lxml.html.etree

parse = etree.HTMLParser(encoding='utf-8')
html = etree.parse('51job.html',parse)
result = etree.tostring(html,encoding='utf-8').decode('utf-8')
#print(html) # <lxml.etree._ElementTree object at 0x0287B0D0>
print(result)

# 找出岗位名称
jobinfos = []
jobs = html.xpath('//p[@class="t1 "]/span/a')
print(len(jobs))
for i in range(len(jobs)):

    job = {"job":jobs[i].text.strip()}
    jobinfos.append(job)

print(jobinfos)
~~~



### requests+xpath爬取图片



~~~python
import requests
import random
import lxml.html

# 解析网站数据
def xpath_data():
    etree = lxml.html.etree
    parse = etree.HTMLParser(encoding='utf-8')
    html = etree.parse('hunsha.html', parse)
    #result = etree.tostring(html, encoding='utf-8').decode('utf-8')
    #print(result)

    imgs = html.xpath('//div[@class="pli"]//img/@src')

    imgs_paths = []
    for img in imgs:
        img_path = 'http://www.studioartiz.com'+img

        #获取图片名称
        img_name = str(img).split("/")[-1]
        #print(img_name)

        #imgs_paths.append(img_path)
        with requests.get(img_path) as response:
            data = response.content

            with open(img_name,'wb') as f:
                f.write(data)
                f.close()

    #print(imgs_paths)


# 获取网站数据
def my_spider():
    url = 'http://www.studioartiz.com/prise/'

    ua_list = [
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.71 Safari/537.36",
        "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11",
        "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.133 Safari/534.16",
        "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2",
        "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:34.0) Gecko/20100101 Firefox/34.0",
        "Mozilla/5.0 (X11; U; Linux x86_64; zh-CN; rv:1.9.2.10) Gecko/20100922 Ubuntu/10.10 (maverick) Firefox/3.6.10"
    ]

    user_agent = random.choice(ua_list)

    with requests.get(url,user_agent) as response:
        response.encoding='utf-8'
        data = response.text
        with open('hunsha.html','w',encoding='utf-8') as f:
            f.write(data)
            f.close()

if __name__ == '__main__':
    # my_spider()
    xpath_data()
~~~





  