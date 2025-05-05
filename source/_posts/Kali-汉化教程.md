title: Kali 汉化教程
author: l1n6yun
tags: 
 - Kali Linux
 - 系统汉化
 - 英文切换到中文
categories: []
date: 2021-01-12 23:50:00
---
1. 在 Kali 的桌面按下 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>T</kbd> 打开终端

2. 使用 root 权限执行 `sudo dpkg-reconfigure locales`
  ![upload successful](/images/pasted-37.png)

3. 选中 **en_US.UTF-8 UTF-8**  和  **zh_CN.UTF-8 UTF-8**  (注意:按下**空格键**选中,选好后按下**TAB**键退出编码格式选项,跳到**OK选项**)
  ![upload successful](/images/pasted-38.png)

4. 选择 **zh_CN.UTF-8**，选择OK
 ![upload successful](/images/pasted-39.png)

5. 在终端键入`reboot`, 重启Kali
  ![upload successful](/images/pasted-40.png)

6. 重启Kali后, 建议选择**保留旧的名称**
  ![upload successful](/images/pasted-41.png)

7. 此时可以看到界面已经汉化完成了!
  ![upload successful](/images/pasted-42.png)