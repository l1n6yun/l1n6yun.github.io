title: Docker 国内镜像源配置指南
author: l1n6yun
date: 2024-11-30 15:16:30
tags:
---
> 随着 Docker 的广泛应用，越来越多的开发者和企业开始使用 Docker 来构建和部署应用。然而，由于网络原因，直接从 Docker Hub 拉取镜像可能会遇到速度慢或者不稳定的问题。为了解决这一问题，本文将介绍如何配置国内镜像源，以加速 Docker 镜像的拉取速度。

## 为什么需要配置国内镜像源？

直接从 Docker Hub 拉取镜像可能会受到网络限制的影响，导致速度慢或者失败。配置国内镜像源可以有效地解决这一问题，提高镜像拉取的速度和稳定性。

## 配置步骤

以下是配置国内 Docker 镜像源的具体步骤：

### 1. 创建或修改 Docker 配置文件

在 Linux 系统中，你需要修改或创建 `/etc/docker/daemon.json` 文件。如果文件不存在，你可以使用以下命令创建它：

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "https://docker.1ms.run",
        "https://doublezonline.cloud",
        "https://dislabaiot.xyz",
        "https://docker.fxxk.dedyn.io",
        "https://dockerpull.org",
        "https://docker.unsee.tech",
        "https://hub.rat.dev",
        "https://docker.1panel.live",
        "https://docker.nastool.de",
        "https://docker.zhai.cm",
        "https://docker.5z5f.com",
        "https://a.ussh.net",
        "https://docker.udayun.com",
        "https://hub.geekery.cn"
    ]
}
EOF
```

### 2. 重启 Docker 服务

修改配置文件后，需要重启 Docker 服务以使配置生效：

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

#### 3. 验证配置是否成功

使用以下命令检查 Docker 信息，确认镜像源是否已经更改：

```bash
docker info
```

以上步骤可以帮助你配置国内镜像源，以加速 Docker 镜像的拉取速度。请根据实际情况选择可用的镜像源进行配置。这些镜像源均来自最新的搜索结果，确保了时效性。