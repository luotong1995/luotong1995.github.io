---
title: Docker安装配置及使用
date: 2019-04-22 19:50:52
tags:
categories: Docker
updated: 2019-04-22 16:29:00
---
![Docker](/images/docker/docker.png)

Docker作为一个开源的应用容器引擎，已经在各个平台上进行了支持其中包括Linux、Mac、Windows、AWS和Azure。在正常使用的过程中都是基于Linux的，下面简单介绍一些在Linux上的安装和使用。
## Docker安装配置
Docker安装分为离线安装和在线安装，离线安装是基于二进制的方式进行安装。在线安装是根据不同的Linux环境进行安装。

### 离线安装
如果需要在外网络环境下使用Docker，可以使用静态二进制文件进行安装，在需要进行升级的时候也使用二进制文件进行安装和升级。
#### 首先下载Docker二进制文件
进入如下网址选择自己想要的版本进行下载安装，[Dokcer二进制文件下载地址](https://download.docker.com/linux/static/stable/ "Dokcer二进制文件下载地址")，在本博客中选择的是18.06版本进行安装使用。
``` bash
[root@host_name ～] wget https://download.docker.com/linux/static/stable/x86_64/docker-18.06.0-ce.tgz
```
#### 安装Docker
- 解压上一步已经下载的Docker二进制文件
``` bash
# 解压二进制文件压缩包
[root@host_name ～] tar xzvf docker-18.06.0-ce.tgz
```
- 将解压完成之后的可执行二进制文件拷贝到/usr/bin目录
```bash
# 拷贝可运行的二进制文件到/usr/bin目录下
[root@host_name ～] sudo cp docker/* /usr/bin
```
- 为Docker配置后台系统，在这里使用Linux的系统服务来完成Docker的后台启动
``` bash
# 添加Docker到system service, 添加docker.service文件
[root@host_name ~] vim /usr/lib/systemd/system/docker.service
```
- 在docker.service文件中添加如下内容
```bash
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
#TasksMax=infinity
TimeoutStartSec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
# kill only the docker process, not all processes in the cgroup
KillMode=process
# restart the docker process if it exits prematurely
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target
```
- 启动Docker服务
```bash
# 启动Docker
[root@host_name ~] systemctl start docker.service
```

- 停止Docker服务
```bash
# 停止Docker
[root@host_name ~] systemctl stop docker.service
```

- 重启Docker服务
``` bash
# 重启Docker
[root@host_name ~] systemctl restart docker.service
```

#### 根据需要添加私有镜像库
Docker私有镜像库是一个用来存储已经创建好的镜像的地方，Harbor是目前得到广泛使用的镜像库，接下来也会介绍一下Harbor的安装和使用。下面就是进行私有镜像库配置的过程。

- 修改docker启动文件, 可以根据需要添加镜像库的ip地址，下面的bash命令中的ip应改为已经存在的私有镜像库的ip地址
``` bash
[root@host_name ~] vim /usr/lib/systemd/system/docker.service
ExecStart=/usr/bin/dockerd --insecure-registry <ip1> --insecure-registry <ip2>
```
- 修改完成之后重新启动Docker
```bash
[root@host_name ~] systemctl daemon-reload
[root@host_name ~] systemctl restart docker.service
```

### 在线安装
Docker的在线安装在不同平台下的Linux方式有所不同，Ubuntu、CentOS、Debian、Fedora，详细信息可以参考，[Linux安装Docker](https://docs.docker.com/install/linux/docker-ce/centos/, "Linux安装Docker")，下面介绍一下Ubuntu的安装过程。
#### 卸载老版本（如果存在）
卸载老版本Docker
``` bash
[root@host_name ~] sudo apt-get remove docker docker-engine docker.io containerd runc
```
#### 设置Docker官方版本库
- 更新apt-get库
``` bash
[root@host_name ~] sudo apt-get update
```
- 安装设置版本库所需要的库
``` bash
[root@host_name ~] sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
- 添加Docker官方gpg
``` bash
[root@host_name ~] curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
- 设置Docker版本库
``` bash
[root@host_name ~] sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
#### 安装Docker
```bash
[root@host_name ~] sudo apt-get update
[root@host_name ~] sudo apt-get install docker-ce
```

## 安装Harbor
Harbor是一个用于存储和分发Docker镜像的企业级Registry服务器，harbor的镜像存储使用的是官方的docker registry服务去完成，harbor的功能是在此之上提供用户权限管理、镜像复制等功能，提高使用的registry的效率，所以说Harbor给用户提供了用户权限管理、项目管理和镜像复制等功能。

### 安装docker-compose
- 下载docker-compose二进制文件
``` bash
[root@host_name ~] sudo curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
- 添加可执行权限
``` bash
[root@host_name ~] sudo chmod +x /usr/local/bin/docker-compose
```

### 下载Harbor并安装
- 下载Harbor离线安装文件
```bash
[root@host_name ~] wget https://storage.googleapis.com/harbor-releases/release-1.6.0/harbor-offline-installer-v1.6.0.tgz
```
- 解压
``` bash
[root@host_name ~] tar xvf harbor-offline-installer-v1.6.0.tgz
```
- 修改Harbor配置文件
```bash
[root@host_name ~] vim harbor.cfg
# 修改下面两项内容
hostname = 9.111.215.134
harbor_admin_password = Password
```
- 安装Harbor
```bash
# --with-chartmuseum 是增加helm-chart的管理功能
[root@host_name ~] sudo ./install.sh --with-chartmuseum
```

### Harbor使用命令
- 停止Harbor
``` bash
[root@host_name ~] docker-compose stop
[root@host_name ~] docker-compose down -v
```
- 重启Harbor
```bash
[root@host_name ~] docker-compose start
```
- 删除Harbor数据
```bash
[root@host_name ~] rm -r /data/database
[root@host_name ~] rm -r /data/registry
```

### Docker build