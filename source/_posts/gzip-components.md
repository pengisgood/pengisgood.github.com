---
title: "前端性能调优(4) ┃ Gzip 压缩"
date: 2016-12-15 18:56:15
tags: [Best Practice, 翻译]
categories: [Web, Performance]
---

<img src="/imgs/gzip.png" width="100%">

> *原文链接：[https://developer.yahoo.com/performance/rules.html#gzip](https://developer.yahoo.com/performance/rules.html#gzip)*

通过网络传输 HTTP 请求和响应所需的时间可以根据前端工程师做出的决定大大减少。的确，终端用户的带宽速度、互联网服务提供商、对等交换点的距离等都超出了开发团队的控制范围。但还有其他影响响应时间的变量。压缩通过减少 HTTP 响应的大小来减少响应时间。

<!-- more -->

从 HTTP/1.1 开始，Web 客户端在 HTTP 请求中的 `Accept-Encoding` 头中指定是否支持压缩。

```
      Accept-Encoding: gzip, deflate
```

如果 Web 服务器在请求中看到此标头，它可以使用客户端列出的方法之一来压缩响应。Web 服务器通过响应中的 `Content-Encoding` 头来通知 Web 客户端。

```
      Content-Encoding: gzip
```

Gzip 是目前最流行和最有效的压缩方法。它是由 GNU 项目开发的，并在 [RFC 1952](http://www.ietf.org/rfc/rfc1952.txt) 中标准化。你可能看到的唯一其他压缩格式是 `deflate`，但它的效率较低且没有那么流行。

Gzip 通常将响应大小减小约70％。目前浏览器中大约90％的互联网流量声称支持 gzip。如果使用 Apache，那么配置 gzip 的模块取决于你的版本：Apache 1.3 使用 [mod_gzip](https://sourceforge.net/projects/mod-gzip/)，而Apache 2.x 使用 [mod_deflate](http://httpd.apache.org/docs/2.0/mod/mod_deflate.html)。

有一些已知的浏览器和代理问题，可能导致浏览器期望的内容与接收到的压缩内容不匹配。幸运的是，这些边缘情况随着旧版浏览器的使用率下降正在逐渐减少。Apache 模块也会自动添加适当的 `Vary` 响应头来帮助解决这些问题。

服务器根据文件类型选择 gzip 的内容，但是它们决定压缩的内容通常太受限制。大多数网站 gzip 它们的 HTML 文档。还有一点值得 gzip 的是脚本和样式表，但许多网站错过了这个机会。事实上，压缩任何文本响应，包括 XML 和 JSON 都是值得压缩的。**图像和 PDF 文件不应该 gzip，因为它们已经压缩过了**。尝试 gzip 它们不仅浪费 CPU，还有可能增加文件大小。

Gzip 尽可能多的文件类型是减少页面重量和加快用户体验的简单方法。

## 原文

> ### Gzip Components
>tag: server
>
>The time it takes to transfer an HTTP request and response across the network can be significantly reduced by decisions made by front-end engineers. It's true that the end-user's bandwidth speed, Internet service provider, proximity to peering exchange points, etc. are beyond the control of the development team. But there are other variables that affect response times. Compression reduces response times by reducing the size of the HTTP response.
>
>Starting with HTTP/1.1, web clients indicate support for compression with the Accept-Encoding header in the HTTP request.
>
```
      Accept-Encoding: gzip, deflate
```

>If the web server sees this header in the request, it may compress the response using one of the methods listed by the client. The web server notifies the web client of this via the Content-Encoding header in the response.
```
      Content-Encoding: gzip
```

>Gzip is the most popular and effective compression method at this time. It was developed by the GNU project and standardized by [RFC 1952](http://www.ietf.org/rfc/rfc1952.txt). The only other compression format you're likely to see is deflate, but it's less effective and less popular.
>
>Gzipping generally reduces the response size by about 70%. Approximately 90% of today's Internet traffic travels through browsers that claim to support gzip. If you use Apache, the module configuring gzip depends on your version: Apache 1.3 uses [mod_gzip](https://sourceforge.net/projects/mod-gzip/) while Apache 2.x uses [mod_deflate](http://httpd.apache.org/docs/2.0/mod/mod_deflate.html).
>
>There are known issues with browsers and proxies that may cause a mismatch in what the browser expects and what it receives with regard to compressed content. Fortunately, these edge cases are dwindling as the use of older browsers drops off. The Apache modules help out by adding appropriate Vary response headers automatically.
>
>Servers choose what to gzip based on file type, but are typically too limited in what they decide to compress. Most web sites gzip their HTML documents. It's also worthwhile to gzip your scripts and stylesheets, but many web sites miss this opportunity. In fact, it's worthwhile to compress any text response including XML and JSON. Image and PDF files should not be gzipped because they are already compressed. Trying to gzip them not only wastes CPU but can potentially increase file sizes.
>
>Gzipping as many file types as possible is an easy way to reduce page weight and accelerate the user experience.
