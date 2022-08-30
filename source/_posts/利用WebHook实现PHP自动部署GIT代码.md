title: 利用WebHook实现PHP自动部署git代码
tags:
  - git
  - WebHook
categories: []
date: 2019-11-12 10:18:00
---

1. 生成公钥

   ```shell
    sudo -Hu www ssh-keygen -t rsa
    sudo cat /var/www/.ssh/id_rsa.pub 
   ```

2. 修改GIT配置

   ```shell
   sudo -Hu www git config --global user.name "l1n6yun"
   sudo -Hu www git config --global user.email "l1n6yun@gmail.com"
   ```

3. 初始化仓库

   ```shell
   sudo -Hu www git clone git@github.com:you/project.git /www/wwwroot/project --depth=1
   ```

4. 添加钩子文件

   ```php
   error_reporting(1);
   
   $target = '/www/wwwroot/project';
   $token = 'Your hook token';
   
   $json = json_decode(file_get_contents('php://input'), true);
   
   if (empty($json['token']) || $json['token'] !== $token) {
   	exit('error request');
   }
   
   $cmd = "sudo -Hu www cd $target && git pull";
   shell_exec($cmd);
   ```

5. 在托管平台上添加 `hook`

6. 测试

   ```shell
   git commit -am "test hook" --allow-empty
   git push
   ```

    OK，稍微一几秒，正常的话你在代码里配置的目标目录里就会有你的项目文件了。 