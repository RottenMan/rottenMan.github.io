---

categories:
    - 前端
tags:
    - ES6
date: 2020-08-15
---


## 1. let 命令

### 1.1 基本用法

ES6 新增的`let`命令，用来声明遍历，类似于`var`，但是声明的变量只在`let`命令所在的代码块内有效

`var`和`let`的区别是：`let`有块级作用域,`var`没有块级作用域

```javascript
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```

用`let`声明了变量a，用`var`声明了变量b，然后在外部调用这两个变量，`let`声明的变量报错,`var`声明的变量返回了正确的值



`for`循环的计数器就很适合`let`声明方式

```javascript
for(let i = 0;i < 10;i++){

}
console.log(i) //i is not defined
```

`i`是在`for`循环内部有效，在循环体外部引用报错



```javascript
for(var i = 0;i < 10;i++){

}
console.log(i) //10
```

如果使用`var`，最后输出的是`10`



`for`循环的特别之处：设置循环变量的那部分的作用域和循环体内部的作用域是两个不同的作用域。

```javascript
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```



### 1.2 不存在变量提升

`var`命令会发生“变量提升”现象，即变量可以在声明之前使用

```javascript
console.log(foo); // 输出undefined
var foo = 2;
```



`let`命令改变了语法行为，`let`声明的变量只能是在声明后使用，否则报错。

```javascript
console.log(foo); // 报错ReferenceError
let foo = 2;
```



### 1.3 暂时性死区

只要块级作用域内存在`let`命令，它所声明的变量就会 "绑定" 这个区域，不再受外部的影响。

```javascript
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

上述代码，全局环境中存在一个变量`tmp`，但是在块级作用域内`let`又声明了一个局部变量`tmp`，会导致后者绑定这个块级作用域，所以在`let`声明变量前，对`tmp`赋值会报错。



在代码块内，使用`let`命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”

```javascript
if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
```

上述代码，在`let`声明变量`tmp`之前，都属于`tmp`的死区



暂时性死区会导致`typeof`不再是一个完全安全的操作

```javascript
typeof x; // ReferenceError
let x;
```



如果一个变量从未声明过，那么`typeof`不会报错

```javascript
typeof variable // undefined
```

上述代码，`variable`是一个不存在的变量名，结果返回`undefined`



一些隐蔽的死区，不容易被发现

```javascript
function bar(x = y, y = 2) {
  return [x, y];
}

bar(); // 报错
```

上述代码，调用函数`bar` 之所以报错，因为参数`x`默认值为`y`，但是`y`此时还没有声明，属于死区状态，如果`y`的默认值是`x`，就不会报错，因为`x`已经声明过了

```javascript
function bar(x = 2, y = x) {
  return [x, y];
}
bar(); // [2, 2]
```



### 1.4 不允许重复声明

`let`不允许在相同作用域内，重复声明同一个变量。

```javascript
// 报错
function func() {
  let a = 10;
  var a = 1;
}

// 报错
function func() {
  let a = 10;
  let a = 1;
}
```

因此，不能在函数内部重新声明参数。

```javascript
function func(arg) {
  let arg;
}
func() // 报错

function func(arg) {
  {
    let arg;
  }
}
func() // 不报错
```



## 2. 块级作用域

### 2.1 为什么需要会计作用域

在ES5中只有全局作用域和函数作用域，没有块级作用域，可能会带来一些意料之外的场景



第一中，内层变量可能会覆盖外层变量

```javascript
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
```

上述代码，`if`代码的外部使用的是外层`tmp`变量，内部使用的是内层的`tmp`变量，但是由于变量提升的原因，导致了内层的`tmp`覆盖了外层的`tmp`变量



第二中，用来计数的循环变量成为了全局变量

```javascript
for(var i = 0;i < 10;i++){

}
console.log(i) //10
```

上述代码，变量`i`是用来控制循环，但是循环结束后，`i`并没有消失，成为了全局变量



### 2.2 ES6的块级作用域

`let`为 JavaScript 新增了块级作用域。

```javascript
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```

上述代码，有两个作用域块，`f1()`和`if`，都声明了变量`n`，运行结果输出为5，这表示外层的代码块不会受到内层代码块的影响，如果使用`va`r声明，最后结果才是10



ES6 允许块级作用域的任意嵌套。

```javascript
{{{{
  {let insane = 'Hello World'}
  console.log(insane); // 报错
}}}};
```

上述代码使用了一个五层的块级作用域，每一层都是一个单独的作用域，第四层作用域无法访问第五层的作用域



内层作用域可以定义外层作用域的同名变量

```javascript
{{{{
  let insane = 'Hello World';
  {let insane = 'Hello World'}
}}}};
```



## 3. const命令

### 3.1 基本用法

`const`声明一个只读的常量，常量的值不能被改变

```javascript
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.
```

修改常量的值会报错



```javascript
const PI
// SyntaxError: Missing initializer in const declaration
```

如果只声明，不赋值也会报错



`const`的作用域与`let`命令相同：只在声明所在的块级作用域内有效

```javascript
if (true) {
  const MAX = 5;
}

MAX // Uncaught ReferenceError: MAX is not defined
```



`const`命令也没有变量提升，也存在暂时性死区

```javascript
if (true) {
  console.log(MAX); // ReferenceError
  const MAX = 5;
}
```



`const`声明的常量，也与`let`一样不可重复声明。

```javascript
var message = "Hello!";
let age = 25;

// 报错
const message = "Goodbye!";
const age = 30;
```



### 3.2 本质

`const`的本质，并不是保证变量的值不会被改动，而是保证变量的指向的内存地址所保存的数据不会被改动，但是对于复合类型，比如对象和数组，变量指向的是内存地址，保存的是一个指向数据的指针，`const`只能保证指向的变量的指针不会改变，但是指针指向的数据结构，`const`不能控制

```javascript
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```



数组例子

```javascript
const a = [];
a.push('Hello'); // 可执行
a.length = 0;    // 可执行
a = ['Dave'];    // 报错
```



如果让数据结构不被改变，可以将对象冻结 使用`Object.freeze`方法

```javascript
const foo = Object.freeze({});

// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
```



## 4. 顶层对象的属性

顶层对象在浏览器当中指的是`window`对象，在 Node 指的是`global`对象

ES5中，顶层对象和全局变量是相等的

```javascript
window.a = 1;
a // 1

a = 2;
window.a // 2
```



在ES6中，用`let`命令、`const`命令、`class`命令声明的全局变量，不属于顶层对象的属性

```javascript
var a = 1;
window.a // 1

let b = 1;
window.b // undefined
```

全局变量`a`由`var`命令声明，所以它是顶层对象的属性；全局变量`b`由`let`命令声明，所以它不是顶层对象的属性，返回`undefined`。

