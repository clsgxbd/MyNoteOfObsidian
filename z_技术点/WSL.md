#wsl   Windows Subsystem for Linux 
## 安装与启动
### 安装默认可以安装的子系统发行版
打开命令提示符
wsl --install   默认安装Ubuntu系统,国内网络下载较慢可以在后面加上 --web-download
wsl --list --online 查看可以安装的系统
wsl --install [NAME] 这样可以安装指定的系统
wsl -l -v 可以查看已经安装的系统  NAME前面带星号的代表默认的wsl
wsl  进入默认的子系统
wsl -d [NAME] 进入指定NAME的子系统
wsl --set-default [NAME] 设置默认的子系统

默认安装路径：
### 安装其他版本的子系统
安装其他版本的子系统 需要先进入一个子系统安装docker 或者直接安装windows版本的docker

docker run -it --rm centos:7 bash  自动拉取centos:7镜像并进入自动创建的容器， 已经进入容器后显示

```
[root@b90fd8f5bc65 /]#
```
我们需要复制这个id ：  b90fd8f5bc65

然后再打开一个可以使用docker的窗口
docker export b90fd8f5bc65 -o centos.tar 
mv centos.tar /mnt/c/Users/damu/Desktop  把这个tar包移动到桌面

然后在打开一个cmd终端窗口
wsl --import centos <u>E:\WSL\centos</u> <u>Desktop\centos.tar</u>  安装centos到指定的位置

wsl -l -v  查看已经安装的子系统  就可以看到centos了
wsl -d centos  进入centos子系统

### 默认安装位置：
WSL 2: WSL 2 使用一个虚拟硬盘（VHD），存储在 Windows 用户目录下，路径类似于：
```
C:\Users\<YourUsername>\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_<version>\LocalState\ext4.vhdx
```

WSL 1: WSL 1 的文件存储在 Windows 文件系统的目录下，路径类似于：
```
C:\Users\<YourUsername>\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_<version>\LocalState\rootfs
```

### 修改子系统安装位置
查看wsl版本
```
wsl -l -v
```
列出所有已经安装的wsl发行版及其版本信息，确定是wsl2（VERSION显示2，说明是wsl2）

1. 创建目标目录
```
mkdir E:\WSL\BACKUP
```
2. 确认路径权限
```
echo "test" > E:\WSL\BACKUP\test.txt
```
3. 导出当前的wsl发行版
```
wsl --export Ubuntu E:\WSL\BACKUP\Ubuntu.tar
```
4. 注销（即卸载）当前发行版
```
wsl --unregister Ubuntu
```
5. 导入WSL发行版到新的位置
```
wsl --import Ubuntu E:\WSL\ubuntu E:\WSL\BACKUP\Ubuntu.tar
```
6. 验证安装
```
wsl -l -v
```
7. 启动wsl
```
wsl -d Ubuntu
```
通过这些步骤，即可成功导出、注销并导入 WSL 发行版到新的位置。


## Windows和WSL相互调用访问
### windows中调用wsl命令
比如桌面上的tar包“centos.tar” 我们可以用linux中的命令查看它的md5值
先进入桌面
md5sum centos.tar  原本的windows中没有这个命令

wsl md5sum centos.tar  使用默认的子系统调用这个命令
wsl -d centos md5sum centos.tar  使用指定的centos子系统调用该命令

### wsl中调用windows命令
在子系统中
explorer.exe .    使用windows的文件管理器查看当前目录下的文件

code .  在 VS code 装上wsl插件后，使用该命令可以在wsl环境下打开VS code

cd /mnt/c   查看C盘下的文件


## 备份还原

wsl --export centos <u>C:\users\damu\desktop\centos_backup.tar</u>  备份到桌面
wsl --import centos  <u>E:\WSL\centos</u> <u>C:\users\damu\desktop\centos_backup.tar</u> 从桌面导入centos子系统
导入的子系统目录 <u>E:\WSL\centos</u> 下会生成一个ext4.vhdx 虚拟磁盘文件，直接备份这个文件也是可以的


## 问题
### 旧版本wsl不支持systemd问题
之前的wsl版本是不支持systemd的，只需要在 /etc/wsl.conf 文件的boot分区下加上 systemd=true 即可 ，加好后如下

``` cat /etc/wsl.conf
[boot]
systemd=true
command = /home/damu/start.sh
```
command = /home/damu/start.sh 这个是开机自动启动的一个脚本，可以根据自己的情况自己设置

### wsl无法联网问题
原教程网站:https://www.ghostchu.com/archives/fix-wsl-no-internet-connection

安装 WSL-VPNKIT

访问 https://github.com/sakai135/wsl-vpnkit/releases/tag/v0.3.2 下载构建好的二进制文件，不要解压。

同目录下使用 PowerShell 运行：

```
wsl --import wsl-vpnkit $env:USERPROFILE\wsl-vpnkit wsl-vpnkit.tar.gz --version 2
```

运行`wsl-vpnkit`。这将`wsl-vpnkit`在前台运行。
```
wsl.exe -d wsl-vpnkit --cd /app wsl-vpnkit
```

或者运行下面代码创建服务：

```
wsl.exe -d wsl-vpnkit service wsl-vpnkit start
```

启动服务，WSL-VPNKIT 会创建一个到 Windows 宿主机的 VPN 连接，共享网络，WSL 的网络随即恢复。

WSL 的网络在启动 WSL-VPNKIT 后恢复正常

开机自启动
由于 WSL-VPNKIT 不会开机自启动，需要创建一个脚本帮助恢复桥接。

新建一个脚本文件文件命名为：
```
start-wsl2-vpn-bridge.bat
```

文件内容为：
```
@echo off
wsl.exe -d wsl-vpnkit service wsl-vpnkit start
```

新建一个计划任务开机启动该脚本文件即可
特别需要注意的是，一定要勾选 “使用最高权限运行”。




相关资料：
[如何优雅的使用WSL](https://www.bilibili.com/video/BV1Ku4y1f7nq/?share_source=copy_web&vd_source=**f2fa7181cec391d8d313fc7ffb8e1302)
[修改安装包实际安装位置](http://t.csdnimg.cn/TRbcX)