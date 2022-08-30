title: php 最简单的验证码
tags:
  - php
  - 验证码
  - 生成图片
date: 2016-12-17 16:18:55
categories:
---
create_code.php

```php
<?php
session_start();
//生成验证码图片
header("Content-type: image/png");
// 全数字
$str = "1,2,3,4,5,6,7,8,9,a,b,c,d,f,g";      //要显示的字符，可自己进行增删
$list = explode(",", $str);
$cmax = count($list) - 1;
$verifyCode = '';
for ( $i=0; $i < 5; $i++ ){
      $randnum = mt_rand(0, $cmax);
      $verifyCode .= $list[$randnum];           //取出字符，组合成为我们要的验证码字符
}
$_SESSION['code'] = $verifyCode;        //将字符放入SESSION中
  
$im = imagecreate(58,28);    //生成图片
$black = imagecolorallocate($im, 0,0,0);     //此条及以下三条为设置的颜色
$white = imagecolorallocate($im, 255,255,255);
$gray = imagecolorallocate($im, 200,200,200);
$red = imagecolorallocate($im, 255, 0, 0);
imagefill($im,0,0,$white);     //给图片填充颜色
  
//将验证码绘入图片
imagestring($im, 5, 10, 8, $verifyCode, $black);    //将验证码写入到图片中
  
for($i=0;$i<50;$i++)  //加入干扰象素
{
     imagesetpixel($im, rand()p , rand()0 , $black);    //加入点状干扰素
     imagesetpixel($im, rand()p , rand()0 , $red);
     imagesetpixel($im, rand()p , rand()0 , $gray);
     //imagearc($im, rand()p, rand()p, 20, 20, 75, 170, $black);    //加入弧线状干扰素
     //imageline($im, rand()p, rand()p, rand()p, rand()p, $red);    //加入线条状干扰素
}
imagepng($im);
imagedestroy($im);
?>
```

引用
demo.html
```html
<!-- DOCTYPE html PUBLIC "-//WAPFORUM//DTD XHTML Mobile 1.0//EN" "http://www.wapforum.org/DTD/xhtml-mobile10.dtd" -->
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
</head>
 
<body>
<form action="act.php" method="post">
<input type="text" name="code" />
<img id="code" src="create_code.php" alt="看不清楚，换一张" style="cursor: pointer; vertical-align:middle;" onClick="create_code()"/>
<!--<button type="button" onClick="create_code()">更换</button>-->
<button type="submit">提交</button>
</form>
<script>
function create_code(){
    document.getElementById('code').src = 'create_code.php?'+Math.random()*10000;
}
</script>
</body>
</html>
```

//处理，判断是否输入正确
act.php
```php
<?php
session_start();
 
if($_POST['code'] == $_SESSION['code']){
    echo 'ok';
}else{
    echo 'no';
}
?>
```