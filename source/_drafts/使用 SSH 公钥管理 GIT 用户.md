---
title: 将公钥部署到远程Git仓库
date: 2016-12-23 22:21:48
categories: 
tags: Git

---
在主机上建立一个 `git` 账户，让每个需要写权限的人发送一个 `SSH` 公钥，然后将其加入 `git` 账户的 `~/.ssh/authorized_keys` 文件。这样一来，所有人都将通过 `git` 账户访问主机。这丝毫不会影响提交的数据——访问主机用的身份不会影响 `commit` 的记录。

```shell
ssh-keygen -t rsa -C "username@example.com"
```

接下来点击enter键即可（也可以输入密码）。

生成的文件保存在 `C:\Users\Administrator\.ssh` ,文件名：`id_rsa` (私钥)，`id_rsa.pub`(公钥)

然后登陆Coding,打开账户->SSH公钥。将id_rsa.pub，复制其中全部内容，填写到SSH_RSA公钥key下的一栏，公钥名称可以随意起名字。

完成后点击“添加”，然后输入密码即可添加完成。