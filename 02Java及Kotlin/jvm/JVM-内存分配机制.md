# Java 虚拟机内存分配机制

## 内存区域划分

对于大多数的程序员来说，Java 内存比较流行的说法便是堆和栈，这其实是非常粗略的一种划分，这种划分的“堆”对应内存模型的 Java 堆，“栈”是指虚拟机栈，然而 Java 内存模型远比这更复杂，想深入了解 Java 的内存，还是有必要明白整个内存区域分。

了解 Java GC 机制，必须先清楚在 JVM 中内存区域的划分。 在 Java 运行时的数据区里，由 JVM 管理的内存区域分为下图几个模块：

<img src="https://raw.githubusercontent.com/jeanboydev/Android-ReadTheFuckingSourceCode/master/resources/images/java/jvm/jvm_memory_area.png" alt="JVM 内存划分"/>

### 程序计数器（Program Counter Register）

程序计数器是一个比较小的内存区域，用于指示当前线程所执行的字节码执行到了第几行，可以理解为是当前线程的行号指示器。 字节码解释器在工作时，会通过改变这个计数器的值来取下一条语句指令。

每个程序计数器只用来记录一个线程的行号，所以它是线程私有（一个线程就有一个程序计数器）的。

如果程序执行的是一个 Java 方法，则计数器记录的是正在执行的虚拟机字节码指令地址；如果正在执行的是一个本地（ native，由 C 语言编写完成）方法，则计数器的值为 Undefined，由于程序计数器只是记录当前指令地址，所以不存在内存溢出的情况，因此，程序计数器也是所有JVM内存区域中唯一一个没有定义 OutOfMemoryError 的区域。

### 虚拟机栈（JVM Stack）

一个线程的每个方法在执行的同时，都会创建一个栈帧（Statck Frame），栈帧中存储的有局部变量表、操作数栈、动态链接、方法出口等，当方法被调用时，栈帧在 JVM 栈中入栈，当方法执行完成时，栈帧出栈。

局部变量表中存储着方法的相关局部变量，包括各种基本数据类型，对象的引用，返回地址等。 在局部变量表中，只有 long 和 double 类型会占用 2 个局部变量空间（Slot，对于32位机器，一个 Slot 就是 32 个 bit），其它都是 1 个 Slot。 需要注意的是，局部变量表是在编译时就已经确定好的，方法运行所需要分配的空间在栈帧中是完全确定的，在方法的生命周期内都不会改变。

虚拟机栈中定义了两种异常，如果线程调用的栈深度大于虚拟机允许的最大深度，则抛出 StatckOverFlowError（栈溢出）；不过多数 Java 虚拟机都允许动态扩展虚拟机栈的大小（有少部分是固定长度的），所以线程可以一直申请栈，直到内存不足，此时，会抛出 OutOfMemoryError（内存溢出）。

每个线程对应着一个虚拟机栈，因此虚拟机栈也是线程私有的。

### 本地方法栈（Native Method Statck）

本地方法栈在作用，运行机制，异常类型等方面都与虚拟机栈相同，唯一的区别是：虚拟机栈是执行 Java 方法的，而本地方法栈是用来执行 native 方法的，在很多虚拟机中（如：Sun 的 JDK 默认的 HotSpot 虚拟机），会将本地方法栈与虚拟机栈放在一起使用。

本地方法栈也是线程私有的。

### 堆区（Heap）

堆区是理解 Java GC 机制最重要的区域，没有之一。 在 JVM 所管理的内存中，堆区是最大的一块，堆区也是 Java GC 机制所管理的主要内存区域，堆区由所有线程共享，在虚拟机启动时创建。 堆区的存在是为了存储对象实例，原则上讲，所有的对象都在堆区上分配内存（不过现代技术里，也不是这么绝对的，也有栈上直接分配的）。

一般的，根据 Java 虚拟机规范规定，堆内存需要在逻辑上是连续的（在物理上不需要），在实现时，可以是固定大小的，也可以是可扩展的，目前主流的虚拟机都是可扩展的。 如果在执行垃圾回收之后，仍没有足够的内存分配，也不能再扩展，将会抛出 OutOfMemoryError:Java heap space 异常。

关于堆区的内容还有很多，将在下面“内存分配机制”中详细介绍。

### 方法区（Method Area）

在 Java 虚拟机规范中，将方法区作为堆的一个逻辑部分来对待，但事实上，方法区并不是堆（Non-Heap）；另外，不少人的博客中，将 Java GC 的分代收集机制分为 3 个代：青年代，老年代，永久代，这些作者将方法区定义为“永久代”，这是因为，对于之前的 HotSpot Java 虚拟机的实现方式中，将分代收集的思想扩展到了方法区，并将方法区设计成了永久代。 不过，除 HotSpot 之外的多数虚拟机，并不将方法区当做永久代，HotSpot 本身，也计划取消永久代。 本文中，由于主要使用 Oracle JDK6.0，因此仍将使用永久代一词。

方法区是各个线程共享的区域，用于存储已经被虚拟机加载的类信息（即加载类时需要加载的信息，包括版本、field、方法、接口等信息）、final 常量、静态变量、编译器即时编译的代码等。

方法区在物理上也不需要是连续的，可以选择固定大小或可扩展大小，并且方法区比堆还多了一个限制：可以选择是否执行垃圾收集。 一般的，方法区上执行的垃圾收集是很少的，这也是方法区被称为永久代的原因之一（HotSpot），但这也不代表着在方法区上完全没有垃圾收集，其上的垃圾收集主要是针对常量池的内存回收和对已加载类的卸载。

在方法区上进行垃圾收集，条件苛刻而且相当困难，效果也不令人满意，所以一般不做太多考虑，可以留作以后进一步深入研究时使用。

在方法区上定义了 OutOfMemoryError:PermGen space 异常，在内存不足时抛出。

- **运行时常量池（Runtime Constant Pool）** 

方法区的一部分，用于存储编译期就生成的字面常量、符号引用、翻译出来的直接引用（符号引用就是编码是用字符串表示某个变量、接口的位置，直接引用就是根据符号引用翻译出来的地址，将在类链接阶段完成翻译）；运行时常量池除了存储编译期常量外，也可以存储在运行时间产生的常量（比如 String 类的 intern() 方法，作用是 String 维护了一个常量池，如果调用的字符 “abc” 已经在常量池中，则返回池中的字符串地址，否则，新建一个常量加入池中，并返回地址）。

### 直接内存（Direct Memory）

直接内存并不是 JVM 管理的内存，可以这样理解，直接内存，就是 JVM 以外的机器内存。

比如：你有 4G 的内存，JVM占用了1G，则其余的 3G 就是直接内存，JDK 中有一种基于通道（Channel）和缓冲区（Buffer）的内存分配方式，将由 C 语言实现的 native 函数库分配在直接内存中，用存储在 JVM 堆中的 DirectByteBuffer 来引用。 由于直接内存受到本机器内存的限制，所以也可能出现 OutOfMemoryError 的异常。


## 内存分配机制

以下面代码为例，来分析，Java 的实例对象在内存中的空间分配。

```Java
//JVM 启动时将 Person.class 放入方法区
public class Person {

	//new Person 创建实例后，name 引用放入堆区，name 对象放入常量池
    private String name;

	//new Person 创建实例后，age = 0 放入堆区
    private int age;

	//Person 方法放入方法区，方法内代码作为 Code 属性放入方法区
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

	//toString 方法放入方法区，方法内代码作为 Code 属性放入方法区
    @Override
    public String toString() {
        return "Person{" + "name='" + name + '\'' + ", age=" + age + '}';
    }
}
```

```Java
//JVM 启动时将 Test.class 放入方法区
public class Test {

	//main 方法放入方法区，方法内代码作为 Code 属性放入方法区
    public static void main(String[] args) {

        //person1 是引用放入虚拟机栈区，new 关键字开辟堆内存 Person 自定义对象放入堆区
        Person person1 = new Person("张三", 18);
        Person person2 = new Person("李四", 20);

        //通过 person 引用创建 toString() 方法栈帧
        person1.toString();
        person2.toString();
    }
}
```

1. 首先 JVM 会将 Test.class, Person.class 加载到方法区，找到有 main() 方法的类开始执行。

<img src="https://raw.githubusercontent.com/jeanboydev/Android-ReadTheFuckingSourceCode/master/resources/images/java/jvm/jvm_memory_area_simple1.png" alt="JVM 内存划分 实例1"/>

如上图所示，JVM 找到 main() 方法入口，创建 main() 方法的栈帧放入虚拟机栈，开始执行 main() 方法。

```Java
Person person1 = new Person("张三", 18);
```

执行到这句代码时，JVM 会先创建 Person 

实例放入堆区，person2 也同理。

2. 创建完 Person 两个实例，main() 方法中的 person1，person2 会指向堆区中的 0x001，0x002（这里的内存地址仅作为示范）。紧接着会调用 Person 的构造函数进行赋值，如下图：

<img src="https://raw.githubusercontent.com/jeanboydev/Android-ReadTheFuckingSourceCode/master/resources/images/java/jvm/jvm_memory_area_simple2.png" alt="JVM 内存划分 实例2"/>

如上图所示，新创建的的 Person 实例中的 name, age 开始都是默认值。 调用构造函数之后进行赋值，name 是 String 引用类型，会在常量池中创建并将地址赋值给 name，age 是基本数据类型将直接保存数值。

注：Java 中基本类型的包装类的大部分都实现了常量池技术，这些类是 Byte, Short, Integer, Long, Character, Boolean，另外两种浮点数类型的包装类则没有实现。


| 基本数据类型	| 包装类 （是否实现了常量池技术）	|
| :---------	| :-----------------------		|
| byte			| Byte	是						|
| boolean		| Boolean	是					|
| short			| Short	是						|
| char			| Character	是					|
| int			| Integer	是					|
| long			| Long	是						|
| float			| Float	否						|
| double		| Double	否					|

3. Person 实例初始化完后，执行到 toString() 方法，同 main() 方法一样 JVM 会创建一个 toString() 的栈帧放入虚拟机栈中，执行完之后返回一个值。

<img src="https://raw.githubusercontent.com/jeanboydev/Android-ReadTheFuckingSourceCode/master/resources/images/java/jvm/jvm_memory_area_simple3.png" alt="JVM 内存划分 实例3"/>

## 参考资料

《深入理解 Java 虚拟机》

## 我的公众号

欢迎你「扫一扫」下面的二维码，关注我的公众号，可以接受最新的文章推送，有丰厚的抽奖活动和福利等着你哦！😍

<img src="https://raw.githubusercontent.com/jeanboydev/Android-ReadTheFuckingSourceCode/master/resources/images/about_me/qrcode_android_besos_black_512.png" width=250 height=250 />

如果你有什么疑问或者问题，可以 [点击这里](https://github.com/jeanboydev/Android-ReadTheFuckingSourceCode/issues) 提交 issue，也可以发邮件给我 [jeanboy@foxmail.com](mailto:jeanboy@foxmail.com)。

同时欢迎你 [![Android技术进阶：386463747](https://camo.githubusercontent.com/615c9901677f501582b6057efc9396b3ed27dc29/687474703a2f2f7075622e69647171696d672e636f6d2f7770612f696d616765732f67726f75702e706e67)](http://shang.qq.com/wpa/qunwpa?idkey=0b505511df9ead28ec678df4eeb7a1a8f994ea8b75f2c10412b57e667d81b50d) 来一起交流学习，群里有很多大牛和学习资料，相信一定能帮助到你！


