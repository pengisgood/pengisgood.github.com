---
title: "[译] Sass 和 SCSS 到底是什么关系？"
date: 2016-08-11 16:51:37
tags: [Sass, SCSS]
categories: Web 前端
---
![](/imgs/scss-vs-sass.jpeg)

> 原文链接：[https://www.sitepoint.com/whats-difference-sass-scss/](https://www.sitepoint.com/whats-difference-sass-scss/)

我近期[写了很多](http://www.sitepoint.com/author/hgiraudel/)关于 Sass 的文章，但是最近的一些评论显现出并不是每个人都清楚地知道 Sass 指的是什么。这里做一些澄清。

当我们谈到 Sass 的时候，通常指的是预处理器（preprocessor）和语言整体。比如我们会说，“我们在用 Sass”，或者 “这是一个 Sass mixin”。同时，Sass（这里指预处理器）允许两种不同的语法：

* **Sass**，也叫缩进语法
* **SCSS**，类 CSS 语法

<!-- more -->

## 溯源

最开始，Sass 是 [Haml](http://haml.info/) 预处理器的一部分，是由 Ruby 程序员设计和编写的。因此，Sass 样式表用的是类 Ruby 语法，没有花括号、分号并且严格控制缩进，像这样：

```sass
// Variable
!primary-color= hotpink

// Mixin
=border-radius(!radius)
    -webkit-border-radius= !radius
    -moz-border-radius= !radius
    border-radius= !radius

.my-element
    color= !primary-color
    width= 100%
    overflow= hidden

.my-other-element
    +border-radius(5px)
```

正如你所看到的，这跟普通的 CSS 差别很大！即使你是一个 Sass（这里指预处理器）用户，你也会觉得这和我们过去习惯使用的完全不同。变量符号是 `!` 而不是 `$`，赋值符号是 `=` 而不是 `:`。感觉非常奇怪。

但是直到2010年5月份发布 Sass 3.0，这就是它的模样。Sass 3.0 引入了一种全新的语法叫做 SCSS（Sassy CSS，漂亮的 CSS）。它引入了对 CSS 友好的语法并将消除 Sass 与 CSS 之间的鸿沟作为目标。

```scss
// Variable
$primary-color: hotpink;

// Mixin
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    border-radius: $radius;
}

.my-element {
    color: $primary-color;
    width: 100%;
    overflow: hidden;
}

.my-other-element {
    @include border-radius(5px);
}
```

SCSS 肯定比 Sass 更接近 CSS。话虽如此，Sass 的维护者们也一直在努力的将两种语法变得越来越接近。在缩进语法中借鉴 SCSS 中的 `$` 和 `:`，换掉原来的变量符号 `!` 和赋值符号 `=`。

现在，当你新起一个项目的时候，你也许会纠结应该用哪种语法。让我来成为引路明灯并解释每种语法的优点和缺点。

## Sass 缩进语法的优点

即使这种语法看起来有点怪异，它还是有一些有趣的点。首先，输入起来它更**简短**更**容易**。你也不用关心花括号和分号了。甚至都不需要 `@mixin` 和  `@include` 了，只需要一个 `=` 或者 `+` 就足够了。

而且，Sass 语法依赖于缩进，从而**强迫执行整洁代码的标准**。因为一个错误的缩进可能就会破坏整个 `.sass` 样式文件。它每时每刻都能保证代码是整洁的、格式良好的。有一个好的方法去编写 Sass 代码。

但要小心！在 Sass 中，缩进意味着一些重要的东西。当缩进一个选择器的时候，它表示该选择器嵌套在前一个选择器中。比如：

```sass
.element-a
    color: hotpink

    .element-b
        float: left
```

它会被编成下面的 CSS：

```css
.element-a {
    color: hotpink;
}

.element-a .element-b {
    float: left;
}
```

一个简单的事实：将 `.element-b` 往右缩进一个层级意味着将它变成 `.element-a` 的孩子，CSS 也会随着改变。一定要小心你的缩进！

作为题外话，我感觉基于缩进的语法可能会更加适合 Ruby、Python 团队，和 PHP、Java 团队相比的话（尽管这是有争论性的，我也很愿意听到对立的意见）。

## SCSS 语法的优点

对于初学者，他对 CSS 是**完全兼容**的。这意味着，你可以直接将 CSS 文件重命名成 `.scss`，并且它能够直接工作。自从 SCSS 发布之后，保持 SCSS 完全兼容 CSS 一直是 Sass 维护者们高优先级的事情，而且我认为这是一件大事情。不仅如此，他们尽可能的贴近任何将来可能成为合法的 CSS 语法（比如 `@directives`）。

因为 SCSS 对 CSS 是兼容的，意味着**几乎没有学习曲线**。这种语法已经人尽皆知了，毕竟它就是 CSS 添加了一些额外的东西。和经验比较欠缺的开发者一起工作，这点非常重要：在完全不了解 Sass 的时候他们就可以快速的开始编码。

而且，它具有**很强的可读性**因为都有有意义的。当你读到 `@mixin` 的时候，你知道它是一个 `mixin` 的声明；当你看到 `@include` 的时候，你知道正在调用一个  `mixin`。它没有任何简写，当你大声读出来的时候都是有意义的。

不仅如此，几乎所有现存的工具、插件和例子的开发都是用 SCSS 语法。随着时间推移，该语法变得卓越并成了默认选项（如果只能有一个选项的话），主要归功于前面的原因。比如，很难为 Sass 缩进语法找到一款简洁的语法高亮的工具。通常，只提供给 SCSS。

## 结语

选择完全取决于你，但是除非你有特别好的原因去使用缩进语法，不然我会强烈建议你使用 SCSS 语法。它不仅更加简单，也更加方便。

我曾经自己尝试过缩进语法并且很喜欢它。我喜欢它的简单明了。在最后一分钟改变主意之前，我在工作中差点将整个项目都迁移到 Sass 语法。我感谢过去的自己停止了这一举动，因为如果我们使用缩进语法的话它将很难和我们已有的几个工具一起工作。

最后，请注意 Sass 从来都不是全部大写字母的，不论你谈论的是语言还是语法。同时，SCSS 永远都是大写字母。需要一个提醒？让[ SassnotSASS.com ](SassnotSASS.com)来拯救你！
