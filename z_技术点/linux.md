
## #Linux启动与停止服务

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

