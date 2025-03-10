#ngrok-小米球
小米球ngrok(我感觉最好用的)  

小米ngrok下载地址参考：[http://ngrok.ciqiuwl.cn/](http://ngrok.ciqiuwl.cn/)

再程序目录下，shift右键，->在此处打开命令窗口，运行  
ngrok -config=ngrok.cfg -subdomain taohang 8080  

taohang 是你自定义的域名前缀，8080是端口

dos中会出现  
Tunnel Status online  
Version 1.7/1.7  
Forwarding http://taohang.ngrok.xiaomiqiu.cn -> 127.0.0.1:8080  
Forwarding https://taohang.ngrok.xiaomiqiu.cn -> 127.0.0.1:8080  
Web Interface 127.0.0.1:4040  
Conn 19  
Avg Conn Time 8769.42ms  
http://taohang.ngrok.xiaomiqiu.cn 和 https://taohang.ngrok.xiaomiqiu.cn  
就是你本地的8080端口

在浏览器输入后如果出现Invalid Host header （无效的请求头）  
解决：我用的是webpack-cli运行的项目（Vue项目）。  
·1.在项目的根目录的package.json文件中若有"dev",若没有添加即可。  
“dev”: “webpack-dev-server --content-base ./  
–open --inline --hot–compress --history-api-fallback --config build/webpack.dev.config.js”  
的后面添加–host 172.20.10.2（填你自己的ip地址）。  
也就是在最后一行大括号前面添加:

“dev”: “webpack-dev-server --content-base ./ --open --inline --hot–compress --history-api-fallback --config build/webpack.dev.config.js --host 172.20.10.2”  

这样就能通过ip访问了。  
2.但是通过服务器域名访问时还是显示  
Invalid Host header，这是由于新版的webpack-dev-server出于安全考虑，默认检查hostname，如果hostname  
不是配置内的，将中断访问。

解决：可以在build目录中的webpack.base.config.js中module.exports = { } 对象下，添加属性如下

```
devServer: {
  disableHostCheck: true,
},
```
若是出现手机端无加载资源现象，则是因为接口写的是localhost/127.0.0.1 换成上面的172.20.10.2（自己的IP即可）
