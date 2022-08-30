---
title: js实现返回顶部功能的解决方案
date: 2019-06-28 10:50:05
categories:
tags:
---

# 纯 js (无动画版本)

```javascript
window.scrollTo(x-coord, y-coord);  
window.scrollTo(0,0);  
```

# 纯 js (带动画版本)

```javascript
var scrollToTop = window.setInterval(function() {
  var pos = window.pageYOffset;
  if ( pos > 0 ) {
    window.scrollTo( 0, pos - 20 ); // how far to scroll on each step
  } else {
    window.clearInterval( scrollToTop );
  }
}, 16); // how fast to scroll (this equals roughly 60 fps)
```
# jQuery (带动画版本)

<iframe sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin" src="//jsrun.net/PFyKp/embedded/all/dark" width="100%" height="300" frameborder="0"></iframe>