# docker

>Docker 是一个开源的应用容器引擎，基于 Go 语言并遵从Apache2.0协议开源。
>
>Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。
>
>容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

##一、docker优点

1. 持续集成
2. 版本控制
3. 可移植性
4. 隔离性
5. 安全性

## 二、docker应用场景

+ web应用的自动化打包和发布

+ 自动化测试和持续集成、发布

+ 在服务型环境中部署和调整数据库或其他的后台应用

+ 简化配置

  > Docker在降低额外开销的情况下提供了同样的功能. 它能让你将运行环境和配置放在代码汇总然后部署, 同一个Docker的配置可以在不同的环境环境中使用, 这样就降低了硬件要求和应用环境之间耦合度. 

+ 保持开发、测试、上线环境的一致性

  > 代码从开发者的机器到最终在生产环境上的部署, 需要经过很多的中坚环境. 而每一个中间环境都有自己微小的差别, Docker给应用提供了一个从开发到上线均一致的环境, 让代码的流水线变得简单不少. 

+ 提升开发效率

+ 隔离应用

  >开发时会在一个台机器上运行不同的应用.
  >
  >一、 为了降低成本, 进行服务器整合
  >
  >二、将一个整体式的应用拆分成低耦合的单个服务(微服务架构)

+ 快速部署 

  > Docker为进程创建一个容器, 不需要启动一个操作系统, 时间缩短为秒级别. 

## 三、docker架构

> Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。
>
> Docker 容器通过 Docker 镜像来创建。

### 1、docker基本组成

+ docker镜像

  > docker镜像是用于创建docker容器的模板

+ docker容器

  > 容器是独立运行的一个或一组应用

+ docker客户端

  > docker客户端通过命令行或其他工具使用docker API与docker的守护进程通信

+ docker主机

  > 一个物理或者虚拟的机器用于执行 Docker 守护进程和容器

+ docker仓库

  > Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库
  >
  > Docker Hub([https://hub.docker.com](https://hub.docker.com/)) 提供了庞大的镜像集合供使用

+ Docker Machine 

  > Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure

## 四、docker

###（1）docker依赖的Linux内核特性

1. Namespaces 命名空间

   > + 系统资源的隔离
   >
   >   > + PID(Process ID) 进程隔离
   >   > + NET(Network) 管理网络接口
   >   > + IPC(InterProcess Communication) 管理跨进程通信的访问
   >   > + MNT(Mount) 管理挂载点
   >   > + UTS(Unix Timesharing System) 隔离内核和版本标识
   >
   > + 进程、网络、文件系统...

2. Control groups(cgroups) 控制组

+ 资源限制
+ 优先级设定
+ 资源计量
+ 资源控制

### （2）docker容器的能力

+ 文件系统隔离：每个容器都有自己的root文件系统
+ 进程隔离：每个容器都运行在自己的进程环境中
+ 网络隔离：容器间的虚拟网络接口和IP地址是分开的
+ 资源隔离和分组：使用cgroups将CPU和内存之类的资源独立分配给每个docker容器

## 五、容器的基本操作

### 启动容器

> docker run IMAGE [COMMAND][ARG...]
>
> run 在新容器中执行命令

### 启动交互式容器

> docker run -i -t IMAGE /bin/bash
>
> -i --interactive=true | fasle 默认是false
>
> -t --tty=true | false 默认是false

### 查看容器

> docker ps [-a\][-l]
>
> -a 所有容器
>
> -l 最新的容器

> docker inspect

### 自定义容器名

> docker run --name=**  -i -t *** /bin/bash

### 重新启动停止的容器

> docker start [-i] 容器名

### 删除停止的容器

> docker rm 容器名

## 六、守护式容器

### 什么是守护式容器

+ 能够长期运行
+ 没有交互式会话
+ 适合运行应用程序和服务

### 以守护形式运行容器

> docker run -i -t IMAGE /bin/bash
>
> 使用ctrl+p ctrl+q以守护形式运行容器

### 附加到运行中的容器

> docker attach 容器名

### 启动守护式容器

> docker run -d 镜像名 [COMMAND\][ARG...]

###查看容器内部运行情况

1. 查看容器日志

> docker logs [-f\][-t\][--tail] 容器名
>
> -f --follows=true | false 默认为false
>
> -t --timestamps=true | false 默认为false
>
> --tail="all" 默认返回所有日志

2. 查看容器内部进程

> docker top 容器名

### 在运行中的容器内启动新进程

> docker exec [-d\][-i\][-t\] 容器名[COMMAND\][ARG...]

###停止守护式容器

> docker stop 容器名 （等待服务器的停止）
>
> docker kill 容器名（直接停止容器）

##七、在容器中部署静态网站--设置容器的端口映射

####设置容器的端口映射

> run [-P\][-p]

> -P,--publish-all=true | false默认为false
> docker run -P -i -t ubuntu /bin/bash

> -p,--publish=[]
>
> > 1. containerPort
> >
> >    docker run -p 80 -i -t ubuntu /bin/bash
> >
> > 2. hostPort:containerPort
> >    docker run -p 8080:80 -i -t ubuntu /bin/bash
> >
> > 3. ip::containerPort
> >    docker run -p 0.0.0.0:80 -i -t ubuntu /bin/bash
> >
> > 4. ip:hostPort:containerPort
> >    docker run -p 0.0.0.0:8080:80 -i -t ubuntu /bin/bash
>

## 八、查看和删除镜像

+ docker images [OPTSIONS\][REPOSITORY]

> -a,--all=false
>
> -f,--filter=[]
>
> --no-trunc=false
>
> -q,--quiet=false