## docker 知识汇总
### 容器
    依托于Linux内核的虚拟化技术(一种虚拟化的方案)，与传统虚拟机不同，传统虚拟机通过中间层将一台或多台独立的机器虚拟运行于物理硬件之上，而容器则直接运行在操作系统内核之上的用户空间，因此容器虚拟化也被称为操作系统虚拟化，因为依赖操作系统，所以只能运行在相同或者相似内核的操作系统，虚拟机需要模拟硬件的行为，耗费资源大
    优点:  资源占用比较小，性能好
### 什么是docker
    将应用自动部署到容器的开源引擎，使用Go语言编写，2013年发布，特点：docker在虚拟化的容器中增加了一个应用程序部署引擎。该引擎的目标就是提供一个轻量快速的能够运行开发者应用程序的环境。
### docker的目标    
    提供简单轻量的建模方式。-- docker 非常容易上手，用户只需几分钟就可以将自己的应用程序docker化
    职责的逻辑分离。 -- 开发人员只需关心容器中运行的应用程序，运维人员只需维护容器。
    快速高效的开发生命周期。-- 缩短代码从开发-测试-正式环境部署的时间，相同环境。
    鼓励使用面向服务的架构。-- docker推荐单个容器只运行一个应用容器，这样就形成了一个分布式的应用程序模型，在这种模型下，应用程序或者服务都可以认为是内部互联的容器。  
### docker的使用场景
    1, 使用Docker容器的开发，测试，部署服务。
    2, 创建隔离的运行环境。
    3, 创建测试环境。
    4, 构建多用户的平台即服务（PAAS）基础设施
    5, 提高软件即服务(SAAS)的应用程序。
    6, 高性能，超高规模的宿主机部署。
### Docker的基本组成
    1, Docker Client客户端 和 Docker Daemon守护进程
       Docker 是C/S架构，Docker客户端发出命令，由docker守护进程处理并返回结果，dokcer 客户端对守护进程的访问既可以通过本地，也可以通过远程访问
    2, Docker Image镜像
       Docker容器的基石，容器基于镜像启动和运行，镜像就好比容器的源代码，存储容器的运行的各种条件。
       层叠的只读系统，它的最低端是一个引导文件系(bootfs)，当容器启动后，将会被移到内存中，而bootfs将会被卸载，第二层是root文件系统(rootfs),可以是一种或者多种的文件操作系统，比如ubantu,centos.
       docker利用联合加载技术在rootfs加载更多的只读文件系统。联合加载指的是一次同时加载多个文件系统，但是对于用户来说只能看到一个文件系统，联合加载会将所有的文件系统叠加在一起，最终的文件系统会包含所有的底层文件
    3, Docker Container容器
       通过镜像启动，容器的启动和运行阶段，写时复制
    4, Docker Registry仓库
       公有，私有   === Docker Hub
### Docker的相关技术
    NAMESPACESS 命名空间
        PID (process ID) 进程隔离
        NET(Network) 管理网络接口
        IPC(InterProcess Communcation) 管理跨进程通信的访问
        MNT(Mount) 管理挂载点
        UTS(UNIX Timesharing System) 隔离内核和版本知识
    Control groups
        资源限制
        优先级设定
        资源计量
        资源控制
    提供给docker系统的能力: 
        文件系统的隔离:每个容器都有自己的root文件系统。
        进程隔离:每个容器都运行在自己的进程环境中。
        网络隔离: 使用cgroups将CPU和内存之类的资源独立分配给每个Docker容器
### 容器的基本操作
    $ docker run -i -t IMAGE /bin/bash
        -i 用来告诉docker的守护进程为容器打开标准输入
        -t 为容器创建一个伪tty终端
    $ docker ps -a | -l
      不加参数 表示正在运行的容器
      -a 所有的容器
      -l 最新运行的容器
    $ docker inspect cn_id | cn_name
      返回容器的参数信息
    $ docker run --name = 自定义名称 -i -t
      为容器自定义名称
    $ docker start [-i] 容器名
      重新启动停止的容器
### 守护式容器 
创建守护式进程的方式
1，ctrl+p|q 退出运行时容器
2，docker run --name dc1 -d ubuntu /bin/sh -c "while true; do echo hello world; sleep     1; done" 
查看容器log
docker logs -tf --tail 0 dc1
-t 查看时间
-f 最新的log
--tail 最新的多少行
查看容器内进程
docker top 容器名
在容器内运行其他容器
docker exec -i[-d] -t 容器名

### 搭建web服务
1,暴露80端口: docker run -p 80 -i -t ubuntu /bin/bash
2,apt-get update
同步 /etc/apt/sources.list 和 /etc/apt/sources.list.d 中列出的源的索引，这样才能获取到最新的软件包。
3,apt-get install -y nginx

### docker cs 模式





    