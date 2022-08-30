title: CSS3自定义滚动条样式 -webkit-scrollbar
tags:
  - CSS3
categories: []
date: 2016-12-16 15:39:00
---

> 有没有觉得浏览器自带的原始滚动条很不美观,同时也有看到很多网站的自定义滚动条显得高端,就连 `chrome32.0` 开发板都抛弃了原始的滚动条,美观多了。那 ** webkit浏览器 ** 是如何自定义滚动条的呢？

## 前言

`webkit` 支持拥有 `overflow` 属性的区域,列表框,下拉菜单,`textarea` 的滚动条自定义样式,所以用处还是挺大的。当然,兼容所有浏览器的滚动条样式目前是不存在的。

## 演示

来看看这2个滚动条demo： [demo1(图片版)](http://www.xuanfengge.com/demo/201311/scroll/index.html)、[demo2(纯CSS3版)](http://www.xuanfengge.com/demo/201311/scroll/css3-scroll.html)

## 滚动条组成

| 样式名                            | 介绍                                                                                   |
| --------------------------------- | -------------------------------------------------------------------------------------- |
| `::-webkit-scrollbar`             | 滚动条整体部分                                                                         |
| `::-webkit-scrollbar-thumb`       | 滚动条里面的小方块,能向上向下移动(或往左往右移动,取决于是垂直滚动条还是水平滚动条)     |
| `::-webkit-scrollbar-track`       | 滚动条的轨道(里面装有`Thumb`)                                                          |
| `::-webkit-scrollbar-button`      | 滚动条的轨道的两端按钮,允许通过点击微调小方块的位置。                                  |
| `::-webkit-scrollbar-track-piece` | 内层轨道,滚动条中间部分(除去)                                                          |
| `::-webkit-scrollbar-corner`      | 边角,即两个滚动条的交汇处                                                              |
| `::-webkit-resizer`               | 两个滚动条的交汇处上用于通过拖动调整元素大小的小控件                                   |

## 简洁版

这里就不贴出详细代码了, demo 里面可以通过查看源码寻找具体样式的设置。来看看 demo2 中第二个滚动条的样式

```CSS
/*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/  
::-webkit-scrollbar  
{  
    width: 16px;  
    height: 16px;  
    background-color: #F5F5F5;  
}  
  
/*定义滚动条轨道 内阴影+圆角*/  
::-webkit-scrollbar-track  
{  
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);  
    border-radius: 10px;  
    background-color: #F5F5F5;  
}  
  
/*定义滑块 内阴影+圆角*/  
::-webkit-scrollbar-thumb  
{  
    border-radius: 10px;  
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);  
    background-color: #555;  
}
```

## 详细设置

定义滚动条就是利用伪元素与伪类,那什么是伪元素和伪类呢？

伪类大家应该很熟悉 `:link` , `:focus` , `:hover` ,此外 [CSS3](http://lib.csdn.net/base/css3) 中又增加了许多伪类选择器,如 `:nth-child` , `:last-child` , `:nth-last-of-type()` 等。

`CSS` 中的伪元素大家以前看过： `:first-line` , `:first-letter` , `:before` , `:after` 。那么在 `CSS3` 中,伪元素进行了调整,在以前的基础上增加了一个 `:` 也就是现在变成了" `::first-letter` , `::first-line` , `::before` , `::after` ",另外 `CSS3` 还增加了一个 `::selection` 。两个 `::` 和一个 `:` 在 `CSS3` 中主要用来区分伪类和伪元素。

webkit的伪类和伪元素的实现很强,可以把滚动条当成一个页面元素来定义,再结合一些高级的 `CSS3` 属性,比如渐变、圆角、RGBa等等。然后如果有些地方要用图片,可以把图片也可以转换成 `Base64` ,不然每次都得加载那个多个图片,增加请求数。

任何对象都可以设置：边框、阴影、背景图片等等,创建的滚动条任然会按照[操作系统](http://lib.csdn.net/base/operatingsystem)本身的设置来完成其交互的行为。下面的伪类可以应用到上面的伪元素中。有点小复杂,具体怎么写可以看第一个 demo ,那里也有注释。

| 样式名                                                  | 介绍                                                                                                               |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `:horizontal`                                           | `horizontal` 伪类适用于任何水平方向上的滚动条                                                                      |
| `:vertical`                                             | `vertical` 伪类适用于任何垂直方向的滚动条                                                                          |
| `:decrement`                                            | `decrement` 伪类适用于按钮和轨道碎片。表示递减的按钮或轨道碎片,例如可以使区域向上或者向右移动的区域和按钮          |
| `:increment`                                            | `increment` 伪类适用于按钮和轨道碎片。表示递增的按钮或轨道碎片,例如可以使区域向下或者向左移动的区域和按钮          |
| `:start`                                                | `start` 伪类适用于按钮和轨道碎片。表示对象(按钮 轨道碎片)是否放在滑块的前面                                        |
| `:end`                                                  | `end` 伪类适用于按钮和轨道碎片。表示对象(按钮 轨道碎片)是否放在滑块的后面                                          |
| `:double-button`                                        | `double-button` 伪类适用于按钮和轨道碎片。判断轨道结束的位置是否是一对按钮。也就是轨道碎片紧挨着一对在一起的按钮。 |
| `:single-button`                                        | `single-button` 伪类适用于按钮和轨道碎片。判断轨道结束的位置是否是一个按钮。也就是轨道碎片紧挨着一个单独的按钮。   |
| `:no-button`                                            | `no-button` 伪类表示轨道结束的位置没有按钮。                                                                       |
| `:corner-present`                                       | `corner-present` 伪类表示滚动条的角落是否存在。                                                                    |
| `:window-inactive`                                      | 适用于所有滚动条,表示包含滚动条的区域,焦点不在该窗口的时候。                                                       |
| `::-webkit-scrollbar-track-piece:start`                 | 滚动条上半边或左半边                                                                                               |
| `::-webkit-scrollbar-thumb:window-inactive`             | 当焦点不在当前区域滑块的状态                                                                                       |
| `::-webkit-scrollbar-button:horizontal:decrement:hover` | 当鼠标在水平滚动条下面的按钮上的状态                                                                               |