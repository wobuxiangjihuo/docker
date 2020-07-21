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







