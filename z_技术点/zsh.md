#zsh

linux终端美化 代码补全 
Zsh 是一个 UNIX 命令解释器 （shell），可用作 交互式登录 shell 和作为 shell 脚本命令 处理器。在标准 shell 中，zsh 最类似于 ksh 中，但包含许多增强功能。Zsh 具有命令行编辑功能， 内置拼写更正、可编程命令完成、 shell 函数（带有自动加载）、历史机制和 许多其他功能 如果您想尝试不同的 shell，请安装 zsh 软件包。

## 安装
这里直接安装大佬配置好的
### 在线安装方式
1. 首先安装zsh和git

```
yum install -y zsh git
```

2. 用下面任意方式安装oh my zsh
```
# curl方式安装
sh -c "$(curl -fsSL https://gitee.com/caiguang_cc/ohmyzsh/raw/master/tools/install.sh)"

# wget方式安装
sh -c "$(wget -O- https://gitee.com/caiguang_cc/ohmyzsh/raw/master/tools/install.sh)"

# fetch方式安装
sh -c "$(fetch -o - https://gitee.com/caiguang_cc/ohmyzsh/raw/master/tools/install.sh)"
```

3. 启用插件，修改~/.zshrc配置文件中的plugins

```
plugins=(
  	zsh-autosuggestions
	zsh-syntax-highlighting
  	incr
)
```

### 离线方式安装
1. 提前下载好zsh的离线rpm安装包和ccaiguang配置好的文件  [点击下载 密码：1234](https://wwen.lanzout.com/b0ukkkekf)

3. rpm方式安装zsh
```
rpm -ivh zsh-5.9-15.fc41.x86_64.rpm 
```

3. 解压caiguang的配置文件
```
tar -zxvf caiguang-zsh.tar.gz
```

4. 设置zsh作为默认开机shell
> 	打开 /etc/passwd 文件     vi /etc/passwd
> 	找到 root:x:0:0:root:/root:/bin/bash  将最后的 /bash 修改为 /zsh





配置文件 ~/.zshrc 文件修改配置




相关连接
[美观高效的命令行Shell,ZSH的安装与配置](https://www.bilibili.com/video/BV1Ga411g7Eh/?share_source=copy_web&vd_source=f2fa7181cec391d8d313fc7ffb8e1302)
[gitee仓库](https://gitee.com/damugitee/ohmyzsh)
[zsh的rpm安装包](http://rpmfind.net/linux/rpm2html/search.php?query=zsh)

