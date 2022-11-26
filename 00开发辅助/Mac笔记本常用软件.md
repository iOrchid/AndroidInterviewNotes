## Mac笔记本常用软件

> 常用MacOS软件搜集，便捷你的工作

| 软件名称                                 | 用途                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| Home Brew                                | 用于shell内管理软件的工具                                    |
| Chrome/Firefox/Opera                     | 浏览器                                                       |
| 迅雷/FreeDownloadManger/Flox/Downie/IDM  | 下载工具                                                     |
| ResilioSync/Syncthing                    | p2p的文件同步工具，可用作私有云盘搭建                        |
| JumpDesktop/Screen4/RemoteDesktopManager | 电脑远程工具                                                 |
| Ipscanner                                | 局域网ip查看工具                                             |
| LittleSnich/Hands On                     | 联网控制防火墙                                               |
| CleanMyMac/MacBooster                    | 系统优化工具                                                 |
| Dr.Antivirus/Antivirus one               | 趋势安全软件（有和谐版，所以收录，另avast/kaspersky也很好）  |
| Foldery/FolderDesigner                   | 文件夹美化工具，可以更改颜色或图标                           |
| Archiver                                 | 解压缩工具                                                   |
| CheatSheet                               | 查看当前软件的快捷键配置的工具                               |
| Bartender                                | 管理状态栏图标的工具                                         |
| Alfred4                                  | 快速搜索工具                                                 |
| TG pro                                   | 系统硬件温度检测工具                                         |
| command one                              | 增强的文件管理器，可替换finder                               |
| one Switch                               | 电脑便捷开关                                                 |
| TuxeraDiskManager/NTFS for Mac           | 用于读取移动硬盘的管理工具                                   |
| KeyCastr                                 | 用于显示当前键盘输入到屏幕上，提示作用。可用于录制视屏时候，快捷键操作的屏幕显示 |
| MultiTouch                               | 触控板的增强工具，管理手势操作                               |
| Typora                                   | markdown工具，真心推荐                                       |
| 欧路词典/金山词典/有道词典               | 英文翻译工具                                                 |
| Wps/Office                               | 文档office工具                                               |
| Xmind zen                                | 思维导图工具                                                 |
| OmniGraffle                              | 图形绘制工具                                                 |
| 印象笔记                                 | 笔记记录工具                                                 |
| Dash                                     | api文档管理                                                  |
| postman                                  | api调试工具                                                  |
| Charles/Debookee                         | 接口抓包工具                                                 |
| StarUML                                  | UML工具                                                      |
| Monodraw                                 | 简约好用的流程图工具                                         |
| Obsidian                                 | 知识笔记图谱工具                                             |
|                                          |                                                              |
| iTerms                                   | 终端工具                                                     |
| BeyondCompare                            | 文件对比工具                                                 |
| Navicat                                  | 数据库管理工具                                               |
| Expressions                              | 正则表达式检测工具                                           |
| pxcook                                   | 像素大厨，查看设计稿用                                       |
| PS/Sketch/Gifox/Pixelmator               | 图形图像工具                                                 |
| Photomate/PhotoSweeper X                 | 相册管理工具                                                 |
| screenflow                               | 录屏工具                                                     |
| A Better Finder Rename                   | 批量重命名工具                                               |
| Applocker                                | 应用锁                                                       |
| Gemini2                                  | 重复文件查找工具                                             |
| Qsapce                                   | 多文件窗口工具                                               |
| QtScrcpy                                 | Android控制工具                                              |
| TinyCal                                  | 简约清爽的日历工具                                           |
| PPet                                     | 无聊时解乏的小桌宠                                           |
| platelet                                 | 无聊时解乏的小桌宠，血小板                                   |
| Pock                                     | TouchBar的增强工具                                           |
| Qbserve                                  | Mac使用时间统计工具，分类统计                                |
| Timey 3                                  | 简单倒计时工具                                               |
| MacDroid/HandShaker                      | Mac链接Android的工具                                         |
| flux                                     | 屏幕自动色温调节                                             |
| iMazing                                  | iPhone手机管理工具                                           |
| colorful folders                         | 文件夹美化工具                                               |
| Sublime/Vscode                           | 编辑器工具                                                   |
| Axure                                    | 原型图工具                                                   |
| xnip                                     | 小巧好用的截图工具                                           |
| Snipaste                                 | 截图工具                                                     |
| Sip                                      | 取色板                                                       |



> **另：在最新的MacOS Catalina之后，好多破解软件会提示损坏，或者无法打开之类的问题**，可以使用下面三条命令基本解决问题
>
> <font color=red>有需要对应mac平台软件和谐版的小伙伴，也可以私信留言或者issue帮忙共享</font>

```shell
sudo spctl --master-disable # 这个是关闭软件签名的限制，相反enable是开启。
xattr -r -d com.apple.quarantine /Applications/App的名字.app# 这个命令基本用于修复提示损坏之类的无法打开破解软件的问题。
codesign --force --deep --sign - /Application/Appname.app# 这个用于处理软件被修改后，来个系统自签名，使之正常使用。
```