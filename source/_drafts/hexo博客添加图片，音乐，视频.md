---
title: hexo博客添加图片，音乐，视频
date: 2017-01-07 11:36:48
categories: 
tags: 

---

### 插入外部链接图片

```
![图片描述](图片地址) 
```

### 添加本地图片

在\hexo\source目录下新建文件夹，命名为images或者其他你喜欢的名字，然后编辑你的md博文，插入下面的代码样式：

```
![图片描述](/images/你的图片名字.JPG) 
```

### 插入音乐

比如网易云音乐，找到喜欢的歌曲，点击分享按钮，把里面的代码复制下来，直接粘贴到博文中即可

```
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" height="86"  
    src="http://music.163.com/outchain/player?type=2&id=25706282&auto=0&height=66"> 
</iframe> 
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" height="86" src="http://music.163.com/outchain/player?type=2&id=25706282&auto=0&height=66"> 
</iframe> 

### 插入视频

```
Idina Menze和Caleb Hyles激情对唱Let It Go： 
<iframe  
    height="498" width="510"  
    src="http://player.youku.com/embed/XNjcyMDU4Njg0"  
    frameborder=0 allowfullscreen> 
</iframe> 
```
<iframe height="498" width="510" src="http://player.youku.com/embed/XNjcyMDU4Njg0" frameborder=0 allowfullscreen></iframe>

