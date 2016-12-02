---
title: "前端性能调优(2) ┃ 使用 CDN"
date: 2016-12-02 17:31:47
tags: [Best Practice, 翻译]
categories: [Web, Performance]
---
<img src="/imgs/cdn.png" width="100%">

> *原文链接：[https://developer.yahoo.com/performance/rules.html#cdn](https://developer.yahoo.com/performance/rules.html#cdn)*

用户与Web服务器的距离对响应时间有影响。将你的内容部署在多个地理位置的分布式的服务器上，可以使得用户更快地加载网页。但是你应该从哪里开始呢？

<!-- more -->

作为实现从地理的角度分散内容的第一步，不要尝试重新设计你的Web应用程序使其能在分布式体系结构中工作。根据应用程序，更改架构可能会包括艰巨的任务，如跨服务器同步会话状态和复制数据库事务。更改应用程序体系结构可能会使你尝试减少用户和你的内容之间的距离的步骤延迟或者根本通过不了。

请记住，终端用户的响应时间的80-90％用于下载页面中的所有组件：图片（images)，样式表(stylesheets)，脚本(scripts)，Flash等，这是性能黄金法则。与其从重新设计应用程序架构的艰巨任务开始，最好先分散你的静态内容。这不仅实现了大量缩短响应时间，而且更加简单都归功于内容分发网络。

内容分发网络（CDN）是分布在多个位置的web服务器的集合，以更高效地向用户分发内容。被选择用于向特定用户分发内容的服务器通常基于网络接近度的测量。例如，选择具有最少网络跳数的服务器或具有最快响应时间的服务器。

一些大型互联网公司拥有自己的CDN，但使用CDN服务提供商（例如[Akamai Technologies](http://www.akamai.com/)，[EdgeCast](http://www.edgecast.com/)或[level3](http://www.level3.com/index.cfm?pageID=36)）是具有成本效益的。对于初创公司和私人网站，CDN服务的成本可能是令人望而却步的，但是随着您的目标受众变得越来越大并变得越来越全球化，CDN是实现快速响应时间所必需的。在Yahoo!，将静态内容从其应用程序Web服务器迁移到CDN（如上所述的第三方以及Yahoo自己的[CDN](https://cwiki.apache.org/TS/traffic-server.html)）使得终端用户的响应时间缩短了20％或更多。迁移到CDN是一个相对容易的将会大大提高您的网站的速度的代码更改。

## 原文

> ### Use a Content Delivery Network
>tag: server

>The user's proximity to your web server has an impact on response times. Deploying your content across multiple, geographically dispersed servers will make your pages load faster from the user's perspective. But where should you start?

>As a first step to implementing geographically dispersed content, don't attempt to redesign your web application to work in a distributed architecture. Depending on the application, changing the architecture could include daunting tasks such as synchronizing session state and replicating database transactions across server locations. Attempts to reduce the distance between users and your content could be delayed by, or never pass, this application architecture step.

>Remember that 80-90% of the end-user response time is spent downloading all the components in the page: images, stylesheets, scripts, Flash, etc. This is the Performance Golden Rule. Rather than starting with the difficult task of redesigning your application architecture, it's better to first disperse your static content. This not only achieves a bigger reduction in response times, but it's easier thanks to content delivery networks.

>A content delivery network (CDN) is a collection of web servers distributed across multiple locations to deliver content more efficiently to users. The server selected for delivering content to a specific user is typically based on a measure of network proximity. For example, the server with the fewest network hops or the server with the quickest response time is chosen.

>Some large Internet companies own their own CDN, but it's cost-effective to use a CDN service provider, such as [Akamai Technologies](http://www.akamai.com/), [EdgeCast](http://www.edgecast.com/), or [level3](http://www.level3.com/index.cfm?pageID=36). For start-up companies and private web sites, the cost of a CDN service can be prohibitive, but as your target audience grows larger and becomes more global, a CDN is necessary to achieve fast response times. At Yahoo!, properties that moved static content off their application web servers to a CDN (both 3rd party as mentioned above as well as Yahoo’s own [CDN](https://cwiki.apache.org/TS/traffic-server.html)) improved end-user response times by 20% or more. Switching to a CDN is a relatively easy code change that will dramatically improve the speed of your web site.
