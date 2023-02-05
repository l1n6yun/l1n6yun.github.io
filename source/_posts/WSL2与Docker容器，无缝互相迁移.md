title: WSL2与Docker容器，无缝互相迁移
author: l1n6yun
tags: []
categories: []
date: 2022-04-04 17:23:54
---
>  本文分享自华为云社区《[WSL2与Docker容器，无缝互相迁移](https://bbs.huaweicloud.com/blogs/336923)》，作者： tsjsdbd 。

 注：本文提到的WSL都是指WSL2

# WSL与Docker

 WSL非常像windows版的Docker，可以直接启动“容器”（故意加引号，下面有解释），并且在容器世界里面，可以执行各种Linux操作。像下面这样，是不是和Docker很像？


![upload successful](/images/pasted-43.png)

 ps：上面提到的“容器”，实际是安全容器（即：虚机），WSL2内部架构如下：


![upload successful](/images/pasted-44.png)

虽WSL和Docker很像，但是它对WSL镜像有要求，就是得从 MS的应用商店下载：

![upload successful](/images/pasted-45.png)

这个就稍显不那么方便，毕竟你可能已经有很多现成的Docker容器了，这里又得重新安装一遍。

我是Windows上有WSL，我的Linux开发机上有Docker，那我怎么在WSL里面跑Docker呢？

# WSL运行Docker容器

常见的WSL里面运行Docker，是这样子的：


![upload successful](/images/pasted-46.png)

就是把WSL当做一台新的开发机，然后在WSL里面，安装Docker软件。

这样子，也能解决问题。就是稍微麻烦了一点点。那能不能WSL直接运行Docker镜像呢？

答案是可以的：

参考：https://docs.microsoft.com/en-us/windows/wsl/use-custom-distro



这种方法下，是类似这个样子的：


![upload successful](/images/pasted-47.png)

具体操作如下：

1. 在Docker里面，将容器导出来

```
//查看正在运行的容器

docker ps

//根据容器ID，导出镜像包

docker export $ContainerID > ./tsjsdbd.tar
```

1. 然后再WSL里面，将Docker镜像导入：

```
wsl --import <DistributionName> <InstallLocation> <FileName>
```

## 示例

比如，我这里用Docker运行了一个busybox容器：

```
docker run -it busybox /bin/sh
```

然后查询容器ID：

```
docker ps
CONTAINER ID  IMAGE
c1e9e8f77336  busybox
```

导出：

```
docker export c1e9e8f77336 > tsjsdbd_busybox.tar
```

然后我把这个 镜像文件，拷贝到我的windows电脑上。

并在wsl里面导入：

```
wsl --import tsjsdbd_busybox ./busybox ./tsjsdbd_busybox.tar
```

导入后查看：

```
wsl -l
```

这时，我启动这个 busybox 镜像。

```
wsl -d tsjsdbd_busybox
```

OK，这时我已经在WSL容器里面了，这是一个busybox的Docker容器镜像。

# Docker运行WSL镜像

从上面的操作可以看出来，WSL和Docker的镜像是相通的。所以WSL系统，也可以导出给Docker直接运行。类似这个样子：

![upload successful](/images/pasted-48.png)

 具体操作如下：

先查看下当前跑了哪些wsl容器：

```
wsl -l -v
 NAME        STATE      VERSION
* Ubuntu-18.04    Stopped     2
  tsjsdbd_busybox  Running     2
```

导出指定的wsl镜像

```
wsl --export tsjsdbd_busybox ./mybox.tar
```

其中 “*tsjsdbd_busybox*” 就是你希望导给Docker运行的WSL容器（里面可能安装了一些你需要的软件）。导出的tar包，就可以看做是WSL镜像了（可以直接导入给Docker）

最后，在Docker里面，导入这个镜像：

```
docker import - mybox < mybox.tar
```

可以查询此镜像

```
docker images |grep mybox
```

并启动

```
docker run -it mybox /bin/sh
```

# WSL镜像与Docker镜像

大体上，我给个示意：


![upload successful](/images/pasted-49.png)

所以WSL确实挺香的。

# WSL启动GUI界面

整体方案是利用 X11 Server，原理参考：

《Docker运行带UI界面的应用，并将它的界面投射到你的Windows电脑》

https://bbs.huaweicloud.com/blogs/281862


![upload successful](/images/pasted-50.png)

X11 Server，一般网上推荐 VcXsrv，我自己用下来，感觉 MobaXterm 更傻瓜一些。所以我都用 MobaXterm 的。

# 附：安装WSL

## 1. 系统要求

Windows 10，版本 2004 以上。

比如我的是 20H2，是OK的。

点击：开始-设置-关于，查询自己的版本

## 2. 判断wsl2是否已有

打开 power shell，输入 

```
wsl -l -o
```

如OK，则不用后续步骤了（说明你的windows版本已经比较高）。

不行，则手动执行后续步骤

## 3. 允许开发者模式

![upload successful](/images/pasted-51.png)

## 4. 启动WSL2功能

启用WSL2

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

启用虚拟机平台

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

启用Hyper-V

```
dism.exe /online /enable-feature /featurename:Microsoft-Hyper-V /all /norestart
```

设置WSL2为默认

```
wsl --set-default-version 2
```

## 5. 重启，安装wsl补丁

```
wsl_update_x64.msi
```

执行以上补丁包。

补丁包下载地址：

https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

# 附：高阶版 GPU，GUI界面

Windows版本高的（win10 21H2），可以在WSL2里面跑CUDA

https://docs.microsoft.com/en-us/windows/ai/directml/gpu-cuda-in-wsl

再高一点（win11），可以原生支持带GUI界面的Linux程序。

https://docs.microsoft.com/en-us/windows/wsl/tutorials/gui-apps