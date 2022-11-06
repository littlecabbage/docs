---
Author: sync
date: 2022-11-06 22:12 Sunday
tag: 杂/Parralles
---

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [luoxx.top](https://luoxx.top/archives/pd-18-active)

> 破解方法来自 github 上一个大佬 somebasj ，不过直接用他提供的脚本激活基本没法成功，所以我这边出个教程帮助大家成功激活。

> 破解方法来自 github 上一个大佬 [somebasj](https://github.com/somebasj/ParallelsDesktopCrack) ，不过直接用他提供的脚本激活基本没法成功，所以我这边出个教程帮助大家成功激活。

> 按照 github 上的描述，这个激活方式是全面支持 intel 和 arm 芯片的，不过我这边只有 m1 的 mac，所以 intel 芯片的激活未测试。

## 准备工作

1. 下载 pd\
   官网下载地址： <https://download.parallels.com/desktop/v18/18.0.1-53056/ParallelsDesktop-18.0.1-53056.dmg>\
   我也上传了一份安装包到云盘，也可以在这里下载（默认网盘是我自己建的网盘，速度不会很快，有百度网盘会员的可以用百度网盘链接下载）。\
   ParallelsDesktop-18.0.1-53056.dmg 来源：默认网盘 | 提取码：w1ziln[](https://pan.luoxx.top/s/mzTW?password=w1ziln)\
   ParallelsDesktop-18.0.1-53056.dmg 来源：百度网盘 | 提取码：v61h[](https://pan.baidu.com/s/1KteyiOdYAG8KRwk3o6pxeA?pwd=v61h)

## 激活方式 1

> github 作者更新了激活脚本，应该是可以直接一键激活了，大家可以先试一下，不行再使用 `激活方式2`

- 新的破解补丁下载地址\
  ParallelsDesktopCrack-main.zip 来源：默认网盘 | 提取码：fpe9z1[](https://pan.luoxx.top/s/Gyhg?password=fpe9z1)
- 使用方法\
  下载后解压，然后 cd 进入解压后的目录，然后执行 `chmod +x ./install.sh && sudo ./install.sh` 命令即可。\
  ps：执行该命令会需要输入密码以授权。

## 激活方式 2

- 下载破解补丁\
  为了方便无法访问 github 的同学下载，我这里也上传了一份到云盘\
  ParallelsDesktop_18.0.1.53056_Patch.zip 来源：默认网盘 | 提取码：ea5jdf[](https://pan.luoxx.top/s/ZYuW?password=ea5jdf)

  ParallelsDesktop_18.0.1.53056_Patch.zip 来源：默认网盘 | 提取码：ea5jdf[](https://pan.luoxx.top/s/ZYuW?password=ea5jdf)

  ParallelsDesktop_18.0.1.53056_Patch.zip 来源：默认网盘 | 提取码：ea5jdf[](https://pan.luoxx.top/s/ZYuW?password=ea5jdf)

  ParallelsDesktop_18.0.1.53056_Patch.zip 来源：默认网盘 | 提取码：ea5jdf[](https://pan.luoxx.top/s/ZYuW?password=ea5jdf)

  ParallelsDesktop_18.0.1.53056_Patch.zip

  来源：默认网盘 | 提取码：ea5jdf

  [](https://pan.luoxx.top/s/ZYuW?password=ea5jdf)

- 如果安装过 pd17 或者更早版本，可以完全卸载以确保之后能成功激活。（可选，不卸载也行，不过可能会有坑）\
  建议使用 [App Cleaner](https://macwk.com/soft/app-cleaner-and-uninstaller-pro) 来卸载，这样卸载的比较干净\
  ps：**卸载之前请先备份好自己的虚拟机**，不然卸载完就啥都没了，虚拟机文件存放目录为 `~/Parallels`。 就是如下图这样的一个文件，复制一份到其他地方保存好。\
  ![](https://cdn.luoxx.top/halo/image-1662517049731.png)

- 下载 pd18 安装文件，安装，安装之后到激活那一步就不用继续走了，退出 pd。

- 下载破解补丁到 `下载目录` ，也就是 `~/Downloads`, 然后解压缩，不要修改解压后的文件夹名称，这样操作都是为了保障后续执行脚本路径正确。

- 打开终端，开始执行命令破解，作者提供的一键破解脚本有点问题，所以 `install.sh` 文件忽略即可，我们手动执行命令破解

1. 进入破解补丁的目录
2. 杀掉 pd 的进程
3. 复制破解补丁文件到 pd 目录 (执行命令时提示要输入密码的话就输入自己电脑的密码然后回车)
4. 签名 (执行命令时提示要输入密码的话就输入自己电脑的密码然后回车)
5. 创建并编辑激活秘钥文件 (执行命令时提示要输入密码的话就输入自己电脑的密码然后回车)

- 至此，破解已完成，再次打开 pd 就不会弹出激活弹出了。\
  ps：恢复之前备份的虚拟机，只要在创建虚拟机的流程那里选择打开，然后选择之前备份的虚拟机文件即可。

## 激活方式 3

> 由于前两种激活方式有一小部分朋友的机器上会有各种各样的报错情况，所以这边又加上了一种更直接的方式。\
> tnt 团队也出了一个这个版本的破解程序，也是用的这个 github 大佬提供的资源。\
> 如果前两种方式行不通的，可以试一试。

下载地址：

- tnt 网站地址：<https://appstorrent.ru/61-parallels-desktop.html> （需要科学上网才能访问）

- Parallels Desktop 18.0.1-53056 Intel for macOS 11+\
  ParallelsDesktop-18_0_1-53056_by_Day.dmg 来源：百度网盘 | 提取码：36hi[](https://pan.baidu.com/s/14Raub9nSoWWwK-tL7P3Vsw?pwd=36hi)

  ParallelsDesktop-18_0_1-53056_by_Day.dmg 来源：百度网盘 | 提取码：36hi[](https://pan.baidu.com/s/14Raub9nSoWWwK-tL7P3Vsw?pwd=36hi)

  ParallelsDesktop-18_0_1-53056_by_Day.dmg 来源：百度网盘 | 提取码：36hi[](https://pan.baidu.com/s/14Raub9nSoWWwK-tL7P3Vsw?pwd=36hi)

  ParallelsDesktop-18_0_1-53056_by_Day.dmg 来源：百度网盘 | 提取码：36hi[](https://pan.baidu.com/s/14Raub9nSoWWwK-tL7P3Vsw?pwd=36hi)

  ParallelsDesktop-18_0_1-53056_by_Day.dmg

  来源：百度网盘 | 提取码：36hi

  [](https://pan.baidu.com/s/14Raub9nSoWWwK-tL7P3Vsw?pwd=36hi)

- Parallels Desktop 18.0.1-53056 U2B\
  Parallels_Desktop_18_0_1-53056_U2B.dmg 来源：百度网盘 | 提取码：2r3h[](https://pan.baidu.com/s/11ioZuUl70iWjIZEu8X6Q8A?pwd=2r3h)

  Parallels_Desktop_18_0_1-53056_U2B.dmg 来源：百度网盘 | 提取码：2r3h[](https://pan.baidu.com/s/11ioZuUl70iWjIZEu8X6Q8A?pwd=2r3h)

  Parallels_Desktop_18_0_1-53056_U2B.dmg 来源：百度网盘 | 提取码：2r3h[](https://pan.baidu.com/s/11ioZuUl70iWjIZEu8X6Q8A?pwd=2r3h)

  Parallels_Desktop_18_0_1-53056_U2B.dmg 来源：百度网盘 | 提取码：2r3h[](https://pan.baidu.com/s/11ioZuUl70iWjIZEu8X6Q8A?pwd=2r3h)

  Parallels_Desktop_18_0_1-53056_U2B.dmg

  来源：百度网盘 | 提取码：2r3h

  [](https://pan.baidu.com/s/11ioZuUl70iWjIZEu8X6Q8A?pwd=2r3h)

ps1：tnt 网站上提供了两种安装包，我也不知道 `U2B` 是个啥版本，不过建议 arm 芯片的还是先安装这个版本试试，毕竟另外那个明确写了是 intel 版本\
ps2：建议完全卸载老版本 pd 之后再安装这个版本\
ps3：这种方式博主没试过，不确定是不是百分百 ok

## 其他补充操作

1. 配置 host 屏蔽 pd 检测（以防万一）\
   补丁作者原话：

> Parallels Desktop may upload client info or logs to server.\
> You can use a firewall block there domains.\
> Or use Hosts, AdGuardHome filter DNS resolve.

大概就是说 pd 服务器可能会检测你本地的 pd 状态，最好使用防火墙或者 host 屏蔽掉 pd 的检测，以防哪天破解失效。

我们这边采用配置 host 的方式，比较简单。

编辑 host 文件

在文件最后面加上以下配置

然后 esc + :wq 保存修改即可

使用 clash 代理的时候，会导致 host 配置失效，解决方法为：在 clash 的 yml 配置文件中，修改配置 dns: enable: false。

这样修改之后 host 就能生效了，但是如果 clash 设置了自动更新订阅的话，更新订阅的时候配置文件又会被覆盖掉，着实比较麻烦，没有特别好的解决办法，估计只能自己写一个定时任务，定时修改这个配置了，定时任务我自己写了一个，详见这篇博客 [https://luoxx.top/archives/mac-clash-yml-auto-update。](https://luoxx.top/archives/mac-clash-yml-auto-update%E3%80%82)

- 补充一个新的更方便的防止 host 文件被修改的方法，感谢网友 `ttkanni` 提供的方法

## 激活出错的情况

有一些朋友激活时遇到无权限的问题，我这边始终没法复现，不过遇到报错之后可以尝试一下以下方案

1. 文件赋予最高的权限

2. 给予终端全盘访问权限\
   `设置-安全性与隐私-隐私-完全磁盘访问权限`\
   这个里面勾上终端，没有的话点击左下角加号手动添加一下终端

## 效果展示

![](https://cdn.luoxx.top/halo/image-1662518173883.png)

> 如果你还有其他疑问，可以在下方评论区留言，博主看到会尽快回复。
