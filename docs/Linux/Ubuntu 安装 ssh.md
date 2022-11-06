

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.schwarzeni.com](https://blog.schwarzeni.com/2018/12/24/%E4%BD%BF%E7%94%A8ssh%E7%99%BB%E9%99%86Parallels%E7%9A%84Ubuntu%E8%99%9A%E6%8B%9F%E6%9C%BA/)

> 彻底告别图形界面！

彻底告别图形界面！

## 环境
- MacOS v10.14
- Parallels Desktop v13.2
- Ubuntu v17.04
首先需要知道本机的 ip 地址，需要使用到`ifconfig`工具
```
# 首先下载相应的工具包
sudo apt-get install net-tools

# 输入命令查看
ifconfig
```

应该就在前面几行就会有一个 IP 地址，我的是 10.211.55.24
![[Pasted image 20221106114516.png]]

ok，现在就可以在本地终端通过 ssh 进行登录了

```
#   用户名         IP地址
ssh schwarzeni@10.211.55.24
```

## 一个问题

如果碰到了如下的报错

```
port 22: Connection refused
```

解决方案如下
首先，这需要在虚拟机的终端内操作
输入如下命令即可

```
sudo apt-get update
sudo apt-get install openssh-server
sudo ufw allow 22
```

## 参考链接

- [Why am I getting a “port 22: Connection refused” error?](https://askubuntu.com/questions/218344/why-am-i-getting-a-port-22-connection-refused-error)
- [Ubuntu17.04 查看本机 IP](https://blog.csdn.net/phenomenonsTell/article/details/78308544)
