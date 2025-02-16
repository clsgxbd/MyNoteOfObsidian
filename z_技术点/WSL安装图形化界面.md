#wsl安装图形化页面

## 这里以ubuntu系统为例

### 一、查看可安装的linux版本

```
wsl --list --online
```

### 二、安装linux发行版本

```
wsl --install -d ubuntu-24.04
```

### 三、更换国内软件源
软件源保存在文件：/etc/apt/sources.list.d/ubuntu.sources  

#### 1. 备份原来的软件源
将ubuntu.sources重命名为ubuntu.sources.bak

#### 2. 新建一个文件以 ".sources" 结尾，例如 "tsinghua.sources"，文件内容如下：
```
Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu/
#如果使用其他镜像站，上面这行可以改成其他镜像站的网址(如：https://mirrors.aliyun.com/ubuntu/)
Suites: noble noble-updates noble-backports
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu/
#如果安全更新需要使用镜像站，上面这行可以改成其他镜像站的网址(如：https://mirrors.aliyun.com/ubuntu/)
Suites: noble-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

更新源地址，可以用下面国内的源（tsinghua源或aliyun源等）
```
清华源 `https://mirrors.tuna.tsinghua.edu.cn/ubuntu/`  
阿里云 `https://mirrors.aliyun.com/ubuntu/`  
腾讯云 `https://mirrors.cloud.tencent.com/ubuntu/`  
华为云 `https://mirrors.huaweicloud.com/ubuntu/`  
中科大源 `https://mirrors.ustc.edu.cn/ubuntu/`
```

#### 3. 更新源列表
```
sudo apt update
```
#### 3. 更新系统软件包
```
sudo apt upgrade -y
```

### 四、图形化界面
#### 1. 安装Ubuntu桌面版
可以根据自己的喜好安装 KDE、Gnome、xfce、lxde 等桌面环境。这里以Gnome为例进行安装。WSL Ubuntu命令行输入：
```
# 安装 Ubuntu 桌面版
sudo apt install ubuntu-desktop
```
安装其他桌面的话：
```
#KDE
sudo apt install kubuntu-desktop 

#Xfce 
sudo apt install xubuntu-desktop
```
#### 2. 安装XRDP
‌ 在使用xrdp进行远程桌面连接时，默认情况下，xrdp使用/etc/ssl/private/ssl-cert-snakeoil.key证书，该证书仅对ssl-cert用户组可读。因此，需要将xrdp用户添加到ssl-cert用户组，以确保xrdp服务能够正常访问和验证SSL证书‌。
具体操作步骤如下：
（1）安装xrdp服务
```
sudo apt-get install xrdp
```

（2）启动xrdp服务
```
sudo systemctl start xrdp
```

（3）将xrdp用户添加到`ssl-cert`用户组
```
sudo adduser xrdp ssl-cert
```

（4）将端口从3389改为3390，因为此前默认的3389端口已保留用于ubuntu shell
```
sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
```

（5）配置启动session，否则远程桌面登录输入密码之后会直接闪退
```
echo "gnome-session" > ~/.xsession
```

（6）重启xrdp服务
```
sudo systemctl restart xrdp
```

（7）这时可以查看xrdp服务状态，有绿色的running代表服务成功运行
```
sudo systemctl status xrdp
```

### 五、从Windows启动桌面版Ubuntu  
1. 按住Windows+ R，在运行框中输入mstsc后按回车
  ![](image/Pasted%20image%2020250216213202.png)
2. 计算机输入localhost:3390后按回车
	注：这里:为英文状态下输入；在这里可以在显示选项里将这个连接另存到桌面，方便以后打开。
![](image/Pasted%20image%2020250216213325.png)
3. 在这里输入你的账户和密码并点击OK
![](image/Pasted%20image%2020250216213855.png)
4. 首次进入会有几分钟黑屏，正常现象，稍等一会儿就进入桌面了
![](image/Pasted%20image%2020250216214054.png)


### 六、解决原生 GNOME 桌面布局,即缺少了左侧的 Dock 栏、桌面图标、托盘图标
使用 gnome-shell-extensions 包 （最简单最安全的方式）
1. 安装gnome tweak tool
```text
sudo apt install gnome-tweak-tool
```
2. 安装 gnome-shell-extensions 包
```text
sudo apt install gnome-shell-extensions
```
3. 重启ubuntu
4. 按照图示打开扩展gnome-tweak
	   ![](image/Pasted%20image%2020250216214914.png)
5. 开启你需要的扩展即可，例如桌面图标(Desktop Icons NG)，下方任务栏Ubuntu Dock等
   注： 你也可以在扩展右侧的三个小点里找到设置选项来对扩展进行个性化设置
	![](image/Pasted%20image%2020250216215318.png)



参考链接：
[# 通过WSL2安装Ubuntu24.04系统及图形化界面](https://blog.csdn.net/ddafei/article/details/142798010?fromshare=blogdetail&sharetype=blogdetail&sharerId=142798010&sharerefer=PC&sharesource=weixin_56293388&sharefrom=from_link)
[# 基于wsl的Ubuntu20.04上安装桌面环境](https://blog.csdn.net/weixin_51551506/article/details/137457894?fromshare=blogdetail&sharetype=blogdetail&sharerId=137457894&sharerefer=PC&sharesource=weixin_56293388&sharefrom=from_link)
[如何使用 GNOME Shell 扩展 - Linux中国的文章 - 知乎](https://zhuanlan.zhihu.com/p/34608388)