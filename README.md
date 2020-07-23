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
- 运行容器
 
	docker run 镜像名	--------------------------	镜像名新建并启动容器
                --name 					  别名为容器起一个名字
                -d					  启动守护式容器（在后台启动容器）
                -p 					  映射端口号：原始端口号		 指定端口号启动

	例：docker run -it --name myTomcat -p 8888:8080 tomcat
   	 docker run -d --name myTomcat -P tomcat

- 查看运行的容器

	docker ps					--------------------------	列出所有正在运行的容器
	-a			                       正在运行的和历史运行过的容器
	-q			                       静默模式，只显示容器编号

- 停止|关闭|重启容器
 
	docker start   容器名字或者容器id  --------------- 开启容器
	docker restart 容器名或者容器id    --------------- 重启容器
	docker stop  容器名或者容器id 	    ------------------ 正常停止容器运行
	docker kill  容器名或者容器id      ------------------ 立即停止容器运行
- 删除容器

	docker rm -f 容器id和容器名     
	docker rm -f $(docker ps -aq)		--------------------------	删除所有容器
- 查看容器内进程
 
	docker top 容器id或者容器名 ------------------ 查看容器内的进程
- 查看查看容器内部细节
 
	docker inspect 容器id 		------------------ 查看容器内部细节
- 查看容器的运行日志

	docker logs [OPTIONS] 容器id或容器名	------------------ 查看容器日志
                 -t			 加入时间戳
                 -f			 跟随最新的日志打印
                 --tail 	 数字	显示最后多少条


```


##  进阶命令(容器内数据交互)

```
进入容器内部
	docker exec [options] 容器id 容器内命令 ------------------ 进入容器执行命令
		-i		以交互模式运行容器，通常与-t一起使用
                -t		分配一个伪终端    shell窗口   /bin/bash 

容器内安装软件
	apt-get update
	apt-get install 安装包名称

修改容器内文件
	
退出容器
	exit		退出容器

将容器打包为新的镜像
	docker commit -a="作者" -m="描述信息" 容器ID 目标镜像名称:TAG

从容器中复制文件到宿主机目录中
	docker cp 容器id:容器内资源路径 宿主机目录路径  -----------------   将容器内资源拷贝到主机上

设置容器和宿主机共享目录
		docker run -it -v /宿主机的路径:/容器内的路径:ro(只读) 镜像名
			注意: 宿主机路径必须是绝对路径,宿主机目录会覆盖容器内目录内容	
		运行 docker inspect 容器id 命令 检查json串里有没有以下内容，如果有则证明卷挂载成功。
		"Mounts": [
            {
                "Type": "bind",
                "Source": "/hostDataValueme",
                "Destination": "/containerDataValueme",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ]

打包镜像
		docker save 镜像名 -o  名称.tar
载入镜像
		docker load -i   名称.tar
```



###  安装mysql

```markdown
# 1.拉取mysql镜像到本地
	docker pull mysql:tag (tag不加默认最新版本)
	
# 2.运行mysql服务
	docker run --name mysql -e MYSQL_ROOT_PASSWORD=root -d mysql:tag  						  --没有暴露外部端口外部不能连接
	docker run --name mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d  mysql:tag  --没有暴露外部端口

# 3.进入mysql容器
	docker exec -it 容器名称|容器id bash

# 4.外部查看mysql日志
	docker logs 容器名称|容器id

# 5.使用自定义配置参数
	docker run --name mysql -v /root/mysql/conf.d:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=root -d mysql:tag

# 6.将容器数据位置与宿主机位置挂载保证数据安全
	docker run --name mysql -v /root/mysql/data:/var/lib/mysql -v /root/mysql/conf.d:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql:tag

# 7.通过其他客户端访问 如在window系统|macos系统使用客户端工具访问
	
# 8.将mysql数据库备份为sql文件
	docker exec mysql|容器id sh -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' > /root/all-databases.sql  --导出全部数据
	docker exec mysql sh -c 'exec mysqldump --databases 库表 -uroot -p"$MYSQL_ROOT_PASSWORD"' > /root/all-databases.sql  --导出指定库数据
	docker exec mysql sh -c 'exec mysqldump --no-data --databases 库表 -uroot -p"$MYSQL_ROOT_PASSWORD"' > /root/all-databases.sql  --导出指定库数据不要数据

# 9.执行sql文件到mysql中
	docker exec -i mysql sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD"' < /root/xxx.sql
```

###  安装Redis服务

```markdown
# 1.在docker hub搜索redis镜像
	docker search redis

# 2.拉取redis镜像到本地
	docker pull redis

# 3.启动redis服务运行容器
	docker run --name redis -d redis:tag (没有暴露外部端口)
	docker run --name redis -p 6379:6379 -d redis:tag (暴露外部宿主机端口为6379进行连接) 

# 4.查看启动日志
	docker logs -t -f 容器id|容器名称

# 5.进入容器内部查看
	docker exec -it 容器id|名称 bash  

# 6.加载外部自定义配置启动redis容器
	默认情况下redis官方镜像中没有redis.conf配置文件 需要去官网下载指定版本的配置文件
	1. wget http://download.redis.io/releases/redis-5.0.8.tar.gz  下载官方安装包
	2. 将官方安装包中配置文件进行复制到宿主机指定目录中如 /root/redis/redis.conf文件
	3. 修改需要自定义的配置
		 bind 0.0.0.0 开启远程权限
		 appenonly yes 开启aof持久化
	4. 加载配置启动
	docker run --name redis -v /root/redis:/usr/local/etc/redis -p 6379:6379 -d redis redis-server /usr/local/etc/redis/redis.conf  

# 7.将数据目录挂在到本地保证数据安全
	docker run --name redis -v /root/redis/data:/data -v /root/redis/redis.conf:/usr/local/etc/redis/redis.conf -p 6379:6379 -d redis redis-server 					/usr/local/etc/redis/redis.conf  
```

###  安装Nginx

```markdown
# 1.在docker hub搜索nginx
	docker search nginx

# 2.拉取nginx镜像到本地
	[root@localhost ~]# docker pull nginx
    Using default tag: latest
    latest: Pulling from library/nginx
    afb6ec6fdc1c: Pull complete 
    b90c53a0b692: Pull complete 
    11fa52a0fdc0: Pull complete 
    Digest: sha256:30dfa439718a17baafefadf16c5e7c9d0a1cde97b4fd84f63b69e13513be7097
    Status: Downloaded newer image for nginx:latest
    docker.io/library/nginx:latest

# 3.启动nginx容器
		docker run -p 80:80 --name nginx01 -d nginx

# 4.进入容器
		docker exec -it nginx01 /bin/bash
		查找目录:  whereis nginx
		配置文件:  /etc/nginx/nginx.conf

# 5.复制配置文件到宿主机
		docker cp nginx01(容器id|容器名称):/etc/nginx/nginx.conf 宿主机名录

# 6.挂在nginx配置以及html到宿主机外部
		docker run --name nginx02 -v /root/nginx/nginx.conf:/etc/nginx/nginx.conf -v /root/nginx/html:/usr/share/nginx/html -p 80:80 -d nginx		
```

----

###  安装Tomcat

```markdown
# 1.在docker hub搜索tomcat
	docker search tomcat

# 2.下载tomcat镜像
	docker pull tomcat

# 3.运行tomcat镜像
	docker run -p 8080:8080 -d --name mytomcat tomcat

# 4.进入tomcat容器
	docker exec -it mytomcat /bin/bash

# 5.将webapps目录挂载在外部
	docker run -p 8080:8080 -v /root/webapps:/usr/local/tomcat/webapps -d --name mytomcat tomcat

```

-----

###  安装MongoDB数据库

```markdown
# 1.运行mongDB
	docker run -d -p 27017:27017 --name mymongo mongo  ---无须权限
	docker logs -f mymongo --查看mongo运行日志

# 2.进入mongodb容器
	docker exec -it mymongo /bin/bash
		直接执行mongo命令进行操作

# 3.常见具有权限的容器
	docker run --name  mymongo  -p 27017:27017  -d mongo --auth

# 4.进入容器配置用户名密码
	mongo
	use admin 选择admin库
	db.createUser({user:"root",pwd:"root",roles:[{role:'root',db:'admin'}]})   //创建用户,此用户创建成功,则后续操作都需要用户认证
	exit

# 5.将mongoDB中数据目录映射到宿主机中
	docker run -d -p 27017:27017 -v /root/mongo/data:/data/db --name mymongo mongo 
```

