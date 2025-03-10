#ngrox-国内版

2.2ngrox国内简介  
因为国外服务器的速度限制，所有ngrox还有个国内的，叫做Sunny-Ngrok。  

网站首页：[https://www.ngrok.cc/_book/start/ngrok_windows.html](https://www.ngrok.cc/_book/start/ngrok_windows.html)

后台地址v[https://www.ngrok.cc/user](https://www.ngrok.cc/user)

现在网站呢注册好之后，登录后台地址，输入账户密码，如下：  

在这里插入图片描述

使用：  
第一种：在sunny.exe所在的目录 通过cmd命令行执行sunny.exe clientid 隧道id  
多个隧道启动，执行：sunny.exe clientid 隧道id,隧道id 也就是中间加了个逗号  
第二种：另一种方式通过 Sunny-Ngrok启动工具.bat 启动，直接输入隧道id就好了

Sunny-Ngrok 和ngrok不同的是:

它是国内的，只需要绑定隧道id即可使用，网速较快，而ngrok是国外的，网速较慢；  
Sunny-Ngrok端口配置是在后台界面配置的（后台界面->隧道管理），里面可以配置隧道id,隧道名称，隧道协议，本地端口，服务器类型，到期日期，赠送域名，还可以编辑，删除等  
在链接好隧道id后，它会直接弹出配置好的本地端口所对应的万网链接地址，浏览器输入地址即可访问