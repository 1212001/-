## docker的安装过程

### 1、卸载旧的版本

```shell
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 3、配置阿里云镜像仓库

```shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```



### 2、通过[设置Docker的存储库](https://docs.docker.com/engine/install/centos/#install-using-the-repository)进行安装

#### 2.1、安装`yum-utils`软件包（提供`yum-config-manager` 实用程序）并设置**稳定的**存储库。

```shell
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

#### 2.2、安装Docker Engine-Community

```shell
yum install docker-ce docker-ce-cli containerd.io
```

#### 2.3、启动docker

```shell
systemctl start docker
```

2.4、查看运行状态

```shell
service --status-all
```

