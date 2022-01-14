# 基础

**Docker**是一个[开放源代码](https://zh.wikipedia.org/wiki/開放原始碼)软件项目，让应用程序部署在[软件货柜](https://zh.wikipedia.org/wiki/作業系統層虛擬化)下的工作可以自动化进行，借此在[Linux](https://zh.wikipedia.org/wiki/Linux)操作系统上，提供一个额外的软件[抽象层](https://zh.wikipedia.org/wiki/抽象層)，以及[操作系统层虚拟化](https://zh.wikipedia.org/wiki/作業系統層虛擬化)的自动管理机制

简单来说,Docker 通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户程序及其运行环境能够做到“一次封装，到处运行”。

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200525135454.png)

**Docker镜像的设计，使得Docker得以打破过去「程序即应用」的观念。透过镜像(images)将作业系统核心除外，运作应用程式所需要的系统环境，由下而上打包，达到应用程式跨平台间的无缝接轨运作。**

Docker是基于Go语言实现的云开源项目。

 Docker 就是在Linux 容器技术的基础上发展过来的，将应用运行在 Docker 容器上面，而 Docker 容器在任何操作系统上都是一致的，这就实现了`跨平台、跨服务器`。

> 简单来说,Docker 的出现解决了运行环境和配置问题软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术

**Docker与VM对比**

1. Docker有着比虚拟机更少的抽象层。由于docker不需要Hypervisor实现硬件资源虚拟化，在CPU、内存利用率上docker将会在效率上有明显优势。

2. Docker利用的是宿主机的内核，而不需要重新加载操作系统。

**Docker 和 VM 对比图**

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200525150538.png)

**Docker 和 VM 特点对比图**

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200525150633.png)

**Docker 虚拟化**

* 虚拟机技术

  虚拟机（virtual machine）就是带环境安装的一种解决方案。

  ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200525140019.png)

  虚拟机的缺点：

  1. 资源占用多
  2. 冗余步骤多
  3. 启动慢

* 容器虚拟化技术

  Linux 容器虚拟化不是模拟一个完整的操作系统，而是对进程进行隔离。有了容器，就可以将软件运行所需资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的库资源和设置。

  

  ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200525140222.png)

  

比较 Docker 和传统虚拟化方式的不同之处：

- 传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；
- 而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。

- 每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源。

**Docker的优势**

1. 更快速的应用交付和部署

2. 更便捷的升级和扩缩容

3. 更简单的系统运维

4. 更高效的计算资源利用

# 安装

**docker三要素**

镜像(Image)

- Docker 镜像（Image）就是一个只读的模板。镜像可以用来创建 Docker 容器，一个镜像可以创建很多容器。

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200525141456.png)

容器(Container)

- Docker 利用容器（Container）独立运行一个或一组应用。容器是用镜像创建的运行实例。

- 它可以被启动、开始、停止、删除。每个容器都是相互隔离的、保证安全的平台。

- 可以把容器看做是一个简易版的 Linux 环境（包括root用户权限、进程空间、用户空间和网络空间等）和运行在其中的应用程序。

容器的定义和镜像几乎一模一样，也是一堆层的统一视角，唯一区别在于容器的最上面那一层是可读可写的。

仓库（Repository）

- 仓库（Repository）是集中存放镜像文件的场所。

- 仓库(Repository)和仓库注册服务器（Registry）是有区别的。仓库注册服务器上往往存放着多个仓库，每个仓库中又包含了多个镜像，每个镜像有不同的标签（tag）。

仓库分为公开仓库（Public）和私有仓库（Private）两种形式。 



>需要正确的理解仓库/镜像/容器这几个概念:
>
>Docker 本身是一个容器运行载体。把应用程序和配置依赖打包好形成一个可交付的运行环境，即 image镜像文件。只有通过这个镜像文件才能生成容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。
>
>- image 文件生成容器实例，称为镜像文件。
>- 一个容器运行一种服务，当我们需要的时候，就可以通过docker客户端创建一个对应的运行实例，也就是我们的容器
>- 至于仓库，就是放了一堆镜像的地方，我们可以把镜像发布到仓储中，需要的时候从仓储中拉下来就可以了。

Docker 简易流程图

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200525141906.png)

**脚本安装**

```shell
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

**启动docker**

```shell
systemctl start docker
```

**docker加速**

以阿里云为例,首先注册一个阿里云账号,然后进入镜像加速页面

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200525145414.png)

配置本机的 Docker 文件

如果没有`/etc/docker/daemon.json`就自己创建

```shell
sudo vim `/etc/docker/daemon.json`
```

重启 Docker 服务

```shell
systemctl daemon-reload
systemctl restart docker
```

**运行容器**

直接通过 `docker run hello-world` 命令，便可运行容器，先从本地查找后，再去远程仓库查找。

docker run 命令运行流程图

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200525145919.png)

# 常用命令

## 帮助命令

docker version

docker info

docker --help(-h)

## 镜像命令

1. docker images 		列出本地主机上的镜像

   ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200525152014.png)

   - REPOSITORY：表示镜像的仓库源

   - TAG：镜像的标签

   - IMAGE ID：镜像ID

   - CREATED：镜像创建时间

   - SIZE：镜像大小


   * images 命令 的 option 参数

     * -a:列出本地所有的镜像（含中间映像层）

     * -q :只显示镜像ID

     * --digests :显示镜像的摘要信息

     * --no-trunc :显示完整的镜像信息

2. docker search 镜像名      从 Docker hub 官网搜索镜像

   * 参数说明

     * -s : 列出收藏数不小于指定值的镜像

     * --no-trunc : 显示完整的镜像描述

     * --automated : 只列出 automated build类型的镜像

3. docker pull 镜像名:tag     拉取镜像

   - 如果 `docker pull 镜像名` 后面不加参数,默认下载最新版本

4. docker rmi 镜像名/镜像id          删除 Docker 镜像

   - `docker rmi 镜像名` 默认会删除标签为 :latest 的镜像，如果要删除指定标签的镜像,在镜像名后面指定 tag 即可，若无法删除，可以使用 -f 强制删除。

   - 删除多个镜像：docker rmi -f 镜像名1:tag 镜像名2:tag
   - 删除全部镜像：docker rmi -f $(docker images -qa)

   



## 容器命令

## 常用命令总结

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200526095411.png)

```
attach    Attach to a running container                 # 当前 shell 下 attach 连接指定运行镜像

build     Build an image from a Dockerfile              # 通过 Dockerfile 定制镜像

commit    Create a new image from a container changes   # 提交当前容器为新的镜像

cp        Copy files/folders from the containers filesystem to the host path   #从容器中拷贝指定文件或者目录到宿主机中

create    Create a new container                        # 创建一个新的容器，同 run，但不启动容器

diff      Inspect changes on a container's filesystem   # 查看 docker 容器变化

events    Get real time events from the server          # 从 docker 服务获取容器实时事件

exec      Run a command in an existing container        # 在已存在的容器上运行命令

export    Stream the contents of a container as a tar archive   # 导出容器的内容流作为一个 tar 归档文件[对应 import ]

history   Show the history of an image                  # 展示一个镜像形成历史

images    List images                                   # 列出系统当前镜像

import    Create a new filesystem image from the contents of a tarball # 从tar包中的内容创建一个新的文件系统映像[对应export]

info      Display system-wide information               # 显示系统相关信息

inspect   Return low-level information on a container   # 查看容器详细信息

kill      Kill a running container                      # kill 指定 docker 容器

load      Load an image from a tar archive              # 从一个 tar 包中加载一个镜像[对应 save]

login     Register or Login to the docker registry server    # 注册或者登陆一个 docker 源服务器

logout    Log out from a Docker registry server          # 从当前 Docker registry 退出

logs      Fetch the logs of a container                 # 输出当前容器日志信息

port      Lookup the public-facing port which is NAT-ed to PRIVATE_PORT    # 查看映射端口对应的容器内部源端口

pause     Pause all processes within a container        # 暂停容器

ps        List containers                               # 列出容器列表

pull      Pull an image or a repository from the docker registry server   # 从docker镜像源服务器拉取指定镜像或者库镜像

push      Push an image or a repository to the docker registry server    # 推送指定镜像或者库镜像至docker源服务器

restart   Restart a running container                   # 重启运行的容器

rm        Remove one or more containers                 # 移除一个或者多个容器

rmi       Remove one or more images             # 移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除]

run       Run a command in a new container              # 创建一个新的容器并运行一个命令

save      Save an image to a tar archive                # 保存一个镜像为一个 tar 包[对应 

load]

search    Search for an image on the Docker Hub         # 在 docker hub 中搜索镜像

start     Start a stopped containers                    # 启动容器

stop      Stop a running containers                     # 停止容器

tag       Tag an image into a repository                # 给源中镜像打标签

top       Lookup the running processes of a container   # 查看容器中运行的进程信息

unpause   Unpause a paused container                    # 取消暂停容器

version   Show the docker version information           # 查看 docker 版本号

wait      Block until a container stops, then print its exit code   # 截取容器停止时的退出状态值
```

# 镜像

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。

而 Docker 镜像的底层实现原理是 UnionFS 联合文件系统

**UnionFS 联合文件系统**

Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

> 特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

**为什么 Docker 镜像要采用这种分层结构呢?**

最大的一个好处就是 - `共享资源`。

所以,Docker镜像都是只读的,当容器启动时，一个新的可写层被加载到镜像的顶部。这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。

**提交镜像**

基础命令

```
docker commit 用于提交容器副本使之成为一个新的镜像
```

完整格式如下

```
docker commit -m=“提交的描述信息” -a=“作者” 容器ID 要创建的目标镜像名:[标签名]
```

# 数据卷

需求:

- Docker 可以将运行的环境打包形成容器运行，但是我们对 Docker 容器的数据的要求希望是持久化的
- 容器之间希望共享数据

Docker容器产生的数据，如果不通过docker commit生成新的镜像，使得数据做为镜像的一部分保存下来， 那么当容器删除后，数据自然也就没有了。

为了能保存数据在docker中我们使用数据卷。

**用处**

数据卷就是目录或文件，存在于一个或多个容器中，由docker挂载到容器，但不属于联合文件系统，因此能够绕过Union File System提供一些用于持续存储或共享数据的特性：

数据卷的设计目的就是数据的持久化，完全独立于容器的生存周期，因此Docker不会在容器删除时删除其挂载的数据卷

特点：

1. 数据卷可在容器之间共享或重用数据
2. 卷中的更改可以直接生效
3. 数据卷中的更改不会包含在镜像的更新中
4. 数据卷的生命周期一直持续到没有容器使用它为止

**使用**

**一、直接通过命令添加数据卷**

```
docker run -it -v /宿主机绝对路径目录:/容器内目录 镜像名
```

即便容器关闭,宿主机依然可以对数据卷进行数据操作,当容器重新开启时,数据卷会自动进行同步

如果想要设置权限,例如容器只能对数据卷进行读和同步,宿主机可以操作数据卷,那么只需要添加一个参数即可

```
docker run -it -v /宿主机绝对路径目录:/容器内目录:ro 镜像名
```

`ro` 就表示 read-only 权限(针对容器)

**二、DockerFile添加数据卷**

DockerFile 简单来说,就是描述 Docker 镜像的描述文件

流程简单梳理如下

1. 宿主机新建一个 DockerFile,可在Dockerfile中使用VOLUME指令来给镜像添加一个或多个数据卷。**由于宿主机目录是依赖于特定宿主机的，并不能够保证在所有的宿主机上都存在这样的特定目录**。

2. build 构建镜像

   ```
   docker build -f dockerfile文件名 -t 镜像名:标签
   ```

3. 运行镜像用inspect查看

**容器与容器间共享**

主要用于容器和容器之间的数据共享

命令: --volumes-from

容器之间共享数据的传递，数据卷的生命周期一直持续到没有容器使用它为止

# DockerFile

Dockerfile是用来构建Docker镜像的构建文件，是由一系列命令和参数构成的脚本。

通常使用 DockerFile 的三个步骤都是:

1. 编写 DockerFile 文件
2. 执行 docker build 编译命令
3. 执行docker run 启动容器命令

以 CentOS 为例, Docker Hub 上的 CentOS 的 DockerFile 文件如下

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200526132103.png)

## 基础

原则：

1. 每条保留字指令(红色字体)都必须为大写字母且后面要跟随至少一个参数
2. 指令按照从上到下，顺序执行
3. \#表示注释
4. 每条指令都会创建一个新的镜像层，并对镜像进行提交

Dockerfile：Dockerfile定义了进程需要的一切东西。Dockerfile涉及的内容包括执行代码或者是文件、环境变量、依赖包、运行时环境、动态链接库、操作系统的发行版、服务进程和内核进程等;

Docker镜像：在用Dockerfile定义一个文件之后，docker build时会产生一个Docker镜像，当运行 Docker镜像时，会真正开始提供服务;

Docker容器：直接提供服务。

## 保留字

- FROM : 基础镜像，当前新镜像是基于哪个镜像的

- MAINTAINER : 镜像维护者的姓名和邮箱地址

- RUN : 容器构建时需要运行的命令

- EXPOSE : 当前容器对外暴露出的端口

- WORKDIR : 指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点

- ENV : 用来在构建镜像过程中设置环境变量

- ADD : 将宿主机目录下的文件拷贝进镜像且ADD命令会自动处理URL和解压tar压缩包

- COPY : 类似ADD，拷贝文件和目录到镜像中。 将从构建上下文目录中 <源路径> 的文件/目录复制到新的一层的镜像内的 <目标路径> 位置

- VOLUME : 容器数据卷，用于数据保存和持久化工作

- CMD : 指定一个容器启动时要运行的命令

  ![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200526132533.png)

  

  注意: Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效，CMD 会被 docker run 之后的参数替换

- ENTRYPOINT : 指定一个容器启动时要运行的命令;ENTRYPOINT 的目的和 CMD 一样，都是在指定容器启动程序及参数,但是不会被 docker run 后面的参数替换,而是追加

- ONBUILD : 当构建一个被继承的Dockerfile时运行命令，父镜像在被子继承后父镜像的onbuild被触发

总结:

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200526132641.png)

例子

```dockerfile
FROM         centos
MAINTAINER    krislin_zhao
#把宿主机当前上下文的c.txt拷贝到容器/usr/local/路径下
COPY copy.txt /usr/local/cincontainer.txt
#把java与tomcat添加到容器中
ADD jdk-8u171-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.35.tar.gz /usr/local/
#安装vim编辑器
RUN yum -y install vim
#设置工作访问时候的WORKDIR路径，登录落脚点
ENV MYPATH /usr/local
WORKDIR $MYPATH
#配置java与tomcat环境变量
ENV JAVA_HOME /usr/local/jdk1.8.0_171
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.35
ENV CATALINA_BASE /usr/local/apache-tomcat-9.0.35
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
#容器运行时监听的端口
EXPOSE  8080
#启动时运行tomcat
# ENTRYPOINT ["/usr/local/apache-tomcat-9.0.35/bin/startup.sh" ]
# CMD ["/usr/local/apache-tomcat-9.0.35/bin/catalina.sh","run"]
CMD /usr/local/apache-tomcat-9.0.35/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.35/bin/logs/catalina.out
```

