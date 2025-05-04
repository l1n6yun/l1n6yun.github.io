title: frp：一款强大的内网穿透代理工具
author: l1n6yun
tags: []
categories: []
date: 2024-12-07 16:05:00
---
> frp（Fast Reverse Proxy）是一款开源的高性能内网穿透代理工具，它允许你将位于NAT或防火墙后面的本地服务器暴露到公网上。frp支持TCP、UDP、HTTP和HTTPS协议，使得内部服务可以通过域名被外部访问。此外，frp还提供了P2P连接模式，进一步增强了其灵活性和可用性。

## 功能特点

frp以其简洁的设计和丰富的功能而闻名，以下是一些核心特性：

1. **协议支持**：支持TCP、UDP、HTTP和HTTPS协议，适用于多种网络环境。
2. **P2P连接**：提供P2P连接模式，实现客户端间的直接通信，无需经过服务器。
3. **配置灵活**：支持TOML、YAML和JSON格式的配置文件，方便不同用户的需求。
4. **安全性**：支持Token和OIDC认证，增强连接的安全性。
5. **性能优化**：提供TLS加密和数据压缩，保障数据传输的安全和效率。
6. **监控与日志**：集成Prometheus监控，支持实时监控代理状态和流量。
7. **负载均衡**：支持通过分组实现简单的负载均衡。
8. **健康检查**：提供TCP和HTTP健康检查，确保服务的高可用性。
9. **自定义域名**：支持自定义子域名，方便在共享服务器上区分不同用户的服务。
10. **插件系统**：支持客户端和服务器端的插件扩展，增加了功能的灵活性和多样性。

## 安装

### 步骤1：访问GitHub Releases页面

打开浏览器，访问frp的[GitHub Releases页面](https://github.com/fatedier/frp/releases)。
在页面中，找到最新发布的版本，这通常会被标记为“Latest”或有相应的版本号。

### 步骤2：下载二进制文件

根据你的操作系统（Windows、Linux或macOS）和架构（如x86_64、arm等），选择相应的预编译二进制文件。
  - 对于Linux用户，你会找到以`linux_amd64`或`linux_arm64`等命名的压缩文件。
  - 对于macOS用户，文件名通常包含`darwin`。
  - 对于Windows用户，文件名会包含`windows_4.0`。
点击相应的文件名，下载到你的计算机上。

### 步骤3：解压缩文件

解压缩下载的文件。对于Linux和macOS用户，可以使用`tar`命令：
  
  ```bash
  tar -zxvf frp_0.39.0_linux_amd64.tar.gz
  ```
  
  对于Windows用户，可以使用文件资源管理器或第三方解压缩软件解压`.zip`文件。
  

### 步骤4：移动到合适的目录

在解压缩的目录中，你会找到`frps`和`frpc`两个可执行文件，分别对应服务器端和客户端。
  
将`frps`和`frpc`移动到合适的目录，例如`/usr/local/bin`（需要管理员权限）：
  
  ```bash
  sudo mv frps frpc /usr/local/bin/
  ```
  

### 步骤5：检查运行

在命令行中运行`frps -v`和`frpc -v`来检查二进制文件是否正确无误：
  
  ```bash
  frps -v
  frpc -v
  ```
  
  这将显示frp的版本信息，确认安装成功。
  

## 示例用法

frp的使用涉及两个主要组件：`frps`（服务器端）和`frpc`（客户端）。以下是一些基本的示例用法，帮助你快速开始使用frp。

### 1. 通过SSH访问内网计算机

#### 服务端（frps）配置和启动：

1. 在拥有公网IP的服务器上配置`frps.toml`：
  
  ```toml
  # frps.toml
  bindPort = 7000
  ```
  
2. 启动`frps`：
  
  ```bash
  ./frps -c ./frps.toml
  ```
  

#### 客户端（frpc）配置和启动：

3. 在内网计算机上配置`frpc.toml`：
  
  ```toml
  # frpc.toml
  serverAddr = "x.x.x.x"  # 服务端公网IP
  serverPort = 7000
  
  [[proxies]]
  name = "ssh"
  type = "tcp"
  localIP = "127.0.0.1"
  localPort = 22
  remotePort = 6000
  ```
  
4. 启动`frpc`：
  
  ```bash
  ./frpc -c ./frpc.toml
  ```
  
5. 通过SSH访问内网计算机：
  
  ```bash
  ssh -p 6000 用户名@服务端公网IP
  ```
  

### 2. 使用自定义域名访问内网Web服务

#### 服务端（frps）配置和启动：

1. 配置`frps.toml`：
  
  ```toml
  # frps.toml
  bindPort = 7000
  vhostHTTPPort = 8080
  ```
  
2. 启动`frps`：
  
  ```bash
  ./frps -c ./frps.toml
  ```
  

#### 客户端（frpc）配置和启动：

3. 配置`frpc.toml`：
  
  ```toml
  # frpc.toml
  serverAddr = "x.x.x.x"
  serverPort = 7000
  
  [[proxies]]
  name = "web"
  type = "http"
  localPort = 80
  customDomains = ["www.example.com"]
  ```
  
4. 启动`frpc`：
  
  ```bash
  ./frpc -c ./frpc.toml
  ```
  
5. 配置域名解析：
  将`www.example.com`的A记录指向`frps`服务器的公网IP。
  
6. 访问Web服务：
  通过浏览器访问`http://www.example.com:8080`。
  

这些示例仅展示了frp的一小部分功能。frp的灵活性和强大的功能使其成为内网穿透的有力工具。你可以根据具体需求调整配置，实现更复杂的网络穿透和代理需求。