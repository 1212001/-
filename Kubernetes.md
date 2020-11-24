# Kubernetes 

### 1、是什么？

Kubernetes 是一个可移植的、可扩展的开源平台，用于管理容器化的工作负载和服务，可促进声明式配置和自动化。 Kubernetes 拥有一个庞大且快速增长的生态系统。Kubernetes 的服务、支持和工具广泛可用。

### 2、为什么需要 Kubernetes，它能做什么?

- **服务发现和负载均衡**

  Kubernetes 可以使用 DNS 名称或自己的 IP 地址公开容器，如果进入容器的流量很大， Kubernetes 可以负载均衡并分配网络流量，从而使部署稳定。

- **存储编排**

  Kubernetes 允许你自动挂载你选择的存储系统，例如本地存储、公共云提供商等。

- **自动部署和回滚**

  你可以使用 Kubernetes 描述已部署容器的所需状态，它可以以受控的速率将实际状态 更改为期望状态。例如，你可以自动化 Kubernetes 来为你的部署创建新容器， 删除现有容器并将它们的所有资源用于新容器。

- **自动完成装箱计算**

  Kubernetes 允许你指定每个容器所需 CPU 和内存（RAM）。 当容器指定了资源请求时，Kubernetes 可以做出更好的决策来管理容器的资源。

- **自我修复**

  Kubernetes 重新启动失败的容器、替换容器、杀死不响应用户定义的 运行状况检查的容器，并且在准备好服务之前不将其通告给客户端。

- **密钥与配置管理**

  Kubernetes 允许你存储和管理敏感信息，例如密码、OAuth 令牌和 ssh 密钥。 你可以在不重建容器镜像的情况下部署和更新密钥和应用程序配置，也无需在堆栈配置中暴露密钥。

### 3、Kubernetes 名词解释

一个 Kubernetes 集群包含 集群由一组被称作节点的机器组成。这些节点上运行 Kubernetes 所管理的容器化应用。集群具有至少一个工作节点和至少一个主节点。

工作节点托管作为应用程序组件的 Pod 。主节点管理集群中的工作节点和 Pod 。多个主节点用于为集群提供故障转移和高可用性。

本文档概述了交付正常运行的 Kubernetes 集群所需的各种组件。

#### 控制平面组件（Control Plane Components）

控制平面的组件对集群做出全局决策(比如调度)，以及检测和响应集群事件（例如，当不满足部署的 `replicas` 字段时，启动新的 [pod](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)）。

控制平面组件可以在集群中的任何节点上运行。 然而，为了简单起见，设置脚本通常会在同一个计算机上启动所有控制平面组件，并且不会在此计算机上运行用户容器。 请参阅[构建高可用性集群](https://kubernetes.io/zh/docs/setup/production-environment/tools/kubeadm/high-availability/) 中对于多主机 VM 的设置示例

##### kube-apiserver

[kube-apiserver](https://kubernetes.io/docs/admin/kube-apiserver)用于暴露Kubernetes API。任何的资源请求/调用操作都是通过kube-apiserver提供的接口进行

##### etcd

[etcd](https://kubernetes.io/docs/admin/etcd)是Kubernetes提供默认的存储系统，保存所有集群数据，使用时需要为etcd数据提供备份计划。

##### kube-controller-manager

[kube-controller-manager](https://kubernetes.io/docs/admin/kube-controller-manager)运行管理控制器，它们是集群中处理常规任务的后台线程。逻辑上，每个控制器是一个单独的进程，但为了降低复杂性，它们都被编译成单个二进制文件，并在单个进程中运行。

这些控制器包括：

- [节点（Node）控制器](http://docs.kubernetes.org.cn/304.html)。
- 副本（Replication）控制器：负责维护系统中每个副本中的pod。
- 端点（Endpoints）控制器：填充Endpoints对象（即连接Services＆Pods）。
- [Service Account](http://docs.kubernetes.org.cn/84.html)和Token控制器：为新的[Namespace](http://docs.kubernetes.org.cn/242.html) 创建默认帐户访问API Token。

##### lable(标签)

一、**Label是什么？**

Label是Kubernetes系统中的一个核心概念。
Label以key/value键值对的形式附加到各种对象上，如Pod、Service、RC、Node等。
Label定义了这些对象的可识别属性，用来对它们进行管理和选择。Label可以在创建时附加到对象上，也可以在对象创建后通过API进行管理。

二、**Label Selector**

在为对象定义好Label后，其他对象就可以使用Label Selector（选择器）来定义其作用的对象了。

Label Selector的定义由多个逗号分隔的条件组成。

"labels": {
"key1": "value1",
"key2": "value2"
}

当前有两种Label Selector：

基于等式的（Equality-based）和基于集合的（Set-based），在使用时可以将多个Label进行组合来选择。


1、基于等式的Label Selector使用等式类的表达式来进行选择：

name = redis-slave: 选择所有包含Label中key="name"且value="redis-slave"的对象；
env != production: 选择所有包括Label中的key="env"且value不等于"production"的对象。


2、基于集合的Label Selector使用集合操作的表达式来进行选择：

name in (redis-master, redis-slave): 选择所有包含Label中的key="name"且value="redis-master"或"redis-slave"的对象；
name not in (php-frontend): 选择所有包含Label中的key="name"且value不等于"php-frontend"的对象。

ps：在某些对象需要对另一些对象进行选择时，可以将多个Label Selector进行组合，使用逗号","进行分隔即可。
基于等式的LabelSelector和基于集合的Label Selector可以任意组合。
例如：

name=redis-slave,env!=production
name not in (php-frontend),env!=production

##### secret

Secret是用来保存小片敏感数据的k8s资源，例如密码，token，或者秘钥。这类数据当然也可以存放在Pod或者镜像中，但是放在Secret中是为了更方便的控制如何使用数据，并减少暴露的风险。

用户可以创建自己的secret，系统也会有自己的secret。

Pod需要先引用才能使用某个secret，Pod有2种方式来使用secret：作为volume的一个域被一个或多个容器挂载；在拉取镜像的时候被kubelet引用。

•系统自动创建API认证

•用户可自定义secrect – base64

•作为文件挂载到容器中

•作为环境变量传递给容器

##### configMap

 ConfigMap用于保存配置数据的键值对，可以用来保存单个属性，也可以用来保存配置文件。ConfigMap跟secret很类似，但它可以更方便地处理不包含敏感信息的字符串

##### Container Runtime容器运行时

负责运行容器的软件

##### DNS

尽管其他插件都并非严格意义上的必需组件，但几乎所有 Kubernetes 集群都应该 有[集群 DNS](https://kubernetes.io/zh/docs/concepts/services-networking/dns-pod-service/)， 因为很多示例都需要 DNS 服务。

集群 DNS 是一个 DNS 服务器，和环境中的其他 DNS 服务器一起工作，它为 Kubernetes 服务提供 DNS 记录。

Kubernetes 启动的容器自动将此 DNS 服务器包含在其 DNS 搜索列表中。

#### kubelet

在kubernetes集群中，每个Node节点都会启动kubelet进程，用来处理Master节点下发到本节点的任务，管理Pod和其中的容器。kubelet会在API Server上注册节点信息，定期向Master汇报节点资源使用情况，并通过cAdvisor监控容器和节点资源。可以把kubelet理解成【Server-Agent】架构中的agent，是Node上的pod管家。

##### 节点管理

节点通过设置kubelet的启动参数“--register-node”，来决定是否向API Server注册自己，默认为true。可以通过kubelet --help或者查看kubernetes源码【cmd/kubelet/app/server.go中】来查看该参数。

##### kubelet的配置文件

默认配置文件在/etc/kubernetes/kubelet中，其中

- --api-servers：用来配置Master节点的IP和端口。
- --kubeconfig：用来配置kubeconfig的路径，kubeconfig文件常用来指定证书。
- --hostname-override：用来配置该节点在集群中显示的主机名。
- --node-status-update-frequency：配置kubelet向Master心跳上报的频率，默认为10s。

##### Pod管理

kubelet有几种方式获取自身Node上所需要运行的Pod清单。但本文只讨论通过API Server监听etcd目录，同步Pod列表的方式。

kubelet通过API Server Client使用WatchAndList的方式监听etcd中/registry/nodes/${当前节点名称}和/registry/pods的目录，将获取的信息同步到本地缓存中。

kubelet监听etcd，执行对Pod的操作，对容器的操作则是通过Docker Client执行，例如启动删除容器等。

##### kubelet创建和修改Pod流程：

1. 为该Pod创建一个数据目录。
2. 从API Server读取该Pod清单。
3. 为该Pod挂载外部卷（External Volume）
4. 下载Pod用到的Secret。
5. 检查运行的Pod，执行Pod中未完成的任务。
6. 先创建一个Pause容器，该容器接管Pod的网络，再创建其他容器。
7. Pod中容器的处理流程： 1）比较容器hash值并做相应处理。 2）如果容器被终止了且没有指定重启策略，则不做任何处理。 3）调用Docker Client下载容器镜像，调用Docker Client运行容器。

#### kubectl

kubectl是一个用于操作kubernetes集群的命令行接口，通过利用kubectl的各种命令可以实现各种功能。

#### Volume

Volume是Pod中能够被多个容器共享的磁盘目录。我们知道，默认情况下Docker容器中的数据都是非持久化的，在容器消亡后数据也会消失。因此Docker提供了Volume机制以便实现数据的持久化。Kubernetes中Volume的概念与Docker中的Volume类似，但不完全相同。具体区别如下：

- Kubernetes中的Volume与Pod的生命周期相同，但与容器的生命周期不相关。当容器终止或重启时，Volume中的数据也不会丢失。
- 当Pod被删除时，Volume才会被清理。并且数据是否丢失取决于Volume的具体类型，比如emptyDir类型的Volume数据会丢失，而PV类型的数据则不会丢失。

Kubernetes提供了非常丰富的Volume类型，下面是一些常用的Volume类型：

- emptyDir
- hostPath
- gcePersistentDisk

> 注意，这些 volume 并非全部都是持久化的，比如 emptyDir、secret、gitRepo 等，这些 volume 会随着 Pod 的消亡而消失。

##### emptyDir

个emptyDir volume是在Pod分配到Node时创建的。顾名思义，它的初始内容为空，在同一个Pod中的所有容器均可以读写这个emptyDir volume。当 Pod 从 Node 上被删除（Pod 被删除，或者 Pod 发生迁移），emptyDir 也会被删除，并且数据永久丢失。

emptyDir类型的volume适合于以下场景：

- 临时空间。例如某些程序运行时所需的临时目录，无需永久保存。
- 一个容器需要从另一容器中获取数据的目录（多容器共享目录）

##### hostPath

hostPath类型的volume允许用户挂在Node上的文件系统到Pod中，如果 Pod 需要使用 Node 上的文件，可以使用 hostPath。

hostPath volume通常用于以下场景：

- 容器中的应用程序产生的日志文件需要永久保存，可以使用宿主机的文件系统进行存储。
- 需要访问宿主机上Docker引擎内部数据结构的容器应用，通过定义hostPath为/var/lib/docker目录，使容器内应用可以直接访问Docker的文件系统。

在使用hostPath volume时，需要注意：

- 在不同的Node上具有相同配置的Pod，可能会因为宿主机上的目录和文件不同，而导致对Volume上目录和文件的访问结果不一致。

##### gcePersistentDisk

gcePersistentDisk 可以挂载 GCE（Google Compute Engine） 上的永久磁盘到容器，需要 Kubernetes 运行在 GCE 的 VM 中。需要注意的是，你必须先创建一个永久磁盘（Persistent Disk，PD）才能使用gcePersistentDisk volume。

##### awsElasticBlockStore

与gcePersistentDisk类似，awsElasticBlockStore 使用Amazon Web Service（AWS）提供的EBS Volume，挂在到Pod中。需要 Kubernetes 运行在 AWS 的 EC2 上。另外，需要先创建一个EBS Volume才能使用awsElasticBlockStore。

##### nfs

NFS 是 Network File System 的缩写，即网络文件系统。Kubernetes 中通过简单地配置就可以挂载 NFS 到 Pod 中，而 NFS 中的数据是可以永久保存的，同时 NFS 支持同时写操作。

##### gitRepo

通过挂在一个空目录，并从GIT仓库克隆一个repository供Pod使用。

##### subPath

Pod 的多个容器使用同一个 Volume 时，subPath 非常有用。

### 4、集群资源管理

#### Node

kubernetes集群的工作节点，可以是物理机也可以是虚拟机。

为了管理Pod，每个Node节点上至少要运行container runtime（比如docker或者rkt）、kubelet和kube-proxy服务。

##### Node的状态

•地址：包括hostname、外网IP和内网IP

•条件（Condition）：包括OutOfDisk、Ready、MemoryPressure和DiskPressure

•容量（Capacity）：Node上的可用资源，包括CPU、内存和Pod总数

•基本信息（Info）：包括内核版本、容器引擎版本、OS类型等

#### Namespace

在一个Kubernetes集群中可以使用namespace创建多个“虚拟集群”，这些namespace之间可以完全隔离



•Namespace是对一组资源和对象的抽象集合，比如可以用来将系统内部的对象划分为不同的项目组或用户组。

•常见的pods, services, replication controllers和deployments等都是属于某一个namespace的（默认是default），而node, persistentVolumes等则不属于任何namespace。

##### Namespace操作

•kubectl可以通过--namespace或者-n选项指定namespace。如果不指定，默认为default。查看操作下,也可以通过设置--all-namespace=true来查看所有namespace下的资源。

• **查询**

•$ kubectl get namespaces

•NAME     STATUS  AGE

•default    Active  11d

•kube-system  Active  11d

•注意：namespace包含两种状态"Active"和"Terminating"。在namespace删除过程中，namespace状态被设置成"Terminating"。

##### 删除Namespace

•$ kubectl delete namespaces new-namespace

•删除一个namespace会自动删除所有属于该namespace的资源。

•default和kube-system命名空间不可删除。

•PersistentVolume是不属于任何namespace的，但PersistentVolumeClaim是属于某个特定namespace的。

•Event是否属于namespace取决于产生event的对象。

•v1.7版本增加了kube-public命名空间，该命名空间用来存放公共的信息，一般以ConfigMap的形式存放。

•kubectl get configmap -n=kube-public

#### Kubernetes存储卷	

•我们知道默认情况下容器的数据都是非持久化的，在容器消亡以后数据也跟着丢失，所以Docker提供了Volume机制以便将数据持久化存储。类似的，Kubernetes提供了更强大的Volume机制和丰富的插件，解决了容器数据持久化和容器间共享数据的问题。

•与Docker不同，Kubernetes Volume的生命周期与Pod绑定

•容器挂掉后Kubelet再次重启容器时，Volume的数据依然还在

•而Pod删除时，Volume才会清理。数据是否丢失取决于具体的Volume类型，比如emptyDir的数据会丢失，而PV的数据则不会丢。

### 4、节点

Kubernetes 通过将容器放入在节点（Node）上运行的 Pod 中来执行你的工作负载。 节点可以是一个虚拟机或者物理机器，取决于所在的集群配置。每个节点都包含用于运行 [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/) 所需要的服务，这些服务由 [控制面](https://kubernetes.io/zh/docs/reference/glossary/?all=true#term-control-plane)负责管理。

通常集群中会有若干个节点；而在一个学习用或者资源受限的环境中，你的集群中也可能 只有一个节点。

节点上的[组件](https://kubernetes.io/zh/docs/concepts/overview/components/#node-components)包括 [kubelet](https://kubernetes.io/docs/reference/generated/kubelet)、 [容器运行时](https://kubernetes.io/docs/setup/production-environment/container-runtimes)以及 [kube-proxy](https://kubernetes.io/zh/docs/reference/command-line-tools-reference/kube-proxy/)。

#### 管理

向 [API 服务器](https://kubernetes.io/zh/docs/reference/command-line-tools-reference/kube-apiserver/)添加节点的方式主要有两种：

1. 节点上的 `kubelet` 向控制面执行自注册；
2. 你，或者别的什么人，手动添加一个 Node 对象。

在你创建了 Node 对象或者节点上的 `kubelet` 执行了自注册操作之后， 控制面会检查新的 Node 对象是否合法。例如，如果你使用下面的 JSON 对象来创建 Node 对象：

```json
{
  "kind": "Node",
  "apiVersion": "v1",
  "metadata": {
    "name": "10.240.79.157",
    "labels": {
      "name": "my-first-k8s-node"
    }
  }
}
```

Kubernetes 会在内部创建一个 Node 对象作为节点的表示。Kubernetes 检查 `kubelet` 向 API 服务器注册节点时使用的 `metadata.name` 字段是否匹配。 如果节点是健康的（即所有必要的服务都在运行中），则该节点可以用来运行 Pod。 否则，直到该节点变为健康之前，所有的集群活动都会忽略该节点。

> **说明：** Kubernetes 会一直保存着非法节点对应的对象，并持续检查该节点是否已经 变得健康。 你，或者某个[控制器](https://kubernetes.io/docs/admin/kube-controller-manager/)必需显式地 删除该 Node 对象以停止健康检查操作。

Node 对象的名称必须是合法的 [DNS 子域名](https://kubernetes.io/zh/docs/concepts/overview/working-with-objects/names#dns-subdomain-names)

#### 节点自注册

当 kubelet 标志 `--register-node` 为 true（默认）时，它会尝试向 API 服务注册自己。 这是首选模式，被绝大多数发行版选用。

对于自注册模式，kubelet 使用下列参数启动：

- `--kubeconfig` - 用于向 API 服务器表明身份的凭据路径。
- `--cloud-provider` - 与某[云驱动](https://kubernetes.io/zh/docs/reference/glossary/?all=true#term-cloud-provider) 进行通信以读取与自身相关的元数据的方式。
- `--register-node` - 自动向 API 服务注册。
- `--register-with-taints` - 使用所给的污点列表（逗号分隔的 `<key>=<value>:<effect>`）注册节点。 当 `register-node` 为 false 时无效。
- `--node-ip` - 节点 IP 地址。
- `--node-labels` - 在集群中注册节点时要添加的 [标签](https://kubernetes.io/zh/docs/concepts/overview/working-with-objects/labels/)。 （参见 [NodeRestriction 准入控制插件](https://kubernetes.io/zh/docs/reference/access-authn-authz/admission-controllers/#noderestriction)所实施的标签限制）。
- `--node-status-update-frequency` - 指定 kubelet 向控制面发送状态的频率。

#### 手动节点管理

你可以使用 [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/) 来创建和修改 Node 对象。

如果你希望手动创建节点对象时，请设置 kubelet 标志 `--register-node=false`。

你可以修改 Node 对象（忽略 `--register-node` 设置）。 例如，修改节点上的标签或标记其为不可调度。

你可以结合使用节点上的标签和 Pod 上的选择算符来控制调度。 例如，你可以限制某 Pod 只能在符合要求的节点子集上运行。

如果标记节点为不可调度（unschedulable），将阻止新 Pod 调度到该节点之上，但不会 影响任何已经在其上的 Pod。 这是重启节点或者执行其他维护操作之前的一个有用的准备步骤。

要标记一个节点为不可调度，执行以下命令：

```shell
kubectl cordon $NODENAME
```

> **说明：** 被 [DaemonSet](https://kubernetes.io/zh/docs/concepts/workloads/controllers/daemonset/) 控制器创建的 Pod 能够容忍节点的不可调度属性。 DaemonSet 通常提供节点本地的服务，即使节点上的负载应用已经被腾空，这些服务也仍需 运行在节点之上。

#### 节点状态

一个节点的状态包含以下信息:

- [地址](https://kubernetes.io/zh/docs/concepts/architecture/nodes/#addresses)
- [状况](https://kubernetes.io/zh/docs/concepts/architecture/nodes/#condition)
- [容量与可分配](https://kubernetes.io/zh/docs/concepts/architecture/nodes/#capacity)
- [信息](https://kubernetes.io/zh/docs/concepts/architecture/nodes/#info)

使用kubectl命令查看节点的详细信息

```
kubectl describe node <节点名称>
```

##### 地址

这些字段的用法取决于你的云服务商或者物理机配置。

- HostName：由节点的内核设置。可以通过 kubelet 的 `--hostname-override` 参数覆盖。
- ExternalIP：通常是节点的可外部路由（从集群外可访问）的 IP 地址。
- InternalIP：通常是节点的仅可在集群内部路由的 IP 地址

##### 状况

`conditions` 字段描述了所有 `Running` 节点的状态。状况的示例包括：

> **说明：** 如果使用命令行工具来打印已保护（Cordoned）节点的细节，其中的 Condition 字段可能 包括 `SchedulingDisabled`。`SchedulingDisabled` 不是 Kubernetes API 中定义的 Condition，被保护起来的节点在其规约中被标记为不可调度（Unschedulable）。

节点条件使用 JSON 对象表示。例如，下面的响应描述了一个健康的节点。

```json
"conditions": [
  {
    "type": "Ready",
    "status": "True",
    "reason": "KubeletReady",
    "message": "kubelet is posting ready status",
    "lastHeartbeatTime": "2019-06-05T18:38:35Z",
    "lastTransitionTime": "2019-06-05T11:41:27Z"
  }
]
```

如果 Ready 条件处于 `Unknown` 或者 `False` 状态的时间超过了 `pod-eviction-timeout` 值， （一个传递给 [kube-controller-manager](https://kubernetes.io/docs/reference/generated/kube-controller-manager/) 的参数）， 节点上的所有 Pod 都会被节点控制器计划删除。默认的逐出超时时长为 **5 分钟**。 某些情况下，当节点不可达时，API 服务器不能和其上的 kubelet 通信。 删除 Pod 的决定不能传达给 kubelet，直到它重新建立和 API 服务器的连接为止。 与此同时，被计划删除的 Pod 可能会继续在游离的节点上运行。

节点控制器在确认 Pod 在集群中已经停止运行前，不会强制删除它们。 你可以看到这些可能在无法访问的节点上运行的 Pod 处于 `Terminating` 或者 `Unknown` 状态。 如果 kubernetes 不能基于下层基础设施推断出某节点是否已经永久离开了集群， 集群管理员可能需要手动删除该节点对象。 从 Kubernetes 删除节点对象将导致 API 服务器删除节点上所有运行的 Pod 对象并释放它们的名字。

节点生命周期控制器会自动创建代表状况的 [污点](https://kubernetes.io/zh/docs/concepts/scheduling-eviction/taint-and-toleration/)。 当调度器将 Pod 指派给某节点时，会考虑节点上的污点。 Pod 则可以通过容忍度（Toleration）表达所能容忍的污点。

`capacity` 块中的字段标示节点拥有的资源总量。 `allocatable` 块指示节点上可供普通 Pod 消耗的资源量。



### 基本命令

#### 创建deployment

命令行创建：

kubectl create deployment java-deploy --image=registry.cluster.local/lijun/java:v1  --namespace lijun

yaml文件创建：

```
kubectl create -f pod.yaml
```

#### 删除deployment

kubectl delete deployment java-deploy -n lijun

#### 扩展副本

kubectl -n lijun scale deployment java-deploy --replicas=4

#### 查看具体的deployment的yaml

kubectl get  deployments java-deploy -n lijun -o yaml

#### 修改具体资源的yaml文件

kubectl -n lijun edit deployment java-deploy

#### 暴漏端口到外网

kubectl expose deployment/java-deploy --type="NodePort" --port=8080 -n lijun

#### 创建secret对象

官方教程地址：https://kubernetes.io/zh/docs/concepts/configuration/secret/

例如，要使用 `data` 字段将两个字符串存储在 Secret 中，请按如下所示将它们转换为 base64：

```shell
echo -n 'admin' | base64
```

解码 `password` 字段：

```shell
echo 'MWYyZDFlMmU2N2Rm' | base64 --decode
```

输出类似于：

```
YWRtaW4=
```

现在可以像这样写一个 Secret 对象：

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

使用 [`kubectl apply`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply) 创建 Secret 对象：

```shell
kubectl apply -f ./secret.yaml
```

#### 创建ConfigMaps对象

官方地址：https://kubernetes.io/zh/docs/concepts/configuration/configmap/

#### Ingress使用

官方地址：https://kubernetes.io/zh/docs/concepts/services-networking/ingress/

#### 1、更新 deployment中的镜像

```shell
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1 --record


#或者
kubectl --record deployment.apps/nginx-deployment set image \
   deployment.v1.apps/nginx-deployment nginx=nginx:1.9.1
   
#或者，可以 edit Deployment 并将 .spec.template.spec.containers[0].image 从 nginx:1.14.2 更改至 nginx:1.16.1。

kubectl edit deployment.v1.apps/nginx-deployment
```

#### 2、要查看上线状态，运行：

```shell
kubectl rollout status deployment.v1.apps/nginx-deployment
```

#### 3、检查 Deployment 修订历史：

```shell
kubectl rollout history deployment.v1.apps/nginx-deployment
```

输出类似于：

```shell
deployments "nginx-deployment"
REVISION    CHANGE-CAUSE
1           kubectl apply --filename=https://k8s.io/examples/controllers/nginx-deployment.yaml --record=true
2           kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.9.1 --record=true
3           kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.91 --record=true
```

`CHANGE-CAUSE` 的内容是从 Deployment 的 `kubernetes.io/change-cause` 注解复制过来的。 复制动作发生在修订版本创建时。你可以通过以下方式设置 `CHANGE-CAUSE` 消息：

- 使用 `kubectl annotate deployment.v1.apps/nginx-deployment kubernetes.io/change-cause="image updated to 1.9.1"` 为 Deployment 添加注解。
- 追加 `--record` 命令行标志以保存正在更改资源的 `kubectl` 命令。
- 手动编辑资源的清单。

要查看修订历史的详细信息，运行：

```shell
kubectl rollout history deployment.v1.apps/nginx-deployment --revision=2
```

#### 4、回滚到之前的修订版本

假定现在你已决定撤消当前上线并回滚到以前的修订版本：

```shell
kubectl rollout undo deployment.v1.apps/nginx-deployment
```

输出类似于：

```
deployment.apps/nginx-deployment
```

或者，你也可以通过使用 `--to-revision` 来回滚到特定修订版本：

```shell
kubectl rollout undo deployment.v1.apps/nginx-deployment --to-revision=2
```

输出类似于：

```
deployment.apps/nginx-deployment
```

与回滚相关的指令的更详细信息，请参考 [`kubectl rollout`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#rollout)。

现在，Deployment 正在回滚到以前的稳定版本。正如你所看到的，Deployment 控制器生成了 回滚到修订版本 2 的 `DeploymentRollback` 事件。

#### 5、缩放 Deployment

你可以使用如下指令缩放 Deployment：

```shell
kubectl scale deployment.v1.apps/nginx-deployment --replicas=10
```

输出类似于：

```
deployment.apps/nginx-deployment scaled
```

假设集群启用了[Pod 的水平自动缩放](https://kubernetes.io/zh/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)， 你可以为 Deployment 设置自动缩放器，并基于现有 Pods 的 CPU 利用率选择 要运行的 Pods 个数下限和上限。

```shell
kubectl autoscale deployment.v1.apps/nginx-deployment --min=10 --max=15 --cpu-percent=80
```

输出类似于：

```
deployment.apps/nginx-deployment scaled
```

#### 6、比例缩放

RollingUpdate 的 Deployment 支持同时运行应用程序的多个版本。 当自动缩放器缩放处于上线进程（仍在进行中或暂停）中的 RollingUpdate Deployment 时， Deployment 控制器会平衡现有的活跃状态的 ReplicaSets（含 Pods 的 ReplicaSets）中的额外副本， 以降低风险。这称为 *比例缩放（Proportional Scaling）*。

例如，你正在运行一个 10 个副本的 Deployment，其 [maxSurge](https://kubernetes.io/zh/docs/concepts/workloads/controllers/deployment/#max-surge)=3，[maxUnavailable](https://kubernetes.io/zh/docs/concepts/workloads/controllers/deployment/#max-unavailable)=2。

- 确保 Deployment 的这 10 个副本都在运行。

  ```shell
  kubectl get deploy
  ```

  输出类似于：

  ```
  NAME                 DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
  nginx-deployment     10        10        10           10          50s
  ```

- 更新 Deployment 使用新镜像，碰巧该镜像无法从集群内部解析。

  ```shell
  kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:sometag
  ```

  输出类似于：

  ```
  deployment.apps/nginx-deployment image updated
  ```

- 镜像更新使用 ReplicaSet `nginx-deployment-1989198191` 启动新的上线过程， 但由于上面提到的 `maxUnavailable` 要求，该进程被阻塞了。检查上线状态：

  ```shell
  kubectl get rs
  ```

  输出类似于：

  ```
  NAME                          DESIRED   CURRENT   READY     AGE
  nginx-deployment-1989198191   5         5         0         9s
  nginx-deployment-618515232    8         8         8         1m
  ```

- 然后，出现了新的 Deployment 扩缩请求。自动缩放器将 Deployment 副本增加到 15。 Deployment 控制器需要决定在何处添加 5 个新副本。如果未使用比例缩放，所有 5 个副本 都将添加到新的 ReplicaSet 中。使用比例缩放时，可以将额外的副本分布到所有 ReplicaSet。 较大比例的副本会被添加到拥有最多副本的 ReplicaSet，而较低比例的副本会进入到 副本较少的 ReplicaSet。所有剩下的副本都会添加到副本最多的 ReplicaSet。 具有零副本的 ReplicaSets 不会被扩容。

在上面的示例中，3 个副本被添加到旧 ReplicaSet 中，2 个副本被添加到新 ReplicaSet。 假定新的副本都很健康，上线过程最终应将所有副本迁移到新的 ReplicaSet 中。 要确认这一点，请运行：

```shell
kubectl get deploy
```

输出类似于：

```
NAME                 DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment     15        18        7            8           7m
```

上线状态确认了副本是如何被添加到每个 ReplicaSet 的。

```shell
kubectl get rs
```

输出类似于：

```shell
NAME                          DESIRED   CURRENT   READY     AGE
nginx-deployment-1989198191   7         7         0         7m
nginx-deployment-618515232    11        11        11        7m
```

7、暂停、恢复Deployment

你可以在触发一个或多个更新之前暂停 Deployment，然后再恢复其执行。 这样做使得你能够在暂停和恢复执行之间应用多个修补程序，而不会触发不必要的上线操作。

- 例如，对于一个刚刚创建的 Deployment： 获取 Deployment 信息：

  ```shell
  kubectl get deploy
  ```

  输出类似于：

  ```
  NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
  nginx     3         3         3            3           1m
  ```

  获取上线状态：

  ```shell
  kubectl get rs
  ```

  输出类似于：

  ```shell
  NAME               DESIRED   CURRENT   READY     AGE
  nginx-2142116321   3         3         3         1m
  ```

- 使用如下指令暂停运行：

  ```shell
  kubectl rollout pause deployment.v1.apps/nginx-deployment
  ```

  输出类似于：

  ```shell
  deployment.apps/nginx-deployment paused
  ```

- 接下来更新 Deployment 镜像：

  ```shell
  kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.16.1
  ```

  输出类似于：

  ```
  deployment.apps/nginx-deployment image updated
  ```

- 注意没有新的上线被触发：

  ```shell
  kubectl rollout history deployment.v1.apps/nginx-deployment
  ```

  输出类似于：

  ```shell
  deployments "nginx"
  REVISION  CHANGE-CAUSE
  1   <none>
  ```

- 获取上线状态确保 Deployment 更新已经成功：

  ```shell
  kubectl get rs
  ```

  输出类似于：

  ```shell
  NAME               DESIRED   CURRENT   READY     AGE
  nginx-2142116321   3         3         3         2m
  ```

- 你可以根据需要执行很多更新操作，例如，可以要使用的资源：

  ```shell
  kubectl set resources deployment.v1.apps/nginx-deployment -c=nginx --limits=cpu=200m,memory=512Mi
  ```

  输出类似于：

  ```
  deployment.apps/nginx-deployment resource requirements updated
  ```

  暂停 Deployment 之前的初始状态将继续发挥作用，但新的更新在 Deployment 被 暂停期间不会产生任何效果。

- 最终，恢复 Deployment 执行并观察新的 ReplicaSet 的创建过程，其中包含了所应用的所有更新：

  ```shell
  kubectl rollout resume deployment.v1.apps/nginx-deployment
  ```

  输出：

  ```shell
  deployment.apps/nginx-deployment resumed
  ```

- 观察上线的状态，直到完成。

  ```shell
  kubectl get rs -w
  ```

  输出类似于：

  ```
  NAME               DESIRED   CURRENT   READY     AGE
  nginx-2142116321   2         2         2         2m
  nginx-3926361531   2         2         0         6s
  nginx-3926361531   2         2         1         18s
  nginx-2142116321   1         2         2         2m
  nginx-2142116321   1         2         2         2m
  nginx-3926361531   3         2         1         18s
  nginx-3926361531   3         2         1         18s
  nginx-2142116321   1         1         1         2m
  nginx-3926361531   3         3         1         18s
  nginx-3926361531   3         3         2         19s
  nginx-2142116321   0         1         1         2m
  nginx-2142116321   0         1         1         2m
  nginx-2142116321   0         0         0         2m
  nginx-3926361531   3         3         3         20s
  ```

- 获取最近上线的状态：

  ```shell
  kubectl get rs
  ```

  输出类似于：

  ```
  NAME               DESIRED   CURRENT   READY     AGE
  nginx-2142116321   0         0         0         2m
  nginx-3926361531   3         3         3         28s
  ```

> **说明：** 你不可以回滚处于暂停状态的 Deployment，除非先恢复其执行状态。

