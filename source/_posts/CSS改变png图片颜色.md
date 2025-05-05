title: CSS改变png图片颜色
tags: 
 - JavaScript
 - HTML
 - CSS
 - 移动开发
categories: []
date: 2017-05-22 22:30:00
---
css
```CSS
div.icon{height:20px;width:20px;overflow: hidden;}
.icon .icon{width: 20px;height: 20px;display:block;position: relative;left: -20px;border-right: 20px solid transparent;
    background: url(img/icon.png) no-repeat;-webkit-filter: drop-shadow(#000 20px 0);filter: drop-shadow(#000 20px 0);   
}
```

html
```HTML
<div class="icon">
    <span class='icon' id='icon'></span>
</div>
<input type="color" id='color' />
```

JS
```JavaScript
document.getElementById('color').onchange = function(){
	var c = this.value;
	document.getElementById('icon').style.webkitFilter = 'drop-shadow('+c+' 20px 0)';
}
```

在谷歌、火狐手机端、都是可以用的，使用的技术是 css 里滤镜里的投影。

我要睡到下一个世界大战