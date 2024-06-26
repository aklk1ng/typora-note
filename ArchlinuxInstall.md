# Archlinux-KDE-Install

<!-- vim-markdown-toc GFM -->

- [1. 连接网络(wifi)](#1-连接网络wifi)
- [2.更新系统时钟](#2更新系统时钟)
- [3.分区](#3分区)
- [4.格式化](#4格式化)
- [5.挂载](#5挂载)
- [6.镜像源的选择](#6镜像源的选择)
- [7.安装系统](#7安装系统)
- [8.生成 fstab 文件](#8生成-fstab-文件)
- [9.切换到安装好的系统](#9切换到安装好的系统)
- [10.时区设置](#10时区设置)
- [11.设置 Locale 进行本地化](#11设置-locale-进行本地化)
- [12.设置主机名](#12设置主机名)
- [13.设置 root 用户密码](#13设置-root-用户密码)
- [14.安装微码](#14安装微码)
- [15.安装引导程序](#15安装引导程序)
- [16.完成无界面安装](#16完成无界面安装)
- [17.再次配置](#17再次配置)
- [18.开启 32 位支持库](#18开启-32-位支持库)
- [19.添加普通用户](#19添加普通用户)
- [20.添加 archlinuxcn 源(非必要,只是多一些国人更常用的软件)](#20添加-archlinuxcn-源非必要只是多一些国人更常用的软件)
- [21.安装显卡驱动](#21安装显卡驱动)
- [22.安装桌面环境](#22安装桌面环境)
- [23.设置系统中文](#23设置系统中文)
- [24.安装 yay](#24安装-yay)
- [25.重启（欢迎来到 archlinux :joy:）](#25重启欢迎来到-archlinux-joy)
- [26.安装输入法(有一些可能是找不到的)](#26安装输入法有一些可能是找不到的)
- [27.启动蓝牙](#27启动蓝牙)

<!-- vim-markdown-toc -->

### 1. 连接网络(wifi)

```bash
iwctl                                           #进入交互命令行
device list                                     #列出设备名，例如网卡wlan0
staction                                        #扫描网络
station wlan0 get-networks                        #列出可连接的网络
station wlan0 connect THE-WIRELESS-NAME            #进行连接，之后输入密码即可
exit                                            #退出
```

可以使用**ping**命令检验

### 2.更新系统时钟

```bash
timedatectl set-ntp 1/true        #将系统时间与网络时间进行同步
timedatectl status                #检查服务状态
```

### 3.分区

**对于分区而言，有许多中的方案，这里之介绍一种**

- EFI 分区：几百 M 即可，可以直接挂载 windows 的 EFI 分区，也可以在分一块新的内存
- 根目录：直接将剩余的磁盘分给它，在 Manjaro 的安装中，我试过将它分为/，/home，/opt,但是效果不是很好，有意思的是，采取这种方案无法查看/home 目录的内存大小（可以都尝试一下

```bash
fdisk -l            #对硬盘进行扫描，注意好需要分的空间
cfdisk /device        #这里才用可视化的工具进行分区，也可以用fdisk,但对于较麻烦的可视化跟友好
```

### 4.格式化

与上对应

```bash
mkfs.vfat            #格式化为引导目录
mkfs.ext4            #linux系统普通目录格式
```

### 5.挂载

在挂载时，先挂载根目录，在挂载 EFI 分区，这里的 sdax 只是例子

```bash
mount /dev/sdax /mnt
mkdir /mnt/boot                #创建boot目录
mount /dev/sdax /mnt/boot
```

### 6.镜像源的选择

```bash
vim /etc/pacman.d/mirrorlist
```

新版的镜像会提供不同国家的镜像源，直接将合适的源粘贴到首行即可，这里提供几个源

```bash
Server = https://mirrors.163.com/archlinux/$repo/os/$arch                #163源
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch            #中科大
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch    #清华
Server = https://mirror.0xem.ma/arch/$repo/os/$arch    #北美洲地区:加拿大
Server = https://mirror.aktkn.sg/archlinux/$repo/os/$arch    #东南亚地区:新加坡
Server = https://archlinux.uk.mirror.allworldit.com/archlinux/$repo/os/$arch    #欧洲地区:英国
Server = https://mirrors.cat.net/archlinux/$repo/os/$arch    #东亚地区:日本
```

### 7.安装系统

[my neovim config](https://github.com/aklk1ng/nvim.git)

```bash
pacstrap /mnt base base-devel linux linux-headers linux-firmware dhcpcd vim neovim dialog networkmanager netctl #base-devel在AUR包的安装是必须的
```

### 8.生成 fstab 文件

```bash
genfstab -L /mnt >> /mnt/etc/fstab
```

**cat /mnt/etc/fstab**检查文件内容

### 9.切换到安装好的系统

```bash
arch-chroot /mnt
```

### 10.时区设置

```bash
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc        #对硬件进行时间设置，将正确的UTC时间写入硬件时间
```

### 11.设置 Locale 进行本地化

```bash
--使用vim进行编辑，后续才会进行下载(sudo pacman -S vim)
vim /etc/locale.gen                                #去掉 en_US.UTF-8 所在行以及 zh_CN.UTF-8 所在行的注释符号（#）
locale-gen                                        #生成 locale
echo 'LANG=en_US.UTF-8'  > /etc/locale.conf        #向 /etc/locale.conf 导入内容
```

### 12.设置主机名

```bash
vim /etc/hostname        #直接写入保存即可
vim /etc/hosts
#加入以下内容：
127.0.0.1   localhost
::1         localhost
127.0.1.1   YOUR-HOSTNAME.localdomain YOUR-HOSTNAME
```

### 13.设置 root 用户密码

```bash
passwd root
```

### 14.安装微码

```bash
pacman -S intel-ucode   #Intel
pacman -S amd-ucode     #AMD
```

### 15.安装引导程序

```bash
pacman -S os-prober ntfs-3g grub efibootmgr   #grub是启动引导器，efibootmgr被 grub 脚本用来将启动项写入 NVRAM。
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
grub-mkconfig -o /boot/grub/grub.cfg            #生成 GRUB 所需的配置文件
```

**os-prober 需要手动安装，在/etc/dafault/grub 中，取消 GRUB_DISABLE_OS_PROBER=false 的注释，这样在开机时进入 bios 中将 grub 的启动项设置为最高优先级，即可进入 grub 选择操作系统界面,并可以选择 windows 系统**

### 16.完成无界面安装

```bash
exit                # 退回安装环境#
umount -R  /mnt     # 卸载新分区
reboot              # 重启
```

**注意，重启前要先拔掉优盘，否则你重启后还是进安装程序而不是安装好的系统**

### 17.再次配置

```bash
systemctl enable NetworkManager                                        #允许网络服务
systemctl start NetworkManager                                        #开启
nmcli dev wifi list                                                    #进行网络扫描
nmcli dev wifi connect "THE-WIRELESS-NAME" password "THE-PASSWORD"    #进行连接
dd if=/dev/zero of=/swapfile bs=1M count=512 status=progress        #设置交换分区（非必要）
chmod 600 /swapfile            #设置权限
mkswap /swapfile            #格式化
swapon /swapfile            #启用swapfile
vim /etc/fstab
#添加以下内容：/swapfile none swap defaults 0 0
```

### 18.开启 32 位支持库

```bash
vim /etc/pacman.conf            #去掉[multilib]一节中两行的注释，来开启 32 位库支持
pacman -Syyu                    #最后:wq 保存退出，刷新 pacman 数据库
```

### 19.添加普通用户

```bash
useradd -m -G wheel YOUR-NAME        #wheel为所属用户组
passwd YOUR-NAME
pacman -S sudo
sudo vim /etc/sudoers                #若还是无法修改需更改文件权限
取消wheel行的注释
```

### 20.添加 archlinuxcn 源(非必要,只是多一些国人更常用的软件)

```bash
su YOUR-NAME
sudo vim /etc/pacman.conf
#可以在multilib下方添加：
[archlinuxcn]
Server = https://repo.archlinuxcn.org/$arch
sudo pacman -Syy
sudo pacman -S archlinuxcn-keyring                #添加软件签名，如果不介意签名，可以再添加一行 SigLevel = Never
sudo pacman -Syyu
```

### 21.安装显卡驱动

```bash
sudo pacman -S xf86-video-intel mesa        #intel用户,其他显卡可以在archlinux官网查询
```

### 22.安装桌面环境

**非必要，你可以使用 window managers such as i3wm--what i used, dwm--what i use now, bspwm--what i haven't used and so on**

```bash
--- kde
sudo pacman -S xorg plasma kde-applications sddm network-manager-applet    #桌面基础包
sudo systemctl enable sddm                #允许登陆欢迎服务
sudo systemctl enable NetworkManager    #允许网络服务
```

**基础功能包(可选则性安装)**

```bash
sudo pacman -S sof-firmware alsa-firmware alsa-ucm-conf                     #一些可能需要的声音固件
sudo pacman -S ntfs-3g                                                      #识别NTFS格式的硬盘
sudo pacman -S adobe-source-han-serif-cn-fonts wqy-zenhei                   #安装几个开源中文字体 一般装上文泉驿就能解决大多wine应用中文方块的问题
sudo pacman -S noto-fonts-cjk noto-fonts-emoji noto-fonts-extra             #安装谷歌开源字体及表情
sudo pacman -S firefox chromium                                             #安装常用的火狐、谷歌浏览器
sudo pacman -S ark                                                          #与dolphin同用右键解压
sudo pacman -S p7zip unrar unarchiver lzop lrzip                            #安装ark可选依赖
sudo pacman -S packagekit-qt5 packagekit appstream-qt appstream             #确保Discover(软件中心）可用 需重启
sudo pacman -S gwenview                                                     #图片查看器
sudo pacman -S git wget kate bind                                                #一些工具
```

**如果存在找不到目标包，先舍弃即可，不要安装过多字体：在字体超过 255 种时，某些 QT 程序可能无法正确显示某些表情和符号**

### 23.设置系统中文

```bash
sudo vim /etc/locale.conf
#添加如下内容：
LANG=zh_CN.UTF-8
```

### 24.安装 yay

```bash
#对go进行换源
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct

git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

### 25.重启（欢迎来到 archlinux :joy:）

```bash
sudo reboot
```

### 26.安装输入法(有一些可能是找不到的)

```bash
sudo pacman -S fcitx5-im #基础包组
sudo pacman -S fcitx5-chinese-addons #官方中文输入引擎
sudo pacman -S fcitx5-anthy #日文输入引擎
yay -S fcitx5-pinyin-moegirl #萌娘百科词库
sudo pacman -S fcitx5-pinyin-zhwiki #中文维基百科词库
sudo pacman -S fcitx5-material-color #主题

sudo vim /etc/environment
#添加如下内容：
GTK_IM_MODULE=fcitx5
QT_IM_MODULE=fcitx5
XMODIFIERS=@im=fcitx5
SDL_IM_MODULE=fcitx5
```

**可能要重启后才能开始使用 fcitx5 输入法,如果你没有使用桌面环境，可以使用 fcitx5-configtool 工具来进行配置**

- 打开 _系统设置_ > _区域设置_ > _输入法_，先点击`运行Fcitx`即可，拼音为默认添加项
- 接下来点击 _拼音_ 右侧的配置按钮，点选`云拼音`和`在程序中显示预编辑文本` 最后应用
- 回到输入法设置，点击`配置附加组件`，找到 _经典用户界面_ 在主题里选择一个你喜欢的颜色 最后应用
- 注销，重新登陆，就可以发现已经可以在各个软件中输入中文了

### 27.启动蓝牙

```bash
sudo pacman -S bluez bluez-utils
sudo systemctl enable --now bluetooth
#如果要连接蓝牙音频设备，需要安装 pulseaudio-bluetooth 并重启 pulseaudio
sudo pacman -S pulseaudio-bluetooth
pulseaudio -k
```
