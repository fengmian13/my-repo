# python学习

## 基础知识：

字面量的含义：

代码中，被写在代码中的固定的值，称之为字面量

______________________________________________



注释：在程序代码中对程序代码进行解释说明的文字

	单行注释：以#开头，#右边的所有文字当作说明，而不是真正要执行的程序，起辅助说明作用
	#我是单行注释

```
多行注释：
以 一对三个双引号引起来("""注释内容""")来解释说明一段代码的作用使用方法
```

__________________________

变量

变量名 = 变量值

```python
money = 50
```

____________________

数据类型

查看数据的类型：type()语句

```python
print(type(666))
```

- 转换类型

```python
转换语句：
int(x)		将x转换成一个整数
float(x)	将x转换成一个浮点数
str(x)		将x转换成字符串
.......
```

-  任何类型都可以转换成字符串
- 只有字符串内是数字才能转换成“数字类型”
- 浮点数转整数会丢失精度，小数部分

```python
num = input("请输入一个数字：")
print("input的标砖输入类型是：",type(num))
num = int(num)
print("转换后num的输出类型是：",type(num))
```



____________

标识符：是用户在编程的时候所使用的一系列名字，用于给变量、类、方法等命名

只允许出现：

- 英文
- 中文（不推荐）
- 数字（不能放在开头）
- 下划线  _

1. 标识符大小写敏感（英文全小写）
2. 不可使用关键字

________

运算符（有区别的）

| 运算符 | 描述       | 案例        |
| ------ | ---------- | ----------- |
| //     | 去整除     | 11 // 2 = 5 |
| **     | 指数       | 2 ** 3 = 8  |
| =      | 赋值运算符 |             |

-------

字符串

- 单引号定义法：'定义的内容'
- 双引号定义法："双引号定义的内容"
- 三引号定义法：写法和多行注释是一样的

```python
nane  = 
"""
我是
wl
一个程序员
"""
```

```python
#在字符串内 包含双引号	name ='"黑马程序员"'
#在字符申内 包含单引号	name ="'黑马程序员'"
#使用转义字符|解除引号的效用
#name ="\"黑马程序员\""
#"name ='\'黑马程序员\''
```

**字符串的拼接**：通过 + 连接

注意：拼接操作 无法与非字符串类型进行拼接

占位拼接：%s + % 变量

```python
name ="黑马程序员"
message ="学IT来:%s" % name
print(message)
结果：	学IT来:黑马程序员
```

```python
class_num =57
avg_salary = 16781
message ="Python大数据学科，北京%s期，毕业平均工资:%s"%(class_num，avg_salary)
print(message)
结果：Python大数据学科，北京57期，毕业平均工资:16781
```

**python**中的占位符

| 格式符号 | 转化                             |
| -------- | -------------------------------- |
| %s       | 将内容转换成字符串，放入占位位置 |
| %d       | 将内容转换成整数，放入占位位置   |
| %f       | 将内容转换成浮点型，放入占位位置 |

**字符串格式化-数字精度控制**

我们可以使用辅助符号"m.n"来控制数据的宽度和精度
m，控制宽度，要求是数字(很少使用),设置的宽度小于数字自身，不生效.n，控制小数点精度，要求是数字，会进行小数的四舍五入
示例:
%5d:表示将整数的宽度控制在5位，如数字11，被设置为5d，就会变成:[空格]|[空格]|[空格]11，用三个空格补足宽度。
%5.2f:表示将宽度控制为5，将小数点精度设置为2小数点和小数部分也算入宽度计算。如，对11.345设置了%7.2f后，结果是:[空格][空格]11.35。2个空格补足宽度，小数部分限制2位精度后，四舍五入为.35
%.2f:表示不限制宽度，只设置小数点精度为2，如11.345设置%.2f后，结果是11.35

*宽度小于数字长度时，无效*

**快速字符串格式化**

通过语法:f"内容(变量)"的格式来快速格式化-------不限制数据类型，不进行精度控制

```python
name ="传智播客"
set_up_year =2006
stock_price = 19.99
#f:format
print(f"我是{name}，我成立于:{set_up_year}年，我今天的股价是:{stock_price}")
结果是：我是传智播客，我成立于:2006年，我今天的股价是:19.99
```

**字符串格式化 - 表达式格式化**

一条具有明确执行结果的代码语句

--------------------

**数据输入input**

```python
name = input("请告诉我你是谁")
```

input永远输入内容的数据类型是 **字符串类型**

-------------

### 判断

```python
result = 10 > 5		-->true
result = "itnlah" == "boosdb"    --->false
```

![image-20241001103256220](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241001103256220.png)

#### **if**

注意事项：

- 判断条件的结果一定要是布尔类型
- 不要忘记判断条件后的：冒号
- 归属于if语句的代码块，需要在前方填充4个空格缩进

```python
if 要判断的条件：
	条件成立时，要做的事

age = 30
if age >= 18:
	print("我已经成年了")
else：
	print("您未成年，可以免费游玩")

 if ....:
    print(".....")
elif ......:
    print("......")
else
	print("......]")
```

----------------

### 小types

 不换行：

```python
print("hello",end='')
print("world")
结果：helloworld
```

 制表符	\t

```python
print("Hello\tWorld")
结果：Hello	world
```

换行：

```python
print(" ")
```



##### **is和==的区别**

```
==是python标准操作符中的比较操作符，用来比较判断两个对象的value(值)是否相等；
is也被叫做同一性运算符，这个运算符比较判断的是对象间的唯一身份标识，也就是``id``是否相同。
```

------

### for循环

轮询机制，对内容逐个处理》对待处理数据集里面的数据挨个取出，每次循环将当前数据赋予给临时变量

语法中的:待处理数据集，严格来说，称之为:序列类型序列类型指，其内容可以一个个依次取出的一种类型，包括:字符串，列表，元组，等

#### 基础语法

- for循环的语法格式是：

```
for临时变量in待处理数据集（序列）：
	循环满足条件时执行代码
```

- for循环的注意点

1. 无法定义循环条件，只能被动取出数据处理
2. 循环内的语句，需要有空格缩进

#### range语句

- 语法1	range(num)

- 获取一个从0开始，到num结束的数字序列(不含num本身)

  ```python
  for x in range(10)
  ```

  - 语法2	range(num1, num2)

  从num1开始，到num2结束(不包括num2本身)的一个数字序列,数字间隔是1

  ```python
  for x in range(5, 10):
  ```

- 语法3 range(num1, num2, step)

从num1开始，到num2结束(不包括num2本身)的一个数字序列,数字间隔是step

```python
for x in range(5,10,2):
```

-------------

### 函数

组织好的，可重复使用的、用来实现特定功能的代码段

-   将代码封装在函数内，可供随时随地重复使用
- 提高代码的复用率，减少重复代码，提高开发效率

#### 函数的定义

```
def 函数名(传入参数)：
	函数体
	return 返回值
```

如何将函数内定义的变量声明为全局变量

- 使用global关键字，global变量

------

### 数据容器

####  列表[]

列表内的每一个数据，称之为元素

- 以[]作为标识符
- 列表内每一个元素之间用，逗号隔开

定义变量：
$$
变量名称 = [元素1,元素2,元素3,元素4，.....]
$$
定义空列表
$$
变量名称 = []
变量名称 = list()
$$

```
列表的下标索引是什么？
列表的每一个元素，都有编号称之为下标索引
```

**列表的查询功能：**查找元素在列表中的下标
$$
语法：列表.index(元素)
$$

```
my_list = ["bhjabfi","abdsiavi","abndouab"]
index =  my_list.index("abdsiavi")
```

**列表的修改功能：**修改列表中指定下标的元素
$$
语法：列表[下标] = 值
$$

```
my_list = ["bhjabfi","abdsiavi","abndouab"]
my_list[0] = "oisobioh"
```

**插入元素**
$$
语法:列表.insert(下标,元素)，在指定的下标位置，插入指定的元素
$$

```
my_list = ["bhjabfi","abdsiavi","abndouab"]
my_list,insert(1,"svcdokibu")
my_list = ["bhjabfi","svcdokibu","abdsiavi","abndouab"]
```

**追加功能(单个）**
$$
语法:列表append(元素)，将指定元素，追加到列表的尾部
$$

```
my_list = ["bhjabfi","abdsiavi","abndouab"]
my_list,append("saocub")
my_list = ["bhjabfi","abdsiavi","abndouab","saocub"]
```

**一批元素**
$$
语法:列表.extend（数据容器)，将其他数据容器的内容取出，依次追加到列表尾部
$$

```
my_list = ["bhjabfi","abdsiavi","abndouab"]
my_list.extend(["hresv","kjhgf","qwegfv"])
my_list = ["hresv","kjhgf","qwegfv","hresv","kjhgf","qwegfv"]
```

**删除功能**
$$
说明一下del	 pop 与remove的区别：可以简单的记为一命令二方法，del为删除命令，pop与remove为list的内置方法。
$$
方式一：列表[下标]

```
del my_list[2]
```

方法二：列表（下标）

将列表下标的值取出来，返回出去

```
my_list = ["bhjabfi","abdsiavi","abndouab"]
elementy = my_list.pop(2)
```

**删除某元素在列表中的第一个匹配项**
$$
语法：列表。remove(元素)
$$

```
my_list = [1,2,3,4,2,1]
my_list.remove(2)
结果：[1,3,4,2,1]
```

**清空列表内容**
$$
语法：列表.clear()
$$

```
my_list = [1,2,3,4,2,1]
my_list.clear()
结果：[]
```

**统计某元素在列表中的数量**
$$
语法：列表.count（元素）
$$
**统计全部元素的数量**
$$
len(列表)
$$

```
my_list = [1,2,3,4,2,1]
count = len(my_list)
结果：6
```

------

### **元组()**

元组同列表一样，都是可以封装多个、不同类型的元素在内。 

但最大的不同点在于:
元组一旦定义完成，就不可修改
$$
元组定义:定义元组使用小括号，且使用逗号隔开各个数据，数据可以是不同的数据类型。
$$

```python
变量名称 =(元素，元素元素)
变量名称 = ()		#方法1
变量名称 = tuple()	#方法2
```

**注意:元组只有一个数据，这个数据后面要添加逗号**

![image-20241005090803882](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241005090803882.png)

------

### **字符串**

**index方法**
$$
字符串.index()
$$
**字符串的代替**
$$
语法:字符串.replace(字符串1，字符串2)功能:将字符串内的全部:字符串1，替换为字符串2
注意:不是修改字符串本身，而是得到了一个新字符串
$$

```
str = "aoifsgduu hb kla"
num = str.index("hb")
print(num)
new = str.replace("hb", "hello")
print(f"旧的{str},新的{new}")

结果：
10
旧的aoifsgduu hb kla,新的aoifsgduu hello kla
```

**字符串分割**

```
语法:字符串.split(分隔符字符串)功能:按照指定的分隔符字符串，将字符串划分为多个字符串，并存入列表对象中
注意:字符串本身不变，而是得到了一个列表对象
```

![image-20241005102910032](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241005102910032.png)

-----

### **序列**

内容连续、有序，可使用下标索引的一类数据容器

**切片**

序列支持切片，即:列表、元组、字符串，均支持进行切片操作切片:从一个序列中，取出一个子序列
语法:序列**[起始下标:结束下标:步长]**表示从序列中，从指定位置开始，依次取出元素，到指定位置结束，得到一个新序列:起始下标表示从何处开始，可以留空，留空视作从头开始 结束下标(不含)表示何处结束，可以留空，留空视作截取到结尾

步长1表示，一个个取元素
步长2表示，每次跳过1个元素取
步长N表示，每次跳过N-1个元素取
步长为负数表示，反向取(注意，起始下标和结束下标也要反向标记)

----

### **集合**{}

- 不允许重复
- 无序的，所以不支持：下标索引
- 允许修改

```
{元素，元素，元素，元素}
变量名称 = {元素，元素，.....，元素}
定义空集合
变量名称 = set()
```

**添加元素.add()**

```python
my_set.add("isaudgf")
```

**移除元素.remove()**

```py
my_set.remove("fsdakjb")
```

**随机取元素.pop()**

```python
emp = my_set.pop()
```

**清空集合.clear()**

```python
my_set.clear()
```

**取2个集合的差集.difference(集合2)**

语法:集合1.difference(集合2)，功能:取出集合1和集合2的差集(集合1有而集合2没有的)

```python
set1 = {1,2,3,4}
set2 = {3,4,5,6}
set3 = set1.difference(set2)

set3 = {1,2}
```

**消除2个集合的差集**

语法:集合1.difference_update(集合2)

功能:对比集合1和集合2，在集合1内，删除和集合2相同的元素。

结果:集合1被修改，集合2不变

```python
set1 = {1,2,3,4}
set2 = {3,4,5,6}
set3 = set1.difference_update(set2)
print(set1)
print(set2)
print(set3)

 {1, 2}
{3, 4, 5, 6}
None
```

**两个集合合并	集合1.union(集合2)**

语法:集合1.union(集合2)
功能:将集合1和集合2组合成新集合

结果:得到新集合，集合1和集合2不变

```python
set1 = {1,2,3,4}
set2 = {3,4,5,6}
set3 = set1.union(set2)

set3 = {1,2,3,4,5,6}
set1 = {1,2,3,4}
set2 = {3,4,5,6}
```

**统计合计元素数量len()**

```python
set1 = {1,2,3,4,5}
num = len(set1)
num = 5
```

**集合遍历for**

集合不支持下标索引，不能用while循环可以用**for**循环

```python
set1 =  {1,2,3,4,5}
for a in set1:
	print(a)
```

![image-20241005155647885](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241005155647885.png)

------

### 字典

- 不支持下标索引

字典的定义，同样使用{}，不过存储的元素是一个个的:键值对，如下语法

```python
# 定义字典字面量
{key:value,key:value,......,key: value}# 定义字典变量
my_dict={key: value, key:value,key: value}# 定义空字典
my_dict ={}				#空字典定义方式1
my_dict = dict()		# 空字典定义方式2
```

  

**新增元素**

```python
my_dict["张信哲"] = 66
```

**更新元素**

```python
my_dict["张信哲"] = 33
```

两者取决于元素是否存在，存在就**更新**不存在就**新增**

**删除元素**

```python
score = my_dict.pop("周杰伦")
```

**清空元素**

```python
my_dict.clear()
```

**获取全部的key**

```python
my_dict.keys()
```

**遍历字典**

```python
my_dict = {"张学友": "中国", "周杰伦": "中国", "usher": "美国"}
keys = my_dict.keys()
#for key in keys:
#	print(key)
for key in keys:
	print(my_dict[key])
    
 for key in my_dict:
    print(key)
 

```

**统计数量len**

```
num = len(my_dict)
```

--------

### 数据容器的分类

![image-20241005172924405](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241005172924405.png)

![image-20241005172942821](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241005172942821.png)

![image-20241005173055646](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241005173055646.png)

### **数据容器的通用操作**

最大元素 max()

最小元素 min()

**类型转化：容器转列表**list()

```python
mylist = [1,2,3,4,5]
mytuple = (1,2,3,4,5)
str = "hello world"
myset = {1,2,3,4,5}
mydict = {"name":"itcast","age":18}
print(list(mylist))
print(list(mytuple))
print(list(str))
print(list(myset))
print(list(mydict))
结果：
[1, 2, 3, 4, 5]
[1, 2, 3, 4, 5]
['h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd']
[1, 2, 3, 4, 5]
['name', 'age']
```

**类型转化：容器转元组**tuple()

```python
mylist = [1,2,3,4,5]
mytuple = (1,2,3,4,5)
str = "hello world"
myset = {1,2,3,4,5}
mydict = {"name":"itcast","age":18}
print(tuple(mylist))
print(tuple(mytuple))
print(tuple(str))
print(tuple(myset))
print(tuple(mydict))
结果：
(1, 2, 3, 4, 5)
(1, 2, 3, 4, 5)
('h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd')
(1, 2, 3, 4, 5)
('name', 'age')
```

**类型转化：容器转字符串**str()

```python
mylist = [1,2,3,4,5]
mytuple = (1,2,3,4,5)
mystr = "hello world"
myset = {1,2,3,4,5}
mydict = {"name":"itcast","age":18}
print(f"你好，我是{str(mylist)}")
print(f"你好，我是{str(mytuple)}")
print(f"你好，我是{str(mystr)}")
print(f"你好，我是{str(myset)}")
print(f"你好，我是{str(mydict)}")
结果：
你好，我是[1, 2, 3, 4, 5]
你好，我是(1, 2, 3, 4, 5)
你好，我是hello world
你好，我是{1, 2, 3, 4, 5}
你好，我是{'name': 'itcast', 'age': 18}
```

**类型转化：容器转集合**set()

```python
mylist = [1,2,3,4,5]
mytuple = (1,2,3,4,5)
mystr = "hello world"
myset = {1,2,3,4,5}
mydict = {"name":"itcast","age":18}
print(set(mylist))
print(set(mytuple))
print(set(mystr))
print(set(myset))
print(set(mydict))
结果：
{1, 2, 3, 4, 5}
{1, 2, 3, 4, 5}
{'d', 'r', 'l', ' ', 'o', 'w', 'h', 'e'}	#由于无序性，导致混乱，且去重了
{1, 2, 3, 4, 5}
{'age', 'name'}
```

### **容器的通用排序功能**

sorted(容器，[reverse = Ture])将容器进行排序，reverse控制排序结果，Ture反转

对内容进行排序，放入列表中

-----

### **函数扩展**

**多个返回值**

```
def test_return():
	return 1,{2,3,4}
	
x, y = test_return()

print(x)
print(y)
1
{2, 3, 4}
```

**多种传参种类**

关键字参数：函数调用时通过“键=值”形式传参

作用：可以让函数更加清晰、容易使用，同时也清除了参数的顺序需求

函数调用时，如果有位置参数时，位置参数必须在关键字参数的前面，但关键字参数之间不存在先后顺序

![image-20241005210004357](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241005210004357.png)

**缺省参数**默认参数

 默认的必须写到最后

**不定长**

不定长-位置不定长，*号
不定长-关键字不定长，**号

![image-20241005212751971](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241005212751971.png)

![image-20241005212824474](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241005212824474.png)

**lambda匿名函数**
函数的定义中
def关键字，可以定义带有名称的函数，可以基于名称重复使用
lambda关键字，可以定义匿名函数(无名称)有名称的函数，无名称的匿名函数，只可临时使用一次，
匿名函数定义语法:
$$
lambda 传入参数:函数体(一行代码)
$$
lambda 是关键字，表示定义匿名函数
传入参数表示匿名函数的形式参数，如:x，y表示接收2个形式参数函数体，就是函数的执行逻辑，要注意:只能写一行，无法写多行代码

![image-20241005214720842](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241005214720842.png)

----------

### 文件的操作

1.什么是编码?
编码就是一种规则集合，记录了内容和二进制间进行相互转换的逻辑。编码有许多中，我们最常用的是UTF-8编码
2.为什么需要使用编码?
计算机只认识0和1，所以需要将内容翻译成0和1才能保存在计算机中。
同时也需要编码，将计算机保存的0和1，反向翻译回可以识别的内容。

**open()打开函数**

在Python，使用open函数，可以打开一个已经存在的文件，或者创建一个新文件，语法如下

```python
open(name, mode, encoding)
```

name :是要打开的目标文件名的字符串(可以包含文件所在的具体路径)。mode:设置打开文件的模式(访问模式):只读、写入、追加等,encoding:编码格式(推荐使用UTF-8)

mode:

- r读

- w写：没有时创建，存在时覆盖
- a追加

**read()方法**

```python
文件对象.read(num)
```

num表示要从文件中读取的数据的长度(单位是字节)，如果没有传入num，那么就表示读取文件中所有的数据

**readlines()方法:**
readlines可以按照行的方式把整个文件中的内容进行一次性读取，并且返回的是一个**列表**，其中每一行的数据为一个元素。

![image-20241006112039399](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241006112039399.png)

**readline()方法**:

一次读取一行内容,返回的是一个**字符串**

```python
例子：注意里面指针的问题
f = open("E:/测试.txt","r",encoding="utf-8")
print(type(f))
print(f"读取12字节\t{f.read(12)}")
print(f"读取全部内容\t{f.read()}")

lines = f.readlines()
print(type(lines))
print(lines)
for line in lines:
    print(line)
    
lined = f.readline()
print(type(lined))
print(lined)
for line in lined:
    print(line)
```

**close()关闭文件对象**

![image-20241006114121727](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241006114121727.png)

**with open语法**

![image-20241006115044287](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241006115044287.png)

**文件写入**

write（）方法：

直接调用write，内容并未真正写入文件，而是会积攒在程序的内存中，称之为缓冲

当调用flush的时候，内容会真正写入文件这样做是避免频繁的操作硬盘，导致效率下降(攒一堆，一次性写磁盘)

**flush()**

刷新内容到硬盘

**文件的追加**

mode：a模式，文件不存在时会创建一个新文件

----

### 异常     模块       包

#### **异常捕获**

```
捕获所有异常
try:
	可能出现的异常
except (Exception as e):
	如果出现异常执行的代码
	
捕获指定异常
try:
	可能出现的异常
except NameError as e:
	如果出现异常执行的代码

捕获多个异常
try:
	可能出现的异常
except (NameError,ZeroDivisionError) as e:
	如果出现异常执行的代码
	print(e)

```

异常是具有传递性的

**模块**

Python 模块(Module)，是一个 Python 文件，以.py 结尾. 模块能定义函数，类和变量，模块里也能包含可执行的代码.
模块的作用:python中有很多各种不同的模块，每一个模块都可以帮助我们快速的实现一些功能，比如实现和时间相关的功能就可以使用time模块我们可以认为一个模块就是一个工具包，每一个工具包中都有各种不同的工具供我们使用进而实现各种不同的功能.

![image-20241006171548874](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241006171548874.png)

**自定义模块**

![p'ypy](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20241006175640406.png)

-----

**python包**

从物理上看，包就是一个文件夹，在该文件夹下包含了一个 _init-.py 文件，该文件夹可用于包含多个模块文件从逻辑上看，包的本质依然是模块

```python
# import ni.my_module1
# import ni.my_module2
#
# ni.my_module1.info_print()
# ni.my_module2.info_print()

from ni import my_module1
from ni.my_module2 import info_print2()
my_module1.info_print()
info_print2()
```







-----

### **题**

#### **您如何将字符串转换为全部小写？**

***回答：\***要将字符串转换为小写，可以使用lower（）函数。

**例：**

```python
stg='ABCD'
print(stg.lower())
```

**输出：**

```python
abcd
```

**换大写**upper()

例：

```python
str = "asdqwe"
print(str.upper())

结果：
ASDQWE
```

