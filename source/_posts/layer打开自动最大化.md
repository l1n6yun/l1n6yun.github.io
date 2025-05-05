title: layer打开自动最大化
description: 使用layer时候 想在弹出层 在打开的时候默认就是最大值
date: 2019-04-12 13:09:55
categories:
tags: 
 - Layer.js
 - JavaScript
 - 界面优化
 - 弹出窗口
---

使用layer时候 想在弹出层 在打开的时候默认就是最大值

代码如下

```javascript
var index = layer.open({
  type: 1,
  area: ['420px', '240px'], // 宽高
  content: 'html内容',
  maxmin:true  // 最大最小化
});
layer.full(index);
```