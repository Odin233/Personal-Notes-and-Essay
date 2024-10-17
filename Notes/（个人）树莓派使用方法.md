## 目录

  - 树莓派环境配置（Respbian 11 arm-Liunx内核系统的硬件与python环境配置）
    - 操作系统选择
    - 存储选择与系统烧录
    - Linux系统操作
      - Linux系统常用命令
      - Linux内置文本编辑器Vim命令语法
      - Permission denied报错问题解决方案
    - 树莓派Raspbian 11系统的常识
    - 通过电脑端远程控制树莓派图形化系统
      - 通过网线组成有线局域网下获得树莓派有线局域网ip地址方法
      - 验证树莓派已与电脑端建立连接的方法
      - 通过ssh服务进行树莓派远程控制（为VNC图形化远程控制做准备工作）
      - 为树莓派配置无线网络
      - 通过手机热点组成无线局域网下获得树莓派无线局域网ip地址方法
      - 固定无线局域网ip地址
      - 固定有线局域网ip地址
    - 安装opencv（使用pip安装）
      - 树莓派系统
    - 安装opencv（源码编译安装OpenCV4）
      - 构建和编译OpenCV4（使用CMake）
  - OpenCV图像处理（入门操作）
    - 准备工作
    - 加载和显示图片文件
    - 获取特定坐标的RGB值
    - 图像通道分离进行颜色识别
    - 数组切片和裁剪（提取图像的局部切片）
    - 调整图片的大小
    - 旋转图像
    - 平滑图像（模糊图像）
    - 在图像上绘图（例如框框）
      - 矩形（框）
      - 圆形
      - 直线
      - 文本
  - 使用OpenCV进行图像中对象计数（基础案例解析）
    - 准备工作
    - 将图片转化为灰阶图（仅有黑白程度的图片）
    - 边缘检测
    - 二值化/闸值化灰阶图像
    - 查找、计数并画轮廓线
    - 腐蚀和膨胀
    - 蒙版和按位操作
  - 使用相机模块拍摄照片（驱动相机）
    - 解决连接摄像头造成的VNC分辨率不符合导致无法远程桌面的问题
    - 解决树莓派直接连接屏幕，屏幕坐上加载标志一直闪的问题
    - 使用摄像头拍摄并查看拍摄的照片
  - 使USB摄像头在树莓派系统中显示实时监控的画面并附带处理效果（驱动相机）
    - USB摄像头的可用函数
    - 仅从摄像头捕获视频
    - 保存摄像头捕获的视频（默认保存于当前目录）
    - 读取已经保存的视频文件
    - 捕获视频时采集照片（可用于采集训练样本）
  - 发光物体的识别+测距+追踪（实战）
    - 使用Haar分类器识别物体（自动提取特征，主要根据的是物体纹理）
      - 准备训练样本的过程详解（以WIndows环境为例）
      - 准备训练样本并生成样本描述文件（Linux环境）
      - 使用样本进行自定义训练（Linux环境）
      - 使用样本进行自定义训练（Windows环境）
      - 进行目标识别（处理输出视频的图像）
  - Linux下其他方便有用的库/工具
    - scrot截屏工具
    - python版本管理工具pyenv与虚拟环境工具virtualenv、virtualenvwrapper结合搭建任意版本python环境

## 树莓派环境配置（Respbian 11 arm-Liunx内核系统的硬件与python环境配置）

### 操作系统选择

官方推荐：NOOBS、Raspbian（基于Debian的ARM定制版本，最为推荐）

其他系统往往只在某一方面特别突出，在其他方面的兼容性不是很好。

- Raspbian Jessie：官方推荐系统，基于Debain 8，基于PIXEL图形界面。最新版为Raspbian Buster。
- Raspbian Jessie Lite：官方推荐系统，基于Debain 8，不带图形界面。
- Ubuntu MATE：Ubuntu针对树莓派的版本。
- Snappy Ubuntu Core：Ubuntu针对物联网（IoT）的一个发行版本，支持树莓派。
- CentOS：CentOS针对ARM的发行版，支持树莓派。
- Windows IoT：微软针对物联网的Windows版本，支持树莓派。
- FreeBSD：FreeBSD针对树莓派的发行版。
- Kali：Kali针对树莓派的发行版。
- Pidora：在Fedora Remix基础上针对树莓派优化的操作系统。

### 存储选择与系统烧录

树莓派开发板没有板载闪存，支持插入SD卡启动。

需要通过一台PC和读卡器进行系统烧写。

SD卡应不少于8GB，一般使用32GB。

系统镜像在树莓派官方网站："https://www.raspberrypi.org/downloads/raspbian/"下载。

SD烧录前需要使用SDFormatter进行SD卡的格式化。

SD烧录系统镜像软件选择：Win32DiskImager（Linux的文件类型Windows不可识别，因此烧录需要管理员权限，因此需要使用管理员权限打开此烧录软件）。烧录失败需要使用SDFormatter重新进行格式化再次进行烧录。

烧录完成后插入树莓派背面的SD卡卡槽内。

### Linux系统操作

我们知道，官方推荐的Raspbian系统依旧为以Linux为内核的一种系统，因此，Linux系统的常用命令和操作在Raspbian系统里同样适用。

#### Linux系统常用命令

- cd命令

cd命令可以用来切换当前目录。在Windows系统有类似的命令：

例子：

```
cd /TargetPath #切换到TargetPath路径下，默认为绝对路径

cd ./TargetPath #切换到当前目录的TargetPath目录下，"."是相对路径，代表当前目录

cd ../TargetPath #切换到上一级目录的TargetPath目录下，".."是相对路径，代表上一级目录
```

- ls命令

ls命令用于查看当前目录下所有的文件与子目录。可以搭配参数使用。

例子：

```
ls -l # 列出长数据传，包含文件的属性与权限数据等
ls -a # 列出全部文件，连同隐藏文件（隐藏文件的文件名开头为"."字符）一起也列出
ls -d # 仅列出目录本身，而不是列出目录的文件数据
ls -h # 将文件容量以较易读的方式（如GB、KB等）列出来
ls -R # 连同子目录的内容一起列出（递归列出），等于该目录下的所有文件都会显示出来
```

- mv命令

mv命令用于移动文件。

例子：

```
mv -f [file] [dir] # 移动file到dir，并直接覆盖同盟文件。

# -f：force是强制的疑似，指如果有同名文件已经存在，则不需要经过询问直接进行覆盖
# -i：指如果有同名文件已经存在，需要询问是否覆盖
# -u：指如果有同名文件已经存在，且比已经存在的同名文件更新，才会进行更新

mv [file1] [file2] [file3] dir # 把文件"file1"、"file2"、"file3"移动到目录"dir"中去

mv [file1] [file2] # 注意是把文件"file1"冲命名为"file2"
```

- rm命令

rm命令用于删除文件或者目录。

例子：

```
# -f：删除时，忽略不存在的文件，不会出现警告信息
# -i：互动模式，删除前会询问用户是否确认该操作
# -r：递归删除，将路径下的所有文件和目录全部删除，非常危险。

rm -i [file] # 删除文件"file"，在删除前会询问是否进行该操作

rm -fr [dir] # 强制删除目录dir中的所有文件
```

- apt-get命令

apt（Advance Package Tool）是Linux系统中常用的高级软件工具，用来安装和卸载软件。

例子：

```
# sudo指的是使用管理员权限
sudo apt-get install [software] # 安装[software]软件

sudo apt-get remove [software] # 卸载[software]软件

sudo apt-get update [software] # 更新软件列表

sudo apt-get upgrade [software] # 更新已安装软件
```

- 其他常用命令

```
cat /proc/version # 查看系统版本

cat /proc/cpuinfo # 查看主板版本

df -h # 查看SD卡空间使用情况

ifconfig # 查看IP地址

sudo -i # 直接登录root账户，没有目录和文件，只有根目录也就是"~"

sudo su # 直接登录root账户，保留所在路径目录和文件

exit # 退出root账户，或者关闭命令终端

touch 路径 # 新建文件
```

#### Linux内置文本编辑器Vim命令语法

Linux中的Vim就相当于Windows系统中的记事本，是常用的文本编辑器。

语法：

```
vim [filename] # 使用Vim打开相应的文件进行编辑
```

Vim有两种模式：

- 命令模式：在命令模式中可以移动光标、删除字符，但是不可以输入字符。还可以保存文件，设置或退出Vim。

常用命令语法如下：

```
vim [filename] # 使用Vim打开相应的文件进行编辑

:w # 保存文件

:q # 退出Vim，用于未修改文件时

:q! # 强制退出Vim，不保存文件

:wq # 保存并退出

x # 删除当前字符

nx # 删除从光标开始的n个字符

dd # 删除当前行

ndd # 向下删除当前行在内的n行

u # 撤销上一步操作

U # 撤销对当前行的所有操作

yy # 将当前行复制到缓冲区

nyy # 将当前行向下n行复制到缓冲区

yw # 复制从光标开始到行尾的字符

nyw # 复制从光标开始的n个单词

y^ # 复制从光标到行首的字符

y$ # 复制从光标到行尾的字符

p # 粘贴剪切板里的内容到光标之后

P # 粘贴剪切板里的内容到光标前

:set nu # 显示行号

:set nonu # 取消显示行号
```

- 插入模式：在插入模式中可以输入字符，按下ESC进入命令模式。

常用命令语法如下：

```
a # 在当前光标位置的右边添加文本

i # 在当前光标位置的左边添加文本

A # 在当前行的末尾位置添加文本

I # 在当前行的开始位置添加文本（非空字符行首）

O # 在当前行上面新建一行

o # 在当前行下面新建一行

R # 替换（覆盖）当前光标位置以及后面的若干文本

J # 合并光标所在行及下一行为一行
```

#### Permission denied报错问题解决方案

这个报错的意思是权限不够。在Linux系统里，使用sudo作为命令的开头将会拥有管理员权限，可以避免权限不够的报错问题。

### 树莓派Raspbian 11系统的常识

波浪号`~`代表的是当前登录的账户的家目录路径的省略形式，在系统里等效为`/home/账户名/`，pi为默认账户名，这个文件夹保存着用户的个人设置文件。

文件名字前带有句号`.`的文件是隐藏文件，在命令行终端可以正常访问和操作，在图形化界面使用Ctrl+H开启/关闭查看隐藏文件。

命令前带有的sudo是指通过管理员权限运行，树莓派的第一个默认账号"pi"是拥有管理员权限的账号，但并没有root权限。

shell就是命令行终端窗口，不同的shell可以拥有不同的设置和环境。在关闭一个shell之后这个shell的所有设置和环境都会被清除。

每次进入系统时都需要进行一次```source ~/.profile```以配置应用的环境变量，source命令是使执行结果可以反映到本shell的执行脚本命令，而```~/.profile```这个文件通常是树莓派系统的各种应用的路径和环境变量的配置文件。（在其他Linux系统里，充当这个文件作用的一般是~/.bash_profile）

nano基本使用快捷键：从nano编辑模式保存并退出的方法：先Ctrl+O保存，然后Enter退出，或者Ctrl+X退出，询问是否保存选择Y，然后Ctrl+T选择另存为的文件（如果只是保存就直接按Enter键）。

Linux系统快捷键：

Ctrl+Alt+T   呼出命令行终端

Ctrl+C   强制中断任何正在执行的命令或程序

Ctrl+Z   强制暂停部分可以暂停的程序的执行

Ctrl+H   使当前文件管理器显示/关闭显示隐藏文件

### 通过电脑端远程控制树莓派图形化系统

如果有独立的显示器（部分笔记本不支持作为树莓派的显示器），通过电脑端远程控制是更好和方便的选择。

使用读卡器，在Windows下可见的boot路径根目录创建一个无后缀无格式文件，命名为"ssh"，这样就开启了ssh服务。完成后把SD卡插回树莓派启动树莓派。除此之外，在树莓派系统内的命令行终端内运行```sudo /etc/init.d/ssh start```命令也能开启ssh服务，但是因为第一次树莓派连接通常都是通过ssh进行远程控制的，因此还是建议第一种方法。

然后，需要获得树莓派的局域网ip地址，这样才能通过电脑端与树莓派组成局域网进行远程控制。

获取树莓派ip的办法一是直接使用网线连接电脑和树莓派组成有线局域网，二是使用手机热点作路由器使电脑和树莓派组成无限局域网。（树莓派需要开机才能组成局域网和获得树莓派的局域网ip地址）

#### 通过网线组成有线局域网下获得树莓派有线局域网ip地址方法

使用IP Scanner软件进行ip扫描得到，重点是192.168.137.X网段（通过网线组成有线局域网的默认网段）。扫描树莓派有线局域网ip地址不一定要插入烧录了系统的SD卡。

树莓派第一次进行网线连接时扫出的有线局域网ip地址一定可用，且会维持一段时间的静态。在断电一定时间后树莓派的有线局域网ip地址会发生变化，需要进行重新扫描（第二次及以后扫出的有线局域网ip地址不一定可用）或者固定有线局域网ip地址。

注意：不要在电脑端连接校园网的情况下扫描（校园网是比较特殊的局域网，扫描会有很多ip地址干扰）。应当使电脑端连接手机个人热点（ip地址占用较少）或者不连接互联网（仅扫描局域网）的情况下进行扫描。

IP Scanner第一次扫描出名称为raspberrypi的ip地址可直接使用ssh服务进行远程控制。但是每次扫描出名称为raspberrypi的ip地址均不会删除，因此每次新出现的raspberrypi的ip地址是真实的（但问题是有时候扫不出来，就是ip地址被分配到另外的特殊网段了，此时约等于无法得到确切的ip地址）。如果没有固定ip地址，树莓派的局域网ip地址仍然可能会被分到其他特殊的ip网段。

连接wifi形成无线局域网时的网段由局域网的路由器也就是网关设备决定（很有可能是特殊的网段）。而如果运气不好，组成有线局域网时树莓派被分配的ip地址可能也会跳到比较特殊的网段。注意IPScanner默认不会对一些特殊网段进行扫描，因此固定ip地址才能安心进行树莓派的调试。

树莓派拥有的多个物理地址（MAC地址）是相同的（例如这块4B均以e4开头），与电脑端其他普通的地址有着明显不同。

#### 验证树莓派已与电脑端建立连接的方法

一般来说，已经组成稳定的连接的标志是能够通过VNC Viewer进行图形化远程控制。

- 可以使用Windows命令行窗口指令验证连接：

```c
ping -4 raspberrypi.local 
//IPV4局域网地址

ping -6 raspberrypi.local 
//IPV6局域网地址，仅可以使用putty通过ssh进行远程控制，但不能使用VNC进行图形化远程控制。

//只要树莓派成功在同一局域网，两个指令其中一个必定生效。用于判断树莓派是否与电脑端组成局域网。
//使用ping得到的IPV6的地址均可以进行树莓派的ssh登录，但不能进行图形化远程登录。
//如果两种ip地址均ping通，则既可以进行ssh远程控制，也可以进行图形化系统远程控制。
//在插入网线的时候，ping树莓派得到的ip地址优先遵循有线局域网时的分配方式。
//但是putty和VNC远程控制为优先遵循无线局域网的ip地址分配方式。
```

- 可以使用putty的ssh服务远程控制来验证：打开putty，输入树莓派的局域网IPV4地址，如果可以进行登录，则也可以通过VNC进行图形化远程控制。

- 在组成有线局域网的情况下（无线局域网的情况下arp-a检测不出树莓派的局域网ip地址），可以随时使用电脑端cmd的arp -a命令检测树莓派是否与电脑端组成有线局域网（通过树莓派独特的MAC地址进行辨别），在此命令下显示的树莓派的ip地址才"有可能"可以进行远程控制。arp -a命令检测出的ip地址后面显示的静态或者动态是无关紧要的（一般情况下每次重启都会变化的ip地址显示为静态，手动固定的ip地址显示为动态）。

#### 通过ssh服务进行树莓派远程控制（为VNC图形化远程控制做准备工作）

在获得确切的树莓派的有线局域网IPV4地址后，就可以通过putty的ssh服务连接树莓派（如果组成了无线局域网，则输入树莓优先局域网ip地址，注意可以使用IPV6地址）后，登录过程如下：

```
login as：默认账号：pi
pi@ ip地址 's password：默认密码：raspberry
（注意此处输入密码的字符与背景同色，也就是看不到打出来的密码的字符）
```

成功登录后，如果需要使用网络，是需要让电脑端的无线网共享才能做到（树莓派共享了电脑连接的WiFi）。具体步骤上网查询，非常简单。

第一次进入后，需要依次一条一条地执行下列语句，为开启图形化界面远程桌面连接作准备工作。

```
sudo apt-get install tightvncserver  //安装vncserver

sudo apt-get install xrdp  //安装xrdp远程桌面服务

sudo /etc/init.d/xrdp start  //开启xrdp服务

sudo update-rc.d xrdp defaults //将xrdp服务加入到默认系统启动列表

sudo raspi-config //打开interface选项页面，把ssh和VNC服务均使能也就是开启，并在其他选项里把VNC分辨率调整为1280 * 720（分辨率不对可能VNC连接会失败）。
```

然后可以在使用putty连接ssh的情况下，打开电脑端的"远程桌面连接"功能，输入树莓派ip地址，进行桌面远程控制。

如果发生错误，则使用VNC Viewer，直接输入树莓派的IPV4局域网地址登录即可。

#### 为树莓派配置无线网络

烧录好系统并连上电源和外设（包括显示器、鼠标、键盘）进入图形化系统，或者通过电脑端图形化远程控制，就可以直接操作了。

在设置新系统的途中，可以设置新的账号密码，可以修复任务栏的显示问题。

接下来，就是配置无线网络，才能让树莓派得到用武之地。

```
rfkill unblock wifi //启用wifi

sudo ifconfig wlan0 up //激活网卡

sudo iwlist wlan0 scan //扫描并列出可连接的wifi
```

如果还是没能检测到wifi信号，则可以直接在命令端口执行以下操作：

```py
nano /etc/wpa_supplicant/wpa_supplicant.conf
# 创建并跳转到设置登入的wifi名称及密码的配置文件，进入nano界面

# 配置文件内容如下：

ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=CN
network=
{
ssid="Odin233" # wifi名称
psk="114514810" # wifi密码
key_mgmt=WPA-PSK # wifi类型，可以在"属性"查看，此行最好不加。
priority=1 # 连接wifi优先度，数字越大优先度越高，此行最好不加
}
#注意：树莓派搜索的wifi名称如果含有中文名，会变成乱码。应尽量使wifi或者热点的名称只包含英文和特殊符号。
# 注意此自动连接WIFI只看wifi名字和密码正确与否，不看设备是否是同一部。
```

然后重启树莓派，再次使用VNC连接，查看是否已经连接上wifi。（GUI图形化界面的右上角处，即使树莓派通过网线连接到了电脑端，其有线连接共享网络的图标也会变成WiFi图案）

这样，以后每次重启树莓派都会自动连接相应的wifi了。

有时候，会在树莓派长时间没有启动的情况下，自动连不上wifi，此时需要连上网线进行一次有线连接（可以在VNC里面搜索WIFI效果更好），这样可以在一边通过网线远程控制树莓派，一边用wifi作为树莓派的网络，同时自动连接wifi的功能也会恢复正常。

#### 通过手机热点组成无线局域网下获得树莓派无线局域网ip地址方法

安卓手机可以查看树莓派连接到热点的局域网ip地址，最好使用安卓手机作热点（局域网路由器/网关）。

苹果手机没有显示热点用户的相关功能的接口，需要使用Fing软件（手机和电脑版均有）进行监控获得树莓派和苹果手机的默认作为局域网网关时的ip地址所处网段（虽然树莓派大多数时候是扫不出来的，但是只要知道苹果手机的热点的默认局域网网段即可）。

只要知道了所属网段，就可以通过设置静态ip地址来一劳永逸了。

注意静态ip地址只能同时服务于一个网段的静态地址，也就是只认一个特定热点的网段。

#### 固定无线局域网ip地址

得到一个特定网段的ip地址后，为了防止下次通电ip地址的改变，需要设置静态IP地址。

在树莓派的命令行终端下：

```
ifconfig 
//查看当前有线IP地址和所属网段，取每个接口的第一个inet地址也就是IPV4地址的前三段数字为默认网段（除非被人为固定过IP）。wlan0的inet和inet6分别对应此时树莓派的WiFi连接时的IPV4的ip地址和IPV6的ip地址。eth0的inet和inet6分别对应此时树莓派的网线连接时的IPV4的ip地址和IPV6的ip地址。一般使用IPV4地址进行VNC远程控制，IPV6地址使用ssh进行远程控制。也可以使用ifconfig wlan0仅查看无线的ip信息。

sudo nano /etc/dhcpcd.conf 
// 编辑此文件的结尾被注释的设置静态ip地址的代码，把开头的注释符"#"去掉，然后进行改动：
// 改动的内容如下：

interface wlan0
//wlan0指设置的是无线的静态IP地址，而eth0指设置的是有线的静态IP地址

static ip_address=172.20.10.6/24
//172.20.10.6、192.168.43.137为自定义的树莓派的静态IP地址（需要与无线局域网网关设备的局域网ip地址处于同一网段内，且没有其他设备占用的地址），/24代表子网掩码为255.255.255.0，不需要进行更改。

static ip6_address=fd51:42f8:caae:d92e::ff/64
//此行为设置IPV6的静态ip地址。不需要进行改动，没有影响。

static routers=172.20.10.1
//此行填写的是当进行无线连接时，提供信号的主体设备的局域网IP地址。也就是："WIFI主机信号源/局域网的总路由器/局域网的网关/局域的DNS服务器"的IP地址。这个IP地址可以通过Windows系统连接上WIFI后的"属性"一栏查看。我的苹果手机默认是172.20.10.1，我的安卓三星默认是192.168.43.1。

static domain_name_servers=192.168.0.1 8.8.8.8 fd51:42f8:caae:d92e::1
//此行为DNS设置，不需要进行改动，没有影响
```

类似的，有线局域网静态IP地址设置操作（无效）：

```
interface eth0
static ip_address=192.168.137.43/24
static ip6_address=fd51:42f8:caae:d92e::ff/64
static routers=192.168.137.1
//比较奇怪的是，无论是Windows的"属性"一栏，还是使用Fing软件进行扫描，使用网线连接时的网关显示均是不准确的。
static domain_name_servers=192.168.0.1 8.8.8.8 fd51:42f8:caae:d92e::1

//Ctrl+O保存，Enter确定，Ctrl+X退出nano。
//然后重启树莓派生效。
//手动设置的静态IP不能跟路由器DHCP之前所自动分配的IP重复，否则树莓派就有可能无法正常联网。
```

如果尝试固定有线局域网IP地址的话，使用上述方法是无效的。但是如果第一次使用网线进行连接，则树莓派会保持一定时间的静态ip，但是一段时间后就会变成动态ip从而无法得到树莓派的ip地址。同时，此时使用IPScanner和Fing软件也扫描不出准确的ip地址。

#### 固定有线局域网ip地址

因此，需要拔出树莓派的SD卡，在boot根目录的"cmdline.txt"的文件开头添加我们想要固定的有线静态IP地址：

```
ip=192.168.137.X
//注意记得和原来的内容添加一个空格隔开
```

一般我们通过网线共享的网络，默认使用192.168.137.0/255作为局域网地址，192.168.137.1作为网关（Gateway），因此可以直接ip=192.168.137.X。如果想要改动，则需要将电脑端的以太网的"属性"里面的"Internet协议4 TCP/IPV4"协议的手动ip地址改成想要的网段的第一个地址（192.168.XXX.1），然后将上述的静态ip地址设置为与以太网IPV4的地址的相同的网段才行。

### 安装opencv（使用pip安装）

我们可以从opencv官网上下载源码并进行编译安装，但不方便。

使用pip工具来安装和管理python软件包则是一种快速便捷的方法。PyPI（Python Package Index）是Python官方的第三方库的仓库，而pip（官方网址："https://pypi.org/project/pip/"）是PyPI的推荐的Python包管理工具，提供了对Python包的查找、下载、安装、卸载的功能。同时，pip是跨平台的，在某些应用中，不止在单片机上还要在宿主机上进行安装。

因此下面介绍Ubuntu系统、macOS、树莓派系统的pip安装opencv的方法。（注意：使用pip安装的opencv不包含opencv中具有专利的算法，因为是Python官方而不是opencv官方的库）

通过pip工具安装opencv具有4种版本可选：
- opencv-python：只提供opencv的基础模块，不建议安装。
- opencv-contrib-python：提供了opencv的基础模块和扩展模块，基本上包含了opencv的全部功能，建议安装。
- opencv-python-headless：在opencv-python的基础上去掉GUI（用户图形界面）的版本，适用于命令行系统。
- opencv-python-contrib-headless：在opencv-contrib-python的基础上去掉GUI（用户图形界面）的版本，适用于命令行系统。

无论是那种系统，都有两个安装方案：
- 将opencv安装在Python的全局环境中
- 将opencv安装在虚拟环境中（推荐）

#### 树莓派系统

准备工作：

```
sudo apt-get install aptitude # 这是一个比较方便的依赖项安装工具，具体用法：sudo aptitude install 文件名/依赖名（就像普通的apt-get的用法一样）

sudo apt-get install build-essential cmake git pkg-config # 安装CMake开发人员工具，即使使用的是Python也可以使用
```

安装Python（如果系统是Raspberry Pi 11则自带python3，无需额外安装）：

```
# 安装Python3。
sudo apt install python3

# 删除掉原先 python 的链接。
sudo rm /usr/bin/python

# 创建一个新的链接指向刚刚安装的 python3。
sudo ln -s /usr/bin/python3 /usr/bin/python

# 测试，若能正常输出python3x版本则安装成功。
python3
```

安装依赖项的时间设置不一致，源的不同，DNS设置不正确等都会导致出错。建议连接无线网络而不是网线进行安装。

先确定树莓派系统的大版本是什么，再进行源的更换：stretch、jessis、buster、bullseye等等。可以通过指令```lsb_release -a```查询。

自带的源在海外，访问速度比较慢，更换成阿里云的源后安装软件速度会提升很大。

设置DNS的方法：

```
sudo nano /etc/dhcpcd.conf # 就是设置静态ip地址的文件

# 在设置的静态ip地址的DNS地址行改写内容为：

static domain_name_servers=192.168.1.1 8.8.8.8 8.8.4.4 # 添加了两个新的DNS地址

# 然后依次运行下列命令：

sudo service dhcpcd restart

sudo systemctl daemon-reload

sudo /etc/init.d/networking restart

# 成功的话会显示：Restarting networking (via systemctl): networking.service.

# 然后重启树莓派即可。
# 改resolv.conf文件其实也可以，但是这个文件是指向dhcpcd.conf文件的，所以这样更好。
```

更换源的方法：
```py
sudo nano /etc/apt/sources.list # 这个文件记录了树莓派安装和获取依赖项和库的文件源网址URL

将本文件内的全部内容删除，并替换为：

deb [arch=armhf] http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ bullseye main non-free contrib rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ bullseye main non-free contrib rpi
# 2022.2.26源有效，仅少了libqtgui4相关的依赖项，这个项限制已经不需要了

# 接下来换另一个

sudo nano /etc/apt/sources.list.d/raspi.list # 这个文件记录了一个镜像地址

将本文件内的全部内容删除，并替换为：

deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ bullseye main

# 更换完源后需要进行更新：

sudo apt-get update # 更新软件列表，使依赖项和库的源得到更新

sudo apt-get upgrade # 更新软件
```

然后安装opencv需要的依赖项（注意，根据源的不同和时间的更迭，包的名字可能会发生变化，应寻找安装时的博客了解需要的包和新的名字）：

```
sudo apt-get update

sudo apt-get upgrade

sudo apt-get install libhdf5-dev libhdf5-serial-dev libhdf5-103 python3-pyqt5 # 注意版本

sudo apt-get install libqtgui4 libqtwebkit4 libqt4-test #这些库疑似已经被删除/淘汰/更新了，目前最新的源没有这些依赖项

sudo apt-get install libatlas-base-dev libjasper-dev

sudo apt-get install libjpeg9-dev libjasper-dev libpng-dev libglu1-mesa-dev libavcodec-dev libavformat-dev libxvidcore-dev libx264-dev libgtk2.0-dev libatlas-base-dev gfortran

# 多种图片格式支持包
sudo apt-get install -y libjpeg-dev libtiff5-dev libjasper-dev libpng-dev

# 视频支持包（支持视频文件 & 视频串流）
sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev

# OpenCV的子包highgui（用于图像处理）所必需的GTK development library相关包
sudo apt-get install -y libfontconfig1-dev libcairo2-dev libgdk-pixbuf2.0-dev libpango1.0-dev libgtk2.0-dev libgtk-3-dev

# 加速opencv矩阵运算的包
sudo apt-get install -y libatlas-base-dev gfortran

# 编译opencv+python时所需的python头文件
sudo apt-get install -y python3-dev
```

Raspbian 11版本的树莓派才适用的直接安装方法：

```
sudo pip3 install --upgrade numpy # 更新numpy
sudo pip3 install opencv-python # 直接安装opencv
```

通过wget的方法安装pip工具，并在虚拟环境中安装python和opencv：

```py
wget http://bootstrap.pypa.io/get-pip.py #获得安装pip的脚本

sudo python3 get-pip.py # 调用安装pip的脚本

# 将opencv安装到Python的全局环境中：

sudo pip install opencv-contrib-python
```

```py
wget http://bootstrap.pypa.io/get-pip.py #获得安装pip的脚本

sudo python3 get-pip.py # 调用安装pip的脚本

# 将opencv安装到虚拟环境中（推荐）：

sudo pip install virtualenv virtualenvwrapper # 安装虚拟环境管理工具virtualenv和virtualenvwrapper

sudo rm -rf ~/get-pip.py ~/.cache/pip # 使用CMake安装OpenCV4的时候在这一步执行，pip安装忽略这一句命令。

sudo nano ~/.profile # 打开这个文件，这类似于Ubuntu下的~/.bashrc和macOS下的~/.bash_profile

# 并在文件的末尾加上以下语句，保存退出：

export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
source /usr/local/bin/virtualenvwrapper.sh

# 然后使用下列命令确认，没有报错消息即可：
source ~/.profile
# 或者使用：virtualenv --version获取virtualenv版本

# 这样已经安装好管理虚拟环境的工具了，回到opencv的安装。
```

```
# 创建一个虚拟环境，可以自己命名，此处我命名为p3v31
mkvirtualenv p3v31 -p python3

# 激活虚拟环境，此时命令行头会出现（虚拟环境名）的字样
workon p3v31

# 如果切换失败，或者找不到virtualenv相关的命令（有时树莓派重启后会发生），执行下列语句：
source ~/.profile
workon p3v31

# 安装opencv：
pip install opencv-contrib-python
# 需要选择特定版本安装时，格式为pip install opencv-contrib-python==版本号（注意查看python与opencv的版本互相兼容问题，opencv安装包后面的pyxx就是最高支持的python的版本，同时随便使用一个版本号可以看到pip的库里面有什么版本还存在）。
```

```
# 检查opencv的安装是否成功：
cd ~ # 转移到根目录

workon p3v31 # 激活虚拟环境

python # 打开python

>>>import cv2 # 注意版本，4.5.5版本时依然为cv2，其他版本不明cv?

>>>cv2.__version__ # 注意版本
# 如果输出opencv版本则安装成功。
```

```
# 退出虚拟环境
deactivate

# 删除虚拟环境（必须先退出虚拟环境内部才能删除当前虚拟环境）
rmvirtualenv 虚拟环境名

# 退出Python的命令
quit() 
# 或者exit(), 或者Ctrl-D
```

### 安装opencv（源码编译安装OpenCV4）

准备工作：

```
sudo raspi-config # 选择"Advanced Options"的"Expand File System"，然后重启树莓派。这样可以拓展TF卡，使用整个micro-SD卡的空间，为opencv4的安装做准备。

df -h # 查看系统中空间使用情况
```

安装开发者工具：
```
sudo apt-get install build-essential
sudo apt-get install cmake
sudo apt-get install unzip
sudo apt-get install pkg-config
```

安装必要的依赖：
```
# 详见上方pip安装时的依赖项，加上：
pip install numpy # 使用pip工具安装这个必要的包
```

安装GTK（GUI的后端）：
```
sudo apt-get install libgtk-3-dev
sudo apt-get install libcanberra-gtk* # 可以减少GTK使用时的错误
```

安装一些可以优化opencv的包：
```
sudo apt-get install libatlas-base-dev
sudo apt-get install gfortran
```

最后安装python3 development headers（开发者头文件）：
```
sudo apt-get install python3-dev
```

然后，切换到用户主目录，下载opencv和opencv_contrib，opencv中包含了OpenCV4的基本功能，而opencv_contrib包含了很多常用的模块和函数，因此需要同时安装两部分。
```
cd ~ # 切换到主目录
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.0.0.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv/archive/4.0.0.zip
```
然后分别进行解压：
```
unzip opencv.zip
unzip opencv_contrib.zip
```
解压完成后重命名目录：
```
mv opencv_4.0.0 opencv # 将opencv_4.0.0转移到"opencv"重命名的文件夹中了
mv opencv_contrib-4.0.0 opencv_contrib # 同理，设定好路径便于CMake。
# 如果跳过重命名这一步，之后配置CMake路径时需要改成相应的目录。
```

安装虚拟环境，详见上方虚拟环境安装时的步骤。

#### 构建和编译OpenCV4（使用CMake）

首先，在~/opencv目录下创建一个build子目录：
```
cd ~/opencv
mkdir build # 创建文件夹build
cd build # 打开文件夹build
```

使用CMake来构建OpenCV4：
```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_co-D ntrib/modules \
-D ENABLE_NEON=ON \
-D ENABLE_VFPV3=ON \
-D BUILD_TESTS=OFF \
-D OPENCV_ENABLE_NONFREE=ON \
-D INSTALL_PYTHON_EXAMPLES=OFF \
-D BUILD_EXAMPLES=OFF ..
```
OPENCV_ENABLE_NONFREE=ON这个选项，设置为True时，可以使用SIFT、SURF以及其他具有专利的算法。

在这里，要确认OPENCV_EXTRA_MODULES_PATH的目录是之前下载的opencv_contrib的解压目录，如果自定义了解压目录则上述的CMake代码需要相应的修改。

接下来是编译的准备工作，由于编译需要的时间以小时为单位，因此需要扩大一下交换空间（swap space），可以使得树莓派在编译时用上全部的4个核。

```
sudo nano /etc/dphys-swapfile # 打开这个文件

# 将CONF_SWAPSIZE的数值改为2048(默认为100MB)
# 然后保存退出

sudo /etc/init.d/dphys-swapfile stop
sudo /etc/init.d/dphys-swapfile start
```
扩大交换空间可能会烧毁micro-SD卡，由于Flash存储器对数据写入有一定的限制。因此需要提前备份系统文件。

接下来，开始编译：
```
make -j4 # -j4代表使用4个核来编译，如果报错把-j4去掉

# 接下来是漫长的等待时间...编译完成且没有报错以后，执行以下命令进行收尾

sudo make install # 安装OpenCV4
sudo ldconfig

# 然后记得把交换空间大小改回100MB，并重启swap服务。
```

在Linux命令行下，输入字符后，按两次Tab键，shell就会列出以这些字符打头的所有可用命令。如果只有一个命令匹配到，按一次Tab键就自动将这个命令补全。当然，除了命令补全，还有路径、文件名补全。在cd 到特定目录时特别好用。

最后，需要将OpenCV4链接至创建的Python3虚拟环境中，这一步至关重要。要把OpenCV4链接至虚拟环境的site-packages。为了保证链接路径适应于不同的树莓派，在输入下列命令的过程中建议使用Tab键自动补全目录：

```
cd ~/.virtualenvs/虚拟环境名/lib/python版本号/site-packages/

ln -s /usr/local/python/cv2/python-版本号/cv2.cpython-35m-arm-linux-gnueabihf.so cv2.so

cd ~
```

最后，测试OpenCV4：

```
# 检查OpenCV4的安装是否成功：

workon 虚拟环境名 # 激活虚拟环境

# 如果切换失败，执行下列语句：
source ~/.profile
workon 虚拟环境名

python # 打开python

>>>import cv2 # 注意版本，4.0.0版本时依然为cv2，其他版本不明cv?

>>>cv2.__version__ # 注意版本，cv?
# 如果输出OpenCV4版本则安装成功。
```

## OpenCV图像处理（入门操作）

### 准备工作

安装好opencv后，还需要安装imutils这个包，之所以现在才装是因为它需要在安装了opencv的同一个环境进行安装。虚拟环境下命令如下：

```
workon 虚拟环境名 # 先进入虚拟环境
pip install imutils # 安装imutils包
```

（可选）可以选择在Github上下载易于学习的材料和脚本，使用wget命令来下载（下载的文件默认路径为命令行终端的当前路径下）：

```py
wget 文件网址URL
cd ~/Downloads # 转移到默认下载路径
unzip 文件名.zip # 解压
cd 文件夹名 # 转移到解压出的文件夹内
tree # 分析当前目录结构
```

也可以自己新建py脚本文件，并进行编辑。在虚拟环境中：

```py
cd ~/Desktop/py # 定位到想要创建脚本的文件夹，~代指/home/pi/，如果想要定位到桌面的py文件夹，则cd ~/Desktop/py

sudo nano Test001.py # 创建名为"Test001"的py文件，并使用nano编辑器打开

# 编辑完成后，保存退出。
# 在虚拟环境中，使用以下指令运行python脚本

python3 ~/Desktop/py/Test001.py # 运行具体的路径的py文件
```

（可选）上述进入对应路径和对应虚拟环境等操作太过繁琐，需要写一个脚本来一次性执行，采用shell脚本，方法如下：

```py
# 当前目录存在：脚本名.sh
# 打开终端

sudo sh 脚本文件路径
```

当前，树莓派系统内的shell脚本内的cd命令不能跳转到根目录以下的路径，因此不好用。同时虚拟环境需要的每次重启设置source在脚本内也是无效的，因此暂时不使用shell脚本。

重启树莓派后需要的操作命令：

```
source ~/.profile
workon p3v31
# 激活虚拟环境

cd ~/Desktop/py
# 转移到py脚本文件所在的目录
```

### 加载和显示图片文件

加载预先准备好的图片文件，实现代码如下：

```py
# 加载输入图像并显示其尺寸
# 先导入必要的库,imutils是入门OpenCV的方便库，cv2是OpenCV第2版本的包，但是兼容以后的版本
import imutils
import cv2

i1=cv2.imread("image.jpg") 
# 数组i1被赋值为一个NumPy多维数组，这是图像的表示形式。
# "image.jpg"为当前目录下待进行处理的图片文件名
# 注意opencv读取路径时不能打错和出现中文字符。

(x,y,z)=i1.shape 
# .shape是cv2定义的属性。分别为：行数（height），列数（width），通道数（depth）。（其中灰度图通道数为1，默认不显示）

print("width={},height={},depth={}".format(y,x,z))
#输出尺寸，高度，宽度和通道数

cv2.imshow("ImageName1",i1)
# 将图像显示在屏幕上

cv2.waitKey(0) # 参数为0表示直到任意按键被按下，显示图像。参数不为0则表示图像停留的毫秒数。
# 需要点击OpenCV打开的窗口并按下任意键以继续运行其他程序

```

### 获取特定坐标的RGB值

图像的像素数为高度数 * 宽度数。灰阶图像中的每个像素有一个灰阶代表的值。在OpenCV中有256种灰阶。因此灰阶图像中有与每个像素相关联的灰阶值。灰阶值从0到255越大，像素越亮。

而彩色图像中的像素还有另一层附加信息。为了方便，这里只考虑RGB色彩模式。在OpenCV中，RGB（红色，绿色，蓝色）被设置为3元组(B，G，R)的形式（历史遗留问题）。BGR3元组的每个值的范围为`[0,255]`RGB色彩模式下，三种颜色合计的数值越大，像素越亮。

因此接下来，是查找并访问图像中单个像素的值，实现代码如下：

```py
(a,b,c)=i1[100,50] #注意是BGR的3元组顺序进行赋值，注意坐标用中括号括起来
# 访问位于x=50，y=100坐标的RGB模式的像素

print("R={},G={},B={}".format(c,b,a))
# 根据RGB的顺序进行顺序，注意format后面的变量顺序
```

### 图像通道分离进行颜色识别

```py
import cv2
import numpy as np
import argparse

img01=cv2.imread("image01.jpg")
# 导入图片为numpy数组

(B,G,R)=cv2.split(img01)
# 给B，G，R三个numpy数组赋值三个通道的图像。


# cv2.split(m, mv):将一个多通道数组分离成几个单通道数组。
# m：我们需要进行分离的多通道数组。
# mv：函数的输出数组或者输出的vector容器。

cv2.imshow("001",R)
cv2.imshow("002",G)
cv2.imshow("003",B)
# 显示三个通道的图像

print("R_size:",R.shape,"G_size:",G.shape,"B_size:"B.shape)
# 输出三个图像

# 如果想要得到有颜色的对应通道图像，需要进行拓展通道：

Z = np.zeros(img01.shape[:2], np.uint8)
img_B = cv2.merge([B,Z,Z])
cv2.imshow("B_", img_B)
img_G = cv2.merge([Z,G,Z])
cv2.imshow("G_", img_G)
img_R = cv2.merge([Z,Z,R])
cv2.imshow("R_", img_R)
# 通道扩展（通过merge函数将有颜色非灰阶通道图进行合成，得到有颜色的通道图）

# "np.zeros"不能更改，它是cv2库里的关键词，代表的是一个空的通道图像。
# np代表的是"numpy"库的缩写，在import numpy as np定义了其缩写，也就是说，也可以用numpy代替。
# uint8是专门用于存储各种图像的（包括RGB，灰度图像等），范围是从0~255。
# numpy.zeros(shape,numpy.type,order)，创建给定形状（shape）和存储类型（type）的全零数组/矩阵，order只有'C'和'F'可选，C是按行存储，F是按列存储。因此对应颜色的数组和Z数组合成其实就等于仅显示对应颜色的通道图了。

# img01.shape[:2]是取彩色图片的长、宽。
# img01.shape[:3]是取彩色图片的长、宽、通道。
# img01.shape[0]是取图像的垂直尺寸。
# img01.shape[1]是取图像的水平尺寸。
# img01.shape[2]是取图像的通道数。
# 在矩阵中，[0]表示行数，[1]表示列数。

cv2.waitKey(0)
cv2.destroyAllWindows()

# 分离出来的三个通道图像均为灰阶化的通道图。通道图像中的对应颜色会表现为白色，通道图像中的非对应颜色或者说对应颜色的值较小的会表现为黑色。（其实就是图片中的每个像素都只取对应颜色的0~255的通道值，越接近255越白，越接近0越黑）

# merge()函数是split()函数的逆向操作：将多个数组合并成一个多通道的数组。它通过组合一些给定的但通道数组，将这些孤立的单通道数组合并成一个多通道的数组，从而创建出一个由多个单通道阵列组成的多通道阵列。
```

### 数组切片和裁剪（提取图像的局部切片）

提取"感兴趣区域（ROI）"是图像处理的一个重要技能。现在进行手动提取ROI，实现代码如下：

```py
i2=i1[150:250,100:300]
# 从图像数组中提取从x=100,y=150开始，结束于x=300,y=250的图像数组切片，格式为：图像数组名[开始Y:结束Y,开始X:结束X]

cv2.imshow("ImageName2",i2)
# 显示提取出的图像局部切片
cv2.waitKey(0)
```

### 调整图片的大小

更大的图像可以适应屏幕，更小的图像可以使需要处理的像素变少，在进行深度学习时很有用处。因此调整图片大小很有必要。

```py
i3=cv2.resize(i1,(200,200))
# 将图像调整大小为200*200px，并赋值给另一个NumPy数组i2

cv2.imshow("ImageName3",i3)
cv2.waitKey(0)
# 此种方法忽略了纵横比，图像会失真，像是被裁剪了一样。
```

因此，我们需要在调整图像的大小的同时，保留原始图像的纵横比，使其不会被压扁和扭曲，实现代码如下：

```py
# 固定调整大小和扭曲纵横比
# 具体方法为：输入想要的宽度，根据原图像的纵横比计算出相应的高度即可

A=300.0/y 
# y是前面步骤得到的原图像宽度，A为前后图像的宽度比率

B=(300,int(x * A)) 
#x是前面步骤得到的原图像高度，B是将用于resize函数的尺寸

i4=cv2.resize(i1,B)
# 将图像调整大小为300宽度下符合纵横比的图像，并赋值给另一个NumPy数组i3

cv2.imshow("ImageName4",i4)
cv2.waitKey(0)
```

### 旋转图像

实现代码如下：

```py
# 首先需要手动计算出图像的中心
# 然后构造出旋转矩阵，最后执行（给每个像素都执行一次，坐标变化后效果就等同于旋转了）

C=(y//2,x//2) 
# 通过高度和宽度各自除以2，得到图像中心点像素的坐标c。//是整数运算，不会留下小数位。

M=cv2.getRotationMatrix2D(C,-45,1.0) 
# 通过图像中心和将图像顺时针旋转45度，得到相应的旋转矩阵。1.0为缩放比例，1.0为保持原样缩放。

i5=cv2.warpAffine(i1,M,(y,x))
# i1是输入图像，m是变换矩阵，(y,x)是图像的大小（宽度，高度）

cv2.imshow("ImageName5",i5)
cv2.waitKey(0)

# warpAffine原函数：cv2.warpAffine(src,M,dsize[,dst[,flags[,borderMode[,borderValue]]]])->dst，src代表输入的图像数组，M代表变换矩阵，dsize代表图像数组的尺寸（宽度，高度），之后就是非必须的：flags代表插值方法的组合（int类型参数），borderMode边界像素模式（int类型参数），borderValue边界填充值（默认为0）。

# 其中中括号"[]"括住的内容是表示"非必须"的可选的形式参数。而这个中括号的嵌套可以通过先将所有中括号去掉来理解。(a,b,c,d)变成(a[,b,c,d])，也就是必须的参数只有a，而bcd均为可选的。然后变成（a[,b[,c,d]]）也就是当可选的b被传递时，才判定后面的[,c,d]，也就是只有当b被传递，cd才可选。这样到（a,[b,[,c[,d]]]）这种形式，就等于可以一个一个按顺序按需求添加参数，但是必须的参数只有a。同时，这种函数里bcd一般应该有个缺省值（也就是默认值）。
```

只是，这种直接的旋转会使图像遭到裁剪，OpenCV并不关心图像是否被裁剪和完整，如果想要使整个图像处于视野内，可以自定义函数，实现函数如下（未试验过）：

```py
import cv2
from math import *

def rotate_bound_advance(ima,degree)
    
	(height,width) = img.shape[:2]
	# 提取目标图像的长宽
    
	heightNew = int(width * fabs(sin(radians(degree))) + height * fabs(cos(radians(degree))))
	#
	
	widthNew = int(height * fabs(sin(radians(degree))) + width * fabs(cos(radians(degree))))
	#
	
	matRotation = cv2.getRotationMatrix2D((width / 2, height / 2), degree, 1)
	#
	
	matRotation[0, 2] += (widthNew - width) / 2
	matRotation[1, 2] += (heightNew - height) / 2
	imgRotation = cv2.warpAffine(img, matRotation, (widthNew, heightNew), borderValue=(255, 255, 255))
	#
	
	cv2.imshow("img", img)
	cv2.imshow("imgRotation", imgRotation)
	cv2.waitKey(0)
# 输出图像

image=cv2.imread("image.jpg")
rotate_bound_advance(image,45)
# 函数调用测试
```

也可以使用初学者库imutils库的bound函数，实现代码如下：

```
i5=imutils.rotate_bound(i1,45)
#调用bound函数，输入待旋转图片和角度即可

cv2.imshow("ImageName5",i5)
cv2.waitKey(0)
```

### 平滑图像（模糊图像）

在许多图像处理流程中，必须模糊图像以减少高频噪声，使得算法更容易检测和理解图像内容，以免算法被噪声"干扰"，在OpenCV中GaussianBlur函数可以轻松实现：

```
# 降低高频噪声时非常有用

i6=cv2.GaussianBlur(i1,(11,11),0)
# 使用11*11的卷积核进行高斯模糊，使图像趋于平滑。较大的卷积核会产生更模糊的图像，较小的卷积核会产生较轻微的图像。

cv2.imshow("ImageName6",i6)
cv2.waitKey(0)
```

### 在图像上绘图（例如框框）

在进行绘图操作前，需要明白OpenCV的绘图操作会将原图替换掉，因此每次绘图需要制作原始图像的副本并存为output，然后在output上绘制。

#### 矩形（框）

实现代码如下：

```
output=i1.copy()
# 建立原图像副本

cv2.rectangle(output,(0,100),(200,200),(0,0,255),2)
# opencv中绘制矩形的专用函数，参数分别为：（需要进行绘图并替换的图像，矩形左上角像素坐标，矩形右上角像素坐标，矩形颜色的BGR元组，线条粗细度（如果小于零也就是负值就统一为实心矩形））

cv2.imshow("ImageName7",output)
cv2.waitKey(0)
```

#### 圆形

实现代码如下：

```
# 在以x=170,y=110为中心的图像上，绘制一个蓝色的20像素（相当于填充）圆圈

output=i1.copy()
# 建立原图像副本

cv2.circle(output,(170,110),20,(255,0,0),-1)
# opencv中绘制圆形的专用函数，参数分别为：（需要进行绘图并替换的图像，圆心坐标，圆的半径，圆形颜色的BGR元组，线条粗细度（如果小于零也就是负值就统一为实心矩形））

cv2.imshow("ImageName8",output)
cv2.waitKey(0)
```

#### 直线

实现代码如下：

```
# 从x=200，y=100到x=100，y=200绘制5像素粗细的红线

output=i1.copy()
# 建立原图像副本

cv2.line(output,(200,100),(100,200),(0,0,255),5)
# opencv中绘制直线的专用函数，参数分别为：（需要进行绘图并替换的图像，起点坐标，终点坐标，直线颜色的BGR元组，线条粗细度）

cv2.imshow("ImageName9",output)
cv2.waitKey(0)
```

#### 文本

实现代码如下：

```
# 在图像上添加绿色文本

output=i1.copy()
# 建立原图像副本

cv2.putText(output,"Hello There",(10,25),cv2.FONT_HERSHEY_SIMPLEX,0.7,(0,255,0),2)
# opencv中绘制直线的专用函数，参数分别为：（需要进行绘图并替换的图像，要在图像上显示的字符串，文本的起点坐标，字体，字体大小，文字颜色BGR元组，线条粗细度）

cv2.imshow("ImageName10",output)
cv2.waitKey(0)
```

## 使用OpenCV进行图像中对象计数（基础案例解析）

### 准备工作

代码如下：

```py
import argparse # 一个命令行参数解析包，所有python版本都带有这个包，其在使用命令行窗口运行py脚本文件时带上特定的参数可以提供有用的功能，这个包可以自定义命令行参数
import cv2
import imutils

P=argparse.ArgumentParser() 
# 首先创建一个ArgumentParser对象，创建一个参数解析器。

P.add_argument("-i","--image",required=True,help="The path to input image") 
# 添加参数。自定义了一个参数"-i"，自定义了第二个参数为图像文件的路径参数"--I"（关键词为"--"），设置输入help时显示"The path to input image"。

T=vars(P.parse_args())
# 使用parse_args()方法解析参数，它将检查命令行，把每个参数转换为适当的类型然后调用相应的操作。vars() 函数返回对象object的属性和属性值的字典对象。

# 定义了脚本中的["I"]，就是指运行时输入图像的路径。
# 运行时：
python3 Test001.py -i ~/Desktop/py/image.jpg
# 脚本内：
i7=cv2.imread(T["I"])
cv2.imshow("ImageName11",i7)
cv2.waitKey(0)
```

命令行参数的使用方式：

```
python3 Test001.py -h
# 显示关于各参数的形式参数和帮助信息

python3 Test001.py -i ~/Desktop/py/image.jpg
# 上方例子自定义的参数用法，注意第二个是图片文件路径
```

### 将图片转化为灰阶图（仅有黑白程度的图片）

实现代码如下（待处理图片文件名为els.jpg）：

```
I1=cv2.imread("els.jpg") 
# 创建待处理图片els.jpg的NumPy数组

i8=cv2.cvtColor(I1,cv2.COLOR_BGR2GRAY) 
# 转化为灰阶图

cv2.imshow("ImageName12",i8)
cv2.waitKey(0)
```

### 边缘检测

实现代码如下：

```
i9=cv2.Canny(i8,30,150)
# 对灰阶图使用Canny算法，cv2.Canny的4个参数分别为：灰阶图像，最小闸值，最大闸值，Sobel算子的大小（默认为3）

cv2.imshow("ImageName13",i9)
cv2.waitKey(0)
```

### 二值化/闸值化灰阶图像

闸值处理是图像处理过程中的重要步骤，闸值处理可以去除较亮或较暗的区域并找到图像轮廓，还需要反复进行人工试验和调试，以达到较好的效果。

```
i10=cv2.threshold(i8,225,255,cv2.THRESH_BINARY_INV)[1]
# 注意是对灰阶图而不是边缘图进行处理。第二个参数225代表把灰度大于225的所有像素设置为0（黑色），把灰度小于225的全部设置为255（白色）。一般来说第二个参数越大越不灵敏，更容易让一个图形不被分割。

cv2.imshow("ImageName14",i10)
cv2.waitKey(0)
```

### 查找、计数并画轮廓线

轮廓线和边缘是不同的，先找出轮廓线：

```
L=cv2.findContours(i10.copy(),cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
# 使用cv2.findContours检测图像中的轮廓。

L=imutils.grab_contours(L)
# 因为cv2.findContours函数在不同的OpenCV版本中的实现不一样，因此可以使用imutils库的兼容性代码。

output=i8.copy()
# 建立灰阶图副本

for r in L:
# r是计数器，用于循环遍历全部的轮廓
  cv2.drawContours(output,[r],-1,(30,0,30),3)
  # 在指定图像上描绘每个轮廓，使用BGR元组(30,0,30)指定近黑色，3指定轮廓线的粗细程度
  cv2.imshow("ImageName15",output)
  # 每次按键会描绘下一个轮廓线
  cv2.waitKey(0)

Txt="I found {} objects!".format(len(L))
# 定义好字符串变量，记录轮廓的数量。len(L)代表轮廓列表L的长度，也就是目标总数。

cv2.putText(output,Txt,(10,25),cv2.FONT_HERSHEY_SIMPLEX,0.3,(0,0,0),1)
# 调用输出文本的函数，0.3代表字体大小，(0,0,0)指定黑色，1代表字体粗细

cv2.imshow("ImageName16",output)
cv2.waitKey(0)

# 轮廓往往都会画出并不期待的图像的轮廓，因为二值化后有非常多的噪点和类似的干扰物，因此可以通过面积大小进行过滤。计算面积的函数为contourArea()，可以通过对检测轮廓的面积大小设置闸值来去除过大或者过小的轮廓线。
# 除了面积过滤，还有根据待识别目标的特征指定更细致化的特征检测。例如车牌的矩形是有一定的长宽比的限制的，这样就可以设定一个"长宽比过滤"。同时，提前确定形状也意味着可以提前设置识别框画出的形状。
# 除了待识别目标的特征，还可以考虑非待识别目标的特征，类待识别目标的特征，进行有针对性的剔除过滤。
# 以上是人脑提取特征的基本步骤和方法。
```

### 腐蚀和膨胀

腐蚀和膨胀通常用于降低二值图像中的噪声。（噪声是闸值化的副作用，例如前景中残留的少许黑色斑点，背景中多突出的白色区域等）

为了减少前景对象的大小，可以在给定多次迭代的情况下腐蚀掉一些像素。

```
i11=i10.copy()
# 注意处理对象是闸值图像

i11=cv2.erode(i11,None,iterations=3)
# 腐蚀操作，会替换掉原图像，因此使用i11本身来进行腐蚀操作。"iteration=5"代表迭代次数为3次，可以进行适当调整。

cv2.imshow("ImageName17",i11)
cv2.waitKey(0)
```

相对于腐蚀，膨胀也可以做到近似的去除噪音的效果，只是要避免两个轮廓贴合在一起，腐蚀和膨胀两种方法都可以达成去除噪音目标：

```
i12=i10.copy()
# 注意处理对象是闸值图像

i12=cv2.dilate(i12,None,iterations=3)
# 膨胀操作，会替换掉原图像，因此使用i12本身来进行膨胀操作。"iteration=5"代表迭代次数为3次，可以进行适当调整。

cv2.imshow("ImageName18",i12)
cv2.waitKey(0)
```

### 蒙版和按位操作

蒙版可以遮掩住不感兴趣的图像区域，因此将它称为"mask"，它们会遮罩我们不关心的图像区域。

如果使用闸值图像作为蒙版与原始图像相比时，彩色区域重新出现，因为图像的其余部分被"遮罩"了。当然，可以使用其他多样的蒙版以达到自己想要的效果：

```
i13=i10.copy()
# 注意处理对象是闸值图像

output=cv2.bitwise_and(I1,I1,mask=i13)
# 第一个和第二个参数均为待处理图像，第三个参数是掩膜闸值图像（mask=xxx）。cv2.bitwise_and使用的是进行和运算。

cv2.imshow("ImageName19",output)
cv2.waitKey(0)
```

## 使用相机模块拍摄照片（驱动相机）

首先，树莓派上的摄像头应该是不支持热插拔的，所以安装和拆卸均需要先切断树莓派的电源。

安装好摄像头后，需要在树莓派的系统设置中启用摄像头。通过命令：```sudo raspi-config```，选择Interfacing Options下的的Camera，并使能（启用）即可。设置完成之后，将树莓派重启。

### 解决连接摄像头造成的VNC分辨率不符合导致无法远程桌面的问题

在配置摄像头后，突然出现VNC连接不上远程桌面的问题（cannot currently show the desktop），同时ssh却可以正常进行登录。网上最多的情况下使用树莓派ssh中的设置改动VNC分辨率（只有预设，设定得更大）后，问题仍然没有得到解决。最终解决方法：

原因：树莓派没接显示器时，执行的是默认分辨率，这个分辨率VNC不支持，导致不能正常显示。我们可以将HDMI接口改为强制热插拔，自动检测可以解决问题。

解决方法：修改配置文件：```sudo nano /boot/config.txt``` 找到```#hdmi_force_hotplug=1```这一行，把前面的#号去掉。可以理解为如果未检测到hdmi显示且正在输出合成，则取消注释。然后重启即可。

### 解决树莓派直接连接屏幕，屏幕坐上加载标志一直闪的问题

等待一段时间，树莓派会自动适配分辨率，加载系统也是需要时间的，全部完成后就自动进入系统（全程等待即可，不需要操作）。

虽然键盘没亮灯，但是是有效的。

移动树莓派的具体解决方案为：高容量的5V3A特制充电宝（树莓派4B要求）＋USB Hub（对高功率设备独立供电）

### 使用摄像头拍摄并查看拍摄的照片

接下来，正式测试我们的摄像头，拍摄一张照片。

使用CSI接口和USB接口的摄像头调用的库不一样，CSI接口的库更全面一点，但是USB的不需要驱动。注意使用CSI摄像头对应的库驱动USB摄像头会报错。

测试摄像头设备：

```
方法1：使用 lsusb 命令

利用 lsusb 命令可以查看树莓派上挂载的所有 usb 外设。将摄像头插入到树莓派的USB口中，然后输入此命令。会显示树莓派当前接入的USB设备列表，我们可以先不插摄像头执行 lsusb，然后插上摄像头执行 lsusb，就可以看到USB摄像头对应的是哪个设备了。

方法2：查看设备文件

输入 ls /dev/video* 如果存在 /dev/video0 ，说明识别到了设备
```

CSI接口的摄像头使用的库为：raspistill

```
raspistill -o ~/Desktop/image.jpg # 拍摄一张照片，命名为"image.jpg"。"~"代指用户主目录，"~/Desktop/image.jpg"代表照片文件的存储路径。
```

USB接口的摄像头使用的库为：fswebcam

```
sudo apt-get install fswebcam # 安装fswebcam库。

fswebcam -h # 获得fswebcam库的使用方法帮助文档。

fswebcam -S 10 image.jpg # 拍摄一张照片，image.jpg是文件名。-S代表延后n帧再进行采集，10代表10帧。路径默认保存在"pi"文件夹，也就是主目录。相机延后0帧为黑屏，延后帧数从0到10画面逐渐变亮，10帧为正常的照片采光。

fswebcam -d /dev/video0 --no-banner -r 320x240 -S 10 ./image.jpg 
# 使用"/dev/video0 --no-banner"拍出来照片没水印。"-r 320x240"指定照片尺寸。"./image.jpg"设置存储的路径和照片文件名。

gpicview image.jpg # 查看图片，image.jpg是文件名，不需要输入路径。

更多指令可见fswebcam -h。
```

## 使USB摄像头在树莓派系统中显示实时监控的画面并附带处理效果（驱动相机）

### USB摄像头的可用函数

cv2.VodeoCapture几乎可以获得摄像头的一切权限，以下是它的一些用法：

```
videoCapture(0) 参数0表示调用笔记本内置摄像头，对树莓派来说，就是调用连接的第一个摄像头

videoCapture("~\Desktop\py\001.avi") 参数路径，表示调用存储的视频
```

### 仅从摄像头捕获视频

```py
import cv2
import numpy

I = cv2.VideoCapture(0)
# 打开编号为0的摄像头设备，初期化USB摄像头

while(I.isOpened()):
# 当USB摄像头工作时
  
  ret,ImageCurrent = I.read()
  # 读取一帧图像，ImageCurrent是一个类似文件夹的东西，不断读取每一帧图像进去，到窗口显示时，再对其进行解包，释放出一帧一帧的图像，就像视频一样。
  # read要保证一直调用，需要返回一个true值使它不断继续调用，因此ret被赋值为一个布尔值，返回true则继续调用，返回false说明调用完毕或出现错误。
  
  if ret==True:
  
     # 此处可以嵌入对图像数组ImageCurrent进行处理的代码，然后再输出就能输出处理过的图像了。
  
     cv2.imshow('Video0',ImageCurrent)
     # 显示被处理后的图像窗口在树莓派的屏幕上，注意if内代码需要缩进。
  
  key = cv2.waitKey(1)
  
  #print( '%08X' % (key&0xFFFFFFFF) )
  
  if key & 0x00FF == ord('q'):
    break
	# 如果在画面窗口中按下q键后（有时需要加上Enter键）则退出循环

I.release()
cv2.destroyAllWindows()
# 释放资源和关闭窗口

```

### 保存摄像头捕获的视频（默认保存于当前目录）

```py
import numpy as np
import cv2

I=cv2.VideoCapture(0)

FC=cv2.cv2.VideoWriter_fourcc('X','V','I','D')
# 设置FourCC编码格式

V=cv2.VideoWriter('video001.avi',FC,20.0,(640,480))
# 设置视频保存的参数

while(I.isOpened()):
   ret,frame=I.read()
   if ret==True:
	     frame=cv2.flip(frame,0)
	     V.write(frame)
	     cv2.imshow('video',frame)
   else:
      break
   if cv2.waitKey(1)&0xFF==ord('q'):
      break
#释放
I.release()
V.release()
cv2.destroyAllWindows()
```

### 读取已经保存的视频文件

```
import numpy as np
import cv2

I=cv2.VideoCapture('video001.avi')

while(I.isOpened()):
   ret,frame=I.read()
   if ret==True:
	     frame=cv2.flip(frame,0)
	     cv2.imshow('video',frame)
   else:
      break
   if cv2.waitKey(25)&0xFF==ord('q'):
   # 在播放每一帧时，使用cv2.waiKey() 设置适当的持续时间。如果设置的太低视频就会播放的非常快，如果设置的太高就会播放的很慢（可以使用这种方法控制视频的播放速度）
      break
#释放
I.release()
cv2.destroyAllWindows()
```

### 捕获视频时采集照片（可用于采集训练样本）

```py
import cv2
import os

print("=============================================")
print("=  热键(请在摄像头的窗口使用)：             =")
print("=  z: 更改存储目录                          =")
print("=  x: 拍摄图片                              =")
print("=  q: 退出                                  =")
print("=============================================")
# 运行时显示帮助信息 

class_name = input("请输入存储目录：")
# 输入文件夹名

while os.path.exists(class_name):
    class_name = input("目录已存在！请输入存储目录：")
# 重输文件夹名

os.mkdir(class_name)
# 生成文件夹

index = 1
I = cv2.VideoCapture(0)
width = 640
height = 480
w = 360
# 设置参数

I.set(cv2.CAP_PROP_FRAME_WIDTH, width)
I.set(cv2.CAP_PROP_FRAME_HEIGHT, height)
# 设置高和宽

crop_w_start = (width-w)//2
crop_h_start = (height-w)//2
# 计算打算截取的宽和高的起点

print(width, height)
# 输出自定义的宽和高

while True:
    
    ret, frame = I.read()
    
    frame = frame[crop_h_start:crop_h_start+w, crop_w_start:crop_w_start+w]
	# 通过限定起点和终点，截取为正方形的画面
    frame = cv2.flip(frame,1,dst=None)
    cv2.imshow("capture", frame)

    input = cv2.waitKey(1) & 0xFF
	# 检测键盘输入

    if input == ord('z'):
	# 按"z"改变存储目录
        class_name = input("请输入存储目录：") # 实际上是文件夹名，存储文件夹默认在当前目录
        while os.path.exists(class_name):
            class_name = input("目录已存在！请输入存储目录：")
        os.mkdir(class_name)
    elif input == ord('x'):
	# 按"x"拍摄照片
        cv2.imwrite("%s/%d.jpeg" % (class_name, index),
                    cv2.resize(frame, (224, 224), interpolation=cv2.INTER_AREA))
        print("%s: %d 张图片" % (class_name, index))
        index += 1
    if input == ord('q'):
	# 按"q"终止循环
        break

I.release()
cv2.destroyAllWindows()
```

当前版本opencv-python(3.4.3.18)中摄像头有关属性为cv2.XXXX，其获取和设置函数分别如下：（以帧的宽和高为例）

```
# 获取
width = int(VideoCapture对象名.get(cv2.CV_CAP_PROP_FRAME_WIDTH))
height = int(videoCapture对象名.get(cv2.CV_CAP_PROP_FRAME_HEIGHT))

# 设置
cv2.VideoCapture(0).set(cv2.CAP_PROP_FRAME_WIDTH, width) # width设置为自定义宽度
cv2.VideoCapture(0).set(cv2.CAP_PROP_FRAME_HEIGHT, height) # height设置为自定义高度
```

帧宽和高默认为640x480（这是窗口的大小），画面比例为显示器分辨率，例如显示器分辨率为1920x1080，则摄像头画面以640x360的大小显示在窗口中央，并用黑边填充上下部分，摄像头画面长宽比似乎无法被改变。

read得到的帧（frame ）可以视为普通的图像来处理，本质上这个程序就是不断read一张图片并显示在窗口上，因此可以使用opencv有关图像处理的各种函数对frame进行操作并显示，我这里就是使用这个原理裁剪frame，使摄像头画面显示成正方形。

还有，前置摄像头获取的画面是非镜面的，即左手会出现在画面的右侧，此处使用flip进行水平镜像处理。

## 发光物体的识别+测距+追踪（实战）

筛选方法：

使用USB摄像头库（免驱动），发光物品的RGB模式下应该三个值总和较大。故意把曝光时间或者等待时间缩短，把画面整体拉暗，凸显出发光物体的亮度（灰阶值增大），设置合理的闸值以易于去除暗色背景识别。

发光物体的颜色不设限定，因此可以仅使用灰度值图来进行识别，样本也容易找到，检测的仅仅是灰度。

测距这一方面有点困难，考虑到比赛场地的发光符文亮度不会太高，因此可以适当获得除了台灯这种光源其他发光广告牌这种训练样本。

追踪在识别还可以的情况下可以直接套用网上现成的代码。而如何把追踪与辅助瞄准结合还需要日后的探索和打磨。

### 使用Haar分类器识别物体（自动提取特征，主要根据的是物体纹理）

步骤如下：

1.准备训练样本图片，包括正例及反例样本

2.生成样本描述文件

3.训练样本

4.目标识别

#### 准备训练样本的过程详解（以WIndows环境为例）

OpenCV中有专门的训练、生成样本、导入模型等功能的函数，但是部分功能需要可执行文件（未执行文件）。

训练分类器需要训练集。

所谓正样本，是指只包含待识别的物体的图片，一般是一些局部的图片，且最好能转化为灰度图。（目标的各个角度一般10~20张左右）尽可能只包含需要进行识别的物体。我们有两种图形标定工具：OpenCV的imageClipper、objectmarker。这两个工具都支持傻瓜式地对图片中的物体进行矩形标定，可以自动生成样本说明文件，自动逐帧读取文件夹内的下一帧。

在标定的时候尽量保持长宽比例一致，也就是尽量用接近正方形的矩形去标定待识别的物体，至于正方形的大小影响并不大。尽管OpenCV推荐训练样本的最佳尺寸是20x20，但是在生成样本描述vec文件的步骤时可以轻松地将其尺寸缩放到20x20。

***

所谓负样本，是指不包含待识别物体的任何图片，因此你可以将天空、海滩、大山等所有东西都拿来当负样本。但是，很多时候你这样做是事倍功半的。大多数模式识别问题都是用在视频监控领域，摄像机的角度跟高度都相对固定。如果你知道你的项目中摄像机一般都在拍什么，那负样本可以非常有针对性地选取，而且可以事半功倍。（大概1000~3000张左右）在没有目标物体的不同光照不同地点下取得负样本图像，然后在这些图上随机取图像块，每个块x * x（x的大小取决于待识别的物体的大致尺寸），每个块就是一个负样本。这几张图就能缠上数以千计数以万计的负样本！而且针对性强。

负样本图像的大小只要不小于正样本就可以。opencv在使用你提供的一张负样本图片时会自动从其中抠出一块与正样本同样大小的图像作为负样本。

***

接下来生成样本描述文件，样本描述文件也即.vec文件，里面存放二进制数据，是为OpenCV训练做准备的。只有正样本需要生成.vec文件，负样本不需要，负样本用.dat文件就可以。

新建两个文件夹分别存放正样本和负样本的图片，这是指没有标定的大图而不是被切割出的局部图。

在准备正样本步骤会自动生成样本的说明的文档文件txt，需要对其格式做一定的微调。

生成完vec文件后就可以投入训练器开始训练了。

***

注意事项：

- 样本图片最好使用灰度图，且最好能根据实际情况做一定的预处理

- 样本选择的原则是：数量越多越好，尽量高于1000；样本间差异性越大越好

- 正负样本比例为1：3或者3：7，尺寸为20x20比较适合

#### 准备训练样本并生成样本描述文件（Linux环境）

批量进行图片灰阶化的实现代码：

```
import cv2

for  i in range(1,500):#批量处理照片，填入需要处理的照片数(1,n)
    img = cv2.imread('/home/pi/Desktop/py/pos_1/'+str(i)+'.jpeg',cv2.IMREAD_GRAYSCALE) 
    # 读入照片，并转灰度。需要绝对路径（不能使用波浪号~）。
    # 注意读取的路径格式和循环指定图片名的格式。
    img1 = cv2.resize(img,(50,50))#调整大小,最终尺寸调整可以到生成描述文件再进行调整
    cv2.imwrite('/home/pi/Desktop/py/pos/('+str(i)+').jpeg',img1) 
		# 保存图片，需要绝对路径（不能使用波浪号~）

print('批量转灰度成功！')

for  i in range(1,1500):#批量处理照片
    img = cv2.imread('/home/pi/Desktop/py/neg_1/'+str(i)+'.jpeg',cv2.IMREAD_GRAYSCALE) 
    # 读入照片，并转灰度。需要绝对路径（不能使用波浪号~）。
    # 注意读取的路径格式和循环指定图片名的格式。
    img1 = cv2.resize(img,(224,224))#调整大小，负样本这一步就是最终调整
    cv2.imwrite('/home/pi/Desktop/py/neg/('+str(i)+').jpeg',img1) 
    # 保存图片，需要绝对路径（不能使用波浪号~）

print('批量转灰度成功！')
```

生成正样本/负样本txt描述文件（包含路径与样本文件名）实现代码：

```py
import os
def create_pos_n_neg():
  for file_type in ['pos']: 
  #此处修改为pos或者neg即可生成pos文件夹或者neg文件夹中的正负样本的描述文件。
    for img in os.listdir(file_type):
      if(file_type=='neg'):
        line=file_type + '/' + img + '\n'
        with open('bg.txt','a') as f:
          f.write(line)
      elif(file_type=='pos'):
        line=file_type+'/'+img+' 1 0 0 50 50\n'
        with open('info.txt','a') as f:
          f.write(line)

if __name__ == '__main__':
  create_pos_n_neg()
  print('样本描述txt文件已生成')
  
# 默认生成在py文件同一目录
```

#### 使用样本进行自定义训练（Linux环境）

注意：要通过opencv进行特征训练（包括LBP、Haar、HOG，最常使用Haar）必须通过两个应用：opencv_createsamples和opencv_traincascade。这两个应用是独立的，在opencv环境下可以运行。因此使用pip进行安装opencv是不带这两个应用的，需要下载源码进行编译得到可执行的文件。

但是，从opencv4.0版本开始，编译这两项的语句默认被注释，即使编译其中opencv_createsamples已经无法编译成功。因此可以考虑使用Windows系统下别人编译好的opencv_createsamples和opencv_traincascade的exe文件训练好xml文件再导入回Linux系统。

（Linux下步骤）生成pos.vec文件。在当前目录打开命令行终端，输入以下命令：

```
python3 # 打开python

import cv2 # 暂时导入cv2库

opencv_createsamples -info /home/pi/Desktop/py/info.txt -num 30 -w 50 -h 50 -vec /home/pi/Desktop/py/pos.vec
# 其中，-info字段填写正样本描述文件；-num制定最终正样本的数目；-w和-h分别指定正样本的宽和高（-w和-h越大，训练耗时越大）；-vec用于保存制作的正样本描述文件的路径。
# 此函数从4.0版本开始默认不安装，且不生效。

mkdir XML_file
# 创建XML_file文件夹，用于存储xml分类器数据

opencv_traincascade -data XML_file -vec pos.vec -bg bg.txt -numPos 5 -numNeg 10 -numStages 16 -w 50 -h 50
# 训练数据。
# -data XML_file：训练后同级目录的XML_file文件夹下会存储训练过程中生成的xml文件
# -vec positives.vec：Pos.vec是通过opencv_createsamples生成的vec文件
# -bg bg.txt：bg.txt是负样本文件的数据
# -numPos ：正样本的数目，这个数值一定要比准备正样本时的数目少，不然会报can not get new positive sample.太大会导致层数变少。
# -numNeg ：负样本数目，数值可以比负样本大
# -numStages ：训练分类器的级数
# -w 50：必须与opencv_createsample中使用的-w值一致
# -h 50：必须与opencv_createsample中使用的-h值一致
# 注：-w和-h的大小对训练时间的影响非常大
# 此函数从4.0开始被禁用。

# 替代方案：

# 注意：树莓派内存较小，numPos和numNeg的数值太大会被系统终止训练，因此适当取numPos和numNeg的数值。
# 进入XML_file文件夹下，就可以看到生成训练出来的xml文件cascade.xml。
```

训练好的模型具有的载体为XML文件，导入OpenCV的函数即可使用。

可以使用Windows下使用Windows环境编译的opencv_createsamples.exe和opencv_traincascade.exe，可以有效利用算力。（网上搜索资源，最好一起携带需要的库和依赖项，一般是编译后的opencv_bin文件夹的内容加上需要的依赖库即可）

#### 使用样本进行自定义训练（Windows环境）

（Windows）把Windows的Powershell当作Linux的命令终端使用，准备工作：

```
pip install opencv-python -i http://mirrors.aliyun.com/pypi/simple/   --trusted-host mirrors.aliyun.com
# 在Windows已经安装了全局python环境下直接下载并安装任意版本的opencv-python

python
import cv2
# 测试是否安装成功

exit()
# 退出python

pip install scikit-image
# 一次性把依赖库全部装上（opencv需要的）

python
import skimage.io as io
# 测试是否已经安装完成依赖库
```

在Windows某个目录新建一个文件夹，在里面解压别人编译好的open_cv文件夹，注意路径必须全英文。在打开open_cv文件夹的目录下打开Powershell，输入以下指令就可以进行训练了。

但是注意正样本文件夹pos，负样本文件夹neg，正样本描述文件info.txt，负样本描述文件bg.txt，这四个文件，既需要在下列进行处理的代码的所处绝对路径，也需要在open_cv文件夹内也就是与opencv_createsamples.exe和opencv_traincascade.exe同一目录内，同时存在两份才能进行训练：

```py
.\opencv_createsamples.exe -vec E:\English_Path_Need_application\OpenCV_traincascade_application\Sample_and_Materials\Light_Test\info.vec -info E:\English_Path_Need_application\OpenCV_traincascade_application\Sample_and_Materials\Light_Test\info.txt -bg E:\English_Path_Need_application\OpenCV_traincascade_application\Sample_and_Materials\Light_Test\bg.txt -num 500 -w 20 -h 20
# 注意需要的材料的路径必须正确，不然会报错。vec文件只要一份即可。
# 利用opencv_createsamples将少数正样本嵌入多数负样本图像就得到了最终样本，最终样本的说明文件正是.vec文件。
-num是生成的最终正样本数量（生成是指将灰阶小正样本嵌入灰阶大样本生成的最终样本）

-vec <vec_file_name>
	输出文件，内含用于训练的正样本。他应该有一个.vec文件扩展名。

-info <file_name>
	这是指定输入示例集合的文件的名字，包括文件名和在图像中示例目标的位置（例如自己创建的.dat
	文件）。

-img <image_file_name>
	这是-info的替代（必须提供其中一个）。使用-img，你可以提供单个裁剪的正向示例。在使用-img的
	模式中，将产生多个输出，且都来自于这一个输入。

-bg <background_file_name>
	背景图像的描述文件，文件中包含一系列的图像文件名，这些图像将被随机选作物体的背景。

-num <number_of_samples>
	生成的正样本的数目。

-bgcolor <background_color>
	背景颜色（目前为灰度图）；背景颜色表示透明颜色。因为图像压缩可造成颜色偏差，颜色的容差
	可以由 -bgthresh 指定。所有处于 bgcolor-bgthresh 和 bgcolor+bgthresh 之间的像素都被设置为
	透明像素。

-bgthresh <background_color_threshold>

-inv
	如果指定该标志，前景图像的颜色将翻转。

-randinv
	如果指定该标志，颜色将随机地翻转。

-maxidev <max_intensity_deviation>
	前景样本里像素的亮度梯度的最大值。

-maxxangle <max_x_rotation_angle>
	X轴最大旋转角度，必须以弧度为单位。

-maxyangle <max_y_rotation_angle>
	Y轴最大旋转角度，必须以弧度为单位。

-maxzangle <max_z_rotation_angle>
	Z轴最大旋转角度，必须以弧度为单位。

-show
	很有用的调试选项。如果指定该选项，每个样本都将被显示。如果按下 Esc 键，程序将继续创建样
	本但不再显示。

-w <sample_width>
	输出样本的宽度（以像素为单位）。

-h <sample_height>
	输出样本的高度（以像素为单位）。


.\opencv_traincascade.exe -data E:\English_Path_Need_application\OpenCV_traincascade_application\xml_file -vec E:\English_Path_Need_application\OpenCV_traincascade_application\Sample_and_Materials\Light_Test\info.vec -bg E:\English_Path_Need_application\OpenCV_traincascade_application\Sample_and_Materials\Light_Test\bg.txt -numPos 450 -numNeg 1300 -numStages 18 -w 20 -h 20
# 注意需要的材料的路径必须正确，不然会报错。注意可以自定义特征Haar，LBP，HOG。
# -numPos为每层使用的正样本数，应小于最终正样本数。-numNeg为每层使用的负样本数，可以大于准备的负样本数，-numStages是训练层数。

# 通用参数：
	-data <cascade_dir_name>
		目录名，如不存在训练程序会创建它，用于存放训练好的分类器。

	-vec <vec_file_name>
		包含正样本的vec文件名（由 opencv_createsamples 程序生成）。

	-bg <background_file_name>
		背景描述文件，也就是包含负样本文件名的那个描述文件。

	-numPos <number_of_positive_samples>
		每级分类器训练时所用的正样本数目，通常小于所提供的示例的数量。

	-numNeg <number_of_negative_samples>
		每级分类器训练时所用的负样本数目，可以大于 -bg 指定的图片数目。

	-numStages <number_of_stages>
		训练的分类器的级数。

	-precalcValBufSize <precalculated_vals_buffer_size_in_Mb>
		缓存大小，用于存储预先计算的特征值(feature values)，单位为MB。

	-precalcIdxBufSize <precalculated_idxs_buffer_size_in_Mb>
		缓存大小，用于存储预先计算的特征索引(feature indices)，单位为MB。内存越大，训练时间越短。

	-baseFormatSave
		这个参数仅在使用Haar特征时有效。如果指定这个参数，那么级联分类器将以老的格式存储。

级联参数：
	-stageType <BOOST(default)>
		级别（stage）参数。目前只支持将BOOST分类器作为级别的类型。

	-featureType<{HAAR(default), LBP}>
		特征的类型： HAAR - 类Haar特征； LBP - 局部纹理模式特征。

	-w <sampleWidth>
	-h <sampleHeight>
		训练样本的尺寸（单位为像素）。必须跟训练样本创建（使用 opencv_createsamples 程序创建）
		时的尺寸保持一致。

Boosted分类器参数：
	-bt <{DAB, RAB, LB, GAB(default)}>
		Boosted分类器的类型： DAB - Discrete AdaBoost, RAB - Real AdaBoost, LB - LogitBoost, 
		GAB - Gentle AdaBoost。

	-minHitRate <min_hit_rate>
		分类器的每一级希望得到的最小检测率。总的检测率大约为 min_hit_rate^number_of_stages。
		默认值为0.995对应于99.5%。

	-maxFalseAlarmRate <max_false_alarm_rate>
		分类器的每一级希望得到的最大误检率。总的误检率大约max_false_alarm_rate^number_of_stages.
		默认值0.50对应于50%。

	-weightTrimRate <weight_trim_rate>
		Specifies whether trimming should be used and its weight. 一个还不错的数值是0.95。

	-maxDepth <max_depth_of_weak_tree>
		弱分类器树最大的深度。注意，这不是级联器的深度。一个还不错的数值是1，是二叉树（stumps）。

	-maxWeakCount <max_weak_tree_count>
		每一级中的弱分类器的最大数目。类似-maxDepth，参数-maxWeakCount被直接传递给级联分类
		器的boosting，同时设置可以用于形成每一个强分类器（即，分类器的每一阶段）的弱级联器的最大
		数量。这个参数的默认值是100，但是这并不意味着弱分类器一定会使用这个数。

类Haar特征参数：
	-mode <BASIC (default) | CORE | ALL>
		参数-mode与类Haar特征一起使用，选择训练过程中使用的Haar特征的类型。 BASIC 只使用右上
		特征， ALL 使用所有右上特征和45度旋转特征。

LBP特征参数：
	LBP特征无参数。
```

#### 进行目标识别（处理输出视频的图像）

如果是直接调用OpenCV自带的训练好了的xml文件，则先确定需要进行引用的训练好的样本（XML文件）的路径：

```py
pip list 
# 查看列出各种已经安装的包的版本

pip show 包的名字
# 查看特定的包的位置
# 一般为：~/.virtualenvs/p3v31/lib/python3.9/site-packages/cv2/data下的XML文件。
#~波浪号代表/home/pi即默认家目录

cd ~/.virtualenvs/p3v31/lib/python3.9/site-packages/cv2/data
tree
# 列出本目录下的所有文件，有利于进行选择

# 下列是OpenCV自带的常用的模型：
# haarcascade_frontalface_default.xml 脸部识别模型
# haarcascade_eye.xml 眼部识别模型
# haarcascade_eye_tree_eyeglasses.xml 眼部（带眼镜）识别模型
```

例程（调用OpenCV内置训练好的模型XML文件）：

```py
import cv2
import numpy as np
import os

# 脸部识别处理函数
def detect_face(img):
    face_cascade = cv2.CascadeClassifier('/home/pi/.virtualenvs/p3v31/lib/python3.9/site-packages/cv2/data/haarcascade_frontalface_default.xml') 
	# 注意这里使用绝对路径（不能使用波浪号~），路径不对会报错，调用训练好的模型XML文件。
	# 注意CascadeClassifier仅支持Haar特征的xml文件，LBP特征的xml文件不可以。
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    faces = face_cascade.detectMultiScale(gray, 1.3, 5)
    for (x, y, w, h) in faces:
        img = cv2.rectangle(img, (x, y), (x + w, y + h), (0, 255, 0), 2)

# 眼部识别处理函数
def detect_eyes(img):
    eye_cascade = cv2.CascadeClassifier('/home/pi/.virtualenvs/p3v31/lib/python3.9/site-packages/cv2/data/haarcascade_eye.xml) 
	# 注意这里使用绝对路径（不能使用波浪号~），路径不对会报错，调用训练好的模型XML文件
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    eyes = eye_cascade.detectMultiScale(gray, 1.03, 5, 0, (40, 40))
    for (x, y, w, h) in eyes:
        img = cv2.rectangle(img, (x, y), (x + w, y + h), (0, 0, 255), 1)
		
# 调用函数的方法：detect_face(image) # image是从摄像头读取的帧图片，函数相当于对其进行画框处理。考虑到树莓派算力不足，可以采用：多核运算、跳帧处理（每几帧才识别一次）等方法。 
```

例程（调用自己训练好的模型XML文件，仅处理一张图片）：

```
import cv2

WC = cv2.CascadeClassifier('/home/pi/opencv_createsamples/data/cascade.xml') # 分类器路径

img = cv2.imread('Image1.jpg')#需要识别的照片，放到opencv_createsamples文件夹下

gray = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)

watches = WC.detectMultiScale(gray,1.3,5) # 分类器的用法：最核心的就是detectMultiScale函数，它有两个主要的参数scaleFactor和minNeighbors，scaleFactor是图像的缩放因子，而minNeighbors表示构成检测目标的相邻矩形的最小个数（越小误测概率越大）。

for (x,y,w,h) in watches:
    cv2.rectangle(img,(x,y),(x+w,y+h),(0,255,0),2)#建立方框，(0,255,0)表示绿色

    roi_gray = gray[y:y+h,x:x+w]
    roi_color = img[y:y+h,x:x+w]

cv2.imshow('识别窗口',img)
k = cv2.waitKey(0)
```

最终代码（调用自己训练好的XML模型，并处理视频流的图像）：

```
import cv2
import numpy
# 引用必要的OpenCV和Numpy库

def Detect_Light_Object(image):
  # 定义函数，调用训练好的Haar模型，并将识别出的目标物体使用矩形框标识出来。
  Cas=cv2.CascadeClassifier('/home/pi/Desktop/py/cascade_002.xml')
  # 调用训练好的分类器XML文件
  gray=cv2.cvtColor(image,cv2.COLOR_BGR2GRAY)
  # 灰阶化图像
  Objects=Cas.detectMultiScale(gray,1.1,15,cv2.CASCADE_SCALE_IMAGE,(50,50),(100,100))
  for(x,y,w,h)in Objects:
    image=cv2.rectangle(image,(x,y),(x+w,y+h),(0,255,0),2)
  # 将被识别出物体使用绿色矩形方框标识

Camera=cv2.VideoCapture(0)
# 声明相机对象

width=640
height=480
w=360
# 设置长宽

Camera.set(cv2.CAP_PROP_FRAME_WIDTH,width)
Camera.set(cv2.CAP_PROP_FRAME_HEIGHT,height)
Camera.set(cv2.CAP_PROP_EXPOSURE,100)
# 设置相机画面长宽以及曝光强度

w_start=(width-w)//2
h_start=(height-w)//2
# 设置截取的画面起点

print("宽度：{},高度：{}".format(width,height))
# 说明宽度和高度

i=0
# 计数器

while True:
  ret,frame=Camera.read()
  # 读取相机图像
  frame=frame[h_start:h_start+w,w_start:w_start+w]
  # 截取部分图像
  frame=cv2.flip(frame,1)
  # 水平翻转图像
  if i<1:
    i=i+1
  # 算力不足，为提高流畅度，采用每2帧进行一次识别
  else:
    Detect_Light_Object(frame)
    i=0
  # 重置计数器 
  
  cv2.imshow("capture",frame)
  # 显示处理好的图像

  input=cv2.waitKey(1)&0xFF
  if input==ord('q'):
    break
  # 按下"q"键推出。

Camera.release()
# 释放相机对象
cv2.destroyAllWindows()
# 关闭所有窗口
```

## Linux下其他方便有用的库/工具

### scrot截屏工具

安装：

```
sudo apt-get install scrot

sudo apt-get install shotwell # 安装可以通过终端打开截图的shotwell
```

常用命令：

```
scrot # 截取整个屏幕，路径默认为当前目录

scrot Image1.png # 命名截图文件名为Image1，扩展名不要丢，另外改变扩展名也不能改变文件格式，默认为png格式

scrot /home/pi/Desktop/Image.png # 指定特定的路径保存截图，并命名为Image1

scrot -s # 类似Windows自带的选定区域截屏

scrot -s /home/pi/Desktop/Image1.png # 选定区域截屏并指定特定路径并命名，推荐

scrot -d 10 # 倒计时10秒截屏

scrot -q 75 # 指定图像质量的百分率（默认为75）

shotwell "Image1.png" # 在图片目录下打开图片的命令

# 更多见：
scrot -h # 显示这个库的帮助文档
```

### python版本管理工具pyenv与虚拟环境工具virtualenv、virtualenvwrapper结合搭建任意版本python环境

面对多个 Python 开发项目时，需要针对不同的项目创建相应的开发环境。通常情况下，使用 virtualenv 创建一个虚拟的独立 Python 环境，但是 virtualenv 创建的环境相对分散不便于管理。这里推荐使用 virtualenvwrapper 来创建集中的便于管理的 Python 环境，同时可以结合 pyenv 为不同的项目选定不同的 Python 版本。

pyenv是利用系统环境变量PATH的优先级，劫持python的命令到pyenv上，根据用户所在的环境或目录，使用不同版本的python。

对于系统环境变量 PATH ，里面包含了一串由冒号分隔的路径，例如 /usr/local/bin:/usr/bin:/bin。每当在系统中执行一个命令时，例如 python 或 pip，操作系统就会在 PATH 的所有路径中从左至右依次寻找对应的命令。因为是依次寻找，因此排在左边的路径具有更高的优先级。在PATH 最前面插入一个 ( pyenvroot ) / shims 目 录 ， (pyenv root)/shims 目录，(pyenvroot)/shims目录。(pyenv root)/shims目录里包含名称为python以及pip等可执行脚本文件；当用户执行python或pip命令时，根据查找优先级，系统会优先执行shims目录中的同名脚本。pyenv 正是通过这些脚本，来灵活地切换至我们所需的Python版本。

虚拟环境，可以认为是一个与本来全局python版本相同或者相异的python版本，需要手动创建。手动创建的虚拟环境可以自定义名字，可以自定义它的基础是python的哪个版本的克隆，进入虚拟环境配置的包会安装到虚拟环境所在的目录里，也就是一个独立的可自定义python环境。

Python 虚拟环境是一个虚拟化，从电脑独立开辟出来的环境，它以某个版本的 python 为基础。python 虚拟环境相当于一个独立的 python 版本，有自己独立的目录，也可以独立的安装第三方库，而不会相互干扰。

pyenv的安装：

```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
# 安装pyenv

git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
# 安装pyenv-virtualenv

git clone https://github.com/pyenv/pyenv-virtualenvwrapper.git $(pyenv root)/plugins/pyenv-virtualenvwrapper
# 安装pyenv-virtualenvwrapper
 
# 在家目录使用git克隆项目安装，此链接2022.3.4仍有效，无效就去找新的git文件进行克隆安装。
```

pyenv 是管理 python 版本的工具，virtualenvwrapper 是创建虚拟 python 环境的工具，pyenv-virtualenvwrapper 是可以创建不同 python 版本虚拟环境的工具。

```
sudo nano ~/.profile 
# 打开Linux此版本系统的shell配置文件，在末尾追加以下代码：

export PYENV_ROOT="$HOME/.pyenv"
# $HOME是家目录，等效于"~"。这里填入pyenv安装的根目录。
export PATH="$PYENV_ROOT/bin:$PATH" 
# 注意这里必须等于.pyenv的可执行文件/管理环境文件的路径（树莓派为"bin"文件夹，当创建的虚拟环境python版本没有变化时改成"shims"文件夹并更新和创建环境，然后再改回"bin"文件夹），不然python版本无法切换，也就是环境变量不对。$PYENV_ROOT代表pyenv的根目录路径。
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

eval "$(pyenv init -path)
# 这是pyenv2的需要添加的代码，未试验过。

source ~/.profile
# 确认是否安装成功。
# 和virtualenv类似的，使用前都需要运行一次：source ~/.profile 才能使用。

pyenv 
# 查看是否安装成功，显示pyenv的信息

pyenv --version 
# 查看pyenv当前版本
```

pyenv的简单使用（注意pyenv只能管理系统自带和pyenv安装的python版本）

```
pyenv version
# 查看当前python版本

pyenv versions
# 执行命令将打印出安装了的所有 Python 版本，*表示当前使用的 Python 版本（虚拟环境），system表示系统默认版本。
# 这个命令会列出所有存在的虚拟环境，每个虚拟环境会出现两次，分别对应相应虚拟环境目录的真身和链接。

pyenv install --list
# 查看所有可安装的软件版本（不止是python）

pyenv install 3.6.3
# 安装指定版本（建议先用镜像源把安装包下载下来再安装）

pyenv uninstall 3.6.3
# 卸载指定版本或者虚拟环境

pyenv rehash
# 安装新版本后rehash一下，创建垫片路径（为所有已安装的可执行文件创建 shims，如：~/.pyenv/versions/3.6.3(一般为python版本号)/bin/，因此，每当你增删了 Python 版本或带有可执行文件的包（如 pip）以后，都应该执行一次本命令）。

pyenv global 3.6.3
# 指定全局版本（设置全局的 Python 版本，通过将版本号写入 ~/.pyenv/version 文件的方式。）更改本机版本，重启不会造成再次更改。系统的"版本号"默认为"system"。
# 因为系统本身常常会依赖自带的 python 版本，所以尽量不要修改 global。

pyenv global 3.6.3 2.7.14
# 指定多个全局版本, 3版本优先

pyenv shell 3.6.3  
# 设置面向 shell 的 Python 版本，通过设置当前 shell 的 PYENV_VERSION 环境变量的方式。#  更改本shell的版本，临时生效。

pyenv local 3.6.3  
# 设置 Python 本地版本，通过将版本号写入当前目录下的 .python-version 文件的方式。更改本地的版本，只是临时生效，重启什么的会恢复系统版本。通过这种方式设置的 Python 版本优先级较 global 高。
# 当进入目前这个目录时，会自动激活相应的虚拟环境，退出这个目录时，会自动关闭相应的虚拟环境（在 pyenv 中，虚拟环境和正式的 python 版本具有同样的地位，通过 pyenv versions 查看 python 版本时，虚拟环境也是作为一个独立的 python 版本出现的）。

pyenv shell --unset 
# 取消 shell python 版本 

pyenv local --unset 
# 取消 local python 版本

# 注意优先级为：shell > local > global。pyenv 会从当前目录开始向上逐级查找 .python-version 文件，直到根目录为止。若找不到，就用 global 版本。

# 实际上当你切换版本后, 相应的pip和包仓库都是会自动切换过去的
```

（推荐）如果压缩包下载得太慢/无法下载，可以使用镜像加速下载，再进行本地安装python（需要仅wifi环境下进行下载，dns才不报错）：

```
v=3.6.3;wget https://npm.taobao.org/mirrors/python/$v/Python-$v.tar.xz -P $(pyenv root)/cache/;
# 注意：v 为 Python 版本号，请根据需要自行更改。这一步仅为下载安装包。

pyenv install 3.6.3 
# 安装指定的下载好的版本安装包

pyenv rehash
# 更新本地数据库
```

pyenv-virtualenv的简单使用：

```
pyenv virtualenv [python版本] [自定义环境名称] 
# 创建基于[python版本]的虚拟环境，可以以某版本的 python 为基础创建名为 virtualenv-name 的虚拟环境，如果不指定 python 的版本，那么就会以当前的 python 版本为基础创建虚拟环境。
# 虚拟环境创建时，会在 $(pyenv root)/versions 目录下创建一个对应虚拟环境名的目录，这个目录只是一个链接，真身在对应的 python 版本目录下的 envs 目录下。

pyenv activate [环境名称] 
#激活虚拟环境

pyenv deactivate
# 或者source deactivate
# 退出虚拟环境

# 当然也可以：
pyenv local/global [环境名称]
# 设置全局/当前目录为某个虚拟环境
```

通过pyenv-virtualenv创建一个版本的虚拟环境并命名：

```
pyenv install 3.6.3 
# 安装指定版本python，用于充当构建虚拟环境的基础

pyenv virtualenv 3.6.3 p363v31
# 创建一个名为p363v31的python3.6.3版本的虚拟小版本，可以在版本列表中存在，就和3.6.3是一样的，就是一个版本了。 真实目录在~/.pyenv/versions/下(profile文件里的PATH的设置)，以后只要使用这个虚拟版本，包就会按照到这些对应的目录下去，而不是使用3.6.3。

cd ~/Desktop/py
# 转移到需要特定虚拟环境的路径

pyenv local p363v31
# 把本路径设定为恒适用虚拟环境，下次启动也自动进入

pyenv activate p363v1
# 激活虚拟环境

python --version
# 会显示当前的环境的python版本

pyenv deactivate
# 退出虚拟环境
```

```
pyenv activate p363v1
# 激活虚拟环境

python -m pip install --upgrade pip
# 根据提示更新pip（安装opencv报错才执行）

pip install --upgrade setuptools
# 更新setuptools（安装opencv报错才执行）

pip install opencv-contrib-python==3.4.11.45 -timeout 480
# 安装opencv（可以先用3.4.1来试探出报错列表的存在版本）这里将timeout设置为480秒，也就是八分钟，绝大多数电脑八分钟进度条都会动一下，一旦进度变化，八分钟就会重新计算。

pip install --upgrade opencv-contrib-python==3.4.11.45
# 如果多次下载到最后出错哈希值不对，则使用这种方法进行安装。（安装opencv报错才执行）

pyenv deactivate p363v1
# 关闭虚拟环境
```

pip安装opencv时出现错误，很多问题都是因为setuptools版本太低的问题导致的，遇到类似的情况，通常第一反应去更新setuptools，这是使用pip的一个良好的习惯。

```
cd /usr/bin && ls |grep python
# 查看系统python的启动文件和配置文件
```
