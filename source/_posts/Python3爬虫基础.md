---
title: Python3爬虫基础
author: 圣奇宝枣
description: Python3的爬虫教程，完全可以满足个人基本需求，有一定理解难度，内含爬虫所用各个模块教程与信息提取教程
sticky: 0
date: 2022-04-28
updated: 2022-05-09
readmore: true
tags:
  - Python3
  - 基础语法
  - 网络爬虫
categories:
  - 编程语言入门
---

---

#### **爬虫简单入门**

---

##### **爬虫合法性-君子协议**

- 关于爬虫的合法性，有**君子协议**  
  在网站**网址**后加上`/robots.txt`查看君子协议

##### **准备注意事项**

- 做爬虫前尽量不要使用任何**网络代理**，否则容易出现莫名的问题

<!-- more -->

##### **手刃一个小爬虫(request 模块实现)**

- **简单试做**：将百度搜索源码爬取：

  ```python
  #百度
  #需求：用程序模拟浏览器，输入一个网址，从该网址中获取到资源或者内容
  from urllib.request import urlopen      #从包中导入模块

  url="http://www.baidu.com"          #准备网址
  resp = urlopen(url)            #用urlopen模拟浏览器打开网址，将返回的响应存入resp

  """
  先print(resp.read())查看返回的内容
  从中找到编码格式,一般为charset后位置
  再进行解码

  print(resp.read().decode("utf-8"))         #resp.read()从响应中读取内容,并用decode解码
  """

  with open("D:\desktop\代码\python测试\Mywebsite.html",mode="w",encoding="utf-8") as web:            #打开名为"Mywebsite.html"的文件,模式为w写入，as语句将其简称为web,设置encoding打开编码
      web.write(resp.read().decode("utf-8"))              #resp.read()从响应中读取内容,并用decode解码,将其写入到上述文件
  ```

---

#### **Web 请求、HTTP 协议、抓包**

---

##### **Web 请求过程解析**

- 1.**服务器渲染**：在服务器直接把**数据**和 **html** 整合在一起，**统一返回**给浏览器。  
  举例：输入**www.baidu.com**，浏览器向百度服务器发送请求，百度返回 html 页面源代码；在百度里搜索关键词，百度在服务器将关键词有关数据写入 html 页面源代码中，一并返回给浏览器
- 2.**客户端渲染**：**第一次请求**只要一个 **html 骨架**，**第二次请求**拿到**数据**，进行**数据展示**。在页面**源代码**中，**看不到数据**。  
  举例：例如豆瓣电影排行榜的分类筛选网页，浏览器先向服务器请求，服务器返回 html 骨架(不包含数据)，浏览器第二次请求，服务器返回数据，浏览器将 html 骨架与数据渲染结合，呈现页面。在源代码处搜索呈现的数据，无法找到。
- 熟练使用**浏览器抓包工具**：
  Chrome 浏览器右键检查或者 F12，上方大类选择 Network；
  刷新页面，此时所有返回的请求都在此处显示。点击文件可以打开源代码，通常第一个文件为网页骨架；
  Headers 中 Request URL 写有 url 地址，Preview 可以查看预览效果。在这些文件中通过预览找到和页面内容匹配的数据，回到 Headers 即可找到数据 url
- 想要得到**数据**，**无需骨架**，对于爬虫而言，目的为得到数据，骨架无影响

##### **HTTP 协议**

- **HTTP 协议基本概念**
  - **协议**：两台**计算机之间**为了能流畅的进行沟通而设置的一个**君子协定**，常见的协议有 `TCP/IP`，`SOAP` 协议，`HTTP` 协议，`SMTP` 协议等
  - **HTTP 协议**：Hyper Text Transfer Protocol（**超文本传输协议**）的缩写，是用于从**万维网**（WWW:World Wide Web）服务器传输超文本到本地浏览器的传输协议。直白点儿，浏览器和服务器之间的数据交互遵守的就是 HTTP 协议
  - HTTP 协议把一条消息分为**三大块内容**，无论是请求还是响应都是三块内容
    - **请求**：
      > 1、**请求行** → 请求方式(get/post)，请求 url 地址，协议  
      > 2、**请求头** → 放一些服务器要使用的附加信息  
      > 3、**请求体** → 一般放一些请求参数
    - **响应**：
      > 1、**状态行** → 协议，状态码  
      > 2、**响应头** → 放一些客户端要使用的附加信息  
      > 3、**响应体** → 服务器返回的真正客户端要用的内容(HTML,json 等)
- **抓包工具及获得的重要信息**：

  - Network-Headers-General：一般信息
    > **Request URL**：URL 地址  
    > **Request Method**：请求方式  
    > **Status Code**：状态码
  - Network-Headers-**Response Headers**：**响应头**
    > **cookie**：本地字符串数据信息(用户登录信息，反爬的 token)  
    > **其他**：各种神奇的莫名其妙的字符串(这个需要经验，一般都是 token 字样，防止各种攻击和反爬)
  - Network-Headers-**Request Headers**：**请求头**
    > **User-Agent**：请求载体的身份标识(用啥发送的请求，如浏览器信息)  
    > **Referer**：防盗链(这次请求是从哪个页面来的，反爬需要)  
    > **cookie**：本地字符串数据信息(用户登录信息，反爬的 token)

- 附：**请求方式**：

  - Get：显示提交(常用于搜索，通常只读)
  - Post：隐式提交(常用于对数据增删改，通常可写入)

---

#### **requests 模块入门**

---

##### **模块安装**

- `requests` 模块为第三方支持库，需要**手动安装**

  ```bash
  pip install requests
  ```

##### **Requests 入门-1**

- **GET 请求**：将搜狗搜索内容爬取，并学习简单的反爬

  ```python
  import requests

  url = "https://www.sogou.com/web?query=周杰伦"          #保存网址字符串给变量，中文可能转码错误，手动打上去
  #第10行处被拦截，可以将更多请求头信息补入，定义一个字典headers，将User-Agent写入字典，User-Agent通过抓包网页骨架中的Request Headers(请求头)找到，注意直接复制后Mozilla前会多一个空格，记得删除
  dict = {"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.80 Safari/537.36"}

  #用get请求方式请求url，所有地址栏中的url都是get方式请求，将响应存入resp。第四行信息补充完成后，将字典写入headers参数，处理简单的反爬
  resp = requests.get(url,headers=dict)

  #print(resp)         #打印resp，返回网页状态码，返回200正常
  print(resp.text)        #打印页面源代码，但爬虫被拦截了,前往第四行补充信息

  resp.close()            #关闭请求
  ```

- 可以进行一些小修改，做到更改搜索对象：

  ```python
  import requests

  #手动输入搜索的内容
  query=input("输入你要搜索的内容：")
  #利用f-string，做到搜索内容更改
  url = f"https://www.sogou.com/web?query={query}"          #保存网址字符串给变量，中文可能转码错误，手动打上去
  #第10行处被拦截，可以将更多请求头信息补入，定义一个字典headers，将User-Agent写入字典，User-Agent通过抓包网页骨架中的Request Headers(请求头)找到，注意直接复制后Mozilla前会多一个空格，记得删除
  dict = {"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.80 Safari/537.36"}

  #用get请求方式请求url，所有地址栏中的url都是get方式请求，将响应存入resp。第四行信息补充完成后，将字典写入headers参数，处理简单的反爬
  resp = requests.get(url,headers=dict)

  #print(resp)         #打印resp，返回网页状态码，返回200正常
  print(resp.text)        #打印页面源代码，但爬虫被拦截了,前往第四行补充信息

  resp.close()            #关闭请求
  ```

##### **Requests 入门-2**

- **POST 请求**：爬取百度翻译的结果

  ```python
  """
  打开百度翻译后按F12进入抓包工具，清除多余的文件，注意输入法切换为英文，输入英文单词后，翻译框下方有一个小列表
  在抓包工具中通过preview预览尝试寻找列表的数据文件，发现sug文件为数据文件
  打开sug文件的Headers，获取需要的信息：url地址，请求方式为POST
  打开Payload，找对From Data，为POST传参数据，对于上个GET程序中利用f-string传入参数的方式就不灵了
  """
  import requests

  url = "https://fanyi.baidu.com/sug"         #准备url，注意url为数据的url，即sug文件Headers的url

  word = input("请输入你要翻译的英文:")        #准备翻译的单词
  dat = {"kw":word}               #由于POST传参数据来源为From Data，所以按照From Data中的格式，将搜索数据改写入字典，此时可以通过变量更改数据

  resp = requests.post(url,data=dat)             #由于网页访问方式为POST，故使用POST访问，将dat传入data参数，即传入From Data。将响应存入resp
  #print(resp.text)               #输出发现文件有乱码，可以另外直接输出json文件
  print(resp.json())                #将服务器返回的内容直接处理成json()，按照python字典方式输出

  resp.close()            #关闭请求

  #总结，对于POST请求，发送的数据必须放在字典中，通过data参数进行传递
  ```

##### **Requests 入门-3**

- **浏览器渲染的二次 GET 请求网页**：  
  [豆瓣电影分类排行榜-喜剧](https://movie.douban.com/typerank?type_name=剧情&type=11&interval_id=100:90&action=)

- 通常网站 url 里有问号"?"，问号前的是 url，问号后的是参数

  ```python
  """
  豆瓣电影分类排行榜网页通过浏览器渲染，有两次数据传递
  在抓包工具中选择筛选XHR类别(常表示二次请求数据)，找到跟页面差不多的蕴含量大一些的XHR文件，就是页面的数据文件找到数据文件Headers：
  查看url，通常网站url里有问号"?"，问号前的是url，问号后的是参数，查看请求方式为GET方式
  在Payload中有Query String Parameters(url问号后参数)，
  """
  import requests

  url = "https://movie.douban.com/j/chart/top_list"        #参数过长，可以重新封装url参数，url问号后参数部分可以删除

  #重新封装参数。将抓包Query String Parameters的参数复制进字典，分别打双引号，加逗号
  param = {
      "type": "24",
      "interval_id": "100:90",
      "action":"" ,
      "start": "0",
      "limit": "20"
  }

  header = {"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.80 Safari/537.36"}        #布置user-agent

  resp = requests.get(url,params=param,headers=header)              #将param的参数传入params(截止params)。将返回的响应存入resp
  #print(resp.request.url)         #输出按照参数重组后的url地址
  #print(resp.text)                #code 0，什么都没有，说明被反爬了
  print(resp.json())

  resp.close()            #关闭请求

  #反爬处理

  #首先尝试修改user-agent
  #print(resp.request.headers)         #(补充)查看默认信息，user-agent
  #获取浏览器抓包user-agent，准备(第20行)，写入requests.get的参数
  #成功拿到数据，但有乱码，将24行优化为25行，获取json文件
  ```

- 在豆瓣中下拉，刷新出新的电影，同时 Query String Parameters 中出现新的数据，与原数据对比发现只有 Query String Parameters 的 start 参数变化，可以借此修改代码中 start 参数实现新效果

---

#### **数据解析**

---

##### **数据解析概述**

- 爬取到的网站内容和数据被夹在了 `html` 内，想要提取需要的数据，这便涉及到了数据提取  
  本课程提供三种解析方式：
  > **Re 解析**(理论运行速度最快)  
  > **Bs4 解析**(代码简单，但效率较低)  
  > **Xpath 解析**(目前较流行，中规中矩)
- 三种方式可以混合进行使用，完全以结果做导向，只要能拿到想要的数据，用什么方法不重要，当掌握了这些之后再考虑性能的问题

##### **Re 解析\_正则表达式**

- Re 解析：Regular Expression 的简写，正则表达式，一种**使用表达式**的方式对**字符串**进行**匹配**的语法规则
- 我们抓取到的网页源代码本质上就是一个超长的字符串。，想从里面提取内容，用正则表达式再合适不过了
- 优点：**速度快**，**效率高**，**准确性高**
  缺点：新手上手难度较大
- 不过只要掌握了正则编写的的逻辑关系，写出一个提取页面内容的正则并不复杂
- 正则的语法：使用元字符进行排列组合用来匹配字符串
  在线测试正则表达式[https://tool.oschina.net/regex/](https://tool.oschina.net/regex/)
- 元字符：具有**固定含义**的特殊符号
- **常用元字符**

  | 元字符 | 说明                             |
  | :----: | -------------------------------- |
  |   .    | 匹配除换行符以外的所有字符       |
  |   \w   | 匹配字母数字下划线               |
  |   \s   | 匹配任意的空白符                 |
  |   \d   | 匹配数字                         |
  |   \n   | 匹配一个换行符                   |
  |   \t   | 匹配一个制表符                   |
  |   ^    | 匹配字符串的开始                 |
  |   $    | 匹配字符串的结尾                 |
  |   \W   | 匹配非字母数字下划线             |
  |   \D   | 匹配非数字                       |
  |   \S   | 匹配非空白符                     |
  |  a\|b  | 匹配字符 a 或字符 b              |
  |   ()   | 匹配括号内的表达式，也表示一个组 |
  | [...]  | 匹配字符组中的字符               |
  | [^...] | 匹配除了字符组中的字符的所有字符 |

- **量词**：控制前面的元字符出现的次数

  | 量词  | 说明              |
  | :---: | ----------------- |
  |  \*   | 重复零次或更多次  |
  |   +   | 重复一次或更多次  |
  |   ?   | 重复零次或一次    |
  |  {n}  | 重复 n 次         |
  | {n,}  | 重复 n 次或更多次 |
  | {n,m} | 重复 n 到 m 次    |

- **贪婪匹配**和**惰性匹配**

  | 符号 |   类型   |
  | :--: | :------: |
  | .\*  | 贪婪匹配 |
  | .\*? | 惰性匹配 |

  - 这两个着重说一下，写爬虫**用的最多**的就是**惰性匹配**
  - `*？`表示尽可能少的让`*`匹配东西

##### **Bs4 解析\_HTML 语法**

- Bs4 解析：Beautiful Soup4 的简写，简单易用的 **HTML 解析器**，需要掌握一些 HTML 语法
- HTML(Hyper Text Markup Language)超文本标记语言，是编写网页**最基本**、**最核心**的语言，其语法就是用**不同的标签**，对网页上的内容进行**标记**，从而使网页显示不同的效果，简单举例：

  ```html
  <h1>I Love You</h1>
  ```

- **常用标签**：

  | 标签 |          说明          |
  | :--: | :--------------------: |
  |  h1  |        一级标题        |
  |  h2  |        二级标题        |
  |  p   |          段落          |
  | font | 字体(被废弃了，但能用) |
  | body |          主体          |

- **属性**：标签内后跟的**控制标签行为**的**属性**，其后所写的为**属性值**，简单举例：  
  (借此实现标题文字**右对齐**，其中，`align`为属性，`right`为属性值)

  ```html
  <h1 align="right">I Love You</h1>
  ```

- 由此，HTML**基本语法格式**为：

  ```html
  <标签 属性="值" 属性="值">被标记的内容</标签>
  ```

##### **Xpath 解析\_XML 概念**

- Xpath 解析：XML 解析器，用来**提取**XML 文档中的**节点**，Xpath 是在 XML 文档中**搜索**的一门语言。HTML 是 XML 的一个**子集**

- **基础概念**：

  ```html
  <book>
    <id>1</id>
    <name>野花遍地香</name>
    <price>1.23<price>
    <author>'
      <nick>周大强</nick>
      <nick>周芷若</nick>
    </author>
  </book>
  ```

- 在上述 html 中：

  > 1、`book`,`id`,`name`,`price`等都被称为**节点**  
  > 2、`id`,`name`,`price`,`author`被称为`book`的**子节点**，`book`被称为他们的**父节点**  
  > 3、`id`,`name`,`price`,`author`被称为**同胞节点**

---

#### **python 实现 Re 解析**

---

##### **Python 的 re 模块使用**

- 在 python 中使用正则表达式，可以使用`re`模块，`re`模块记住几个常用功能就足够我们日常使用了：

  ```python
  import re           #引入re模块

  #findall：匹配字符串中所有的符合正则的内容
  list = re.findall("\d+","我的电话号是10086,我朋友的电话是10010")           #findall的结果是一个列表
  print(list,"\n")

  #列表效率低下，面对大量数据难以应对，按如下处理
  #finditer：匹配字符串中所有的内容[返回的是迭代器],从迭代器中遍历拿到内容需要.group()函数
  it = re.finditer("\d+","我的电话号是10086,我朋友的电话是10010")
  #print(it)
  for i in it:
      print(i.group())
  print()

  #search返回的结果是match对象，那数据需要.group()，此外search全文检索，检索到一个就直接返回
  s = re.search("\d+","我的电话号是10086,我朋友的电话是10010")
  #print(s)
  print(s.group(),"\n")

  #match从头开始匹配，可以认为默认在正则前加了^符号，如下方10086前加一个非数字，则匹配为空
  a = re.match("\d+","10086,我朋友的电话是10010")
  print(a.group(),"\n")

  #compile预加载正则表达式，能够提高一定的运行效率
  obj = re.compile("\d+")
  #此时obj即预加载\d+的正则，下次使用可以obj.函数，如下：
  ret = obj.finditer("我的电话号是10086,我朋友的电话是10010")
  #print(ret)
  for it in ret:
      print(it.group())
  print()

  #用正则表示全部文本信息
  s="""
  <div class='jay'><span id='1'>雷军</span></div>
  <div class='jj'><span id='2'>李彦宏</span></div>
  <div class='jolin'><span id='3'>张小龙</span></div>
  <div class='sylar'><span id='4'>马云</span></div>
  <div class='tory'><span id='5'>马化腾</span></div>
  """
  obj1 = re.compile("<div class='.*?'><span id='\d+'>.*?</span></div>",re.S)            #re.S作用：让点.能匹配换行符
  #<div class='部分都一样，后面不一样部分.*?代替，匹配后jay双引号后部分一样，一直到id=后单引号后不同，
  #用\d或\d+或者.*?代替，后同理，略
  result = obj1.finditer(s)
  for it in result:
      print(it.group())
  print()

  #从全部文本信息中提取想要的信息(在上述代码中修改)
  #在要提取的文本.*?等正则符号处，用小括号包住，小括号前写?P<组名> ，最后在遍历的组括号(60行)写入这个组名
  obj1 = re.compile("<div class='.*?'><span id='\d+'>(?P<gualudeng>.*?)</span></div>",re.S)            #re.S作用：让点.能匹配换行符
  #<div class='部分都一样，后面不一样部分.*?代替，匹配后jay双引号后部分一样，一直到id=后单引号后不同，
  #用\d或\d+或者.*?代替，后同理，略
  result = obj1.finditer(s)
  for it in result:
      print(it.group("gualudeng"))
  print()
  ```

##### **手刃豆瓣 top250**

[豆瓣 top250](https://movie.douban.com/top250)

```python
#数据在页面源代码中
#思路：拿到页面源代码，通过re正则提取我们想要的有效信息
from email import header
import requests,re,csv

url = "https://movie.douban.com/top250"
ua = {"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.80 Safari/537.36"}
resp = requests.get(url,headers=ua)             #简单的提取源代码和反反爬
#print(resp.text)            #检查页面源码
page_content = resp.text            #保存源代码至变量

#解析数据
#正则表达式定位，建议找需要数据的上几层标签做定位
#<li>为上层标签，换行时的空白可能是换行可能是空格，使用.*?表示，继续匹配到下一行，后面多行都用.*?匹配，直接找到需要的title，在需要部分单独列组()，补充后面的截止部分(此处截止至</span>处)，后略
obj = re.compile('<li>.*?<div class="item">.*?<span class="title">(?P<title>.*?)</span>.*?<p class="">.*?<br>(?P<year>.*?)&nbsp.*?<span class="rating_num" property="v:average">(?P<score>.*?)</span>',re.S)               #编写正则方法

#使用finditer进行正则筛选
result = obj.finditer(page_content)
#遍历result，得到数据
for it in result:
    print("\n电影名：",it.group("title"),"\n年份：",it.group("year").strip(),"\n评分：",it.group("score"))            #group中的名字均为正则中的组名， .strip()为去除空白(空格)

#将数据存入文件，建议存储为csv格式。引入csv模块，.csv文件默认以逗号进行数据分割
f = open("data.csv",mode="w",encoding="utf-8")          #打开文件data.csv，没有文件自动创建，模式为r写入，打开格式为utf-8
csvwriter = csv.writer(f)               #创建csvwriter，写入数据时写入f文件，注意写入数据格式应为字典
result = obj.finditer(page_content)     #同18行
for it in result:
    dic= it.groupdict()                 #创建字典，将上述20-21行数据整理进字典
    dic["year"] = dic["year"].strip()           #单独处理需要去掉空格的year组
    csvwriter.writerow(dic.values())              #writerow为写入一行函数，括号()内为写入数据，写入的为字典的数据.values()
f.close()                               #关闭文件
print("over!")

#目前完成了top25的整理，而翻页数据只需要修改url后的参数即可，比如第二页url为https://movie.douban.com/top250?start=25&filter=
#由此得第一页参数start=0，第三页start=50，所以输出top250排行榜，可以此为方向研究
```

参考源代码：  
![](https://jsd.cky.codes/gh/ShengQiBaoZao/Image/pythoncankao1.png)

##### **屠戮盗版天堂电影信息**

[盗版天堂](https://dytt89.com/)

- 补充 html 中 a 标签超链接知识

  > 1、确认数据在页面源码中，定位到 2022 必看热片  
  > 2、从 2022 必看热片中提取到子页面链接地址  
  > 3、请求子页面的链接地址，拿到想要的下载地址

- 实际操作

  ```python
  import requests,re

  main_url = "https://dytt89.com/"            #主界面url
  child_url_list = []
  #原老版网站存在https加密，requests模块也有安全验证，所以会报错，可以使用verify=False关闭安全验证来解决，运行时最上部的警告意为“请求没有进行安全验证”。新版网站已取消
  resp = requests.get(main_url , verify=False)          #verify=False关闭安全验证
  resp.encoding = "gb2312"            #指定字符集编码
  #print(resp.text)            #输出乱码，需要重新编码，网页编码格式通常在源码charset处会写明，找到后补充上一行代码，更改默认编码
  #定位提取ul里面的li
  obj1 = re.compile('2022必看热片.*?<ul>(?P<ul>.*?)</ul>',re.S)           #提取需要的部分
  obj2 = re.compile("<a href='(?P<href>.*?)'",re.S)                       #提取a标签中的url链接

  #开始筛选提取
  result1 = obj1.finditer(resp.text)             #第一次提取板块源码部分
  for it in result1:
      ul = it.group("ul")                         #存入ul
      #print(ul)              #检验输出
      #html知识补充：在html中，a标签表示超链接，如：<a href='url'>周杰伦</a>，网页上显示周杰伦的超链接，跳转地址为href=后的url
      #提取子页面链接(href后url)
      result2 = obj2.finditer(ul)             #第二次从板块源码部分提取url，但提取的url为参数，需要与main_url拼接
      for itt in result2:
          add = itt.group("href")                 #存入add
          #print(add)              #检验输出
          child_url = main_url + add.strip("/")              #拼接url，使用strip除去拼接处多余的一个/符号
          #print(child_url)            #检验输出
          child_url_list.append(child_url)            #将网址保存进列表里(注意空列表已经提前定义)

  #提取子页面内容
  obj3 = re.compile('◎片　　名(?P<movie>.*?)<br />.*?<td style="WORD-WRAP: break-word" bgcolor="#fdfddf"><a href="(?P<download>.*?)">',re.S)
  for url in child_url_list:
      child_resp = requests.get(url)           #操作基本同上
      child_resp.encoding = "gb2312"
      #下两行仅为测试使用
      #print(child_resp.text)
      #break
      result3 = obj3.search(child_resp.text)
      print(result3.group("movie"))
      print(result3.group("download"))
  ```

- 参考源代码：
  ![](https://jsd.cky.codes/gh/ShengQiBaoZao/Image/pythoncankao2.png)

![](https://jsd.cky.codes/gh/ShengQiBaoZao/Image/pythoncankao3.png)

---

#### **python 实现 Bs4 解析**

---

##### **Python 的 bs4 模块使用**

- python 的 `bs4` 模块为第三方模块，需要先安装，安装 cmd 语法如下：

  ```bash
  pip install bs4
  ```

##### **抓取示例：北京新发地菜价**(已失效，仅可参考)

- **注**：页面重构，下示例代码仅可参考，无法运行，网站改为浏览器渲染，使用 POST 请求
  [北京新发地地址(已重构)](http://www.xinfadi.com.cn/priceDetail.html)

  ```python
  # 页面源代码中能找到数据，所以直接爬取，后使用bs4提取数据即可
  import requests
  import csv
  from bs4 import BeautifulSoup
  url = "http://www.xinfadi.com.cn/marketanalysis/0/list/1.shtml"
  resp = requests.get(url)
  # print(resp.text)           #测试

  # 准备需要写入的文件
  f = open("菜价.csv", mode="w")
  csvwriter = csv.writer(f)

  # 解析数据，把页面源代码交给beautiful soup处理，生成bs4的对象
  page = BeautifulSoup(resp.text, "html.parser")  # 括号第二个参数指定html解析器

  # 从bs4对象查找数据(find / find_all(标签 属性="值"))
  # 查找内容。由于class是python关键字，所以写class_代替
  table = page.find("table", class_="hq_table")
  # print(table)           #测试

  # 得到的是表格，表格内每一行为tr标签，每一行内每列为td标签
  # 再次筛选tr，拿到所有数据行，做切片，从1行开始切，去除0行的表头
  trs = table.find_all("tr")[1:]
  for tr in trs:              # 每一行的数据进行遍历
      tds = tr.find_all("td")              # 拿到每行中的所有td
      name = tds[0].text              # .text表示拿到被标签标记的内容
      low = tds[1].text
      avg = tds[2].text
      high = tds[3].text
      kind = tds[4].text
      set = tds[5].text
      date = tds[6].text
      # print(name, low, avg, high, kind, set, date)           #输出测试
      csvwriter.writerow([name, low, avg, high, kind, set, date])  # 写入文件，需要列表
  f.close()
  print("over")
  ```

- 参考源代码：  
  ![](https://jsd.cky.codes/gh/ShengQiBaoZao/Image/pythoncankao4.png)

##### **抓取优美图库的图片**(已失效，仅可参考)

[优美图库(已失效)](https://umei.cc/bizhitupian/weimeibizhi)

```python
# 1.拿到主页面的源代码，然后提取到子页面的链接地址，href
# 2.通过href拿到子页面的数据内容，提取图片的下载地址，img->src
# 3.下载图片

import requests
import time         # 对应37行代码
from bs4 import BeautifulSoup

url = "https://umei.cc/bizhitupian/weimeibizhi"
resp = requests.get(url)
resp.encoding = "utf-8"          # 解码处理
# print(resp.text)            #测试

# 把源代码交给bs4
main_page = BeautifulSoup(resp.text, "html.parser")
# 取得typelist后提取a标签
alist = main_page.find("div", class_="TypeList").find_all("a")
# print(alist)            #测试
for a in alist:             # 循环遍历每一个a标签
    # print(a.get("href"))        #测试，直接通过get就可以得到属性值
    href = a.get("href")
    # 至此任务1完成。进行任务2

    # 拿到子页面源代码
    child_resp = requests.get(href)
    child_resp.encoding = "utf-8"
    child_page_text = child_resp.text
    # 从子页面中拿到图片的下载路径
    child_page = BeautifulSoup(child_page_text, "html.parser")
    p = child_page.find("p", align="center")
    img = p.find("img")
    src = img.get("src")

    # 下载图片
    img_resp = requests.get(src)
    # img_resp.content         # content获取到的是字节，写回到文件就是图片
    img_name = src.split("/")[-1]        # 图片命名，对src链接以"/"切割，并取最后一部分命名
    with open(img_name, mode="wb") as f:          # wb写入二进制图片
        f.write(img_resp.content)       # 写入图片
    print("part success!", img_name)
    time.sleep(1)           # 防止访问过于频繁被封ip，休息1秒钟
print("all over!")
```

- 参考源代码：  
  ![](https://jsd.cky.codes/gh/ShengQiBaoZao/Image/pythoncankao5.png)  
  ![](https://jsd.cky.codes/gh/ShengQiBaoZao/Image/pythoncankao6.png)

---

#### **python 实现 Xpath 解析**

---

##### **Python 的 lxml 模块使用**

- python 的 `lxml` 模块为第三方模块，需要先安装，安装 cmd 语法如下：

  ```bash
  pip install lxml
  ```

##### **python 中 xpath 解析的使用**

- **示例程序**

  ```python
  from lxml import etree

  xml = """......"""          # 将XML文档存入变量，(此处省略，本程序无法直接运行)
  tree = etree.XML(xml)       # 生成etree的XML文档
  # result = tree.xpath("/book")        # xpath查找book节点，"/"表示层级关系，第一个"/"是根节点
  result1 = tree.xpath("/book/name/text()")        # text()表示获取被标记的内容
  print(result1)
  # 双斜杠"//"表示范围内跨层级搜索(全局搜索)
  result2 = tree.xpath("/book/author//nick/text()")
  # xpath中"*"符号为通配符，表示任意，同正则表达式的"."
  result3 = tree.xpath("/book/author/*/nick/text()")
  ```

- **补充**

  - `etree`可以打开存有 html 代码的文件：`tree = etree.parse(存有html的文件)`
  - `etree.xxx`需要跟相对应的类型，如源码为 html 则为`etree.HTML()`
  - 可以在需要的路径后跟`[n]`，表示索引，提取第 n 个节点(**注：从 1 开始**)，如：`/book[1]/name/text()`
  - 可以在需要的路径后跟`[@属性="值"]`，表示提取属性值为"值"的节点，如：`/book[@id="1"]/name/text()`。不加属性值，表示**定位到属性**
  - 使用**相对节点查找**时，可以使用`./`，代替表示当前节点
  - **小技巧**：当层级较多时，可以使用浏览器抓包工具，网页`检查`，找到对应代码，右键，copy->Xpath，就可以直接获取路径

- **爬取猪八戒网信息**  
  [青岛猪八戒网-搜索写入 saas](https://qingdao.zbj.com/search/f/?kw=saas)

  ```python
  import requests
  from lxml import etree

  url = "https://qingdao.zbj.com/search/f/?kw=saas"
  resp = requests.get(url)
  # print(resp.text)            #测试

  # 提取
  html = etree.HTML(resp.text)
  # 利用"检查"工具，查找一个代码块的路径，一层一层向外寻找，直到整个大页面的前一层，获取这一个窗口块的路径
  # xpath解析，处理[]的数量，使其对应所有窗口快，拿到每一个div
  divs = html.xpath("/html/body/div[6]/div/div/div[2]/div[5]/div[1]/div")
  for div in divs:            # for遍历divs
      # 使用相对路径查找

      # price = div.xpath("./div/div/a[1]/div[2]/div[1]/span[1]/text()")
      # 获取东西存放在列表，使用[0]获取列表第一个元素去除列表框，strip去除¥符号
      price = div.xpath("./div/div/a[1]/div[2]/div[1]/span[1]/text()")[0].strip("¥")

      # title = div.xpath("./div/div/a[1]/div[2]/div[2]/p/text()")
      # 由于saas是被搜索对象，被独立高亮没有获取到，所以将其拼接起来
      title = "saas".join(div.xpath("./div/div/a[1]/div[2]/div[2]/p/text()"))

      com_name = div.xpath("./div/div/a[2]/div[1]/p/text()")[0]
      place = div.xpath("./div/div/a[2]/div[1]/div/span/text()")[0]

      print(price, title, com_name, place)
  ```

---

#### **requests 模块进阶**

---

##### **requests 进阶概述**

- 我们在之前的爬虫中其实已经使用过`headers`了，`header`为 HTTP 协议中的**请求头**，一般存放一些和请求内容无关的数据，有时也会存放一些**安全验证信息**，比如常见的`User-Agent`，`token`，`cookie`等
- 通过`requests`发送的请求，我们可以把请求头信息放在 headers 中.也可以单独进行存放，最终由 requests 自动帮我们**拼接成完整的 http 请求头**
- 本章内容
  > 1、模拟浏览器登录 ---> 处理 cookie  
  > 2、防盗链处理 ---> 抓取梨视频  
  > 3、使用代理 ---> 防止被封 IP

##### **处理 cookie 模拟浏览器登录**

- 浏览器登录网页时，将用户名与密码上传到服务器，服务器内部进行校验，成功后将**登录信息返回写入 cookie**，下次进行请求时，可以连带 cookie 一起发送给服务器
- 示例：爬取 17k 小说网的用户书架内容  
  [17k 小说网](https://www.17k.com/)

  ```python
  # 登录，得到cookie
  # 带着cookie，去请求到书架的url，得到书架上的内容
  # 必须把上述两个操作连起来
  # 我们可以使用session进行请求，session你可以认为是一连串的请求，在这个过程中的cookie不会丢失
  import requests

  # 会话，直接获取session，创建会话
  session = requests.session()

  # 1.登录
  # 在用户登录界面F12抓包，登录后查看login文件，为请求文件
  # 内部url为登录界面url，from-data中loginName和password就是服务器需要的用户名和密码
  url = "https://passport/17k.com/ck/user/login"
  dat = {
      "loginName": "18614075987",
      "password": "q6039545"
  }
  session.post(url, data=dat)
  # resp = session.post(url, data=dat)        # 验证下述信息
  # print(resp.text)          # 查看登录后响应信息，判断是否成功
  # print(resp.cookies)       # 看cookie，确认信息

  # 2.拿书架数据
  # 在书架查看源代码，没发现书名，说明是浏览器渲染，抓包找到对应的数据文件
  # 不能使用requests.get，因为那样就打开了一个新的请求，不知道cookie，而会话session知道
  resp = session.get(
      "https://user.17k.com/ck/author/shelf?page=1&appKey=2406394919")
  print(resp.text)

  # 另一种使用requests.get的方法，在已登录书架界面，抓包到数据，将cookie手动补充到url请求头
  resp_ = requests.get("https://user.17k.com/ck/author/shelf?page=1&appKey=2406394919", headers={
      "cookie": "所需要的cookie，数据文件处抓包获得"
  })
  print(resp_.text)
  ```

##### **防盗链的处理**

- 示例：爬取梨视频的视频  
  [梨视频中的一段视频](https://www.pearvideo.com/video_1759202)

  - 梨视频的视频在**抓包工具**中视频存放在 `video` 标签下，而**源代码**中**看不到** video 标签，大致可以推断，视频 video 标签是后期通过 JS 脚本生成出来的
  - 所以对于视频，首先抓包检查工具大致确定视频地址，**再在源代码中确认一次**
  - 梨视频，抓包工具，刷新页面，`XHR` 中能找到有关视频链接的数据文件，从中获取到抓取到的视频地址
  - 抓到的视频不能直接播放，将其与原视频 url 对比，发现其中**有一段不同**。不同的部分，不能打开的链接将原先的 `cont-xxxx` 替换为了抓包得到数据中的**系统时间**，在视频的主页面，链接最后的一串数字就是`cont-`后需要的数字

  ```python
  # 1.拿到contId做拼接
  # 2.拿到videoStatus数据文件返回的json ---> srcURL被处理的视频链接
  # 3.将srcURL内的内容进行修整
  # 4.拿到视频的真实地址，进行下载

  import requests
  # 主页面的url地址，需要最后的一串数字contId作拼接
  url = "https://pearvideo.com/video_1759202"
  contId = url.split("_")[1]          # 通过下划线进行切割，取[1]段，即可取得1759202
  # 数据文件的url地址
  # videoStatus = "https://pearvideo.com/videoStatus.jsp?contId=1759202&mrd=0.20093235215815763"
  # 通过f-string进行contId的替换
  videoStatus = f"https://pearvideo.com/videoStatus.jsp?contId={contId}&mrd=0.20093235215815763"

  # 准备反反爬用的UA与防盗链
  header = {
      "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.80 Safari/537.36",
      # 防盗链Referer：请求到videoStatus时，其会进行溯源，可以类比为白名单，仅可从此链接请求才可以通过
      # 此处防盗链地址就是上面的url变量存的地址，即也可将此处字符串直接写为url变量
      "Referer": "https://pearvideo.com/video_1759202"
  }
  resp = requests.get(videoStatus, headers=header)
  # 首次输出显示"该文章已下线"，表示被反爬了，添加UA后二次输出仍不能解决，视频被防盗链Referer保护，将防盗链补充到请求中
  # print(resp.text)            # 测试输出

  # 需要获取到视频地址srcURL，使用.json获取json文件内容，按层级取需要的内容
  dic = resp.json()
  srcURL = dic["videoInfo"]["videos"]["srcUrl"]
  systemTime = dic["systemTime"]
  # 进行替换
  srcURL = srcURL.replace(systemTime, f"cont-{contId}")
  # print(srcURL)             # 测试

  # 下载视频
  with open("a.mp4", mode="wb") as f:
      f.write(requests.get(srcURL).content)           # content直接获取文件内容
  ```

##### **代理**

- 原理：通过**第三方**的机器去发送请求(需要自己提前寻找代理 IP)  
  [站大爷 IP](https://www.zdaye.com/dayProxy.html)  
  [66 免费代理](http://www.66ip.cn/)

  ```python
  import requests

  url = "https://www.baidu.com"

  # 本次所取代理IP：60.217.136.129
  # 代理端口：8060

  # 准备代理
  proxies = {
      # 协议是http还是https取决于访问的网站协议，字典value为"协议名://代理IP:端口"
      "https": "https://60.217.136.129:8060"
  }

  resp = requests.get(url, proxies=proxies)
  resp.encoding = "utf-8"
  print(resp.text)
  ```

##### **抓包工具的补充使用**

- 浏览器抓包工具中`Initiator`中`request call back`项记录了网站调用的 JS 栈，从下往上按时间顺序排列。点击可以进入 JS 源码，点击窗口左下方的**大括号**可以对源码进行缩进排版，找到需要的**发送行**设置断点，利用**断点调试**找到需要的信息，可以借此得到一些网站的**加密过程**或其他源码(涉及逆向 JS，较为复杂)

---

#### **线程与进程**

---

##### **基础概念**

- **进程**：操作系统运行程序时，会为其开辟一块**内存空间**，专门用于存放与此程序相关的数据，这块**内存区域**称为**xxx 进程**
- **线程**：在**xxx 进程**中存在多个**线程**，共同完成工作
- **进程**是**资源单位**，**线程**是**执行单位**。每一个进程**至少要有一个线程**，且程序执行时会有**一个主线程**，即可认为启动一个程序默认会有一个主线程

##### **多线程**

- 举例**单线程**：  
  (一个简单的线性单线程程序，主函数中，`func`函数执行完毕后才会执行主函数的`for`循环)

  ```python
  def func():
      for i in range(1000):
          print("func", i)

  if __name__ == "__main__":
      func()
      for j in range(1000):
          print("main", j)
  ```

- **多线程**示例 1，直接利用`Thread`类：

  ```python
  from threading import Thread            # 导入线程的类

  def func():
      for i in range(1000):
          print("func", i)

  if __name__ == "__main__":
      t = Thread(target=func)             # 创建一个线程类的对象，并且target=告诉程序这个线程执行的话会执行谁，为线程安排任务
      t.start()                           # 设置多线程状态为可以执行状态，具体的执行时间由CPU决定
      for j in range(1000):
          print("main", j)
  ```

- **多线程**示例 2，利用面向对象的特性，自己写一个继承`Thread`的`class`类：

  ```python
  from threading import Thread

  class MyThread(Thread):         # 继承Thread
      def run(self):          # 固定的        # 当线程被设置可以执行之后，被执行的就是run()
          for i in range(1000):
              print("子线程", i)

  if __name__ == "__main__":
      t = MyThread()
      # t.run()         #千万不能使用t.run，否则就是类方法的调用！--->单线程
      t.start()
      for j in range(1000):
          print("主线程", j)
  ```

- 多线程的**函数传参**

  ```python
  from threading import Thread

  def func(name):
      for i in range(1000):
          print(name, i)

  if __name__ == "__main__":
      # 在Thread函数中，添加args进行传参，且args接收的数据类型必须是元组
      # 注意，元组内只有一个元素的时候需要加逗号
      t1 = Thread(target=func, args=("周杰伦",))
      t1.start()
      t2 = Thread(target=func, args=("王力宏",))
      t2.start()
  ```

##### **多进程**

- 相对于多线程而言，多进程会**开辟新的空间**，**增加占用**，所以平常使用机会不大，多半可以由多线程代替
- **多进程**示例(基本与多线程类似)：

  ```python
  from multiprocessing import Process

  def func():
      for i in range(1000):
          print("子进程", i)

  if __name__ == "__main__":
      p = Process(target=func)
      p.start()
      for j in range(1000):
          print("主进程", j)
  ```

##### **线程池与进程池**

- **线程池**：**一次性**开辟一些线程，用户直接给线程池**提交任务**，可以节省开辟线程的资源，线程任务的**调度**交给**线程池来完成**
- **进程池**：同上线程池类似
- **线程池示例**

  ```python
  from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
  # 注：ThreadPoolExecutor为线程池，ProcessPoolExecutor为进程池，按需引入

  def fn(name):
      for i in range(1000):
          print(name, i)

  if __name__ == "__main__":
      # 创建线程池
      # 创建一个由50个线程组成的线程池，其中ThreadPoolExecutor的参数控制线程数量
      with ThreadPoolExecutor(50) as t:
          # 表示100个任务的for循环
          for i in range(100):
              # 给线程提交任务
              t.submit(fn, name=f"线程{i}")
      # 等待线程池的内容全部完成才会执行(守护)
      print("over")
  ```

---

#### **协程与异步**

---

##### **协程概念**

- **样例理解**：

  ```python
  import time

  def func():
      print("123")
      time.sleep(3)           # 让当前线程处于阻塞状态，CPU不为此程序工作
      print("456")

  if __name__ == "__main__":
      func()
  # 执行input()时，程序也是处于阻塞状态
  # requests.get()请求等待过程中，程序也是处于阻塞状态
  # 一般情况下，当程序处于IO操作时，线程都会处于阻塞状态
  ```

- **协程**：当程序遇见`IO操作`的时候，可以选择性的**切换到其他任务**上  
  **在微观上**是一个任务一个任务的**进行切换**，**在宏观上**我们能看见的是**多个任务一起共同执行**  
  这种操作称为**多任务异步操作**  
  上方所讲的一切，都是在**单线程的条件下**

##### **多任务异步协程**

- **语法理解**

  ```python
  import asyncio
  # 用async定义异步协程函数

  async def func():
      print("你好，我叫塞丽娜")

  if __name__ == "__main__":
      #此时的函数是异步协程函数，此时函数执行得到的是一个协程对象
      g = func()
      # print(g)            # 测试
      asyncio.run(func)           # 协程程序运行需要asyncio模块的支持
  ```

- **基本语法 1**

  ```python
  import asyncio
  import time

  async def func1():
      print("你好，我叫潘金莲")
      # time.sleep(3)           # 当程序出现同步操作时，异步就中断了
      await asyncio.sleep(3)            # 异步模块的sleep，使用await挂起，切到其他任务
      print("你好，我叫潘金莲")

  async def func2():
      print("你好，我叫王建国")
      # time.sleep(2)
      await asyncio.sleep(2)
      print("你好，我叫王建国")

  async def func3():
      print("你好，我叫李雪琴")
      # time.sleep(4)
      await asyncio.sleep(4)
      print("你好，我叫李雪琴")

  if __name__ == '__main__':
      f1 = func1()
      f2 = func2()
      f3 = func3()
      # 将需要执行的任务放入列表
      tasks_ = [
          f1, f2, f3
      ]
      t1 = time.time()          # 记录执行前的时间
      # 一次性启动多个任务(协程)
      asyncio.run(asyncio.wait(tasks_))
      t2 = time.time()          # 记录执行后的时间
      print(t2-t1)            # 输出运行时间
  ```

- **基本语法 2(推荐)**

  ```python
  import asyncio

  async def func1():
      print("你好，我叫潘金莲")
      await asyncio.sleep(3)
      print("你好，我叫潘金莲")

  async def func2():
      print("你好，我叫王建国")
      await asyncio.sleep(2)
      print("你好，我叫王建国")

  async def func3():
      print("你好，我叫李雪琴")
      await asyncio.sleep(4)
      print("你好，我叫李雪琴")

  async def main():
      # 第一种写法
      # f1 = func1()
      # await f1            #await通常放在协程对象前面
      # 后略
      #第二种写法(推荐)
      tasks_=[
          func1(),func2(),func3()
      ]
      await asyncio.wait(tasks_)

  if __name__ == '__main__':
      asyncio.run(main())
  ```

- 注意：**Python3.8**后会报**警告**，由于版本迭代，**Python3.11**后将不再支持`await asyncio.wait()`中直接传入协程对象，而是需要将协程对象通过`asyncio.create_task()`转换为**asyncio.Task**对象，才能使用，如下：

  ```python
  import asyncio

  async def func1():
      print("你好，我叫潘金莲")
      await asyncio.sleep(3)
      print("你好，我叫潘金莲")

  async def func2():
      print("你好，我叫王建国")
      await asyncio.sleep(2)
      print("你好，我叫王建国")

  async def func3():
      print("你好，我叫李雪琴")
      await asyncio.sleep(4)
      print("你好，我叫李雪琴")

  async def main():
      tasks_=[
          asyncio.create_task(func1()),
          asyncio.create_task(func2()),
          asyncio.create_task(func3())
      ]
      await asyncio.wait(tasks_)

  if __name__ == '__main__':
      asyncio.run(main())
  ```

---

#### **异步 http 请求**

---

##### **概述**

- 发送请求时，原先的`requests.get()`是一个同步操作，会将异步程序转为同步，需要换成 **异步请求操作**

##### **Python 的 aiohttp 模块使用**

- python 的 `aiohttp` 模块为第三方模块，需要先安装，安装 cmd 语法如下：

  ```bash
  pip install aiohttp
  ```

##### **基本语法-下载网站图片**

- **示例程序**

  ```python
  import asyncio
  import aiohttp

  urls = [
      "photo_url1",
      "photo_url2",
      "photo_url3"
  ]

  async def aiodownload(url):
      # 发送请求，得到图片内容，保存到文件
      # 得到异步会话
      # s = aiohttp.ClientSession()         # 等价于原来的requests模块，例如可使用s.get
      # 使用with上下文管理器，可以省去手动session.close()
      async with aiohttp.ClientSession() as session:
          # 发送请求，得到resp
          async with session.get(url) as resp:
              # 此处为读取图片，所以使用content
              # resp.content.read()         # 等价于原先的resp.content，此处需要多.read()读取
              # resp.text()               # 读取文本，需要多一个()
              # resp.json()               # 同原先相同
              # 写入文件
              with open("文件名(可以按url地址切割存入变量)", mode="wb") as f:
                  # IO操作，需要await挂起
                  f.write(await resp.content.read())

  async def main():
      tasks_ = []
      for url in urls:
          tasks_.append(aiodownload(url))
      await asyncio.wait(tasks_)

  if __name__ == '__main__':
      asyncio.run(main())
  ```

##### **示例：百度小说《西游记》**

- [百度小说《西游记》](https://dushu.baidu.com/pc/detail?gid=4306063500)，注意抓包时展开全部章节

  ```python
  # 所有章节内容的url(章节名称，cid)
  # https://dushu.baidu.com/api/pc/getCatalog?data={"book_id":"4306063500"}
  # 章节内部的具体内容
  # https://dushu.baidu.com/api/pc/getChapterContent?data={"book_id":"4306063500","cid":"4306063500|1569782244","need_bookinfo":1}

  import requests
  import asyncio
  import aiohttp
  import aiofiles
  import json

  # 1.同步操作，访问getCatalog，拿到所有章节的cid和名称
  # 2.异步操作：访问getChapterContent，拿到小说内容并下载

  async def aiodownload(cid, book_id, title):
      # 准备参数，作为json代码
      data = {
          "book_id": f"{book_id}",
          "cid": f"{book_id}|{cid}",
          "need_bookinfo": 1
      }
      # 转换json为字符串
      data = json.dumps(data)
      # 拼接url
      url = f"https://dushu.baidu.com/api/pc/getChapterContent?data={data}"
      # 准备异步请求
      async with aiohttp.ClientSession() as session:
          async with session.get(url) as resp:
              dic = await resp.json()
              # dic["data"]["novel"]["content"]           #小说的内容
              # 利用aiofiles异步写入文件
              async with aiofiles.open(f"D:\Desktop\代码程序\python测试\新建文件夹\{title}.txt", mode="w", encoding="utf-8") as f:
                  await f.write(dic["data"]["novel"]["content"])

  async def getCatalog(url):
      resp = requests.get(url)
      # print(resp.text)          # 测试
      dic = resp.json()
      # 准备异步的空列表
      tasks_ = []
      for item in dic["data"]["novel"]["items"]:              # item对应每个章节的名称和cid
          title = item["title"]
          cid = item["cid"]
          # print(cid,title)          # 测试
          # 每一个cid就是一个url，准备异步任务
          tasks_.append(aiodownload(cid, book_id, title))
      await asyncio.wait(tasks_)

  if __name__ == '__main__':
      # 将book_id提取为单独的变量备用
      book_id = "4306063500"
      url = 'https://dushu.baidu.com/api/pc/getCatalog?data={"book_id":"' + book_id + '"}'
      asyncio.run(getCatalog(url))
  ```

---

#### **selenium 模块**

---

##### **selenium 引入概念**

- 某些网页的**数据内容**被**加密**了，而浏览器显示的为**正常数据**，让程序**连接到浏览器**，让浏览器来完成各种复杂的操作，我们**只接收最终的结果**
- `selenium`模块：**自动化测试工具**。它可以打开浏览器，然后**像人一样**去操作浏览器。程序员可以从`selenium`中直接**提取**网页上的各种信息
- `selenium`的**配置安装**：
  > **安装 selenium 模块**：`pip install selenium`  
  > **下载浏览器驱动**：[selenium 驱动下载地址](https://www.selenium.dev/documentation/webdriver/getting_started/install_drivers/)，下载**对应浏览器**的**对应版本**驱动  
  > **驱动的存放**：将解压后的**浏览器驱动**放在**Python 解释器**所在的文件夹下，windows 下即为**python 安装目录**文件夹中
- **测试**(注意模块 Chrome 或其他浏览器要**首字母大写**):

  ```python
  from selenium.webdriver import Chrome

  # 1.创建浏览器对象
  web = Chrome()
  # 2.打开一个网址
  web.get("http://www.baidu.com")
  print(web.title)
  ```

##### **selenium 基础操作**

- **示例：抓取拉钩网站**  
  [拉钩网址](http://lagou.com)

  ```python
  from selenium.webdriver import Chrome
  from selenium.webdriver.common.keys import Keys

  import time
  web = Chrome()
  web.get("http://lagou.com")
  # 点击页面中的某个元素，通过在页面检查元素，复制xpath
  el = web.find_element_by_xpath('//*[@id="changeCityBox"]/p[1]/a')          # 找到元素
  el.click()              # 点击元素

  # 也可以直接写为web.find_element_by_xpath('//*[@id="changeCityBox"]/p[1]/a').click()
  # 可以通过by后不同的查找方式查找，如div标签这种页面中存在很多的元素，可以通过find_elements全部获取
  # web.find_elements_by_tag_name("div")

  # 防止刷新速度慢，暂停1秒
  time.sleep(1)

  # 找到输入框，输入python ---> 输入回车/点击搜索
  # 此处实现输入回车，找到输入框，使用.send_keys()输入内容
  # 键盘回车通过第二行的包中的Keys模块实现，点进Keys可以查看所有能实现的键盘按键
  web.find_element_by_xpath('//*[@id="search_input"]').send_keys("python", Keys.ENTER)

  time.sleep(1)

  # 查找存放数据的位置，进行数据提取(注：此处代码由于网页重构已失效，无法运行！)
  # 找到存放数据的所有li，注意获取多个最后li的[]索引要删除
  li_list = web.find_elements_by_xpath('//*[@id="s_position_list"]/ul/li')
  for li in li_list:
      job_name = li.find_element_by_tag_name("h3").text
      job_price = li.find_element_by_xpath("./div/div/div[2]/div/span").text
      company_name = li.find_element_by_xpath("./div/div[2]/div/a").text
      print(job_name, company_name, job_price)
  ```

##### **窗口之间的切换**

- **示例 1：抓取拉钩网站工作详情**

  ```python
  from selenium.webdriver import Chrome
  from selenium.webdriver.common.keys import Keys
  import time

  web = Chrome()
  web.get("http://lagou.com")
  web.find_element_by_xpath('//*[@id="changeCityBox"]/p[1]/a').click()
  time.sleep(0.5)
  web.find_element_by_xpath('//*[@id="search_input"]').send_keys("python", Keys.ENTER)
  time.sleep(0.5)

  # 点击岗位查看详情，会跳转到新页面
  # 点击岗位
  web.find_element_by_xpath('//*[@id="jobList"]/div[1]/div[1]/div[1]/div[1]/div[1]/a').click()

  # 如何进入到新窗口进行提取
  # 注意，即使浏览器已经切换新窗口，在selenium的眼中，新出现的窗口默认是不切换的(未被选中)
  # 切换窗口，使用window_handles[-1]选中最后一个窗口选项卡
  web.switch_to.window(web.window_handles[-1])

  # 在新窗口中提取内容
  job_detail = web.find_element_by_xpath('//*[@id="job_detail"]/dd[2]/div').text
  print(job_detail)
  # 关闭窗口
  web.close()
  # 重新调整窗口焦点
  web.switch_to.window(web.window_handles[0])
  ```

- **示例 2：处理 iframe 标签，示例站点 91 看剧**  
  [91 看剧视频(网址有效，视频失效)](https://www.91kanju.com/vod-play/541-2-1.html)

  ```python
  from selenium.webdriver import Chrome

  web = Chrome()
  # 页面中遇到iframe怎么处理
  web.get("https://www.91kanju.com/vod-play/541-2-1.html")
  # 要处理iframe，必须先得到iframe，然后切换视角到iframe，才能拿数据
  # 定位到iframe
  iframe = web.find_element_by_xpath('//*[@id="player_iframe"]')
  # 切入窗口视角到iframe
  web.switch_to.frame(iframe)
  # 切出窗口视角到默认窗口视角
  # web.switch_to.default_content()
  tx = web.find_element_by_xpath('//*[@id="main"]/h3[1]').text
  print(tx)
  ```

##### **无头浏览器、下拉菜单 select 的处理、拿到 elements 页面源码**

- **无头浏览器**：对于爬虫而言，浏览器的显示界面可以隐藏
- **示例：艺恩电影排行**  
  [艺恩电影排行(网址已失效)](https://endata.com.cn/BoxOffice/BO/Year/index.html)

  ```python
  from selenium.webdriver import Chrome
  from selenium.webdriver.chrome.options import Options
  from selenium.webdriver.support.select import Select
  import time

  # 无头浏览器
  # 准备好配置参数
  opt = Options()
  # 添加参数
  opt.add_argument("--headless")          # 无头
  opt.add_argument("--disable-gpu")           # 禁用gpu
  # =================================================================

  # 在Chrome()中参加无头参数
  web = Chrome(options=opt)
  web.get("https://endata.com.cn/BoxOffice/BO/Year/index.html")

  # 网址中有select下拉列表元素，如何处理
  # 定位到下拉列表
  sel_el = web.find_element_by_xpath('//*[@id="OptionDate"]')
  # 对元素进行包装，包装成下拉菜单，需要引入第二行的包
  sel = Select(sel_el)
  # 让浏览器进行调整选项
  # sel.options下拉框的列表的长度作为for循环次数，i就是每一个下拉框选项的索引位置
  for i in range(len(sel.options)):
      # select_by-xxx处，by_index为按照索引进行切换，by_value为按照select选项value进行切换，by_visible_text为按照所见文本进行切换
      sel.select_by_index(i)          # 按照索引i进行切换
      time.sleep(2)           # 等待切换
      # 提取数据，此处省略

  # =================================================================
  # 如何拿到页面源代码Elements数据(经过数据加载以及JS执行之后的结果的html内容)
  print(web.page_source)
  ```

##### **处理验证码**

- 处理简单的文字验证码可以通过写图像识别完成，但操作困难，难度大，且局限性强，因此使用市面上现成的验证码处理平台实现，如超级鹰

---

#### **页底评论**

---
