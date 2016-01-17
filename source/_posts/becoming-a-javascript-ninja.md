---
title: 成为 JavaScript 忍者
date: 2016-01-16 10:13:29
tags: JavaScript
---

我努力地训练着想成为大家所说的“JavaScript 忍者”。在这片文章中我将会分享一些迄今为止我学到的比较重要的东西。

![忍者](../img/ninja.jpg)

## 1. 使用代码约定

代码约定是针对一门特定编程语言的编程规范、实践和方法等一系列指导方针的集合。这些约定通常包含文件组织，缩进，注释，定义，声明，空格，命名，编程实践，编程原则，编程经验法则，架构的最佳实践等。这些是软件结构质量的指导方针。为了帮助提高代码的可读性和软件的可维护性，强烈建议软件开发工程师遵循这些指导方针。

有一些工具可以帮助你确保你的团队遵循 JavaScript 的代码约定：

| 代码约定工具 | 描述 |
|------------|-----|
| JSLint | JSLint 是一个用于查找 JavaScript 程序问题的 JavaScript 程序。它是由 Douglas Crockford 开发的一个代码质量工具。JSLint 会扫描 JavaScript 源码。如果发现问题，它会返回描述问题的信息和在源码中的大概位置。该问题不一定是语法错误，尽管经常确实是。JSLint 也会做一些代码风格和结构的检查。它并不证明你的程序是正确的。它只是从另一个角度帮助发现问题。可以从[这里](www.jslint.com)下载 JSLint。|
| JSHint | JSHint 是 Anton Kovalyov 从 JSLint 创建的一个分支，因为他相信代码质量工具应该是社区驱动的并且有时候由我们自己决定是否要遵循一些代码约定。因此 JSHint 比 JSLint 更具有可配置性。可以从[这里](www.jshint.com)下载 JSHint。 |

## 2. 为代码编写文档

我确信你会很不赖烦的听到别人说你需要为你的代码编写文档。我确信你正在这样做但是有时候不容易发现它的价值，但是如果你创建的注释最终可以形成类似MSDN 或者 Java 文档这样的文档站点，似乎有更多的价值。幸运的是，我们也有帮助我们生成文档的工具：

| 文档生成工具 | 描述 |
|------------|-----|
| JsDoc Toolkit |	它是用 JavaScript 编写的一个应用程序，用于根据 JavaScript 源码中的注释自动生成通过模板格式化的多页面的 HTML（或者 XML， JSON，或者其他基于文本文件的）文档。你可以从[这里](http://usejsdoc.org/)下载 JsDoc Toolkit。 |

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
```

``` html
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

The general recommendation is to avoid using undefined at all times.

I guess that you must be know wondering how should you do the following without using undefined or null?
```javascript
//bad
function doSomething(arg){
    if (arg != null) {
        soSomethingElse();
    }
}
```
Comparing a variable against null doesn’t give you enough information about the value to determine whether is safe to proceed. Furtunately, JavaScript gives youi a few ways to determine the true value of a variable:

a) Primitive values: If your expecting a value to be a primitive type (sting, number, boolean) then the typeof operator is your best option.
``` javascript
// detect a number
if(typeof count === "number") {
    //do something
}
```
b) Reference values: The instanceof operator is the best way to detect objects of a particular reference type.
``` javascript
// detect a date
if(value instanceof Date) {
    //do something
}
```
** c) Functions: Using typeof is the best way to detect functions. **
``` javascript
// detect a function
if(MyApp.Helpers.Parsing.DateParser typeof === "function") {
    MyApp.Helpers.Parsing.DateParser();
}
```
d) Arrays: The best way to detect arrays is to use the isArray() function. The only problem is that isArray does not work in old versions of internet explorer. But you can sue the following code for cross browser support.
``` javascript
function isArray(value) {
    if (typeof Array.isArray === "function") {
        return Array.isArray(value);
    } else {
        return Object.prototype.toString.call(value) === "[object array]";
    }
}
```
e) Properties: The best way to detect a property is to use the in operator or the hasOwnProperty() function.
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
6. Handle errors

Throwing custom errors in JavaScript can help you to decrease your debugging time. It is not easy to know when you should throw a custom error but in general errors should be thrown only in the deepest part of your application stack. Any code that handles application-especific logig should have error-handling capabilities. You can use the following code to create your custom errors:
``` javascript
function MyError(message){
    this.message = message;
}
MyError.prototype = new Error(); //extending base error class
```
Is also a good idea to chek for specifict error types (Error, EvalError, RangeError, ReferenceError, SyntaxError, TypeError, URIError) to have a more robust error handling:
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
7. Don’t modify objects that you don’t own

There are things that you don’t own (objects that has not been written by you or your team)

a ) Native objects (e.g. Object, Array, etc.)

b ) DOM objects (e.g. document)

c ) Browser object model (e.g. window)

d ) Library objects (e.g. Jquery, $, _, Handlebars, etc.)

And thing that you should not try on objects that you don’t own:

a ) Don’t override methods

b ) Don’t add new methods

c ) Don’t remove existing methods

If you really need to extend or modify an object that you don’t own, you should create a class that inherits from it and modify your new object. You can use one of the JavaScript inheritance basic forms:

a) Object-based Inheritance: an object inherits from another without calling a constructor function.
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
a) Type-based inheritance: tipically requires two steps: prototypal inheritance and then constructor inheritance.
``` javascript
function Person(name) {
    this.name = name;
}

function Author(name) {
    Person.call(this,name); // constructor inheritance
}

Author.prototype = new Person(); // prototypal inheritance
```
8. Test everything

As the complexity of JavaScript applications grows we must introduce the same design patterns and preactices that we have been using for years in our server-side and desktop applications code to ensure high quality solutions. So it is time to start writting tests (unit, performance, integration…) for your JavaScript code. The good news is that there are several tools that you can use to help you:

a) Jasmine

b) JsTestDrive

c) PhantonJS

d) QUnit

e) Selenium

f) Yeti

g) YUI Test


9. Automate everything

Continuous Integration is a software development practice where members of a team integrate their work frequently, usually each person integrates at least daily - leading to multiple integrations per day. Each integration is verified by an automated build (including test) to detect integration errors as quickly as possible. Many teams find that this approach leads to significantly reduced integration problems and allows a team to develop cohesive software more rapidly.

Continuous Integrations doesn’t get rid of bugs, but it does make them dramatically easier to find and remove. In this respect it’s rather like self-testing code. If you introduce a bug and detect it quickly it’s far easier to get rid of.

Continuous Integration has been used for a while together with TDD (Test Driven Development). But it was more traditionally linked to server-side code. In the previous point we said that it is time to test our JavasCript code. In this point I want to highlight that it is also time to continuous integrate it.

Build

In a professional development environment you will normaly find the following types of build:

a) Development Build: Run by developers as they are working, it should be as fast as possible to not interrupt productivity.

b) Integration Build: An automated build that run on a regular schedule. These are sometimes run for each commit, but on large projects, they tend to run on intervals for a few minutes.

c) Release Build: an on-deman build that is run prior to production push.

Continuous integration

There are serveral continuous integration servers but the most popular is Jenkings, the most popular build tool is apache ant. The process of building your JavaScript code includes several taks:

a) Test automation As discussed on point 8 you should use test automation tools for testing your JavaScript code.

b) Validation You should add code quality validation to your build process. You can use tools like JSLint or JSHint

c) Concatenation You should join all your JavaScript files in one single file.

d) Baking You can perform tasks like adding a code the license or version code as part of the build.

e) Minification You should integrate as part the build the minification by tools like uglify.js.

f) Compression You should gzip your JavaScript as part of your build.

g) Documentation You should integrate as part of your build the auto-generation id the dosumentation by tools like JS Doc Toolkit.


10. Find a great master

A real ninja never stop learning, so you better find a great master!

pai-mei1.jpg

Where can you find one? The answer is of course the Internet. I recommend to read the blogs of some of the chome or mozilla developers about javascript as well as other js libraries like jquery.
