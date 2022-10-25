---
Author: sync
date: 2022-05-29 23:02 Sunday
tag: #k8s
date updated: 2022-05-31 22:02 Tuesday
---

# I. K8S 概览

K8S 的架构、作用和目的。需要首先对 K8S 整体有所了解。

## 1.1 K8S 是什么？

Kubernetes is an open source system for managing containerized applications across multiple hosts. It provides basic mechanisms for deployment, maintenance, and scaling of applications.

- 用于自动部署、扩展和管理“容器化（containerized）应用程序”的开源系统。

插一句题外话：
为什么 Kubernetes 要叫 Kubernetes 呢？维基百科已经交代了（老美对星际是真的痴迷）：
Kubernetes（在希腊语意为“舵手”或“驾驶员”）由 Joe Beda、Brendan Burns 和 Craig McLuckie 创立，并由其他谷歌工程师，包括 Brian Grant 和 Tim Hockin 等进行加盟创作，并由谷歌在 2014 年首次对外宣布 。该系统的开发和设计都深受谷歌的 Borg 系统的影响，其许多顶级贡献者之前也是 Borg 系统的开发者。在谷歌内部，Kubernetes 的原始代号曾经是 Seven，即星际迷航中的 Borg（博格人）。Kubernetes 标识中舵轮有七个轮辐就是对该项目代号的致意。
为什么 Kubernetes 的缩写是 K8S 呢？我个人赞同 Why Kubernetes is Abbreviated k8s 中说的观点“嘛，写全称也太累了吧，不如整个缩写”。其实只保留首位字符，用具体数字来替代省略的字符个数的做法，还是比较常见的。

## 1.2 为什么是 K8S？

- 传统后端部署方法的不足
- 就是 K8S 要做的事情：<mark style="background: #FFF3A3A6;">自动化运维管理 Docker（容器化）程序。</mark>

<mark style="background: #FFF3A3A6;">试想下传统的后端部署办法</mark> ：把程序包（包括可执行二进制文件、配置文件等）放到服务器上，接着运行启动脚本把程序跑起来，同时启动守护脚本定期检查程序运行状态、必要的话重新拉起程序。
有问题吗？显然有！最大的一个问题在于：如果服务的请求量上来，已部署的服务响应不过来怎么办？传统的做法往往是，如果请求量、内存、CPU 超过阈值做了告警，运维马上再加几台服务器，部署好服务之后，接入负载均衡来分担已有服务的压力。
问题出现了：从监控告警到部署服务，中间需要人力介入！那么，有没有办法自动完成服务的部署、更新、卸载和扩容、缩容呢？
这，就是 K8S 要做的事情：<mark style="background: #FFF3A3A6;">自动化运维管理 Docker（容器化）程序。</mark>

## 1.3 K8S 怎么做？

我们已经知道了 K8S 的核心功能：<mark style="background: #FFF3A3A6;">自动化运维管理多个容器化程序。</mark>
那么 K8S 怎么做到的呢？这里，我们从宏观架构上来学习 K8S 的设计思想。首先看下图，图片来自文章 Components of Kubernetes Architecture：

- ![](https://miro.medium.com/max/1400/1*HXbT0c4Q5XaiCIp6y3VMvw.png)

### 1.3.1 Overview

- K8S 是属于主从<mark style="background: #FFF3A3A6;">设备模型（Master-Slave 架构</mark> ）
- <mark style="background: #FFF3A3A6;">Master Node 和 Worker Node 是分别安装了 K8S 的 Master 和 Woker 组件的实体服务器</mark>

K8S 是属于主从<mark style="background: #FFF3A3A6;">设备模型（Master-Slave 架构</mark> ），即有 Master 节点负责核心的调度、管理和运维，Slave 节点则在执行用户的程序。但是在 K8S 中，主节点一般被称为 Master Node 或者 Head Node（本文采用 Master Node 称呼方式），而从节点则被称为 Worker Node 或者 Node（本文采用 Worker Node 称呼方式）。

要注意一点：<mark style="background: #FFF3A3A6;">Master Node 和 Worker Node 是分别安装了 K8S 的 Master 和 Woker 组件的实体服务器</mark> ，<mark style="background: #FF5582A6;">每个 Node 都对应了一台实体服务器（虽然 Master Node 可以和其中一个 Worker Node 安装在同一台服务器，但是建议 Master Node 单独部署），</mark> 所有 Master Node 和 Worker Node 组成了 K8S 集群，同一个集群可能存在多个 Master Node 和 Worker Node。

### 1.3.2 Master Node 组件

- **API Server： K8S 的请求入口服务**
- **Scheduler： K8S 所有 Worker Node 的调度器**
- **Controller Manager： K8S 所有 Worker Node 的监控器**
- **etcd： K8S 的存储服务**

首先来看 **Master Node** 都有哪些组件：

<mark style="background: #FF5582A6;">API Server。</mark> K8S 的请求入口服务。**API Server 负责接收 K8S 所有请求（来自 UI 界面或者 CLI 命令行工具），然后，API Server 根据用户的具体请求，去通知其他组件干活。** <mark style="background: #FF5582A6;">Scheduler。</mark> K8S 所有 Worker Node 的调度器。**当用户要部署服务时，Scheduler 会选择最合适的 Worker Node（服务器）来部署。** <mark style="background: #FF5582A6;">Controller Manager。</mark> K8S 所有 Worker Node 的监控器。Controller Manager 有很多具体的 Controller，在文章 Components of Kubernetes Architecture 中提到的有<mark style="background: #BBFABBA6;">Node
Controller</mark> 、<mark style="background: #BBFABBA6;">Service Controller</mark> 、<mark style="background: #BBFABBA6;">Volume Controller</mark> 等。Controller 负责监控和调整在 Worker Node 上部署的服务的状态，比如用户要求 A 服务部署 2 个副本，那么当其中一个服务挂了的时候，Controller 会马上调整，让 Scheduler 再选择一个 Worker Node 重新部署服务。 <mark style="background: #FF5582A6;">etcd。</mark> K8S 的存储服务。etcd 存储了 K8S 的关键配置和用户配置，**K8S 中仅 API Server 才具备读写权限，其他组件必须通过 API Server 的接口才能读写数据（见 Kubernetes Works Like an Operating System）。**

### 1.3.3 Worker Node 组件

- **Kublet**: **Worker Node 的监视器，以及与 Master Node 的通讯器**
- **Kube-Proxy: K8S 的网络代理**
- **Container Runtime: Worker Node 的运行环境**
- **Logging Layer: K8S 的监控状态收集器**
- **Add-Ones: K8S 管理运维 Worker Node 的插件组件**

接着来看**Worker Node **的组件，笔者更赞同 HOW DO APPLICATIONS RUN ON KUBERNETES 文章中提到的组件介绍：

<mark style="background: #FF5582A6;">Kubelet</mark> 。Worker Node 的监视器，以及与 Master Node 的通讯器。Kubelet 是 Master Node 安插在 Worker Node 上的“眼线”，它会定期向 Worker Node 汇报自己 Node 上运行的服务的状态，并接受来自 Master Node 的指示采取调整措施。 <mark style="background: #FF5582A6;">Kube-Proxy</mark> 。K8S 的网络代理。私以为称呼为 Network-Proxy 可能更适合？Kube-Proxy 负责 Node 在 K8S 的网络通讯、以及对外部网络流量的负载均衡。 <mark style="background: #FF5582A6;">Container Runtime</mark> 。Worker Node 的运行环境。即安装了容器化所需的软件环境确保容器化程序能够跑起来，比如 Docker Engine。大白话就是帮忙装好了 Docker 运行环境。 <mark style="background: #FF5582A6;">Logging Layer</mark> 。K8S 的监控状态收集器。私以为称呼为 Monitor 可能更合适？Logging Layer 负责采集 Node 上所有服务的 CPU、内存、磁盘、网络等监控项信息。 <mark style="background: #FF5582A6;">Add-Ons</mark> 。K8S 管理运维 Worker Node 的插件组件。有些文章认为 Worker Node 只有三大组件，不包含 Add-On，但笔者认为 K8S 系统提供了 Add-On 机制，让用户可以扩展更多定制化功能，是很不错的亮点。

### 1.3.4 总结

总结来看，

- K8S 的 Master Node 具备：
  - 请求入口管理（API Server）
  - Worker Node 调度（Scheduler）
  - 监控和自动调节（Controller Manager）
  - 以及存储功能（etcd）
- 而 K8S 的 Worker Node 具备：
  - 状态和监控收集（Kubelet）
  - 网络和负载均衡（Kube-Proxy）
  - 保障容器化运行环境（Container Runtime）
  - 以及定制化功能（Add-Ons）

到这里，相信你已经对 K8S 究竟是做什么的，有了大概认识。接下来，再来认识下**K8S 的 Deployment、Pod、Replica Set、Service **等，但凡谈到 K8S，就绕不开这些名词，而这些名词也是最让 K8S 新手们感到头疼、困惑的。

# II. K8S 重要概念

## 2.1 Pod 实例

官方对于 Pod 的解释是：

- **Pod 是可以在 Kubernetes 中创建和管理的、最小的可部署的计算单元**

这样的解释还是很难让人明白究竟 Pod 是什么，但是对于 K8S 而言，Pod 可以说是所有对象中最重要的概念了！因此，我们必须首先清楚地知道“Pod 是什么”，再去了解其他的对象。
从官方给出的定义，联想下“最小的 xxx 单元”，是不是可以想到本科在学校里学习“进程”的时候，教科书上有一段类似的描述：资源分配的最小单位；还有”线程“的描述是：CPU 调度的最小单位。什么意思呢？**”最小 xx 单位“要么就是事物的衡量标准单位，要么就是资源的闭包、集合。** 前者比如长度米、时间秒；后者比如一个”进程“是存储和计算的闭包，一个”线程“是 CPU 资源（包括寄存器、ALU 等）的闭包。 <mark style="background: #FFF3A3A6;">同样的，Pod 就是 K8S 中一个服务的闭包。</mark> 这么说的好像还是有点玄乎，更加云里雾里了。**简单来说，Pod 可以被理解成一群可以共享网络、存储和计算资源的容器化服务的集合。** <mark style="background: #BBFABBA6;">再打个形象的比喻，在同一个 Pod 里的几个 Docker 服务/程序，好像被部署在同一台机器上，可以通过 localhost 互相访问，并且可以共用 Pod 里的存储资源（这里是指 Docker 可以挂载 Pod 内的数据卷，数据卷的概念，后文会详细讲述，暂时理解为“需要手动 mount 的磁盘”）。</mark>
笔者总结 Pod 如下图，可以看到：**同一个 Pod 之间的 Container 可以通过 localhost 互相访问，并且可以挂载 Pod 内所有的数据卷；但是不同的 Pod 之间的 Container 不能用 localhost 访问，也不能挂载其他 Pod 的数据卷。**

![](https://pic1.zhimg.com/80/v2-ceb8fc9f98f4ce1f83f9962edc2bd804_720w.jpg)

对 Pod 有直观的认识之后，接着来看 K8S 中 Pod 究竟长什么样子，具体包括哪些资源？

K8S 中所有的对象都通过 yaml 来表示，笔者从官方网站摘录了一个最简单的 Pod 的 yaml：

![[FigureBed 🌄/Pasted/Pasted image 20220529120203.png]]

看不懂不必慌张，且耐心听下面的解释：

- `apiVersion`: 记录 K8S 的 API Server 版本，现在看到的都是 v1，用户不用管。
- `kind`: 记录该 yaml 的对象，比如这是一份 Pod 的 yaml 配置文件，那么值内容就是 Pod。
- `metadata`:记录了 Pod 自身的元数据，比如这个 Pod 的名字、这个 Pod 属于哪个 namespace（命名空间的概念，后文会详述，暂时理解为“<mark style="background: #FFF3A3A6;">同一个命名空间内的对象互相可见</mark> ”）。
- `spec` 记录了 Pod 内部所有的资源的详细信息，看懂这个很重要：
  - `containers` 记录了 Pod 内的容器信息，containers 包括了：
    - `name` : 容器名
    - `image` :容器的镜像地址
    - `resources` : 容器需要的 CPU、内存、GPU 等资源
    - `command` : 容器的入口命令
    - `args` : 容器的入口参数
    - `volumeMounts` 容器要挂载的 Pod 数据卷等。可以看到，上述这些信息都是启动容器的必要和必需的信息。
  - `volumes` 记录了 Pod 内的数据卷信息，后文会详细介绍 Pod 的数据卷。

## 2.2 Volume 数据卷

- <mark style="background: #FFF3A3A6;">数据卷 volume 是 Pod 内部的磁盘资源。</mark> 是 K8S 的对象，对应一个实体的数据卷
- volumeMounts 只是 container 的挂载点，<mark style="background: #FFF3A3A6;">对应 container 的其中一个参数。</mark>
- volumeMounts 依赖于 volume，只有当 Pod 内有 volume 资源的时候，该 Pod 内部的 container 才可能有 volumeMounts。

K8S 支持很多类型的 volume 数据卷挂载，具体请参见 K8S 卷。
前文就“如何理解 volume”提到：“需要手动 mount 的磁盘”，此外，有一点可以帮助理解：<mark style="background: #FFF3A3A6;">数据卷 volume 是 Pod 内部的磁盘资源。</mark>

其实，单单就 Volume 来说，不难理解。但是上面还看到了 volumeMounts，这俩是什么关系呢？
volume 是 K8S 的对象，对应一个实体的数据卷；而 volumeMounts 只是 container 的挂载点，对应 container 的其中一个参数。
但是，volumeMounts 依赖于 volume，只有当 Pod 内有 volume 资源的时候，该 Pod 内部的 container 才可能有 volumeMounts。

## 2.3 Container 容器

本文中提到的镜像 Image、容器 Container，都指代了 Pod 下的一个 container。关于 K8S 中的容器，在 2.1Pod 章节都已经交代了，这里无非再啰嗦一句：一个 Pod 内可以有多个容器 container。

在 Pod 中，容器也有分类，对这个感兴趣的同学欢迎自行阅读更多资料：

- 标准容器 Application Container。
- 初始化容器 Init Container。
- 边车容器 Sidecar Container。
- 临时容器 Ephemeral Container。
- **一般来说，我们部署的大多是标准容器（ Application Container）。**

## 2.4 Deployment 和 ReplicaSet（简称 RS）

- **Deployment、ReplicationController 和 ReplicaSet 主要管控 Pod 程序服务**
- **Deployment ：Deployment 的作用是管理和控制 Pod 和 ReplicaSet，管控它们运行在用户期望的状态中。**
- **ReplicaSets： ReplicaSet 的作用就是管理和控制 Pod，管控他们好好干活。**

除了 Pod 之外，K8S 中最常听到的另一个对象就是 Deployment 了。那么，什么是 Deployment 呢？官方给出了一个要命的解释： <font color = 'grey'>一个 Deployment 控制器为 Pods 和 ReplicaSets 提供声明式的更新能力。
你负责描述 Deployment 中的 目标状态，而 Deployment 控制器以受控速率更改实际状态， 使其变为期望状态。你可以定义 Deployment 以创建新的 ReplicaSet，或删除现有 Deployment，并通过新的 Deployment 收养其资源。</font>

翻译一下：**Deployment 的作用是管理和控制 Pod 和 ReplicaSet，管控它们运行在用户期望的状态中。** 哎，打个形象的比喻，Deployment 就是包工头，主要负责监督底下的工人 Pod 干活，确保每时每刻有用户要求数量的 Pod 在工作。如果一旦发现某个工人 Pod 不行了，就赶紧新拉一个 Pod 过来替换它。

新的问题又来了：那什么是 ReplicaSets 呢？ <font color = 'grey'>ReplicaSet 的目的是维护一组在任何时候都处于运行状态的 Pod 副本的稳定集合。 因此，它通常用来保证给定数量的、完全相同的 Pod 的可用性。</font>

再来翻译下：**ReplicaSet 的作用就是管理和控制 Pod，管控他们好好干活。** 但是，ReplicaSet 受控于 Deployment。形象来说，ReplicaSet 就是总包工头手下的小包工头。

![[FigureBed 🌄/Pasted/Pasted image 20220529215541.png]]
但是，从 K8S 使用者角度来看，用户会直接操作 Deployment 部署服务，而当 Deployment 被部署的时候，K8S 会自动生成要求的 ReplicaSet 和 Pod。

- **在 K8S 官方文档中也指出用户只需要关心 Deployment 而不操心 ReplicaSet：**

<font color = 'grey'> This actually means that you may never need to manipulate ReplicaSet objects: use a Deployment instead, and define your application in the spec section.这实际上意味着您可能永远不需要操作 ReplicaSet 对象：直接使用 Deployments 并在规范部分定义应用程序。</font>
**补充说明：在 K8S 中还有一个对象 --- ReplicationController（简称 RC），官方文档对它的定义是：** <font color = 'grey'>ReplicationController 确保在任何时候都有特定数量的 Pod 副本处于运行状态。 换句话说，ReplicationController 确保一个 Pod 或一组同类的 Pod 总是可用的。</font>

怎么样，和 ReplicaSet 是不是很相近？在 Deployments, ReplicaSets, and pods 教程中说“ReplicationController 是 ReplicaSet 的前身”，官方也推荐用 Deployment 取代 ReplicationController 来部署服务。

## 2.5 Service 和 Ingress

- ![](https://pic4.zhimg.com/v2-5f199950c6a1a5cbeb5d098b5d3bd357_r.jpg)

吐槽下 K8S 的概念/对象/资源是真的多啊！前文介绍的 Deployment、ReplicationController 和 ReplicaSet 主要管控 Pod 程序服务；
那么，

- <mark style="background: #FFF3A3A6;">Service 和 Ingress 则负责管控 Pod 网络服务。</mark>
- K8S 中的服务（Service）并不是我们常说的“服务”的含义，<mark style="background: #FFF3A3A6;">而更像是网关层，是若干个 Pod 的流量入口、流量均衡器。</mark>

我们先来看看官方文档中 Service 的定义： <font color = 'grey'> 将运行在一组 Pods 上的应用程序公开为网络服务的抽象方法。
使用 Kubernetes，您无需修改应用程序即可使用不熟悉的服务发现机制。 Kubernetes 为 Pods 提供自己的 IP 地址，并为一组 Pod 提供相同的 DNS 名， 并且可以在它们之间进行负载均衡。</font>
翻译下：K8S 中的服务（Service）并不是我们常说的“服务”的含义，而更像是网关层，是若干个 Pod 的流量入口、流量均衡器。

### 为什么要 Service 呢？

私以为在这一点上，官方文档讲解地非常清楚： <font color = 'grey'>Kubernetes Pod 是有生命周期的。 它们可以被创建，而且销毁之后不会再启动。 如果您使用 Deployment 来运行您的应用程序，则它可以动态创建和销毁 Pod。</font> <font color = 'grey'>每个 Pod 都有自己的 IP 地址，但是在 Deployment 中，在同一时刻运行的 Pod 集合可能与稍后运行该应用程序的 Pod 集合不同。</font> <font color = 'grey'>这导致了一个问题： 如果一组 Pod（称为“后端”）为群集内的其他 Pod（称为“前端”）提供功能， 那么前端如何找出并跟踪要连接的 IP 地址，以便前端可以使用工作量的后端部分？</font>

补充说明：K8S 集群的网络管理和拓扑也有特别的设计，以后会专门出一章节来详细介绍 K8S 中的网络。这里需要清楚一点：K8S 集群内的每一个 Pod 都有自己的 IP（是不是很类似一个 Pod 就是一台服务器，然而事实上是多个 Pod 存在于一台服务器上，只不过是 K8S 做了网络隔离），在 K8S 集群内部还有 DNS 等网络服务（一个 K8S 集群就如同管理了多区域的服务器，可以做复杂的网络拓扑）。

此外，笔者推荐 [k8s 外网如何访问业务应用](https://www.jianshu.com/p/50b930fa7ca3) 对于Service的介绍，不过对于新手而言，推荐阅读前半部分对于service的介绍即可，后半部分就太复杂了。我这里做了简单的总结： <mark style="background: #FFF3A3A6;">Service是K8S服务的核心，屏蔽了服务细节，统一对外暴露服务接口，真正做到了“微服务”。</mark> 举个例子，我们的一个服务A，部署了3个备份，也就是3个Pod；对于用户来说，只需要关注一个Service的入口就可以，而不需要操心究竟应该请求哪一个Pod。优势非常明显：一方面<mark style="background: #BBFABBA6;">外部用户不需要感知因为Pod上服务的意外崩溃、K8S重新拉起Pod而造成的IP变更，外部用户也不需要感知因升级、变更服务带来的Pod替换而造成的IP变化</mark> ，另一方面，Service还可以做<mark style="background: #BBFABBA6;">流量负载均衡</mark> 。

但是，Service 主要负责 K8S 集群内部的网络拓扑。那么集群外部怎么访问集群内部呢？这个时候就需要 Ingress 了，官方文档中的解释是： <font color = 'grey'>Ingress 是对集群中服务的外部访问进行管理的 API 对象，典型的访问方式是 HTTP。</font> <font color = 'grey'>Ingress 可以提供负载均衡、SSL 终结和基于名称的虚拟托管。</font>

翻译一下：Ingress 是整个 K8S 集群的接入层，复杂集群内外通讯。

## 2.6 namespace 命名空间

- <mark style="background: #FFF3A3A6;">namespace 是为了把一个 K8S 集群划分为若干个资源不可共享的虚拟集群而诞生的。</mark>
- 也就是说，<mark style="background: #FFF3A3A6;">可以通过在 K8S 集群内创建 namespace 来分隔资源和对象。</mark>

和前文介绍的所有的概念都不一样，namespace 跟 Pod 没有直接关系，而是 K8S 另一个维度的对象。或者说，前文提到的概念都是为了服务 Pod 的，而 namespace 则是为了服务整个 K8S 集群的。
那么，namespace 是什么呢？
上官方文档定义： <font color = 'grey'>Kubernetes 支持多个虚拟集群，它们底层依赖于同一个物理集群。 这些虚拟集群被称为名字空间。 </font>
翻译一下：<mark style="background: #FFF3A3A6;">namespace 是为了把一个 K8S 集群划分为若干个资源不可共享的虚拟集群而诞生的。</mark>

也就是说，<mark style="background: #FFF3A3A6;">可以通过在 K8S 集群内创建 namespace 来分隔资源和对象。</mark>

比如我有 2 个业务 A 和 B，那么我可以创建 ns-a 和 ns-b 分别部署业务 A 和 B 的服务，如在 ns-a 中部署了一个<mark style="background: #FF5582A6;">deployment</mark> ，名字是 hello，返回用户的是“hello a”；在 ns-b 中也部署了一个 deployment，名字恰巧也是 hello，返回用户的是“hello b”<mark style="background: #FF5582A6;">（要知道，在同一个 namespace 下 deployment 不能同名；但是不同 namespace 之间没有影响）</mark> 。前文提到的所有对象，都是在 namespace 下的；当然，也有一些对象是不隶属于 namespace 的，而是在 K8S 集群内全局可见的，官方文档提到的可以通过命令来查看，具体命令的使用办法，笔者会出后续的实战文章来介绍，先贴下命令：

```shell
# 位于名字空间中的资源
kubectl api-resources --namespaced=true

# 不在名字空间中的资源
kubectl api-resources --namespaced=false
```

![](https://pic2.zhimg.com/80/v2-c150dfbe556f78f67b5abc945458278d_720w.jpg)

在 namespace 下的对象有（部分）：

![](https://pic1.zhimg.com/80/v2-fcbe57f895ad2c7331f0efde0e96cff4_720w.jpg)

## 2.7 其他

K8S 的对象实在太多了，2.1-2.6 介绍的是在实际使用 K8S 部署服务最常见的。其他的还有 Job、CronJob 等等，在对 K8S 有了比较清楚的认知之后，再去学习更多的 K8S 对象，不是难事。

# 写在后面

本文是 K8S 系列文章第一篇，希望能够帮助对 K8S 不了解的新手快速了解 K8S。如果文章中有纰漏，非常欢迎留言或者私信指出；有理解错误的地方，更是欢迎留言或者私信告知。

笔者一边写文章，一边查阅和整理 K8S 资料，过程中越发感觉 K8S 架构的完备、设计的精妙，是值得深入研究的，K8S 大受欢迎是有道理的！再次感叹下。
