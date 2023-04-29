# Shell学习笔记

[toc]

## 简介：

Shell 是一个用C语言编写的程序，它是用户使用Linux的桥梁。Shell既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

Ken Thompson的sh是第一种Unix Shell，Windows Explorer是一个典型的图形界面Shell。

## Shell教程入门

### 1、shell脚本

> shell脚本`shell script`，是一种为shell而编写的脚本程序。然而通常所说的shell却是指shell脚本，而非shell本身。

Linux系统shell种类众多，常用的有`sh`和`bash`。

### 2、shell脚本实践

shell脚本用`#!/bin/bash`或`#!/bin/sh`之类的方式，制定运行脚本的shell,`#!`是识别符号。

```sh
#!/bin/sh
echo "Hello World"
```

**运行shell脚本的两种方法：**

- 作为可执行程序

  将shell文本保存为`.sh`格式文件，赋予执行权限

  ```sh
  chmod + x ./test.sh # 赋予可执行权限
  ./test.sh #执行脚本，注意此处需要指明当前目录下的test.sh哦，不然会去path路径找的，呵呵。
  ```

- 作为解释其参数

  直接解释运行,如此就不用写`#!/bin/sh`之类的引用注释了。

  ```sh
  /bin/sh test.sh
  # 类似java、php、python脚本
  /bin/php test.php
  ```

## Shell变量

变量命名语法，不需要`$`符号，php需要。命名要求：

- 首字母必须字母`a-z或A-Z`
- 不能空格，可用`_`
- 不能标点
- 不能使用`bash`关键字

```sh
#注意，不同于其他编程语言，等号之间不要有空格。
your_variable="your,name"
```

```shell
# 使用语句给变量赋值，如下循环显示出/etc目录下的文件名
for file in 'ls /etc'
```

### 1、使用变量

使用已定义变量，只需在变量前加`$`符号即可：

```shell
your_variable='yourname'
echo $your_variable
echo ${your_variable}
```

变量名可以加`{}`来标识变量名的范围，如：

```sh
for skill in Ada Coffe Action Java; do
	echo "I am good at ${skill}Script"
done
```

*如果不加`{}`，`skill`就可能被认为`skillScript`而导致变量找不到*

已定义的变量可重新定义：

```sh
your_name="tom"
echo $your_name
your_name="john"
echo $your_name
```

### 2、只读变量

使用`readonly`命令将变量只读，则不可再改变，否则报错。

```sh
#!/bin/bash
myUrl="http://www.w3cschool.cc"
readonly myUrl
myUrl="new url" #此处就会报错，因为变量只读了。
```

### 3、删除变量

使用`unset`命令删除变量：

```sh
unset variable_name
```

==删除变量后不能再用，而`unset`不能删除只读变量==

```sh
#!/bin/sh
myUrl="http://www.google.com"
unset myUrl
echo $myUrl #此时输出就没接过了，因为变量被删除了。
```

### 4、变量类型

运行shell时，会同时存在三种变量：

- 局部变量：脚本中定义的变量，仅作用于本shell脚本内。
- 环境变量：所有程序，包括shell启动程序都能访问的环境变量。
- shell变量：shell程序的特殊变量。

### 5、shell字符串

shell常用`number`和`string`，其中`string`可以单引号、双引号或者不用引号。但是略有区别：

- 单引号

  ```sh
  str='string test'
  ```

  单引号`''`之间的字符原样输出，里面的变量也会失效。其内部不能再有单引号，哪怕转义符号都失效。

- 双引号

  ```shell
  your_name='your name'
  str="Hello ,world ,\"$your_name\"! \n"
  ```

  双引号里面可以有变量，可以有转义符号。

- 字符串拼接

  ```sh
  n1="abc"
  test="hello, "$n1" !"
  test1="hello, ${n1} !"
  echo $test $test1
  ```

- 获取字符串长度

  使用`#`标识变量长度

  ```sh
  str="abcdef"
  #输出字符串长度
  echo ${#str} 
  ```

- 获取子字符串

  ```sh
  str="abcdef"
  #下标从左至右，0开始，
  echo ${str:1:4}
  ```

- 查找子字符串

  使用\`符号

  ```sh
  str="hello world nihaome"
  #查找字符i或s的位置,反引号
  echo `expr index "$str" is`
  ```
  
- 字符串匹配

  - 从左侧开始匹配
    - `#`表示最小限度开始匹配
    - `##`最大限度的匹配
  - 从右侧开始匹配
    - `%`最小限度
    - `%%`最大限度
  
  ```shell
  # 匹配到“”中间的字符
  name=rootproject.name = "shellScript"
  #想要获取shellScript这个字符
  echo ${${name#*\"}%\"}
  # 这里是用的都是最小匹配，如果是## 或 %% 就匹配不到，因为## 会找到最后一个“之后
  ```

### 6、Shell数组

bash仅支持一维数组，可利用下标或表达式操作元素。

- 定义数组

  shell中使用`()`表示数组，元素用**空格**来分割。

  ```sh
  array=(1 2 3 4 5 6 7)
  #或者
  array=(
  a
  b
  c
  )
  #也可以单独定义,下标可以不连续，也无范围限制。
  array[0]=1
  array[1]=2
  array[3]=7
  ```

- 读取数组

  ```sh
  variable=${array[index]}
  # @符号代替index表示获取所有元素
  echo ${variable[@]}
  ```

- 获取数组长度

  类似字符串的获取

  ```sh
  #获取元素个数
  length=${#array_name[@]}
  #或者*通配符
  length=${#array_name[*]}
  #获取数组单个元素的长度
  length_n=${#array_name[n]}
  ```

### 7、Shell注释

使用`#`至于行首，表示该行注释，shell无多行注释，只能每行都`#`

```sh
#-----------------------
#我是个多行注释
#但是只能这么写
#-----------------------
```

要是多行代码需要注释，可以将定义为函数，加`{}`包裹起来，该函数不被调用，则类似于注释掉。

## Shell传递参数

在执行shell脚本时，可以向脚本传递参数，脚本内获取参数的格式：`$n`，n代表数字编号，为脚本内需要获取的参数的编号。

### 1、实例

示例向脚本传递三个参数，并输出，`$0`为执行文件名：

```shell
#!/bin/bash
#传参测试

echo "Shell 传参测试";
echo "file name: $0";
echo "first variable: $1";
echo "second variable: $2"
echo "third variable: $3";
```

通过赋予权限，或者指定执行，可实现输出：

```shell
$ chmod +x test.sh
# $符号在次表示root用户执行，下面是传入1，2，3，三个数到脚本
$ ./test.sh 1 2 3 

#输出结果：
Shell 传参测试
file name: ./test.sh
first variable: 1
second variable 2
third variable 3
```

另有几个特殊字符处理参数：

| 参数处理 | 说明                                               |
| -------- | -------------------------------------------------- |
| $#       | 传递到脚本的参数的个数                             |
| $*       | 以一个单字符串显示所有向脚本传递的参数。           |
| $$       | 脚本运行的当前进程ID号                             |
| $!       | 后台运行的最后一个进程的ID号                       |
| $@       | 类似$*，使用时许加引号，并在引号中返回每个参数。   |
| $-       | 显示shell使用的当前选选项，类似`set`命令           |
| $?       | 显示最后命令的退出状态。0 表示无错误。其他都是错。 |

```sh
#!/bin/sh

echo "Shell 传递参数实例！";
echo "第一个参数为：$1";

echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";
```

执行效果：

```shell
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
第一个参数为：1
参数个数为：3
传递的参数作为一个字符串显示：1 2 3
```

`$*`与`$@`的异同：

- 都是应用所有参数

- 不同：只有在双引号中体现。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。

  ```sh
  #!/bin/bash
  
  echo "-- \$* demo ---"
  for i in "$*";do
  	echo $i
  done
  
  echo "-- \$@ demo ---"
  for i in "$@"; do
  	echo $i
  done
  ```

  执行效果：

  ```shell
  $ chmod +x test.sh 
  $ ./test.sh 1 2 3
  -- $* 演示 ---
  1 2 3
  -- $@ 演示 ---
  1
  2
  3
  ```

## Shell数组

Bash shell仅支持一维数组，不限定大小，初始化时候不需要指定大小。下标0开始，`()`包裹，空格分割元素。

```shell
array=(a b c d)
```

- 读取数组

  格式`${array[index]}`

```shell
#!/bin/bash
my_array=(a b "c" d)

echo "first: ${my_array[0]}"

#然后执行文件，获得输出结果
#可用@或*来代替index获取所有元素
```

- 数组长度

  类似字符串的长度获取

  ```shell
  ${#array[index]}
  ```

## Shell运算符

shell支持多种运算符：

- 算数运算符
- 关系运算符
- 布尔运算符
- 字符串运算符
- 文件测试运算符

原生Bash不支持简单的数学运算，可以用`awk`和`expr`实现。

```sh
#!/bin/sh

val=`expr 2 + 2`
echo $val

#注意，expr用反引号，表达式和运算符之间必须有空格，2+2就不行。
```

### 1、算术运算符

若a = 10，b = 20

| 运算符 | 说明                         | 举例                            |
| ------ | ---------------------------- | ------------------------------- |
| +      | 加号                         | \`expr \$a + \$b\`，result = 30 |
| -      | 减号                         | \`expr \$a - \$b`，result = -10 |
| *      | 乘号                         | \`expr \$a * \$b`，result = 200 |
| /      | 除号                         | \`expr \$b / \$a`，result = 2   |
| %      | 取余                         | \`expr \$b % \$a`，result=0     |
| =      | 赋值                         | a=$b，将b的值赋给a              |
| ==     | 相等，比较数字，同则true。   | [\$a == \$b]返回false           |
| !=     | 不等，比较数字，不同的true。 | [\$a != \$b]返回true。          |

```shell
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a 等于 b"
fi
if [ $a != $b ]
then
   echo "a 不等于 b"
fi
```

执行结果：

```shell
a + b : 30
a - b : -10
a * b : 200
b / a : 2
b % a : 0
a 不等于 b
```



**注意：**条件表达式必须在`[]`之间，且必须有空格，如**[\$a==\$b]**是错的！

> `*`乘号需要在`expr`表达式内用`\`转义
>
> Mac电脑的shell的`expr`表达式：**$((表达式))**所以它的乘号`*`不用转义

### 2、关系运算符

布尔类型，关系运算符仅支持数字，除非字符串的值也是数字。示例，若a = 10 ,b = 20:

| 运算符 | 说明        |
| ------ | ----------- |
| -eq    | equal       |
| -ne    | not equal   |
| -gt    | great than  |
| -lt    | less than   |
| -ge    | great equal |
| -le    | less equal  |

### 3、布尔运算符

| 运算符 | 说明                                     |
| ------ | ---------------------------------------- |
| !      | `非`运算，表达式为true，则返回false。    |
| -o     | `或`运算，一个表达式为true，则返回true。 |
| -a     | `与`运算，两个都true，才返回true。       |

### 4、逻辑运算符

| 运算符 | 说明    |
| ------ | ------- |
| &&     | 逻辑and |
| \|\|   | 逻辑 or |

> `shell`命令多条指令一起执行的方式：
>
> - 并行，使用`;`分隔指令，使用`&`操作符将命令执行交给后台，自身则继续执行下一个指令。各指令执行互不影响。
>
>   ```shell
>   # 如此则是执行两个指令，并行执行，各自指令的成功与否，不会影响其他指令的执行。此处的演示可以看出，testDir并不会在existDoc目录下，而是在当前目录下创建。
>   cd ~/existDoc &; mkdir testDir;
>   ```
>
> - 串行
>
>   1. 结果互不干涉。如上，仅使用`;`分隔各个指令，虽然串行都会执行，但不保证执行结果，其各个指令的结果，有可能会有影响偏差；
>
>      ```shell
>      # 要cd到一个不存在的noThisDoc目录，会报错。两条指令串行，mkdir的目的路径从期待的 noThisDoc/testDir变成了 当前目录下的testDir。
>      cd ~/noThisDoc ; mkdir testDir;
>      ```
>
>   2. 成功则继续的串行方式，使用 `&&`符号
>
>      ```shell
>      # 要cd到一个不存在的doc目录，会报错。则不会执行mkdir指令。
>      cd ~/doc && mkdir testDir;
>      ```
>   
>   3. 使用`||`表示指令**或**的关系，即前面的指令如果执行成功，则后面的不必执行。若前面指令失败，则会执行后面的指令
>   
>      ```shell
>      # 如下指令，cd的目录如果存在，则直接cd后创建testDir，如果不存在则会执行创建doc目录，而后在mkdir testDir
>      cd ~/noThisDir || mkdir ~/doc ; mkdir testDir
>      ```
>

### 5、字符串运算符

| 运算符 | 说明                               |
| ------ | ---------------------------------- |
| =      | 检测两字符串是否相等               |
| !=     | 检测两字符串是否不等               |
| -z     | zero检测字符串长度是否为0，0则true |
| -n     | not zero检测字符串长度是否非0。    |
| str    | 检测字符串是否为空，不空则true。   |

### 6、文件测试运算符

用于检测类unix 文件的各种属性

| 操作符  | 说明                                               |
| ------- | -------------------------------------------------- |
| -b file | 是否是块设备文件，是则true                         |
| -c file | 是否是字符设备文件，是则true                       |
| -d file | 是否是目录，是则true                               |
| -f file | 是否是普通文件(既非目录，亦非设备文件)，若是则true |
| -g file | 是否设置了SGID位，是则true                         |
| -k file | 是否设置粘着位，是则true                           |
| -p file | 是否有名管道，是则true                             |
| -u file | 是否设置SUID，是则true                             |
| -r file | 是否可读，是则true                                 |
| -w file | 是否可写，是则true                                 |
| -x file | 是否可执行，是则true                               |
| -s file | 是否为空或大小是否大于0，非空则true                |
| -e file | 是否存在，是则true                                 |

==注意shell脚本的表达式都要在`[]`内哦==

```shell
-e 判断对象是否存在
-d 判断对象是否存在，并且为目录
-f 判断对象是否存在，并且为常规文件
-L 判断对象是否存在，并且为符号链接
-h 判断对象是否存在，并且为软链接
-s 判断对象是否存在，并且长度不为0
-r 判断对象是否存在，并且可读
-w 判断对象是否存在，并且可写
-x 判断对象是否存在，并且可执行
-O 判断对象是否存在，并且属于当前用户
-G 判断对象是否存在，并且属于当前用户组
-nt 判断file1是否比file2新  [ "/data/file1" -nt "/data/file2" ]
-ot 判断file1是否比file2旧  [ "/data/file1" -ot "/data/file2" ]
```

## Shell echo命令

类似于php的echo，shell的echo用于输出字符串，格式`echo string`

- 显示普通字符串

```shell
echo "Hello World"
#可以不带引号
echo Hello World
```

- 显示转义字符

  ```sh
  echo "\"It is a test\""
  #输出结果
  "It is a test"
  ```

- 显示变量

  `read`命令从标准输入中读取一行，并把输入行的每个字段指定给shell变量

  ```shell
  #!/bin/sh
  read name
  echo "$name It is a test"

  #运行效果：如果 read -s 参数，则表示控制台不显示输入过程，隐藏输入的值
  [root@www ~]# sh test.sh
  OK                     #标准输入
  OK It is a test        #输出
  ```

- 显示换行

  ```shell
  echo -e "Ok ! \n" # -e 开启转义
  echo "It is a test"
  #输出结果：注意ok后面有换行
  OK!

  It it a tes
  ```

- 显示不换行

  ```sh
  #!/bin/sh
  echo -e "OK! \c" # -e 开启转义 \c 不换行
  echo "It is a test"
  #结果：
  OK! It is a test
  ```

- 显示结果定向至文件

  ```shell
  echo "It is a test" > test.txt
  ```

- 原样输出字符，不转义不取变量，需要结合单引号

  ```sh
  echo '$name\"'
  #输出结果
  $name\"
  ```

- 显示命令执行结果

  ==命令用``  ` ``来包裹==

  ```sh
  echo `data`
  #结果：
  Thu Jul 24 10:08:46 CST 2014
  ```

## Shell printf命令

类似C语言的printf()函数，shell使用printf会比echo更具有跨平台移植性。可以类似C的printf()函数使用一些复杂的表达式，printf不支持自动换行，需要借助`\n`

```shell
printf format-string [args...]
```

示例，模拟shell输出，

```sh
$ echo "hello ,shell"
hello ,shell
$ printf "hello ,shell \n"
hello ,shell
$
```

脚本化的printf命令使用：

```sh
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com
 
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543 
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876 
```

执行脚本，显示结果：

```
姓名     性别   体重kg
郭靖     男      66.12
杨过     男      48.65
郭芙     女      47.99
```

`%s,%c,%d,%f`都是格式替换符，`%-10s`指一个宽度为10个字符（-表示左对齐，没有则右对齐），任何字符都会被显示在10个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。

%-4.2f 指格式化为小数，其中.2指保留2位小数。

```sh
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com
 
# format-string为双引号
printf "%d %s\n" 1 "abc"

# 单引号与双引号效果一样 
printf '%d %s\n' 1 "abc" 

# 没有引号也可以输出
printf %s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def

printf "%s\n" abc def

printf "%s %s %s\n" a b c d e f g h i j

# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n"
```

**Printf的转移序列**

| 序列             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| \a               | 警告字符，通常为ASCII的BEL字符                               |
| \b               | 后退                                                         |
| \c               | 抑制不显示输出结果中任何姐wide换行字符（只在%b格式指示控制符下的参数字符串中有效），而且任何留在参数里的字符，任何接下来的采纳书以及任何留在格式字符串中的字符，都被忽略。 |
| \f               | 换页                                                         |
| \n               | 换行                                                         |
| \r               | 回车                                                         |
| \t               | 水平tab                                                      |
| \v               | 竖直tab                                                      |
| \\\              | 转义后输出一个\符号                                          |
| \d     dd        | 表示1--3位的八进制字符，仅在格式字符串中有效。               |
| \0     dd      d | 表示1--3位的八进制字符                                       |

## Shell test命令

shell中test命令用于测试条件是否成立，可进行数字、字符和文件的条件测试

```sh
#!/bin/bash

#用之前的各种运算符，测试test
num1=100
num2=200
if test $[num1] -eq $[num2]
then
	echo 'true'
else
	echo 'false'
fi
```

## Shell 流程控制

区别于其他编程语言，shell的流程控制不可为空，如

```java
if(a>b){
  System.out.println("ok");
}else{
  //此处不做任何事
}
```

但是在shell中不能出现不做任何事的分支语句

### 1、if else

- if语句格式

  ```sh
  if condition
  then
  	command1
  	command2
  	...
  fi # 是if的倒写
  ```

  每个句子可以用`;`分号结束，可以写在一行好几个语句。

- if else

  ```sh
  if condition
  then
  	command1
  	command2
  	...
  else
  	command
  fi
  ```

- if else-if else

  ```sh
  if condition1
  then
  	command1
  elif condition2 #类似python中，else if是写作elif
  then
  	command2
  else
  	command
  fi
  ```

### 2、for循环

shell的for循环格式：

```sh
for var in item1 item2 ... itemN
do
	command1
	command2
	...
	
done #for循环结束的标识
#写成一行
for var in item1 item2 ... itemN;do command1; command2;...;done;
```

### 3、while语句

格式：

```sh
while condition
do
	command
done
#示例

#!/bin/sh
int=1
while(( $int<=5 ))
do
        echo $int
        let "int++" # let是个关键命令
done
```

可以结合`read`命令接收输入信息

### 4、无限循环

```sh
while :
do
	command
done

#或者
while true
do
	command
done

#或者
for (( ; ; ))
```

### 5、until循环

shell所有的`until`循环类似于一个特殊的for循环，知道满足条件时候才停止。一般还是`while`

```sh
until condition
do
	command
done
```

**条件可为任意测试条件，测试发生在循环末尾，因此循环至少执行一次—请注意这一点。**

### 6、case

类似其他语言的switch...case语句

```sh
case value in
mode1)
	command1
	...
	;; #case的结束标志
mode2)
	command2
	...
	;;
esac #case的反写
```

value只会匹配一个case，或者不匹配，则mode可用*号通配

```sh
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

### 7、跳出循环

shell也使用`break`和`continue`来跳出循环。

- break

  跳出所有循环，终止后面的执行。

  ```sh
  #!/bin/bash
  while :
  do
      echo -n "输入 1 到 5 之间的数字:"
      read aNum
      case $aNum in
          1|2|3|4|5) echo "你输入的数字为 $aNum!"
          ;;
          *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
              break
          ;;
      esac
  done
  ```

- continue

  跳出本次循环，执行下一轮循环

  ```sh
  #!/bin/bash
  while :
  do
      echo -n "输入 1 到 5 之间的数字: "
      read aNum
      case $aNum in
          1|2|3|4|5) echo "你输入的数字为 $aNum!"
          ;;
          *) echo "你输入的数字不是 1 到 5 之间的!"
              continue
              echo "游戏结束"
          ;;
      esac
  done
  ```

### 8、esac

case语句区别于C语言，用esac作为结束标志，`)`和`;;`标识每个case。



## Shell函数

shell可以自定义函数，然后自己调用。函数格式：

```sh
#function 关键字为可选项，参数也是可选
[ function ] funname [()]

{
  
  action;
  
  [return int;]
  
}
```

**说明：**

1. 可以带function fun()定义，也可以fun()定义，不带参数。
2. 参数返回，可以显示加: return 返回，若不加，则返回最后一条指令结果。

```shell
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

demoFun(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
```

含有返回值的函数：

```sh
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```

==调用函数返回值，用`$?`符号==，函数必须在被调用前定义。

- 函数参数

  在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...

  ```sh
  #!/bin/bash
  # author:菜鸟教程
  # url:www.runoob.com
  
  funWithParam(){
      echo "第一个参数为 $1 !"
      echo "第二个参数为 $2 !"
      echo "第十个参数为 $10 !"
      echo "第十个参数为 ${10} !"
      echo "第十一个参数为 ${11} !"
      echo "参数总数有 $# 个!"
      echo "作为一个字符串输出所有参数 $* !"
  }
  funWithParam 1 2 3 4 5 6 7 8 9 34 73
  #输出结果：
  第一个参数为 1 !
  第二个参数为 2 !
  第十个参数为 10 !
  第十个参数为 34 !
  第十一个参数为 73 !
  参数总数有 11 个!
  作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !
  ```

  **注意：**\$10不能获取第10个参数，因为当n>=10时候，要用\$(n)来获取参数。

## Shell 输入/输出重定向

类Uinx系统中，默认标准输入输出设备就是shell终端。重定向命令列表：

| 命令            | 说明                                       |
| --------------- | ------------------------------------------ |
| command > file  | 输出重定向到file                           |
| command < file  | 输出重定向到file                           |
| command >> file | 输出追加到file                             |
| n > file        | 文件描述符为n的文件重定向到file            |
| n >> file       | 文件描述符为n的文件追加到file              |
| n >& m          | 输出文件m和n合并                           |
| n <& m          | 输入文件m和n合并                           |
| << tag          | 开始标记tag和结束标记tag之间的内容作为输入 |

> **注意：**需要注意的是文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。

### 1、输出重定向

```sh
command1 > file1
```

若file1存在，则被替代。可以用`>>`追加符号，则不替代。

### 2、输入重定向

```sh
command1 < file1
```

```
command1 < infile > outfile
```

同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中。

### 3、重定向深入讲解

一般类unix系统命令运行会同时打开三个文件：

- 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
- 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
- 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息

默认情况下，command > file 将 stdout 重定向到 file，command < file 将stdin 重定向到 file。

如果希望 stderr 重定向到 file，可以这样写：

```sh
#模拟终端
$ command 2 > file
```

若要追加`>>`符号，其中2代表标准错误文件的代号。

如果希望将stdout和stderr合并后重定向到file

```sh
command > file 2>&1
#或者
command >> file 2>&1
#如果输入、输出都重定向
command <file1 >file2
```

### 4、Here Document

Here Document是shell的一种特殊重定向方式，用于将输入重定向到一个交互式shell脚本或程序：

```sh
command << delimiter
	document
delimiter
```

作用将delimiter之间的document作为输入传递给command

**注意：**结尾的`delimiter`前后不得有任何符号，包括tab和空格。

```sh
wc -l << EOF
	nihao
	zhendehenhao
	shime
	ok
EOF 
#输出结果，获得的EOF之间的内容的行数。
4
```

### 5、/dev/null文件

若希望执行命令不在屏幕输出，可重定向到/dev/null

```sh
command > /dev/null
```

`/dev/null`为特殊文件，写入的内容立即不见，不可读出。

如果屏蔽stdout和stderr

```sh
command > /dev/null 2>&1
```

## Shell文件包含

shell也可以使用外部脚本，便于封装：

```sh
. filename #注意点号(.)与文件名之间有空格
或
source filename
```

示例：test1.sh

```sh
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

url="http://www.runoob.com"
```

test2.sh

```sh
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

#使用 . 号来引用test1.sh 文件
. ./test1.sh

# 或者使用以下包含文件代码
# source ./test1.sh

echo "菜鸟教程官网地址：$url"
```

执行时候test2.sh只需要test2.sh有执行权限即可，test1.sh不一定需要。