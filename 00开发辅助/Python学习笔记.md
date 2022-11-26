# Python学习笔记
[toc]

## 一、基本语法

`python2.x与python3.x区别很大，新版特性很强，更优化，此笔记学习新版本特性，暂不关注旧版本。`

- 标识符

  > 1. 标识符：数字、字母、下划线，且数字不开头。
  > 2. 区分大小写；
  > 3. 单下划线开头`_foo`表示不能直接访问，需要用接口访问，不可"from xxx import"导入。`__foo`双下划线表示私有。前后双下划线`___foo__`特殊函数。

- 保留字符

  > and、exec、not、assert、finally、or、break、for、pass、class、from、print、continue、global、raise、def、if、return、del、import、try、elif、in、while、else、is、with、except、lambda、yield。

- 行与缩进

  > **Python区别于其他语言最大处，python不用{}来控制类、函数和逻辑块。而是用缩进，空格数无妨，但是逻辑块前后必须相同个数空格，使用空格或者tab都行，但是最好别混用**
  >
  > ```python
  > if True:
  >  print "True"
  > else:
  >  print "False"
  > ```
  >
  > python一般新的一行意味着上个语句块结束。但是多行语句可以用`\`符号：
  >
  > ```python
  > string = abc + \
  > 		bcd + \
  >  	efg
  > # 要是有{}、[]、()的语句，断为多行也无妨。
  > girls = ['allen','angle'
  >      'candy','lily'
  >      'merry']
  > ```

- 引号与注释

  > python的字符串可以用单引号`'`、双引号`"`、三引号`'''`包括，其中三引号可以多行分段，有时候可以用作注释。
  >
  > `#`符号表示注释，可写在当行，或者代码尾部。多行注释使用三个单引号` ''' `，或三个双引号`"""`包括。
  >
  > ```python
  > #单行注释
  > if True
  > 	print 'hello' #单个注释，新版中需要加()，视为函数
  > '''
  > 多行注释
  > 是这么写的
  > 真的可以么
  > 在markdown语法中
  > '''
  > """
  > 似乎可以
  > 好像是的哦
  > """    
  > ```
  >
  > *函数之间，类与函数间用`空行`分开，便于阅读和维护*

- 零碎语法

  >- 多条语句同一行，可以`;`分割。
  >
  >- 同一代码组，首行关键字后`:`结尾，同一代码组前后空白数相同。
  >
  >- 旧版中文编码需要头文件注释
  >
  >- Python中True和False，似乎不能小写哦！
  >
  > ```python
  > #!/usr/bin/python
  > #_*_coding:utf-8 _*_
  > #或者
  > #coding=utf-8
  > #新版python已经默认支持了。
  > ```
  >


##  二、 数据与逻辑

- 变量类型

  > **python变量赋值不需要类型声明**
  >
  > ```python
  > counter = 100 #整型
  > miles = 1000.0 # 浮点型
  > name = "John" #字符串
  > a = b = c = 1 #多个变量同时赋值
  > a, b, c = 1,2,"John" #多变量多类型赋值,个数必须对应。
  > ```
  >
  > python五种标准数据类型：
  >
  > - Numbers数字
  >
  >   该类型变量赋值后不可改变，重新赋值实质上是分配新的对象。
  >
  >   ```python
  >   a = 1,b=2,c=9
  >   #del是删除语句
  >   del a
  >   del a,b
  >   # Number有四种不同类型，int、long、float、complex（复数）
  >   #注意：python新版中，没有long，多了一个bytes。
  >   8899887897l#后面的l容易与1混淆，一般写作L
  >   ```
  >
  > - String字符串
  >
  >   由字母、数字、下划线组成，两种顺序，0开始至后，-1开始向前。
  >
  >   string的截取
  >
  >   ```python
  >   s = 'I`m a good boy'
  >   s[3:9]#其结果是截取对应标号的字符串为新的字符，可以0--size，也可以-1--向前。记住区间[)右边取不到哦
  >   print s #输出s字符串
  >   print s[1]#输入下标为1的字母
  >   print s[1:5]#输出下标1-5，但不含5那个字符
  >   print s + "add" # 字符串连接
  >   print s * 2 # 重复输出两次
  >   ```
  >
  > - List列表
  >
  >   python列表可以混合不同类型数据，嵌套列表，可以使用类似string的方法取子列表。`[]`表示
  >
  >   ```python
  >   list = ['python',666,2.14,'study']
  >   tinylist = [123,'John']
  >   print list #完整输出
  >   print list[0]#输出指定下标元素
  >   print list[1:3]#输出1-3的元素
  >   print list[2:]#输出2之后所有元素
  >   print tinylist * 2 # 重复两次输出
  >   print list + tinylist#组合列表
  >   ```
  >
  > - Tuple元组
  >
  >   元组类似list，`()`标识，`,`分割，不能二次赋值。
  >
  >   ```python
  >   list = [1,2,'abc']
  >   tuple = (1,2,'abc')
  >   list[0] = 'ABC'#可以更新
  >   tuple[0]= 4 #错误，元组数据不能更新
  >   ```
  >
  > - Dictionary字典
  >
  >   Dictionary被视为python最为灵活的内置数据结构，列表为有序对象集合，字典则为无序元素结合。区别在于，Dictionary类似与map集合，键值对key-value。`{}`标识
  >
  >   ```python
  >   dict = {}
  >   dict['one'] = "This is one"
  >   dict[2] = "This is 2"
  >   tinydict = {'name':'john','code':1234,'dept':'good',22:879.0L}
  >   print dict['one'] #根据键值输出元素
  >   print dict # 输出所有元素
  >   print dict.keys()#输出所有key
  >   print tinydict.vales()#输出所有值value
  >   ```
  >
  > **类型转换**，对应数据类型作为函数名即可,返回结果。
  >
  > |         函数          |                        描述                         |
  > | :-------------------: | :-------------------------------------------------: |
  > |    int(x [,base])     |                  将x转换为一个整数                  |
  > |   long(x [,base] )    |                 将x转换为一个长整数                 |
  > |       float(x)        |                 将x转换到一个浮点数                 |
  > | complex(real [,imag]) |                    创建一个复数                     |
  > |        str(x)         |                将对象 x 转换为字符串                |
  > |        repr(x)        |             将对象 x 转换为表达式字符串             |
  > |       eval(str)       | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
  > |       tuple(s)        |               将序列 s 转换为一个元组               |
  > |        list(s)        |               将序列 s 转换为一个列表               |
  > |        set(s)         |                   转换为可变集合                    |
  > |        dict(d)        |  创建一个字典。d 必须是一个序列 (key,value)元组。   |
  > |     frozenset(s)      |                  转换为不可变集合                   |
  > |        chr(x)         |              将一个整数转换为一个字符               |
  > |       unichr(x)       |             将一个整数转换为Unicode字符             |
  > |        ord(x)         |             将一个字符转换为它的整数值              |
  > |        hex(x)         |         将一个整数转换为一个十六进制字符串          |
  > |        oct(x)         |          将一个整数转换为一个八进制字符串           |
  >

- 运算符号

  >Python运算符支持一下类型：
  >
  >- 算术运算符
  >- 比较（关系）运算符
  >- 赋值运算符
  >- 逻辑运算符
  >- 位运算符
  >- 成员运算符
  >- 身份运算符
  >
  >**运算符有优先级**
  >
  >1. 算术运算符
  >
  >| 运算符 | 描述       | 实例    |
  >| ------ | ---------- | ------- |
  >| +      | 加号       | 1+2得3  |
  >| -      | 减号       | 3-1得2  |
  >| *      | 乘号       | 2*2得4  |
  >| /      | 除号       | 9/3得3  |
  >| %      | 取模，求余 | 5%2得1  |
  >| **     | 幂         | 2**3得8 |
  >| //     | 取整除     | 9//2得4 |
  >
  >  示例：
  >
  >  ```python
  >  #算术运算符，得到运算结果
  >  a,b,c = 12,2,3#多变量同时赋值
  >  print (a+b)
  >  print (a-b)
  >  print (a*b)
  >  print (a/b)
  >  print (a%c)
  >  print (b**c)
  >  print (c//b)
  >  ```
  >
  >2. 比较运算符
  >
  >| 运算符 | 描述                   |
  >| ------ | ---------------------- |
  >| ==     | 等于，比较对象是否相等 |
  >| !=     | 不等于                 |
  >| <>     | 不等于                 |
  >| >      | 大于                   |
  >| <      | 小于                   |
  >| \>=    | 大于等于               |
  >| <=     | 小于等于               |
  >
  >  示例：
  >
  >  ```python
  >  #关系运算符，返回结果为 True或False
  >  a,b,c = 12,2,3
  >  print(a==b)
  >  print(a!=b)
  >  print(a<>b)#，新版python中，已经废弃。
  >  print(a>b)
  >  print(a<b)
  >  print(a>=b)
  >  print(a<=b)
  >  ```
  >
  >3. 赋值运算符
  >
  >| 运算符 | 描述     | 实例                     |
  >| ------ | -------- | ------------------------ |
  >| =      | 简单赋值 | c=a+b,将a+b的结果赋值给c |
  >| +=     | 加法赋值 | c+=a等效于c=c+a          |
  >| -=     | 减法赋值 | c-=a等效于c=c-a          |
  >| *=     | 乘法赋值 | c*=a等效于c=c\*a         |
  >| /=     | 除法赋值 | c/=a等效于c=c/a          |
  >| %=     | 取模赋值 | c%=a等效于c=c%a          |
  >| **=    | 幂赋值   | c\*\*=a等效于c=c\*\*a    |
  >| //=    | 取整赋值 | c//=a等效于c=c//a        |
  >
  >  示例：
  >
  >  ```python
  >  #简单演示
  >  a,b,c=12,2,3
  >  #print(c=a)#不能直接在print内写赋值
  >  c=a
  >  print(c)
  >  c=c**b
  >  print(c**=b)
  >  ```
  >
  >4. 位运算符
  >
  >  位运算既是将数字看作二进制，进行运算。
  >
  >| 运算符 | 描述                                                         |
  >| ------ | ------------------------------------------------------------ |
  >| &      | **`按位与`**：两数二进制对齐，对应位置都是1，则为1，否则为0。 |
  >| \|     | **`按位或`**：两数二进制，对应位置有一个为1，则为1。         |
  >| ^      | **`按位异或`**：两数二进制，对应位置相异，则为1。            |
  >| ~      | **`按位取反`**：对数据的二进制，各个位置取相反，变1为0，变0为1。 |
  >| <<     | **`左移动`**：运算数二进制全部左移动若干位，"<<"右边的数决定左移位数。**高位丢弃，低位补0** |
  >| \>>    | **`右移动`**：运算数二进制全部右移动软敢为，">>"右边的数决定右移位数。 |
  >
  >  示例：
  >
  >  ```python
  >  a = 60 # 60 = 0011 1100
  >  b = 13 # 13 = 0000 1101
  >  c = 0
  >  c = a & b
  >  print("a & b :",c)# 12 = 0000 1100
  >  c = a | b
  >  print("a | b :",c)# 61 = 0011 1101
  >  c = a ^ b
  >  print("a ^ b :",c)# 49 = 0011 0001
  >  c = ~a
  >  print("~a :",c)# -61 = 1100 0011
  >  c = a<<2
  >  print("a<<2 :",c) # 240 = 1111 0000
  >  c = a>>2
  >  print("a>>2 :",c) # 15 = 0000 1111
  >  ```
  >
  >5. 逻辑运算符
  >
  >| 运算符 | 表达式  | 描述                                                         |
  >| ------ | ------- | ------------------------------------------------------------ |
  >| and    | x and y | **`布尔与`** ：如果x为False，x and y返回False，否则返回y的计算值。 |
  >| or     | x or y  | **`布尔或`**：如果x非零，返回x值，否则返回y的计算值。        |
  >| not    | not x   | **`布尔非`**：若x为True，返回False，若x为False，返回True。   |
  >
  >  示例:
  >
  >  ```python
  >  #True是1，False是0
  >  """所以可以在逻辑运算中判断，是否为0 或者1，来决定true和false"""
  >  a, b, c = 12,2,3
  >  print(a and b)#似乎非0，即认为True
  >  print(a or b)
  >  print(not a)
  >  ```
  >
  >6. 成员运算符
  >
  >| 运算符 | 描述                                                   |
  >| ------ | ------------------------------------------------------ |
  >| in     | x in y，若有True，无False。**`y为列表、字符串、元组`** |
  >| not in | x not in y 若y中无x则True，有则False。                 |
  >
  >  示例：
  >
  >  ```python
  >  a = 10;b = 20;list = [1,2,10,15]
  >  print(a in list)
  >  print(a not in list)
  >  print(b in list)
  >  ```
  >
  >7. 身份运算符
  >
  >| 运算符 | 描述                                                         |
  >| ------ | ------------------------------------------------------------ |
  >| is     | `is`判断两标识符是否引用同一对象，id同则返回True，否则False。 |
  >| is not | `is not`判断两标识符是不是引用不同对象，id不同则返回True，否则False。 |
  >
  >  示例：
  >
  >  ```python
  >  a = 10;b = 20;
  >  print(a is b);print(a is not b);
  >  ```
  >
  >8. 运算符优先级
  >
  >| 运算符                          | 优先级描述                       |
  >| ------------------------------- | -------------------------------- |
  >| **                              | 指数，最高优先级                 |
  >| ~，+，-                         | 按位取反、一元加(+@)、一元减(-@) |
  >| *，/，%，//                     | 乘、除、取模、取整               |
  >| +，-                            | 加法、减法                       |
  >| \>>，<<                         | 右移动、左移动                   |
  >| &                               | 位'AND'                          |
  >| ^，\|                           | 位运算                           |
  >| <=，<，>，>=                    | 比较运算符                       |
  >| <>，==，!=                      | 等于运算符                       |
  >| =，%=，/=，//=，-=，+=，*=，**= | 赋值运算符                       |
  >| is，is not                      | 身份运算符                       |
  >| in ，not in                     | 成员运算符                       |
  >| not，or，and                    | 逻辑运算符                       |
  >
  >  **注意：若是不确定优先级，可以用()来控制**

- 条件语句

  > Python中指定非0，和非null，的值为Ture，0和null为False。
  >
  > ```python
  > # python 中多个条件语句，if..elif...elif...else
  > if condition1:
  >  statement1
  > elif condition2:
  >  statement2
  > else:
  >  statement
  > if (1!=2) : print "OK";#单行语句，可以放在一起。
  > ```
  >
  > **python 没有switch语句，只能多个elif语句结合逻辑符号`and,or,not`来控制多分支语句。**

- 循环语句

  > 编程语言基本都有for、while和嵌套循环。python没有do...while循环。
  >
  > 循环控制语句有break、continue和pass。
  >
  > - while循环
  >
  >   ```python
  >   # while 循环格式
  >   while condition:#条件只有True和False的本质区别，但是请记住，Python中的True的定义比较广，非0，非null即为True。
  >       statement
  >   ```
  >
  >   控制语句break、continue
  >
  >   - break，退出循环。
  >   - continue，跳出本次循环，进入下一次。不执行continue之后的语句。
  >
  >   *无限循环：判断条件为永远为True*
  >
  > - $for...else$
  >
  >   Python中有for...else和while...else循环。
  >
  >   `表示for或while语句正常执行后，else语句执行。`注意：循环体正常执行，而不是break出来，才会运行else语句。
  >
  >   每一轮的循环，for或while内没有break的话，都会执行else语句。
  >
  > - for循环
  >
  >   用于遍历任何序列项目，如列表字符串。格式：
  >
  >   - 普通方式
  >
  >   ```python
  >   for iterating_var in sequence:
  >       statements
  >   
  >   #如下示例
  >   
  >   for letter in 'python text':
  >       print ('当前字母：',letter)
  >   
  >   fruits = ['banana','apple','orange']
  >   for fruit in fruits:
  >       print("水果：",fruit)
  >   ```
  >
  >   - 使用索引
  >
  >     ```python
  >     fruits = ['banana','orange','apple']
  >     for index in range(len(fruits)):
  >         print("fruit:",fruits[index])
  >     ```

- 循环嵌套

  > 类似其他编程语言，python也允许循环嵌套。
  >
  > ```python
  > for iterating_var in sequence:
  >  for interating_var in sequence:
  >      statements
  >  statements
  > #python中注意缩进来区分代码块，while嵌套类似，也可以互相嵌套。
  > ```
  >
  > - break、continue语句类似其他编程语言，上面已做介绍。
  >
  > - pass语句，不做任何事情，一般用做占位语句
  >
  >   ```python
  >   for letter in 'python':
  >       if letter == 'h':
  >           pass
  >       	print("just for blank")
  >       print("letter:",letter)
  >   ```


## 三、结构与api

- Number数据类型

  > Python的Number数据类型，不允许改变，每次赋值都会是一个新的对象。
  >
  > ```python
  > a = 10
  > b = 12
  > a = 20 #内存地址已经变了，不像java那样。
  > #del语句删除
  > del a
  > del a,b
  > ```
  >
  > **Number数据支持四种子类型**
  >
  > - int整型，正数、负数，无小数点。
  > - long长整型，无限大小的整数，后加`L`或`l`。
  > - float浮点型，整数、小数和科学计数。
  > - complex复数，实数和虚数构成，a+bj或complex(a,b)，a实数，b虚数。

- Python数学函数

  > | 函数            | 返回值                                                       |
  > | --------------- | ------------------------------------------------------------ |
  > | abs(x)          | 绝对值：返回x的绝对值                                        |
  > | ceil(x)         | 取大整：返回x的上入整数，不是四舍五入。如math.ceil(4.1)得5   |
  > | cmp(x,y)        | 对比：x\<y，返回-1，x=y返回0，x\>y 返回1                     |
  > | exp(x)          | e次幂：e的x次幂                                              |
  > | fabs(x)         | 绝对值：返回x的绝对值，含小数点。                            |
  > | floor(x)        | 取小整：返回x的向下整数。                                    |
  > | log(x)          | log函数                                                      |
  > | max（x1，x2...) | Max函数。                                                    |
  > | min(x1,x2...)   | Min函数。                                                    |
  > | modf(x)         | 分离：返回x的整数和小数部分，符号与x相同，整数部分为float型。 |
  > | pow(x,y)        | 幂函数：x**y的值                                             |
  > | rount(x[,n])    | 舍入值：x的四舍五入，舍入到小数后n位。                       |
  > | sqrt(x)         | 开方：x的平方根，x可为负数，返回实数。                       |
  >
  > **Math函数**
  >
  > - 随机函数random
  >
  >   | 函数                           | 描述                                             |
  >   | ------------------------------ | ------------------------------------------------ |
  >   | choice(seq)                    | random.choice(range(10))，从0--9随机挑一个整数。 |
  >   | randrange([start],stop,[step]) | 指定范围制定基数获取随机数。                     |
  >   | random()                       | [0,1)内随机数                                    |
  >   | seed([x])                      | 随机数生成器的种子？？                           |
  >   | shuffle(lst)                   | 序列元素的随机排序                               |
  >   | uniform(x,y)                   | [x,y]范围内随机生成下一个实数                    |
  >
  >   ==注意函数参数[step]之类的，表示为可选参数==
  >
  > - 三角函数
  >
  >   | 函数       | 描述                                    |
  >   | ---------- | --------------------------------------- |
  >   | acos(x)    | 反余弦（弧度值）                        |
  >   | asin(x)    | 反正弦                                  |
  >   | atan(x)    | 反正切                                  |
  >   | atan2(y,x) | 指定x，y坐标的反正切值                  |
  >   | cos(x)     | 余弦                                    |
  >   | hypot(x,y) | 返回欧几里得范数sqrt(x\*x+y\*y)         |
  >   | sin(x)     | 正弦                                    |
  >   | tan(x)     | 正切                                    |
  >   | degress(x) | 弧度转角度。如degress(math.pi/2),得90.0 |
  >   | radins(x)  | 角度转弧度                              |
  >
  > - 数学常量
  >
  >   | 常量 | 描述      |
  >   | ---- | --------- |
  >   | pi   | 圆周率    |
  >   | e    | 自然常数e |

- 字符串

  > python中字符串string可用`'`或`"`符号，无char类型。
  >
  > ```python
  > #字符串可以用[]类似java数组方式截取
  > a = 'abc'
  > b = "hello python"
  > 
  > print('a[0]',a[0])
  > print("b[2:7],b[2:7])#关于字符串的截取可参照上一章数据类型里的描述。
  > ```
  >
  > python可以对string字符串更新操作：
  >
  > ```python
  > a = 'hello python'
  > print('updated:',a[:6]+'beautiful girl'
  > ```
  >
  > - 类似其他语言，python中也使用`\`转义其他特殊字符。
  >
  >   | 转义字符     | 描述                                 |
  >   | ------------ | ------------------------------------ |
  >   | \ (在行尾时) | 续行符号                             |
  >   | \\\          | 反斜杠                               |
  >   | \'           | 单引号                               |
  >   | \"           | 双引号                               |
  >   | \a           | 响铃                                 |
  >   | \b           | 退格                                 |
  >   | \e           | 转义                                 |
  >   | \000         | 空                                   |
  >   | \n           | 换行                                 |
  >   | \v           | 纵向制表符                           |
  >   | \t           | 横向制表符                           |
  >   | \r           | 回车                                 |
  >   | \f           | 换页                                 |
  >   | \oyy         | 八进制，yy表字符，如：\o12代表换行   |
  >   | \xyy         | 十六进制，yy表字符，如：\x0a代表换行 |
  >   | \other       | 其他字符将以普通格式输出             |
  >
  > - 字符串运算
  >
  >   | 操作符 | 描述                                                   |
  >   | ------ | ------------------------------------------------------ |
  >   | +      | 串联                                                   |
  >   | *      | 重复                                                   |
  >   | []     | 截取[index],index位置的字符                            |
  >   | [ : ]  | 截取`:`前后数字范围内，如，[1:4)取不到右边数字的字符。 |
  >   | in     | 成员运算符，含有返回True                               |
  >   | not in | 成员运算符，不含有返回True                             |
  >   | r/R    | 原始字符串，不转义，原始输出。                         |
  >
  > - 字符串格式化
  >
  >   类似C 语言的printf函数，将需要格式化的数字字符，格式化后传入%s之类的占据的位置。==格式化==
  >
  >   | 符号 | 描述                       |
  >   | ---- | -------------------------- |
  >   | %c   | 字符及其ASCII码            |
  >   | %s   | 字符串                     |
  >   | %d   | 整数                       |
  >   | %u   | 无符号整型                 |
  >   | %o   | 无符号八进制数             |
  >   | %x   | 无符号十六进制数           |
  >   | %X   | 无符号十六进制数，大写     |
  >   | %f   | 浮点数子，可指定小数点精度 |
  >   | %e   | 科学计数法格式化浮点数     |
  >   | %E   | 同%e                       |
  >   | %g   | %f和%e的简写               |
  >   | %G   | %f和%e的简写               |
  >   | %p   | 十六进制格式化变量的地址   |
  >
  >   - 格式化操作符辅助指令
  >
  >     | 符号   | 功能                                |
  >     | ------ | ----------------------------------- |
  >     | *      | 定义宽度或小数精度                  |
  >     | -      | 左对齐                              |
  >     | +      | 正数前显示+号                       |
  >     | `<sp>` | 正数前显示空格                      |
  >     | #      | 八进制前显示0，十六进制前显示0x或0X |
  >     | 0      | 显示的数字前填充0，而不是空格       |
  >     | %      | '%%'输出显示一个'%'                 |
  >     | (var)  | 映射变量（字典参数）                |
  >     | m.n.   | m显示最小总宽度，n小数后的位数。    |
  >
  > - python的三引号
  >
  >   **三引号` ````通常把复杂的字符串，整段的复制输出，而不管其中是否换行、转义之类的。
  >
  >   ==`u`==符号表示Unicode编码，如：
  >
  >   ```python
  >   #如下，则为Unicode格式
  >   u'hello world'
  >   u'hello\u0020world'#效果如上，内部使用了\0020代表空格
  >   ```
  >
  > ==往后的api会越来越多，本笔记将不再赘述各个类型和函数的api。==

- List列表

  > Python有6个序列类型，常见的为==列表==和==元组==。序列常用操作：`索引`、`切片`、`加`、`乘`、`检查成员`，以及最大最小值的获取。
  >
  > - 列表，格式`[ ]`内用`,分隔。
  >   ```python
  >   	list = [1,'abc',False,list,"hello pythono"];#列表元素类型可以不同，可以嵌套列表。类似数组，可以索引，左0，右-1
  >   ```
  >   更新，直接赋值新的元素。删除，del对应元素。
  >
  > - python列表脚本操作符
  >
  >   | 表达式                   | 结果                      | 描述                 |
  >   | ------------------------ | ------------------------- | -------------------- |
  >   | len([1,2,3])             | 3                         | 长度                 |
  >   | [1,2,3]+[4,5,6]          | [1,2,3,4,5,6]             | zu'he                |
  >   | ['Hi!']*4                | ['Hi!','Hi!','Hi!','Hi!'] | 重复                 |
  >   | 3 in [1,2,3]             | True                      | 判断元素是否属于列表 |
  >   | for x in [1,2,3]:print x | 1 2 3                     | 遍历 迭代            |
  >
  >   **列表的截取，类似字符串的操作。通过索引，取值范围来截取。**
  >
  > - Python列表相关的函数&方法
  >
  >   ```python
  >   cmp(list1,list2);#比较两个列表
  >   len(list);#list长度、元素个数
  >   max(list);#list最大元素值
  >   min(list);#list最小元素值
  >   list(seq);#将元组转化为列表
  >   
  >   list.append(obj);#列表尾新增对象
  >   list.count(obj);#统计某元素出现次数
  >   list.extend(seq);#用新列表扩展
  >   list.index(obg);#元素第一次出现位置
  >   list.insert(index,obj);#指定位置插入
  >   list.pop(obj=list[-1]);#移除列表中最后一个元素，或指定位置的。
  >   list.remove(obj);#移除第一个找到的该元素
  >   list.reverse();#反向列表
  >   list.sort([func]);#排序
  >   ```

- 元组

  >元组类似列表，但是==不能修改元素==。
  >
  >格式`( )`，用`,`分隔。
  >
  >```python
  >tup = ('adb',12,list);
  >tup2 = ();#空元组
  >tup3 = (2,);#元组只有一个元素，则必须有个逗号,
  >```
  >
  >**访问元组，类似列表和字符串的查找**，元组不能修改，但是可以==串接==。
  >
  >```python
  >tup1 = (1,2,2);
  >tup2 = ('ab','cd','ddf');
  >tup3 = tup1 + tup2;#元组串接。
  >```
  >
  >***元组元素不能删除，但是==元组可以被删除==***
  >
  >```python
  >tup = (1,2,2);
  >del tup;
  >```
  >
  >`任意无符号的对象，以,分隔，默认为元组`
  >
  >- 元组的一些方法和函数，类似列表
  >
  > ```python
  > tuple(seq);#列表转化为元组
  > ```

- 字典Dictionary

  > 字典类似于java中的map集合。使用键值对`key-value`，格式：=={key1:value1,key2:value2}==
  >
  > - **key值唯一**
  >
  > - **value值可以任何类型，key值必是不可变类型，如字符串、数字和元组。**
  >
  > - 字典内，所有的key不需要都为统一类型
  >
  >   ```python
  >   dict = {"abc":'adb','def':23,55:'adb'}
  >   print ("dict[55]");#根据key值，若是没有，会报错。
  >   ```
  >
  > - 字典元素的修改，删除
  >
  >   ```python
  >   del dict[key];#删除指定元素
  >   dict.clear();#清空字典
  >   del dict;#删除字典
  >   ```
  >
  > - 字典的函数&方法
  >
  >   ```python
  >   cmp(dict1,dict2);#比较
  >   len(dict);#计数
  >   str(dict);#字符输出字典元素
  >   type(variable);#变量的类型
  >   
  >   radiansdict.clear();#清空字典
  >   radiansdict.copy();#字典浅复制
  >   radiansdict.get(key,default=None);#获取值，若无，返回默认值。
  >   radiansdict.has_key(key);#查询是否包含指定key值
  >   radiansdict.items();#遍历显示字典元素数组
  >   radiansdict.keys();#列表显示所有key
  >   radiansdict.setdefault(key,default=None);#类似get，若key不存在，则添加。
  >   radiansdict.update(dict2);#将dict2更新到dict中。
  >   radiansdict.values();#返回所有value值。
  >   ```

- python日期时间

  > 类似其他编程语言，时间基于1970年1月1日。Unix和windows支持到2038年？
  >
  > - time
  >
  >   ```python
  >   time.time();#获取时间戳
  >   time.localtime(time.time());#获得时间的元组
  >   time.asctime(time.localtime(time.time()));#格式化时间
  >   time.strftime(format[,t]);#自定义字符格式化时间
  >   # 格式化成2016-03-20 11:45:39形式
  >   print time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()) 
  >
  >   # 格式化成Sat Mar 28 22:24:24 2016形式
  >   print time.strftime("%a %b %d %H:%M:%S %Y", time.localtime()) 
  >
  >   # 将格式字符串转换为时间戳
  >   a = "Sat Mar 28 22:24:24 2016"
  >   print time.mktime(time.strptime(a,"%a %b %d %H:%M:%S %Y"));
  >   ```
  >
  > - calendar
  >
  >   ==0-6表示一周，0表示周一。==
  >
  >   ```python
  >   calendar.month(2016,10);#获取制定月份的日历。
  >   ```
  >
  >   **函数和方法不在赘述**
  >
  >   datetime、pytz、dateutil模块也是处理时间相关。

- 函数

  > 类似其他编程语言的函数&方法定义，python函数格式：
  >
  > ```python
  > def functionname( parameters ):
  >  "函数文档"
  >  function_suite
  >  return [expression]
  > #def 关键字，声明函数
  > #functionname 函数名
  > #(parameters) 参数，多个参数可以，分隔？还是多个括号
  > #可写声明文档，
  > #返回值，可以是None。
  > ```
  >
  > **按值传递与按引用传递**
  >
  > Python中所有参数的传递，都是==引用传递==，一处修改，原始值则变。
  >
  > - 函数的参数：
  >
  >   `必备参数`、`关键字参数`、`默认参数`、`不定长参数`
  >
  >   ```python
  >   #必备参数，必须传入的形式，否则报错
  >   def printStr(str):
  >       ...
  >   	return;    
  >   #关键字参数，输入参数的格式顺序，不必和声明的顺序一致。
  >   def printInfo(name,age):
  >       print("name:",name);
  >       print("age:",age)
  >       return;
  >   printInfo(age = 27,name = 'jack');
  >   def printDefault(name,age=20):
  >       ......
  >       return;
  >   #此时可以使用默认参数
  >   printDefault(name = 'jack');#age 默认了。
  >
  >   #不定长参数
  >   def printLong(arg1,*args):#星号*的那个参数，为可变参数。
  >       print(arg2);
  >       for var in args:
  >           print var;
  >       return;
  >   ```
  >
  > - Python使用lambda创建匿名函数。
  >
  >   - lambda是表达式，函数比def简单
  >   - 只能在lambda表达式中构建逻辑
  >   - lambda仅能访问自有参数。
  >   - lambda似乎只能写一行。
  >
  >   ==语法==
  >
  >   ```python
  >   lambda [arg2[,arg2,......]]:expression
  >   #示例
  >   sum = lambda arg1,arg2:arg1+arg2;
  >   
  >   #调用sum函数
  >   sum(1,2);
  >   ```
  >
  >   **return用于退出函数，无有返回值则是None。**
  >
  >   变量作用域，全局和局部，都是作用于其声明范围内。

- Python模块

  > python模块指一段逻辑或函数方法，在python中，模块也是一个对象，可以命名，引用。类似于java中的类文件？
  >
  > ```python
  > import module1[,module2,...]#导入模块，就可以使用其方法和逻辑。
  > #用类名/文件名.方法/函数,来调用。
  > from modname import name1[,name2,...]#从指定空间导入模块。
  > from modname import * #导入指定名称空间下的所有模块。
  > ```
  >
  > 寻找模块的顺序是：当前文件、path路径、系统默认路径。
  >
  > - 变量默认都是局部范围的，全局变量需要global声明。
  > - dir()函数，列表显示。
  > - globals()和locals(),reload();
  > - python中也有包，文件的概念。

- Python文件IO和File、异常

  >- 文件I/O
  >
  > `raw_input`
  >
  > `input`
  >
  > ```python
  > #raw_input 接受输入的一行
  > str = raw_input("Please input :");
  > print("输入内容为：",str);#str会接收键盘的输入内容
  > #input类似与raw_input，可以接受输入表达式
  > str = input("Please input:");
  > print("输入内容为：",str);
  > #上面可以输入：[x*5 for x in range(2,10,2)]
  > ```
  >
  >- file对象操作文件
  >
  > ```python
  > file object = open(file_name[,access_mode][,buffering]);#用open()方法打开文件
  > file.closed;# true or false
  > file.mode;
  > file.name;
  > file.softspace;#有空格符来结束，需要返回False，不需要，True。
  > file.write(string);#不会默认行尾加换行的，。
  > file.read([count]);#读取指定个数的字节数。
  >
  > tell();#可知文件内，当前位置。
  > seek(offset[,from]);#改变当前位置
  >
  > #python 的os模块有删除、重命名文件的方法
  > os.rename(old_name,new_name);
  > os.remove("test.txt");#删除文件
  > os.mkdir("abc");os.chdir("new");os.getcwd();#显示当前目录。
  > os.rmdir();
  > ```
  >
  >- **异常处理**
  >
  > try/except语句
  >
  > ```python
  > try:
  >     <statements>#运行代码
  > except <exception>:
  >     <e1>#try到异常name
  > except <exception>,<data>:
  >     <e2>#try到异常，并有数据
  > else:
  >     <statements2>#无异常时候运行
  > #类似java，python中可以不写exception的name，就会捕获所有异常。
  > #也可以
  > except(e1,e2,...):
  >     ...
  > #类似java 有try finally
  > try:
  >     <>
  > finally:
  >     <>#总会执行的语句。
  > ```
  >
  > ==raise==语句自动触发异常
  >
  > ```python
  > raise [Exception[,args[,traceback]]]
  > def functionName( level ):
  >     if level < 1:
  >         raise Exception("Invalid level!", level)
  >         # 触发异常后，后面的代码就不会再执行
  >
  > try:
  >    # 正常逻辑
  > except "Invalid level!":#对应自定义的异常名，如上。
  >    # 触发自定义异常    
  > else:
  >    # 其余代码
  >
  > class MyError(RuntimeError):
  >     def init(self,arg):
  >         self.args = arg
  >
  > #自己触发异常
  > try:
  >     raise MyError("hao xiang shu ru cuo le ")
  > except Networkerror,e:
  >     print e.args
  > ```


## 四、Python高级教程

- 面向对象

  > Python是一种面向对象的解释型语言，类似于其他面向对象的编程语言，需要熟悉一下面向对象的名词概念：
  >
  > - `类(class)`：用于描述具有相同属性和方法的对象的集合。描述一类事物，对象是类的实例。
  > - `类变量`：类中公开化的变量，作用于类的范围内，区别于实例变量。
  > - `数据成员`：类变量或实例变量，用于处理实例对象相关的数据。
  > - `方法重写`：重写继承自父类的方法，实现自有的逻辑。override。
  > - `实例变量`：定义在方法中的变量，作用于方法内。
  > - `继承`：即一个派生类继承基类的字段和方法。
  > - `实例化`：创建一个类的实例，类的具体对象。
  > - `方法`：类中定义的函数。
  > - `对象`：根据类定义的数据结构构建的一个实例，包含类的数据成员和方法。
  >
  > 1. 创建类
  >
  >    ```python
  >    class ClassName:#定义类的关键字class
  >        '类的描述信息' #帮助文档
  >        class_suite #类题
  >    #例如：
  >    class Employee:
  >       '所有员工的基类'
  >       empCount = 0
  >    
  >       def __init__(self, name, salary):#构造函数
  >          self.name = name
  >          self.salary = salary
  >          Employee.empCount += 1
  >    
  >       def displayCount(self):#方法函数
  >         print "Total Employee %d" % Employee.empCount
  >    
  >       def displayEmployee(self):
  >          print "Name : ", self.name,  ", Salary: ", self.salary
  >    #实例化对象
  >    objectName = ClassName(...);#根据类的构造函数创建对象。
  >    #访问方法属性，用object.method();
  >    objectName.displayCount();
  >    #可以添加、删除、修改类的属性
  >    objectName.age = 20;# add attribution
  >    objectName.age =26;#modify attribution
  >    del objectName.age # delete attribution
  >    ```
  >
  >
  >    **Python有专门的函数，处理类的属性和方法**
  >    getattr(obj,name[,default]);
  >    setattr(obj,name,value);
  >    hasattr(obj,name);
  >    delattr(obj,name);
  >    ```
  > 
  > 2. Python内置属性
  > 
  >    ```python
  >    #python attribution，用ClassName.function调用。
  >    __dict__:类的属性
  >    __doc__:类的文档字符串
  >    __name__:类名
  >    __module__:类定义所在的模块
  >    __bases__类的所有父类构成元素
  >    ```
  >
  > 3. 类似java，python使用内置引用计数器，处理垃圾回收。
  >
  > 4. Python的继承：
  >
  >    - 格式：class SubClassName [ParentClass1[,ParentClass2,...]]:
  >
  >    - Python类的继承==支持多继承==
  >
  >    - 派生类继承基类，构造函数不会被自动调用，需要专门调用。
  >
  >    - 调用基类方法时候，需要用BaseClassName.而且需要self参数。区别于本类内的函数方法。
  >
  >    - 同类函数名，则优先调用本类中的函数方法，若无，再去基类中寻找。
  >
  >      ```python
  >      issbuclass();#判断是否是另一个类的派生类，issubclass(sub,super);
  >      isinstance(obj,class);#判断一个对象，是不是类的实例。
  >      ```
  >
  > 5. 方法重写与重载
  >
  >    - 重写基类的方法，用于实现自己的逻辑。
  >
  >    - 基础重载：
  >
  >      ```python
  >      #函数方法，前后两个下划线__init__
  >      __init__(self[,args])#构造函数
  >      __del__(self)#删除对象
  >      __repr__(self)#转化为编译器格式
  >      __str__(self)#转化为可阅读模式
  >      __cmp__(self,x)#对象比较
  >      ```
  >
  >    - 运算符重载
  >
  >      ```python
  >      class Vector:
  >         def __init__(self, a, b):
  >            self.a = a
  >            self.b = b
  >   
  >         def __str__(self):
  >            return 'Vector (%d, %d)' % (self.a, self.b)
  >   
  >         def __add__(self,other):
  >            return Vector(self.a + other.a, self.b + other.b)
  >   
  >      v1 = Vector(2,10)
  >      v2 = Vector(5,-2)
  >      print v1 + v2#运算符的重载
  >      #输出结果
  >      Vector(7,8)
  >      ```
  >
  > 6. **类的属性和方法**
  >
  >    - 类的私有属性
  >
  >      __private_attrs：两个下划线开头，仅能在类内部使用。self.\_\_private_attrs。
  >
  >    - 类的方法
  >
  >      关键字`def`定义类的方法，类似定义函数。但是类的方法必须包含参数`self`且为第一参数，私有方法：__private_method，内部调用self.\_\_private_method
  >
  >      ```python
  >      def method(self[,args]):
  >          .....
  >
  >      ```
  >
  >    - Python不允许实例化的类访问私有数据，可用obj._classname\_attrName访问属性。

- 正则表达式

  > Python的re模块包含了全部的正则表达式。
  >
  > compile函数用于构建正则表达式。
  >
  > 1. re.match
  >
  >    从字符串起始位置匹配，起始不成功匹配，返回None。
  >
  >    ```python
  >    #pattern 正则表达式，string 匹配字符串，flags标志位，如区分大小写，多行匹配等。
  >    re.match(pattern,string,flags=0);
  >    group(num= 0)#匹配整个表达式字符串，group可以依次输入多个组号。
  >    groups();#返回包含所有小组字符串的元组。
  >    ```
  >
  >    示例：
  >
  >    ```python
  >    import re
  >    print(re.match('www', 'www.runoob.com').span())  # 在起始位置匹配
  >    print(re.match('com', 'www.runoob.com'))         # 不在起始位置匹配
  >
  >    #输出结果
  >    (0,3)
  >    None
  >    ```
  >
  >    ```python
  >    #!/usr/bin/python
  >    import re
  >
  >    line = "Cats are smarter than dogs"
  >
  >    matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)
  >
  >    if matchObj:
  >       print "matchObj.group() : ", matchObj.group()
  >       print "matchObj.group(1) : ", matchObj.group(1)
  >       print "matchObj.group(2) : ", matchObj.group(2)
  >    else:
  >       print "No match!!"
  >
  >    #输出结果
  >    matchObj.group() :  Cats are smarter than dogs
  >    matchObj.group(1) :  Cats
  >    matchObj.group(2) :  smarter
  >    ```
  >
  > 2. re.search方法
  >
  >    扫描整个字符串，并返回第一个成功的匹配。
  >
  >    ```python
  >    re.search(pattern,string,flags=0);
  >    ```
  >
  >    示例：
  >
  >    ```python
  >    #!/usr/bin/python
  >    # -*- coding: UTF-8 -*- 
  >
  >    import re
  >    print(re.search('www', 'www.runoob.com').span())  # 在起始位置匹配
  >    print(re.search('com', 'www.runoob.com').span())         # 不在起始位置匹配
  >
  >    #输出结果
  >    (0,3)
  >    (11,14)
  >    ```
  >
  >    ```python
  >    #!/usr/bin/python
  >    import re
  >
  >    line = "Cats are smarter than dogs";
  >
  >    searchObj = re.search( r'(.*) are (.*?) .*', line, re.M|re.I)
  >
  >    if searchObj:
  >       print "searchObj.group() : ", searchObj.group()
  >       print "searchObj.group(1) : ", searchObj.group(1)
  >       print "searchObj.group(2) : ", searchObj.group(2)
  >    else:
  >       print "Nothing found!!"
  >
  >    #输出结果
  >    searchObj.group() :  Cats are smarter than dogs
  >    searchObj.group(1) :  Cats
  >    searchObj.group(2) :  smarter
  >    ```
  >
  > 3. re.match & re.search
  >
  >    `re.match`匹配起始，不成功则None。
  >
  >    `re.search`匹配全部。
  >
  >    ```python
  >    #!/usr/bin/python
  >    import re
  >
  >    line = "Cats are smarter than dogs";
  >
  >    matchObj = re.match( r'dogs', line, re.M|re.I)
  >    if matchObj:
  >       print "match --> matchObj.group() : ", matchObj.group()
  >    else:
  >       print "No match!!"
  >
  >    matchObj = re.search( r'dogs', line, re.M|re.I)
  >    if matchObj:
  >       print "search --> matchObj.group() : ", matchObj.group()
  >    else:
  >       print "No match!!"
  >
  >    #输出结果
  >    No match!!
  >    serach --> matchObj.group() : dogs
  >    ```
  >
  > 4. re.sub检索和替换
  >
  >    ```python
  >    re.sub(pattern,rep1,string,max = 0);#count >=0,匹配次数。
  >    ```
  >
  >    示例：
  >
  >    ```python
  >    #!/usr/bin/python
  >    import re
  >
  >    phone = "2004-959-559 # This is Phone Number"
  >
  >    # Delete Python-style comments
  >    num = re.sub(r'#.*$', "", phone)
  >    print "Phone Num : ", num
  >
  >    # Remove anything other than digits
  >    num = re.sub(r'\D', "", phone)    
  >    print "Phone Num : ", num
  >
  >    #输出结果
  >    Phone Num :  2004-959-559
  >    Phone Num :  2004959559
  >    ```
  >
  > 5. 正则表达式的修饰符-可选标志
  >
  >    *标志修饰符控制匹配模式，多个标识符可以用按位OR`|`指定*
  >
  >    | 修饰符 | 描绘                                        |
  >    | ------ | ------------------------------------------- |
  >    | re.l   | 匹配不分大小写                              |
  >    | re.L   | 本地化识别(local-aware)匹配                 |
  >    | re.M   | 多行匹配，影响`^`和`$`                      |
  >    | re.S   | 使`.`匹配包括行在内的所有字符               |
  >    | re.U   | Unicode解析字符，影响`\w`、`\W`、`\b`、`\B` |
  >    | re.X   | 灵活格式                                    |
  >
  > 6. 正则表达式模式
  >
  >    - `字母和数字`表达自身。
  >    - 多数字母和数字前加`\`会转义
  >    - 标点符号均是特殊意思，除非转义。
  >    - `\`是转义符
  >
  >    ```python
  >    ^	匹配字符串的开头
  >    $	匹配字符串的末尾。
  >    .	匹配任意字符，除了换行符\n，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符。
  >    [...]	用来表示一组字符,单独列出：[amk] 匹配 'a'，'m'或'k'
  >    [^...]	不在[]中的字符：[^abc] 匹配除了a,b,c之外的字符。
  >    re*	匹配0个或多个的表达式。
  >    re+	匹配1个或多个的表达式。
  >    re?	匹配0个或1个由前面的正则表达式定义的片段，非贪婪方式
  >    re{ n}	
  >    re{ n,}	精确匹配n个前面表达式。
  >    re{ n, m}	匹配 n 到 m 次由前面的正则表达式定义的片段，贪婪方式
  >    a| b	匹配a或b
  >    (re)	G匹配括号内的表达式，也表示一个组
  >    (?imx)	正则表达式包含三种可选标志：i, m, 或 x 。只影响括号中的区域。
  >    (?-imx)	正则表达式关闭 i, m, 或 x 可选标志。只影响括号中的区域。
  >    (?: re)	类似 (...), 但是不表示一个组
  >    (?imx: re)	在括号中使用i, m, 或 x 可选标志
  >    (?-imx: re)	在括号中不使用i, m, 或 x 可选标志
  >    (?#...)	注释.
  >    (?= re)	前向肯定界定符。如果所含正则表达式，以 ... 表示，在当前位置成功匹配时成功，否则失败。但一旦所含表达式已经尝试，匹配引擎根本没有提高；模式的剩余部分还要尝试界定符的右边。
  >    (?! re)	前向否定界定符。与肯定界定符相反；当所含表达式不能在字符串当前位置匹配时成功
  >    (?> re)	匹配的独立模式，省去回溯。
  >    \w	匹配字母数字及下划线
  >    \W	匹配非字母数字及下划线
  >    \s	匹配任意空白字符，等价于 [\t\n\r\f].
  >    \S	匹配任意非空字符
  >    \d	匹配任意数字，等价于 [0-9].
  >    \D	匹配任意非数字
  >    \A	匹配字符串开始
  >    \Z	匹配字符串结束，如果是存在换行，只匹配到换行前的结束字符串。c
  >    \z	匹配字符串结束
  >    \G	匹配最后匹配完成的位置。
  >    \b	匹配一个单词边界，也就是指单词和空格间的位置。例如， 'er\b' 可以匹配"never" 中的 'er'，但不能匹配 "verb" 中的 'er'。
  >    \B	匹配非单词边界。'er\B' 能匹配 "verb" 中的 'er'，但不能匹配 "never" 中的 'er'。
  >    \n, \t, 等.	匹配一个换行符。匹配一个制表符。等
  >    \1...\9	匹配第n个分组的子表达式。
  >    \10	匹配第n个分组的子表达式，如果它经匹配。否则指的是八进制字符码的表达式。
  >    ```

- CGI通用网关接口

  > CGI程序，运行在服务器，python脚本、perl脚本、shell脚本、C/C++程序等。
  >
  > Web服务器需支持cgi，通常在/cgi-bin/ /var/www/cgi-bin/
  >
  > cgi文件`.cgi`或者各自语言的源文件。
  >
  > ```python
  > #!/usr/bin/python
  > # -*- coding: UTF-8 -*-
  > 
  > print "Content-type:text/html"	#向浏览器显示的内容类型
  > print                               # 空行，告诉服务器结束头部
  > print '<html>'
  > print '<head>'
  > print '<meta charset="utf-8">'
  > print '<title>Hello Word - 我的第一个 CGI 程序！</title>'
  > print '</head>'
  > print '<body>'
  > print '<h2>Hello Word! 我是来自菜鸟教程的第一CGI程序</h2>'
  > print '</body>'
  > print '</html>'
  > ```
  >
  > 如上保存为hello.py，修改权限755。放置于cgi-bin/目录下，即可在浏览器中访问。
  >
  > - CGI程序中HTTP头部常用信息
  >
  >   | 头                 | 描述                       |
  >   | ------------------ | -------------------------- |
  >   | Content-type       | 请求与实体对应的MIME信息。 |
  >   | Expires:Date       | 响应过期的日期和时间       |
  >   | Location:URL       | 重定向                     |
  >   | Last-modified:Date | 请求资源的最后修改时间     |
  >   | Content-length:N   | 请求内容长度               |
  >   | Set-Cookie:String  | 设置Http Cookie            |
  >
  > - CGI环境变量
  >
  >   所有CGI程序都接收以下环境变量
  >
  >   | 变量名          | 描述                                                    |
  >   | --------------- | ------------------------------------------------------- |
  >   | CONTENT_TYPE    | MIME类型                                                |
  >   | CONTENT_LENGTH  | 有效数据字节数                                          |
  >   | HTTP_COOKIE     | 客户机内cookie内容                                      |
  >   | HTTP_USER_AGENT | 客户及版本，浏览器信息等。                              |
  >   | PATH_INFO       | 表示CGI程序名之后的其他路径信息                         |
  >   | QUERY_STRING    | GET请求时，代表所传递的信息。                           |
  >   | REMOTE_ADDR     | 客户机ip地址                                            |
  >   | REMOTE_HOST     | 客户机主机名                                            |
  >   | REQUEST_METHOD  | 提供脚本被调用的方法，http/1.0协议，仅GET和POST有意义。 |
  >   | SCRIPT_FILENAME | CGI脚本完整路径                                         |
  >   | SERVER_NAME     | CGI脚本完整名称                                         |
  >   | SERVER_SOFTWARE | 调用CGI程序的http服务器的名称，版本信息。               |
  >
  >   ---
  >
  > ```python
  >       #输出所有cgi环境变量信息
  >       import os
  >       print "Content-type: text/html"
  >       print
  >       print "<meta charset=\"utf-8\">"
  >       print "<b>环境变量</b><br>"
  >       print "<ul>"
  >       for key in os.environ.keys():
  >           print "<li><span style='color:green'>%30s </span>:%s</li>" % (key,os.environ[key])
  >       print "</ul>"
  > ```
  >
  > - GET&POST
  >
  >   浏览器请求服务器的两中主要方式
  >
  >   - GET方法
  >
  >     ```python
  >     http://www.test.com/cgi-bin/hello.py?key1=value1&key2=value2
  >     #GET方法相关注释
  >     '''
  >     请求可被缓存，请求保存在浏览器历史纪录，可被收藏书签，请求不应在处理敏感数据时使用，请求有长度限制，请求只应用于取回数据。
  >     '''
  >     ```
  >
  >   - POST方法
  >
  >     post方法较为安全可靠。
  >
  > - CGI中使用cookie
  >
  >   ```python
  >   Set-cookie:name=name;expires=date;path=path;domain=domain;secure
  >   ```
  >
  >   **注释：**
  >
  >   `name=name`: 需要设置cookie的值(name不能使用";"和","号),有多个name值时用 ";" 分隔，例如：name1=name1;name2=name2;name3=name3。
  >   `expires=date`: cookie的有效期限,格式： expires="Wdy,DD-Mon-YYYY HH:MM:SS"
  >   `path=path`: 设置cookie支持的路径,如果path是一个路径，则cookie对这个目录下的所有文件及子目录生效，例如： path="/cgi-bin/"，如果path是一个文件，则cookie指对这个文件生效，例如：path="/cgi-bin/cookie.cgi"。
  >   `domain=domain`: 对cookie生效的域名，例如：domain="www.runoob.com"
  >   `secure`: 如果给出此标志，表示cookie只能通过SSL协议的https服务器来传递。
  >   cookie的接收是通过设置环境变量HTTP_COOKIE来实现的，CGI程序可以通过检索该变量获取cookie信息。
  >
  >   - cookie检索
  >
  >     ```python
  >     #cookie信息存储在CGI环境变量HTTP_COOKIE中，
  >     key1=value1;key=value2;......
  >     ```
  >
  >   - 文件上传
  >
  >     ```html
  >     <!DOCTYPE html>
  >     <html>
  >     <head>
  >     <meta charset="utf-8">
  >     <title>菜鸟教程(runoob.com)</title>
  >     </head>
  >     <body>
  >      <form enctype="multipart/form-data" 
  >                          action="/cgi-bin/save_file.py" method="post">
  >        <p>选中文件: <input type="file" name="filename" /></p>
  >        <p><input type="submit" value="上传" /></p>
  >        </form>
  >     </body>
  >     </html>
  >     ```
  >
  >     上面的html形成一个界面，可以选择上传文件，调用save_file.py脚本
  >
  >     ```python
  >     #!/usr/bin/python
  >     # -*- coding: UTF-8 -*-
  >     
  >     import cgi, os
  >     import cgitb; cgitb.enable()
  >     
  >     form = cgi.FieldStorage()
  >     
  >     # 获取文件名
  >     fileitem = form['filename']
  >     
  >     # 检测文件是否上传
  >     if fileitem.filename:
  >        # 设置文件路径 
  >        fn = os.path.basename(fileitem.filename)
  >        open('/tmp/' + fn, 'wb').write(fileitem.file.read())
  >     
  >        message = '文件 "' + fn + '" 上传成功'
  >     
  >     else:
  >        message = '文件没有上传'
  >     
  >     print """\
  >     Content-Type: text/html\n
  >     <html>
  >     <head>
  >     <meta charset="utf-8">
  >     <title>菜鸟教程(runoob.com)</title>
  >     </head>
  >     <body>
  >        <p>%s</p>
  >     </body>
  >     </html>
  >     """ % (message,)
  >     ```
  >
  >     **记得文件的权限设置。**
  >
  >     ```python
  >     #!/usr/bin/python
  >     # -*- coding: UTF-8 -*-
  >     
  >     # HTTP 头部
  >     print "Content-Disposition: attachment; filename=\"foo.txt\"";
  >     print
  >     # 打开文件
  >     fo = open("foo.txt", "rb")
  >     
  >     str = fo.read();
  >     print str
  >     
  >     # 关闭文件
  >     fo.close()
  >     ```
  >
  >     **以上为文件下载**

- Python操作mysql数据库

  > python支持多种数据库，有对应的模块。DB-API使用流程
  >
  > 1. 引入api模块。
  >
  > 2. 获取数据库链接。
  >
  > 3. 执行sql语句和存储。
  >
  > 4. 关闭连接。
  >
  >    ```python
  >    #!/usr/bin/python
  >    # -*- coding: UTF-8 -*-
  >    
  >    import MySQLdb
  >    
  >    # 打开数据库连接
  >    db = MySQLdb.connect("localhost","testuser","test123","TESTDB" )
  >    
  >    # 使用cursor()方法获取操作游标 
  >    cursor = db.cursor()
  >    
  >    # 使用execute方法执行SQL语句
  >    cursor.execute("SELECT VERSION()")
  >    
  >    # 使用 fetchone() 方法获取一条数据库。
  >    data = cursor.fetchone()
  >    
  >    print "Database version : %s " % data
  >    
  >    # 关闭数据库连接
  >    db.close()
  >    ```
  >
  > - python数据库mysql，需要mysqldb
  >
  >   ```python
  >   fetchone();#获取下一个查询结果集
  >   fetchall();#接收全部返回结果行
  >   rowcount;#只读属性，返回执行execute()后影响的行数
  >   ```
  >
  > - 事务
  >
  >   事务的四个属性：
  >
  >   - 原子性（atomicity）。一个事务是一个不可分割的工作单位，事务中包括的诸操作要么都做，要么都不做。
  >   - 一致性（consistency）。事务必须是使数据库从一个一致性状态变到另一个一致性状态。一致性与原子性是密切相关的。
  >   - 隔离性（isolation）。一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。
  >   - 持久性（durability）。持续性也称永久性（permanence），指一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响。
  >
  >   ***Python DB API 2.0 的事务提供了两个方法 commit 或 rollback。***

- Python网络编程

  > python提供两个级别的网络服务：
  >
  > 1、低级别支持基本Socket
  >
  > 2、高级别SocketServer
  >
  > ==Socket==套接字用于程序在主机间或者进程间通讯。
  >
  > ```python
  > #pyton中socket函数
  > socket.socket([family[,type[,proto]]])
  > # family 套接字家族，可用AF_UNIX或AF_INET
  > #type 类型，面向连接与否，SOCK_STREAM和SOCK_DGRAM
  > #proto 默认为0
  > ```
  >
  > **Python中socket函数可以参照api文档，此处不在赘述。**
  >
  > 示例：
  >
  > ```python
  > #服务端
  > #!/usr/bin/python
  > # -*- coding: UTF-8 -*-
  > # 文件名：server.py
  > 
  > import socket               # 导入 socket 模块
  > 
  > s = socket.socket()         # 创建 socket 对象
  > host = socket.gethostname() # 获取本地主机名
  > port = 12345                # 设置端口
  > s.bind((host, port))        # 绑定端口
  > 
  > s.listen(5)                 # 等待客户端连接
  > while True:
  >  c, addr = s.accept()     # 建立客户端连接。
  >  print '连接地址：', addr
  >  c.send('欢迎访问菜鸟教程！')
  >  c.close()                # 关闭连接
  > 
  > 
  > #客户端
  > #!/usr/bin/python
  > # -*- coding: UTF-8 -*-
  > # 文件名：client.py
  > 
  > import socket               # 导入 socket 模块
  > 
  > s = socket.socket()         # 创建 socket 对象
  > host = socket.gethostname() # 获取本地主机名
  > port = 12345                # 设置端口好
  > 
  > s.connect((host, port))
  > print s.recv(1024)
  > s.close()  
  > ```
  >
  > **Python Internet模块**
  >
  > | 协议   | 功能用处           | 端口号 | Python模块                 |
  > | ------ | ------------------ | ------ | -------------------------- |
  > | HTTP   | 网页访问           | 80     | httplib、urllib、xmlrpclib |
  > | NNTP   | 阅读、张贴新闻文章 | 119    | nntplib                    |
  > | FTP    | 文件传输           | 20     | ftplib、urllib             |
  > | SMTP   | 发送邮件           | 25     | smtplib                    |
  > | POP3   | 接收邮件           | 110    | poplib                     |
  > | IMAP4  | 获取邮件           | 143    | imaplib                    |
  > | Telnet | 命令行             | 23     | telnetlib                  |
  > | Gopher | 信息查找           | 70     | gopherlib、urllib          |
  >
  > - smtp邮件发送
  >
  >   语法格式：
  >
  >   ```python
  >   import smtplib
  >   #创建对象
  >   smtpObj = smtplib.SMTP([host[,port[,local_hostname]]])
  >   #发送邮件
  >   SMTP.sendmail(from_addr,to_addr,msg[,mail_options,rcpt_options])
  >   ```
  >
  >   示例：
  >
  >   ```python
  >   #!/usr/bin/python
  >   # -*- coding: UTF-8 -*-
  >   
  >   import smtplib
  >   from email.mime.text import MIMEText
  >   from email.header import Header
  >   
  >   sender = 'server@server.com'
  >   receivers = ['receiver@receiver.com']  # 接收邮件，可设置为你的QQ邮箱或者其他邮箱
  >   
  >   # 三个参数：第一个为文本内容，第二个 plain 设置文本格式，可以是html，text等。第三个 utf-8 设置编码
  >   message = MIMEText('Python 邮件发送测试...', 'plain', 'utf-8')
  >   message['From'] = Header("菜鸟教程", 'utf-8')
  >   message['To'] =  Header("测试", 'utf-8')
  >   
  >   subject = 'Python SMTP 邮件测试'
  >   message['Subject'] = Header(subject, 'utf-8')
  >   ```
  >
  >
  >   try:
  >       smtpObj = smtplib.SMTP('localhost')
  >       smtpObj.sendmail(sender, receivers, message.as_string())
  >       print "邮件发送成功"
  >   except smtplib.SMTPException:
  >       print "Error: 无法发送邮件"
  >   ```
  > 
  >   **使用第三方SMTP服务**
  > 
  >   ```python
  >   # 第三方 SMTP 服务
  >   mail_host="smtp.XXX.com"  #设置服务器
  >   mail_user="XXXX"    #用户名
  >   mail_pass="XXXXXX"   #密码
  > 
  >   sender = 'from@runoob.com'
  >   receivers = ['429240967@qq.com']  # 接收邮件，可设置为你的QQ邮箱或者其他邮箱
  > 
  >   message = MIMEText('Python 邮件发送测试...', 'plain', 'utf-8')
  >   message['From'] = Header("菜鸟教程", 'utf-8')
  >   message['To'] =  Header("测试", 'utf-8')
  > 
  >   subject = 'Python SMTP 邮件测试'
  >   message['Subject'] = Header(subject, 'utf-8')
  > 
  > 
  >   try:
  >       smtpObj = smtplib.SMTP() 
  >       smtpObj.connect(mail_host, 25)    # 25 为 SMTP 端口号
  >       smtpObj.login(mail_user,mail_pass)  
  >       smtpObj.sendmail(sender, receivers, message.as_string())
  >       print "邮件发送成功"
  >   except smtplib.SMTPException:
  >       print "Error: 无法发送邮件"
  >   ```
  >
  >   创建带附件的邮件发送
  >
  > ```python
  >   #!/usr/bin/python
  >   # -*- coding: UTF-8 -*-
  > 
  >   import smtplib
  >   from email.mime.text import MIMEText
  >   from email.mime.multipart import MIMEMultipart
  >   from email.header import Header
  > 
  >   sender = 'from@runoob.com'
  >   receivers = ['429240967@qq.com']  # 接收邮件，可设置为你的QQ邮箱或者其他邮箱
  > 
  >   #创建一个带附件的实例
  >   message = MIMEMultipart()
  >   message['From'] = Header("菜鸟教程", 'utf-8')
  >   message['To'] =  Header("测试", 'utf-8')
  >   subject = 'Python SMTP 邮件测试'
  >   message['Subject'] = Header(subject, 'utf-8')
  > 
  >   #邮件正文内容
  >   message.attach(MIMEText('这是菜鸟教程Python 邮件发送测试……', 'plain', 'utf-8'))
  > 
  >   # 构造附件1，传送当前目录下的 test.txt 文件
  >   att1 = MIMEText(open('test.txt', 'rb').read(), 'base64', 'utf-8')
  >   att1["Content-Type"] = 'application/octet-stream'
  >   # 这里的filename可以任意写，写什么名字，邮件中显示什么名字
  >   att1["Content-Disposition"] = 'attachment; filename="test.txt"'
  >   message.attach(att1)
  > 
  >   # 构造附件2，传送当前目录下的 runoob.txt 文件
  >   att2 = MIMEText(open('runoob.txt', 'rb').read(), 'base64', 'utf-8')
  >   att2["Content-Type"] = 'application/octet-stream'
  >   att2["Content-Disposition"] = 'attachment; filename="runoob.txt"'
  >   message.attach(att2)
  > 
  >   try:
  >       smtpObj = smtplib.SMTP('localhost')
  >       smtpObj.sendmail(sender, receivers, message.as_string())
  >       print "邮件发送成功"
  >   except smtplib.SMTPException:
  >       print "Error: 无法发送邮件"
  > ```
  >
  > - **Python多线程**
  >
  >   关于多线程需要注意两点：
  >
  >   - 线程可以被中断(抢占)
  >   - 其他线程运行时，线程可以休眠(退让)
  >
  >   **python使用线程两种方式**
  >
  >   1、函数
  >   ```python
  >   thread.start_new_thread(function,args[,kwargs])
  >   # 注释：function-线程函数，args-参数，必须是tuple类型。kwargs-可选参数。
  >   ```
  >   示例：
  >   ```python
  >   #!/usr/bin/python
  >   # -*- coding: UTF-8 -*-
  >
  >   import thread
  >   import time
  >
  >   # 为线程定义一个函数
  >   def print_time( threadName, delay):
  >      count = 0
  >      while count < 5:
  >         time.sleep(delay)
  >         count += 1
  >         print "%s: %s" % ( threadName, time.ctime(time.time()) )
  >
  >   # 创建两个线程
  >   try:
  >      thread.start_new_thread( print_time, ("Thread-1", 2, ) )
  >      thread.start_new_thread( print_time, ("Thread-2", 4, ) )
  >   except:
  >      print "Error: unable to start thread"
  >
  >   while 1:
  >      pass
  >   ```
  >
  >   *线程结束依靠线程函数，或者手动`thread.exit()`,`抛异常`。*
  >
  > - ***线程模块***
  >
  >   1、`thread`和`threading`两个标准库
  >
  >   ```python
  >   threading.currentThread();
  >   threading.enumerate();
  >   threading.activeCount();
  >   run();start();join([time]);isAlive();getName();setName();
  >   ```
  >
  >   2、使用Threading模块创建线程
  >
  >   ```python
  >   #继承threading.Thread然后重写__init__方法和run方法
  >   #!/usr/bin/python
  >   # -*- coding: UTF-8 -*-
  >
  >   import threading
  >   import time
  >
  >   exitFlag = 0
  >
  >   class myThread (threading.Thread):   #继承父类threading.Thread
  >       def __init__(self, threadID, name, counter):
  >           threading.Thread.__init__(self)
  >           self.threadID = threadID
  >           self.name = name
  >           self.counter = counter
  >       def run(self):                   #把要执行的代码写到run函数里面 线程在创建后会直接运行run函数 
  >           print "Starting " + self.name
  >           print_time(self.name, self.counter, 5)
  >           print "Exiting " + self.name
  >
  >   def print_time(threadName, delay, counter):
  >       while counter:
  >           if exitFlag:
  >               thread.exit()
  >           time.sleep(delay)
  >           print "%s: %s" % (threadName, time.ctime(time.time()))
  >           counter -= 1
  >
  >   # 创建新线程
  >   thread1 = myThread(1, "Thread-1", 1)
  >   thread2 = myThread(2, "Thread-2", 2)
  >
  >   # 开启线程
  >   thread1.start()
  >   thread2.start()
  >
  >   print "Exiting Main Thread"
  >   ```
  >
  > - 线程同步
  >
  >   Python中线程同步使用`Lock`和`Rlock`两个对象，都含有`acquire`和`release`方法。
  >
  >   示例：
  >
  >   ```python
  >   #!/usr/bin/python
  >   # -*- coding: UTF-8 -*-
  >
  >   import threading
  >   import time
  >
  >   class myThread (threading.Thread):
  >       def __init__(self, threadID, name, counter):
  >           threading.Thread.__init__(self)
  >           self.threadID = threadID
  >           self.name = name
  >           self.counter = counter
  >       def run(self):
  >           print "Starting " + self.name
  >          # 获得锁，成功获得锁定后返回True
  >          # 可选的timeout参数不填时将一直阻塞直到获得锁定
  >          # 否则超时后将返回False
  >           threadLock.acquire()
  >           print_time(self.name, self.counter, 3)
  >           # 释放锁
  >           threadLock.release()
  >
  >   def print_time(threadName, delay, counter):
  >       while counter:
  >           time.sleep(delay)
  >           print "%s: %s" % (threadName, time.ctime(time.time()))
  >           counter -= 1
  >
  >   threadLock = threading.Lock()
  >   threads = []
  >
  >   # 创建新线程
  >   thread1 = myThread(1, "Thread-1", 1)
  >   thread2 = myThread(2, "Thread-2", 2)
  >
  >   # 开启新线程
  >   thread1.start()
  >   thread2.start()
  >
  >   # 添加线程到线程列表
  >   threads.append(thread1)
  >   threads.append(thread2)
  >
  >   # 等待所有线程完成
  >   for t in threads:
  >       t.join()
  >   print "Exiting Main Thread"
  >   ```
  >
  > - **线程优先级队列**
  >
  >   Python的Queue模块提供了FIFO和LIFO队列，Queue、LifoQueue和优先级PriorityQueue。

- Python其他特性

  > - xml解析
  >
  > xml解析通用的有`SAX`、`DOM`、python有`ElementTree`
  >
  > *一般编程中都很少用SAX和DOM解析，都有自己平台的优化解析方式。*
  >
  > **注：**因DOM需要将XML数据映射到内存中的树，一是比较慢，二是比较耗内存，而SAX流式读取XML文件，比较快，占用内存少，但需要用户实现回调函数（handler）。
  >
  > - GUI编程
  >
  >   python提供多种图形界面库`Thinter`、`wxPython`、`Jython`
  >
  >   **Tkinter**
  >
  >   ```python
  >   import Tkinter
  >   top = Tkinter.Tk()
  >   #进入消息循环
  >   top.mainloog()
  >   #如上代码执行，可产生小窗口界面了。
  >   ```
  >
  >   Tkinter包含常用的窗口控件，以及属性和方法，使用时候可差用api。
  >
  > - JSON
  >
  >   python使用Demjson，`decode`和`encode`解码编码。
  >
  >   ```python
  >   demjson.encode(self,obj,nest_level=0)
  >   ```
  >
  >   示例：(构建json)
  >
  >   ```python
  >   #!/usr/bin/python
  >   import demjson
  >   
  >   data = [{'a':1,'b':2,'c':3,'d':4,'e':5}]
  >   
  >   json = demjson.encode(data)
  >   print(json)
  >   ```
  >
  >   解析Json
  >
  >   ```python
  >   demjson.decode(self,txt)
  >   ```
  >
  >   ```python
  >   #!/usr/bin/python
  >   import demjson
  >   
  >   json = '{"a":1,"b":2,"c":3,"d":4,"e":5}';
  >   
  >   text = demjson.decode(json)
  >   print(text)
  >   ```



**初次接触Python，笔记难免简单初级，仅供自己学习只用，希望也对网友有所益处。**