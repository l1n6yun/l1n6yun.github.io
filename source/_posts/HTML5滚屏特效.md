title: HTML5滚屏特效
date: 2017-05-22 00:04:02
categories:
tags:
---
```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<style type="text/css">
*{margin: 0px; padding: 0px;}
html,body{width: 100%; height: 100%;overflow: hidden;}

.section-wrap{width: 100%; height: 100%; overflow: visible;transition:transform 1s cubic-bezier(0.86,0,0.03,1);-webkit-transition:-webkit-transform 1s cubic-bezier(0.86,0,0.03,1);}
.section-wrap .section{width: 100%; height: 100%;}

.section-1{background: #0f0}
.section-2{background: #FF0}

.put-section-1{ transform:translateY(0);-webkit-transform:translateY(0);}
.put-section-2{ transform:translateY(-100%);-webkit-transform:translateY(-100%);}

.section-btn{ width:14px;position:fixed;right:4%;top:50%;}
.section-btn li{ width:14px;height:14px;cursor:pointer;text-indent:-9999px;border-radius:50%;-webkit-border-radius:50%;margin-bottom:12px; background:#BD362F;text-align:center; color:#fff; onsor:pointer;}
.section-btn li.on{ background:#fff}
</style>

<body>
<section class='section-wrap put-section-1'>
	<div class="section section-1">
		<div class='title'>
			<p class="tit">111</p>
		</div>
	</div>
	<div class="section section-2">
		<div class='title'>
			<p class="tit">222</p>
		</div>
	</div>
</section>
<ul class="section-btn">
	<li class="on"></li>
	<li></li>
</ul>

<script type="text/javascript" src='jiaoben3135/js/jquery.min.js'></script>
<script type="text/javascript">

$(function(){
	var i=1;
	var $btn = $('.section-btn li'),
		$wrap = $('.section-wrap'),
		$arrow = $('.arrow');
	
	/*当前页面赋值*/
	function up(){i++;if(i>$btn.length){i=1};}
	function down(){i--;if(i<1){i=$btn.length};}
	
	// /*页面滑动*/
	function run(){
		$btn.eq(i-1).addClass('on').siblings().removeClass('on');	
		$wrap.attr("class","section-wrap").addClass(function() { return "put-section-"+i; }).find('.section').eq(i).find('.title').addClass('active');
	};
	
	// /*右侧按钮点击*/
	$btn.each(function(index) {
		$(this).click(function(){
			i=index+1;
			run();
		})
	});
	
	// /*翻页按钮点击*/
	$arrow.one('click',go);
	function go(){
		up();run();	
		setTimeout(function(){$arrow.one('click',go)},1000)
	};
	
	// /*响应鼠标*/
	$wrap.one('mousewheel',mouse_);
	function mouse_(event){
		if(event.deltaY<0) {up()}
		else{down()}
		run();
		setTimeout(function(){$wrap.one('mousewheel',mouse_)},1000)
	};
	
	// /*响应键盘上下键*/
	$(document).one('keydown',k);
	function k(event){
		var e=event||window.event;
		var key=e.keyCode||e.which||e.charCode;
		switch(key)	{
			case 38: down();run();	
			break;
			case 40: up();run();	
			break;
		};
		setTimeout(function(){$(document).one('keydown',k)},1000);
	}
});
</script>
</body>
</html>
```
