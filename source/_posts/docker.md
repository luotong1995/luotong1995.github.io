---
title: Docker介绍
date: 2019-04-22 14:50:52
tags:
categories: Docker
updated: 2019-04-22 16:29:00
---
![Docker](/images/docker/docker.png)
### 1. Docker介绍
Docker是一个开源的应用容器引擎，由DotCloud开源的，可以让开发人员随意的将其应用打包到一个容器中。基于Docker的沙箱环境可以实现轻型隔离，多个容器间不会相互影响；Dokcer可以允许应用程序在笔记本电脑、内部服务器、公有云、私有云等上允许，Docker可以自动化打包和部署任何应用，方便地创建一个轻量级私有PaaS云，也可以用于搭建开发测试环境以及部署可扩展的web应用等。


### 2. Docker和VM对比
Docker的实现原理是虚拟化，但是却不同于VM，VM是在宿主机上运行一个完成的操作系统，会占用很多CPU、内存、硬盘等资源。但是Docker不同于VM，Docker的运行只包含应用程序和运行程序的一些依赖库，基于容器机制运行在宿主机器上，容器将应用程序服务或功能与所有库，配置文件，依赖项和其他必要部分进行打包，能够在几秒钟完成启动。与VM相比，Docker的优势包括高效的应用程序开发，更低的资源使用和更快的部署。
![VM和容器的对比](/images/docker/VM-and-Docker.png)

| VM| 容器| 
| ------ | ------ | 
| 应用程序需要操作系统的完整实例 |  应用程序与服务器共享操作系统，因此启动/关闭非常快|
| Hypervisor管理虚拟分区，有助于提高计算性能，但是它有些庞大  | Docker守护程序使用Docker API或命令行监视和控制容器 |
|通过管理程序执行运行的进程会导致开销 | 进程在服务器上以本机方式运行，从而实现低CPU /内存开销|

### 3. Docker架构
下面是一个使用Docker进行开发的架构图。
![Docker架构图](/images/docker/Docker-Architecture.png)
- Docker Clinet(docker)，是允许使用REST API（http请求）在用户和Docker守护程序之间进行通信的接口。

- Docker daemon(docker),在主机上运行，处理服务请求（例如，构建和存储映像，创建，运行和监视容器）。

- Docker registry，是具有公共和私有访问权限的Docker容器映像的备份，是一个存储库，包括Harbor等。

- Docker file，包含构建Docker镜像的说明，就是构建镜像的必要文件，也是主要依赖和编写的文件。

- Docker image，是一个只读模板，其中包含用于创建Docker容器的指令/说明（当构建映像时，它将作为容器生效）。

- Docker container，正在运行应用程序。可以有多个容器基于同一镜像运行。 可以使用Docker API或命令行创建，启动，停止，移动或删除。

同样重要的是要注意Docker使用以下操作系统功能：
- Namespace，命名空间确保在容器中运行的进程无法查看或影响在容器外部运行的进程。
- Control Groups，用于资源管理和控制
- UnionFS (FileSystem)，用于进行文件管理，是Linux中将不同的物理地址的目录挂载在一起的一种方法