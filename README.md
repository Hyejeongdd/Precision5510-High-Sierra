每年的9月起才能拿到Apple的教育优惠 没办法了 只能提前在笔记本上黑苹果体验了

事实证明 就算出了 也莫得钱买 继续黑果吧

本文源地址为：https://hyejeong.cn/hackintosh5510

如果您在安装过程中出现了问题 欢迎发表Issues我会在看到后第一时间处理 谢谢

首先先感谢 @darkhandz @z65881120 @panruohuai @1148070455 @wmchris @Scottsanett 等大佬的分享

这是一篇完全针对新手向的教程 适用于XPS15 9550/Precision 5510

望您能耐心的看完文章 尽管他有些啰嗦

    2018/01/25  初版1.0
    2018/06/24  Release 1.0
    2018/07/04  Release 1.1

------------
## 机型配置

    机型：DELL Precision5510
    CPU：Intel i7 6820HQ Processor
    内存：Micron Technology DDR4 2133 16G
    硬盘：Samsung PM951 512G + HGST 1T 7200 Rpm
    显卡：Intel HD Graphics 530
    显示器：SHARP 3840x2160
    声卡：Realtek ALC298
    无线网卡：DW1830(BCM94360)
    BIOS: 1.7.0

## 注意事项

进入系统后 一定不要对系统账户进行操作 若丢失了管理员 目前的办法只能是格式化重装！

原机的Intel Wireless 8260AC无解 替换DW1830(BCM94360) TB 180入手




## 遗留问题

 Type-C部分设备无法驱动 已知DA200可以正常运行 绿联AX88772无法运作

## 准备清单

    MacOS High Sierra 10.13.5(17F77) with Clover 4512
    TransMAC 12.0
    Diskgenius x64
    一枚大于8G的USB3.0 U盘 如果你不想读条心态爆炸 建议你准备一个USB3.0的U盘
    一台备用的电脑 可以进行修改Plist和替换EFI的行为

## 动手前必须看的东西

黑苹果有一定的门槛和难度 如果你没有一点想动手的能力 劝你右上角点掉这个网页 避免让你感到糟心

如果你觉得接下来的操作对你有难度 而且你不愿意看如此长篇大论 还是请你关掉这个网页

------------

## 镜像写入

话不多说 开始操作

我所使用的镜像是MacOS High Sierra 10.13.5(17F77) with Clover 4512 当然 保险起见 还是希望你和我的版本一致

[镜像下载地址](https://blog.daliansky.net/macOS-High-Sierra-10.13.5-17F77-Release-Version-with-Clover-4512-original-mirror.html "镜像下载地址")

再次提醒！ 下载完请校对MD5值！下载完请校对MD5值！下载完请校对MD5值！

右键点击TransMac - 以管理员身份运行软件 ，找到准备制作的U盘，右键Restore with Disk Image，选择下载好的印象 等待几分钟 出现Restore Complete就说明完成制作了

<a href="https://img.hyejeong.cn/180123/X1.jpg">![](https://img.hyejeong.cn/180123/X1.jpg)</a>

写完镜像之后，U盘会变成两个分区，一个叫EFI的就是U盘引导区，里面存在镜像自带的Clover；另外一个叫U盘的提示你未格式化，这个时候千万不要手贱去格式化，这是因为Windows无法识别Mac的文件格式

打开Diskgenius，你可以大概浏览一下U盘EFI引导区目录结构：

<a href="https://img.hyejeong.cn/180123/X2.png">![](https://img.hyejeong.cn/180123/X2.png)</a>

制作好镜像后 在下面你可以找到Installer所需要的EFI文件 下载解压 用DiskGenius替换U盘里的EFI下的Clover文件夹 然后保存后 安全弹出U盘 强行移除可能会出现未知的错误

- 如果你和我一样是i7的CPU，可以进入下一步BIOS设置了。
- 如果你是i5的CPU，用Notepad++打开config.plist，搜索191b0000，改成19160000，保存。

在文章下面你可以找到Install所需要的EFI文件 下载解压 用DiskGenius替换U盘里的EFI下的Clover文件夹

保存后 安全弹出U盘

强行移除可能会出现未知的错误

------------

## BIOS设置

重启，开机狂按F2，直到DELL Logo下方出现蓝色进度条，进入后 执行以下操作

- Secure Boot - Secure Boot Enable里改成Disabled
- System Configuration - SATA Operation 改成 AHCI
- System Configuration - Miscellaneous Devices 去除 Enable Secure Digital(SD)Card

<a href="https://img.hyejeong.cn/MacOS/X1.jpg">![](https://img.hyejeong.cn/MacOS/X1.jpg)</a>

<a href="https://img.hyejeong.cn/MacOS/X2.jpg">![](https://img.hyejeong.cn/MacOS/X2.jpg)</a>

<a href="https://img.hyejeong.cn/MacOS/X3.jpg">![](https://img.hyejeong.cn/MacOS/X3.jpg)</a>

Warning: 若你原本是SATA Operation为Raid On 更改成AHCI后将会导致无法进入原系统

## 启动项设置

在 General - Boot Sequence，在右边列表找到一个UEFI: U盘型号, Partition 1这样的启动项，把它移动到最顶部，然后Apply，OK，Exit，就可以让U盘第一启动顺序了。

- 如果你找到这样的启动项，跳到下面的Clover引导
- 如果你没找到，进行下面的操作

若BIOS未能识别启动项，右边点击Add Boot Option，Boot Option Name可以随便填，然后点击File Name右边的按钮进入选择引导的EFI文件。

<a href="https://img.hyejeong.cn/MacOS/X5.jpg">![](https://img.hyejeong.cn/MacOS/X5.jpg)</a>

<a href="https://img.hyejeong.cn/MacOS/X4.jpg">![](https://img.hyejeong.cn/MacOS/X4.jpg)</a>

选择CLOVERX64.efi 保存 把它移动到最顶部，然后OK，Apply，Exit，就可以让U盘第一启动顺序了。

## Clover引导

如果一切正常 重启后 我想你已经见到Clover界面 距离成功又近了一步

<a href="https://img.hyejeong.cn/180123/X12.jpg">![](https://img.hyejeong.cn/180123/X12.jpg)</a>

选择Boot OS X Install from Install macOS High Sierra 这个安装图标 按下空格键 勾选上Verbose这个选项 然后选择Boot with selected mode

如果顺利的话，就会进入满屏英文滚动，两三分钟后进入安装界面，这个等待的时间每个人可能都不一样 给点耐心 否则 右上角关掉网页

如果突然卡住 你可以拍照下来咨询网友 我们可以尽可能的为你解决问题 但拒绝伸手党

Message QQ 756876988

同时 请跳过下面的slide计算，进入安装这一节。

如果很不幸，选择安装图标后出现了类似下面的画面，提示can not allocate relocation block...的话，就需要手动计算slide值了。

<a href="https://img.hyejeong.cn/180123/X8.jpg">![](https://img.hyejeong.cn/180123/X8.jpg)</a>

## slide计算


留意错误信息Error allocating 0x13ed0 pages at 0x0000000019166000 ...，记住0x13ed0这个数值，你将会面临本教程最艰难的部分,不同的机器这个数值也是不一样的 根据自身的状况进行计算

重启，再回到Clover引导画面，如下图，选取下面行的第一个图标，进入UEFI Shell 64界面

当命令行准备好之后，输入memmap命令，输出如下图

<a href="https://img.hyejeong.cn/180123/X10.jpg">![](https://img.hyejeong.cn/180123/X10.jpg)</a>

不同的内存容量和不同的BIOS版本，上图的数据是不同的，上图是我Micron 16G内存在1.6.1 BIOS下的情况。

我们在图中找符合以下条件的行：
1. Type列的值是Available
2. Pages列的值大于等于13ed0这个值
3. Start列的值比100000大

不难得出两个结果：
12374000    1A9BC
100000000  3BE000

第二个Start数值太大了，我们只要第一个12734000即可。

打开计算器[在线计算](http://www.yunsuan.org/app/s3127 "在线计算") (计算结果忽略小数点)

用公式：Start / 200000 + 1 计算出Slide值：12734000 / 200000 = 93.9A（忽略小数点后数字），92 + 1 = 94，转换成10进制[在线转换](http://tool.oschina.net/hexconvert "在线转换")，显示为148。

以上计算方法参考自 @wmchris @darkhandz的教程.

实测一下，在Clover启动画面，选UEFI Shell 64图标那行的第三个Options，如图，把Boot Args的slide=168改为slide=148（对这行按空格，就进入编辑模式了，用方向箭头移动光标），改完按回车，然后Esc键。

<a href="https://img.hyejeong.cn/180123/X11.jpg">![](https://img.hyejeong.cn/180123/X11.jpg)</a>

再次选择安装图标，一般来说你就能进入安装界面了，进入下一节

------------

## slide注意事项

如果计算出来的值还是有问题（出现前面同样的错误，或者只显示一排++++号就不动了），那你可以试试把计算出的值加1或减1，比如计算出来是148,即slide=147和slide=149都试试。

要注意的是，这里只是临时修改启动参数，你每次重启它都会恢复回原来的样子。所以我们要备用电脑，打开config.plist把slide=168改成slide=你测试出的可启动值，保存，然后再进入安装界面。

还有要注意的是，你的kernel cache变化的时候（安装了第三方驱动到系统），有可能会导致slide值需要重新计算。

------------

## 安装

希望你不是经历了上面梦魇般的slide计算步骤才来到这里的，如果是的话，您辛苦了。

进入安装界面后，你可以用磁盘工具对你的SSD进行分区，目前我的5510的固态全部分给MacOS使用。

第一次进入安装过程2分钟左右就会重启，然后Clover多了一个叫Boot macOS Install from Mac（系统分区名）的图标，直接对着它回车。

然后第二次进入的话，几秒钟就重启了。

第三次进入，大概10多分钟就安装好系统了。

安装完之后笔记本自动重启，Clover的引导画面已经多了一个系统图标：Boot macOS from mac，选择它进入就可以启动系统了，设置时区、语言、无线连接、用户登陆什么的。

有个比较重要的就是提示是否开启FileVault，这东西似乎和用户数据加密有关，开启需要更换部分驱动 个人建议不开启

进入桌面后建议你再重启一次系统，不然亮度调节无效。


------------

## 硬盘EFI引导

目前都是靠U盘的Clover引导才能进入系统的，所以我们要把Clover安装到硬盘的EFI分区，让系统脱离U盘引导

找到提前准备好的 Clover.pkg （Mac下对NTFS分区只能读不能写 你可以提前将这些放在仓库盘内）

双击打开，提示来自身份不明的开发者，这个时候你可以按组合键Alt + 空格，然后输入term回车，就跳出终端窗口了。

在终端输入：sudo spctl --master-disable 回车，提示输入密码执行，然后没有任何输出，这就代表执行成功了

<a href="https://img.hyejeong.cn/180123/X13.png">![](https://img.hyejeong.cn/180123/X13.png)</a>

再次双击刚才的pkg进入安装

### Clover安装

出现安装窗口，点击继续 ，再继续，点击更改安装位置，选择你的 （Mac）系统分区，再点击左下角的自定义，按下图的勾选

<a href="https://img.hyejeong.cn/180123/X14.png">![](https://img.hyejeong.cn/180123/X14.png)</a>

点击安装 输入账户密码 完成安装


------------

## EFI完善

### Clover Configurator

打开准备好的Clover Configurator 现在对硬盘里的EFI文件进行替换

<a href="https://img.hyejeong.cn/180123/X15.png">![](https://img.hyejeong.cn/180123/X15.png)</a>

点击左边栏的Mount EFI - 在EFI Partitions 下 点击 Mount Partition - Open Partition

<a href="https://img.hyejeong.cn/MacOS/X10.png">![](https://img.hyejeong.cn/MacOS/X10.png)</a>

此时 你可以将CLOVER文件夹删除 替换我提供的成品EFI文件

请注意 记得对Config.plist下的Slide值进行修改

一切就绪 替换完后进行重启 黑屏的时候拔掉U盘，耐心等待，Clover画面再次出现的话，说明硬盘Clover引导成功。


------------

## 网卡顺序修正

如果顺序错误，就不能在App Store下载东西。

打开终端，执行命令：networksetup -listallhardwareports，如下图，留意en0对应的Port是否Wi-Fi，如果是，说明顺序正确了，跳过本步骤。

<a href="https://img.hyejeong.cn/180123/X16.png">![](https://img.hyejeong.cn/180123/X16.png)</a>

如果顺序不正确，你需要点击屏幕左上角的苹果图标，打开系统偏好设置-网络，点击左下角的减号，把所有列表项删除干净，然后执行命令：`sudo rm -f /Library/Preferences/SystemConfiguration/NetworkInterfaces.plist`，重启。

重启后，再进入系统偏好设置-网络，点击左下角的加号，先添加WiFi，再添加蓝牙，然后应用，就可以了。

------------


## 补丁修正

部分安装后出现耳机播放音乐有电流音 安装VerbStub补丁后可即可修复 下载地址统一在文件下载里

下载后解压 我担心操作过于复杂 特意录制了GIF供你们参考

<a href="https://img.hyejeong.cn/MacOS/X1.gif">![](https://img.hyejeong.cn/MacOS/X1.gif)</a>



------------

## 系统升级

截止到今天7月5号 最新版本为10.13.6 可以正常更新

Mojave的EFI驱动也可以支持到DB3 强调一下只能用于升级不能用于安装 当然可以重新安装 下面有提供Mojave的Install EFI

<a href="https://img.hyejeong.cn/MacOS/X1.png">![](https://img.hyejeong.cn/MacOS/X1.png)</a>

<a href="https://img.hyejeong.cn/MacOS/X9.jpg">![](https://img.hyejeong.cn/MacOS/X9.jpg)</a>

------------

## 白果三码

注入白果三码就可以激活iMessage、Facetime 获得更好的体验

获取途径这里不进行讲解 提取白果的三码也很简单

下载iMessageDebug后 执行命令就可以了

<a href="https://img.hyejeong.cn/MacOS/X2.gif">![](https://img.hyejeong.cn/MacOS/X2.gif)</a>

执行了chmod a+x 后 iMessageDebug变成了一个可执行文件 打开

会出现一堆东西 拍照保存

黑苹果在Clover Configurator里替换即可

<a href="https://img.hyejeong.cn/MacOS/X6.png">![](https://img.hyejeong.cn/MacOS/X6.png)</a>

红色框选的部位是最重要的部分

<a href="https://img.hyejeong.cn/MacOS/X7.png">![](https://img.hyejeong.cn/MacOS/X7.png)</a>

<a href="https://img.hyejeong.cn/MacOS/X8.png">![](https://img.hyejeong.cn/MacOS/X8.png)</a>

数据对照白果的填入即可  其中SmUUID填入HardWare UUID  Custom UUID填入System-ID

保存后重启即可

------------
## 文件下载

文件全部转存至针针云盘

[TransMac](https://zhenbaby.cn/s/fnjvsoe6 "TransMac")<br>
[Install EFI 10.13 ](https://zhenbaby.cn/s/ezdeiecl "Install EFI 10.13")<br>
[Install EFI 10.14 ](https://zhenbaby.cn/s/2tyuqt58 "Install EFI 10.14")<br>
[Final EFI 10.13/10.14 ](https://zhenbaby.cn/s/o5pedf35 "Final EFI 10.13/10.14")<br>
[VerbStub](https://zhenbaby.cn/s/me8kveyp "VerbStub")<br>
[Clover](https://zhenbaby.cn/s/a9npkyoz "Clover")<br>
[Clover Configurator](https://zhenbaby.cn/s/h09qorp2 "Clover Configurator")<br>

------------


## 最后

到这里为止 整个教程也算告一个段落了 目前的版本已经可以满足你的日常功能使用 尽管他还不算十分的完美

整个教程并非我一人独自完成 许多部分都是借鉴摘抄 @darkhandz 大神的教程

最新的EFI配置文件是由 @Scottsanett 搭配生成的

十分感谢大神的贡献 才能整合出一个相对完美的黑苹果版本

如果我的工作对您有所帮助  你可以选择[捐赠](https://img.hyejeong.cn/donate.png "捐赠")我


2018.7.5

针针
