#docker 

 注意: 如果防火墙是开启的，`则先关闭防火墙，并重启docker`，否则后续安装后可能无法启动 
 ```
 关闭防火墙:
	systemctl stop firewalld
 禁止防火墙开机自启: 
	 systemctl disable firewalld
```

## docker 常用命令总结

### 安装与卸载docker
1.    官网中文安装参考手册
[https://docs.docker.com/install/linux/docker-ce/centos/](https://docs.docker.com/install/linux/docker-ce/centos/)

2.    确定你是CentOS7及以上版本
cat /etc/redhat-release

3.    yum安装gcc相关
o    CentOS7能上外网
o    检查gcc和g++是否安装好，如果没有安装好，则需要安装。
o    安装gcc和g++
yum -y install gcc
yum -y install gcc-c++

4.    安装需要的软件包
yum install -y yum-utils device-mapper-persistent-data lvm2

5.    设置镜像仓库
o    推荐：阿里云服务器
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

6.    更新yum软件包索引
yum makecache fast

7.    安装DOCKER CE（社区版）（DOCKER EE企业版收费）
yum -y install docker-ce

8.    启动docker
o    手动启动：systemctl start docker
o    自动启动：systemctl enable docker 

9.    测试
o    检查版本：docker version
o    下载并运行HelloWorld：docker run hello-world
·         如果下载不下来，可以配置镜像加速器
·         输出这段提示以后，hello world就会停止运行，容器自动终止。

10. 配置镜像加速CentOS7版本
mkdir -p /etc/docker
vim  /etc/docker/daemon.json
·         **#****网易云**
```
{
"registry-mirrors": ["http://hub-mirror.c.163.com"]
}
```

·         **#****阿里云(推荐)**
```
{
"registry-mirrors": ["https://8y2y8njn.mirror.aliyuncs.com"]
}
```

·         **#ustc**
#是老牌的linux镜像服务提供者了，还在遥远的ubuntu 5.04版本的时候就在用。ustc的docker镜像加速器速度很快。
#ustc docker mirror的优势之一就是不需要注册，是真正的公共服务。
#[https://lug.ustc.edu.cn/wiki/mirrors/help/docker](https://lug.ustc.edu.cn/wiki/mirrors/help/docker)
在该文件中输入如下内容：
```
{  
"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]  
}
```
systemctl daemon-reload
systemctl restart docker

11. 卸载

o    systemctl stop docker
o    yum -y remove docker-ce
o    rm -rf /var/lib/docker
o    卸载旧版本
o    2019.11英文官网版本
·         最新的英文版：https://docs.docker.com/install/linux/docker-ce/centos/#uninstall-old-versions
```
yum remove docker \
		docker-client \
		docker-client-latest \
		docker-common \
		docker-latest \
		docker-latest-logrotate \
		docker-logrotate \
		docker-engine
```




### 操作 docker
```docker
1. 查看版本
   docker -v 
2. 查看详情
   docker info 
3. 启动
   systemctl start docker 
4. 重启
   systemctl restart docker 
5. 关闭
   systemctl stop docker 
6. 开机自启
   systemctl enable docker 
```


### 操作镜像 images
```images
1. 查看镜像列表
   docker images 
2. 搜索镜像
   docker search 镜像名称:镜像版本 
3. 拉取镜像
   docker pull 镜像名称:镜像版本
4. 删除镜像
   docker rmi 镜像名称:镜像版本
5. 镜像保存成文件
   docker save -o 文件名 镜像名称:镜像版本
6. 文件转换成镜像
   docker load -i 文件名称(xxx.zip)
7. 制作镜像 
   docker build -t 镜像名称:镜像版本 . (注意最后有个 . 号)
```


### 操作容器

```
1. 查看运行中的容器
   docker ps
2. 查看所有容器
   docker ps -a
3. 创建容器
      docker run -p 宿主机端口:容器内部服务端口 (端口映射)
			  -i (运行)
			  -t (交互)
			  -e 环境变量 (环境)
			  -d (守护式启动/后台启动)
			  --name=容器名称 (容器名称)
			  --restart=always (随docker自启)
			  -v (挂载)
			  容器id(或 容器名称)
4. 删除容器
   docker rm -f (加 -f 可以删除运行中的容器)
5. 启动容器
   docker start 容器id(或 容器名称)
6. 关闭容器
   docker stop 容器id(或 容器名称)
7. 重启容器
   docker restart 容器id(或 容器名称)
8. 进入容器
   docker exec -it 容器id(或 容器名称) /bin/bash
   docker attach 容器id  ( 注意: 这种方法进入容器会存在一个问题，当多个终端同时进入容器时，所有窗口会同步显示，所以不太适合生产环境使用)
9. 查看日志
   docker -logs -f 容器id(或 容器名称)
```

