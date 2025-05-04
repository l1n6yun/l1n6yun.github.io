title: 巧用 Dark Reader 破解阅读全文
author: l1n6yun
tags: 
 - Dark Reader
 - 全文阅读
 - 前端破解
 - 自定义 CSS
categories: []
date: 2022-09-30 23:53:00
---
在阅读一些网站文章时，时常会遇到文章内容只展示一部分，用户需要 关注博主、或者关注公众号 的一系列障碍。

![upload successful](/images/pasted-33.png)

![upload successful](/images/pasted-34.png)

于是使用“开发者工具”分析了一下前端代码发现。大多数实现逻辑基本上都是将文章内容元素设置一个较小高度，超出的部分隐藏掉。再在后追加一个“查看全文”的元素。

想起 Dark Reader （暗黑）插件的自定义CSS功能，因为在使用通用暗黑方案后不能满足所有网站，所以要对一些不能完美适配的网站添加亿点自定义代码。在这正好派到了用场。（当然你也可以使用其他插件）

打开 Dark Reader 插件，点击开发者工具

![upload successful](/images/pasted-35.png)

在 主题编辑器 中添加一下代码：（这里用了IT屋和CSDN做演示）

```css
================================

it1352.com

CSS
.arc-body-main-more{
  display: none !important;
}
.arc-body-main{
  height: auto !important;
}

================================

blog.csdn.net

CSS
.article_content{
  height: auto !important;
}
div.hide-article-box {
  display: none !important;
}
```

![upload successful](/images/pasted-36.png)

点击 Apply，刷新网站，就可以清除全文阅读限制了

实现原理很简单：隐藏关注元素，清除文章内容元素的高度限制。

注：此方法只能用于前端限制，后端限制是不行的。