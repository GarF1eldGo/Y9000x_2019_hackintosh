# 联想拯救者y9000x 2019款 黑苹果安装经历

### 写在前面
2021年暑假的时候安装的黑苹果，前前后后弄了很久，也遇到了一些坑，写下这篇文章记录一下我的安装历程。  
这不是一篇黑苹果安装教程，只是单纯记录一下本人安装黑苹果过程中几个关键的部分和遇到的问题。  
<br>

### 电脑初始配置
* 显卡：Intel(R) UHD Graphics 630  
* 硬盘：PM981A  
* 处理器：Intel(R) Core(TM) i7-9750H CPU @2.6GHz  
* 网卡：Intel AX200  
* 分辨率：4k  
<br>

### 1.硬件准备
* 安装新固态硬盘  
由于PM981A硬盘的固件与黑苹果系统存在兼容问题，所以选择更换了固态硬盘SN750。后来也曾想过尝试在PM981A上安装黑苹果系统，不过在看了[《为什么PM981，PM981A不能支持安装黑苹果？》](http://k61.org/1fbb0d)这篇文章后打消了继续尝试的念头。总的来说,对于小白而言这样的尝试还是有比较大的挑战，同时即使能够在PM981A上安装黑苹果系统，也可能会出现各种各样的问题。  

* 更换网卡  
虽然本机自带的网卡可以通过安装驱动的方式实现黑苹果上网、蓝牙功能。但是我在当时还是一个小白，对这方面的了解不多，所以按照网上教程介绍的方式，更换了博通网卡DW1820A,其实这款网卡也是需要添加驱动的，不过相比DW1830 DW1860这种黑苹果免驱网卡的价格，DW1820A的价格更加亲民，同时由于网上的EFI文件很多添加了博通网卡驱动，所以很大可能上并不需要在安装过程中手动添加该网卡的驱动。
* 更换硬件注意事项  
我在更换硬件的时候踩了雷，当时自己拆了机替换网卡，结果替换完网卡后，发现电脑开不了机，尝试了多次仍然开不了机。后来拿到联想维修店去维修，才知道原来替换硬件的时候，**需要将连接电池的电池排线拔下来，来防止短路。** 这里提到的电池排线并不是平时给笔记本充电的电源线，而是在笔记本内部，为电池供电的电池排线。关于电池排线的详细内容可以查看[笔记本的电源排线怎么拔？ - ZKBDJ的回答 - 知乎](https://www.zhihu.com/question/382769128/answer/1953352782)
<br>

### 2.原理学习  
虽然网上有不少安装教程，可以直接根据安装教程来安装黑苹果，不过在安装黑苹果前先弄清楚黑苹果的工作原理，对我而言更加重要，所以我在动手操作前阅读了一些文章，其中对我帮助比较大的一篇文章叫[《黑苹果入门完全指南》](https://astrobear.top/2020/02/14/Introduction_to_hackintosh/)，这篇文章介绍了黑苹果的工作原理，并且介绍了clover引导中各种重要的操作。现在看来，虽然安装clover引导可能是个走弯路的行为，因为我想要引导界面加载图片，所以后来不得不换成了opencore引导，但是这篇文章很大程度上帮助我理解了黑苹果的工作原理，这让我在后续完善黑苹果遇到问题时，能够更加有方向地去尝试解决问题，而非两眼抓瞎不知所措。  
<br>

### 3.初次安装尝试
* EFI选择  
首先需要指出，github上就有多种机型EFI文件的仓库，其github地址为[daliansky/Hackintosh](https://github.com/daliansky/Hackintosh)，所以不必在网上花钱下载各种机型的EFI文件。同时我不得不对提供EFI文件和维护仓库的人表示感谢，正是因为他们的存在才让我这种小白安装黑苹果成为了可能。Y9000X有四个EFI文件可供选择，我初次安装时选择了[WangRicky/Y9000X-HACKINTOSH](https://github.com/WangRicky/Y9000X-HACKINTOSH)的EFI，因为该EFI可以解决外接显示器的问题。
* 镜像选择  
安装镜像我选择了Catalina 10.15.6 版本，从[黑果小兵](https://blog.daliansky.net/macOS-Catalina-10.15.6-19G73-Release-version-with-Clover-5119-original-image-Double-EFI-Version-UEFI-and-MBR.html)处获得。当时选择10.15.6是因为网上已有的教程表示，10.15.6是比较稳定能够实现黑苹果的版本，而更低的版本会影响软件的使用体验，更高的版本可能存在新的问题，所以我选择了10.15.6来进行第一次安装。  
* 安装结果  
由于下载的镜像本身带有黑果小兵准备的EFI文件，所以需要删除其EFI中的内容，然后替换为专门为Y9000X设计的EFI文件，我在本次安装中将CLOVER文件夹放入到EFI文件中。  
然后根据教程进行安装，可以成功安装，但是很多功能不能实现。包括网络、蓝牙、外接显示器等功能，所以这是一次失败的尝试，第一次安装尝试就这样暂告一段了。  
* 可能的原因  
现在看来，这一次安装没有成功，是因为我直接使用了EFI/CLOVER文件，而没有对其中的内容做针对性的修改；因为在其EFI仓库中的readme中写明了，“目前的config.plist是按照CFG LOCK解锁后配置的，如未解锁，请设置AppleCpuPmCfgLock和AppleXcpmCfgLock为true”，而我没有解锁CFG LOCK，所以应该修改config.plist，但是我在这次安装中没有修改config.plist。  
<br>

### 4.第二次安装尝试
* EFI选择  
第一次安装尝试失败后，我怀疑是我选择的EFI没有正常工作，因为例如联网等功能需要网卡驱动来启动，而驱动在EFI文件中，所以可能是EFI文件的问题。因此我想尝试使用黑果小兵的安装镜像中自带的EFI文件，因为其中有适配各种机型的驱动。
* 镜像选择  
Catalina 10.15.6。  
* 安装结果  
安装过程中，电脑发热比较明显，进入到系统中后，发现可以使用蓝牙，但是使用不了网络，也不能使用外接显示器。  
* 可能的原因  
后来查看黑果小兵博客中该镜像的帖子，发现需要修改config.plist中的“brcmfx-driver=0”来开启wifi功能......  
<br>

### 5.第三次安装尝试
* EFI选择  
选择了第一次安装尝试时的EFI（即WangRicky提供的EFI），不过这一次对EFI进行了修改，因为文章中提到了要修改CFG LOCK相关参数。  
* 镜像选择  
Catalina 10.15.6  
* 安装结果
mac系统的所有功能可以正常使用，包括网络wifi、蓝牙、外接显示器、触控板手势等。这次安装后，便开启了我使用黑苹果的历程。  
* windows完善  
因为更换了博通的网卡，需要为windows添加网卡驱动；我在B站上找到了相关的指导视频[DW1820A无线网卡Windows10驱动食用方法](https://www.bilibili.com/video/BV1wJ41117hB?from=search&seid=6221512991241222369&spm_id_from=333.337.0.0),然后使用mac系统从视频中提供的地址下载了相关驱动，才意识到要将驱动软件弄到windows系统上才行；而问题是U盘的文件系统（NTFS)并不支持mac系统，无法对U盘进行写入操作，也就无法拷贝下载好的内容给windows；如果我使用FAT32文件系统的U盘来拷贝驱动，又不能在windows上识别。鉴于windows系统目前没有安装驱动，无法联网下载驱动，所以我当时认为我陷入了一个僵局。  
后来在mac系统中找到了一个软件来支持NTFS文件系统的读写操作，软件叫《Tuxera NTFS for Mac 2021》，正版需要付费。我使用该软件成功将驱动放到了U盘中，并且在windows上用U盘将驱动添加好。windows完善的事情就告一段落了。  
<br>

### 6.从clover到opencore
* 起因  
其实clover引导已经可以很好的使用黑苹果了，但是我还有一点小遗憾，就是引导界面是全黑背景+图标的样子，我想要的引导背景是怡人的风景；于是在强迫症的驱使下，我打算将clover引导换为opencore引导（因为clover引导不支持引导界面加载图片）。
* EFI选择  
还是WangRicky提供的EFI文件，因为其EFI文件中既有clover引导的文件，也有opencore引导的文件。  
* 

