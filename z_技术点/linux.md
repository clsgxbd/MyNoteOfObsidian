
## Linux启动与停止服务

```shell
#启动
systemctl start mysqld
#关闭
systemctl stop mysqld
#重启
systemctl restart mysqld
#查看服务状态
systemctl status mysqld
```

## 修改yum镜像源
教程： http://t.csdnimg.cn/kO2cF

Centos7本身自带的yum源下载速度教慢，需要国内更换yum源进行提速。

1. 首先备份/etc/yum.repos.d/CentOS-Base.repo

```
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

2. 下载最新的 CentOS-Base.repo 文件替换源文件

```
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```
或者
```
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```

3. 生成缓存

```
yum clean all
yum makecache
```




[rpm命令](rpm命令.md)



## centos安装桌面环境
1. 更新yum
```
sudo yum update
```

2. 安装桌面环境
```
sudo yum groupinstall "GNOME Desktop" -y
```

3. 设置默认启动方式
```
sudo systemctl set-default graphical.target
```

4. 重新启动系统
```
sudo reboot
```


## 问题
### dpkg 处理软件包 xxx (configure)时出错解决办法
#dpkg报错
**第一步：备份**
```
sudo mv /var/lib/dpkg/info /var/lib/dpkg/info.bak
```
**第二步：新建**
```
sudo mkdir /var/lib/dpkg/info
```
**第三步：更新**
```
sudo apt update && sudo apt install -f
```
**第四步：替换**   
把更新的文件替换到备份文件夹
```
sudo mv /var/lib/dpkg/info/* /var/lib/dpkg/info.bak 
```
**第五步：删除**
把自己新建的info文件夹删掉
```
sudo rm -rf /var/lib/dpkg/info
```
**第六步：还原**
把备份的info.bak还原
```
sudo mv /var/lib/dpkg/info.bak /var/lib/dpkg/info
```
上述过程中如果出现错误，暂时忽略，将上述六步完成后再执行安装命令观察问题是否解决。

参考链接：
[dpkg 处理软件包 xxx (configure)时出错解决办法](https://www.cnblogs.com/while19/p/16197181.html)