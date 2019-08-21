# 要求
 1. 主板支持VT-d和VT-x
 3. 一张独立显卡，以及支持集显的CPU
 4. 用户需要对Linux 系统有一定了解
 5. 分配给客户机的 GPU 的 ROM 必须支持 UEFI
  5. 多信号源显示器，或多个显示器（多条视频线）


# 本人电脑配置
显卡：GTX1070
主板：华硕玩家国度 Z270-GAMING（UEFI，CSM关闭）
处理器：i7-6700K
内存：16G * 2 （DDR4）
（如果主板是与我的相似的话，是最好的）

![GPU_1](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/graphics1.PNG)

![GPU-2](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/graphics2.PNG)

![CPU](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/cpu.PNG)

![MOTHERBOARD](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/motherboard.PNG)

# 写在（伪）最前

UP 主还只是一位高中生，由于时间以及能力原因，我不可能能够回答所有的问题（可能你们的问题我一个都回答不来，当然如果我能帮一下的话我会尽力的）。
如果可以的话，厚着脸皮求个点赞呗（我知道这个视频的制作水平（也包括这份文档））远远差过其他UP 主的视频，但我确实是尽力了，就当我求你们了呗（所以才没求投币（当然如果你想的话更欢迎，嘻嘻嘻）））。
如果有问题的话，请在评论区或弹幕内提醒我，我会抽空更新这个文档的（当然视频就不太可能更新了）
请各位不要喷我，我真的是尽力了（我承认我自己仍然对QEMU 和虚拟系统管理器仍然还是在一知半解的状态，我也是一边安装一边学（我也承认这也是我第一次使用QEMU））。
最后谢谢各位了！！

# 优劣

## 优点
1. 不需要再对clover 进行大修改即可运行N 卡
2. 恢复方便，就算把EFI 分区弄坏了也能直接恢复
3. 备份方便，可以直接拷贝硬盘文件走
（好了我编不出来了）

## 缺点
1. 对主板的虚拟化的各项功能有较高要求
2. 多显示屏，要求多次切换显示器信号源
3. 需要CPU 支持集显（其实不支持也是可以的，但是本方法是针对有集显的用户的
4. 无法使用DDR4 内存，降为DDR3

**不过你都已经点进来了，对吧**


# 什么是KVM & QEMU
KVM:
> 基于内核的虚拟机（英语：Kernel-based Virtual Machine，縮
>寫為KVM）是一种用於Linux內核中的虛擬化基礎设施，可將
>Linux內核轉化為一個虚拟机监视器。KVM于2007年2月5日被
>導入Linux 2.6.20核心中。KVM需要支持硬件虚拟化拓展特
>性的处理器。
>KVM 起 初 支 持 x86 平 台 处 理 器 并 随 后 被 移 植 到 了
>S/390、PowerPC、和IA-64平台上。在3.9内核合并时也导
>入了ARM移植版。
>诸多客户操作系统支持KVM，包括Linux的诸多发行版、
>BSD、Solaris、Windows、Haiku、ReactOS、Plan 9、AROS研
>究操作系统和OS X。除此之外，还支持Android 2.2、
>GNU/Hurd
>（Debian K16）、Minix 3.1.2a、Solaris 10 U3和
>Darwin 8.0.1，而其他操作系统或新版操作系统都支持KVM，
>仅仅存在一些限制而已。
>VirtIO 半 虚 拟 化 在 Linux 、 OpenBSD 、 FreeBSD 、
>NetBSD、Windows上支持对部分设备的半虚拟化。这项特
>性支持半虚拟化网卡、半虚拟化磁盘控制器、用于调整客
>户端内存使用的气球设备（Balloon device）和使用獨立計算環
>境簡單協議驱动程序的VGA图形接口。


QEMU
>QEMU（quick emulator）是一款由Fabrice Bellard等人编写的免费的可
>执 行 硬 件 虚 拟 化 的 （ hardware virtualization ） 开 源 托 管 虚 拟 机
>（VMM）。
>其与Bochs，PearPC类似，但拥有高速（配合KVM），跨平台的特性。
>QEMU是一个托管的虚拟机镜像，它通过动态的二进制转换，模拟
>CPU，并且提供一组设备模型，使它能够运行多种未修改的客户机
>OS，可以通过与KVM（kernel-based virtual machine开源加速器）一起
>使用进而接近本地速度运行虚拟机（接近真实电脑的速度）。
>QEMU还可以为user-level的进程执行CPU仿真，进而允许了为一种架构
>编译的程序在另外一种架构上面运行（藉由VMM的形式）。

（没错我全在维基百科上抄的）

# 安装

## S1：将视频输出线接入主板的口上

## S2：在自己的主板里启用相应的虚拟化选项

详细看视频

## S3：安装Ubuntu
 1.进入 https://ubuntu.com/download/desktop 下载Ubuntu 安装镜像（我选择了19.04）
 2.使用一款自己顺手的U盘启动制作工具（推荐Rufus）
这里给大家看一下我的配置
![Rufus_conf](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/Rufus_conf.PNG)

 3.开始安装Ubuntu（最好UEFI）

## S4：配置环境
### 安装软件
 先安装git
```shell
sudo apt install -y git
git clone https://github.com/foxlet/macOS-Simple-KVM.git
```
 配置python 环境，顺便下载镜像
```shell
sudo apt install -y python python3 python-pip python3-pip
pip3 install click request
pip install click request
cd macOS-Simple-KVM
./jumpstart.sh --high-sierra
```
 再安装KVM/QEMU 需要的软件
```shell
sudo apt install -y virt-manager qemu-kvm qemu libvirt-daemon-system libvirt-clients ovmf bridge-utils
```
 加入kvm 用户组
```shell
sudo adduser `id -un` kvm
```
 启动libvirt 服务
```shell
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
```
## 配置安装基本macOS 
确认上面几步已经完成的，重启
打开虚拟系统管理器（在活动里搜索，关键词：virt-manager）
可以看到以下界面
![virt-manager_without](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/Capture10.PNG)

在终端内，将当前工作文件夹切换到macOS-Simple-KVM 里，执行
```shell
sudo ./make.sh --add
```
可以看到虚拟系统管理器出现一个新的虚拟机
![virt-manager_with](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/Capture11.PNG)

### 创建虚拟硬盘

```shell
 qemu-img create -f qcow2 MyDisk.qcow2 [disk_size]G
```
其中[disk_size] 里填虚拟硬盘大小

接着，选定该虚拟机，双击，在选项卡下找到一个带‘i’ 字样的图标，如图
![virtmanager_i1](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/Capture13.PNG)

单击“选定“选择或创建自定义存储”并在下方找到“管理”，并再选择“本地浏览”，找到刚才创建的虚拟硬盘，并选定它

### 更改网卡配置

找到旁边有NIC 字样的选项卡，按下图调整
![networkard-0](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/networkard-0.PNG)

![networkcard-1](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/networkcard-1.png)


### 启动虚拟机

单击’i'字样右侧的像播放键的图标，再单击同一栏最左边的图标，启动后clover boot manager 内直接回车

![defaultpage](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/defaultpage.png)

### 安装macOS
来到这个界面（该界面唯一的区别是截图上的Install 应为Reinstall，其它无改变）后

![macos_install](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/macos-install/macos_install.PNG)

点击 Disk Utility, 然后在侧边栏选择你要安装的硬盘（截图与实际会有一些不同，可以看大小大概知道要安装的硬盘）

![diskutility](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/macos-install/diskutility.PNG)

单击Erase

![diskutility_diskerase](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/macos-install/diskutility_diskerase.PNG)

按自己需求改名，接着按Erase（不要改其它内容）

然后关闭Disk Utility

![macos_install](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/macos-install/macos_install.PNG)

选择Reinstall macOS （该截图与实际不符）

来到以下界面![macos_start](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/macos-install/macos_start.PNG)

接下来就接受许可协议，选择安装的硬盘（分区），接着一路下一步

开始安装

### 配置macOS

在这里注意一下，在其它设备上试过选择在上下滑动时死机，建议直接选第一个

![macos_setup-1](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/macOS-setup/macos_setup-1.PNG)

下面就不贴图片了



但是

**不要登录，不要登录，不要登录**

现时

**不要使用任何苹果的在线服务，不要使用任何苹果的在线服务，不要使用任何苹果的在线服务**

其它按需更改

#### 改变地区与语言设置

进入桌面后，打开设置

![i18n](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/macOS-setup/i18n.PNG)

选择Language & Region

![i18n-1](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/macOS-setup/i18n-1.PNG)

按下图改变地区的设置

在界面中找到“+”键，添加简体中文

![i18n-2](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/macOS-setup/i18n-2.PNG)

如果要使用中文的话选择右边的选项，如果要使用英文的话，选左边

![i18n-3](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/macOS-setup/i18n-3.PNG)

**基本安装结束**

## S4：配置显卡直通

### 下载Web Driver

打开 http://www.macvidcards.com/drivers.html ，拉到最下面
找到下图的驱动，双击下载

![1](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/1.PNG)


之后用U 盘将该驱动送到macOS 上

在虚拟机中插入U 盘的教程如下：

![t-1](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/t-1.PNG)

选择“添加硬件”，在弹出的窗口中选择”USB 主机设备“，同时选择你的U 盘，最后按“完成”，如下图![t-2](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia\t-2.PNG)

然后就应该可以在macOS 上打开下载的驱动文件了

### 安装 Web Driver

打开安装文件

![3](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/3.PNG)

一路下一步

如遇到以下窗口

![5](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/5.PNG)

选择左边那个选项

![6](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/6.PNG)

划红线的能点的都点上

结束安装后，让它重启，等它重启完后再关机

### 配置显卡直通
#### 配置
执行
```
sudo nano /etc/default/grub
```
在该配置文件内找到下图划蓝线这一行

![7](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/7.PNG)

加上这些字段```nouveau.modeset=0 rd.driver.blacklist=nouveau intel_iommu=on iommu=pt vfio-pci.ids=```

（题外话，官方的文档上是要求把intel_iommu=on iommu=pt vfio-pci.ids= 加到GRUB_CMDLINE_LINUX=" "（也就是截图的下一行）里的，但我没看清楚，但是加到这里也好像没什么问题）



再打开一个终端窗口，执行```lspci -nn``` ，以我的电脑为例

![9](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/9.PNG)

找到你的显卡，在后面找到设备码，在我这里的例子是10de:1b81

同时给显卡板载声卡也给直通过去，同样的操作，在我这里的例子是10de:10f0

把这两个码复制粘贴到刚才的等号后面，中间使用逗号分隔

贴上我的例子

![8](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/8.PNG)

接着Ctrl + x ，Y 键保存退出，运行
```shell
sudo update-grub
```

完成后重启

#### 检查

打开终端，下载一个叫iommu.sh 的脚本（可在本仓库内找到）

运行```chmod a+x ./iommu.sh``` (假设你的脚本就在当前工作文件夹内)，

接着运行

```shell
./iommu.sh
```

贴上我的运行后的例子

![10](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/10.PNG)



如图，可以看到再每一行开头前都可以找到 IOMMU Group N，接着找到你的显卡，看其对应的组号（本例中为1）接着上下看一下有没有其它设备，如果就像本例中的只有一个PCI bridge（这个东西可以忽略）是你没有见过的，那么就说明直通成功了，如果还有除上述设备的其它设备，那么你就要考虑放弃了（可以考虑使用ACS Override，但那很复杂，具体可以参考https://passthroughpo.st/mac-os-vm-guide-part-2-gpu-passthrough-and-tweaks/ ，拉到最下面Troubleshooting 这一节中的Multiple PCI devices in the same IOMMU group 来寻求更多帮助）

#### 在虚拟机中设置显卡驱动

##### macOS 中设置Web Driver

直通成功后（恭喜你），

再次打开虚拟机，找到在上面那一栏的如图示的图标（已划红线），单击

![11](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/11.PNG)

再单击在下图中鼠标选择的选项

![12](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/12.PNG)

输密码，在以下对话框内选择“Not Now” ，接着手动关机

![14](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/14.PNG)



接着，请再找一条视频线从显卡上的接口接到显示器（不管是另一个还是原来的）上
现在顺便找另一个鼠标（USB）

#### 在虚拟系统管理器中设置显卡直通

移除下列两个设备

![15](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/15.PNG)



将你的显卡添加进来，选择添加硬件，找到PCI 主机设备，在左侧的设备列表中找到你的显卡以及板载显卡的声卡，依次添加

![18](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/18.PNG)

接着参考上面的U 盘插入虚拟机的教程，**先将刚才的U 盘移除**

**再**将刚才找的鼠标和你的键盘以USB 设备形式添加，

移除安装盘（硬盘序号可能不对，但文件名“BaseSystem.img" 还是一样）

![17](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia\17.PNG)

接着开机

![16](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/nvidia/16.PNG)

在该界面上，按Enter 键

接着**立即**（划重点）将显示器的信号源切换到你的显卡所接的信号线所属的信号上

稍等片刻，如果还有闪屏的现象，参阅上面的”macOS 中设置Web Driver“ 节再切换一次，但略有不同的是这次**直接选择重启**而不是手动关机后再开机，**并且不要更改显示器信号源**

在输入密码界面登录时若没有闪屏现象，同时也可以在苹果图标那里打开关于本机，可以看到自己的独显，那么就说明

**该黑苹果已经可以进行基本日常工作**（U 盘暂时无法使用）

截至现在
**不要使用任何苹果的在线服务**
**不要使用任何苹果的在线服务**
**不要使用任何苹果的在线服务**

## S5：Apple ID 设置

首先执行```sudo spctl --master-disable``` 以允许来自所有来源的应用程序

在https://mackie100projects.altervista.org/ 下载Clover Configurator（以下简称CCV）

打开刚刚下载的CCV，在侧边栏里选择”Mount EFI“

![1](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/1.PNG)

在下方的EFI Partitions 中也应该有两个选择，两个都挂载，等会儿看一下哪个是对的

![2](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/2.PNG)

挂载成功以后，单击旁边的Open Partition，如果分区内存在EFI\Clover 一系列文件夹的话，说明就是这个盘

![3](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/3.PNG)

打开到EFI/Clover 文件夹后，打开config.plist 文件（如果不是以CCV 打开，则右键打开方式，再不行直接拖进CCV 的窗口）

选择侧边栏上的SMBIOS ，单击红色框，选择与你最相似的机器（不用死抠）

![5](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/5.PNG)

![6](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/6.PNG)

单击红框的按钮

![7](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/7.PNG)

输完验证码后，若出现以下内容，则说明过了，如果出现一台mac 电脑的话，请再上图的Serial Number 旁边再按一下Generate New，再重复一遍

![9](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/9.PNG)

接着再按下图红框按钮

![7 - Copy](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/7%20-%20Copy.PNG)

（接下来的内容全在https://www.tonymacx86.com/threads/an-idiots-guide-to-imessage.196827/ 抄的）

出现以下内容，也是再按Generate Now 再按Check Coverage 按钮再来一次

![12 - everymac - Wrong](https://https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/12%20-%20everymac%20-%20Wrong.png)

出现以下内容说明成功![13 - everymac - Right](https://https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/13%20-%20everymac%20-%20Right.png)

保存退出

接着打开虚拟机管理界面，选择硬件详情![10](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/10.PNG)

选择NIC 开头的选项

![11](https://raw.githubusercontent.com/zyk2290/macOS-on-KVM/master/Snippings/apple-id/11.PNG)

将右侧中的勾选框中的active 给取消掉，应用，即可断网（开网即相反）

打开终端，执行（将[username] 替换成你的用户名）

```shell
cd /Users/[username]/Library/Caches
sudo rm -rf com.apple.iCloudHelper com.apple.imfoundation.IMRemoteURLConnectionAgent com.apple.Message

cd ..
cd ./Preferences
sudo rm -rf com.apple.iChat.* com.apple.icloud.* com.apple.ids.service com.apple.imagent.* com.apple.imessage.* com.apple.imservice.*
```

重启后开网，在**设置**里登录苹果账号



## S6 evdev 设置

关闭虚拟机

打开Ubuntu 上的终端

键入

```
ls /dev/input/by-id
```

找到你要选的设备，将其设备名替换[input device id]

```shell
sudo cat /dev/input/by-id/[input device id]
```

移动你刚才选的设备（是键盘的就随便敲几下键盘，是鼠标的就移动一下），如果有乱码输出，则说明成功

在另一个终端内键入

```shell
sudo EDITOR=nano virsh edit win10
```

找到相似的地方(看视频你才能知道复制粘贴到哪里)

```xml
<qemu:arg value='-object'/>
 <qemu:arg value='input-linux,id=mouse1,evdev=/dev/input/by-id/MOUSE_NAME'/>
 <qemu:arg value='-object'/>
 <qemu:arg value='input-linux,id=kbd1,evdev=/dev/input/by-id/KEYBOARD_NAME,grab_all=on,repeat=on'/>
```

将刚刚的鼠标名和键盘名替换MOUSE_NAME 以及KEYBOARD_NAME （不要调换）

将[username] 替换成你的用户名

（第1、2行的命令不确定是否要执行，反正也花不了多少时间）

```shell
sudo usermod -a -G input [username]
sudo gpasswd -a [username] input
sudo usermod -a -G input root
sudo gpasswd -a root input
sudo systemctl stop apparmor
sudo systemctl restart libvirtd
sudo systemctl disable apparmor
```


## S7:Have Fun!!!


# 参考

https://www.reddit.com/r/VFIO/comments/bamn5a/permission_denied_when_trying_to_pass_through/
https://passthroughpo.st/mac-os-vm-guide-part-2-gpu-passthrough-and-tweaks/
https://www.jianshu.com/p/74b18e6aa5fa

https://passthroughpo.st/new-and-improved-mac-os-tutorial-part-1-the-basics/

https://www.tonymacx86.com/threads/an-idiots-guide-to-imessage.196827/

https://passthroughpo.st/using-evdev-passthrough-seamless-vm-input/

(想不起来了。。。可能没了)

