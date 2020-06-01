# 基础概念

## 镜像

&nbsp; &nbsp; &nbsp; &nbsp;我们都知道，操作系统分为内核和用户空间。对于 Linux 而言，内核启动后，会挂载 root 文件系统为其提供用户空间支持。而 Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:18.04 就包含了完整的一套 Ubuntu 18.04 最小系统的 root 文件系统。

&nbsp; &nbsp; &nbsp; &nbsp;Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。

&nbsp; &nbsp; &nbsp; &nbsp;因为镜像包含操作系统完整的 root 文件系统，其体积往往是庞大的，因此在 Docker 设计时，就充分利用 Union FS 的技术，将其设计为分层存储的架构。所以严格来说，镜像并非是像一个 ISO 那样的打包文件，镜像只是一个虚拟的概念，其实际体现并非由一个文件组成，而是由一组文件系统组成，或者说，由多层文件系统联合组成。

&nbsp; &nbsp; &nbsp; &nbsp;镜像构建时，会一层层构建，前一层是后一层的基础。每一层构建完就不会再发生改变，后一层上的任何改变只发生在自己这一层。比如，删除前一层文件的操作，实际不是真的删除前一层的文件，而是仅在当前层标记为该文件已删除。在最终容器运行的时候，虽然不会看到这个文件，但是实际上该文件会一直跟随镜像。因此，在构建镜像的时候，需要额外小心，每一层尽量只包含该层需要添加的东西，任何额外的东西应该在该层构建结束前清理掉。

&nbsp; &nbsp; &nbsp; &nbsp;分层存储的特征还使得镜像的复用、定制变的更为容易。甚至可以用之前构建好的镜像作为基础层，然后进一步添加新的层，以定制自己所需的内容，构建新的镜像。

## 容器

&nbsp; &nbsp; &nbsp; &nbsp;镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

&nbsp; &nbsp; &nbsp; &nbsp;容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的 命名空间。因此容器可以拥有自己的 root 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。容器内的进程是运行在一个隔离的环境里，使用起来，就好像是在一个独立于宿主的系统下操作一样。这种特性使得容器封装的应用比直接在宿主运行更加安全。也因为这种隔离的特性，很多人初学 Docker 时常常会混淆容器和虚拟机。

&nbsp; &nbsp; &nbsp; &nbsp;前面讲过镜像使用的是分层存储，容器也是如此。每一个容器运行时，是以镜像为基础层，在其上创建一个当前容器的存储层，我们可以称这个为容器运行时读写而准备的存储层为 容器存储层。

&nbsp; &nbsp; &nbsp; &nbsp;容器存储层的生存周期和容器一样，容器消亡时，容器存储层也随之消亡。因此，任何保存于容器存储层的信息都会随容器删除而丢失。

&nbsp; &nbsp; &nbsp; &nbsp;按照 Docker 最佳实践的要求，容器不应该向其存储层内写入任何数据，容器存储层要保持无状态化。所有的文件写入操作，都应该使用 数据卷（Volume）、或者绑定宿主目录，在这些位置的读写会跳过容器存储层，直接对宿主（或网络存储）发生读写，其性能和稳定性更高。

&nbsp; &nbsp; &nbsp; &nbsp;数据卷的生存周期独立于容器，容器消亡，数据卷不会消亡。因此，使用数据卷后，容器删除或者重新运行之后，数据却不会丢失。

## 仓库
&nbsp; &nbsp; &nbsp; &nbsp;镜像构建完成后，可以很容易的在当前宿主机上运行，但是，如果需要在其它服务器上使用这个镜像，我们就需要一个集中的存储、分发镜像的服务，Docker Registry 就是这样的服务。

&nbsp; &nbsp; &nbsp; &nbsp;一个 Docker Registry 中可以包含多个 仓库（Repository）；每个仓库可以包含多个 标签（Tag）；每个标签对应一个镜像。

&nbsp; &nbsp; &nbsp; &nbsp;通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本。我们可以通过 <仓库名>:<标签> 的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 latest 作为默认标签。

&nbsp; &nbsp; &nbsp; &nbsp;以 Ubuntu 镜像 为例，ubuntu 是仓库的名字，其内包含有不同的版本标签，如，16.04, 18.04。我们可以通过 ubuntu:16.04，或者 ubuntu:18.04 来具体指定所需哪个版本的镜像。如果忽略了标签，比如 ubuntu，那将视为 ubuntu:latest。

&nbsp; &nbsp; &nbsp; &nbsp;仓库名经常以 两段式路径 形式出现，比如 jwilder/nginx-proxy，前者往往意味着 Docker Registry 多用户环境下的用户名，后者则往往是对应的软件名。但这并非绝对，取决于所使用的具体 Docker Registry 的软件或服务。## docker cp
```
[root@centos7 ~]# docker cp /root/sshd.service c1d4b2aa19d3:/data/ddd
[root@centos7 ~]# docker exec -t -i c1d4b2aa19d3 bash
root@c1d4b2aa19d3:/data# ls
ddd
```

# 安装卸载

### 先决条件
!> Docker cannot run correctly if your kernel is older than version 3.10 or if it is missing some modules

&nbsp; &nbsp; &nbsp; &nbsp;To install Docker Engine - Community, you need a maintained version of CentOS 7. Archived versions aren’t supported or tested.

&nbsp; &nbsp; &nbsp; &nbsp;The centos-extras repository must be enabled. This repository is enabled by default, but if you have disabled it, you need to re-enable it.

&nbsp; &nbsp; &nbsp; &nbsp;The overlay2 storage driver is recommended.

### yum 安装

!> yum-utils provides the yum-config-manager utility

```
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
$ sudo yum-config-manager \
  --add-repo \
  https://download.docker.com/linux/centos/docker-ce.repo
$ sudo yum install docker-ce docker-ce-cli containerd.io
$ sudo systemctl start docker
$ sudo docker run hello-world
```

### rpm 安装
?> Go to https://download.docker.com/linux/centos/7/x86_64/stable/Packages/ and download the .rpm file
```
$ curl -O https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm   
$ curl -O https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-19.03.4-3.el7.x86_64.rpm  
$ curl -O https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-cli-19.03.4-3.el7.x86_64.rpm 
$ sudo yum install /path/to/package.rpm
$ sudo systemctl start docker
$ sudo docker run hello-world
```
### scripts 安装
!> Using these scripts is not recommended for production environments
```
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
```
### 镜像加速
```
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://zzftire8.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
```
## 卸载

!> Older versions of Docker were called docker or docker-engine
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

!> The Docker Engine - Community package is now called docker-ce

```
$ sudo yum remove docker-ce
Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

$ sudo rm -rf /var/lib/docker
You must delete any edited configuration files manually.
```


# 常用命令

## docker pull
```
[root@centos7 docker]# docker pull redis
Using default tag: latest
latest: Pulling from library/redis
Digest: sha256:fe80393a67c7058590ca6b6903f64e35b50fa411b0496f604a85c526fb5bd2d2
Status: Image is up to date for redis:latest
docker.io/library/redis:latest
[root@centos7 docker]# docker image ls
redis               latest              de25a81a5a0b        12 days ago         98.2MB
```

## docker run

```
[root@centos7 ~]# docker run ubuntu:latest /bin/echo 'Hello world'
Hello world
```

### -t -i
进入交互式`shell`

> -t              : Allocate a pseudo-tty

> -i              : Keep STDIN open even if not attached

```
[root@centos7 ~]# docker run -t -i ubuntu:latest /bin/bash
[root@centos7 ~]# docker exec -t -i 9d9687eb691c bash
```

### --name
```
[root@centos7 ~]# docker run -d  --name new_container1 -i -t ubuntu bash
82c270d1489129b950fb2b0875cdc6283f96fb6fa672fcf5d18f4d591fe9176c
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
82c270d14891        ubuntu              "bash"              5 seconds ago       Up 4 seconds                            new_container1
```

### --restart=always
!> 即开机自启动
```
[root@centos7 ~]# docker update --restart=always 7c369408716f d2f6179c97d9 2e2ceb8f19b3 28192ea1c749 04296bb5d928 395ab7429a51 2697588d9510 92753fceeecf
7c369408716f
d2f6179c97d9
2e2ceb8f19b3
28192ea1c749
04296bb5d928
395ab7429a51
2697588d9510
92753fceeecf
```


### --env , -e
```
[root@centos7 ~]# docker run -d --env qiumin=ls -i -t ubuntu bash
9d9687eb691c6c130442be681804236bd8ab6545af6a6a6d0cf6037294eeb250
[root@centos7 ~]# docker exec -t -i 9d9687eb691c bash
root@9d9687eb691c:/# echo $qiumin
ls
```

### --detach , -d
后台运行

```
$ docker run -d -p 80:80 my_image nginx -g 'daemon off;'
```

### --publish , -p
发布一个端口映射

```
 qiumin@QiuMin-MacBook-Pro  ~  docker run -d -p 127.0.0.1:8000:8080/tcp qiumin:2.0
eb45981a385052134980eff589e9791bf53cf6288e83ef3db5c497337f713263
 qiumin@QiuMin-MacBook-Pro  ~ 
 qiumin@QiuMin-MacBook-Pro  ~  docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                      NAMES
eb45981a3850        qiumin:2.0          "npm start"         3 seconds ago       Up 3 seconds        127.0.0.1:8000->8080/tcp   happy_mclean
 qiumin@QiuMin-MacBook-Pro  ~ 
```


### --volume , -v
挂载本地磁盘
```
[root@centos7 ~]# docker run -v /root/myproject/:/newqiumin -i -t ubuntu bash
root@c3eb81904c36:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  newqiumin  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@c3eb81904c36:/# ll|grep newqiumin
drwxr-xr-x.   4 root root   71 Oct 25 05:34 newqiumin/
root@c3eb81904c36:/#
```
挂载卷
```
[root@centos7 ~]# docker volume ls
DRIVER              VOLUME NAME
local               qiu
[root@centos7 ~]# docker run -v qiu:/newqiumin2 -i -t ubuntu bash
root@261ce954446e:/#
root@261ce954446e:/#
root@261ce954446e:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  newqiumin2  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

### --hostname , -h
```
[root@centos7 ~]# docker run  --hostname qiumin -i -t ubuntu bash
root@qiumin:/#
root@qiumin:/# hostname
qiumin
```

### --network
```
[root@centos7 ~]# docker run -d -t -i --network my-net -p 6380:6379 redis redis-server
f8518e1aee43e2cacbc9239e58028e89eb16290ad234421832ef4933c3f6f28d
```

## docker stop

- 使用`docker stop`

```
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c5e676ca4dac        ubuntu:latest       "/bin/bash"         26 minutes ago      Up 26 minutes                           awesome_ganguly
[root@centos7 ~]# docker stop c5e676ca4dac
c5e676ca4dac
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@centos7 ~]#
```

- 使用`docker container stop`

```
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
aa14c6139f2d        ubuntu:latest       "/bin/bash"         6 seconds ago       Up 4 seconds                            focused_payne
[root@centos7 ~]# docker container stop aa14c6139f2d
aa14c6139f2d
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@centos7 ~]#
```

## docker attach

!> 使用`Ctrl+q`进入容器并且不退出，如果按`Exit`则会停止容器

```
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
54dfe06163f4        ubuntu:latest       "/bin/bash"         7 seconds ago       Up 6 seconds                            flamboyant_khorana
93e4ebae2155        ubuntu:latest       "/bin/bash"         7 minutes ago       Up 7 minutes                            amazing_brown
[root@centos7 ~]# docker attach 54dfe06163f4
root@54dfe06163f4:/#
root@54dfe06163f4:/#
root@54dfe06163f4:/# read escape sequence
[root@centos7 ~]#
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
54dfe06163f4        ubuntu:latest       "/bin/bash"         34 seconds ago      Up 33 seconds                           flamboyant_khorana
93e4ebae2155        ubuntu:latest       "/bin/bash"         7 minutes ago       Up 7 minutes                            amazing_brown
[root@centos7 ~]#
```

## docker exec

?> 与`docker attach`相比，`Exit`不会自动停止容器

```
[root@centos7 ~]# docker exec -i -t 54dfe06163f4 bash
root@54dfe06163f4:/#
root@54dfe06163f4:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@54dfe06163f4:/# exit
exit
[root@centos7 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
54dfe06163f4        ubuntu:latest       "/bin/bash"         5 minutes ago       Up 5 minutes                            flamboyant_khorana
93e4ebae2155        ubuntu:latest       "/bin/bash"         12 minutes ago      Up 12 minutes                           amazing_brown
[root@centos7 ~]#
```

## docker rm

```
[root@centos7 ~]# docker container ls -a|grep 9d9687eb691c
9d9687eb691c        ubuntu              "bash"                   11 minutes ago      Exited (0) 13 seconds ago                          sharp_shockley
[root@centos7 ~]# docker rm 9d9687eb691c
9d9687eb691c
[root@centos7 ~]# docker container ls -a|grep 9d9687eb691c

```

## docker build
### --tag , -t
指定[标签:版本号]
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/node-bulletin-board/bulletin-board-app   v1  docker build -t qiumin:3.0 .
Sending build context to Docker daemon  46.59kB
Step 1/6 : FROM node:6.11.5
 ---> 852391892b9f
Step 2/6 : WORKDIR /usr/src/app
 ---> Using cache
 ---> a0bf0c06bba1
Step 3/6 : COPY package.json .
 ---> Using cache
 ---> 0f36b154317d
Step 4/6 : RUN npm install
 ---> Using cache
 ---> 74b3ed12b25a
Step 5/6 : COPY . .
 ---> Using cache
 ---> 0912fb0ba247
Step 6/6 : CMD [ "npm", "start" ]
 ---> Using cache
 ---> 806390bae189
Successfully built 806390bae189
Successfully tagged qiumin:3.0
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/node-bulletin-board/bulletin-board-app   v1  docker image ls
REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
qiumin                               3.0                 806390bae189        23 hours ago        681MB

```
### --file , -f
指定文件路径
```
[root@centos7 bulletin-board-app]# ll Dockerfile
-rw-r--r--. 1 root root 109 Oct 25 17:40 Dockerfile
[root@centos7 bulletin-board-app]#  docker build -f Dockerfile .
Sending build context to Docker daemon  45.57kB
Step 1/6 : FROM node:6.11.5
 ---> 852391892b9f
Step 2/6 : WORKDIR /usr/src/app
 ---> Running in 366f98d3831b
Removing intermediate container 366f98d3831b
 ---> 3a68380215c0
Step 3/6 : COPY package.json .
 ---> 9fa26871c042
Step 4/6 : RUN npm install
 ---> Running in f923e003fa05
vue-event-bulletin@1.0.0 /usr/src/app
+-- body-parser@1.19.0
| +-- bytes@3.1.0
| +-- content-type@1.0.4
| +-- debug@2.6.9
| | `-- ms@2.0.0
| +-- depd@1.1.2
| +-- http-errors@1.7.2
| | +-- inherits@2.0.3
| | `-- toidentifier@1.0.0
| +-- iconv-lite@0.4.24
| | `-- safer-buffer@2.1.2
| +-- on-finished@2.3.0
| | `-- ee-first@1.1.1
| +-- qs@6.7.0
| +-- raw-body@2.4.0
| | `-- unpipe@1.0.0
| `-- type-is@1.6.18
|   +-- media-typer@0.3.0
|   `-- mime-types@2.1.24
|     `-- mime-db@1.40.0
+-- bootstrap@3.4.1
+-- ejs@2.7.1
+-- errorhandler@1.5.1
| +-- accepts@1.3.7
| | `-- negotiator@0.6.2
| `-- escape-html@1.0.3
+-- express@4.17.1
| +-- array-flatten@1.1.1
| +-- content-disposition@0.5.3
| +-- cookie@0.4.0
| +-- cookie-signature@1.0.6
| +-- encodeurl@1.0.2
| +-- etag@1.8.1
| +-- finalhandler@1.1.2
| +-- fresh@0.5.2
| +-- merge-descriptors@1.0.1
| +-- methods@1.1.2
| +-- parseurl@1.3.3
| +-- path-to-regexp@0.1.7
| +-- proxy-addr@2.0.5
| | +-- forwarded@0.1.2
| | `-- ipaddr.js@1.9.0
| +-- range-parser@1.2.1
| +-- safe-buffer@5.1.2
| +-- send@0.17.1
| | +-- destroy@1.0.4
| | +-- mime@1.6.0
| | `-- ms@2.1.1
| +-- serve-static@1.14.1
| +-- setprototypeof@1.1.1
| +-- statuses@1.5.0
| +-- utils-merge@1.0.1
| `-- vary@1.1.2
+-- method-override@2.3.10
+-- morgan@1.9.1
| +-- basic-auth@2.0.1
| `-- on-headers@1.0.2
+-- vue@1.0.28
| `-- envify@3.4.1
|   +-- jstransform@11.0.3
|   | +-- base62@1.2.8
|   | +-- commoner@0.10.8
|   | | +-- commander@2.20.3
|   | | +-- detective@4.7.1
|   | | | +-- acorn@5.7.3
|   | | | `-- defined@1.0.0
|   | | +-- glob@5.0.15
|   | | | +-- inflight@1.0.6
|   | | | | `-- wrappy@1.0.2
|   | | | +-- minimatch@3.0.4
|   | | | | `-- brace-expansion@1.1.11
|   | | | |   +-- balanced-match@1.0.0
|   | | | |   `-- concat-map@0.0.1
|   | | | +-- once@1.4.0
|   | | | `-- path-is-absolute@1.0.1
|   | | +-- graceful-fs@4.2.3
|   | | +-- mkdirp@0.5.1
|   | | | `-- minimist@0.0.8
|   | | +-- private@0.1.8
|   | | +-- q@1.5.1
|   | | `-- recast@0.11.23
|   | |   +-- ast-types@0.9.6
|   | |   +-- esprima@3.1.3
|   | |   `-- source-map@0.5.7
|   | +-- esprima-fb@15001.1.0-dev-harmony-fb
|   | +-- object-assign@2.1.1
|   | `-- source-map@0.4.4
|   |   `-- amdefine@1.0.1
|   `-- through@2.3.8
`-- vue-resource@0.1.17

npm WARN vue-event-bulletin@1.0.0 No repository field.
Removing intermediate container f923e003fa05
 ---> d48a04bd2790
Step 5/6 : COPY . .
 ---> 6dddbe1db65e
Step 6/6 : CMD [ "npm", "start" ]
 ---> Running in e0a26671fa33
Removing intermediate container e0a26671fa33
 ---> c0fa3542e73f
Successfully built c0fa3542e73f
[root@centos7 bulletin-board-app]# docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              c0fa3542e73f        7 seconds ago       681MB
```
### - <<EOF
```
[root@centos7 bulletin-board-app]#  docker build - <<EOF
> FROM busybox
> RUN echo "qiumin"
> EOF
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM busybox
 ---> 19485c79a9bb
Step 2/2 : RUN echo "qiumin"
 ---> Running in 80554921be40
qiumin
Removing intermediate container 80554921be40
 ---> 521e38b77780
Successfully built 521e38b77780
[root@centos7 bulletin-board-app]# docker imagels
docker: 'imagels' is not a docker command.
See 'docker --help'
[root@centos7 bulletin-board-app]# docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              521e38b77780        12 seconds ago      1.22MB
```

## docker diff

Symbol|Description
-|-
A|A file or directory was added
D|A file or directory was deleted
C|A file or directory was changed

```
[root@centos7 bulletin-board-app]# docker exec -i -t b6c163c4efe7 bash
root@b6c163c4efe7:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@b6c163c4efe7:/# touch abc
root@b6c163c4efe7:/# exit
exit
[root@centos7 bulletin-board-app]# docker diff b6c163c4efe7
A /abc
C /root
A /root/.bash_history
```

## docker logs
```
[root@centos7 ~]#  docker logs 4b5eb8aeb6d2 -f
1:C 29 Oct 2019 07:50:06.096 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 29 Oct 2019 07:50:06.096 # Redis version=5.0.6, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 29 Oct 2019 07:50:06.096 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
```

## 导入导出镜像
- docker export/import

!> 比如我们从一个 ubuntu 镜像启动一个容器，然后安装一些软件和进行一些设置后，使用 docker export 保存为一个基础镜像。然后，把这个镜像分发给其他人使用，将其作为基础的开发环境。

```
[root@centos7 elastic]# docker export 2e2ceb8f19b3 >abc.tar
[root@centos7 elastic]# docker import - newqiu<abc.tar
sha256:860dfa86a6c58ef82a293daac985306cba40c97069916ccd8ee6d78abbf969d1
[root@centos7 elastic]# docker image ls
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
newqiu                       latest                860dfa86a6c5        4 seconds ago       597MB
```
- docker save/load

!> 如果我们的应用是使用 docker-compose.yml 编排的多个镜像组合，但我们要部署的客户服务器并不能连外网。这时就可以使用 docker save 将用到的镜像打个包，然后拷贝到客户服务器上使用 docker load 载入。

```
[root@centos7 elastic]# docker image ls
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
newqiu                       latest                860dfa86a6c5        41 seconds ago      597MB
[root@centos7 elastic]# docker save 860dfa86a6c5 > abcd.tar
[root@centos7 elastic]# docker image rm 860dfa86a6c5
Untagged: newqiu:latest
Deleted: sha256:860dfa86a6c58ef82a293daac985306cba40c97069916ccd8ee6d78abbf969d1
[root@centos7 elastic]# docker load < abcd.tar
116683e53642: Loading layer [==================================================>]  611.4MB/611.4MB
Loaded image ID: sha256:860dfa86a6c58ef82a293daac985306cba40c97069916ccd8ee6d78abbf969d1
[root@centos7 elastic]# docker image ls
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
<none>                       <none>                860dfa86a6c5        3 minutes ago       597MB
[root@centos7 elastic]# docker image tag 860dfa86a6c5 qiu111:222
[root@centos7 elastic]# docker image ls
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
qiu111                       222                   860dfa86a6c5        9 minutes ago       597MB
```## Tomcat
[官方镜像](https://hub.daocloud.io/repos/47f127d0-8f1d-4f91-9647-739cf3146a04)

!> 使用标签`latest`下载的也是`tomcat:8.5.47`

```shell
[root@centos7 ~]# docker pull tomcat:8.5.47-jdk8-openjdk
[root@centos7 ~]# docker run -i -t -p 80:8080 tomcat:latest
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/openjdk-8
```

?> 需将`jar`包`mount`到`webapp`目录,使用`docker logs -f`看日志
```
[root@centos7 ~]# docker run -v /root/docker/portal/webapps:/usr/local/tomcat/webapps -i -t -p 80:8080 tomcat:latest
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/openjdk-8
```

# 组件部署

## Mysql
[官方镜像](https://hub.docker.com/_/mysql)

!> 注意官方镜像是`mysql8.0`版本的,需要`pull 5.7.28`版本

```
[root@centos7 conf]# docker pull mysql:5.7.28
```
### without my.cnf
```
[root@centos7 mysql]#  docker run -e MYSQL_ROOT_PASSWORD=Meizu123 -d  -p 3306:3306 mysql:5.7.28
[root@centos7 mysql]#  docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
79a5509546e7        mysql:5.7.28               "docker-entrypoint.s…"   5 seconds ago       Up 3 seconds        0.0.0.0:3306->3306/tcp, 33060/tcp   kind_rosalind
```
### with my.cnf
```
[root@centos7 mysql]# docker run -v  /root/docker/mysql/conf/abc.conf:/etc/mysql/conf.d/abc.cnf  -v /root/docker/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=Meizu123 -d -p 3306:3306 mysql:5.7.28
79a5509546e78bf51a89b0143943ba44b36dbfcb171dd0eb77777847217a9c7e
[root@centos7 mysql]#  docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
79a5509546e7        mysql:5.7.28               "docker-entrypoint.s…"   5 seconds ago       Up 3 seconds        0.0.0.0:3306->3306/tcp, 33060/tcp   kind_rosalind
```
### my.cnf
```
[root@centos7 conf]# cat abc.conf
[client]
default-character-set=utf8mb4
[mysql]
default-character-set=utf8mb4
[mysqld]
########basic settings########
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
init_connect='SET NAMES utf8mb4'
server-id = 1
#port = 3306
#user = mysql
#bind_address = 0.0.0.0
character_set_server=utf8mb4
skip_name_resolve = 1
max_connections = 1000
max_connect_errors = 1000
transaction_isolation = READ-COMMITTED
explicit_defaults_for_timestamp = 1
join_buffer_size = 134217728
tmp_table_size = 67108864
tmpdir = /tmp
max_allowed_packet = 16777216
sql_mode = "STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER"
interactive_timeout = 1800
wait_timeout = 1800
read_buffer_size = 16777216
read_rnd_buffer_size = 33554432
sort_buffer_size = 33554432
lower_case_table_names = 1
######log settings########
log_error = error.log
slow_query_log = 1
slow_query_log_file = slow.log
log_queries_not_using_indexes = 1
log_slow_admin_statements = 1
log_slow_slave_statements = 1
log_throttle_queries_not_using_indexes = 10
expire_logs_days = 90
long_query_time = 2
min_examined_row_limit = 100
########replication settings########
master_info_repository = TABLE
relay_log_info_repository = TABLE
log_bin = bin.log
sync_binlog = 1
gtid_mode = on
enforce_gtid_consistency = 1
log_slave_updates
binlog_format = row
relay_log = relay.log
relay_log_recovery = 1
binlog_gtid_simple_recovery = 1
slave_skip_errors = ddl_exist_errors
########innodb settings########
innodb_page_size = 8192
innodb_buffer_pool_size = 6G
innodb_buffer_pool_instances = 8
innodb_buffer_pool_load_at_startup = 1
innodb_buffer_pool_dump_at_shutdown = 1
innodb_lru_scan_depth = 2000
innodb_lock_wait_timeout = 5
innodb_io_capacity = 4000
innodb_io_capacity_max = 8000
innodb_flush_method = O_DIRECT
innodb_file_format = Barracuda
innodb_file_format_max = Barracuda
innodb_undo_logs = 128
innodb_undo_tablespaces = 3
innodb_flush_neighbors = 1
innodb_log_file_size = 4G
innodb_log_buffer_size = 16777216
innodb_purge_threads = 4
innodb_large_prefix = 1
innodb_thread_concurrency = 64
innodb_print_all_deadlocks = 1
innodb_strict_mode = 1
innodb_sort_buffer_size = 67108864
```
## Redis
[官方镜像](https://hub.daocloud.io/repos/beb958f9-ffb6-4f68-817b-c17e1ff476c3)

```
[root@centos7 docker]# docker pull redis
[root@centos7 docker]# docker run -d -t -i -p 6379:6379 redis redis-server
b835539d19741f0e0d7a0bf2354ccf2f265f174cb2e4439c461b8b7dd790d00b
[root@centos7 src]# ./redis-cli
127.0.0.1:6379> info
# Server
redis_version:5.0.6
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:24cefa6406f92a1f
redis_mode:standalone
os:Linux 3.10.0-862.el7.x86_64 x86_64
```

## Memcached
[官方镜像](https://hub.docker.com/_/memcached)
```
[root@centos7 docker]# docker pull memcached
Using default tag: latest
latest: Pulling from library/memcached
Digest: sha256:fcfed7e429eea2e533dfcbc1ce7a3cddfbe91480677aa59d8f2efeddcd216fa4
Status: Image is up to date for memcached:latest
docker.io/library/memcached:latest
[root@centos7 docker]# docker run  -d -p 11211:11211 memcached
b465b1c963a12f20d33482e5acc7639887773b9ee09facfeec9ba5791935b91b
[root@centos7 docker]# telnet 127.0.0.1 11211
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
stats
STAT pid 1
STAT uptime 155
STAT time 1572341562
STAT version 1.5.19
```

## Elasticsearch
[官方镜像](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/docker.html#docker)

!> `elasticsearch`的`6.2`开始即内置了`x-pack`,如果需要使用`x-pack`，需使用`Dockfiles`定制镜像

```
[root@centos7 ]# docker pull elasticsearch:6.8.4
[root@centos7 ~]# docker run -d -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:6.8.4
a2746bd42a7524fdec1cf1ba56e975bcb46548e03f23624b9d2c3ad71a79b16f
[root@centos7 ~]# docker container ls
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                            NAMES
a2746bd42a75        elasticsearch:6.8.4   "/usr/local/bin/dock…"   9 seconds ago       Up 7 seconds        0.0.0.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp   flamboyant_swirles
```

## Kibana
[官方镜像](https://www.elastic.co/guide/en/kibana/6.8/docker.html)

!> 如果`elasticsearch`配置了账号密码,需要修改`/usr/share/kibana/config/kibana.yml`


```
[root@centos7 ~]# docker pull kibana:6.8.4
[root@centos7 ~]# docker run -d --link a2746bd42a75:elasticsearch -p 5601:5601 kibana:6.8.4
[root@centos7 docker]# docker container ls
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                            NAMES
8d79d06dbfbc        kibana:6.8.4          "/usr/local/bin/kiba…"   16 minutes ago      Up 16 minutes       0.0.0.0:5601->5601/tcp                           upbeat_dubinsky
```

## ActiveMQ
[非官方镜像](https://hub.docker.com/r/rmohr/activemq)

!> 注意是非官方镜像，官方未提供docker安装包

```
[root@centos7 ~]# docker run -d -p 61616:61616 -p 8161:8161 rmohr/activemq
d113209c5bd927f8a50b4faa86743f76263a8ff6f0fe1ceb9ef3288e1e93fbcf
[root@centos7 ~]# docker container ls
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                                                                   NAMES
d113209c5bd9        rmohr/activemq        "/bin/sh -c 'bin/act…"   8 seconds ago       Up 7 seconds        1883/tcp, 5672/tcp, 0.0.0.0:8161->8161/tcp, 61613-61614/tcp, 0.0.0.0:61616->61616/tcp   nifty_dewdney
```



# 配置网络

## bridge
!> The default network driver
> User-defined bridge networks are best when you need multiple containers to communicate on the same Docker host.

### User-defined bridge
特点：
- Containers connected to the same user-defined bridge network automatically expose all ports to each other, and no ports to the outside world. if you run the same application stack on the default bridge network, you need to open both the web port and the database port, using the `-p` or `--publish` flag for each

- On a user-defined bridge network, containers can resolve each other by name or alias.

- To remove a container from the default bridge network, you need to stop the container and recreate it with different network options.

- If different groups of applications have different network requirements, you can configure each user-defined bridge separately, as you create it.

创建bridge
```
[root@centos7 ~]# docker network create my-net
8b7c0587c4a9523fae826f27eb2c6e0191d6c5a35f38ada5421f6988470c2003
```
使用bridge创建容器
```
[root@centos7 ~]# docker run -d -t -i --network my-net -p 6380:6379 redis redis-server
f8518e1aee43e2cacbc9239e58028e89eb16290ad234421832ef4933c3f6f28d
```
使用bridge连接已有容器
```
[root@centos7 ~]# docker container ls
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                                                                   NAMES
b7e6804e6036        redis                 "docker-entrypoint.s…"   16 seconds ago      Up 15 seconds       0.0.0.0:6381->6379/tcp

[root@centos7 ~]# docker network connect my-net b7e6804e6036
```


### Deault bridge
 Containers connected to the default bridge network can communicate, but only by IP address, unless they are linked using the legacy `--link` flag

```
[root@centos7 ~]# docker run -d --link a2746bd42a75:elasticsearch -p 5601:5601 kibana:6.8.4
```
## host
!> Use the host’s networking directly

> Host is only available for swarm services on Docker 17.06 and higher.

?> Host networks are best when the network stack should not be isolated from the Docker host, but you want other aspects of the container to be isolated.

## overlay
> You can also use overlay networks to facilitate communication between a swarm service and a standalone container, or between two standalone containers on different Docker daemons.

?> Overlay networks are best when you need containers running on different Docker hosts to communicate, or when multiple applications work together using swarm services.

## macvlan
> Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network

?> Macvlan networks are best when you are migrating from a VM setup or need your containers to look like physical hosts on your network, each with a unique MAC address.

## none
> Disable all networking. Usually used in conjunction with a custom network driver.

## Network plugins
> You can install and use third-party network plugins with Docker.
> Third-party network plugins allow you to integrate Docker with specialized network stacks## volumes

# 存储与卷

> 默认目录 `/var/lib/docker/volumes/`

> A given volume can be mounted into multiple containers simultaneously

!> However, in Docker 17.06 and higher, we recommend using the `--mount` flag.

优势：
1. Volumes are easier to back up or migrate than bind mounts.
2. You can manage volumes using Docker CLI commands or the Docker API.
3. Volumes work on both Linux and Windows containers.
4. Volumes can be more safely shared among multiple containers.
5. Volume drivers let you store volumes on remote hosts or cloud providers, to encrypt the contents of volumes, or to add other functionality.
New volumes can have their content pre-populated by a containe

语法：
```
--mount 'type= bind  or volume or tmpfs,source or src = the name of volume or /,destination or dst or target = /root/??? (readonly)' 

```

例子：
```
[root@centos7 ~]# docker run -d -t -i --mount 'src=qiumin,dst=/root/qiumin' -p 6399:6379 redis redis-server
4b5ca5f99a123f4dd918b9413e7f909864d4ab8ca06ae9b8ea7ebf7fd14d056a
```

```
[root@centos7 ~]# docker run -d -t -i --mount 'src=qiumin,dst=/root/qiumin,readonly' -p 6390:6379 redis redis-server
f23b6ef97cb29d9fce47ff492dc03bd075310971ce2958da85d8f6ba99378ba3
```

## bind mounts
> Bind mounts may be stored anywhere on the host system.

!> &nbsp; &nbsp; &nbsp; &nbsp;One side effect of using bind mounts, for better or for worse, is that you can change the host filesystem via processes running in a container, including creating, modifying, or deleting important system files or directories. 

?> If you use `-v` or `--volume` to bind-mount a file or directory that does not yet exist on the Docker host, `-v` creates the endpoint for you. It is always created as a directory.

?> If you use `--mount` to bind-mount a file or directory that does not yet exist on the Docker host, Docker does not automatically create it for you, but generates an error.

!> 为了避免混淆，建议使用`volume`的时候用`--mount`，用`bind mount`的时候用`-v`

例子：
```
[root@centos7 ~]# docker run -d -t -i -v /root/docker:/root/abc -p 6391:6379 redis redis-server
a18d3d5bd06890e0a79cd99c2d89c21bbef208c2bf7c69b51cdb31cdb92bd0eb
[root@centos7 ~]#
```

## tmpfs
> tmpfs mounts are stored in the host system’s memory only, and are never written to the host system’s filesystem.

?> This functionality is only available if you’re running Docker on Linux.

!> 为了避免混淆，建议使用`tmpfs`的时候用`--tmpfs`

例子：
```
[root@centos7 ~]# docker run -d -t -i --tmpfs /root/abc -p 6392:6379 redis redis-server
c1d4b2aa19d3d8aa7e955b90f52598b9a61be678f7385eea8edd549e49f52677
```

# Dockerfiles

### 描述
?> 最简单的`dockerfiles`

?> You use the -f flag with docker build to point to a Dockerfile anywhere in your file system

```
$ docker build -f /path/to/a/Dockerfile .
```

?> To tag the image into multiple repositories after the build, add multiple -t parameters when you run the build command

```
$ docker build -t shykes/myapp:1.0.2 -t shykes/myapp:latest .
```

```
FROM busybox
COPY /hello /
RUN cat /hello
```
### FROM
?> Whenever possible, use current official images as the basis for your images.
### LABEL
?> You can add labels to your image to help organize images by project
### RUN
?> Always combine RUN apt-get update with apt-get install in the same RUN statement.
```
FROM alpine
RUN export ADMIN_USER="mark" \
    && echo $ADMIN_USER > ./mark \
    && unset ADMIN_USER
CMD sh
```

!> `pipe`

```
RUN set -o pipefail && wget -O - https://some.site | wc -l > /number
```
### CMD
?> The CMD instruction should be used to run the software contained by your image, along with any arguments.
### EXPOSE
?> The EXPOSE instruction indicates the ports on which a container listens for connections. 
### ENV
```
ENV PG_MAJOR 9.3
ENV PG_VERSION 9.3.4
RUN curl -SL http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgress && …
ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH
```
### ADD or COPY
!> the best use for ADD is local tar file auto-extraction into the image, as in `ADD rootfs.tar.xz /`.
?> COPY the file package.json from your host to the present location (.) in your image (so in this case, to /usr/src/app/package.json)
```
COPY requirements.txt /tmp/
RUN pip install --requirement /tmp/requirements.txt
COPY . /tmp/
```
### ENTRYPOINT
?> The best use for ENTRYPOINT is to set the image’s main command, allowing that image to be run as though it was that command 
### VOLUME
?> The VOLUME instruction should be used to expose any database storage area, configuration storage, or files/folders created by your docker container
### USER
!> If a service can run without privileges, use USER to change to a non-root user.
```
RUN groupadd -r redis && useradd -r -g redis redis
USER redis
RUN [ "redis-server" ]
```
### WORKDIR
?> For clarity and reliability, you should always use absolute paths for your WORKDIR. Also, you should use WORKDIR instead of proliferating instructions like `RUN cd … && do-something`, which are hard to read, troubleshoot, and maintain.
!> Use WORKDIR to specify that all subsequent actions should be taken from the directory /usr/src/app in your image filesystem (never the host’s filesystem).
### ONBUILD
?> An ONBUILD command executes after the current Dockerfile build completes

### 例子
#### 构建elasticsearch
```
FROM elasticsearch:6.8.4
COPY elasticsearch-analysis-ik-6.8.4.zip /root
RUN echo "y"|/usr/share/elasticsearch/bin/elasticsearch-plugin install file:///root/elasticsearch-analysis-ik-6.8.4.zip
```

!> 执行`docker build --tag=elastic-qiu .`

```
docker run -t -i -p 9900:9200 -p 9800:9300 -e "discovery.type=single-node" elastic-qiu
```
