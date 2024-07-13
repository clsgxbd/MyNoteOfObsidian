#wsl   Windows Subsystem for Linux 
## 安装与启动
### 安装默认可以安装的子系统发行版
打开命令提示符
wsl --install   默认安装Ubuntu系统
wsl --list --online 查看可以安装的系统
wsl --install [NAME] 这样可以安装指定的系统
wsl -l -v 可以查看已经安装的系统  NAME前面带星号的代表默认的wsl
wsl  进入默认的子系统
wsl -d [NAME] 进入指定NAME的子系统
wsl --set-default [NAME] 设置默认的子系统

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
wsl wsl --import centos  <u>E:\WSL\centos</u> <u>C:\users\damu\desktop\centos_backup.tar</u> 从桌面导入centos子系统
导入的子系统目录 <u>E:\WSL\centos</u> 下会生成一个ext4.vhdx 虚拟磁盘文件，直接备份这个文件也是可以的


## 问题
之前的wsl版本是不支持systemd的，只需要在 /etc/wsl.conf 文件的boot分区下加上 systemd=true 即可 ，加好后如下

``` cat /etc/wsl.conf
[boot]
systemd=true
command = /home/damu/start.sh
```
command = /home/damu/start.sh 这个是开机自动启动的一个脚本，可以根据自己的情况自己设置






相关学习视频： [如何优雅的使用WSL](https://www.bilibili.com/video/BV1Ku4y1f7nq/?share_source=copy_web&vd_source=**f2fa7181cec391d8d313fc7ffb8e1302)