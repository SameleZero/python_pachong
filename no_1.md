# -*- coding: utf-8 -*-
"""
Created on Sun Apr  8 08:45:41 2018

@author: Administrator
"""

"""第一章 零基础python网络爬虫"""
'''目录：
 1.1 认识Python网络爬虫
 1.2 网络爬虫工作原理详解
 1.3 网络爬虫常见类型与应用领域
 1.4 数据提取技术基础：正则表达式基础实例实战
 1.5 编写一个简单的网络爬虫爬取天善智能学院课程数据
'''

'''1.1 认识python爬虫:
网络爬虫是一种互联网信息的自动化采集程序，主要作用是代替人工对
互联网中的数据进行自动采集与整理，以快速地、批量地获取目标数据
从技术手段来说，网络爬虫有多种实现方案
如PHP、 Python（Urllib、requests、 scrapy、 selenium…）…
网络爬虫的难点并不在于网络爬虫本身，而在于网页的分析与爬虫的反爬攻克问题。
'''

#-------------------------------------------------------
'''1.2 网络爬虫工作原理详解:
定义爬取目标 -> 任务队列 -> 下载网页 -> 信息提取 -> 数据库 
循环提取信息：信息提取 -> 新URL -> 过滤 -> 任务队列 -> 下载网页
'''

#--------------------------------------------------------
'''1.3 网络爬虫常见类型与应用领域:
• 如下所示，是网络爬虫可以做的一些事情：
• 批量采集某个领域的招聘数据，对某个行业的招聘情况进行分析
• 批量采集某个行业的电商数据，以分析出具体热销商品，进行商业决策
• 采集目标客户数据，以进行后续营销
• 批量爬取腾讯动漫的漫画，以实现脱网本地集中浏览
• 开发一款火车票抢票程序，以实现自动抢票
'''

#--------------------------------------------------------
'''1.4 数据提取技术基础：正则表达式基础实例实战:
网络爬虫程序在将网页爬下来之后，其中还有一个关键的步骤就是需
对我们关注的目标信息进行提取，而表达式一般就是用于信息筛选提
的工具。
正则表达式是一种功能强大的表达式，并且非常好用，所以建议大家
握。
本知识点将为大家介绍正则表达式的基础，接下来将进入实战讲解
'''

#------------------------------------------------------
# chapter1
# 全局匹配函数使用格式	
# re.compile(正则表达式).findall(源字符串)

'''
普通字符	正常匹配
\n			匹配换行符  
\t 			匹配制表符
\w 			匹配字母、数字、下划线
\W 			匹配除字母、数字、下划线
\d 			匹配十进制数字
\D 			匹配除十进制数字
\s 			匹配空白字符
\S 			匹配除空白字符
[ab89x]		原子表，匹配ab89x中的任意一个
[^ab89x]		原子表，匹配除ab89x以外的任意一个字符
'''

# 实例1：
data = 'aliyunedu'
# 匹配yu
import re
re.compile("yu").findall(data)

# 实例2：
data = """aliyun
        edu"""
# 匹配yun\n
re.compile("yun\n").findall(data)

# 实例3：
data = "aliyu89787nedu"
# 匹配u89787
re.compile("\w\d\d\d\d").findall(data)

# 实例4：
data = "aliyu89787nedu"
# 匹配87ne [abc]匹配方框中a,b,c的任意一个
re.compile("\d\d[nedu]\w").findall(data)

#------------------------------------------------------
# chapter2
'''元字符
.	匹配除换行外任意一个字符
^	匹配开始位置
$	匹配结束位置
*	前一个字符出现0\1\多次 
?	前一个字符出现0\1次
+	前一个字符出现1\多次
{n}	前一个字符恰好出现n次
{n,}	前一个字符至少n次
{n,m}前一个字符至少n，至多m次 
|	模式选择符或
()	模式单元，通俗来说就是：想提取出什么内容，
就在正则中用小括号将其括起来
'''
# 实例5：
data = 'aliyunnnnji87362387aoyubaidu'
# 匹配aliyun
re.compile("aliyun").findall(data)

# 匹配以al开头以及后三个字符
re.compile("^al...").findall(data)

# 匹配以u结尾的至倒数前面五个字符
re.compile("....u$").findall(data)

# 尽可能匹配出多的字符
# Tips：python默认贪婪匹配,即默认尽可能多地进行匹配
re.compile("ali.*").findall(data)

# 运用'+'匹配尽可能多的'n'
re.compile("aliyun+").findall(data)

# 运用'?'匹配一个'n'
re.compile("aliyun?").findall(data)

# 运用'{}'匹配至少1个最多2个'n'
re.compile("aliyun{1,2}").findall(data)

# 以al开头匹配al后面的3个字符
re.compile("^al(...)").findall(data)

#------------------------------------------------------
# chapter3:
'''
贪婪模式：尽可能多地匹配
懒惰模式：尽可能少地匹配，精准模式
python默认贪婪模式
如果出现如下组合，则代表为懒惰模式：
*?
+?
'''
# 实例6:
data = 'poytphonyhjskjsa'
# 匹配'p' 'y'之间的字符
re.compile("p.*y").findall(data)

# 尽可能匹配少的py之间的字符:懒惰模式，精准匹配
re.compile("p.*?y").findall(data)

#------------------------------------------------------
# chapter4:模式修正符
'''
模式修正符:
在不改变正则表达式的情况下通过模式修正符使匹配结果发生更改
re.I(re.IGNORECASE): 忽略大小写（括号内是完整写法，下同）
re.M(re.MULTILINE): 多行模式，改变'^'和'$'的行为
re.S(re.DOTALL): 点任意匹配模式，改变'.'的行为，设置后可以匹配\n
re.L(re.LOCALE): 使预定字符类 \w \W \b \B \s \S 取决于当前区域设定
re.U(re.UNICODE): 使预定字符类 \w \W \b \B \s \S \d \D 取决于unicode定义的字符属性
re.X(re.VERBOSE): 详细模式。这个模式下正则表达式可以是多行，忽略空白字符，并可以加入注释。
<!--Tips:
re.compile(pat, string, flag=0)
re.findall(pat, string, flag=0)
re.match(pat, string, flag=0)
re.search(pat, string, flag=0)

其中flag就是使用标识的地方，可以替换为上述的标识，
如果要使用多个标识，则格式为：re.I|re.M|re.S|
-->
'''
# 实例7：
data = "Python"
# 没注重大小写是匹配不到的
re.compile("pyt").findall(data)

# 增加参数,忽略大小写匹配
re.compile("pyt",re.I).findall(data)

# 实例8：
data ="""我是阿里云大学
欢迎来学习
Python网络爬虫课程
"""
# 匹配大学到课程，因为跨行了所以无法匹配 
re.compile("大学.*Python").findall(data)

# 增加参数,可以跨行
re.compile("大学.*Python",re.S).findall(data)

# 忽略多行忽略大小写
re.compile("大学.*python",re.S|re.I).findall(data)

#----------------------------------------------------------
# 综合应用
data = "ab12\nbdc"  
pat1 = "^a.*2$"  
pat2 = "a.*d"  
pat3 = "^a.*c$" 

# None，因为没有使用多行，所以第一行的结尾为'\n'而不是‘2’
re.compile(pat1).findall(data)

# 返回 'ab12'，因为使用了多行，所以第一行可以匹配出结果
re.compile(pat1,re.M).findall(data)

# 对于跨行的内容进行匹配时，re.M不能生效 
re.compile(pat2,re.M).findall(data)

# 使用了re.S，则没有换行的概念，所以整个字符串作为1行来匹配
re.compile(pat2,re.S).findall(data)
re.compile(pat3,re.S).findall(data)



#---------------------------------------------------
'''1.5 编写一个简单的网络爬虫：爬取天善智能学院课程数据
目标网址：https://edu.hellobi.com/
目标数据：课程名、课程讲师、课程价格

Tips:思路
观测课程网页地址结构https://edu.hellobi.com/course/261
                   https://edu.hellobi.com/course/159
开头都是以https://edu.hellobi.com/course/
后面加上对应数字序号
联想到可以运用 for 循环爬取1-1000序号的课程

运用urllib包抓取解析
1.urllib.request.urlopen函数
2.使用正则提取所需信息
3.if判读信息存在与否
'''

# 引入所需的库
import urllib.request
import re 

# 利用for循环抓取课程
# i = 0
# 建立一个空的列表
Title   = []
Teacher = []
Price   = []
for i in range(0,100):
    # 拼接你所需要的爬的网页
    url_0 = "https://edu.hellobi.com/course/" + str(i + 1)
    ''' 读取网页urllib.request.urlopen(url_0).read()
        decode递交编码 编码的话可以右击网页查看源代码一般会在开头声明'''
    web = urllib.request.urlopen(url_0).read().decode("utf-8","ignore")
    
    '''正则提取课程  直接在查看源代码crt+f搜索:技术更新,
    <li class="active">技术更新，战术升级Python爬虫案例实战从零开始一站通（连载中）</li>
    可以试运行下你写的正则是否正确 re.compile(title_pat,re.S).findall(web)  '''
    title_pat = '<li class="active">(.*?)</li>'
    
    '''正则提取实际价格 直接在查看源代码属于对应的价格查找比如899
    <span class="price-expense"><sub>￥</sub>299.00<b></b></span>
    '''
    price_pat = '<span class="price-expense">.*</sub>(.*?)<b></b></span>'
    
    '''正则提取老师名字
     <a href="https://www.hellobi.com/u/datamofang" target="_blank"
                   class="lec-name">杜雨 <i class="fa fa-vimeo personal" title="个人认证"></i></a>
    '''
    teacher_pat = 'class="lec-name">(.*?)<'
    
    # 匹配信息
    title = re.compile(title_pat,re.S).findall(web)
    if len(title) > 0:
       Title.insert(i, title )
    else:
       Title.insert(i,'缺失')
    
    teacher = re.compile(teacher_pat,re.S).findall(web)
    if len(teacher) > 0:
        Teacher.insert(i,teacher)
    else:
        Teacher.insert(i,'缺失')
    
    price=re.compile(price_pat,re.S).findall(web)
    if(len(price)>0):
        Price.insert(i,price)
    else:
        Price.insert(i,'免费')
    
    print(Title[i])
    print(Teacher[i])
    print(Price[i])
    print("--------------")
# 存储信息
course = {}
course['title'] = Title
course['teacher'] = Teacher
course['price'] = Price
print(course)

# 小羽同学布置的^-^课后任务:
'''除了上述目标增加：
   抓取 课程简介,
   抓取 下单人数,
'''
