#zsh

linux终端美化 代码补全 

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



配置文件 ~/.zshrc 文件修改配置




相关连接
[美观高效的命令行Shell,ZSH的安装与配置](https://www.bilibili.com/video/BV1Ga411g7Eh/?share_source=copy_web&vd_source=f2fa7181cec391d8d313fc7ffb8e1302)
[gitee仓库](https://gitee.com/damugitee/ohmyzsh)
