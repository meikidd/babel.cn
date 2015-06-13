---
layout: docs
title: Learn ES6
description: A detailed overview of ECMAScript 6 features.
permalink: /docs/learn-es6/
redirect_from:
 - /features.html
 - /docs/tour/
---

<blockquote class="babel-callout babel-callout-info">
  <h4>es6features</h4>
  <p>
    这篇文档来自 Luke Hoban 的
    <a href="http://git.io/es6features">es6features</a> ，这是个很棒的开源库。
    去 Github 给他加个 Star 吧！
  </p>
</blockquote>

<blockquote class="babel-callout babel-callout-info">
  <h4>REPL</h4>
  <p>
    在线试用这些新特性：
    <a href="/repl">REPL</a>.
  </p>
</blockquote>

## 简介

> ECMAScript 6 是即将到来的下一个 ECMAScript 标准版本。这个标准计划在 2015 年 6 月批准通过。ES6 对 JavaScript 语言有非常重大的更新，也是 ES5 标准通过以来第一次更新。JavaScript 引擎也正在陆续对这些新特性[提供支持](http://kangax.github.io/es5-compat-table/es6/)。

完整的 ECMAScript 6 规范草案在[这里](https://people.mozilla.org/~jorendorff/es6-draft.html)

## ECMAScript 6 新特性

### 箭头函数

箭头函数是函数的简写方式，使用 `=>` 语法。在语法上看起来很像 C#, Java 8, 和 CoffeeScript 里的相关特性。函数体内可以包含表达式和声明。 跟普通函数不同的是，箭头函数跟它所在的上下文有相同的 `this` 作用域。

```js
// Expression bodies
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);

// Statement bodies
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// Lexical this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

### 类 (Classes)

ES6 的类是一个语法糖，同样还是基于 prototype 的面向对象思想实现的。 由于它的声明方式非常方便，所以用起来非常简单，也更有利于代码的一致性。它支持基于 prototype 的继承，访问父类，实例，静态方法和构造函数。

```js
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

### 增强的对象字面量 (Enhanced Object Literals)

对象字面量在原来的基础上得到了扩展：设置 prototype 构造器，对 `foo: foo` 赋值语句的简写，直接定义函数并通过 super 访问父类。这些特性使得对象字面量和类的声明越来越像了，也使得基于对象的设计越来越方便。

```js
var obj = {
    // __proto__
    __proto__: theProtoObj,
    // Shorthand for ‘handler: handler’
    handler,
    // Methods
    toString() {
     // Super calls
     return "d " + super.toString();
    },
    // Computed (dynamic) property names
    [ "prop_" + (() => 42)() ]: 42
};
```

<blockquote class="babel-callout babel-callout-warning">
  <p>
    <code>__proto__</code> 已经得到 JavaScript 引擎的支持。虽然大部分都已经基于现有标准提供支持, 但还是有
    <a href="http://kangax.github.io/compat-table/es6/#__proto___in_object_literals">一些</a> 并不支持。
  </p>
</blockquote>

### 模板字符串

模板字符串是对针对结构化字符串的一个语法糖，类似 Perl, Python 的插值字符串的特性。可以定义好字符串的结构，在模板字符串里添加一个标记，不必打断字符串就能将数值插入进去。还能根据结构化的数据生成字符串内容。

```js
// Basic literal string creation
`In ES5 "\n" is a line-feed.`

// Multiline strings
`In ES5 this is
 not legal.`

// Interpolate variable bindings
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// Construct an HTTP request prefix is used to interpret the replacements and construction
GET`http://foo.org/bar?a=${a}&b=${b}
    Content-Type: application/json
    X-Credentials: ${credentials}
    { "foo": ${foo},
      "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

### 解构

解构可以绑定模式匹配，支持匹配数组（arrays）和对象（objects）。解构是软失效（fail-soft）的，类似标准的对象查找 `foo["bar"]`，当值不存在时，产生 `undefined`。

```js
// list matching
var [a, , b] = [1,2,3];

// object matching
var { op: a, lhs: { op: b }, rhs: c }
       = getASTNode()

// object matching shorthand
// binds `op`, `lhs` and `rhs` in scope
var {op, lhs, rhs} = getASTNode()

// Can be used in parameter position
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// Fail-soft destructuring
var [a] = [];
a === undefined;

// Fail-soft destructuring with defaults
var [a = 1] = [];
a === 1;
```

### Default + Rest + Spread

default 设置默认参数值：

```js
function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}
f(3) == 15
```

rest 将紧随的连续参数转化为数组：

```js
function f(x, ...y) {
  // y is an Array
  return x * y.length;
}
f(3, "hello", true) == 6
```

spread 将一个数组扩展为连续的参数：

```js
function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f(...[1,2,3]) == 6
```

### Let + Const

块级作用域 `let` 可以认为是一种新的 `var`， `const` 是一次性赋值，是一种静态约束。

```js
function f() {
  {
    let x;
    {
      // okay, block scoped name
      const x = "sneaky";
      // error, const
      x = "foo";
    }
    // error, already declared in block
    let x = "inner";
  }
}
```

### Iterators + For..Of

迭代器对象允许自定义迭代方法，就像 CLR 的 IEnumerable 或 Java 的 Iterable 那样。从 `for..in` 转而使用基于迭代器的 `for..of` 方法。不用去自己实现一个数组，支持类似 LINQ 的懒设计模式。

```js
let fibonacci = {
  [Symbol.iterator]() {
    let pre = 0, cur = 1;
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        return { done: false, value: cur }
      }
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
```


下面的迭代器基于 duck-typed 接口（使用 [TypeScript](http://typescriptlang.org) 风格语法)：

```ts
interface IteratorResult {
  done: boolean;
  value: any;
}
interface Iterator {
  next(): IteratorResult;
}
interface Iterable {
  [Symbol.iterator](): Iterator
}
```



<blockquote class="babel-callout babel-callout-info">
  <h4>通过 polyfill 使用</h4>
  <p>
    必须通过 Babel <a href="/docs/usage/polyfill">polyfill</a> 来使用 Iterators 。
  </p>
</blockquote>


### Generators

Generators 通过 `function*` 和 `yield` 来生成一个 iterator，即通过 function* 声明的函数将返回一个 Generator 实例。Generators 包含 `next` 和 `throw` 两个方法，可以获取每一个 `yield` 表达式右边的值。

提示：也可以用 Generators 写出类似 'await' 风格的异步程序，参考 ES7 `await` [proposal](https://github.com/lukehoban/ecmascript-asyncawait)。

```js
var fibonacci = {
  [Symbol.iterator]: function*() {
    var pre = 0, cur = 1;
    for (;;) {
      var temp = pre;
      pre = cur;
      cur += temp;
      yield cur;
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
```

下面是 generator 接口 (使用 [TypeScript](http://typescriptlang.org) 风格语法):

```ts
interface Generator extends Iterator {
    next(value?: any): IteratorResult;
    throw(exception: any);
}
```

<blockquote class="babel-callout babel-callout-info">
  <h4>通过 polyfill 使用</h4>
  <p>
    必须通过 Babel  <a href="/docs/usage/polyfill">polyfill</a>. 来使用 Generator。
  </p>
</blockquote>

### 解析 Comprehensions

数组和 generator 的解析给我们提供了一种简单的方式来声明列表，就像许多其他函数式编程语言中那样。

```js
// Array comprehensions
var results = [
  for(c of customers)
    if (c.city == "Seattle")
      { name: c.name, age: c.age }
]

// Generator comprehensions
var results = (
  for(c of customers)
    if (c.city == "Seattle")
      { name: c.name, age: c.age }
)
```

<blockquote class="babel-callout babel-callout-warning">
  <h4>默认是不可用的</h4>
  <p>
    必须开启实验特性才能使用解析，查看更多 <a href="/docs/usage/experimental">实验特性</a>。
  </p>
</blockquote>

### Unicode

向下兼容地实现了对 Unicode 全字符集的支持，包括新的 unicode 字面量形式和新的正则表达式 `u` 模式。这些新特性使得 JavaScript 可以更好地创建全球化应用。

Non-breaking additions to support full Unicode, including new unicode literal
form in strings and new RegExp `u` mode to handle code points, as well as new
APIs to process strings at the 21bit code points level.  These additions support
building global apps in JavaScript.

```js
// same as ES5.1
"𠮷".length == 2

// new RegExp behaviour, opt-in ‘u’
"𠮷".match(/./u)[0].length == 2

// new form
"\u{20BB7}" == "𠮷" == "\uD842\uDFB7"

// new String ops
"𠮷".codePointAt(0) == 0x20BB7

// for-of iterates code points
for(var c of "𠮷") {
  console.log(c);
}
```

### 模块 Modules

从编程语言层面支持模块声明，类似当前流行的 JavaScript 模块加载机制 (AMD, CommonJS). 运行时的行为由一个默认的加载器决定，特别是异步模块，只有当模块加载完之后才能执行。

```js
// lib/math.js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```
```js
// app.js
import * as math from "lib/math";
alert("2π = " + math.sum(math.pi, math.pi));
```
```js
// otherApp.js
import {sum, pi} from "lib/math";
alert("2π = " + sum(pi, pi));
```

一些附加特性，例如 `export default` 和 `export *`：

```js
// lib/mathplusplus.js
export * from "lib/math";
export var e = 2.71828182846;
export default function(x) {
    return Math.exp(x);
}
```
```js
// app.js
import exp, {pi, e} from "lib/mathplusplus";
alert("2π = " + exp(pi, e));
```

<blockquote class="babel-callout babel-callout-info">
  <h4>模块格式</h4>
  <p>
    Babel 可以将 ES6 模块转换为各种不同的
    Babel can transpile ES6 Modules to several different formats including
    Common.js, AMD, System, and UMD. You can even create your own. For more
    details see the <a href="/docs/usage/modules">modules docs</a>.
  </p>
</blockquote>

### Module Loaders

Module loaders support:

- Dynamic loading
- State isolation
- Global namespace isolation
- Compilation hooks
- Nested virtualization

The default module loader can be configured, and new loaders can be constructed
to evaluated and load code in isolated or constrained contexts.

```js
// Dynamic loading – ‘System’ is default loader
System.import("lib/math").then(function(m) {
  alert("2π = " + m.sum(m.pi, m.pi));
});

// Create execution sandboxes – new Loaders
var loader = new Loader({
  global: fixup(window) // replace ‘console.log’
});
loader.eval("console.log(\"hello world!\");");

// Directly manipulate module cache
System.get("jquery");
System.set("jquery", Module({$: $})); // WARNING: not yet finalized
```

<blockquote class="babel-callout babel-callout-warning">
  <h4>Additional polyfill needed</h4>
  <p>
    Since Babel defaults to using common.js modules, it does not include the
    polyfill for the module loader API. Get it
    <a href="https://github.com/ModuleLoader/es6-module-loader">here</a>.
  </p>
</blockquote>

<blockquote class="babel-callout babel-callout-info">
  <h4>Using Module Loader</h4>
  <p>
    In order to use this, you'll need to tell Babel to use the
    <code>system</code> module formatter. Also be sure to check out
    <a href="https://github.com/systemjs/systemjs">System.js</a>
  </p>
</blockquote>


### Map + Set + WeakMap + WeakSet

Efficient data structures for common algorithms.  WeakMaps provides leak-free
object-key’d side tables.

```js
// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
// Because the added object has no other references, it will not be held in the set
```

<blockquote class="babel-callout babel-callout-info">
  <h4>Support via polyfill</h4>
  <p>
    In order to support Maps, Sets, WeakMaps, and WeakSets in all environments you must include the Babel <a href="/docs/usage/polyfill">polyfill</a>.
  </p>
</blockquote>

### Proxies

Proxies enable creation of objects with the full range of behaviors available to
host objects.  Can be used for interception, object virtualization,
logging/profiling, etc.

```js
// Proxying a normal object
var target = {};
var handler = {
  get: function (receiver, name) {
    return `Hello, ${name}!`;
  }
};

var p = new Proxy(target, handler);
p.world === "Hello, world!";
```

```js
// Proxying a function object
var target = function () { return "I am the target"; };
var handler = {
  apply: function (receiver, ...args) {
    return "I am the proxy";
  }
};

var p = new Proxy(target, handler);
p() === "I am the proxy";
```

There are traps available for all of the runtime-level meta-operations:

```js
var handler =
{
  get:...,
  set:...,
  has:...,
  deleteProperty:...,
  apply:...,
  construct:...,
  getOwnPropertyDescriptor:...,
  defineProperty:...,
  getPrototypeOf:...,
  setPrototypeOf:...,
  enumerate:...,
  ownKeys:...,
  preventExtensions:...,
  isExtensible:...
}
```

<blockquote class="babel-callout babel-callout-danger">
  <h4>Unsupported feature</h4>
  <p>
    Due to the limitations of ES5, Proxies cannot be transpiled or polyfilled.
    See support from various
    <a href="http://kangax.github.io/compat-table/es6/#Proxy">JavaScript
    engines</a>.
  </p>
</blockquote>

### Symbols

Symbols enable access control for object state. Symbols allow properties to be
keyed by either `string` (as in ES5) or `symbol`. Symbols are a new primitive
type. Optional `name` parameter used in debugging - but is not part of identity.
Symbols are unique (like gensym), but not private since they are exposed via
reflection features like `Object.getOwnPropertySymbols`.

```js
(function() {

  // module scoped symbol
  var key = Symbol("key");

  function MyClass(privateData) {
    this[key] = privateData;
  }

  MyClass.prototype = {
    doStuff: function() {
      ... this[key] ...
    }
  };

})();

var c = new MyClass("hello")
c["key"] === undefined
```

<blockquote class="babel-callout babel-callout-info">
  <h4>Support via polyfill</h4>
  <p>
    In order to support Symbols you must include the Babel <a href="/docs/usage/polyfill">polyfill</a>.
  </p>
</blockquote>

### Subclassable Built-ins

In ES6, built-ins like `Array`, `Date` and DOM `Element`s can be subclassed.

```js
// User code of Array subclass
class MyArray extends Array {
    constructor(...args) { super(...args); }
}

var arr = new MyArray();
arr[1] = 12;
arr.length == 2
```

<blockquote class="babel-callout babel-callout-warning">
  <h4>Partial support</h4>
  <p>
    Built-in subclassability should be evaluated on a case-by-case basis as classes such as <code>HTMLElement</code> <strong>can</strong> be subclassed while many such as <code>Date</code>, <code>Array</code> and <code>Error</code> <strong>cannot</strong> be due to ES5 engine limitations.
  </p>
</blockquote>

### Math + Number + String + Object APIs

Many new library additions, including core Math libraries, Array conversion
helpers, and Object.assign for copying.

```js
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll("*")) // Returns a real Array
Array.of(1, 2, 3) // Similar to new Array(...), but without special one-arg behavior
[0, 0, 0].fill(7, 1) // [0,7,7]
[1,2,3].findIndex(x => x == 2) // 1
["a", "b", "c"].entries() // iterator [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) })
```

<blockquote class="babel-callout babel-callout-warning">
  <h4>Limited support from polyfill</h4>
  <p>
    Most of these APIs are supported by the Babel <a href="/docs/usage/polyfill">polyfill</a>. However, certain
    features are omitted for various reasons (e.g.
    <code>String.prototype.normalize</code> needs a lot of additional code to
    support). You can find more polyfills
    <a href="https://github.com/addyosmani/es6-tools#polyfills">here</a>.
  </p>
</blockquote>

### Binary and Octal Literals
Two new numeric literal forms are added for binary (`b`) and octal (`o`).

```js
0b111110111 === 503 // true
0o767 === 503 // true
```

<blockquote class="babel-callout babel-callout-warning">
  <h4>Only supports literal form</h4>
  <p>
    Babel is only able to transform <code>0o767</code> and not
    <code>Number("0o767")</code>.
  </p>
</blockquote>


### Promises

Promises are a library for asynchronous programming. Promises are a first class
representation of a value that may be made available in the future. Promises are
used in many existing JavaScript libraries.

```js
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```

<blockquote class="babel-callout babel-callout-info">
  <h4>Support via polyfill</h4>
  <p>
    In order to support Promises you must include the Babel <a href="/docs/usage/polyfill">polyfill</a>.
  </p>
</blockquote>

### Reflect API

Full reflection API exposing the runtime-level meta-operations on objects. This
is effectively the inverse of the Proxy API, and allows making calls
corresponding to the same meta-operations as the proxy traps. Especially useful
for implementing proxies.

```js
var O = {a: 1};
Object.defineProperty(O, 'b', {value: 2});
O[Symbol('c')] = 3;

Reflect.ownKeys(O); // ['a', 'b', Symbol(c)]

function C(a, b){
  this.c = a + b;
}
var instance = Reflect.construct(C, [20, 22]);
instance.c; // 42
```

<blockquote class="babel-callout babel-callout-info">
  <h4>Support via polyfill</h4>
  <p>
    In order to use the Reflect API you must include the Babel <a href="/docs/usage/polyfill">polyfill</a>.
  </p>
</blockquote>

### Tail Calls

Calls in tail-position are guaranteed to not grow the stack unboundedly. Makes
recursive algorithms safe in the face of unbounded inputs.

```js
function factorial(n, acc = 1) {
    "use strict";
    if (n <= 1) return acc;
    return factorial(n - 1, n * acc);
}

// Stack overflow in most implementations today,
// but safe on arbitrary inputs in eS6
factorial(100000)
```

<blockquote class="babel-callout babel-callout-warning">
  <h4>Partial support</h4>
  <p>
    Only explicit self referencing tail recursion is supported due to the
    complexity and performance impact of supporting tail calls globally.
  </p>
</blockquote>
