---
title: 多个 ng-app 中 Controllers & Services 之间的通信
date: 2016-01-31 22:15:32
tags: [JavaScript, AngularJS]
categories: Web 前端
---

通常情况下，在 Angular 的单页面应用中不同的 Controller 或者 Service 之间通信是一件非常容易的事情，因为 Angular 已经给我们提供了一些便利的方法：`$on`，`$emit`，`$broadcast`。

在这里用一个简单的例子来演示一下这三个方法的用途，完整版代码也可以[参考这里](https://plnkr.co/edit/aOHDVE)：
``` css style.css
body {
  background-color: #eee;
}

#child {
  background-color: red;
}

#grandChild {
  background-color: yellow;
}

#sibling {
  background-color: pink;
}

.level {
  border: solid 1px;
  margin: 5px;
  padding: 5px;
}
```
``` html index.html
<body ng-app="app" ng-controller="ParentCtrl" class='level'>
  <h2>Parent</h2>
  <button ng-click="broadcastMsg()">Broadcast msg</button>
  <button ng-click="emitMsg()">Emit msg</button>
  <pre>Message from: {{message}}</pre>
  <div id='child' ng-controller="ChildCtrl" class='level'>
    <h2>Child</h2>
    <button ng-click="broadcastMsg()">Broadcast msg</button>
    <button ng-click="emitMsg()">Emit msg</button>
    <pre>Message from: {{message}}</pre>

    <div id='grandChild' ng-controller="GrandChildCtrl" class='level'>
      <h2>Grand child</h2>

      <pre>Message from: {{message}}</pre>
    </div>
  </div>
  <div id='sibling' ng-controller="SiblingCtrl" class='level'>
    <h2>Sibling</h2>
    <button ng-click="broadcastMsg()">Broadcast msg</button>
    <button ng-click="emitMsg()">Emit msg</button>
    <pre>Message from: {{message}}</pre>
  </div>
</body>
```

``` javascript app.js
var app = angular.module('app', [])
app.controller('ParentCtrl', function($scope) {
  $scope.message = ''

  $scope.broadcastMsg = function() {
    $scope.$broadcast('msg_triggered','parent')
  }

  $scope.emitMsg = function() {
    $scope.$emit('msg_triggered','parent')
  }

  $scope.$on('msg_triggered', function(event, from){
    $scope.message = from
  })
})

app.controller('ChildCtrl', function($scope) {
  $scope.message = ''
  $scope.broadcastMsg = function() {
    $scope.$broadcast('msg_triggered','child')
  }

  $scope.emitMsg = function() {
    $scope.$emit('msg_triggered','child')
  }

  $scope.$on('msg_triggered', function(event, from){
    $scope.message = from
  })
})

app.controller('GrandChildCtrl', function($scope) {
  $scope.message = ''

  $scope.$on('msg_triggered', function(event, from){
    $scope.message = from
  })
})

app.controller('SiblingCtrl', function($scope) {
  $scope.message = ''
  $scope.broadcastMsg = function() {
    $scope.$broadcast('msg_triggered','sibling')
  }

  $scope.emitMsg = function() {
    $scope.$emit('msg_triggered','sibling')
  }

  $scope.$on('msg_triggered', function(event, from){
    $scope.message = from
  })
})
```

在上面的例子中我们可以看出，利用 Angular 已有的一些 API 能够很方便的在不同 Controller 之间通信，仅需要广播事件即可。

上面的代码之所以能工作，是因为我们一直都有着一个前提，那就是这些 Controller 都在同一个 ng-app 中。那么，如果在一个页面中存在多个 ng-app 呢？（尽管并不推荐这样做，但是在真实的项目中，尤其是在一些遗留项目中，仍然会遇到这种场景。）

先看一个简单的例子：
``` css style.css
.app-container {
  height: 200px;
  background-color: white;
  padding: 10px;
}

pre {
  font-size: 20px;
}
```
``` html index.html
<body>
  <div class="app-container" ng-app="app1" id="app1"  ng-controller="ACtrl">
    <h1>App1</h1>
    <pre ng-bind="count"></pre>
    <button ng-click="increase()">Increase</button>
  </div>
  <hr />
  <div class="app-container" ng-app="app2" id="app2"  ng-controller="BCtrl">
    <h1>App2</h1>
    <pre ng-bind="count"></pre>
    <button ng-click="increase()">Increase</button>
  </div>
</body>
```
``` javascript app.js
angular
  .module('app1', [])
  .controller('ACtrl', function($scope) {
    $scope.count = 0;

    $scope.increase = function() {
      $scope.count += 1;
    };
  });

angular
  .module('app2', [])
  .controller('BCtrl', function($scope) {
    $scope.count = 0;

    $scope.increase = function() {
      $scope.count += 1;
    };
  });
```

## Angular 的启动方式
直接运行这段代码，我们会发现第二个 ng-app 并没有工作，或者说第二个 ng-app 并没有自动启动。为什么会这样呢？相信对 Angular 了解比较多的人会马上给出答案，那就是 Angular 只会自动启动找到的第一个 ng-app，后面其他的 ng-app 没有机会自动启动。

如何解决这个问题呢？我们可以手动启动后面没有启动的ng-app。举个例子：
``` html hello_world.html
<!doctype html>
<html>
<body>
  <div ng-controller="MyController">
    Hello {{greetMe}}!
  </div>
  <script src="http://code.angularjs.org/snapshot/angular.js"></script>

  <script>
    angular.module('myApp', [])
      .controller('MyController', ['$scope', function ($scope) {
        $scope.greetMe = 'World';
      }]);

    angular.element(document).ready(function() {
      angular.bootstrap(document, ['myApp']);
    });
  </script>
</body>
</html>
```
手动启动需要注意两点：一是当使用手动启动方式时，DOM 中不能再使用 ng-app 指令；二是手动启动不会凭空创建不存在的 module，因此需要先加载 module 相关的代码，再调用`angular.bootstrap`方法。如果你对 Angular 的启动方式还是不太明白的话，请参考[官方文档](https://code.angularjs.org/1.4.9/docs/guide/bootstrap)。

现在关于Angular 启动的问题解决了，可能有的人会问，如果我的页面中在不同的地方有很多需要手动启动的 ng-app 怎么办呢？难道我要一遍一遍的去调用`angualar.bootstrap`吗？这样的代码看上去总觉得哪里不对，重复的代码太多了，因此我们需要重构一下。这里重构的方式可能多种多样，我们采用的方式是这样的：
``` javascript main.js
$(function(){
  $('[data-angular-app]').each(function(){
    var $this = $(this)
    angular.bootstrap($this, [$this.attr('data-angular-app']))
  })
})
```
先将代码中所有的 ng-app 改为 data-angular-app，然后在 document ready 的时候用 jQuery 去解析 DOM 上所有的`data-angular-app`属性，拿到 ng-app 的值，最后用手动启动的方式启动 Angular。

## Mini Pub-Sub
趟过了一个坑，我们再回到另一个问题上，如何才能在多个 ng-app 中通信呢？毕竟它们都已经不在相同的 context 中了。这里需要说明一下，在 Angular 中 ng-app 在 DOM 结构上是不能有嵌套关系的。每个 ng-app 都有自己的 rootScope，我们不能再直接使用 Angular 自己提供的一些 API 了。因为不管是 `$broadcast` 还是`$emit`，它们都不能跨越不同的 ng-app。相信了解发布订阅机制的人（尤其是做过 WinForm 程序的人）能够很快想到一种可行的解决方案，那就是我们自己实现一个简易的发布订阅机制，然后通过发布订阅自定义的事件在不同的 ng-app 中通信。

听起来感觉很简单，实际上做起来也很简单。`Talk is cheap, show me the code.`

首先我们需要一个管理事件的地方，详细的解释[参考 StackOverflow 上的这个帖(http://stackoverflow.com/a/2969692/3049524)。
``` javascript event_manager.js
(function($){
  var eventManager = $({})

  $.subscribe = function(){
    eventManager.bind.apply(eventManager, fn)
  }

  $.publish = function(){
    eventManager.trigger.apply(eventManager, fn)
  }
})(jQuery)
```
暂时只实现了两个 API，一个`subscribe`用于订阅事件，`publish`用于发布事件。

订阅事件:
``` javascript
$.subscribe('user_rank_changed', function(event, data){
  $timeout(function(){
    // do something
  })
})
```

发布事件：
``` javascript
$.publish('user_rank_changed', {/*some data*/})
```
这里用了一个小 trick，因为我们的事件发布订阅都是采用的 jQuery 方式，为了让 Angular 能够感知到 scope 上数据的变化，我们将整个回调函数包在了`$timeout`中，由 JavaScript 自己放到时间循环中去等到空闲的时候开始执行，而不是使用`$scope.$apply()`方法，是因为有些时候直接调用该方法会给我们带来另一个`Error: $digest already in progress`的错误。虽然也可以用`$rootScope.$$phase || $rootScope.$apply();`这种方式来规避，但是个人认为还是略显 tricky，没有`$timeout` 的方式优雅。

因为我们用的是原生的 JavaScript 的事件机制，所以即使我们的 Controller 或者 Service 处于不同的 ng-app 中，我们也能够轻松地相互传输数据了。

## 改进原则
在Angular 的单页面应用中，我们尽量一个应用只有一个 ng-app，然后通过 Module 对业务进行模块划分，而不是 ng-app。不到万不得已，不要和 jQuery 混着用，总是使用 Angular 的思维方式进行开发，否则一不小心就会掉进数据不同步的坑中。
