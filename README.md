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
