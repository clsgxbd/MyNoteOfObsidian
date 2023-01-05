#docker 

 注意: 如果防火墙是开启的，`则先关闭防火墙，并重启docker`，否则后续安装后可能无法启动 
 ```
 关闭防火墙:
	systemctl stop firewalld
 禁止防火墙开机自启: 
	 systemctl disable firewalld
```

## docker 常用命令总结

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
   docker exec -it 容器id(或 容器名称) bash
   docker attach 容器id  ( 注意: 这种方法进入容器会存在一个问题，当多个终端同时进入容器时，所有窗口会同步显示，所以不太适合生产环境使用)
9. 查看日志
   docker -logs -f 容器id(或 容器名称)
```

