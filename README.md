写在前头

我真的是萌新啊 这是我第一使用Github上传代码 希望各位大佬多多见谅



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
<a href="https://img.hyejeong.cn/180123/X1.jpg">![](https://img.hyejeong.cn/180123/X1.jpg)</a>

写完镜像像之后，U盘会变成两个分区，一个叫EFI的就是U盘引导区，里面存在UEFI版本的Clover；另外一个叫U盘的提示你未格式化，这个时候千万不要手贱去格式化，这是因为Windows无法识别Mac的文件格式

打开Diskgenius，你可以大概浏览一下U盘EFI引导区目录结构：

<a href="https://img.hyejeong.cn/180123/X2.png">![](https://img.hyejeong.cn/180123/X2.png)</a>

制作好镜像后 使用我预先准备的EFI文件 可以将镜像U盘下的Clover下全部删除替换为我提供的EFI

- 如果你和我一样是i7的CPU，可以进入下一步BIOS
