title: 当script中的type等于text/html时，我们可以做很多事件！
tags: 
 - text/html
 - jQuery
 - 正则表达式
 - 模板引擎
 - Javascript
date: 2017-07-15 19:47:30
categories:
---

我们可以在 `<script>` 片断中定义一个被JS调用的代码，但代码又不在页面上显示，这时，我们可以使用下面的方法：

```html
<script id="commentTemplate" type="text/html">
    <li>
        <div class="photo">
            <a href="#"><img src="[UserImg]" /></a>
        </div>
        <p><a href="#">[UserName]：</a><span class="time">[CreateDate]</span></p>
        <div class="clear"></div>
    </li>
</script>
```

```html
<div id="comment_ul_2"></div>

<input type="button" id="addFun" value="click me" />

<script type="text/javascript">
    var reg = new RegExp("\\[([^\\[\\]]*?)\\]", 'igm'); //i g m是指分别用于指定区分大小写的匹配、全局匹配和多行匹配。
    $("#addFun").click(function () {
        var html = document.getElementById("commentTemplate").innerHTML;
        var source = html.replace(reg, function (node, key) { return { 'UserImg': '1', 'UserName': 'zhang', 'CreateDate': '2011-1-1'}[key]; });
        $("#comment_ul_2").append(source);
    });

    var zzl = "name:[name]";
    zzl = zzl.replace(reg, function (node, key) { return { 'name': '占占'}[key]; });
    alert(zzl);
</script>
```

OK，这个意思是说，当你单击按钮时，可以把 `commentTemplate` 的内容追到 `comment_ul_2` 里，这很有意思吧，呵呵！

而其中有一个 `replace` ，也很有意思，向在替换时，可以接受一个 json 字符串，然后根据 json 的 `key` 来对比 js 模块里的 `key` ，进行赋值！

真的很有意思！









