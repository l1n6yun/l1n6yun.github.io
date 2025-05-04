title: Markdown——入门指南
tags: []
date: 2016-12-04 18:13:00
categories:
---
导语：

> [Markdown](http://zh.wikipedia.org/wiki/Markdown) 是一种轻量级的「标记语言」，它的优点很多，目前也被越来越多的写作爱好者，撰稿者广泛使用。看到这里请不要被「标记」、「语言」所迷惑，Markdown 的语法十分简单。常用的标记符号也不超过十个，这种相对于更为复杂的 HTML 标记语言来说，Markdown 可谓是十分轻量的，学习成本也不需要太多，且一旦熟悉这种语法规则，会有一劳永逸的效果。

![Ulysses for Mac](http://ww3.sinaimg.cn/large/6aee7dbbjw1eqft66xcg3j21kw12mdub.jpg)

[Ulysses for Mac](http://www.ulyssesapp.com/)

## 一，认识 Markdown

在刚才的导语里提到，Markdown 是一种用来写作的轻量级「标记语言」，它用简洁的语法代替排版，而不像一般我们用的字处理软件 Word 或 Pages 有大量的排版、字体设置。它使我们专心于码字，用「标记」语法，来代替常见的排版格式。例如此文从内容到格式，甚至插图，键盘就可以通通搞定了。目前来看，支持 Markdown 语法的编辑器有很多，包括很多网站（例如[简书](http://jianshu.io/)）也支持了 Markdown 的文字录入。Markdown 从写作到完成，导出格式随心所欲，你可以导出 HTML 格式的文件用来网站发布，也可以十分方便的导出 PDF 格式，这种格式写出的简历更能得到 HR 的好感。甚至可以利用 [CloudApp](http://www.getcloudapp.com/) 这种云服务工具直接上传至网页用来分享你的文章，全球最大的轻博客平台 [Tumblr](http://te1ee.tumblr.com/)，也支持 Mou 这类 Markdown 工具的直接上传。

### Markdown 官方文档

> 这里可以看到官方的 Markdown 语法规则文档，当然，** 后文我也会用自己的方式阐述这些语法的具体用法 **。
* [创始人 John Gruber 的 Markdown 语法说明](http://daringfireball.net/projects/markdown/syntax)
* [Markdown 中文版语法说明](http://wowubuntu.com/markdown/#list)

### 使用 Markdown 的优点

* 专注你的文字内容而不是排版样式，安心写作。
* 轻松的导出 HTML、PDF 和本身的 .md 文件。
* 纯文本内容，兼容所有的文本编辑器与字处理软件。
* 随时修改你的文章版本，不必像字处理软件生成若干文件版本导致混乱。
* 可读、直观、学习成本低。

### 使用 Markdown 的误区

> *We believe that writing is about content, about what you want to say – not about fancy formatting. *
我们坚信写作写的是内容，所思所想，而不是花样格式。
— Ulysses for Mac

* Markdown 旨在简洁、高效，也由于 Markdown 的易读易写，人们用不同的编程语言实现了多个版本的解析器和生成器，这就导致了目前不同的 Markdown 工具集成了不同的功能（基础功能大致相同），例如流程图与时序图，复杂表格与复杂公式的呈现，虽然功能的丰富并没有什么本质的缺点，但终归有些背离初衷，何况在编写的过程中很费神，不如使用专业的工具撰写来的更有效率，所以如果你需实现复杂功能，专业的图形界面工具会更加方便。** 当然，如果你对折腾这些不同客户端对 Markdown 的定制所带来高阶功能感到愉悦的话，那也是无可厚非的。 **

![flowchart.js on Github（使用 Markdown 绘制流程图）](http://ww2.sinaimg.cn/large/6aee7dbbgw1eq320claw3j21kw0kjdpc.jpg)

[flowchart.js on Github（使用 Markdown 绘制流程图）](https://github.com/adrai/flowchart.js)

## 我该用什么工具？

![Mou for Mac](http://mouapp.com/Mou_128.png)

Mou for Mac

* 在 Mac OS X 上，我强烈建议你用 [Mou](http://mouapp.com/) 这款免费且十分好用的 Markdown 编辑器，它支持实时预览，既左边是你编辑 Markdown 语言，右边会实时的生成预览效果。不仅如此，Mou 还有一些有趣的偏好设置（Preference），例如主题（Themes）与样式（CSS），它们可以配置出定制化的文本编辑效果与导出效果，如果你对自带的主题与样式不满意还可以到 [GitHub](https://github.com/search?utf8=%E2%9C%93&q=Mou) 上搜索其它爱好者为 Mou 编写的更多主题样式，导入的方式可以在偏好设置的 Themes 或 CSS 选项中 选择 reload。

![Mou 的编写与预览窗口](http://ww1.sinaimg.cn/large/6aee7dbbgw1effcq2gx92j210j0ustj7.jpg)

Mou 的编写与预览窗口

如果你从事文字工作，我强烈建议你购买 [Ulysses for Mac](http://www.ulyssesapp.com/)，这款软件入围了苹果 Mac App Store 的 `The Best of 2013`。它支持更多的写作格式、多文档的支持。Mou，iA writer 这些软件都是基于单文档的管理方式，而 Ulysses 支持 Folder、Filter 的管理，一个 Folder 里面可以创建多个 Sheet，Sheet 之间也可以进行 Combine 处理。

![Mac 上一些 Markdown 编辑器](http://ww1.sinaimg.cn/large/6aee7dbbjw1eqgrj7suqoj217a0aiq4u.jpg)

Mac 上一些 Markdown 编辑器

* 由于笔者很少接触 Windows，Windows 下的 Markdown 没有过多涉猎，经朋友介绍，有两款还算不错，一款叫做 [MarkdownPad](http://www.markdownpad.com/) ，另一款叫做 [MarkPad](http://code52.org/DownmarkerWPF/)。
* iOS 端很多 app 早已经支持了 Markdown 录入，例如 Drafts，Day One，iA writer 等，另外 Ulysses for iPad 现在已经上架，可以说是 iOS 平台最好的编辑器了。
* 在 Web端，我强烈推荐[简书](http://jianshu.io/)这款产品，上面有无数热爱文字的人在不停的创造，分享。在 Web 端使用 Markdown * 没有比简书更舒服的地方了，同样支持左右两栏的实时预览，字体优雅，简洁。

![简书的编辑预览模式](http://ww2.sinaimg.cn/large/6aee7dbbgw1effdkfijo1j21220nigth.jpg)

简书的编辑预览模式

## 二，Markdown 语法的简要规则

### 标题

![标题](http://ww1.sinaimg.cn/large/6aee7dbbgw1effeaclhiyj20eh09cwez.jpg)

标题

标题是每篇文章都需要也是最常用的格式，在 Markdown 中，如果一段文字被定义为标题，只要在这段文字前加 `#` 号即可。

`# 一级标题`

`## 二级标题`

`### 三级标题`

以此类推，总共六级标题，建议在井号后加一个空格，这是最标准的 Markdown 语法。

### 列表

熟悉 HTML 的同学肯定知道有序列表与无序列表的区别，在 Markdown 下，列表的显示只需要在文字前加上 `-` 或 `*` 即可变为无序列表，有序列表则直接在文字前加`1.` `2.` `3.` 符号要和文字之间加上一个字符的空格。

![无序列表与有序列表](http://ww4.sinaimg.cn/large/6aee7dbbgw1effew5aftij20d80bz3yw.jpg)

无序列表与有序列表

### 引用

如果你需要引用一小段别处的句子，那么就要用引用的格式。

> 例如这样
	
只需要在文本前加入 `>` 这种尖括号（大于号）即可

![引用](http://ww3.sinaimg.cn/large/6aee7dbbgw1effezhonxlj20e009c3yu.jpg)

引用

### 图片与链接

插入链接与插入图片的语法很像，区别在一个 `!` 号

图片为：`![](){ImgCap}{/ImgCap}`

链接为：`[]()`

插入图片的地址需要图床，这里推荐[围脖图床修复计划](http://weibotuchuang.sinaapp.com/) 与 [CloudApp](http://www.getcloudapp.com/) 的服务，生成URL地址即可。

![URL 与图片](http://ww2.sinaimg.cn/large/6aee7dbbgw1efffa67voyj20ix0ctq3n.jpg)

URL 与图片

### 粗体与斜体

Markdown 的粗体和斜体也非常简单，用两个 `*` 包含一段文本就是粗体的语法，用一个 `*` 包含一段文本就是斜体的语法。

例如：** 这里是粗体 ** * 这里是斜体 *

### 表格

表格是我觉得 Markdown 比较累人的地方，例子如下：

```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```

这种语法生成的表格如下：

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

### 代码框

如果你是个程序猿，需要在文章里优雅的引用代码框，在 Markdown下实现也非常简单，只需要用两个 ` 把中间的代码包裹起来。图例：

![使用 `tab` 键即可缩进](http://ww3.sinaimg.cn/large/6aee7dbbgw1effg1lsa97j20lt0a8dgs.jpg)

使用 `tab` 键即可缩进。

### 分割线

分割线的语法只需要三个 `*` 号，例如：

***

[	到这里，Markdown 的基本语法在日常的使用中基本就没什么大问题了，只要多加练习，配合好用的工具，写起东西来肯定会行云流水。更多的语法规则，其实 Mou 的 Help 文档栗子很好，当你第一次使用 Mou 时，就会显示该文档。可以用来对用的查找和学习。

![](http://ww3.sinaimg.cn/large/6aee7dbbgw1effgmnpgqlj210j0us44j.jpg)

## 三，相关推荐:

### 工具

*图床工具用来上传图片获取 URL 地址*

* [Droplr](http://droplr.com/)
* [Cloudapp](http://www.getcloudapp.com/)
* [ezShare for Mac](https://itunes.apple.com/cn/app/yi-xiang/id672522335?mt=12&uo=4)
* [围脖图床修复计划](http://weibotuchuang.sinaapp.com/)

*在线好用的Markdown工具，为印象笔记而生*

* [马克飞象，专为印象笔记打造的Markdown编辑器，非常推荐](http://maxiang.info/)

### 相关文章阅读：

* [为什么作家应该用 Markdown 保存自己的文稿](http://jianshu.io/p/qqgjln)
* [Markdown写作浅谈](http://www.yangzhiping.com/tech/r-markdown-knitr.html)
* [Markdown 工具补完](http://www.appinn.com/markdown-tools/)
* [Drafts + Scriptogr.am + Dropbox 打造移动端 Markdown 风格博客](http://jianshu.io/p/63HYZ6)
* [图灵社区，怎样使用Markdown](http://www.ituring.com.cn/article/23)
* [为什么我们要学习Markdown的三个理由](http://news.cnblogs.com/n/139649/)
* [Markdown 语法写作入门指南 by ibuick](http://ibuick.me/?p=4093)



