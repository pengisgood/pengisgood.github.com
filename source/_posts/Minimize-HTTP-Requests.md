---
title: "前端性能调优(1) ┃ 尽可能减少 HTTP 请求"
date: 2016-12-02 13:06:34
tags: [Best Practice, 翻译]
categories: [Web, Performance]
---
<img src="/imgs/http-request.png" width="100%">

> *原文链接：[https://developer.yahoo.com/performance/rules.html#num_http](https://developer.yahoo.com/performance/rules.html#num_http)*

80％的终端用户响应时间花费在前端。大部分时间是在下载页面中所有的绑定的组件：图片（images），样式表（stylesheets)，脚本（scripts)，Flash等。减少组件数量从而会减少呈现页面所需的HTTP请求数量。这是加快页面加载的关键。

<!-- more -->

简化页面的设计是一种减少页面中组件数量的途径。但是是否有一种方法来构建具有更丰富内容的页面，同时还能让响应时间非常短呢？下面是一些减少HTTP请求数量的技术，同时仍然支持富页面设计。

**合并文件** 是一种通过将所有脚本合并到单个脚本来减少HTTP请求数量的方法，并且类似地将所有CSS合并到单个样式表中。当脚本和样式表在页面之间有所不同时，合并文件具有一定的挑战性，但是使这成为发布流程的一部分会缩短响应时间。

[**CSS Sprites**](http://alistapart.com/article/sprites)是减少图片请求数量的首选方法。将你的背景图片合并为单个图片，然后使用CSS背景图片和背景位置属性来显示所需的图片部分。

[**Image maps**](https://www.w3.org/TR/html401/struct/objects.html#h-13.6)将多个图片合并成单个图片。整体大小大致相同，但减少了HTTP请求的数量从而加快了页面加载。只有当图像在页面中是连续的（例如导航栏）时，Image maps才起作用。定义Image maps的坐标比较乏味并且容易出错。使用Image maps进行导航也不可访问，因此 **不建议使用**。

**Inline images** 使用[`data：URL scheme`](https://tools.ietf.org/html/rfc2397)将图片数据嵌入实际页面里。这会增加HTML文档的大小。将Inline images合并到（缓存的）样式表中是一种减少HTTP请求数量并避免增加网页大小的方法。不是所有主流浏览器都支持Inline images（译者注：现在几乎所有主流浏览器的最新版都已支持 [http://caniuse.com/#feat=datauri](http://caniuse.com/#feat=datauri)）。

减少网页中的HTTP请求数量是开始的第一步。这是提高首次访问性能的最重要的指南。如 Tenni Theurer 的博客 [Browser Cache Usage - Exposed！](http://yuiblog.com/blog/2007/01/04/performance-research-part-2/)中所述，你网站的每日访问者的40-60％都没有缓存。让你的网页能为这些首次访客快速响应是更好的用户体验的关键。

## 原文

> ### Minimize HTTP Requests
>tag: content

>80% of the end-user response time is spent on the front-end. Most of this time is tied up in downloading all the components in the page: images, stylesheets, scripts, Flash, etc. Reducing the number of components in turn reduces the number of HTTP requests required to render the page. This is the key to faster pages.

>One way to reduce the number of components in the page is to simplify the page's design. But is there a way to build pages with richer content while also achieving fast response times? Here are some techniques for reducing the number of HTTP requests, while still supporting rich page designs.

>Combined files are a way to reduce the number of HTTP requests by combining all scripts into a single script, and similarly combining all CSS into a single stylesheet. Combining files is more challenging when the scripts and stylesheets vary from page to page, but making this part of your release process improves response times.

>[**CSS Sprites**](http://alistapart.com/article/sprites) are the preferred method for reducing the number of image requests. Combine your background images into a single image and use the CSS `background-image` and `background-position` properties to display the desired image segment.

>[**Image maps**](https://www.w3.org/TR/html401/struct/objects.html#h-13.6) combine multiple images into a single image. The overall size is about the same, but reducing the number of HTTP requests speeds up the page. Image maps only work if the images are contiguous in the page, such as a navigation bar. Defining the coordinates of image maps can be tedious and error prone. Using image maps for navigation is not accessible too, so it's not recommended.

>**Inline images** use the [`data: URL scheme`](http://tools.ietf.org/html/rfc2397) to embed the image data in the actual page. This can increase the size of your HTML document. Combining inline images into your (cached) stylesheets is a way to reduce HTTP requests and avoid increasing the size of your pages. Inline images are not yet supported across all major browsers.

>Reducing the number of HTTP requests in your page is the place to start. This is the most important guideline for improving performance for first time visitors. As described in Tenni Theurer's blog post [Browser Cache Usage - Exposed!](http://yuiblog.com/blog/2007/01/04/performance-research-part-2/), 40-60% of daily visitors to your site come in with an empty cache. Making your page fast for these first time visitors is key to a better user experience.
