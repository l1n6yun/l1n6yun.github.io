title: mathjax数学公式
categories: []
tags: 
 - MathJax
 - 数学公式
 - Hexo
date: 2017-11-22 16:16:00
---
有时候，我们还需要一些高级功能，比如在网页上显示数学公式。

新建一个文件themes/pacman/layout/_partial/mathjax.ejs，找到mathjax的调用代码复制到文件。

```html
<!-- mathjax config similar to math.stackexchange -->

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
      }
    });
</script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Queue(function() {
        var all = MathJax.Hub.getAllJax(), i;
        for(i=0; i < all.length; i += 1) {
            all[i].SourceElement().parentNode.className += ' has-jax';
        }
    });
</script>

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
```

在themes/pacman/layout/_partial/after_footer.ejs 的最后一行，增加对mathjax的引用，详细内容请查看源代码。

我们修改文章：source/_posts/新的开始.md

增加公式：

```
## 公式
$$J\_\alpha(x)=\sum _{m=0}^\infty \frac{(-1)^ m}{m! \, \Gamma (m + \alpha + 1)}{\left({\frac{x}{2}}\right)}^{2 m + \alpha }$$
```

查看效果:

## 公式
$$J\_\alpha(x)=\sum _{m=0}^\infty \frac{(-1)^ m}{m! \, \Gamma (m + \alpha + 1)}{\left({\frac{x}{2}}\right)}^{2 m + \alpha }$$