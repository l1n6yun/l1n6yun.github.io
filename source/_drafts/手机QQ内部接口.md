---
title: 手机QQ内部接口
date: 2017-4-8 16:00:03
categories: 
tags: [手机QQ]

---
### 分享接口

```javascript
window.parent.frames.location.href = "mqqapi://share/to_fri?file_type=news&src_type=web&version=1&generalpastboard=1&share_id=1105471055&url=[baseurl]&previewimageUrl=[baseimage]&image_url=[baseimage]&title=[basetitle]&description=[basedesc]&callback_type=scheme&thirdAppDisplayName=UVE=&app_name=UVE=&cflag=0&shareType=0";
```

### 内部新窗打开
```javascript
mqqapi://forward/url?version=1&src_type=web&url_prefix=[baseurl]
```

### 添加&打开QQ群
```javascript
mqqapi://card/show_pslcard?src_type=internal&version=1&uin=5555530&card_type=group&source=qrcode
```

### 手机浏览器打开
```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<script src="http://open.mobile.qq.com/sdk/qqapi.js?_bid=152"></script>
	<script type="text/javascript">
		mqq.ui.openUrl({target: 2,url: window.location.href});
	</script>
</head>
<body>
</body>
</html>
```
