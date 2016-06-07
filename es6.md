# javascript 模块化命名规则 -- 刘卿，2015-11-25


## 基本设置

* 4 空格缩进 (tab要转为空格)
* UTF-8 编码

## 声明

使用es6的写法，使用babel来编译

## 命名
*避免单字母命名。命名应具备描述性。*

使用驼峰式命名对象、函数和实例。

```
// bad
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// good
const thisIsMyObject = {};
function thisIsMyFunction() {}
```

使用帕斯卡式命名构造函数或类。

```
// bad
function user(options) {
  this.name = options.name;
}

const bad = new user({
  name: 'nope',
});

// good
class User {
  constructor(options) {
    this.name = options.name;
  }
}

const good = new User({
  name: 'yup',
})
```

使用下划线 _ 开头命名私有属性。

```
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';

// good
this._firstName = 'Panda';
```

如果你的文件只输出一个类，那你的文件名必须和类名完全保持一致。
```
// file contents
class CheckBox {
  // ...
}
export default CheckBox;

// bad
import CheckBox from './checkBox';

// bad
import CheckBox from './check_box';

// good
import CheckBox from './CheckBox';
```



## 运算符

优先使用 `===` 和 `!==` 而不是 `==` 和 `!=`。

## 保留字

避免使用保留字

## 变量
对所有的引用使用 `const` ；避免使用 `var`。

```
// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;
```

如果你一定需要可变动的引用，使用 `let` 代替 `var`。

```
// bad
var count = 1;
if (true) {
  count += 1;
}

// good, use the let.
let count = 1;
if (true) {
  count += 1;
}
```

注意 `let` 和 `const` 都是块级作用域。


一个`const/let`只声明一个变量
```
// bad
const a = b = c = 1;

// good
const a = 1;
const b = 1;
const c = 1;
```


## 对象

使用字面值创建对象。

```
// bad
const obj = new Object();

// good
const obj = {};
```

使用对象方法的简写。

```
// bad
const atom = {
  value: 1,

  addValue: function (value) {
    return atom.value + value;
  },
};

// good
const atom = {
  value: 1,

  addValue(value) {
    return atom.value + value;
  },
};
```

使用对象属性值的简写。

```
const item = 'abc';

// bad
const obj = {
  item: item
}

// good
const obj = {
  item
}
```

## 数组

使用字面值创建数组。

```
// bad
const items = new Array();

// good
const items = [];
```

向数组添加元素时使用 Arrary#push 替代直接赋值。

```
const someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

使用拓展运算符 ... 复制数组。
```
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```

使用数组解构

```
const arr = [1,2,3];

// bad
const first = arr[0];
const two = arr[1];

// good
const [first, two] = arr;
```

## String

字符串使用单引号 ''

```
// bad
const username = "tczc";

// good
const username = 'tczc';
```

模板字符串代替字符串连接

```
const world = 'world';
// bad
const hellowWorld = 'hellow ' + world;

// good
const hellowWorld = 'hellow ${world}';
```
## 函数

使用函数声明代替函数表达式。

> 函数声明是可命名的，所以他们在调用栈中更容易被识别。此外，函数声明会把整个函数提升（hoisted），而函数表达式只会把函数的引用变量名提升。这条规则使得箭头函数可以取代函数表达式。

```
// bad
const foo = function () {};

// good
function foo() {}
```

不要使用 arguments。可以选择 rest 语法 ... 替代。

> 使用 ... 能明确你要传入的参数。另外 rest 参数是一个真正的数组，而 arguments 是一个类数组。

```
// bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// good
function concatenateAll(...args) {
  return args.join('');
}
```

直接给函数的参数指定默认值，不要使用一个变化的函数参数。

```
//bad
function fun(opts) {
  opts = opts || {};
}

// good
function fun(opts = {}) {}
```

## 箭头函数

当你必须使用函数表达式（或传递一个匿名函数）时，使用箭头函数符号。

> 箭头函数创造了新的一个 this 执行环境，通常情况下都能满足你的需求，而且这样的写法更为简洁。

```
// bad
[1, 2, 3].map(function (x) {
  return x * x;
});

// good
[1, 2, 3].map((x) => {
  return x * x;
});
```

*但是要注意不要滥用箭头函数，因为他会改变函数内的this指向。*

## 构造器

总是使用 class。避免直接操作 prototype 。

```
// bad
function Queue(contents = []) {
  this._queue = [...contents];
}
Queue.prototype.pop = function() {
  const value = this._queue[0];
  this._queue.splice(0, 1);
  return value;
}


// good
class Queue {
  constructor(contents = []) {
    this._queue = [...contents];
  }
  pop() {
    const value = this._queue[0];
    this._queue.splice(0, 1);
    return value;
  }
}
```

使用 extends 继承。

```
// bad
const inherits = require('inherits');
function PeekableQueue(contents) {
  Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function() {
  return this._queue[0];
}

// good
class PeekableQueue extends Queue {
  peek() {
    return this._queue[0];
  }
}
```

方法可以返回 this 来帮助链式调用。

```
// bad
Jedi.prototype.jump = function() {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function(height) {
  this.height = height;
};

const luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// good
class Jedi {
  jump() {
    this.jumping = true;
    return this;
  }

  setHeight(height) {
    this.height = height;
    return this;
  }
}

const luke = new Jedi();

luke.jump()
  .setHeight(20);
```

## 模块

使用 `import/export` 而不是非标准的模块系统。

```
// bad
const a = require('a.js');
module.exports = a;

//good
import a from 'a.js';
export default a;
```


