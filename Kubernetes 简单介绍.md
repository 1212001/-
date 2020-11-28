# Kubernetes 简单介绍

# 参考资料

> [k8s 文档](https://kubernetes.io)
>
> [k8s 源码](https://github.com/kubernetes/kubernetes)

# 目录

- [核心组件](## 核心组件 )
- [插件](## 插件)
  - [网络插件](### 网络插件)
- [版本查看](## 版本查看)

## 核心组件

1. **kube-apiserver**
2. kube-controller-manager
3. kube-proxy
4. kube-scheduler
5. kubelet
6. ~~kubectl~~

## 插件

### 网络插件

**部分介绍**

| 名称        | 项目地址                                       |
| ----------- | ---------------------------------------------- |
| calico      | https://github.com/projectcalico/calico        |
| cni-plugins | https://github.com/containernetworking/plugins |

## 版本查看

在 master 节点查看 `kubernetes` 版本

```
[root@master1 ~]# curl localhost:8080/version
{
  "major": "1",
  "minor": "12",
  "gitVersion": "v1.12.5",
  "gitCommit": "51dd616cdd25d6ee22c83a858773b607328a18ec",
  "gitTreeState": "archive",
  "buildDate": "2019-01-25T08:57:03Z",
  "goVersion": "go1.10.7",
  "compiler": "gc",
  "platform": "linux/amd64"
}
```