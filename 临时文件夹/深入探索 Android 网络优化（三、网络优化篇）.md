---

		title:  深入探索 Android 网络优化（三、网络优化篇）
		date: 2020/2/5 17:43:00   
		tags: 
		- Android进阶
		categories: Android进阶
		thumbnail: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1557665970516&di=b58d306a0db07efca58f8c9b655f5c13&imgtype=0&src=http%3A%2F%2Fimg02.tooopen.com%2Fimages%2F20160520%2Ftooopen_sl_055418231108.jpg
---

---

# 前言

### 成为一名优秀的Android开发，需要一份完备的[知识体系](https://github.com/JsonChao/Awesome-Android-Exercise)，在这里，让我们一起成长为自己所想的那样~。

# 本文思维导图

![](https://user-gold-cdn.xitu.io/2020/6/8/172912c4eee1f382?w=3576&h=2136&f=png&s=1034100)


# 一、网络优化维度

## 1、网络优化分析

基础网络的效率就像一辆列车，时延是火车的速度 (启动时间)，而带宽就像火车的车厢装载量，整个传输的物理链路就像火车的铁轨。从网络的通信过程来看，共涉及到 **三个模块**：

- 1）、**网络库 SDK 内部的设计与策略：I/O 并发模型，针对网络问题的优化**。
- 2）、**服务器性能：并发、带宽能力**。
- 3）、**网络相关：用户网络（弱网/强网）、运营商、网络链路等**。


而对于网络的优化，我们可以从以下五个维度来进行。

### 1）、流量优化

精确获取网络流量的消耗量，解决整体均值掩盖单点异常流量的问题。

### 2）、网络监控

建设全面的网络监控，因为粗粒度的监控不能够帮助我们发现和解决问题。

### 3）、流量消耗

- 1、精准获取一段时间的流量消耗、网络类型、前后台。
- 2、用户流量消耗均值、异常率（消耗多、次数多）。
- 3、完整链路全监控（Request、Response）、主动上报。

### 4）、网络请求质量

- 1、请求时长、业务成功率、失败率、TOP 失败接口，导致请求失败的原因通常有两种情况：
    - 1）、弱信号：可以简单看成手机信号只有一两格的时候，这是不仅仅是信令（无线网络通信的都是一个个的信令）发出去困难，还可能导致不断切换网络、基站。App 只能在应用层做重试，因为弱信号一般都是一时的。
    - 2）、拥塞网络：可以类比为堵车、排队的场景，数据包排队，信令也在排队。这时 App 不断重试，只会使得拥塞网络更为严重。我们只能让自己的非核心业务不要去排队，并让核心业务的数据量更少，协议来回更少。
- 2、用户体验
- 3、请求速度、成功率：网络正常时如何更好地利用带宽提升网络请求速度？
- 4、弱网：网络不稳定是如何最大程度上保证网络的连通性？
- 5、安全：如何防止被第三方劫持、窃听甚至篡改？

### 5）、其它

- 1、公司成本
- 2、带宽、服务器数量、CDN
- 3、耗电


## 2、网络优化误区

- 1）、仅仅关注流量消耗，忽视其它维度。
- 2）、仅仅关注均值、整体、忽视个体。


# 二、网络优化工具

## 1、Network Profiler

### 特点

- 1）、显示实时网络活动：发送、接收数据及连接数。
- 2）、需启动高级分析。
- 3）、仅支持 HttpURLConnection 与 OkHttp


### 打开高级分析

> Run => Edit Cofigurations => 界面最右边 Profiling => 打开 Enable advanced profiling （required for API level < 26 only）


### 使用 Network Profiler 调试 WanAndroid 网络请求

选中目标网络请求，可以看到在下方的 Connection View 一栏看到对应的网络数据，如下所示：

- Size
- Type
- Status
- Time
- Timeline


选中 Connection View 特定的一条数据即可在右边看到该请求对应的网络数据。

#### Overview

该网络请求的预览信息

#### 普通 Json 数据请求


![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc2498f34313?w=2394&h=1246&f=png&s=231794)


#### 图片加载请求


![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc35a3b634da?w=2406&h=1250&f=png&s=512906)


![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc3e289ff062?w=2402&h=1312&f=png&s=452439)


#### Response

Response Header 与 Body 信息

#### Request

Request Header 与 Body 信息

#### CallStack

网络请求的调用堆栈信息，
下图就是 Awesome-WanAndroid 发起一个网络请求所经历的调用堆栈：

![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc4b1c16acb6?w=906&h=1154&f=png&s=304570)


Awesome-WanAndroid 使用 Glide 发起一个图片加载请求所经历的调用堆栈：

![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc4ebc7cb5a7?w=968&h=628&f=png&s=168544)


#### 关键细节

高级配置中的 required for API level < 26 only 不是限定调试的手机版本小于26，我使用 API 27 的手机也可以调试。


#### 实践中获得的新知识及感悟

如果想快速搞懂接手项目中的网络/图片加载等框架的w网络请求流程，可以使用 Profiler NETWORK 的 CallStack 功能，并且双击其中任意的一行调用链方法都可以 jump to 指定源码。


> 选中网络请求无法显示数据？

打开高级分析即可。


## 2、Charles

使用 Java 开发的，MAC 上使用较多。

### 特点

- 1）、断点功能
- 2）、Map Local
- 3）、弱网环境模拟


### 安装配置

#### 1、下载 [Charles](https://www.charlesproxy.com)。

#### 2、截获手机端的网络包。

需要配置手机与电脑连接同一 WIFI。

##### 1）、电脑端设置 Charles — 打开 HTTP 代理并设置代理端口

- Charles 菜单栏 => Proxy => Proxy Settings => 填代理端口 8888 并勾选 Enable transparent HTTP proxying。

##### 2）、手机端设置 WIFI 代理及端口

- 获取电脑 IP 地址
    - 点击 Charles 的 help => local address。
- 设置 WIFI 代理及端口号
    - 手机设置 => WLAN => 查看当前连接的 WIFI 详情 => 最底部代理项设置为手动 => 配置 电脑 IP 地址与端口号8888。

##### 3）、设置完成，运行任意联网程序，Charles 会弹出请求连接的确认框，点击 allow 即可。

#### 3、截取 HTTPS

需要信任 Charles 的 CA 证书。

##### 1）、打开 SSL 代理，并配置 Host 与 Port

- 电脑端 Proxy => SSL Proxying Setting => 选中 Enable SSL Proxying 并点击 Add 配置 Host 与 Port 分半为 * 与 443。

##### 2）、信任 Charles Proxy CA 机构

- 电脑端 Help => SSL Proxying => Install Charles Root Certificate => 选中并双击 Charles Proxy CA 根证书颁发机构 => 点击信任 => 使用此证书时选择始终信任。

##### 3）、手机端安装 Charles 颁发的 SSL 证书

- 电脑端 Help => SSL Proxying => Install Charles Root Certificate on a Mobile Device，此时会弹出提示框让手机端访问 http://chls.pro/ssl 去下载证书。

##### 4）、手机端安装证书

- 从文件管理器中找到下载文件 => 如果是 .pem 结尾i，将后缀名改为 .crt 并点击该文件 => 输入锁屏密码 => 等待证书导入后配置证书名（我填的是 Charles）即可。

### 实践过程

#### 选中目标网络请求

从 Overview 中可以看到很全面抓包信息 。

#### 使用断点功能

##### 1）、右键点击要断点的 URL，选中 BreakPoints 开启断点功能。

##### 2）、点击顶部 Proxy => Breadkpoint Settings。

##### 3）、双击 Breakpoints Settings 面板中的目标
URL，在弹出的 Edit Breakpoint 面板中进行编辑。

##### 4）、这里默认选择断点 Request 与 Response，我们可以选择仅断点 Response 或 Request。点击确认即断点设置完成。

##### 5）、然后，我们就可以点击主面板右侧的 Edit Response 编辑 Response，修改完成后点击最下方的 Execute 即可。

#### 使用 Map Local

##### 1）、自由模拟服务端的返回数据，以提前进行接口测试。

##### 1）、右键点击要使用 Map Local 的 URL，选中Map Local 开启断点功能。

##### 1）、然后，我们在 Edit Mapping 面板中选择 Map To 的 Local path，选择本地设定的 maplocal 本地数据（例如 JsonString）


#### 弱网模拟功能

- 1）、注意开启前需将 Map Local 关闭。

- 2）、点击 Proxy => Throttle Setting => 选中 Enable Throttling

- 3）、这里预设了很多模拟设置，我们只需将 网络包传输的速率 Throttle preset 设置为较低的速率（一般设为 256/512）。


### 碰到的问题

- 1）、注意手机与笔记本电脑需要同一 WIFI 下，不能自己开热点或使用公司内网，否则无法在 电脑端 无法弹出手机连接 Charles 的提示确认框，并且也无法下载 Charles 提供的 SSL 证书。
- 2）、手机端下载 Charles 提供的 SSL 证书时最好不使用系统浏览器访问。


## 3、Wireshark

> 强烈推荐 [geektime-webprotocol](https://github.com/geektime-geekbang/geektime-webprotocol)


WireShark 主要可以用来对四种流进行跟踪，如下所示：

- TCP
- UDP 
- SSL
- HTTP


### 1）、WireShark 基本使用

#### 如何捕获报文

- 1）、点击捕获->选项，打开捕获窗口
    - 网卡设备/流量/捕获过滤器，点击“开始”按钮开始抓包
    - 输出(指定缓存文件)/选项(显示、名称解析、自动停止抓包条件) 面板
- 2）、点击捕获->停止，停止抓包


![](https://user-gold-cdn.xitu.io/2020/6/7/1728cbd5e35aed47?w=1738&h=696&f=png&s=499751)


#### Wireshark 面板

![](https://user-gold-cdn.xitu.io/2020/6/7/1728cbe46b1fbf33?w=2112&h=1012&f=png&s=2244828)


#### 快捷方式工具栏


![](https://user-gold-cdn.xitu.io/2020/6/7/1728cbef2412372d?w=1978&h=936&f=png&s=519686)


#### 数据包的颜色(视图->着色规则)


![](https://user-gold-cdn.xitu.io/2020/6/7/1728cbfc4b7c46cc?w=1700&h=764&f=png&s=1098039)


#### 设定时间显示格式

![](https://user-gold-cdn.xitu.io/2020/6/7/1728cc27686a0a0c?w=1156&h=944&f=png&s=572015)


#### 数据包列表面板的标记符号

![](https://user-gold-cdn.xitu.io/2020/6/7/1728cc30efa16f56?w=1560&h=832&f=png&s=190573)


#### 文件操作

- 1）、标记报文 Ctrl+M。
- 2）、导出标记报文(文件->导出特定分组)，亦可按过滤器导出报文 ，
- 3）、合并读入多个报文(文件->合并)。


#### 如何快速抓取移动设备的报文?

- 1、打开手机的 wifi 热点。
- 2、电脑连接手机的 wifi 热点。
- 3、用 Wireshark 打开捕获->选项面板，选择 wifi 热点对应的接口设备抓包即可。


### 2）、Wireshark 过滤器

如果表达式的背景为绿色，则说明过滤器的语法是正确的，红色则说明有错误。

#### 捕获过滤器

它用于减少抓取的报文体积，使用 BPF（Berkeley Packet Filter） 语法，功能相对有限。

BPF 可以在设备驱动级别提供抓包过滤接口，多数抓包工具都支持此语法。而 BPF 的 Expression 表达式由多个 primitives 原语组成。而每一个 primitives 原语则由名称或数字，以及描述它的多个 qualifiers 限定词组成。

##### qualifiers 限定词

- 1、Type：设置数字或者名称所指示类型
    - host、port。
    - net ，设定子网，net 192.168.0.0 mask 255.255.255.0 等价于 net 192.168.0.0/24。
    - portrange，设置端口范围，例如 portrange 6000-8000。
- 2、Dir:设置网络出入方向
    - src、dst、src or dst、src and dst。
    - ra、ta、addr1、addr2、addr3、addr4(仅对 IEEE 802.11 Wireless LAN 有效)。
- 3、Proto:指定协议类型
    - ether、fddi、tr、 wlan、 
    - ip、 ip6、 arp、 rarp、 
    - decnet、 tcp、udp、icmp、igmp、icmp
    - igrp、pim、ah、esp、vrrp
- 4、其他
    - gateway:指明网关 IP 地址，等价于 ether host ehost and not host host。
    - broadcast:广播报文，例如 ether broadcast 或者 ip broadcast。
    - multicast:多播报文，例如 ip multicast 或者 ip6 multicast。
    - less, greater:小于或者大于。


#### 简单示例

```
src or dst portrange 6000-8000 && tcp or ip6
```


#### 显示过滤器

它对已经抓取到的报文进行过滤显示，功能强大。

##### 基于协议域过滤

- 捕获所有 TCP 中的 RST 报文：tcp[13]&4==4。
- 抓取 HTTP GET 报文：port 80 and tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x47455420。（47455420 是 ASCII 码的 16 进制，表示”GET ”）
- 抓取 HTTP POST 报文：port 80 and tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504F5354


##### 显示过滤器的过滤属性

任何在报文细节面板中解析出的字段名，都可以作为过滤属性。在视图->内部->支持的协议面板里，可以看到各字段名对应的属性名。例如，在报文细节面板中 TCP 协议头中的 Source Port，对应着过滤属性为 tcp.srcport。


##### 常用操作符

- 1）、and（&&）：AND 逻辑与，ip.src==10.0.0.5 and tcp.flags.fin。
- 2）、or（||）：OR 逻辑或，ip.scr==10.0.0.5 or ip.src==192.1.1.1。
- 3）、xor（^^）：XOR 逻辑异或，tr.dst[0:3] == 0.6.29 xor tr.src[0:3] == 0.6.29。
- 4）、not（!）：NOT 逻辑非，not llc。
- 5）、[...​]：中括号[]Slice 切片操作符
    - [n:m]表示 n 是起始偏移量，m 是切片长度，例如：eth.src[0:3] == 00:00:83
    - [n-m]表示 n 是起始偏移量，m 是截止偏移量，例如：eth.src[1-2] == 00:83
    - [:m]表示从开始处至 m 截止偏移量，例如：eth.src[:4] == 00:00:83:00
    - [m:]表示 m 是起始偏移量，至字段结尾，例如：eth.src[4:] == 20:20
    - [m]表示取偏移量 m 处的字节，例如：eth.src[2] == 83
    - [,]使用逗号分隔时，允许以上方式同时出现，例如：eth.src[0:3,1-2,:4,4:,2] ==00:00:83:00:83:00:00:83:00:20:20:83
- 6）、in：大括号{}集合操作符，例如 tcp.port in {443 4430..4434} ，实际等价于 tcp.port == 443 || (tcp.port >= 4430 && tcp.port ⇐ 4434)。


##### 可用函数

- upper：将字符串字段转换为大写。
- lower：将字符串字段转换为小写。
- len：返回字符串或字节数组的字节长度。
- count：返回在一帧中字段出现的数量。
- string：将非字符串字段转换为字符串。


##### 显示过滤器的可视化对话框

![](https://user-gold-cdn.xitu.io/2020/6/7/1728cf091d66d139?w=1982&h=952&f=png&s=1094535)


##### 环形缓冲器

例如使用 3 个文件的环形缓存器，从 **.1 => **.2 => **.3 然后又从 **.1 文件开始记录，形成环形。


## 4、TcpDump（网络数据包嗅探器）

### 1）、抓包步骤

#### 1、获取 ROOT 权限的手机一部

#### 2、下载 [tcpdump](www.androidtcpdump.com)

#### 3、将 tcpdump 安装到手机上

```
adb push tcpdump /data/local/tmp
```


#### 4、修改 tcpdump 的权限，使其具有可执行的权限

```
chmod 777 /data/local/tmp/tcpdump
```


#### 5、执行 tcpdump 命令进行抓包，按组合键 Ctrl + C 可以停止抓包

#### 6、将抓到的数据包的信息保存为 Pcap 文件，这里仅需在执行 tcpdump 后加上 -w 参数

```
tcpdump-w /data/local/tmp/tcp.pcap
```

#### 7、把 Pcap 复制到 电脑上，使用 Wireshark 分析数据包的流量。

### 2）、捕获及停止条件

- -D：列举所有网卡设备。
- -i：选择网卡设备。
- -c：抓取多少条报文。
- --time-stamp-precision：指定捕获时的时间精度，默认毫秒 micro，可选纳秒 nano。 
- -s：指定每条报文的最大字节数，默认 262144 字节。


### 3）、文件操作

- -w：输出结果至文件(可被Wireshark读取分析)。
- -C：限制输入文件的大小，超出后以后缀加 1 等数字的形式递增。 注意单位是 1,000,000 字节。
- -W：指定输出文件的最大数量，到达后会重新覆写第 1 个文件。
- -G：指定每隔N秒就重新输出至新文件，注意-w 参数应基于
strftime 参数指定文件名。
- -r：读取一个抓包文件。
- -V：将待读取的多个文件名写入一个文件中，通过读取该文件同时 读取多个文件。


### 4）、输出时间戳格式

- -t：不显示时间戳。
- -tt：自1970年1月1日0点至今的秒数。
- -ttt：显示邻近两行报文间经过的秒数。
- -tttt：带日期的完整时间。
- -ttttt：自第一个抓取的报文起经历的秒数。


### 5）、分析信息详情

- -e：显示数据链路层头部。 
- -q：不显示传输层信息。
- -v：显示网络层头部更多的信息，如 TTL、id 等。
- -n：显示 IP 地址、数字端口代替 hostname 等。
- -S：TCP 信息以绝对序列号替代相对序列号。
- -A：以 ASCII 方式显示报文内容，适用 HTTP 分析。
- -x：以 16 进制方式显示报文内容，不显示数据链路层。
- -xx：以 16 进制方式显示报文内容，显示数据链路层。
- -X：同时以 16 进制及 ACII 方式显示报文内容，不显示数据链路层 • -XX 同时以 16 进制及 ACII 方式显示报文内容，显示数据链路层。


## 5、Stetho

- 1）、在 build.gradle 中，除了 Stetho 依赖外，还需添加 'com.facebook.stetho:stetho-okhttp3:1.5.0'。
- 2）、在 Application 的 onCreate 方法中初始化 'Stetho.initializeWithDefaults(this)'。
- 3）、调用 OkHttp 的 'addNeworkInterceptor' 方法添加 Stetho 用于收集网络信息而提供的网络拦截器。
- 4）、访问 Chrome 调试页面 'chrome://inspect'。


## 6、其它的性能检测工具

- strace：跟踪 Socket 相关的系统调用。
- netstat：记录多种网络栈和接口统计信息。
- ifconfig：记录接口配置。
- ip：记录网络接口统计信息。
- ping：测试网络连通性。
- traceroute：测试网络路由。
- /proc/net 命令：查看网络统计信息，Android TrafficState 使用了 /proc/net/xt_qtaguid/stats 和 /proc/net/xt_qtaguid/iface_stat_fmt 文件来统计 App 的流量信息。


# 三、精准获取流量消耗

## 1、如何判断 App 流量消耗偏高？

- 1）、绝对值看不出高低。
- 2）、对比竞品，相同 Case 对比流浪消耗。
- 3）、异常监控超过正常指标。

## 2、测试方案

- 1）、打开手机设置 => 流量管理 => 仅允许目标 App 联网
- 2）、可以查找出大多数的问题，但是线上场景线下可能遇不到。


## 3、线上流量获取方案

### 1）、TrafficStats

#### 特点

- API 18 以上。
- 记录手机重启以来的数据流量。

#### API

- getMobileRxBytes()：通过蜂窝流量接收到的信息。
- getUidRxBytes(int uid)：获取指定 uid 的接收流量。
- getTotalRxBytes()：总发送流量。


#### 缺点

无法获取某个时间段内的流量消耗。

### 2）、NetworkStatsManager

API 23 之后。

#### 特点

- 1）、获取指定时间间隔内的流量信息。
- 2）、获取不同网络类型下的消耗。


#### NetUtils.getStats 

获取指定时间间隔的 蜂窝 + WIFI 流量总信息


## 4、前后台流量获取方案

### 问题：线上反馈 App 后天流量消耗大？

只获取一个时间段的流量不够全面。

### 实现原理

> 后台定时任务 => 获取时间间隔内流量 => 记录前后台 => 分别计算 => 上报 APM 后台 => 流量治理依据


### 小结

- 1）、该方案无法获取应用在前后台切换时的流量，因此有一定的误差，但这个误差是可以接受的。
- 2）、结合精细化的流量异常报警针对性的解决后台跑流量的问题。


成功 log 如下所示：

```java
2020-05-11 10:47:55.633 4036-4181/json.chao.com.wanandroid I/WanAndroid-LOG: │ [MainActivity.java | 193 | lambda$initEventAndData$1$MainActivity] backNetUseData: 4 MB
2020-05-11 10:47:55.646 4036-4181/json.chao.com.wanandroid I/WanAndroid-LOG: │ [MainActivity.java | 194 | lambda$initEventAndData$1$MainActivity] foreNetUseData: 4 MB
2020-05-11 10:47:55.652 4036-4181/json.chao.com.wanandroid I/WanAndroid-LOG: │ [MainActivity.java | 197 | lambda$initEventAndData$1$MainActivity] totalNetUseData: 8 MB
```

 
# 四、网络请求流量优化

## 1、常见使用网络的场景

### 1）、数据压缩

POST 请求 Body 使用 GZip 压缩，同时服务端返回 Body 也使用 GZip 压缩。

### 2）、图片

- 图片上传前压缩。
- 图片使用策略细化：让 服务端/CDN 云服务器 优先使用缩略图/WebP格式图片。


### 3）、性能日志上报：批量 + 特定场景上报

APM 相关、单点问题相关。例如埋点数据可以等到某一时机点（例如 开启了 WIFI、数据量过大必须上传一部分时）再上传。

### 4）、数据缓存

服务端返回加上过期时间，避免每次重新获取。
节约流量且大幅提高数据访问速度，更好的用户体验。

#### Request 缓存设置

- 1、Pragma:no-cache：去服务器拉取最新的资源，不使用缓存。
- 2、If-Modified-Since:datetime：如果资源在客户端提供的时间后发生改变，服务器会返回新的资源，否则使用缓存。
- 3、If-None-Match:etagvalue：如果资源的标识和服务器的不同，返回新的资源。


当 Request 的头部是 2 和 3 时，如果服务器的资源没有修改，则服务器会返回 HTTP/304 Not Modified，客户端会使用缓存的 Response。

#### Response 缓存设置

HTTP Response 是否可以缓存是由 Response 的头部控制的，服务器可以通过 Expires 和 Cache-Control 控制 Response 如何在客户端缓存。

##### Expires

Expires 头部会包含一个日期，即该资源缓存的有效期，客户端有新的相同请求时，如果资源缓存没有过期，则使用缓存资源，服务器不会返回任何东西。

##### Cache-Control

Cache-Control 可以标明 Response 如何存储及其如何使用，其选项如下所示：

- 1）、public：Response 可以存储在任何 Cache 中，包括共享的 Cache。
- 2）、private：Response 存储在私有 Cache 中，只能被一个用户使用。
- 3）、no-cache：Response 将来不会被使用。
- 4）、no-store：Response 将来不会被使用，也不会写到磁盘上。
- 5）、max-age=#seconds：Response 在设定的时间内可以被重复使用。
- 6）、must-revalidate：和原始服务器确认 Response 是最新后，可以使用缓存。


#### OKHttp 无网数据缓存实现

POST 在 OKHttp 中默认不会缓存，因为 POST 一般是用来修改数据的。在 Awesome-WanAndroid 中的 HttpModule—cacheInterceptor 中就已经实现了 OKHttp 的无网数据缓存，代码如下所示：

```java
File cacheFile = new File(Constants.PATH_CACHE);
Cache cache = new Cache(cacheFile, 1024 * 1024 * 50);
Interceptor cacheInterceptor = chain -> {
    Request request = chain.request();
    if (!CommonUtils.isNetworkConnected()) {
        // 无网时强制使用数据缓存，以提升用户体验。
        request = request.newBuilder()
                .cacheControl(CacheControl.FORCE_CACHE)
                .build();
    }
    Response response = chain.proceed(request);
    if (CommonUtils.isNetworkConnected()) {
        int maxAge = 0;
        // 有网络时, 不缓存, 最大保存时长为0
        response.newBuilder()
                .header("Cache-Control", "public, max-age=" + maxAge)
                .removeHeader("Pragma")
                .build();
    } else {
        // 无网络时，设置超时为4周
        int maxStale = 60 * 60 * 24 * 28;
        response.newBuilder()
                .header("Cache-Control", "public, only-if-cached, max-stale=" + maxStale)
                .removeHeader("Pragma")
                .build();
    }
    return response;
};
// 缓存优化
builder.addNetworkInterceptor(cacheInterceptor);
builder.addInterceptor(cacheInterceptor);
builder.cache(cache);
```


### 5）、离线包、增量数据更新

加上版本的概念，仅传输有变化的数据。

### 6）、请求头压缩

如果请求头不变，服务端可以使用映射缓存 请求头 MD5 : 请求头，之后请求头都使用 MD5 即可。

### 7）、优化发送频率和时机

### 8）、合并网络请求、减少请求次数。

### 9）、流量兜底能力

如果发现流量异常，我们可以通过后台服务器终止协议交互，以避免问题恶化。



## 2、流量统计

我们可以利用 [network-connection-class](https://github.com/facebookarchive/network-connection-class) 进行流量统计，它内部使用的是 API 8 的 TrafficStats 类，用于获取整个手机或者某个 UID 从开机算起的网络流量。


### 1）、四个核心 API

```java
// 从开机开始Mobile网络接收的字节总数，不包括Wifi
getMobileRxBytes()        
// 从开机开始所有网络接收的字节总数，包括Wifi
getTotalRxBytes()     
// 从开机开始Mobile网络发送的字节总数，不包括Wifi
getMobileTxBytes()        
// 从开机开始所有网络发送的字节总数，包括Wifi
getTotalTxBytes()         
```


### 2）、对应的Linux 内核 proc 统计接口

```java
// stats接口提供各个uid在各个网络接口（wlan0, ppp0等）的流量信息
/proc/net/xt_qtaguid/stats
// iface_stat_fmt接口提供各个接口的汇总流量信息
proc/net/xt_qtaguid/iface_stat_fmt
```


### 3）、工作原理

- 1）、读取 proc，并将目标 UID 下面所有网络接口的流量相加。
- 2）、Android 7.0 之后只能通过 TrafficStats 拿到自己应用的流量信息。

、

# 五、网络请求质量优化（🔥）

## 1、Http 请求过程

- 1）、请求到达运营商的 **DNS** 服务器并* *解析** 成对应的 IP 地址。
    - **HTTPDNS**
- 2）、根据 IP 地址找到相应的服务器，进行 TCP 三次握手，**创建连接**。
    - **连接复用**
    - **网络库的连接管理**
- 3）、发送/接收数据。
    - **压缩**
    - **加密**
- 4）、关闭连接。


## 2、HTTPDNS

> 问题：DNS 解析慢/被劫持？

使用 HTTPDSN，HTTPDNS 不是使用 DNS 协议，向 DNS 服务器传统的 53 端口发送请求，而是使用 HTTP 协议向 DSN 服务器的 80 端口发送请求。

### 1）、HTTPDNS 优势

- 1、绕过运营商域名解析的过程，避免 Local DNS 的劫持。
- 2、降低平均访问时延，提供连接成功率。
- 3、HTTPDNS 服务器会增加流量调度、网络拨测/灰度、网络容灾等功能。


### 2）、HTTPDNS + OKHttp 实践

在 Awesome-WanAndroid 中已经实现了 HTTPDNS 优化，其优化代码如下所示：

```java
// HttpModule-provideClient：httpDns 优化
builder.dns(OkHttpDns.getIns(WanAndroidApp.getAppComponent().getContext()));

/**
 * FileName: OkHttpDNS
 * Date: 2020/5/8 16:08
 * Description: HttpDns 优化
 * @author JsonChao
 */
public class OkHttpDns implements Dns {

    private HttpDnsService dnsService;
    private static OkHttpDns instance = null;

    private OkHttpDns(Context context) {
        dnsService = HttpDns.getService(context, "161133");
        // 1、设置预解析的 IP 使用 Https 请求。
        dnsService.setHTTPSRequestEnabled(true);
        // 2、预先注册要使用到的域名，以便 SDK 提前解析，减少后续解析域名时请求的时延。
        ArrayList<String> hostList = new ArrayList<>(Arrays.asList("www.wanandroid.com"));
        dnsService.setPreResolveHosts(hostList);
    }

    public static OkHttpDns getIns(Context context) {
        if (instance == null) {
            synchronized (OkHttpDns.class) {
                if (instance == null) {
                    instance = new OkHttpDns(context);
                }
            }
        }
        return instance;
    }

    @Override
    public List<InetAddress> lookup(String hostname) throws UnknownHostException {
        String ip = dnsService.getIpByHostAsync(hostname);
        LogHelper.i("httpDns: " + ip);
        if(ip != null){
            List<InetAddress> inetAddresses = Arrays.asList(InetAddress.getAllByName(ip));
            return inetAddresses;
        }
        // 3、如果从阿里云 DNS 服务器获取不到 ip 地址，则走运营商域名解析的过程。
        return Dns.SYSTEM.lookup(hostname);
    }
}
```


重新安装 App，通过 HTTPDNS 获取到 IP 地址 log 如下所示：

```java
2020-05-11 10:41:55.139 4036-4184/json.chao.com.wanandroid I/WanAndroid-LOG: │ [OkHttpDns.java | 52 | lookup] httpDns: 47.104.74.169
2020-05-11 10:41:55.142 4036-4185/json.chao.com.wanandroid I/WanAndroid-LOG: │ [OkHttpDns.java | 52 | lookup] httpDns: 47.104.74.169
```


## 3、网络库的连接管理

利用 HTTP 协议的 keep-alive，建立连接后，会先将连接放入连接池中，如果有另一个请求的域名和端口是一样的，就直接使用连接池中对应的连接发送和接收数据。在实现网络库的连接管理时需要注意以下4点：

- 1）、同一个连接仅支持同一个域名。
- 2）、后端支持 HTTP 2.0 需要改造，这里可以通过在网络平台的统一接入层将数据转换到 HTTP 1.1 后再转发到对应域名的服务器即可。
- 3）、当所有请求都集中在一条连接中时，在网络拥塞时容易出现 TCP 队首阻塞问题。
- 4）、在文件下载、视频播放等场景下可能会遇到三方服务器单连接限速的问题，此时可以禁用 HTTP 2.0。


## 4、协议版本升级

### HTTP 1.0

TCP 连接不复用，也就是每发起一个网络请求都要重新建立连接，而刚开始连接都会经历一个慢启动的过程，可谓是慢上加慢，因此 HTTP 1.0 性能非常差。

### HTTP 1.1

引入了持久连接，即 TCP 连接可以复用，但数据通信必须按次序来，也就是后面的请求必须等前面的请求完成才能进行。当所有请求都集中在一条连接中时，在网络拥塞时容易出现 TCP 队首阻塞问题。

### HTTP 2

- 二进制协议
- 多工
- 服务端与客户端可以双向实时通信。


### QUIC

Google 2013 实现，2018 基于 QUIC 协议的 HTTP 被确认为 HTTP3。

QUIC 简单理解为 HTTP/2.0 + TLS 1.3 + UDP。弱网环境下表现好与 TCP。

#### 优势

- 1）、解决了在连接复用中 HTTP2 + TCP 存在的队首阻塞问题，
- 2）、由于是基于 UDP，所以可以灵活控制拥塞协议。例如 Client 端可以直接使用 Google 的 [BBR 算法](https://queue.acm.org/detail.cfm?id=3022184)。
- 3）、连接迁：由于 UDP 通过类似connection id 的特性,使得客户端网络切换的时候不需要重连，用户使用 App 的体验会更加流畅。


#### 目前的缺点

- 1）、NAT 局域网路由、交换机、防火墙等会禁止 UDP 443 通行，因此 QUIC 创建连接成功率只有95%。
- 2）、运营商针对 UDP 通道不支持/支持不足。
- 3）、使用 UDP 不一定会比 TCP 更快，客户端可同时使用 TCP 和 QUIC 竞速，从而选择更优链路。


#### 使用场景

- 1）、实时性
- 2）、可丢弃
- 3）、请求互相依赖
- 4）、可同时使用 TCP & QUIC


#### QUIC 加密协议原理

![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc56664e2a6b?w=1332&h=398&f=png&s=322001)


- 1）、当 Client 与 Server 第一次通信时，会发送 Inchoate Client Hello 消息下载 Server Config（SCFG) 暂存消息。
- 2）、SCFG 中包含一个 Diffie-Hellman 共享，下一次 Client 将使用它派生初始密钥（即 0-RTT 密钥）并利用其加密数据给 Server。
- 3）、之后，Server 将发出一个新的暂存 Diffie-Hellman 共享，并由此派生出一组 前向安全密钥去进行数据的加密通信。


## 5、网络请求质量监控

### 1）、接口请求耗时、成功率、错误码

在 Awesome-WanAndroid 中已经使用 OkHttpEventListener 实现了网络请求的质量监控，其代码如下所示：

```java
// 网络请求质量监控
builder.eventListenerFactory(OkHttpEventListener.FACTORY);

/**
 * FileName: OkHttpEventListener
 * Date: 2020/5/8 16:28
 * Description: OkHttp 网络请求质量监控
 * @author quchao
 */
public class OkHttpEventListener extends EventListener {

    public static final Factory FACTORY = new Factory() {
        @Override
        public EventListener create(Call call) {
            return new OkHttpEventListener();
        }
    };

    OkHttpEvent okHttpEvent;
    public OkHttpEventListener() {
        super();
        okHttpEvent = new OkHttpEvent();
    }

    @Override
    public void callStart(Call call) {
        super.callStart(call);
        LogHelper.i("okHttp Call Start");
        okHttpEvent.callStartTime = System.currentTimeMillis();
    }

    /**
     * DNS 解析开始
     *
     * @param call
     * @param domainName
     */
    @Override
    public void dnsStart(Call call, String domainName) {
        super.dnsStart(call, domainName);
        okHttpEvent.dnsStartTime = System.currentTimeMillis();
    }

    /**
     * DNS 解析结束
     *
     * @param call
     * @param domainName
     * @param inetAddressList
     */
    @Override
    public void dnsEnd(Call call, String domainName, List<InetAddress> inetAddressList) {
        super.dnsEnd(call, domainName, inetAddressList);
        okHttpEvent.dnsEndTime = System.currentTimeMillis();
    }

    @Override
    public void connectStart(Call call, InetSocketAddress inetSocketAddress, Proxy proxy) {
        super.connectStart(call, inetSocketAddress, proxy);
        okHttpEvent.connectStartTime = System.currentTimeMillis();
    }

    @Override
    public void secureConnectStart(Call call) {
        super.secureConnectStart(call);
        okHttpEvent.secureConnectStart = System.currentTimeMillis();
    }

    @Override
    public void secureConnectEnd(Call call, @Nullable Handshake handshake) {
        super.secureConnectEnd(call, handshake);
        okHttpEvent.secureConnectEnd = System.currentTimeMillis();
    }

    @Override
    public void connectEnd(Call call, InetSocketAddress inetSocketAddress, Proxy proxy, @Nullable Protocol protocol) {
        super.connectEnd(call, inetSocketAddress, proxy, protocol);
        okHttpEvent.connectEndTime = System.currentTimeMillis();
    }

    @Override
    public void connectFailed(Call call, InetSocketAddress inetSocketAddress, Proxy proxy, @Nullable Protocol protocol, IOException ioe) {
        super.connectFailed(call, inetSocketAddress, proxy, protocol, ioe);
    }

    @Override
    public void connectionAcquired(Call call, Connection connection) {
        super.connectionAcquired(call, connection);
    }

    @Override
    public void connectionReleased(Call call, Connection connection) {
        super.connectionReleased(call, connection);
    }

    @Override
    public void requestHeadersStart(Call call) {
        super.requestHeadersStart(call);
    }

    @Override
    public void requestHeadersEnd(Call call, Request request) {
        super.requestHeadersEnd(call, request);
    }

    @Override
    public void requestBodyStart(Call call) {
        super.requestBodyStart(call);
    }

    @Override
    public void requestBodyEnd(Call call, long byteCount) {
        super.requestBodyEnd(call, byteCount);
    }

    @Override
    public void responseHeadersStart(Call call) {
        super.responseHeadersStart(call);
    }

    @Override
    public void responseHeadersEnd(Call call, Response response) {
        super.responseHeadersEnd(call, response);
    }

    @Override
    public void responseBodyStart(Call call) {
        super.responseBodyStart(call);
    }

    @Override
    public void responseBodyEnd(Call call, long byteCount) {
        super.responseBodyEnd(call, byteCount);
        // 记录响应体的大小
        okHttpEvent.responseBodySize = byteCount;
    }

    @Override
    public void callEnd(Call call) {
        super.callEnd(call);
        okHttpEvent.callEndTime = System.currentTimeMillis();
        // 记录 API 请求成功
        okHttpEvent.apiSuccess = true;
        LogHelper.i(okHttpEvent.toString());
    }

    @Override
    public void callFailed(Call call, IOException ioe) {
        LogHelper.i("callFailed ");
        super.callFailed(call, ioe);
        // 记录 API 请求失败及原因
        okHttpEvent.apiSuccess = false;
        okHttpEvent.errorReason = Log.getStackTraceString(ioe);
        LogHelper.i("reason " + okHttpEvent.errorReason);
        LogHelper.i(okHttpEvent.toString());
    }
}
```


成功 log 如下所示：

```java
2020-05-11 11:00:42.678 6682-6847/json.chao.com.wanandroid D/OkHttp: --> GET https://www.wanandroid.com/banner/json
2020-05-11 11:00:42.687 6682-6848/json.chao.com.wanandroid I/WanAndroid-LOG: ┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────
2020-05-11 11:00:42.688 6682-6848/json.chao.com.wanandroid I/WanAndroid-LOG: │ Thread: RxCachedThreadScheduler-3
2020-05-11 11:00:42.688 6682-6848/json.chao.com.wanandroid I/WanAndroid-LOG: ├┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄
2020-05-11 11:00:42.688 6682-6848/json.chao.com.wanandroid I/WanAndroid-LOG: │ OkHttpEventListener.callStart  (OkHttpEventListener.java:46)
2020-05-11 11:00:42.688 6682-6848/json.chao.com.wanandroid I/WanAndroid-LOG: │    LogHelper.i  (LogHelper.java:37)
2020-05-11 11:00:42.688 6682-6848/json.chao.com.wanandroid I/WanAndroid-LOG: ├┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄
2020-05-11 11:00:42.688 6682-6848/json.chao.com.wanandroid I/WanAndroid-LOG: │ [OkHttpEventListener.java | 46 | callStart] okHttp Call Start
2020-05-11 11:00:42.688 6682-6848/json.chao.com.wanandroid I/WanAndroid-LOG: └────────────────────────────────────────────────────────────────────────────────────────────────────────────────
2020-05-11 11:00:43.485 6682-6847/json.chao.com.wanandroid D/OkHttp: <-- 200 OK https://www.wanandroid.com/banner/json (806ms, unknown-length body)
2020-05-11 11:00:43.496 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: ┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────
2020-05-11 11:00:43.498 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │ Thread: RxCachedThreadScheduler-2
2020-05-11 11:00:43.498 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: ├┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄
2020-05-11 11:00:43.498 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │ OkHttpEventListener.callEnd  (OkHttpEventListener.java:162)
2020-05-11 11:00:43.498 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │    LogHelper.i  (LogHelper.java:37)
2020-05-11 11:00:43.498 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: ├┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄
2020-05-11 11:00:43.498 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │ [OkHttpEventListener.java | 162 | callEnd] NetData: [
2020-05-11 11:00:43.498 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │ callTime: 817
2020-05-11 11:00:43.498 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │ dnsParseTime: 6
2020-05-11 11:00:43.498 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │ connectTime: 721
2020-05-11 11:00:43.499 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │ secureConnectTime: 269
2020-05-11 11:00:43.499 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │ responseBodySize: 975
2020-05-11 11:00:43.499 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │ apiSuccess: true
2020-05-11 11:00:43.499 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: │ ]
2020-05-11 11:00:43.499 6682-6847/json.chao.com.wanandroid I/WanAndroid-LOG: └────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```


### 2）、根据网络质量来动态设定网络服务的重要参数（超时、并发线程数）

- 根据用户 2G/3G/4G/WIFI 的网络环境。
- 根据用户当前网络的 RTT。


## 6、压缩

### 1）、header（HTTP 2.0 头部压缩）

见 [深入探索 Android 网络优化（二、网络优化基础篇）下 - 首部压缩](https://juejin.im/post/5ecda2b6f265da77084772a1#heading-55)。

### 2）、URL

不变参数客户端只需上传以此，其它参数均在接入层进行扩展。


### 3）、body

使用 Protocol Buffers 替代 JSON 序列化。


### 4）、图片

- 1）、webp
- 2）、hevc
- 3）、SharpP
- 4）、基于 AI 的图片超清化
    - 深度学习 CNN（Convolutional Neural Network，卷积神经网络）。
    - CNN 学习到的是高分辨率图像和低分辨率图像的差。


### 5）、压缩算法

- 1）、GZIP
- 2）、[Google Brotli](https://github.com/google/brotli)
- 3）、[Facebook Z-standard（推荐）](https://github.com/facebook/zstd)：通过业务数据样本训练处合适的字典，因此是压缩率最好的算法，由于各业务线维护字典成本较大，可以在网络平台的统一接入层进行压缩与解压。我们可以抽样1%的数据来训练字典，而字典的下发与更新由统一接入层负责。


## 7、加密

HTTPS 通常需要多消耗 2 RTT 的协商时延。

### 1）、HTTPS 优化

#### 1、提高连接复用率

- 1）、多个域名共用同一个 HTTP2 连接。
- 2）、长连接。


#### 2、减少握手次数（TLS 1.3 实现 0 RTT 协商）

TLS 1.2 引入了 SHA-256 哈希算法，摒弃了 SHA-1，对增强数据完整性有着显著优势。

IETF（Internet Engineering Task Froce，互联网工程任务组）制定的 TLS 1.3 是有史以来最安全、复杂的 TLS 协议。它具有如下特点：

##### 1）、更快的访问速度

相比于 TLS 1.2 及之前的版本，TLS 1.3 的握手不再支持静态的 RSA 密钥交换，使用的是带有前向安全的 Diffie-Hellman 进行全面握手。因此 TLS 1.3 只需 1-RTT 握手时间。

##### 2）、更强的安全性

删除了之前版本的不安全的加密算法。

- 1）、RSA 密钥传输：不支持前向安全性。
- 2）、CBC 模式密码：易受 BEAST 和 Lucky 13 攻击。
- 3）、RC4 流密码：在 HTTPS 中使用并不安全。
- 4）、SHA-1 哈希函数：建议以 SHA-2 替代。
- 5）、任意 Diffie-Hellman 组：CVE-2016-0701 漏洞。
- 6）、输出密码：易受 FREAK 和 LogJam 攻击。


此外，我们可以在 [Google 浏览器设置 TLS 1.3](chrome://flags/#tls13-hardening-for-local-anchors)。

#### 3、slight-ssl

参考 TLS 1.3 协议，合并请求，优化加密算法，使用 session-ticket 等策略，力求在安全和体验间找到一个平衡点。

在 TLS 中性能开销最大的是 TLS 握手阶段的 RSA 加解密。在 slight-ssl 中又尝试如下几种解决方案：

- 1）、硬件加速：使用单独的硬件加速卡处理 RSA 加解密。
- 2）、ECDSA：ECSDSA 最底层的算法和成本对性能的消耗远低于 RSA，相差5~6倍。
- 3）、Session Ticket 机制：将 TLS 握手从 2RTT 降低为 1RTT。



#### 4、微信 mmtls 原理

基于 TLS 1.3 草案标准而实现。

类似于 TLS 协议，mmtls 协议也是位于业务层与网络连接层中间。

##### mmtls 协议组成图

![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc5a1f6b416e?w=1352&h=436&f=png&s=161442)


- 1）、Handshake、Alert 和 Application Protocol 都是 record 协议的上层协议。
- 2）、Record 协议包中有字段用于区分器上层协议是上述3种任一协议。
- 3）、在 mmtls/TLS 中Handshake 子协议负责密钥协商， Record 子协议负责数据对称加密传输。除了性能与效率的因素之外，更利于隔离复杂性。


##### Handshake 协议

TLS 1.3 Handshake 协议有如下几类：

- 1-RTT 密钥协商方式
    - 1-RTT ECDHE
    - 1-RTT PSK（Pre-Shared Key）
- 0-RTT 密钥协商方式
    - 0-RTT PSK
    - 0-RTT ECDH
    - 0-RTT PSK-ECDHE
    - 0-RTT ECDH-ECDHE


而 mmtls Handshake 协议有如下几种：

- 1-RTT ECDHE
- 1-RTT PSK
- 0-RTT PSK


**1-RTT ECDHE 密钥协商原理**

ECDH 密钥交换协议需要使用两个算法：

- 1）、密钥生成算法 ECDH_Generate_Key：生成公私钥对（ECDH_pub_key、ECDH_pri_key)，其中保存私钥，将公钥互相发送给对方。
- 2）、密钥协商算法 ECDH_compute_key：输入对方公钥与自身私钥，计算出通信双方一致的对称密钥 Key。


但是 1-RTT ECDHE 算法容易被中间人攻击，中间人可以截获双方的公钥运行 ECDH_Generate_key 生成自己的公私钥对，然后将公钥发送给某一方。


> 如何解决中间人攻击？


中间人攻击产生的本质原因是没有经过端点认证，需要”带认证的密钥协商“。


> 数据认证的方式？

数据认证有对称与非对称两种方式：

- 1）、基于 MAC（Message Authentication Code，消息认证码）的对称认证
- 2）、基于签名算法的非对称认证。


ECDH 认证密钥协商就是 ECDH 密钥协商 + 数字签名算法 ECDSA。

双方密钥协商会对自身发出的公钥使用签名算法，由于签名算法中的公钥 ECDSA_verify_key 是公开的，中间人没有办法阻止别人获取公钥。

而 mmtls 仅对 Server 做认证，因为通信一方签名其协商数据就不会被中间人攻击。

在 TLS 中，提供了可选的双方相互认证的能力：

- Client 通过选择 CipherSuite 是什么类型来决定是否要对 Server 进行认证。
- Server 通过是否发送 CertificateRequest 握手消息来决定是否要对 Client 进行认证。


**1-RTT PSK 密钥协商原理**

![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc5d11dde4ff?w=1350&h=632&f=png&s=224751)


在之前的 ECDH 握手下，Server 会下发加密的 PSK{key, ticket{key}}，其中：

- key：用来做对称加密密钥的 key 明文。
- ticket{key}：用 server 私密保存的 ticket_key 对 key 进行加密的密文 ticket。


1）、首先，Client 将 ticket{key}、Client_Random 发送给 Server。

2）、然后，Server 使用 ticket_key 解密得到 key、Server_Random、Client_Random 计算 MAC 来认证。

3）、最后，Server 将 Server_Random、MAC 发送给 Client，Client 同 Server 使用 ticket_key 解密得到 key、Server_Random、Client_Random 去计算 MAC 来验证是否与收到的 MAC 匹配。


**0-RTT ECDH 密钥协商原理**

![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc621d0a35e9?w=1342&h=560&f=png&s=212862)


要想实现 0-RTT 密钥协商，就必须在协商一开始就将业务数据安全地传递到对端。

预先生成一对公私钥（static_svr_pub_key, static_svr_pri_key)，并将公钥预置在 Client，私钥持久保存在 Server。

1）、首先，Client 通过 static_svr_pub_key 与 cli_pri_key 生成一个对称密钥SS（Static Secret），用 SS 衍生的密钥对业务数据加密。

2）、然后，Client 
cli_pub_key、Client_Random、SS 加密的 AppData 发送给 Server，Sever 通过 cli_pub_key 和 static_svr_pri_key 算出 SS，解密业务数据包。


**1-RTT PSK 密钥协商原理**

在进行 1-RTT PSK 握手之前，Client 已经有一个对称加密密钥 key 了，直接使用此 key 与 ticket{key} 一起传递给 Server 即可。


> TLS 1.3 为什么要废除 RSA？

- 1）、2015年发现了 FREAK 攻击，出现了 RSA 漏洞。
- 2）、一旦私钥泄露，中间人就可以通过私钥计算出之前所有报文的密钥，破解之前所有的密文。


因此 TLS 1.3 引入了 PFS（perfect forward secrecy，前向安全性），即完全向前保密，一个密钥被破解，并不会影响其它密钥的安全性。

例如 0-RTT ECDH 密钥协商加密依赖了静态 static_svr_pri_key，不符合 PFS，我们可以使用 0-RTT ECDH-ECDHE 密钥协商，即进行 0-RTT ECDH 协商的过程中也进行 ECDHE 协商。0-RTT PSK 密钥协商的静态 ticket_key 同理也可以加入 ECDHE 协商。


> verify_key 如何下发给客户端？

为避免证书链验证带来的时间消耗及传输带来的带宽消耗，直接将 verify_Key 内置客户端即可。


> 如何避免签名密钥 sign_key 泄露带来的影响？

因为 mmtls 内置了 verify_key 在客户端，必要时及时通过强制升级客户端的方式来撤销公钥并更新。


> 为什么要在上述密钥协商过程中都要引入 client_random、server_random、svr_pub_key 一起做签名？

因为 svr_pri_Key 可能会泄露，所有单独使用 svr_pub_key 时会有隐患，因为需要引入 client_random、server_random 来保证得到的签名值唯一对应一次握手。


##### Record 协议

**1、认证加密**

- 1）、使用对称密钥进行安全通信。
- 2）、加密 + 消息认证码：Encrypt-then-MAC
- 3）、TLS 1.3 只使用 AEAD（Authenticated-Encryption With Addtional data）类算法：Encrypt 与 MAC 都集成在一个算法内部，让有经验的密码专家在算法内部解决安全问题。
- 4）、mmtls 使用 AES-GCM 这种 AEAD 类算法。


**2、密钥扩展**

双方使用相同的对称密钥进行加密通信容易被某些对称密钥算法破解，因此，需要对原始对称密钥做扩展变换得到相应的对称加密参数。

密钥变长需要使用密钥延时函数（KDF，Key Derivation Function），而 TLS 1.3 与 mmtls 都使用了 HKDF 做密钥扩展。

**3、防重放**

为解决防重放，我们可以为连接上的每一个业务包都添加一个递增的序列号，只要 Server 检查到新收到的数据包的序列号小于等于之前收到的数据包的序列号，就判断为重放包，mmtls 将序列号作为构造 AES-GCM 算参数 nonce 的一部分，这样就不需要对序列号单独认证。

在 0-RTT 握手下，第一个业务数据包和握手数据包无法使用上述方案，此时需要客户端在业务框架层去协调支持防重放。

##### 小结

mmtls 的 **工作过程** 如下所示：

- 1）、使用 ECDH 做密钥协商。
- 2）、使用 ECDSA 进行签名认证。
- 3）、使用 AES-GCM 对称加密算法对业务数据进行加密。
- 4）、使用 HKDF 进行密钥扩展。
- 5）、使用的摘要算法为 SHA256。


其优势具有如下4点：

- 1）、轻量级：去除客户端认证，内置签名公钥，减少验证时网络交换次数。
- 2）、安全性：TLS 1.3 推荐安全性最高的基础密码组件，0-RTT 防重放由服务端、客户端框架层协同处理。
- 3）、高性能：使用了 0-RTT 握手，优化了 TLS 1.3 中的握手方式和密钥扩展方式。
- 4）、高可用：服务器添加了过载保护，确保其能在容灾模式下提供安全级别稍低的有损服务。


#### 3）、复用 Session Ticket 会话，节省一个 RTT 耗时。


最后，我们可以在统一接入层对传输数据二次加密，需要注意二次加密会增加客户端与服务器的处理耗时。


> 如果手机设置了代理，TLS 加密的数据可以被解开并被利用，如何处理？

可以在 [客户端锁定根证书](https://github.com/WooyunDota/DroidDrops/blob/master/2018/SSL.Pinning.Practice.md#%E5%AE%A2%E6%88%B7%E7%AB%AF%E7%B3%BB%E7%BB%9F%E8%AF%81%E4%B9%A6%E9%94%81%E5%AE%9A-system-ca-pinning)，可以同时兼容老版本与保证证书替换的灵活性。


## 8、网络容灾机制

- 1）、备用服务器分流。
- 2）、多次失败后一定时间内不进行请求，避免雪崩效应。


## 9、资本手段优化

- 1）、CDN 加速，更新后需要记住清理缓存
- 2）、提高带宽
- 3）、动静资源分离
- 4）、部署跨国的专线、加速点
- 5）、多 IDC 就进接入
- 6）、P2P 技术


# 六、网络库设计

## 1、统一的网络中台

在一线互联网公司，都会有统一的网络中台：

- 负责提供前后台一整套的网络解决方案。
- 网关用于解决中间网络的通讯，为上层服务提供高质量的双向通讯能力。


## 2、如何设计一个优秀的统一网络库？

- 1）、统一 API：统一的策略管理、流解析（兼容JSON、XML、Protocol Buffers）等
- 2）、全局网络控制：统一的网络调度、流量监控、容灾管理等
- 3）、高性能：速度、CPU、内存、I/O、失败率、崩溃率、协议兼容性等


## 3、统一网络库的核心模块有哪些？

- 1）、DNS 管理
- 2）、连接管理
- 3）、协议处理
- 4）、并发模型
- 5）、IO 模型
- 6）、预连接
- 7）、错误兼容处理
- 8）、数据解析
- 9）、网络质量监控
- 10）流量监控
- 11）、代理 WebView 网络请求

## 4、高质量网络库

### 1）、Chromium 网络库

- Google 出品，我们可以基于 Chromium 网络库二次开发自己的网络库， 以便享受 Google 后续网络优化的成果，例如 TLS 1.3、QUIC 支持等等。
- 跨平台。
- 需要补足 Mars 的 弱网/连接优化 功能。
- 自定义协议：改造 TLS，将 RSA 更换为 ECDHE，以提升加解密速度。


### 2）、微信 Mars

一个跨平台的 Socket 层解决方案，不支持完整的 HTTP 协议。

Mars 的两个核心模块如下：

- SDT：网络诊断模块
- STN：信令传输模块，适合小数据传输。


其中 STN 模块的组成图如下所示：

![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc65caf1f970?w=2262&h=832&f=png&s=525700)


#### 包包超时

- 每次读取或发送的间隔。
- 获取 sock snd buf 内未发送的数据。
- Android：ioctl 读取 SIOCOUTQ。
- iOS：getsockopt 读取 SO_NWRITE。


#### 动态超时

根据网络情况，调整其它超时的系数或绝对值。


> Mars 是如何进行 连接优化 的？

#### 复合连接

每间隔几秒启动一个新的连接，只要有连接建立成功，则关闭其它连接。=> 有效提升连接成功率。

#### 自动重连优化

- 1）、减少无效等待时间，增加重试次数。
- 2）、但 TCP 层的重传间隔过大时，此时断连重连，能够让 TCP 层保持积极的重连间隔，以提高成功率。
- 3）、当链路存在较大波动或严重拥塞时，通过更换连接以获得更好的性能。


#### 网络切换

通过感知网络的状态切换到更好的网络环境下。


> Mars 是如何进行 弱网优化 的？

#### 常规方案

##### 1）、快速重传

- 减小重传成本（SACK、FEC）
- 尽早发现重传（DUP ACK、FACK、RTO、NACK）


##### 2）、HARQ（Hybrid Automatic Repeat reQuest）

- 3 GPP 标准方案。
- 增加并发度。
- 尽量准确避免拥堵（丢包和拥堵的区别）。


#### 进阶方案

##### TCP 丢包的恢复方式 TLP

- 1、PTO 触发尾包重传。
- 2、尾包的 ACK 带上 SACK 信息。
- 3、SACK 触发 FACK 快速重传和恢复。
- 4、避免了 RTO 导致的慢启动和延迟。


##### 发图-有损下载

在弱网下尽量保证下载完整的图片轮廓显示，提高用户体验。


![](https://user-gold-cdn.xitu.io/2020/6/7/1728dc6873a56c11?w=1694&h=752&f=png&s=1333848)


#### 发图-有损上传数据

- 在弱网下尽量保证上传完整的图片轮廓显示，提高用户体验。
- 能够降低客户端上传失败率 10% 以上。


#### 有损上传数据的流程，有损下载流程同理

- 1）、发送渐进式图片（例如 JPG 等）。
- 2）、服务器接收数据且回复数据确认包。
- 3）、当数据足够时（50%），回复发送成功确认包。
- 4）、发送方继续补充数据
    - 网络正常，数据完整。
    - 网络异常，认为已发送成功。
- 5）、服务器通知发送者。


#### 发图-低成本重传

将分包转成流式传输。

- 1）、分包
    - 降低包大小
    - 增加并发
    - 包头损耗
- 2）、流式
    确认粒度策略灵活
    单线程


# 七、其它优化方案

## 1、异地多活

一个多机房的整体方案，在多个地区同时存在对等的多个机房，以用户维度划分，多机房共同承担全量用户的流量。

在单个机房发送故障时，故障机房的流量可以快速地被迁引到可用机房，减少故障的恢复时间。


## 2、抗抖动优化

应用一种有策略的重试机制，将网络请求以是否发送到 socket 缓冲区作为分割，将网络请求生命周期划分为”请求开始到发送到 socket 缓冲区“和”已经发送到 socket 缓冲区到请求结束“两个阶段。

这样当用户进电梯因为网络抖动的原因网络链接断了，但是数据其实已经请求到了 socket 缓冲区，使用这种有策略的重试机制，我们就可以提升客户端的网络抗抖动能力。


## 3、SYNC 机制

同步差量数据，达到节省流量，提高通信效率与请求成功率。

客户端用户不在线时，SYNC 服务端将差量数据保持在数据库中。当客户端下次连接到服务器时，再同步差量数据给用户。


## 4、高并发流量处理：服务端接入层多级限流

核心思想是保障核心业务在体验可接受范围内做降级非核心功能和业务。从入口到业务接口总共分为四个层级，如下所示：

- 1）、LVS（几十亿级）：多 VIP 多集群。
- 2）、接入网关（亿级）：TCP 限流、核心 RPC 限流。
- 3）、API 网关（千万级）：分级限流算法（对不同请求量的接口使用不同的策略）
    - 高 QPS 限流：简单基数算法，超过这个值直接拒绝。
    - 中 QPS 限流：令牌桶算法，接受一定的流量并发。
    - 低 QPS 限流：分布式限流，保障限流的准确。
- 4）、业务接口（百万级）
    - 返回定制响应、自定义脚本。
    - 客户端静默、Alert、Toast。


## 5、JobScheduler

结合 JobScheduler 来根据实际情况做网络请求. 比方说 Splash 闪屏广告图片, 我们可以在连接到 Wifi 时下载缓存到本地; 新闻类的 App 可以在充电, Wifi 状态下做离线缓存。


## 6、网络请求优先级排序

app应该对网络请求划分优先级尽可能快地展示最有用的信息给用户。（高优先级的服务优先使用长连接）

立刻呈现给用户一些实质的信息是一个比较好的用户体验，相对于让用户等待那些不那么必要的信息来说。这可以减少用户不得不等待的时间，增加APP在慢速网络时的实用性。（低优先级使用短连接）


## 7、建立长连通道

### 实现原理

将众多请求放入等待发送队列中，待长连通道建立完毕后再将等待队列中的请求放在长连通道上依次送出。

### 关键细节

HTTP 的请求头键值对中的的键是允许相同和重复的。例如 Set-Cookie/Cookie 字段可以包含多组相同的键名称数据。在长连通信中，如果对 header 中的键值对用不加处理的字典方式保存和传输，就会造成数据的丢失。


## 8、减少域名和避免重定向。

## 9、没有请求的请求，才是最快的请求。


# 七、网络体系化方案建设

## 1、线下测试

### 1）、正确认识

尽可能将问题在上线前暴露出来。

### 2）、侧重点

- 1）、请求有误、多余
- 2）、网络切换
- 3）、弱网
- 4）、无网


## 2、线上监控

### 1）、服务端监控

#### 宏观监控维度

##### 1）、请求耗时

区分地域、时间段、版本、机型。

##### 2）、失败率

业务失败与请求失败。

##### 3）、Top 失败接口、异常接口

以便进行针对性地优化。

#### 微观监控维度

##### 1）、吞吐量（requests per second）

RPS/TPS/QPS，每秒的请求次数，服务器最基本的性能指标，RPS 越高就说明服务器的性能越好。

##### 2）、并发数（concurrency）

反映服务器的负载能力，即服务器能够同时支持的客户端数量，越大越好。

##### 3）、响应时间（time per request）

反映服务器的处理能力，即快慢程度，响应时间越短越好。

##### 4）、操作系统资源

CPU、内存、硬盘和网卡等系统资源。可以利用 top、vmstat 等工具检测相关性能。


#### 优化方针

- 1）、合理利用系统资源，提高服务器的吞吐量和并发数，降低响应时间。
- 2）、选用高性能的 Web 服务器，开启长连接，提升 TCP 的传输效率。


### 2）、客户端监控

要实现客户端监控，首先我们应该要统一网络库，而客户端需要监控的指标主要有如下三类：

- 1）、时延：一般我们比较关心每次请求的 DNS 时间、建连时间、首包时间、总时间等，会有类似 1 秒快开率、2 秒快开率这些指标。
- 2）、维度：网络类型、国家、省份、城市、运营商、系统、客户端版本、机型、请求域名等，这些维度主要用于分析问题。
- 3）、错误：DNS 失败、连接失败、超时、返回错误码等，会有 DNS 失败率、连接失败率、网络访问的失败率这些指标。

为了运算简单我们可以抛弃 UV，只计算每一分钟部分维度的 PV。


#### 1、Aspect 插桩 — ArgusAPM

关于 ArgusAPM 的网络监控切面源码分析可以参考我之前写的 [深入探索编译插桩技术（二、AspectJ） - 使用 AspectJ 打造自己的性能监控框架](https://juejin.im/post/5e84384af265da47e1593102#heading-26)


##### 缺点

监控不全面，因为 App 可能不使用系统/OkHttp 网络库，或是直接使用 Native 网络请求。

#### 2、Native Hook

需要 Hook 的方法有三类：

- 1）、连接相关：connect
- 2）、发送数据相关：send 和 sendto。
- 3）、接收数据相关：recv 和 recvfrom。


不同版本 Socket 的实现逻辑会有差异，为了兼容性考虑，我们直接 PLT Hook 内存所有的 so，但是需要排除掉 Socket 函数本身所在的 libc.so。其 PLT 的 Hook 代码如下所示：


```java
hook_plt_method_all_lib("libc.so", "connect", (hook_func) &create_hook);
hook_plt_method_all_lib("libc.so, "send", (hook_func) &send_hook);
hook_plt_method_all_lib("libc.so", "recvfrom", (hook_func) &recvfrom_hook);
```


下面，我们使用 PLT Hook 来获取网络请求信息。


> [项目地址](https://github.com/AndroidAdvanceWithGeektime/Chapter17)


其成功 log 如下所示：


```
2020-05-21 15:10:37.328 27507-27507/com.dodola.socket E/HOOOOOOOOK: JNI_OnLoad
2020-05-21 15:10:37.328 27507-27507/com.dodola.socket E/HOOOOOOOOK: enableSocketHook
2020-05-21 15:10:37.415 27507-27507/com.dodola.socket E/HOOOOOOOOK: hook_plt_method
2020-05-21 15:10:58.484 27507-27677/com.dodola.socket E/HOOOOOOOOK: socket_connect_hook sa_family: 10
2020-05-21 15:10:58.495 27507-27677/com.dodola.socket E/HOOOOOOOOK: stack:com.dodola.socket.SocketHook.getStack(SocketHook.java:13)
libcore.io.Linux.connect(Native Method)
libcore.io.BlockGuardOs.connect(BlockGuardOs.java:126)
libcore.io.IoBridge.connectErrno(IoBridge.java:152)
libcore.io.IoBridge.connect(IoBridge.java:130)
java.net.PlainSocketImpl.socketConnect(PlainSocketImpl.java:129)
java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:356)
java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:200)
java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:182)
java.net.SocksSocketImpl.connect(SocksSocketImpl.java:357)
java.net.Socket.connect(Socket.java:616)
com.android.okhttp.internal.Platform.connectSocket(Platform.java:145)
com.android.okhttp.internal.io.RealConnection.connectSocket(RealConnection.java:141)
com.android.okhttp.internal.io.RealConnection.connect(RealConnection.java:112)
com.android.okhttp.internal.http.StreamAllocation.findConnection(StreamAllocation.java:184)
com.android.okhttp.internal.http.Strea
2020-05-21 15:10:58.495 27507-27677/com.dodola.socket E/HOOOOOOOOK: AF_INET6 ipv6 IP===>14.215.177.39:443
2020-05-21 15:10:58.495 27507-27677/com.dodola.socket E/HOOOOOOOOK: socket_connect_hook sa_family: 1
2020-05-21 15:10:58.495 27507-27677/com.dodola.socket E/HOOOOOOOOK: Ignore local socket connect
2020-05-21 15:10:58.523 27507-27677/com.dodola.socket E/HOOOOOOOOK: socket_connect_hook sa_family: 1
2020-05-21 15:10:58.523 27507-27677/com.dodola.socket E/HOOOOOOOOK: Ignore local socket connect
2020-05-21 15:10:58.806 27507-27677/com.dodola.socket E/HOOOOOOOOK: respond:<!DOCTYPE html>
<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=https://ss1.bdstatic.com/5eN1bjq8AAUYm2zgoY3K/r/www/cache/bdorz/baidu.min.css><title>百度一下，你就知道</title></head> <body link=#0000cc> <div id=wrapper> <div id=head> <div class=head_wrapper> <div class=s_form> <div class=s_form_wrapper> <div id=lg> <img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129> </div> <form id=form name=f action=//www.baidu.com/s class=fm> <input type=hidden name=bdorz_come value=1> <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden name=rsv_bp value=1> <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255 autocomplete=off autofocus=autofocus></span><span class="bg s_btn_wr"><input type=submit id=su value=百度一下 class="bg s_btn" autofocus></span> </form> </div> </div> <div id=u1> <a href=http://news.baidu.com name=tj_trnews class=mnav>新闻</a> <a href=https://www.hao123.com name=tj_trhao123 class=mnav>hao123</a> <a href=http://map.baidu.com name=tj_trmap class=mnav>地图</a> <a href=http://v.baidu.com name=tj_trvideo class=mnav>视频</a> <a href=http://tieba.baidu.com name=tj_trtieba class=mnav>贴吧</a> <noscript> <a href=http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 name=tj_login class=lb>登录</a> </noscript> <script>document.write('<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u='+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ '" name="tj_login" class="lb">登录</a>');
</script> <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">更多产品</a> </div> </div> </div> <div id=ftCon> <div id=ftConw> <p id=lh> <a href=http://home.baidu.com>关于百度</a> <a href=http://ir.baidu.com>About Baidu</a> </p> <p id=cp>©2017 Baidu <a href=http://www.baidu.com/duty/>使用百度前必读</a>  <a href=http://jianyi.baidu.com/ class=cp-feedback>意见反馈</a> 京ICP证030173号  <img src=//www.baidu.com/img/gs.gif> </p> </div> </div> </div> </body> </html>
```


此外，我们也可以使用爱奇艺提供的 [android_plt_hook](https://github.com/iqiyi/xHook/blob/master/docs/overview/android_plt_hook_overview.zh-CN.md) 来实现 PLT Hook。


##### 缺点

接管了系统的 Local Socket，需要在代码中增加过滤条件。


### 3）、接入层监控

> 为什么要做接入层监控？

- 1）、服务端更容易做到秒级的实时上报。
- 2）、仅靠客户端的监控数据并不完全可靠。

#### 监控维度

服务的入口和出口流量、服务端的处理时延、错误率等。


### 4）、监控报警

- 1）、秒级或者分钟级别的实时监控只有访问量（PV）、错误率等几个维度：最快速度发现问题。
- 2）、小时或者天级别的监控可以监控全部的维度：更好地定位出问题的区域。


> 监控的同时如何实现准确的自动化报警呢？


- 1）、基于规则，例如失败率与历史数据相比暴涨、流量暴跌等。
- 2）、基于时间序列算法或者神经网络的智能化报警，使用者不需要录入任何规则，只需有足够长的历史数据，就可以实现自动报警。


通常是两种结合使用。


## 3、异常监控体系搭建

### 1）、服务器防刷

超限拒绝访问。

### 2）、客户端

- 1）、大文件预警
- 2）、异常兜底策略：例如客户端超过5次连接失败，则设置更长的重试时间。


### 3）、单点问题追查

如果用户反馈 App 消耗的流量过多，或后台消耗流量较多，我们都可以具体地分析网络请求日志、以及下发命令查看具体时间段的流量、客户端线上监控 + 体系化方案建设 来实现单点问题的追查。


# 八、网络优化常见问题

## 1、在网络方面你们做了哪些监控，建立了哪些指标？

> 注意：体现演进的过程。

网络优化及监控我们刚开始并没有去做，因此我们在 APP 的初期并没有注意到网络的问题，并且我们通常是在 WIFI 场景下进行开发，所以并没有注意到网络方面的问题。

当 APP 增大后，用户增多，逐渐由用户反馈 界面打不开或界面显示慢，也有用户反馈我们 APP 消耗的流量比较多。在我们接受到这些反馈的时候，我们没有数据支撑，无法判断用户反馈是不是正确的。同时，我们也不知道线上用户真实的体验是怎样的。所以，我们就 **建立了线上的网络监控，主要分为 质量监控与流量监控**。

### 1）、质量监控

首先，最重要的是接口的请求成功率与每步的耗时，比如 DNS 的解析时间、建立连接的时间、接口失败的原因，然后在合适的时间点上报给服务器。

### 2）、流量监控

首先，我们获取到了精准的流量消耗情况，并且在 APM 后台，可以下发指令获取用户在具体时间段的流量消耗情况。 => 引出亮点 => 前后台流量获取方案。
关于指标 => 网络监控。


## 2、怎么有效地降低用户的流量消耗？

> 注意：结合实际案例

### 1）、数据：缓存、增量更新（这一步减少了非常多的流量消耗）

首先，我们处理了项目当中展示数据相关的接口，同时，对时效性没那么强的接口做了数据的缓存，也就是一段时间内的重复请求直接走缓存，而不走网络请求，从而避免流量浪费。对于一些数据的更新，例如省市区域、配置信息、离线包等信息，我们 **加上版本号的概念，以实现每次更新只传递变化的数据，即实现了增量更新** => 亮点：离线包增量更新实现原理与关键细节。

### 2）、上传：压缩

然后，我们在上传流量这方面也做了处理，比如针对 POST 请求，我们对 Body 做了 GZip 压缩，而对于图片的发送，必须要经过压缩，它能够在保证清晰度的前提下极大地减少其体积。

### 3）、图片：缩略图、webp

对于图片展示，我们采用了不同场景展示不同图片的策略，比如在列表展示界面，我们只展示了缩略图，而到用户显示大图的时候，我们才去展示原图。 => 引出 webp 的使用策略。


## 3、用户反馈消耗流量多这种问题怎么排查？

首先，部分用户遇到流量消耗多的情况是肯定会存在的，因为线上用户非常多，每个人遇到的情况肯定是不一样的，比如有些用户他的操作路径比较诡异，可能会引发一些异常情况，因此有些用户可能会消耗比较多的流量。

### 1）、精准获取流量的能力

我们在客户端可以精确q地获取到流量的消耗，这样就给我们排查用户的流量消耗提供了依据，我们就知道用户的流量消耗是不是很多。

### 2）、所有请求大小及次数的监控

此外，通过网络请求质量的监控，我们知道了用户所有网络请求的次数与大小，通过大小和次数排查，我们就能知道用户在使用过程中遇到了哪些 bug 或者是执行了一些异常的逻辑导致重复下载，处于不断重试的过程之中。

### 3）、主动预警的能力

在客户端，我们发现了类似的问题之后，我们还需要配备主动预警的能力，及时地通知开发同学进行排除验证，通过以上手段，我们对待用户的反馈就能更加高效的解决，因为我们有了用户所有的网络请求数据。


## 4、系统如何知道当前 WiFi 有问题？

如果一个 WiFi 发送过数据包，但是没有收到任何的 ACK 回包，这个时候就可以初步判断当前的 WiFi 是有问题的。


# 九、总结

网络优化可以说是移动端性能优化领域中水最深的领域之一，要想做好网络优化必须具备非常扎实的技术功底与全链路思维。总所周知，对于一个工程师的技术评级往往是以他最深入的那一两个领域为基准，而不是计算其技术栈的平均值。因此，建议大家能找准一两个点，例如 网络、内存、NDK、Flutter，对其进行深入挖掘，以打造自身的技术壁垒。而笔者后续也会利用晚上的时间继续深入 **网络协议与安全** 的领域，开始持续不断地深入挖掘。


# 参考链接：
---

- 1、[慕课网之Top团队大牛带你玩转Android性能分析与优化 第八章 App网络优化](https://coding.imooc.com/learn/list/308.html)
- 2、[极客时间之Android开发高手课 网络优化](https://time.geekbang.org/column/article/80100)
- 3、《Android移动性能实战》第三章 网络
- 5、[极客时间之Web 协议详解与抓包实战](https://time.geekba4g.org/course/detail/100026801-100973)
- 5、[geektime-geekbang/geektime-webprotocol](https://github.com/geektime-geekbang/geektime-webprotocol)
- 6、[wanandroid 网络优化](https://www.wanandroid.com/article/query?k=%E7%BD%91%E7%BB%9C%E4%BC%98%E5%8C%96)
- 7、[chrome TLS1.3 设置](chrome://flags/#tls13-hardening-for-local-anchors)
- 8、[美团点评移动网络优化实践](https://mp.weixin.qq.com/s?__biz=MjM5NjQ5MTI5OA==&mid=2651746151&idx=2&sn=b21d43476f6a7154b71b008bc263ffb5&chksm=bd12b62a8a653f3caae12f0b20c46fc6855c063f83d8600ff9f9a7879beb70065748108beb2a&scene=38#wechat_redirect)
- 9、[蘑菇街 App Chromium 网络栈实践](https://www.infoq.cn/article/mogujie-app-chromium-network-layer?useSponsorshipSuggestions=true%2F)
- 10、[微信Mars — 移动互联网下的高质量网络连接探索.pdf](https://github.com/WeMobileDev/article/blob/master/%E5%BE%AE%E4%BF%A1Mars%20%E2%80%94%20%E7%A7%BB%E5%8A%A8%E4%BA%92%E8%81%94%E7%BD%91%E4%B8%8B%E7%9A%84%E9%AB%98%E8%B4%A8%E9%87%8F%E7%BD%91%E7%BB%9C%E8%BF%9E%E6%8E%A5%E6%8E%A2%E7%B4%A2.pdf)
- 11、[Mars -- 微信跨平台跨业务基础组件](https://github.com/Tencent/mars/wiki)
- 12、[阿里无线 11.11：手机淘宝移动端接入网关基础架构演进之路](https://www.infoq.cn/article/taobao-mobile-terminal-access-gateway-infrastructure)
- 13、[贾岛：蚂蚁金服亿级并发下的移动端到端网络接入架构解析](https://mp.weixin.qq.com/s/nz8Z3Uj9840KHluWjwyelw)
- 14、[携程 App 的网络性能优化实践](https://www.infoq.cn/article/how-ctrip-improves-app-networking-performance)
- 15、[百度App网络深度优化系列《一》DNS优化](https://mp.weixin.qq.com/s/iaPtSF-twWz-AN66UJUBDg)
- 16、[google/brotli](https://github.com/google/brotli)
- 17、[facebook/zstd](https://github.com/facebook/zstd)
- 18、[看得「深」、看得「清」—— 深度学习在图像超清化的应用](http://imgtec.eetrend.com/d6-imgtec/blog/2017-08/10143.html)
- 19、[TLS1.3 VS TLS1.2，让你明白TLS1.3的强大](https://zhuanlan.zhihu.com/p/44980381)
- 20、[基于TLS1.3的微信安全通信协议mmtls介绍](https://mp.weixin.qq.com/s/tvngTp6NoTZ15Yc206v8fQ)
- 21、[Facebook是如何大幅提升TLS连接效率的？](https://mp.weixin.qq.com/s?__biz=MzI4MTY5NTk4Ng==&mid=2247489465&idx=1&sn=a54e3fe78fc559458fa47104845e764b&source=41#wechat_redirect)
- 22、[SSL Pinning Practice](https://github.com/WooyunDota/DroidDrops/blob/master/2018/SSL.Pinning.Practice.md#%E5%AE%A2%E6%88%B7%E7%AB%AF%E7%B3%BB%E7%BB%9F%E8%AF%81%E4%B9%A6%E9%94%81%E5%AE%9A-system-ca-pinning)
- 23、[P2P如何将视频直播带宽降低75%？](https://mp.weixin.qq.com/s?__biz=MzI4MTY5NTk4Ng==&mid=2247489182&idx=1&sn=e892855fd315ed2f1395f05b765f9c4e&source=41#wechat_redirect)
- 24、[BBR: Congestion-Based Congestion Control](https://queue.acm.org/detail.cfm?id=3022184)
- 25、[QUIC在手机微博中的应用实践.pdf](https://github.com/thinkpiggy/qcon2018ppt/blob/master/QUIC%E5%9C%A8%E6%89%8B%E6%9C%BA%E5%BE%AE%E5%8D%9A%E4%B8%AD%E7%9A%84%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5.pdf)
- 26、[Hook Socket Sample](https://github.com/AndroidAdvanceWithGeektime/Chapter17)
- 27、[RFC 目录](https://tools.ietf.org/rfc/index)



# Contanct Me

##  ●  微信：

> 欢迎关注我的微信：`bcce5360`  

##  ●  微信群：

> **由于微信群人数过多，麻烦大家想进微信群的朋友们，加我微信拉你进群。**
        

##  ●  QQ群：

> 2千人QQ群，**Awesome-Android学习交流群，QQ群号：959936182**， 欢迎大家加入~


## About me

- ### Email: [chao.qu521@gmail.com]()
- ### Blog: [https://jsonchao.github.io/](https://jsonchao.github.io/)
- ### 掘金: [https://juejin.im/user/5a3ba9375188252bca050ade](https://juejin.im/user/5a3ba9375188252bca050ade)
    

### 很感谢您阅读这篇文章，希望您能将它分享给您的朋友或技术群，这对我意义重大。

### 希望我们能成为朋友，在 [Github](https://github.com/JsonChao)、[掘金](https://juejin.im/user/5a3ba9375188252bca050ade)上一起分享知识。



















