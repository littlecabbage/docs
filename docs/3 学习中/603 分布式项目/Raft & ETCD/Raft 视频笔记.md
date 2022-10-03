---
Author: sync
date: 2022-06-13 19:12 Monday
tag: Raft
---

[# 解读共识算法Raft（合集）](https://www.bilibili.com/video/BV1pr4y1b7H5?spm_id_from=333.337.search-card.all.click&vd_source=ab2f866858f0016a32f6db0daf3438df)

# 0. 什么是raft

[00:49](https://www.bilibili.com/video/BV1pr4y1b7H5?spm_id_from=333.337.search-card.all.click&vd_source=ab2f866858f0016a32f6db0daf3438df#t=49.30999)

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220626111840.png)

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220626112016.png)


# 1. 复制状态机

[03:53](https://www.bilibili.com/video/BV1pr4y1b7H5?spm_id_from=333.337.search-card.all.click&vd_source=ab2f866858f0016a32f6db0daf3438df#t=233.422752)

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220626112422.png)

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220626112507.png)

# 2. 状态简化

[05:22](https://www.bilibili.com/video/BV1pr4y1b7H5?spm_id_from=333.337.search-card.all.click&vd_source=ab2f866858f0016a32f6db0daf3438df#t=322.671514)

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220626112626.png)

## 2.1 任期

[06:34](https://www.bilibili.com/video/BV1pr4y1b7H5?spm_id_from=333.337.search-card.all.click&vd_source=ab2f866858f0016a32f6db0daf3438df#t=394.76208)

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220626112742.png)

<mark style="background: #FFF3A3A6;">可以通过判断一台服务是否包含`t2`时刻的日志来判断这台服务器在 `t2`时刻内是否发生过宕机</mark> 

## 2.2 RPC 与新设计的功能 

[07:26](https://www.bilibili.com/video/BV1pr4y1b7H5?spm_id_from=333.337.search-card.all.click&vd_source=ab2f866858f0016a32f6db0daf3438df#t=446.435664)

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220626112947.png)

# 3. 领导者选举

[09:05](https://www.bilibili.com/video/BV1pr4y1b7H5?spm_id_from=333.337.search-card.all.click&vd_source=ab2f866858f0016a32f6db0daf3438df#t=545.265481)

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220626114724.png)

## 3.1 领导者选举可能产生的3种结果

[10:08](https://www.bilibili.com/video/BV1pr4y1b7H5?spm_id_from=333.337.search-card.all.click&vd_source=ab2f866858f0016a32f6db0daf3438df#t=608.531184)

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220626115334.png)

