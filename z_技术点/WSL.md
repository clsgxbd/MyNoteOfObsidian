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






