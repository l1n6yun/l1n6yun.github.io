title: 编写模块化CSS：BEM
description: 你是否做过多页面的大型网站或者其中一部分？如果你做过，你可能会意识到 CSS 架构不够强大所带来的恐惧。你可能还会研究如何编写可维护的 CSS。
date: 2019-08-29 16:22:46
categories:
tags:
---

> 你是否做过多页面的大型网站或者其中一部分？如果你做过，你可能会意识到 CSS 架构不够强大所带来的恐惧。你可能还会研究如何编写可维护的 CSS。

由于我们的行业很棒，我们有很多推荐的解决方案。因为专家们的纷纷加入，于是我们有 BEM，OOCSS，SMACSS，Atomic Design 等许多选择。

现在，问题不是痛苦 **“我不知道该怎么办”**，而是： **“有这么多的方法，我应该尝试哪个？”我是不是应该把所有的都用一遍，是不是只有一种方法才适合我，或者我是不是应该参考它们做一个自己的架构？**。

我开始只用一种方法。然后，当我尝试不同的方法时，我开始把我认为有意义的东西包含在我的探索过程中。 在这篇文章中，我想和大家分享一下我如何构建 CSS 以及为什么我这样做。 希望它可以帮助你找到你喜欢的方法。

## 当我在寻找一个出色的 CSS 架构时我究竟在找什么

当我将不同的方法拼凑在一起以形成我自己的习惯时，我会寻找以下四个特点：

- 我必须 **立即知道编辑一个 class 是否安全**，会不会干扰其他 CSS。这是最重要的，特别是当我需要在短时间内进行修改时。我不想因为改变一处而破坏别的东西。
- 我必须**立即知道一个 class 放在这个伟大工程中的什么地方**，以防止大脑过载。这样我就可以快速修改 `style`，而不必在整个工程里前后引用。
- `class` 必须 **尽可能少**，因为看到一长串的 `class` 时我头很晕。
- 我必须 **立即知道一个组件是否使用了 JavaScript**，所以如果我改变了它的 CSS，我不会意外地破坏任何组件。

在我的探索中，我发现 **BEM** 和 **命名空间** 符合我寻找的标准。

## 从 BEM 开始

BEM 是我的方法的基础。如果你以前从未听说过 BEM，它代表 `block` ， `element` 和 `modifier`。当你第一次接触它时，它看起来是那么的难看。

```css
.block { /* styles */ } 
.block__element { /* styles */ } 
.block--modifier { /* styles */ }
```

当我第一次看见 BEM 的时候，我就很讨厌它，甚至没有给它一个机会。我不记得是什么驱使我尝试 BEM 的，但我现在深深的知道它有多么的强大。让我来完整地解释一下 BEM 是什么（当然，加入了我自己的理解）。

## 块

一个块就是一个组件。这有点抽象，所以让我们用示例来学习。

假设您正在建立一个联系表单。在这种情况下，这个表单可以是一个块。在 BEM 中，块被写为像 `class` 的名字一样，如下所示：

```css
.form { /* styles */ }
```

BEM 使用 `.form` 而不是 `<form>` 元素的原因是因为 **类允许无限的可重用性**，而即使是最基本的元素也可能改变样式。

按钮很好地阐释了可以包含不同样式的块。如果将 `<button>` 元素的背景颜色设置为红色，则所有 `<buttons>` 都将被强制继承红色背景。接下来，你必须通过覆盖你的 `<button>` 元素来修复代码（并且可能会在修复中“伤及无辜” 🤕）。

```css
button { 
    background-color: red; 
} 

.something button { 
    background-color: blue; /* 😱 */
}
```

如果设置了一个 `.button` 类的按钮，则可以在任何 `<button>` 元素上选择是否使用 `.button` 类。那么，如果你需要一个不同的背景颜色，你所做的就是改成一个新的 `class`，比如说 `.button--secondary`，很舒服吧！

```css
.button { 
    background-color: red; 
}

.button--secondary { 
    background-color: blue; /* 😄 */
}
```

这给我们引入了 BEM 的下一部分 —— 修饰符。

## 修饰符

修饰符是改变某个块的外观的标志。要使用修饰符，可以将 `--modifier` 添加到块中。

从上面的按钮示例继续，修改的按钮将被命名为 `.button--secondary`。

在传统的 BEM 中，当你使用修饰符时，你应该 **将块和修饰符添加** 到 HTML 中，以便在新的 `.button--secondary` 中不重写 `.button` 样式。

```html
<button class="button">Primary button</button>
<button class="button button--secondary">Secondary button</button>

.button { 
    padding: 0.5em 0.75em; 
    background-color: red; 
} 
.button--secondary { 
    background-color: green; 
}
```

注意为什么没有必要在 `.button--secondary` 中重新声明 `padding`，因为它已经在 `.button` 中声明了。这很棒，因为 BEM 确保你编写简洁的 CSS，而不需要付出大量的工作。

但是，我并不喜欢在HTML中再加一个 `.button`，因为 `.button--modifier` 已经告诉我，它是一个带有 `--secondary` 标志的 `.button` 。理想情况下，我的 HTML 应该是这样的：

```html
<button class="button">Primary button</button>
<button class="button--secondary">Secondary button</button>
```

这更简洁，不是吗？

不幸的是，如果 HTML 中没有 `.button`，我们必须回到非简洁的 CSS：

```css
.button { 
    padding: 0.5em 0.75em; 
    background-color: red; 
} 
.button--secondary { 
    padding: 0.5em 0.75em;  /* 😱 */
    background-color: green; 
}
```

呃，这么繁琐的东西好恶心 😢。 但是有两种方法可以编写简洁的 CSS，而不需要额外的 `class`！

### 方法 1：使用 mixin

第一种方式，如果使用 Sass 或任何其他预处理器，则 **使用mixin来封装** 需要重用的 **所有代码**。在我们的按钮示例中，我们只需要将 `padding` 写入 mixin。 在这里，我在块中调用这个 mixin：

```scss
@mixin button {
    padding: 0.5em 0.75em; 
} 
.button { 
    @include button;
    background-color: red; // 😄
} 
.button--secondary { 
    @include button;
    background-color: green; // 😄
} 
```

万岁！现在世界静好!🎉🎉🎉

**但是...如果我不使用 Sass 怎么办？**

放轻松！😄即将分享的第二种方法是使用普通的 CSS，所以你也可以使用它！

### 方法 2：使用 CSS 属性选择器

第二种方法 **使用CSS属性选择** 器执行稍微更复杂的选择。我会告诉你它是什么，然后解释为什么这样做：

```
/* 😄 */
[class*='button']:not([class*='button__']) { 
    padding: 0.5em 0.75em; 
}
```

现在，这不是你通常看到的选择器，所以我来解释一下。

第一部分（ `[class*='button']` ）告诉解析器查找包含文本 `button` 的所有 `class`。（ `*=` 搜索与确切字符串匹配的任何内容）。当然，这意味着 CSS 的目标是 `.button` 和 `.button--modifier`。不幸的是，这也意味着选择器也是针对 BEM 元素，这就是为什么引入第二部分的原因。

第二部分（ `:not([class*='button__'])` ）告诉解析器将包含`.button__`任何东西排除在外，于是排除了 BEM 元素。 （BEM 元素具有 `.block__element` 语法）。

在这一点上，你仍然可能不喜欢 BEM 丑陋的 `--modifier` 语法。我知道为什么，但我爱上这个语法是因为**我很讨厌命名**。有时，我发现需要使用很多单词来命名一个 BEM 块或元素。举个例子 `inner-section` 。

因此如果我使用 `-modifier` （如某些方法建议的），我将无法一眼看出 `-section` 是否是修饰符。所以这是一个馊主意。同样，我也不能立即知道`.button-secondary`是否也是修饰符！

很具有讽刺意味，但是这个丑陋的语法让我的代码更简洁，更易于维护。所以强烈推荐你尝试它:)

我们来看看 BEM 的第三个重要部分 —— 元素。

## 元素

元素是块的子节点。为了表明某个东西是一个元素，你需要在块名后添加 `__element`。所以，如果你看到一个像那样的名字，比如 `form__row` ，你将立即知道 `.form` 块中有一个 `row` 元素。

```html
<form class="form" action=""> 
    <div class="form__row"> 
        <!-- ... --> 
    </div> 
</form>

.form__row { 
    /* styles */ 
}
```

**BEM 元素有两个优点** ：

- 你可以让 CSS 的优先级保持相对扁平。
- 你能立即知道哪些东西是一个子元素。

为了解释以上两点，考虑使用两个单独的 `class` 的替代方法（许多框架这么做的）。你可能会用这样的东西：

```html
<form class="form" action=""> 
    <div class="row"> 
        <!-- ... --> 
    </div>
</form> 
```

```css
.form .row { 
    /* styles */ 
}
```

如果你使用 BEM 元素，则可以使用优先级为 `10` 而不是 `20` 的的选择器来为 `.form__row` 提供样式。此外，你可以立即分辨出（不论是在 HTML 还是 CSS 中）`.form__row` 是 `.form`的子节点。

（顺便说一句，如果你还没有克服 `__element` 语法的丑陋，等到你使用第三方插件看到像蛇一样的 `class` 名的时候， 你就懂了😛）

继续，有一件事你需要了解。**永远不应该链式命名 BEM 元素**。 如果你的 `class` 最终像这样 `.form__row__input`，你做的事情是非常错误的。（我开始时这样做过，所以你也不要对自己感到太糟糕！🤗）。

有两种方法可以绕过长长的 BEM 链式命名。 他们是：

1. 只把子子元素链接到有意义的块
2. 创建新的块来保存元素

### 链接孙元素到块

虽然 BEM 建议你将 BEM 元素写作 `.block__element` ，但它不会规定你的 HTML 应如何。所以，只要有意义的话，你可以把你的孙元素连在一起。

接下来是一个例子。在下面的代码中，你将看到 `.article__header` 是 `.article` 的子元素。`.article__title` 是 `article` 的孙元素（或者说是 `.article__header` 的子元素，如果你将它们同时表示为 `.article` 的子元素，就没有冲突，因为这个表单同时只有他们存在。

```html
<article class="article"> 
    <header class="article__header"> 
        <h1 class="article__title"></h1> 
    </header> 
</article>
```

虽然这样有效，你也会遇到无意义的链接孙元素的情况。举个例子：

```html
<section class="comments"> 
    <h2 class="comments__title"></h2> 
    <article class="comments__comment"> 
        <h3 class="comments__comment-title"></h3> 
    </article> 
    <article class="comments__comment"> 
        <h3 class="comments__comment-title"></h3> 
    </article> 
    <!-- ... --> 
</section>
```

呃？

此时你需要创建新块来保存孙元素。

### 创建新的块来保存孙元素

在上述情况下，你可以轻松地将 `.comments__comment` 拆为 `.comments` 和 `.comment` ：

```html
<section class="comments"> 
    <h2 class="comments__title"></h2> 
    <article class="comment"> 
        <h3 class="comment-title"></h3> 
    </article> 
    <article class="comment"> 
        <h3 class="comment-title"></h3> 
    </article> 
    <!-- ... --> 
</section>
```

这更有意义，不是吗？如果你这样做，请确保将 `.comments` 和 `.comment`块放在同一个文件中，以方便参考。

不幸的是，有时候它不像 `.comments__comment` 那么简单。例如，假设在块中有一个列表元素。

```html
<div class="block"> 
    <ul class="block__list"> 
        <li class="block__item"> 
            <!-- how would you name this class? --> 
            <h3 class="???????"></h3> 
        </li> 
        <!-- ... --> 
    </ul> 
</div>
```

如果你注意到，我已经链接了`.block__item` ，这是一个 `.block` 的孙元素。 将 `.block__item` 中的元素链接到 `.block` 没有意义，或可能最终会遇到一些糟糕的局面。

然而，同时由于它们被一起使用，所以为 `.block__list` 或 `.block__item` 创建一个新的块是没有意义的 。你会命名什么来保持在上下文中有意义？

在这种情况下，我一般会为 `block__item` 创建一个名为 `.item` 的伪块。看下面的HTML。

```html
<div class="block"> 
    <h3 class="block__title"></h3> 
    <ul class="block__list"> 
        <li class="block__item"> 
            <h3 class="item__title"></h3> 
        </li> 
        <!-- ... --> 
    </ul> 
</div> 
```

伪块，正如名字所示，是伪的。上面的 HTML 中没有 `.item` 的实际声明。但是，在`.block__item`中有连接到 `.item` 的元素中。

在我的 CSS（Sass）中，我在 `.block__item` 中嵌套 `.item` 元素，赋予了它所需的上下文。

```html
.block__item {
    .item__title {
        /* styles... */ 
    } 
}
```

你可能会说，**“但这是违反 BEM 惯例的！”**是的，但请阅读[下一篇文章](https://www.w3cplus.com/css/css-architecture-2.html) 。你会知道为什么我这样做😉。

接下来，还有一件事，在我的用例中添加为 BEM 添加的 —— 容器。

## 容器

有时（实际上经常），我会遇到这样的情况，我必须在确定其它元素都对齐的同时扩散一个区域的背景色，就像这样：

![Image of a block that contains a background that bleeds out of it](https://www.w3cplus.com/sites/default/files/blogs/2017/1708/css-architecture-1.png)

浅灰色的背景扩散到了对齐的区域的外面

如果你熟悉构建布局，会使用以下方式构建 HTML ：

```html
<section> 
    <div class="l-wrap"> 
        <div class="block"> 
            <!-- ... --> 
        </div> 
    </div> 
</section>
```

问题是，你应该怎么命名块容器？ 或者在这种情况下，怎么命名 `<section>` 元素。我习惯的方法是命名为 `block-container` 。我只在这种情况下使用`-container`，所以我觉得它仍然可以接受。你有更好的主意吗？

（顺便说一下，看见`.l-wrap` 中的 `.l-`了没，这是命名空间，我将[在下一篇文章中](https://www.w3cplus.com/css/css-architecture-2.html)分享更多的内容。

## 总结

所以，这就是我简单地使用 BEM 的方法。如果你注意到我上面设置的标准，你会注意到我只考虑了两个方面：

- `class` 的数量必须*尽可能少* 。
- 我必须**立即知道一个 class 放在这个伟大工程中的什么地方**，以防止大脑过载。

其他两个方面尚未考虑：

- 我必须 **立即知道组件是否使用 JavaScript** 。
- 我必须 **立即知道编辑一个 class 是否安全**,会不会干扰其他 CSS。

我将[在下一篇文章中](https://www.w3cplus.com/css/css-architecture-2.html)讨论**命名空间** 时考虑这两个方面 。

你怎么看？你有没有学到新的东西？我分享了我的学习过程有用吗？我很想在下面的评论中看到你的想法。