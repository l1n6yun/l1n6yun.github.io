---
title: js与jquery中获取当前鼠标的x、y坐标位置的代码
date: 2016-12-27 15:01:14
categories: 
tags: 

---
> jquery中获取当前鼠标的x、y位置位置的代码，需要的朋友可以参考下。

```html
<div id="testDiv">放在我上面</div>
<script type="text/javascript">
$('#testDiv').mousemove(function(e) {
	var xx = e.originalEvent.x || e.originalEvent.layerX || 0;
	var yy = e.originalEvent.y || e.originalEvent.layerY || 0;
	$(this).text(xx + '---' + yy);
});
</script>
```

#### javascript获取鼠标当前位置坐标

鼠标滑动显示鼠标的当前位置坐标，主要是onmousemove函数。

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>javascript获得鼠标位置</title>
</head>
<body>
<script>
function mouseMove(ev){
	Ev= ev || window.event;
	var mousePos = mouseCoords(ev);
	document.getElementById("xxx").value = mousePos.x;
	document.getElementById("yyy").value = mousePos.y;
}
function mouseCoords(ev){
	if(ev.pageX || ev.pageY){
		return {
			x:ev.pageX,
			y:ev.pageY
		};
	}
	return{
		x:ev.clientX + document.body.scrollLeft - document.body.clientLeft,
		y:ev.clientY + document.body.scrollTop - document.body.clientTop
	};
}
document.onmousemove = mouseMove;
</script>
鼠标X轴:
<input id=xxx type=text>
鼠标Y轴:
<input id=yyy type=text>
</body>
```