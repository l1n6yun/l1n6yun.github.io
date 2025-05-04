title: hexo-admin插件在windows系统插入图片地址错误
author: l1n6yun
tags: []
categories: []
date: 2022-01-25 23:14:00
---
在使用hexo-admin插入图片时，我发现插入的图片显示错误，并且链接也有问题。

```markdown
预期
![upload successful](/images/pasted-32.png)
实际
![upload successful](\\images\pasted-32.png\)
```

#### 修复方法

修改 `blog/node_modules/hexo-admin/api.js` 文件

```js
// filename = path.join(imagePath, filename)
filename = imagePath + '/' + filename	// 修改点
var outpath = path.join(hexo.source_dir, filename)

var dataURI = req.body.data.slice('data:image/png;base64,'.length)
var buf = new Buffer(dataURI, 'base64')
hexo.log.d(`saving image to ${outpath}`)
fs.writeFile(outpath, buf, function (err) {
    if (err) {
        console.log(err)
    }
    hexo.source.process().then(function () {
        res.done({
            // src: path.join(hexo.config.root + filename),
            src: filename,	// 修改点
            msg: msg
        })
    });
})
```

