---
title: ES6 30 分钟入门教程
date: 2016-06-21 21:29:02
tags: [ES6, JavaScript]
categories: Web 前端
---
![](/imgs/ES6-the-bits-youll-actually-use.png)

## 本文目标

本文的目标是让你在30分钟之内对 ES6 是什么，并且对它有一些基本的了解，前提是你有一定的 JavaScript 基础。

## ECMAScript 和 JavaScript 是什么关系？

ES6，规范中称之为 ECMAScript 2015（因为是2015年发布的，简称 ES 2015），又因为是该标准的第六版，所有也叫 ECMAScript 6，或者 ES 6。本文纯粹为了少敲几个字符，统一使用 ES6。用一句话概括它们二者之间的关系为：ECMAScript 是 JavaScript 的规格，JavaScript 是 ECMAScript 的实现。既然 ECAMScript 是规格，想必它的的实现应该不止一种，比如微软的 JScript 和 Macromedia（现已被 Adobe 收购）的 ActionScript。更加详细的历史渊源请移步 [Wikipedia](https://en.wikipedia.org/wiki/ECMAScript)。

<!--more-->

## 语法特性

> 本文中的大部分例子来源于阮博士的书[《ECMAScript 6入门》](http://es6.ruanyifeng.com/)

### 1. let 和 const

* let

ES6 引入了 `let` 用作声明变量，用法和之前的 `var` 类似，但是声明之后的效果却有很大的区别。
a) `let` 声明的变量作用域是块级的（block scope），`var` 声明名的变量是函数级的（function scope）。
b) `let` 声明的变量不存在作用域提升。不像 `var`，如果先使用后声明会取得值 undefined。如果先使用再用`let`声明，会报错。
c) `let` 在同一作用域下，不允许重复声明，而`var` 可以。

* const

`const` 声明一个只读的常量。
a) `const` 声明的常量，一旦声明，就必须立即初始化，不能留到以后赋值。
b) `const` 声明的常量作用域和 `let` 相同，也是块级的。
c) `const` 声明的常量也不存在作用域提升。
d) 在同一作用域下，和 `let` 一样，不能重复声明 `const` 常量。
e) 对于引用类型的变量，`const` 声明只是保证变量名所指向的地址不变，并不保证改地址保存的数据不变。

### 2. Destructuring

ES6允许按照一定模式，从数组或对象中提取值，对变量进行赋值，被称为解构（Destructuring）。其用法和 Ruby、Scala 等的模式匹配（Pattern Match）比较类似。

* 数组

只要等号两边的模式相同，等号左边的变量就会被赋予相应的值。有两种特殊情况，一种是等号右边的值不能被解构，这时左边的变量值为 `undefined`；另一种是不完全解构，即等号左边的模式只匹配右边部分值。

```javascript
var [a, b, c] = [1, 2, 3];
//a = 1, b = 2, c = 3

let [e, f] = [4, 5];
//e = 4, f = 5

const [x, , y] = [1, 2, 3];
//x = 1, y = 3

let [head, ...tail] = [1, 2, 3, 4];
//head = 1, tail = [2, 3, 4]

let [x, y, ...z] = ['a'];
//x = 'a', y = undefined, z = []
```

* 对象

对象的解构和数组不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量的取值来自于和它同名的属性，如果没有同名的属性则值为 `undefined`。

```javascript
var { bar, foo } = { foo: "aaa", bar: "bbb" };
//foo = "aaa", bar = "bbb"

var { baz } = { foo: "aaa", bar: "bbb" };
//baz = undefined
```

上面这种解构赋值实际上是一种简写，完整的写法如下：

```javascript
var { bar: bar, foo: foo } = { foo: "aaa", bar: "bbb" };
//foo = "aaa", bar = "bbb"

var { baz: baz } = { foo: "aaa", bar: "bbb" };
//baz = undefined
```

对象的解构机制，是先根据冒号前面的模式从等号右边对象中找到同名属性，然后将其值赋给冒号右边的变量。如果模式和变量名相同，则可以采用简写形式，否则必须采用完整写法。这种机制不仅适用于简单的对象，同样也适用于复杂的对象。比如下面这个复杂的例子：

```javascript
var node = {
  location: {
    start: {
      row: 1,
      column: 5
    }
  }
};

var { location: { start: { row }} } = node;
//line = 1
//location -> error: location is undefined 因为 location 是模式，而非变量
//start -> error: start is undefined 同上
```

* 字符串

字符串可以被转换成类似数组的对象，因此也可以解构赋值。

```javascript
let [a, b, c, d] = 'peng';
//a = 'p', b = 'e', c = 'n', d = 'g'
```

* 数值 & 布尔值

结构的另一条规则是如果等号右边的值不是对象，就会先尝试将其转为对象，然后采用对象的结构规则。这里值得注意的是，因为 `undefined` 和 `null` 无法转为对象，所以对它们解构赋值会报错。

```javascript
let {toString: n} = 123;
n === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true

let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```

* 函数参数

函数的参数支持结构赋值，其规则和数组、对象的解构规则一致。

```javascript
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3

function move({x, y}) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
```

### 3. Template String

在以前的 JavaScript 编程过程中，拼接字符串的场景在所难免。我们往往会写出如下代码：

```javascript
function description(tasks){
  return 'There are <b>' + tasks.count + '</b> in total, and <b>' + tasks.done + '</b> are done.';
}
```

这种写法非常原始和繁琐，在 ES6 里面引入了模板字符串，用反引号（ ` ` ` ）标识。它可以当作普通单行字符串使用，也可以作为多行字符串使用，可以嵌入变量，函数调用。模板字符串中嵌入变量，需要将变量名或函数调用写在`${ }`之中。

```javascript
var [x, y] = [1, 2];

`${x} + ${y * 2} = ${x + y * 2}`
// "1 + 4 = 5"

function fn() {
  return "Hello World";
}

`foo ${fn()} bar`
//foo Hello World bar
```

### 4. Class

JavaScript 在 ES6 之前，定义对象都是通过构造函数的方式进行的。如下

* 基本用法

```javascript
function Point(x,y){
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};
```

这种写法与其他的面向对象的语言表示方法区别比较大，比如 Java。一开始让刚接触这门语言的人很不能理解。而 ES6 引入了 `class` 关键字，提供了一个语法糖（syntax sugar）。让 JavaScript 的面向对象的写法更加直观，和其他语言的面向对象的写法更加相似。

```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

let p = new Point(1, 2);
```

上面的写法看着是否更加亲切？有构造函数，有实例方法。

对于 `constructor` 方法，有些需要注意的地方。 `constructor` 方法是类的默认方法，通过 `new` 命令生成对象实例时，自动调用该方法。一个类必须有 `constructor` 方法，如果没有显式定义，一个空的 `constructor` 方法会被默认添加。 `constructor` 方法默认返回实例对象（即 `this` ），当然你也可以返回另外一个对象，通常不建议你这么干，除非是你猴子🐵派来的逗逼。

另外，类的定义不存在作用域提升。如果先使用后定义会报错。

* 继承

ES6 引入了 `extends` 关键字， `class` 之间可以通过 `extends` 很方便的实现继承。

```javascript
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```

这里在 `constructor` 和 `toString` 方法中都出现了 `super` 关键字。这个关键字有两种用法，一种是作为函数调用，此时 `super` 代表父类的 `constructor` 方法；另一种是作为对象调用，此时 `super` 代表的是父类，既可以应用父类实例的方法和属性，也可以引用父类的静态方法。

如果子类定义了 `constructor` 方法，子类必须在 `constructor` 方法中调用 `super` 方法，否则新建实例时会报错。这是因为子类没有自己的 `this` 对象，而是继承父类的 `this` 对象，然后对其进行加工。如果不调用 `super` 方法，子类就得不到 `this` 对象。这与 ES5 的继承有很大区别。

如果子类没有显示定义 `constructor` 方法，这个方法会被默认添加，代码如下。也就是说，不管有没有显式定义，任何一个子类都有 `constructor` 方法。

```javascript
constructor(...args) {
  super(...args);
}
```

* 静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前加上 `static` 关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这种用法和其他的面向对象语言中的静态方法很类似。而且父类的静态方法可以被子类继承，在子类的定义中，可以通过 `super` 来调用；也可以通过子类直接调用该静态方法。

```javascript
class Foo {
  static f() {
    return 'hello';
  }
}

class Bar extends Foo {
  static b() {
    return super.f() + ', too';
  }
}

Foo.f(); //hello
Bar.f(); //hello
Bar.b(); //hello, too
```

### 6. Iterator

遍历器（Iterator）作为一种机制，为各种不同的数据结构提供统一的访问机制。任何数据结构只要支持Iterator，就可以完成遍历操作。概况地讲，Iterator 有三个作用：一是为各种数据结构，提供一个统一的、简便的访问接口；二是使得数据结构的成员能够按某种次序排列；三是ES6创造了一种新的遍历命令 `for...of` 循环，Iterator接口主要供 `for...of` 消费。

Iterator的遍历过程是这样的:
（1）创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。
（2）第一次调用指针对象的 `next` 方法，可以将指针指向数据结构的第一个成员。
（3）第二次调用指针对象的 `next` 方法，指针就指向数据结构的第二个成员。
（4）不断调用指针对象的 `next` 方法，直到它指向数据结构的结束位置。


### 7. Module

ES6 提出的 `class` 语法糖，在很大程度上方便了面向对象编程，但是并没有解决模块化的问题。追溯 JavaScript 的历史，一直没有添加对模块化的支持，导致在构建大型的复杂的系统时，拆分依赖，按需加载都不是很容易的事情。这也是为什么各大公司、社区争相放出各种规范的原因，比如 AMD、CMD。

ES6模块的设计思想，是尽量的静态化，使得在编译时就能确定模块的依赖关系，以及输入和输出的变量。而CommonJS和AMD模块，都只能在运行时确定这些东西，所以不能很好的实现按需加载。ES6的模块不是对象，而是通过 `export` 语句显式指定输出的代码，输入时也采用静态命令的形式。

```javascript
// ES6模块
import {sin, cos} from 'Math';
```

* export

模块功能主要由两个语句构成： `export` 和 `import` 。 `export` 语句用于规定模块的对外接口， `import` 语句用于输入其他模块提供的功能。

一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用 `export` 语句输出该变量。下面是一个JS文件，里面使用 `export` 语句输出变量。

```javascript
//profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;
```

也可写成：

```javascript
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
```

对于 `function` 和 `class` 写法都类似。通常情况下， `export` 输出的变量就是本来的名字，但是可以使用 `as` 关键字重命名。

```javascript
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion //这里V2可以用不同的名字输出。
};
```

* import

使用 `export` 语句定义了模块的对外接口以后，其他JS文件就可以通过 `import` 语句加载这个模块（文件）。

```javascript
// main.js
import {firstName, lastName, year} from './profile';

function setName(element) {
  element.textContent = firstName + ' ' + lastName;
}
```

引入变量的时候也可以进行重命名，比如：

```javascript
import { lastName as surname } from './profile';
```

* 模块的整体加载

除了像前面的例子指定加载某个输出值，还可以使用整体加载，即用星号（ `*` ）指定一个对象，所有输出值都加载在这个对象上面。

```javascript
// circle.js
export function area(radius) {
  return Math.PI * radius * radius;
}

export function circumference(radius) {
  return 2 * Math.PI * radius;
}

//或者写成
function area(radius) {
  return Math.PI * radius * radius;
}

function circumference(radius) {
  return 2 * Math.PI * radius;
}

export {area, circumference};
```

现在我们需要加载 circle.js 里面所有的函数。一种是在 `import` 的时候列出所有的函数；另一种是使用星号代替所有。

```javascript
// main.js
//方法一
import { area, circumference } from './circle';

console.log('圆面积：' + area(4));
console.log('圆周长：' + circumference(14));

//方法二
import * as circle from './circle';

console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));
```

* export default

从前面的例子中，我们可以看到，想要 `import` 部分函数的时候，类库的使用者必须要知道其中到底包含了哪些函数。这个时候为了给使用者提供方便，我们可以使用 `export default`来为模块指定默认输出参数。

```javascript
// export-default.js
export default function () {
  console.log('foo');
}

// import-default.js
import customName from './export-default';
customName(); // 'foo'
```

上面的 `export` 中声明了一个匿名函数，然后输出为默认值。所以在 `import` 的时候我们可以指定任意名字，因此这时 `import` 语句后面，不使用大括号。当然 `export default` 也适用于非匿名函数。

```javascript
// export-default.js
export default function foo() {
  console.log('foo');
}

// 或者写成
function foo() {
  console.log('foo');
}
export default foo;
```

上面例子中，函数名 `foo` 只在模块内部有效。加载的时候也会被视同为匿名函数。其实本质上，`export default`就是输出一个叫做`default`的变量或函数，然后系统允许你为它取任意名字。

### 8. 其他

针对 ES6中的其他语法特性我就不在这里一一展开了，感兴趣的可以自行了解：`Generator`、`Regular Expression`、`Symbol`、`Promise`、`Set`、`Map`、`Binary Array`、`Proxy`、`Reflect`等。

## ES6 的兼容性
随着时间的推移，各大厂商对 ES6 的支持越来越完善，ES6 的大部分特性都已得到实现。更详细的兼容性列表请移步 [http://kangax.github.io/compat-table/es6/](http://kangax.github.io/compat-table/es6/)。

> 以下数据采集：2016年06月21日22:21:51

Compilers | Desktop browsers | Servers/runtimes | Mobile
:---:|:---:|:---:|:---:
Traceur 58% | Edge 14 90% | Node 6 93% | iOS 9 53%
Babel 74% | Firefox 49 93% ||
| Chrome 51/Opera 38 98% ||

## 参考资料
> 阮博士的书[《ECMAScript 6入门》](http://es6.ruanyifeng.com/)
> [Babeljs.io](http://babeljs.io/docs/learn-es2015/)
> [ECMAScript Support in Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla)
> [http://kangax.github.io/compat-table/es6/](http://kangax.github.io/compat-table/es6/)
