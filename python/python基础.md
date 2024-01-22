# Python基础



## - python安装

www.python.org 安装python解释器

www.jetbrains.com 安装pyCharm



## - pip python包管理器

pip指令

修改pip下载源



## - 注释

单行注释 #

多行注释 ''' ''' （多行字符）

python多行注释是字符串字面量实现的，与单行注释不同，多行字符串是一个实际的值，而不仅仅是用于解释代码的注释。在需要处理包含多行文本的字符串时，多行字符串是一个方便的语法结构。



## - 字面量和变量

代码中，被写在代码中的固定的值，称为字面量；

变量不是字面量，变量可以存储某个字面量， a = 123 在这个表达式中，a是变量，123是字面量

python中变量没有类型，变量的值有类型，也就是字面量的类型，使用``type()``可以判断变量的值的类型



## - 常用的值类型

![image-20240118205153089](C:\Users\ouxiaoxin\AppData\Roaming\Typora\typora-user-images\image-20240118205153089.png)



- ## python的算数运算符

python的运算符同样可以使用复合运算符+=、-=、//=、**= 等

![image-20240119143340306](C:\Users\ouxiaoxin\AppData\Roaming\Typora\typora-user-images\image-20240119143340306.png)

python中，字符串的拼接只能字符串+字符串，如果需要拼接其它类型需要强转成字符串；

字符串 * int表示重复该字符串int次



## - 逻辑运算符

and 短路与 性能优化设计，x and y 当x为False时，y不执行

or 短路或 性能优化设计，x or y 当x为True时，y不执行

not 非



## - 输出

格式化输出

```python
name = "xiaoxin"
age = 18
print("姓名：" + name + "年龄：" + str(age))
# 非字符串要强转 麻烦

# 格式化输出1
print("姓名：%s 年龄：%d" % (name, age))

# 格式化输出2
print(f"姓名：{name} 年龄：{age}")
```

## - 输入

```python
input() / input("输入提示")
# 输入的内容都为字符串，当其它类型使用时需要强转
```



## - 流程控制语句

1. if else

```python
if 条件:
    执行语句1
elif 条件:
    执行语句2
else:
    执行语句3
```

2. for

```python
1. 遍历任何序列，字符串、list、tuple等
str = "abcd"
for i in str:
    print(i) #遍历str字符串
    
2. range(): 控制循环访问，左闭右开
for i in range(5):
    print(i) #range(5) = (0, 1, 2, 3, 4)
    
for i in range(1,5):
    print(i) #range(1,5) = (1, 2, 3, 4)
    
for i in range(1, 11, 3):
    print(i) #range(1, 11, 3) = (1, 4, 7, 10)
    
```

3. while

## - 列表高级

1. 添加元素
   append:在末尾添加元素
   insert:在指定位置插入元素
   exend:合并两个列表

```python
ball_list = ["篮球", "台球", "羽毛球"]

ball_list.append("足球")

ball_list[1] = "乒乓球"

if "足球" in ball_list:
    print("存在")
else:
    print("不存在")
   
del ball_list(0)
ball_list.remove("篮球")
```

2. 修改元素：通过下标访问元素进行更改
3. 判断列表是否存在/不存在该元素：in / not in
4. 删除：
   del：根据**下标**进行删除
   remove：根据**元素的值**进行删除
   pop：弹出最后一个元素



## - 元组高级

区别于列表，**元组中的元素不可以被修改**
当元组中只有一个元素时，需要在这个元素后面加上逗号，否则就不是tuple类型，而是这个元素的类型

```````python
num_tuple = (1, 2, 3, 4, 5)
# num_tuple[0] = 0 元组不支持下标赋值，报错

str_tuple = ("wooo")
# 元组只有一个元素，这就不是元组，pycharm编辑器提示括号多余
# 正确写法：
str_tuple = ("wooo", ) # z

```````



## - 切片

切片是指对操作的对象截取其中一部分的操作。**字符串、列表、元组**都支持切片

切片的语法：[起始下标:终止下标:步长]，步长默认1，左闭右开



## - 字典高级

访问字典某个值的两种方法：

1. 中括号[ ]直接访问；访问不存在的key报错
2. get方法；访问不存在的key返回None值

```python
person = {"name": "john", "age": 18}
print(person["name"]) # 方法一
print(person.get("age")) # 方法二

print(person["gender"]) # 报错KeyError
print(person.get("gender")) # 输出None类型
```

添加/修改

```python
person["gender"] = "女" #首次使用的key添加新的键值对
person["gender"] = "男" #如果key已存在则是修改值
```

删除

```python
del person["gender"] #删除一对键值对
del person #删除整个字典

person.clear() #清除字典，保留字典结构
```

## - 遍历

```python
遍历字典的key
for key in person.keys():
    
遍历字典的value
for value in person.values():
    
遍历key和value
for key, value in person.items():
    
遍历字典的项/元素
for item in person.items():
```



## - 函数

1. ```python
   函数的定义
   def person(name, age):
       print("name:" + name)
       print("age:" + str(age))
   ```

2. ```python
   函数传参
   1.位置传参
   person("john", 18)
   
   2.关键字传参
   person(age = 18, name = "john")
   ```

3. ```python
   返回值
   def add(a, b):
   	return a + b 
   ```



## - 文件I/O

### 文件打开和关闭

```python
fp = open("文件路径", "打开模式")
fp.close()
```

### 打开模式

打开模式有两种：1. 文本模式；2. 二进制模式

![image-20240122115205439](C:\Users\ouxiaoxin\AppData\Roaming\Typora\typora-user-images\image-20240122115205439.png)



## - 序列化

导入json模块：json序列化就是把对象序列化成json字符串，因此仅对对象中的属性和数据进行序列化，而不能对对象中的方法等进行序列化。

使用json模块中的**dump、dumps**序列化方法
使用**load、loads**反序列化方法

```````````````````````````python
import json
name_tuple = ("张三", "lisi", "wanwu")
fp = open("file", "w")
json.dump(name_tuple, fp)
fp.close()
```````````````````````````



## - 异常

````python
捕获异常的格式
try:
	方法体
except 单个异常/（多个异常）:
    异常处理
````



# 爬虫实战

## 爬虫手段

## 反爬手段

## urllib库

urllib作用：模拟浏览器请求目标地址，并对网页的内容进行抓取处理

urllib基础请求：urllib.request.urlopen()

```python
引入urllib库
import urllib.request

使用urllib访问百度网页并接收响应信息
url = 'http://www.baidu.com'
response = urllib.request.urlopen(url)

response对象为HTTPResponse类型
方法：
read()、readline()、readlines()、getcode()、geturl()、getheaders()
```

下载资源：urllib.request.urlretrieve(下载地址， 目标文件)

```````````````````````````````````````````````````python
把百度主页下载到本地html.txt文件中（没有就自动创建）
url = "http://www.baidu.com"
urllib.request.urlretrieve(url, "html.txt")

下载图片
urllib.request.urlretrieve("xxx.jpg", "pho")
```````````````````````````````````````````````````







