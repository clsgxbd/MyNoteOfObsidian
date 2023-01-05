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
