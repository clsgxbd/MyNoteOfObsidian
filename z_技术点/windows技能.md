#windows技能

## 解决端口占用问题
#端口占用
```
	netstat -ano | findstr "8080" 查看 8080端口号占用的进程id
	taskkill -f -t /pid 12345 杀死进程12345
	jps 查看Java进程占用端口
	jstack 查看Java方法栈
```
 

## 端口转发
#端口转发
```
# 在虚拟机中查看i
hostname -I

# 在windows的终端
# 开启端口转发
netsh interface portproxy add v4tov4 listenport=<监听端口> listenaddress=0.0.0.0 connectport=<转发端口> connectaddress=<转发到的虚拟机ip>

# 查看转发的端口
netsh interface portproxy show all

# 删除端口转发
netsh interface portproxy delete v4tov4 listenport=<监听端口> lstenaddress=0.0.0.0

```
![](image/ecd102995b4adca6c2e8dc72dece4b2.png)


## 休眠功能
#休眠功能

```
	开启休眠功能 powercfg -h on
	关闭休眠功能 powercfg -h off
	设置休眠文件大小 powercfg hibernate size XX   (XX: 总内存的百分比)
```


## 检测网络r是否通畅
#检测网络是否通畅
	ping ip或域名
	ping ip或域名 -t    // 不断的ping 直到ping通,用于调试网络

## 刷新网络状态
#刷新网络状态
	刷新网络状态(修改hosts后)	ipconfig /flushdns 


## win11隐藏任务栏日期
#隐藏任务栏日期
```
win+R 输入 regedit
计算机\HKEY_CURRENT_USER\Control Panel\International

以下四个选项的值都改成空格,时间和日期就都不显示了
s1159 默认值 上午
s2359 默认值 下午
sShortDate 默认值 MM-dd
sShortTime 默认值 HH:mm

```

## win11修改任务栏宽度
#修改任务栏宽度
```
win+R 输入 regedit
计算机\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced
新建一个DWORD 32位 值 TaskbarSi 将数值改为“0”即可
```
win11任务栏调整工具
StartAllBack #StartAllBack 

## bitLocker手动上锁
- 1.同时按住windows+r 键，进入“运行”界面，在输入框输入“regedit”后回车。
- 2.在HKEY_CLASSES_ROOT中找到“Drive”,再找到“shell”,右键shell新建“项”并且命名为”runas“，后再左键双击右边框的“默认”（或右键点击“默认”后选“修改”），给个数值数据（取名随便，我取的是“上锁”）。
- 3.右键点击“runas”,同样的方法在它的下面新建项“command”,这时再双击右边的“默认”，将它的数值数据修改为“c:\windows\System32\manage-bde.exe W: -lock”（我加密的是“W”盘，盘符名自己根据需要修改）。
- 4.关闭注册表。这时候你点击硬盘你会发现多了个“上锁”，只有当你需要加密你在注册表中加密的盘的时候才会有效。当你解锁你的加密磁盘后想再次加密时，只需要点击上锁即可
- [查看原文](https://www.likecs.com/show-355258.html)

## Windows手动创建服务
#Windows手动创建服务
```shell
# 打开cmd或 PowerShell（需要管理员权限）。

# 1、创建服务   
sc create MyService binPath= "C:\Path\To\MyProgram.exe" start= auto
	# 用sc命令创建一个新的服务。例如，运行以上命令来创建一个名为“MyService”的服务：
	# 其中，“binPath”指定应用程序的路径，“start”指定服务启动类型为自动。

# 2、删除服务
sc delete MyService

# 3、启动服务。
sc start MyService

# 4、查询服务状态
sc query MyService
	# 如果服务正在运行，输出会包含“STATE : 4 RUNNING”字样。

# 5、停止服务，请运行以下命令：
sc stop MyService

# 6、更多命令
sc help # 所有命令
sc create help # create指令的详细服务
```
来源： [windows手动创建服务](https://www.jianshu.com/p/8d2c8bb987c0)


## WebDAV映射网络驱动器
#WebDAV映射网络驱动器 #映射Alist

### Windowsb不支持HTTP方式的 WebDAV 映射
来源：[WebDAV映射网络驱动器](https://skylens.github.io/2017/12/14/win10_webdav.html)

发现 Windows 默认只支持 HTTPS 方式的 WebDAV 映射，默认不支持 HTTP 方式的 WebDAV 映射，有没有办法在不安装第三方客户端的情况下，让 Windows 支持 HTTP 方式的 WebDAV 映射呢？上网一查是有的，只不过要改注册表

**具体步骤:**
1. 改注册表
计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WebClient\Parameters 下，把 BasicAuthLevel 的值改为 2 ( ‘1’ 默认只支持 HTTPS，’2’ 支持 HTTP 和 HTTPS)
![](https://img2020.cnblogs.com/blog/1988160/202112/1988160-20211218112347729-1225768691.png)


2. 重启服务
**以管理员身份运行**
```dos
net stop webclient 
net start webclient
```

3. 映射网络驱动器
计算机 –> 右键 –> 映射网络驱动器 –> 填写相应的信息 –> 完成
  ![](https://skylens.github.io/assets/post_pictures/win10_webdav.png)

### 问题：Windows文件大小超出允许的限制，无法保存
来源: [Windows文件大小超出允许的限制，无法保存](https://blog.52nyg.com/2021/06/691)

WebDav连接上后，结果提示：
在webdav中复制文件出来会提示此错误:"文件大小超出允许的限制，无法保存"
![Windows文件大小超出允许的限制，无法保存插图](https://blog-cdn.52nyg.com/wp-content/uploads/2021/06/Snipaste_2021-06-15_16-35-21.jpg "Windows文件大小超出允许的限制，无法保存插图")

这是因为Windows默认限制为50MB文件大小 超出不允许复制
解决方法：  
1. 按 Win + R 键  
2. 在运行窗口输入regedit,按回车  
3. 在打开的注册表编辑器中进入这个地址：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WebClient\Parameters  
4. 找到FileSizeLimitInBytes，双击打开  
5. 在打开的设置窗口，选择decimal（十进制）  
6. 修改限制的大小，比如加个0（默认是50M,即50000000）其实有最大值限制，4294967295~4,095.9MB=4GB，也就是32位系统能提供的最大值了
![](https://img2020.cnblogs.com/blog/1988160/202112/1988160-20211218113014692-237175203.png)

7. 修改好后，再选择hexadecimal（十六进制）  
8. 保存，然后重启电脑



## 静态IP地址变成169.254.x.x
来源：[IP地址变成169.254.x.x 的解决办法](https://www.cnblogs.com/harmanchen/p/16058463.html)
原因：一般原因可能是路由器<u> 未开启 DHCP 服务</u> 时，当 IP 地址冲突或其他冲突出现时，客户端操作系统中的  <u>自动专用 IP 寻址（Automatic Private IP Addressing, APIPA）</u> 功能会为本机自动分配一个 169.254.x.x 段地址。

解决方法： win+R → 输入 regedit → 回车 → 打开注册表 → 进入路径：
```
计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters
```
 → 该路径下 新建 DWORD (32 位) 值，命名为  ArpRetryCount ，重启即可



