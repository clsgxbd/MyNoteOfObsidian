#ngrok

官网： [https://dashboard.ngrok.com/get-started/setup](https://dashboard.ngrok.com/get-started/setup)
文档地址：[ngrok - Online in One Line](https://ngrok.com/)

百度百科：ngrok是一个反向代理，通过在公共的端点和本地运行的 Web 服务器之间建立一个安全的通道。ngrok 可捕获和分析所有通道上的流量，便于后期分析和重放.

## 简单使用
### 1. 进入[官网](https://dashboard.ngrok.com/get-started/setup)注册账号并下载ngrok
### 2. 在ngrok.exe目录下cmd打开dos命令(建议添加到环境变量，既可在任意目录下开启)
### 3. 运行以下命令将您的 authtoken 添加到默认**ngrok.yml**[配置文件](https://ngrok.com/docs/agent/config/)。您只需执行一次。Authtoken 保存在默认配置文件中。
	$YOUR_AUTHTOKEN修改为你的Authtoken，[点击查看你的Authtoken](https://dashboard.ngrok.com/get-started/your-authtoken)
```dos
ngrok config add-authtoken $YOUR_AUTHTOKEN
```
执行成功后会出现 Authtoken saved to configuration file: C:\Users\NAME\AppData\Local/ngrok/ngrok.yml
代表已经成功将Authtoken保存在本地
### 4. 将您的应用放在[临时域](https://ngrok.com/docs/network-edge/domains-and-tcp-addresses/#ephemeral-domains)上线并转发到您的上游服务。例如，如果它正在监听端口8080，请运行：
```dos
ngrok http http://localhost:8080
```
  一旦运行，界面会显示  
```
ngrok																							(Ctrl+C to quit)

? Goodbye tunnels, hello Agent Endpoints: https://ngrok.com/r/aep 

ession Status                online
ccount                       NAME (Plan: Free)
ersion                       3.20.0
egion                        Japan (jp)
Latency                      89ms
Web Interface                http://127.0.0.1:4040
orwarding                    https://xxxx.ngrok-free.app -> http://localhost:8080

onnections                   ttl     opn     rt1     rt5     p50     p90
                             0       0       0.00    0.00    0.00    0.00
```
  您的端点将列在 端点页面。
  ### 5. 此时其他人可以通过https://xxxx.ngrok-free.app访问您本地的http://localhost:8080服务

