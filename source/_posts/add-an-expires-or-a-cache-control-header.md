---
title: "前端性能调优(3) ┃ 添加 Expires 或者 Cache-Control 头"
date: 2016-12-13 22:58:13
tags: [Best Practice, 翻译]
categories: [Web, Performance]
---

<img src="/imgs/expires.jpg" width="100%">

> *原文链接：[https://developer.yahoo.com/performance/rules.html#expires](https://developer.yahoo.com/performance/rules.html#expires)*

分两个方面：

* 对于静态组件：通过设置未来很远的`Expires`头来实现“永不过期”策略
* 对于动态组件：使用适当的`Cache-Control`头来帮助浏览器实现按条件请求

网页设计越来越丰富，这意味着页面中有更多的脚本（script），样式表（stylesheet），图像（image）和Flash。你的网页的首次访问者可能必须发出几个HTTP请求，但通过使用`Expires`头，你可以使这些组件成为可缓存的。这样可避免后续网页浏览中出现不必要的HTTP请求。`Expires`头最常用于图片，但它们应该用于所有组件，包括脚本，样式表和Flash组件。

<!-- more -->

浏览器（和代理）使用缓存减少HTTP请求的数量和大小，使网页加载更快。Web服务器使用HTTP响应中的`Expires`头来告诉客户端该组件可以缓存多长时间。这是一个未来的`Expires`头，它告诉浏览器，一直到2010年4月15日（译者注：原文完成早于这个时间）这个响应都不会过期。

```
Expires: Thu, 15 Apr 2010 20:00:00 GMT
```

如果你的服务器是Apache，使用`ExpiresDefault`指令设置相对于当前日期的过期时间。 下面这个例子使用`ExpiresDefault`指令将`Expires`日期设置为从请求时起10年。

```
      ExpiresDefault "access plus 10 years"
```

请记住，如果你使用将来很远的日期作为`Expires`头，你必须在组件更改时更改组件的文件名。在Yahoo!，我们通常将此步骤作为构建过程的一部分：会把版本号作为组件的文件名的一部分，例如，yahoo_2.0.6.js。

使用将来的时间作为`Expires`头只会影响用户首次访问你的网站后，后续的网页浏览。当用户首次访问你的网站并且浏览器的缓存为空时，它对HTTP请求的数量没有影响。因此，该性能提升的效果取决于用户访问到你网页预处理缓存的频率。（“预处理缓存”已经包含页面中的所有组件。）在Yahoo!，我们[测量](http://yuiblog.com/blog/2007/01/04/performance-research-part-2/)这一点时发现带有缓存的页面查看次数为75-85％。通过使用未来的Expires头，您可以增加浏览器缓存的组件数量，并在后续页面视图上重用，而无需通过用户的网络连接发送一个字节。


## 原文

> ### Add an Expires or a Cache-Control Header
>tag: server

>There are two aspects to this rule:

>For static components: implement "Never expire" policy by setting far future `Expires` header
For dynamic components: use an appropriate `Cache-Control` header to help the browser with conditional requests

>Web page designs are getting richer and richer, which means more scripts, stylesheets, images, and Flash in the page. A first-time visitor to your page may have to make several HTTP requests, but by using the Expires header you make those components cacheable. This avoids unnecessary HTTP requests on subsequent page views. Expires headers are most often used with images, but they should be used on all components including scripts, stylesheets, and Flash components.

>Browsers (and proxies) use a cache to reduce the number and size of HTTP requests, making web pages load faster. A web server uses the Expires header in the HTTP response to tell the client how long a component can be cached. This is a far future Expires header, telling the browser that this response won't be stale until April 15, 2010.

```
      Expires: Thu, 15 Apr 2010 20:00:00 GMT
```

>If your server is Apache, use the ExpiresDefault directive to set an expiration date relative to the current date. This example of the ExpiresDefault directive sets the Expires date 10 years out from the time of the request.

```
      ExpiresDefault "access plus 10 years"
```

>Keep in mind, if you use a far future Expires header you have to change the component's filename whenever the component changes. At Yahoo! we often make this step part of the build process: a version number is embedded in the component's filename, for example, yahoo_2.0.6.js.

>Using a far future Expires header affects page views only after a user has already visited your site. It has no effect on the number of HTTP requests when a user visits your site for the first time and the browser's cache is empty. Therefore the impact of this performance improvement depends on how often users hit your pages with a primed cache. (A "primed cache" already contains all of the components in the page.) We [measured this at Yahoo!](http://yuiblog.com/blog/2007/01/04/performance-research-part-2/) and found the number of page views with a primed cache is 75-85%. By using a far future Expires header, you increase the number of components that are cached by the browser and re-used on subsequent page views without sending a single byte over the user's Internet connection.
