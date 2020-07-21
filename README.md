# docker常用指令大全

## docker安装

### 卸载原有docker
```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
### 安装docker
```
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
### 设置docker的yum源

```
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
### 安装最新版的docker

```
$ sudo yum install docker-ce docker-ce-cli containerd.io

```

### 指定版本安装docker

```
$ yum list docker-ce --showduplicates | sort -r
$ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
$ sudo yum install docker-ce-18.09.5-3.el7 docker-ce-cli-18.09.5-3.el7 containerd.io
```
### 启动docker
```
$ sudo systemctl start docker
```

### 关闭docker

```
$ sudo systemctl stop docker
```

### 测试docker安装

```
$ sudo docker run hello-world
```


### docker配置阿里云镜像加速
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://lz2nib3q.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

- `验证docker的镜像加速是否生效`

```
[root@localhost ~]# docker info
		..........
    127.0.0.0/8
   Registry Mirrors:
    'https://lz2nib3q.mirror.aliyuncs.com/'
   Live Restore Enabled: false
   Product License: Community Engine
```


### docker启动程序

```
docker  run hello-world

```


## 常用命令

```
                docker version	--------------------------	查看docker的信息
		docker info	--------------------------	查看更详细的信息
		docker --help	--------------------------	帮助命令
		
		docker images	--------------------------	列出本地所有镜像
			-a			列出所有镜像（包含中间映像层）
  	                -q			只显示镜像id
			
		docker search [options] 镜像名	     去dockerhub上查询当前镜像
			-s 指定值		列出收藏数不少于指定值的镜像
  			--no-trunc	  显示完整的镜像信息
			
		docker pull 镜像名[:TAG|@DIGEST]	----------------- 下载镜像	
		docker rmi 镜像名	--------------------------  删除镜像
		              -f		强制删除
			
```		


## 容器命令 

```
### 运行容器
	docker run 镜像名	--------------------------	镜像名新建并启动容器
    --name 					别名为容器起一个名字
    -d							启动守护式容器（在后台启动容器）
    -p 							映射端口号：原始端口号		 指定端口号启动

	例：docker run -it --name myTomcat -p 8888:8080 tomcat
   	 docker run -d --name myTomcat -P tomcat

### 查看运行的容器
	docker ps					--------------------------	列出所有正在运行的容器
	-a			正在运行的和历史运行过的容器
	-q			静默模式，只显示容器编号

### 停止|关闭|重启容器
	docker start   容器名字或者容器id  --------------- 开启容器
	docker restart 容器名或者容器id    --------------- 重启容器
	docker stop  容器名或者容器id 	    ------------------ 正常停止容器运行
	docker kill  容器名或者容器id      ------------------ 立即停止容器运行

### 删除容器
	docker rm -f 容器id和容器名     
	docker rm -f $(docker ps -aq)		--------------------------	删除所有容器

### 查看容器内进程
	docker top 容器id或者容器名 ------------------ 查看容器内的进程

### 查看查看容器内部细节
	docker inspect 容器id 		------------------ 查看容器内部细节

### 查看容器的运行日志
	docker logs [OPTIONS] 容器id或容器名	------------------ 查看容器日志
    -t			 加入时间戳
    -f			 跟随最新的日志打印
    --tail 	 数字	显示最后多少条


```



