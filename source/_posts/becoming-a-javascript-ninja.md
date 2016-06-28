---
title: "[译]成为 JavaScript 忍者"
date: 2016-01-16 10:13:29
tags: JavaScript
categories: Web 前端
---

> 原文链接：[http://blog.wolksoftware.com/becoming-a-javascript-ninja](http://blog.wolksoftware.com/becoming-a-javascript-ninja)

我努力地训练着想成为大家所说的“JavaScript 忍者”。在这篇文章中我将会分享一些迄今为止我学到的比较重要的东西。

![](/imgs/ninja.jpg)

## 1. 使用代码约定

代码约定是针对一门特定编程语言的编程规范、实践和方法等一系列指导方针的集合。这些约定通常包含文件组织，缩进，注释，定义，声明，空格，命名，编程实践，编程原则，编程经验法则，架构的最佳实践等。这些是软件结构质量的指导方针。为了帮助提高代码的可读性和软件的可维护性，强烈建议软件开发工程师遵循这些指导方针。

<!--more-->

有一些工具可以帮助你确保你的团队遵循 JavaScript 的代码约定：

代码约定工具 | 描述
:---:|:---
JSLint | JSLint 是一个用于查找 JavaScript 程序问题的 JavaScript 程序。它是由 Douglas Crockford 开发的一个代码质量工具。JSLint 会扫描 JavaScript 源码。如果发现问题，它会返回描述问题的信息和在源码中的大概位置。该问题不一定是语法错误，尽管经常确实是。JSLint 也会做一些代码风格和结构的检查。它并不证明你的程序是正确的。它只是从另一个角度帮助发现问题。可以从[这里](www.jslint.com)下载 JSLint。
JSHint | JSHint 是 Anton Kovalyov 从 JSLint 创建的一个分支，因为他相信代码质量工具应该是社区驱动的并且有时候由我们自己决定是否要遵循一些代码约定。因此 JSHint 比 JSLint 更具有可配置性。可以从[这里](www.jshint.com)下载 JSHint。

## 2. 为代码编写文档

我确信你会很不赖烦的听到别人说你需要为你的代码编写文档。我确信你正在这样做但是有时候不容易发现它的价值，但是如果你创建的注释最终可以形成类似MSDN 或者 Java 文档这样的文档站点，似乎有更多的价值。幸运的是，我们也有帮助我们生成文档的工具：

文档生成工具 | 描述
:---:|:---
JsDoc Toolkit |	它是用 JavaScript 编写的一个应用程序，用于根据 JavaScript 源码中的注释自动生成通过模板格式化的多页面的 HTML（或者 XML， JSON，或者其他基于文本文件的）文档。你可以从[这里](http://usejsdoc.org/)下载 JsDoc Toolkit。

## 3. 分离关注点

在计算机科学中，关注点分离（SoC）将计算机程序分开到不同部分中的设计原则，像这样每一个部分表示一个单独的关注点。一个关注点就是一些会影响到计算机程序代码的信息。

分离关注点的价值在于简化计算机程序的开发和维护。当关注点很好的分离之后，每一个部分都能独立开发和更新了。还有一个特别的价值就是它赋予了你在以后的改善或修改一个部分的代码的时候不用关心其他部分的细节，并且不用修改其他部分的能力。

在 JavaScript 应用程序中，你的关注点就是 HTML，CSS，JavaScript 配置代码和 JavaScript 逻辑代码。为了将它们保持分离，你只需要坚持下面几个法则：

** a) 从 JavaScript 代码中移除 HTML：在 JavaScript 中嵌入 HTML 字符串是一种坏的实践。 **

``` javascript
var div = document.getElementById("myDiv");
div.innerHTML = "<h3>Error</h3><p>Invalid email adress.</p>";
```

解决这个问题的最佳方式就是通过 AJAX 从服务端加载 HTML 或者甚至从服务端加载客户端模板。有一些像 `handlebars.js` 的工具可以帮助你在客户端生成 HTML 的问题，而且不用将 HTML 字符串嵌入到 JavaScript 中。

``` javascript
// 渲染逻辑
RenderJson = function(json, template, container) {
    var compiledTemplate = Handlebars.compile(template);
    var html = compiledTemplate(json);
    $(container).html('');
    $(container).html(html);
};

// 模板
var template = "{{#each hero}}<tr><td>{{this.name}}" +
    "<td></tr>{{/each}}";

// model
var json = {
    hero : [
        { name : 'Batman' },
        { name : 'Superman' },
        { name : 'Spiderman' }
    ]
}

// 调用渲染逻辑
RenderJson(json, template, $('#heroes_tbody'));

// DOM where we will insert HTML on rendering
<table>
    <thead><tr><th>Hero</th></tr></thead>
    <tbody id="heroes_tbody">
        <!-- JS 添加的行 -->
    </tbody>
</table>
```

** b) 将 CSS 从 JavaScript 中移除 **

请勿通过 JavaScript 修改 CSS 的属性，尽量只通过 CSS 的类。

``` javascript
// bad
$(this).css( "color", "red" );

// good
$(this).addClass('redFont');
```

** c) 从 CSS 中移除 JavaScript **

如果你不了解 CSS 表达式，不要使用 CSS 表达式（一个 IE8 早期的特性），以免误入歧途。

** d) 从 HTML 中移除 CSS **

总是使用`class`，而不是通过`style`标签添加样式。

** e) 从逻辑代码中移除配置代码 **

尽量把所有的硬编码变量和常量放到一个配置对象中。

``` javascript
var CONFIG = {
    MESSAGE : {
        SUCCESS :"Success!"
    },
    DOM : {
        HEROE_LIST : "#heroes_tbody"
    }
};

// bad
RenderJson(json, template, $('#heroes_tbody'));

// good
RenderJson(json, template, $(CONFIG.DOM.HEROE_LIST));
```
如果这样做你会发现找到问题的根源会更加容易些，想象一个场景，背景颜色不对，如果你知道不会有 JavaScript 或者 HTML 涉及到你的 CSS，你自然而然就知道问题肯定处在某一个 CSS 文件中，你只需要找到那个影响到样式的 CSS `Class`，然后就完成了。

## 4. 避免全局变量

在计算机编程中，全局变量指的是在所有作用域中都能访问的变量。全局变量是一种不好的实践，因为它会导致一些问题，比如一个已经存在的方法和全局变量的覆盖，当我们不知道变量在哪里被定义的时候，代码就变得很难理解和维护了。好的 JavaScript 代码就是没有定义全局变量的。有一些技术可以帮助你让所有的事情都保持在本地：

为了避免全局变量，第一件事情就是要确保所有的代码都被包在函数中。最简单的办法就是把所有的代码都直接放到一个函数中去：
``` javascript
(function(win) {
    "use strict"; // 进一步避免创建全局变量
    var doc = window.document;
    // 在这里声明你的变量
    // 一些其他的代码
}(window));
```
最常用的避免全局变量的方式就是只为应用程序创建唯一的全局变量，像 Jquery中的 `$`。然后你可以使用一种技术叫做命名空间 `namespacing`。命名空间就是在同一全局作用域下对函数从逻辑上进行分组。

有时候每一个 JavaScript 文件都会添加一个自己的命名空间，在这种情况下，需要确保命名空间已经存在了。
```javascript
var MyApp = {
    namespace: function(ns) {
        var parts = ns.split("."),
            object = this, i, len;
        for(i = 0, len = parts.lenght; i < len; i ++) {
            if(!object[parts[i]]) {
                object[parts[i]] = {};
            }
            object = object[parts[i]];
        }
    return object;
    }
};

// 定义命名空间
MyApp.namespace("Helpers.Parsing");

// 你现在可以使用该命名空间了
MyApp.Helpers.Parsing.DateParser = function() {
    //做一些事情
};
```
另一项开发者用来避免全局变量的技术就是封装到模块 `Module` 中。一个模块就是不需要创建新的全局变量或者命名空间的通用的功能。不要将所有的代码都放一个负责执行任务或者发布接口的函数中。最常见的 JavaScript 模块类型就是异步模块定义 `Asynchronous Module Definition (AMD)`。
```javascript
//定义
define( "parsing", //模块名字
        [ "dependency1", "dependency2" ], // 模块依赖
        function( dependency1, dependency2) { //工厂方法

            // Instead of creating a namespace AMD modules
            // are expected to return their public interface
            var Parsing = {};
            Parsing.DateParser = function() {
              //do something
            };
            return Parsing;
        }
);

// 通过 Require.js 加载模块
require(["parsing"], function(Parsing) {
    Parsing.DateParser(); // 使用模块
});
```

## 5. 避免 Null 比较

特殊值 `null` 经常被错误理解并且和 `undefined` 混淆。这个值应该只能出现在一下几个场景：

** a) 初始化一个可能后面会被赋值的对象 **

** b) 和已经被初始化的但不确定是否赋过值的变量比较 **

** c) 作为参数传入一个期待参数为对象的函数 **

** d) 作为一个期待返回对象的函数的返回值 **

有一些场景是不应该使用 `null`的：

** a) 测试是否有传入参数 **

** b) 测试一个未初始化的变量值是否为 `null` **

特殊值 `undefined` 经常和 `null` 混为一谈。部分混淆来自于 `null == undefined` 的值为 `true`。然而，这两个值的用途却不同。未被初始化的变量的默认值为 `undefined`。
```javascript
//bad
var person;
console.log(person == undefined); //true
```
一般性的建议就是要避免总是使用 `undefined`。

我猜想你一定好奇如何不使用 `undefined` 和 `null` 来做下面这件事情？
```javascript
//bad
function doSomething(arg){
    if (arg != null) {
        soSomethingElse();
    }
}
```
比较一个变量和 `null` 不会给你足够的信息判断是否可以安全的进行。幸运的是，JavaScript 提供了一些方法帮助你决定一个变量的真实的值：

** a) 基本值：如果你期待一个值的类型是基本类型（string，number，boolean），那么 `typeof` 操作符就是最佳选择。 **
``` javascript
// detect a number
if(typeof count === "number") {
    //do something
}
```
** b) 引用值：`instanceof` 操作符是检测一个对象是否为特定类型的最佳方式。 **
``` javascript
// detect a date
if(value instanceof Date) {
    //do something
}
```
** c) 函数：`typeof` 操作符是检测函数的最佳方式。 **
``` javascript
// detect a function
if(MyApp.Helpers.Parsing.DateParser typeof === "function") {
    MyApp.Helpers.Parsing.DateParser();
}
```
** d) 数组：最佳方式是使用 `isArray()` 函数。唯一的问题是旧版本的 IE 不支持 `isArray`，但是你可以用下面的代码来支持多浏览器。 **
``` javascript
function isArray(value) {
    if (typeof Array.isArray === "function") {
        return Array.isArray(value);
    } else {
        return Object.prototype.toString.call(value) === "[object array]";
    }
}
```
e) 属性：`hasOwnProperty()` 函数是检测属性的最佳方式。 **
``` javascript
var hero = { name : 'superman '};
//chech property
if (name in hero) {
    // do something
}
//chech NOT inherited property
if (hero.hasOwnProperty('name')) {
    // do something
}
```
## 6. 处理错误

在 JavaScript 中抛自定义的错误可以帮助你减少调试的时间。不是那么容易得出何时应该抛自定义的错误，但是常规错误一般只有在应用程序最深层才抛。任何处理特定应用逻辑的代码应该有处理错误的能力。你可以用下面的代码创建自定义的错误：
``` javascript
function MyError(message){
    this.message = message;
}
MyError.prototype = new Error(); //extending base error class
```
检查特定的错误类型也是个好主意（Error，EvalError，RangeError，ReferenceError，SyntaxError，TypeError，URIError）使得错误处理更加健壮：
``` javascript
try {
    // Do something
} catch(ex) {
    if(ex instanceof TypeError) {
        // Handle error
    } else if (ex instanceof ReferenceError) {
        // Handle error
    } else {
        //Handle all others
    }
}
```
## 7. 不要修改不属于你的对象

有一些东西是不属于你的（不是你自己或者团队写创建的）：

** a) Native 对象 ** （e.g. Object，Array，etc.）

** b) DOM 对象 ** （e.g. document）

** c) 浏览器对象 ** （e.g. window）

** d) 库对象 ** （e.g. Jquery，$，\_，Handlebars，etc.）

有一些事情是你不能在不属于你的对象上做的：

** a) 不要复写方法 **

** b) 不要添加新方法 **

** c) 不要删除已经存在的方法 **

如果你确实需要扩展或者修改不属于你的对象，你应该创建一个类并继承它然后修改你的新类。你可以使用 JavaScript 的一种继承方式：

** a) 基于对象的继承： ** 通过调用构造函数继承。
``` javascript
var person = {
    name : "Bob",
    sayName : function() {
        alert(this.name);
    }
}
var myPerson = Object.create(person);
myPerson.sayName(); // Will display "Bob"
```
** b) 基于类型的继承： ** 一般需要两步：先原型继承然后构造函数继承。
``` javascript
function Person(name) {
    this.name = name;
}

function Author(name) {
    Person.call(this,name); // constructor inheritance
}

Author.prototype = new Person(); // prototypal inheritance
```
## 8. 测试一切事情

随着 JavaScript 应用复杂度的增长，我们应该引入和服务端或者桌面端应用使用了多年的一样的设计模式和实践，以帮助我们保证高质量的解决方案。所以是时候开始为你的 JavaScript 代码写测试了（单元测试，性能测试，集成测试……）。好消息是有一些工具可以帮助我们做这些：

a) Jasmine

b) JsTestDriver

c) PhantomJS

d) QUnit

e) Selenium

f) Yeti

g) YUI Test

## 9. 自动化一切事情

持续集成是一项软件开发实践，多个团队经常集成他们的工作，通常每个人每人至少一次——以至于每天会有多次集成。每一次集成都会通过自动化构建（包括测试）尽早检查发现错误。许多团队发现这种方式可以显著地减少集成问题并且帮助团队快速地开发出内聚的软件。

持续集成不能避免 `bug`，但是它会帮助你更容易发现并且干掉 `bug`。在这方面它更像是自测试代码。如果你引入了一个 `bug`，快速地发现它，更加容易避免它。

持续集成已经和 `TDD（测试驱动开发）` 一起用了一段时间了。但是它过去总是传统的和服务端代码联系在一起。在前面的建议中我们说到是时候为我们的 JavaScript 代码写测试了。在这个建议中我想强调也是时候去持续集成它了。

** 构建 **

在专业的开发环境中你通常会发现以下几种构建：

** a) 开发构建：** 由开发者在工作的时候运行，为了不中断生产率它应该尽可能快。

** b) 集成构建：** 会定期运行的自动化构建。可能会在每次提交之后运行一遍，但是一般在大型项目中他们倾向于每个几分钟运行一遍。

** c) 发布构建：** 在部署到产品环境之前按需运行的构建。

** 持续集成 **

有很多做持续集成的服务器但是 `Jenkins` 是其中最流行的一个，最流行的构建工具是 `Apache Ant`（译者注：现在像 Maven，Gradle 远比 Ant 流行）。构建你的 JavaScript 代码包括以下几个任务：

** a) Test automation ** 根据第8点中的讨论，你应该使用自动化工具帮助你测试你的 JavaScript 代码。

** b) Validation ** 你应该在你的构建过程中添加代码质量的验证。可以使用像 JSLint 或者 JSHint 这样的工具。

** c) Concatenation ** 你应该把所有的 JavaScript 文件连接成为一个单独的文件。

** d) Baking ** 你应该让添加 License 或者 Version 的任务作构建的一部分。

** e) Minification ** 你应该使用像 `uglify.js` 的工具让 Minify 成为集成的一部分。

** f) Compression ** 你应该让 Gzip JavaScript 代码成为够的一部分。

** g) Documentation ** 你应该使用像 JS Doc Toolkit 这样的工具让自动生成文档作为集成的一部分。

## 10. 找一位大师

一个真正的忍者从来没有停止过学习，所以你最好找一位大师！

![](/imgs/master.jpg)

从哪里可以找到一位呢？答案是互联网。我推荐阅读一些 Chrome 或者 Mozilla 的开发者关于 JavaScript 的博客和一些其他像 `jquery` 的 JS 库。
