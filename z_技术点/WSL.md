#wsl   Windows Subsystem for Linux 
## 安装与启动
打开命令提示符
wsl --install   默认安装Ubuntu系统
wsl --list --online 查看可以安装的系统
wsl --install [NAME] 这样可以安装指定的系统
wsl -l -v 可以查看已经安装的系统  NAME前面带星号的代表默认的wsl
wsl  进入默认的子系统
wsl -d [NAME] 进入指定NAME的子系统
wsl --set-default [NAME] 设置默认的子系统
