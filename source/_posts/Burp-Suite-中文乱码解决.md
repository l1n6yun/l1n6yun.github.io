title: Burp Suite 中文乱码解决
author: l1n6yun
tags: []
categories: []
date: 2025-03-21 15:15:00
---
在使用 Burp Suite 进行 HTTP 请求或响应分析时，可能遇到请求参数、响应内容中的中文显示为乱码的情况，例如显示为乱码符号（如方框、问号等），影响数据查看和分析。

![upload successful](/images/pasted-76.png)

Burp Suite 默认的字符编码或字体设置与中文不兼容，导致无法正确解析和显示中文字符。常见原因包括：
编码格式错误：未设置为 UTF-8（中文通用编码）。
字体不支持中文：默认字体无法渲染中文字符。

解决步骤

1. 进入设置页面

	打开 Burp Suite，点击顶部菜单栏的 Settings（设置），选择 User Interface（用户界面）。

2. 修改消息编辑器的编码和字体

	在左侧导航栏中选择 Message editor（消息编辑器）。
    
	在右侧的 Character sets（字符编码）下拉菜单中，选择 UTF-8（确保与目标网站的编码一致）。
	更换支持中文的字体：
    
	在 Font（字体）选项中，点击 Change font（选择字体），从列表中选择支持中文的字体（如 Microsoft YaHei、SimSun 或 黑体），调整字体大小至合适显示。

![upload successful](/images/pasted-77.png)

修改完成后，就可以正常查看中文字体了

![upload successful](/images/pasted-78.png)