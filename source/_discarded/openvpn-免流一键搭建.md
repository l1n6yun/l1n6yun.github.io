title: openvpn+免流一键搭建
tags:
  - openvpn
  - VPN
date: 2017-06-08 20:05:09
categories:
---
这个教程还是在我前几天试试搭建免流时发现的，现在拿出共享一下。免流host整理

这个其实挺有意思的，免流应该只是在openvpn的基础上加上几段伪装host代码。所以说，如果把免流代码删除后，完全可以当一个openvpn一键安装包用，vpn功能亲测可用。

服务器推荐：CentOS 6.x 64位

登录自己的服务器后直接一段代码

```powershell
wget http://xxzml.top/ovpn && bash ovpn
```

如果提示出错 bash: wget: command not found 请执行

```powershell
yum -y install wget
```

提示输入授权码：xxzml0618

确定用户 确定密码

接下来的就不用管了，等待下载安装完成后这里要注意一下；安装完成后会自动上传到国外的一个上传下载网站上，然后会给出你的配置文件下载地址。

下载后解压下载的压缩包，提取OpenVPN.ovpn。

CA证书：ca.crt

TLS密钥：ta.key

成品配置文件：OpenVPN.ovpn

接下来就是所谓的尽情奔放，应用到手机就可以使用了。

补充：

创建账号（终端执行）：

```powershell
echo user pass >>/passwd
#创建示例：echo 123 456 >>/passwd
#（账号：123 密码:456）
#查看账号列表（终端执行）：cat /passwd
```
