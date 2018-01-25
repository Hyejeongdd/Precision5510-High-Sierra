每年的9月起才能拿到Apple的教育优惠 没办法了 只能提前在笔记本上黑苹果体验了

首先先感谢@darkhandz @z65881120 @panruohuai @1148070455 @wmchris 等大佬的分享

这是一篇完全针对新手向的教程 适用于XPS15 9550/Precision 5510

望您能耐心的看完文章 尽管他有些啰嗦

    2018/01/25  初版1.0

------------
## 机型配置

    机型：DELL Precision5510
    CPU：Intel i7 6820HQ Processor
    内存：Micron Technology DDR4 2133 16G
    硬盘：Samsung PM951 512G + HGST 1T 7200 Rpm
    显卡：HD530
    显示器：SHARP 3840x2160
    声卡：Realtek ALC298
    无线网卡：DW1830
    BIOS: 1.6.1

## 注意事项
进入系统后 一定不要对系统账户进行操作 若丢失了管理员 目前的办法只能是格式化重装！

原机的Intel Wireless 8260AC无解 替换DW1830(BCM94360) TB 180入手

Dell XPS15/5510 上的准确应该叫Type-C接口 测试下若开机前插入Type-C设备 则可以正常启动 进入系统后插入Type-C设备无法驱动（测试设备：绿联Type-C Hub AS88772A+GL850A 双芯片）￥JuKJ0OZADYJ￥

HDMI下可正常输出 需配合歪果仁方法（测试设备：LG 29UM59A)
有关HDMI：

    One needs to modify the info.plist in /System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents, and change the corresponding value used in the CLOVER config.plist -> SMBIOS -> board-id from config2 to none.


## 遗留问题

    handoff不能正常使用

## 准备清单

    macOS High Sierra 10.13.1(17B48)正式版 with Clover 4278原版镜像
    TransMAC 11.0
    Diskgenius x64
    一枚大于8G的USB3.0 U盘 如果你不想读条心态爆炸 建议你准备一个USB3.0的U盘
    一台备用的电脑 可以进行修改plist和替换EFI的行为


------------

## 镜像写入

话不多说 开始操作 

我所使用的镜像是macOS High Sierra 10.13.1(17B48)正式版 with Clover 4278 是黑果小兵制作的 是可以正常安装的 当然你也可以选择10.13.2 当然 保险起见 还是希望你和我的版本一致

[镜像下载地址](https://blog.daliansky.net/macOS-High-Sierra-10.13.1-(17B48)-official-version-and-Clover-4278-original-image.html "镜像下载地址")

再次提醒！ 下载完请校对MD5值！下载完请校对MD5值！下载完请校对MD5值！

右键点击TransMac-以管理员身份运行软件 ，右键制作等待几分钟，出现Restore Complete即为写入完成。
<a href="https://img.hyejeong.cn/180123/X1.jpg">![SB](https://img.hyejeong.cn/180123/X1.jpg)</a>

写完镜像像之后，U盘会变成两个分区，一个叫EFI的就是U盘引导区，里面存在UEFI版本的Clover；另外一个叫U盘的提示你未格式化，这个时候千万不要手贱去格式化，这是因为Windows无法识别Mac的文件格式

打开Diskgenius，你可以大概浏览一下U盘EFI引导区目录结构：

<a href="https://img.hyejeong.cn/180123/X2.png">![](https://img.hyejeong.cn/180123/X2.png)</a>

制作好镜像后 使用我预先准备的EFI文件 可以将镜像U盘下的Clover下全部删除替换为我提供的EFI

- 如果你和我一样是i7的CPU，可以进入下一步BIOS设置了。
- 如果你是i5的CPU，用Notepad++打开config.plist，搜索191b0000，改成19160000，保存。

（根据外国友人的提示 在config.plist 里使用19160000可以防止闪屏白条 所以在硬盘EFI中使用的是19160000)

------------

## BIOS设置


重启，开机狂按F2，直到DELL Logo下方出现蓝色进度条，进入后 执行以下操作

- Secure Boot - Secure Boot Enable里改成Disabled
- System Configuration - SATA Operation 改成 AHCI
- System Configuration - Miscellaneous Devices 去除 Enable Secure Digital(SD)Card

<a href="https://img.hyejeong.cn/180123/X3.jpg">![](https://img.hyejeong.cn/180123/X3.jpg)</a>
<a href="https://img.hyejeong.cn/180123/X4.jpg">![](https://img.hyejeong.cn/180123/X4.jpg)</a>
<a href="https://img.hyejeong.cn/180123/X5.jpg">![](https://img.hyejeong.cn/180123/X5.jpg)</a>

Warning: 若你原本是SATA Operation为Raid On 更改成AHCI后将会导致无法进入原系统

## 启动项设置

在 General - Boot Sequence，在右边列表找到一个UEFI: U盘型号, Partition 1这样的启动项，把它移动到最顶部，然后Apply，OK，Exit，就可以让U盘第一启动顺序了。

- 如果你找到这样的启动项，跳到下面的Clover引导
- 如果你没找到，进行下面的操作

若BIOS未能识别启动项，右边点击Add Boot Option，Boot Option Name可以随便填，然后点击File Name右边的按钮进入选择引导的EFI文件。

<a href="https://img.hyejeong.cn/180123/X6.jpg">![](https://img.hyejeong.cn/180123/X6.jpg)</a>

<a href="https://img.hyejeong.cn/180123/X7.jpg">![](https://img.hyejeong.cn/180123/X7.jpg)</a>

选择CLOVERX64.efi 保存 把它移动到最顶部，然后OK，Apply，Exit，就可以让U盘第一启动顺序了。

## Clover引导

如果一切正常 重启后 我想你已经见到Clover界面 距离成功又近了一步

<a href="https://img.hyejeong.cn/180123/X12.jpg">![](https://img.hyejeong.cn/180123/X12.jpg)</a>

选择Boot OS X Install from Install macOS High Sierra 这个安装图标按回车键。

如果顺利的话，就会进入满屏英文滚动，两三分钟后进入安装界面，请跳过下面的slide计算，进入安装这一节。

如果很不幸，选择安装图标后出现了类似下面的画面，提示can not allocate relocation block...的话，就需要手动计算slide值了。

<a href="https://img.hyejeong.cn/180123/X8.jpg">![](https://img.hyejeong.cn/180123/X8.jpg)</a>

## slide计算

用备用电脑把U盘引导里面Driver64UEFI的OsxAptioFixDrv-64.efi改名成OsxAptioFixDrv-64.efix（留做备份）

将OsxAptioFix2Drv-64.efi放入Driver64UEFI下

然后打开config.plist，搜索kext-dev-mode，在它前面加一个slide=148，变成了下面这样：

 

    slide=148 kext-dev-mode=1 dart=0 nv_disable=1 -v

现在保存，然后重启，再次进入Clover引导画面，选择安装图标，看看是否顺利进入满屏英文，两三分钟后进入安装界面，跳到下一节

如果很不幸，选择安装图标后卡住在下面的画面，说明148这个数不适合你：

<a href="https://img.hyejeong.cn/180123/X9.jpg">![](https://img.hyejeong.cn/180123/X9.jpg)</a>

留意错误信息Error allocating 0x13ed0 pages at 0x0000000019166000 ...，记住0x13ed0这个数值，你将会面临本教程最艰难的部分。

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

如果计算出来的值还是有问题（出现前面同样的错误，或者只显示一排++++号就不动了），那你可以试试把计算出的值加1或减1，即slide=147和slide=149都试试。

如果很不幸，你还是无法进入满屏英文的画面，我只能建议你降级BIOS到1.2.21版本，然后用OsxAptioFixDrv-64.efi了，也就是U盘刚被写入dmg映像的状态。

要注意的是，这里只是临时修改启动参数，你每次重启它都会恢复回原来的样子。所以我们要备用电脑，打开config.plist把slide=168改成slide=你测试出的可启动值，保存，然后再进入安装界面。

还有要注意的是，你的kernel cache变化的时候（安装了第三方驱动到系统），有可能会导致slide值需要重新计算。

------------


## 进入安装前的verbose模式

满屏英文就是系统启动过程的各种信息显示，如果你卡住在这个英文画面很久（超过2分钟没有任何变化），你就要去论坛求救一下了。

如果遇到BrcmPatchRAM2: Firmware upgrade not needed.5行不断重复的情况，你可以先把U盘EFI/Clover/kexts/other里面的两个Brcm开头的文件夹剪切到其他地方（可以放AptioFix2文件夹），然后再试试进安装图标。等正常安装完成后再在后面安装硬盘引导的时候把这两个还原回去other里。

------------


## 安装

希望你不是经历了上面梦魇般的slide计算步骤才来到这里的，如果是的话，您辛苦了。

进入安装界面后，你可以用磁盘工具对你的SSD进行分区，比如我就分成两个Mac和Windows两个区了。
第一次进入安装过程2分钟左右就会重启，然后Clover多了一个叫Boot macOS Install from Mac（系统分区名）的图标，直接对着它回车。
然后第二次进入的话，几秒钟就重启了。
第三次进入，大概10多分钟就安装好系统了。

安装完之后笔记本自动重启，Clover的引导画面已经多了一个系统图标：Boot macOS from mac，选择它进入就可以启动系统了，设置时区、语言、无线连接、用户登陆什么的。

有个比较重要的就是提示是否开启FileVault，这东西似乎和用户数据加密有关，开启需要更换部分驱动 个人建议不开启

进入桌面后建议你再重启一次系统，不然亮度调节无效。


------------

## 硬盘EFI引导

目前都是靠U盘的Clover引导才能进入系统的，所以我们要把Clover安装到硬盘的EFI分区，让系统脱离U盘引导

找到提前准备好的 Clover_v2.4k_r4391.pkg （Mac下对NTFS分区只能读不能写 你可以提前将这些放在仓库盘内）

双击打开，提示来自身份不明的开发者，这个时候你可以按组合键Alt + 空格，然后输入term回车，就跳出终端窗口了。

在终端输入：sudo spctl --master-disable 回车，提示输入密码执行，然后没有任何输出，这就代表执行成功了

<a href="https://img.hyejeong.cn/180123/X13.png">![](https://img.hyejeong.cn/18012/X13.png)</a>

再次双击刚才的pkg进入安装

### Clover安装
出现安装窗口，点击继续 ，再继续，点击更改安装位置，选择你的 （Mac）系统分区，再点击左下角的自定义，按下图的勾选

<a href="https://img.hyejeong.cn/180123/X14.png">![](https://img.hyejeong.cn/180123/X14.png)</a>

点击安装 输入账户密码 完成安装


------------

## EFI完善

硬盘Clover安装后 移除U盘 重新启动 若可以进入Clover界面 则代表Clover 安装成功

### Clover Configurator

打开准备好的Clover Configurator 现在对硬盘里的EFI文件进行替换

<a href="https://img.hyejeong.cn/180123/X15.png">![](https://img.hyejeong.cn/180123/X15.png)</a>

点击左边栏的Mount EFI - 在Efi Partitions 下 点击 Mount Partition - Open Partition

此时 你可以将CLOVER文件夹删除 替换我提供的成品EFI文件

请注意 记得对Config.plist下的slide值进行修改 以及对OsxAptioFix2Drv.efi的替换！

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

### LE

这个文件夹的东西是要安装到/Library/Extensions/里面的。

    执行命令：sudo cp -r 把AppleGraphicsDevicePolicyInjector.kext拖过来 /Library/Extensions/
    执行命令：sudo cp -r 把X86PlatformPluginInjector.kext拖过来 /Library/Extensions/
    执行命令：sudo kextcache -i /

第一条命令的AppleGraphicsDevicePolicyInjector.kext是用来打开MacBookPro13,1这个SMBIOS的HDMI图像输出的。 第二条命令的X86PlatformPluginInjector.kext是用来让CPU获得0.8GHz的最低频率的。 第三条是重建缓存，让前面两个驱动在重启后生效。

在Precision5510中 使用X86PlatformPluginInjector.kext后 CPU最低频率约为1.00GHZ 具体原因未知）

### 关闭Verbose

如果你觉得你的系统足够稳定了，但是每次开机依然一屏屏英文字符把你刷得头晕眼花无心工作，那就在config.plist里找到下面这句，去掉-v就行了：

    kext-dev-mode=1 dart=0 nv_disable=1 -v

### 禁止生成休眠文件

简单来说就是SSD的写入寿命有限，而默认的休眠模式3会每次都把内存数据写入SSD（大概8G一次）。

然后说说禁用休眠的命令，3条按顺序执行：

    sudo pmset hibernatemode 0
    sudo rm /var/vm/sleepimage
    sudo mkdir /var/vm/sleepimage

### 原生NTFS读写

 1. 打开终端输入 diskutil list 查看所有分区的卷标（NAME列）
 2. 输入 sudo nano /etc/fstab 再输入密码回车进入配置
 3. 根据自己要配置的NTFS分区的卷标或UUID输入配置信息，下面各列举一条：
 
        UUID=C3C40C2E-7766-48A0-AA99-18305C9BAD3A none ntfs rw,auto,nobrowse
        LABEL=多媒体 none ntfs rw,auto,nobrowse

UUID可以从磁盘工具查到，输入完成之后按Ctrl+X再输入Y再回车进行保存。 

 4. 用磁盘工具将配置好的分区进行卸载再装载使配置生效（无需重启） 
 5. 因为加入了nobrowse所以Finder中看不到修改过的NTFS分区（不加入nobrowse会无法写入）所以要使用快捷方式进行访问. 在终端中输入 sudo ln -s /Volumes/卷标 ~/Desktop/卷标 即可在桌面生成NTFS分区的快捷方式。 
 6. 将快捷方式拖动到Finder的侧边栏就可以很方便的打开NTFS分区进行操作了
 
------------

## 系统升级 

截止到今天1月25号 最新版本为10.13.3 可以正常更新 

10.13.1更新时 需更换最新版的lilu.kext即可正常升级 其它驱动暂时无影响

<a href="https://img.hyejeong.cn/180123/X18.png">![](https://img.hyejeong.cn/180123/X18.png)</a>

如果出现了更新提示 建议你先去Pcbeta tonymac等论坛观望升级情况 确定无重大修改后进行更新

若硬盘充裕 建议启用Time Machine备份工具

进入系统后 一定不要对系统账户进行操作 若丢失了管理员 目前的办法只能是格式化重装！

------------

## 文件下载
近来百度云大肆对账号限速封禁 为此 特开通坚果云作为备用下载 若你有更好的网盘推荐 请联系我转存 谢谢

[坚果云](https://www.jianguoyun.com/p/DQdmkYkQ8J7cBhjC2UE "坚果云")
[百度云](https://pan.baidu.com/s/1c3j7bX2 "百度云")

------------


## 最后

到这里为止 整个教程也算告一个段落了 目前的版本已经可以满足你的日常功能使用 尽管他还不算十分的完美

整个教程并非我一人独自完成 许多部分都是借鉴摘抄 @Darkhandz 大神的教程 

十分感谢大神的贡献 才能整合出一个相对完美的黑苹果版本 

资料参考

http://bbs.pcbeta.com/viewthread-1751493-1-1.html
http://bbs.pcbeta.com/viewthread-1761764-1-1.html
http://bbs.pcbeta.com/viewthread-1766955-1-1.html
https://github.com/darkhandz/XPS15-9550-High-Sierra/issues/12
https://github.com/darkhandz/XPS15-9550-High-Sierra
https://github.com/wmchris/DellXPS15-9550-OSX

2018.1.25

针针
