#windows技能

## 解决端口占用问题
#端口占用
	netstat -ano | findstr "8080" 查看 8080端口号占用的进程id
	taskkill -f -t /pid 12345 杀死进程12345
	jps 查看Java进程占用端口
	jstack 查看Java方法栈

## 休眠功能
#休眠功能
	开启休眠功能 powercfg -h on
	关闭休眠功能 powercfg -h off
	设置休眠文件大小 powercfg hibernate size XX   (XX: 总内存的百分比)

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

