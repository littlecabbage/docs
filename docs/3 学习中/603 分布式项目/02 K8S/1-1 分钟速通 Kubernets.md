# 0. 其他
[kubernetes中文文档](https://kubernetes.io/zh/docs/setup/)
## 0.1 进度
###  第一天：  5月20日
- 学习视频
	- 1 - (1, 2) 前世今生
	- 1 - (3,4) 知识图谱
	- 1 - (5, 6) 组件说明
	- 2- (1, 2 ) pod 概念
	- 2 - (3) 网络通讯方式
- 阅读技术文档



# 1. 5月20 Kubernetes 组件 （视频1-6）

![[FigureBed 🌄/Pasted/Pasted image 20220520150356.png]]

##  Master 组件
apiserver ： 所有服务访问的统一入口
controllerManager： 维持副本期望数目
Scheduler： 负责接受任务，选择合适的节点进行分配任务
etcd: 键值对， 存储K8S集群所有重要信息（持久化： 需要持久化的数据会写到etcd中）

---
### 插件
coreDNS： 可以为集群中的SVC创建一个域名IP的对应关系解析
DASHBOARD： 给K8S提供一个 B/S结构的访问体系


## Node组件
Kubelet：直接跟容器引擎交互，实现容器的生命周期管理
kubeproxy： 负责写入规则至IPTABLES， IPVS 实现服务访问映射

# 2 POD 概念

![[FigureBed 🌄/Pasted/Pasted image 20220521151415.png]]


![[FigureBed 🌄/Pasted/Pasted image 20220521151502.png]]


![[FigureBed 🌄/Pasted/Pasted image 20220521152124.png]]

![[FigureBed 🌄/Pasted/Pasted image 20220521152452.png]]

![[FigureBed 🌄/Pasted/Pasted image 20220521152638.png]]

![[FigureBed 🌄/Pasted/Pasted image 20220521152748.png]]