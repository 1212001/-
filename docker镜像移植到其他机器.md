# docker移植到其他机器

## 一、保存镜像到本地

```shell
dicker save 镜像id > /保存路径/**.tar
```

## 二、拷贝到目标服务器

可以先下载到本地，再上传到其他服务器

## 三、加载镜像到docker

```shell
docker load < /保存路径/**.tar
```

## 四、查看目标镜像

```shell
dicker images
```

## 五、修改当前镜像标签

```shell
docker tag 镜像id (自定义)镜像名字:版本
```

## 六、启动当前容器

```shell
docker run -d -p 8081:80 --name nginx-web nginx-web
```

## 七、访问

