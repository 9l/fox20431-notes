# ArchLinux Installation

## 前期准备

### step 1

准备一个U盘

### step 2

下载镜像

下载地址：  
[清华镜像站][1]  
[科大镜像站][2]

[1]:https://mirrors.tuna.tsinghua.edu.cn/
[2]:http://mirrors.ustc.edu.cn/

### step 3

下载烧录工具

- Windows环境推荐使用**USBWriter**
- Linux环境推荐使用**dd**命令
- Mac未知

### step 4

使用烧录工具将镜像烧入到U盘


### step 5

开机选择从U盘进入系统  

*具体操作搜索引擎查询关键字“开机启动项”*

## 系统安装

### 硬盘处理

Forwords:  
由于硬件类型和分区方式的多样性，非通用命令不在本文记录  

*常用分区命令：parted、fdisk、cfdisk*

#### 1.选择硬盘分区表类型

两种类型：  
1.mbr  
2.gpt

gpt分区表优于mbr分区表

如果主板支持uefi，两者都可以选择  
如果主板不支持uefi，只能选择mbr



#### 2.选择硬盘分区方式

两种方式：  
1.普通分区  
2.逻辑卷分区

普通分区确定大小后不能拉伸缩小容量，逻辑卷可以  
普通分区操作简单，逻辑卷相反

#### 3.规划硬盘分区方案

|分区类型|分区大小|是否必要|
|-|-|-|
|efi分区[^1]|推荐300M|必要|
|swap分区[^2]|推荐内存的1-2倍|非必要|
|家目录分区[^3]|不定|非必要|
|根分区[^4]|不定|必要|

[^1]:efi分区：用于安装引导程序来引导电脑进入系统。
[^2]:swap分区：与windows虚拟内存原理类似，当内存的使用超出实际内存大小的时候swap分区用来充当内存。此外，在休眠状态下内存里的内容转移到swap分区中，继而内存停止工作，从而节省电量；当解除休眠时，内存就会从swap分区中读取数据，从而使电脑正常运行。
[^3]:家目录分区：非root用户在平时活动根据地，用来保存用户的各种信息，比如各个应用的配置文件，下载内容，创建内容等等。
[^4]:根目录分区：储存除挂载(主要指efi分区和swap分区)和swap分区外所有数据。

Tips:  
如果你是双硬盘可以选择一整张硬盘作为家目录分区，避免由于物理或者软件原因导致自己家目录受到牵连。目前市面上笔记本多为一固态一机械，由于机械的读写慢寿命长，所以推荐机械硬盘作为家目录分区，这样就可以在不影响系统速度的情况下将个人数据保存的更长久，同理固态的读写快寿命短，推荐用来安装系统。

#### 4.格式化分区

efi分区文件系统：fat32  
swap分区文件系统：swap  
家目录/根目录分区文件系统：ext4

```
mkfs.vfat /dev/efi分区
mkswap /dev/swap分区
# 开启swap分区
swapon /dev/swap分区
mkfs.ext4 /dev/根目录分区
mkfs.ext4 /dev/家目录分区
```


### 挂载处理

挂载是对分区内容进行操作

```
mount /dev/根目录分区 /mnt
mkdir -p /mnt/boot/efi
mount /dev/efi分区 /mnt/boot/efi
# 如果有家目录再执行下列命令
mkdir /mnt/home
mount /dev/家目录分区 /mnt/home
```

### 网络处理

#### 1.连接网络

- 无线网连接命令：`iwd`
- 有线网连接命令：`dhcpcd`

测试网络:`ping baidu.com`

#### 2.修改镜像

包管理获取软件的镜像站地址全部储存在`/etc/pacman.d/mirrorlist`  
包管理会按照文件内镜像站的顺序作为优先级获取软件  
当最前的镜像站连接超时后自动跳转第二个，以此递推

为了确保网络畅通，通过编辑方式将中国的镜像站放到最前面

*推荐的镜像站：163、stinghua*

```
# 编辑文件
vim /etc/pacman.d/mirrorlist
# 具体操作略
```


### 安装系统

通过pacstrap给挂载的分区安装系统

```
# 安装系统
patrap /mnt base base-devel linux linux-headers linux-docs linux-firmware
# 如果CPU是intel，执行下载操作
pacstrap /mnt intel-ucode
# 如果CPU是AMD，执行下载操作
pacstrap /mnt amd-ucode
```

base:包含最基础工具  
base-devel:base的拓展包  
linux:linux内核  
linux-headers:linux内核的头文件  
linux-docs:linux的说明文档  
linux-firmware:对一些硬件的支持

### 创建fstab

fstab作用：开机的时候告诉系统挂载的分区、挂载点、挂载格式、挂载选项等

```
# 在已安装内的系统内创建fstab
genfstab /mnt >> /mnt/etc/fstab
# 根据内容检查fstab正误
cat /mnt/etc/fstab
```

### 进入系统并执行必要操作

```
# 进入新装的系统
arch-chroot /mnt
# 设置root密码
passwd
# 安装编辑器
pacman -S vim
```

如果你采用lvm分区需要执行额外操作

- 安装lvm2
- 编辑`mkinitpio.conf`，在`HOOKS`所在行加入`lvm2`
- 使修改配置文件生效

```
# 安装lvm2
pacman -S lvm2
# 修改mkinipio.conf
# HOOKS所在行添加lvm2
vim /etc/mkinitcpio.conf
# 是配置文件生效
mkinitpio -p linux
```

### 语言设置

#### 1.生成locale

locale作用：  
显示本地化的文字、货币、时间、日期、特殊字符等包含地域属性的内容

```
# 修改语言的配置文件
# 取消`en_US.UTF-8`的注释
vim /etc/locale.gen
# 生成locale
locale-gen
```

#### 2.设置locale.conf

locale.conf作用:设置系统语言

locale.conf会在系统启动的早期被`systemd`读取

```
# 编辑locale.conf
vim /etc/locale.conf:
# 设置英文需添加：LANG=en_US.UTF-8
# 设置中文需添加：LANG=zh_CN.UTF-8
# 其他语言以类似
```

#### 3.安装中文字体

缺失中文字体会出现方块字情况，需要安装对应语言字体

中文推荐`wqy-zenhei`或`wqy-microhei`字体

```
# 安装中文字体
pacman -S wqy-zenhei wqy-microhei
```

### 时间设置

Arch Linux以BIOS的时间为世界统一时间（UTC）

中国在东八区，即UTC时间加8个小时  
将对应配置文件链接到对应位置即可将时间设置成东八区

*在我这里发生了小问题，安装Arch Linux会修改BIOS时间为东八区时间，需要进BIOS内再修改*

```
# 设置时区为东八区
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

### 网络设置

```
# 安装网络组件
pacman -S wpa_supplicant networkmanager dhcpcd
# 启动无线网络
systemctl enable NetworkManager
# 启动有线网络
systemctl enable dhcpcd

# 最小化安装
pacman -S iwd dhcpcd
# 启动无线网络
systemctl enable iwd
# 启动有线网络
systemctl enable dhcpcd
```

NetworkManager用法：`nmtui-connect`  
dhcpcd用法：`dhcpcd`

### 安装引导

```
# 安装引导所需包
pacman -S grub efibootmgr
# 安装grub
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch
# 使配置文件生效
grub-mkconfig -o /boot/grub/grub.cfg
```

Tips:  
`/etc/default/grub`为grub配置文件  
修改配置文件后，执行

    grub-mkconfig -o /boot/grub/grub.cfg

可以使配置文件的修改生效

*恭喜你，到这一步Arch Linux你已成功安装*  
*为了更好的操作体验，下面将安装图形界面*  
*如果暂不需要图形界面可以直接进入这个步骤:[结束](#结束)*

### 安装图形界面

xorg为各种主流桌面所依赖

在xorg基础上可以安装各种主流桌面

```
# 安装xorg
pacman -S xorg
```

#### 方案一：Gnome桌面

```
pacman -S gnome
systemctl enable gdm
```



#### 方案二：KDE桌面

```
pacman -S plasma
pacman -S kdebase
pacman -S sddm
systemctl enable sddm
```

kdebase包含终端、文件管理器、编辑器等

### 音量处理

一般情况下桌面是无法直接获取和修改音量信息的

需要安装对应的应用

```
pacman -S alsa-utils pulseaudio pulseaudio-alsa
```

### 蓝牙处理

```
# 安装蓝牙组件
pacman -S bluez pulseaudio-bluetooth
# 启用蓝牙服务
systemctl enable bluetooth
```

### 新建用户

root作为超级用户，在平时使用会存在风险  
需要新建用户，然后赋予root的权限

```
# 创建test用户生成家目录并加入wheel组
useradd -m -G wheel test
# 设置test的密码
passwd test
# 允许wheel组获取权限
vim /etc/sudoers
# 取消wheel的其中一个注释
```


### <a id="结束">退出系统并结束所有流程
```
# 退出系统
exit
# 解除挂载
umount -R /mnt
# 重启电脑
reboot
```

*恭喜你完成所有操作，记得拔出U盘*

## 事后准备

类似输入法等软件的安装和配置这里不做说明，wiki讲的很全面

### 增加软件库

```
sudo vim /etc/pacman.conf
```
文件尾添加下面两行
```
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

[multilib]为32位软件库，取消注释

```
sudo pacman -Syu
sudo pacman -S archlinuxcn-keyring
```