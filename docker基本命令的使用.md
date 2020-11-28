## docker基本命令的使用

### 一、docker安装 Ubuntu 镜像

#### 1、Hello world

```shell
docker pull ubuntu
#docker pull ubuntu:latest
docker images #查看本地镜像
docker run -itd --name ubuntu-test ubuntu #运行Ubuntu
docker exec -it ubuntu-test /bin/bash #进入Ubuntu服务中
root@8047180b80d4:/# echo "Hello"   #运行           
Hello
exit #退出当前容器
```

#### 2、运行交互式的容器

```shell
docker run -i -t ubuntu /bin/bash #docker 的两个参数 -i -t，让 docker 运行的容器实现"对话"的能力
root@0123ce188bd8:/#   #表示我们已经进入了Ubuntu系统中
exit #退出当前容器
```

各个参数解析：

- **-t:** 在新容器内指定一个伪终端或终端。
- **-i:** 允许你对容器内的标准输入 (STDIN) 进行交互

#### 3、启动容器（后台模式）

##### 使用以下命令创建一个以进程方式运行的容器

```shell
docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"

#使用docker ps 查看
docker ps
#结果：
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
cc5a321a9522        ubuntu              "/bin/sh -c 'while t…"   12 seconds ago      Up 10 seconds                           zealous_beaver
732f5b5f6571        ubuntu              "/bin/bash"              20 minutes ago      Up 20 minutes 
```

##### docker ps 命令 输出详情：

**CONTAINER ID:** 容器 ID。

**IMAGE:** 使用的镜像。

**COMMAND:** 启动容器时运行的命令。

**CREATED:** 容器的创建时间。

**STATUS:** 容器状态。

状态有7种：

- created（已创建）
- restarting（重启中）
- running（运行中）
- removing（迁移中）
- paused（暂停）
- exited（停止）
- dead（死亡）

**PORTS:** 容器的端口信息和使用的连接类型（tcp\udp）。

**NAMES:** 自动分配的容器名称。

##### 查看容器后台运行的日志

在宿主机上查看日志可以看见后台运行的容器发生了什么

```shell
docker logs id号 
```

##### 停止容器

```shell
docker stop id号 
```

##### 启动已停止的容器

```shell
docker start id号
```

##### 后台运行容器

```
docker run -itd --name ubuntu-test ubuntu /bin/bash
```

**注：**加了 **-d** 参数默认不会进入容器，想要进入容器需要使用指令 **docker exec**（下面会介绍到）。

##### 进入已经启用的容器

```
docker attach id号 #不推荐，进入后退出会导致容器停止
docker exec -it id号 /bin/bash #推荐
```

### 二、导出和导入容器

如果要导出本地某个容器，可以使用 **docker export** 命令。

```shell
docker export 1e560fca3906 > ubuntu.tar
```

导出容器 1e560fca3906 快照到本地文件 ubuntu.tar。

**导入容器快照**

可以使用 docker import 从容器快照文件中再导入为镜像，以下实例将快照文件 ubuntu.tar 导入到镜像 test/ubuntu:v1:

```shell
cat docker/ubuntu.tar | docker import - test/ubuntu:v1
```

此外，也可以通过指定 URL 或者某个目录来导入，例如：

```shell
docker import http://example.com/exampleimage.tgz example/imagerepo
```

### 三、删除容器

删除容器使用 **docker rm** 命令：

```shell
docker rm -f id号
docker container prune #清理掉所有处于终止状态的容器
```

