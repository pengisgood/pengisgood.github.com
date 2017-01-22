---
title: "前端性能调优(5) ┃ 将 Stylesheets 放在顶部"
date: 2017-01-22 14:44:15
tags: [Best Practice, 翻译]
categories: [Web, Performance]
---

<img src="/imgs/stylesheet.jpg" width="100%">

> *原文链接：[https://developer.yahoo.com/performance/rules.html#css_top](https://developer.yahoo.com/performance/rules.html#css_top)*

在 Yahoo！ 研究性能时，我们发现将样式表移动到文档 HEAD 使页面加载得更快。这是因为在 HEAD 中放置样式表允许页面渐进式地渲染。

<!-- more -->

关注性能的前端工程师想要页面渐进式地渲染，也就是说，我们希望浏览器尽快显示它所拥有的任何内容。这对于具有大量内容的网页和用户的网络较慢时尤其重要。给予用户视觉反馈的重要性已经被很好地研究和[记录](https://www.nngroup.com/articles/response-times-3-important-limits/)过了，例如进度指示器。在我们的例子中，HTML 页面是进度指示器！当浏览器逐渐加载页面时，页眉，导航栏，顶部的标志等都用作对等待页面的用户的视觉反馈。这提高了整体用户体验。

将样式表放在文档底部附近的问题是它禁止了许多浏览器（包括 Internet Explorer）中进行渐进式渲染。这些浏览器阻止渲染，以避免如果页面的样式发生变化导致的不得不重绘页面的元素。用户会被卡住，一直看着空白页。

[HTML 规范](https://www.w3.org/TR/html4/struct/links.html#h-12.3)清楚地说明样式表将被包括在页面的 HEAD 中：“与 A 不同，\[LINK\] 只能出现在文档的 HEAD 部分，尽管它可能出现任意次数。无论是空白的白色屏幕还是闪现的没有样式的内容，都不是值得冒风险的替代方案。最佳解决方案是遵循 HTML 规范并在文档 HEAD 中加载样式表。

## 原文

> ### Put Stylesheets at the Top
>tag: css
>
>While researching performance at Yahoo!, we discovered that moving stylesheets to the document HEAD makes pages appear to be loading faster. This is because putting stylesheets in the HEAD allows the page to render progressively.
>
>Front-end engineers that care about performance want a page to load progressively; that is, we want the browser to display whatever content it has as soon as possible. This is especially important for pages with a lot of content and for users on slower Internet connections. The importance of giving users visual feedback, such as progress indicators, has been well researched and [documented](https://www.nngroup.com/articles/response-times-3-important-limits/). In our case the HTML page is the progress indicator! When the browser loads the page progressively the header, the navigation bar, the logo at the top, etc. all serve as visual feedback for the user who is waiting for the page. This improves the overall user experience.
>
>The problem with putting stylesheets near the bottom of the document is that it prohibits progressive rendering in many browsers, including Internet Explorer. These browsers block rendering to avoid having to redraw elements of the page if their styles change. The user is stuck viewing a blank white page.
>
>The [HTML specification](https://www.w3.org/TR/html4/struct/links.html#h-12.3) clearly states that stylesheets are to be included in the HEAD of the page: "Unlike A, [LINK] may only appear in the HEAD section of a document, although it may appear any number of times." Neither of the alternatives, the blank white screen or flash of unstyled content, are worth the risk. The optimal solution is to follow the HTML specification and load your stylesheets in the document HEAD.
