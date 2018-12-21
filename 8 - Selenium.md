# Selenium	

selenium 是一个用于Web应用程序测试的工具。Selenium测试直接运行在浏览器中，就像真正的用户在操作一样。支持的浏览器包括IE（7, 8, 9, 10, 11），Mozilla Firefox，Safari，Google Chrome，Opera等。这个工具的主要功能包括：测试与浏览器的兼容性——测试你的应用程序看是否能够很好得工作在不同浏览器和操作系统之上。测试系统功能——创建回归测试检验软件功能和用户需求。支持自动录制动作和自动生成 .Net、Java、Perl等不同语言的测试脚本。 

**selenium用于爬虫，主要是用来解决javascript渲染的问题** 

1. Selenium 可以根据我们的指令，让浏览器自动加载页面，获取需要的数据，甚至页面截屏，或者判断网站上

   某些动作是否发生。

2. Selenium 自己不带浏览器，不支持浏览器的功能，它需要与第三方浏览器结合在一起才能使用。但是我们有

   时候需要让它内嵌在代码中运行，所以我们可以用一个叫 PhantomJS 的工具代替真实的浏览器。


#### 安装

~~~python
pip install selenium
~~~

[官方文档](https://selenium-python.readthedocs.io/index.html)



#### headless brower

无头模式是运行浏览器的一种非常有用的方式。就像听起来一样，浏览器正常运行，减去任何可见的UI组件。虽

然对网上冲浪不是那么有用，但它通过自动化测试自成一体。

chrome无头浏览器：

https://developers.google.com/web/updates/2017/04/headless-chrome](https://developers.google.com/web/updates/2017/04/headless-chrome)



#### 无头Chrome入门

无头Chrome 在Chrome 59中发布。这是在无头环境中运行Chrome浏览器的一种方式。

基本上，运行Chrome没有铬！它将Chromium和Blink渲染引擎提供的所有现代Web平台功能引入命令行。

为什么这有用？

无头浏览器是自动测试和服务器环境的绝佳工具，您不需要可见的UI shell。例如，您可能希望针对真实网页运行

某些测试，创建PDF，或者仅检查浏览器如何呈现URL。



#### 安装webdriver（chromedriver）









