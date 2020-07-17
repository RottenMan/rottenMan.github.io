---

categories:
    - 前端
tags:
    - promise
    - ES6
---

## Promise基本概念

Promise 对象的构造器（constructor）语法如下

```javascript
let promise = new Promise(function(resolve, reject) {
  // executor
});
```



传递给`new Promise`的函数被称为 **executor**，当 `new Promise` 被创建，executor 会自动运行



它的参数 `resolve` 和 `reject` 是由 JavaScript 自身提供的回调。我们的代码仅在 executor 的内部。



当`executor`获得了结果，他会调用一下回调之一

* `resolve(value)` —  代码成功执行并带有结果`value`
* `reject(error)` — 出现错误`error`

总结：executor 会自动运行并尝试执行一项工作。尝试结束后，如果成功则调用 `resolve`，如果出现 error 则调用 `reject`



由`new Promise`构造器返回的`promised`对象具有以下属性

- `state` — 最初是 `"pending"`，然后在 `resolve` 被调用时变为 `"fulfilled"`，或者在 `reject` 被调用时变为 `"rejected"`。
- `result` — 最初是 `undefined`，然后在 `resolve(value)` 被调用时变为 `value`，或者在 `reject(error)` 被调用时变为 `error`。

![](https://s1.ax1x.com/2020/07/09/UewMQg.png)



```javascript
let promise = new Promise(function(resolve, reject) {
  // 当 promise 被构造完成时，自动执行此函数

  // 1 秒后发出工作已经被完成的信号，并带有结果 "done"
  setTimeout(() => resolve("done"), 1000);
});
```

以上代码是一个成功完成的例子，运行上面代码，会发生两件事

1. executor 被自动且立即调用
2. executor 接受两个参数：`resolve` 和 `reject` (这些函数会由 JavaScipt 引擎预先定义，不需要创建他们)，经过 1 秒的“处理”后，executor 调用 `resolve("done")` 来产生结果，改变`promise`对象的状态



```javascript
let promise = new Promise(function(resolve, reject) {
  // 1 秒后发出工作已经被完成的信号，并带有 error
  setTimeout(() => reject(new Error("Stop!")), 1000);
});
```

以上代码是一个以executor 以 error 拒绝 promise 示例，对 `reject(...)` 的调用将 promise 对象的状态移至 `"rejected"`



**只能有一个结果或者一个error**

executor 只能调用一个 `resolve` 或一个 `reject`，所有其他的再对 `resolve` 和 `reject` 的调用都会被忽略：

```javascript
let promise = new Promise(function(resolve, reject) {
  resolve("done");

  reject(new Error("…")); // 被忽略
  setTimeout(() => resolve("…")); // 被忽略
});
```



**resolve, reject 可以立即执行**

executor 通常是异步执行某些操作，并在一段时间后调用 `resolve/reject`，但是也可以立即调用`resolve`,`reject `

```javascript
let promise = new Promise(function(resolve, reject) {
  resolve(123); // 立即给出结果：123
});
```



**`state`和`result`都是内部的**

Promise 对象的 `state` 和 `result` 属性都是内部的，无法直接访问，可以使用 `.then`/`.catch`/`.finally` 方法



## 消费者：then, catch, finally

**then**

最重要最基础的一个就是 `.then`，基本语法⬇

```javascript
promise.then(
  function(result) { /* handle a successful result */ },
  function(error) { /* handle an error */ }
);
```

`.then` 的第一个参数是一个函数，该函数将在 promise resolved 后运行并接收结果。

`.then` 的第二个参数也是一个函数，该函数将在 promise rejected 后运行并接收 error。



```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done!"), 1000);
});

// resolve 运行 .then 中的第一个函数
promise.then(
  result => alert(result), // 1 秒后显示 "done!"
  error => alert(error) // 不运行
);
```

以上代码是对成功的 resolved的 promise的反应



在 reject 的情况下，运行第二个

```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// reject 运行 .then 中的第二个函数
promise.then(
  result => alert(result), // 不运行
  error => alert(error) // 1 秒后显示 "Error: Whoops!"
);
```



如果只想要成功结果，可以只为`.then` 提供一个函数参数：

```javascript
let promise = new Promise(resolve => {
  setTimeout(() => resolve("done!"), 1000);
});

promise.then(alert); // 1 秒后显示 "done!"
```



**catch**

如果只想要error结果，我们可以用`null`作为第一个参数 `.then(null, errorFunction)`，或者使用`.catch(errorFunction)`

```javascript
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("Stop!")), 1000);
});

// .catch(f) 与 promise.then(null, f) 一样
promise.catch(alert); // 1 秒后显示 "Error: Stop!"
```

`.catch(f)` 调用的就是 `.then(null, f)` 模式，它只是一个简写形式。



**finally**

跟`try{...} catch{...}`中的`finally`子句一样，promise 中也有 `finally`。

`.finally(f)` 调用与 `.then(f, f)` 类似，`f` 总是在 promise 被 固定时运行：即 promise 被 resolve 或 reject



``finally``与`then(f,f)`的区别

1. `finally` 处理程序没有参数，因为不知道不知道 promise 是否成功，`finally`的任务是执行规定的程序
2. `finally` 处理程序将结果和 error 传递给下一个处理程序

```javascript
new Promise((resolve, reject) => {
  setTimeout(() => resolve("result"), 2000)
})
  .finally(() => alert("Promise ready"))
  .then(result => alert(result)); // <-- .then 对结果进行处理
```



promise将error从 `finally` 传递给 `catch`

```javascript
ew Promise((resolve, reject) => {
  throw new Error("error");
})
  .finally(() => alert("Promise ready"))
  .catch(err => alert(err));  // <-- .catch 对 error 对象进行处理
```



**在固定的promise上，`then`会立即执行**

如果 promise 为 挂起状态，`.then/catch/finally` 处理程序会等待，如果如果 promise 已经是 固定状态，它们就会立即执行：

```javascript
let promise = new Promise(resolve => resolve("done!"));
promise.then(alert); // done!（现在显示）
```





## Promise链基本结构

```javascript
new Promise(function(resolve, reject) {

  setTimeout(() => resolve(1), 1000); // (*)

}).then(function(result) { // (**)

  alert(result); // 1
  return result * 2;

}).then(function(result) { // (***)

  alert(result); // 2
  return result * 2;

}).then(function(result) {

  alert(result); // 4
  return result * 2;

});
```

思路是将result 通过 `.then` 处理程序链进行传递



执行流程⬇

1. 初始 promise 在 1 秒后进行 resolve `(*)`
2. 然后 `.then` 处理程序（handler）被调用 `(**)`。
3. 它返回的值被传入下一个 `.then` 处理程序（handler）`(***)` 以此类推

![](https://s1.ax1x.com/2020/07/09/UeIZRg.png)



之所以可以这么运行，因为对`promise.then` 的调用会返回了一个 promise，所以我们可以在其之上调用下一个`.then`



当处理程序（handler）返回一个值时，它将成为该 promise 的结果(result)，所以将使用它调用下一个 `.then`。



**一个经典错误：如果把多个.then 添加到一个 promise 上。这并不是一个 promise 链。**

```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve(1), 1000);
});

promise.then(function(result) {
  alert(result); // 1
  return result * 2;
});

promise.then(function(result) {
  alert(result); // 1
  return result * 2;
});

promise.then(function(result) {
  alert(result); // 1
  return result * 2;
});
```



上述代码所做的只是promise 的几个处理程序，他们不会相互传递 result；它们之间彼此独立运行处理任务。



![](https://s1.ax1x.com/2020/07/09/UeIVJS.png)



在同一个promise上所有的 `.then` 获得的结果都相同



## 返回 promise

`.then(handler)` 中所使用的处理程序可以创建并返回一个 promise，其他的处理程序将等待它settled 后再获得其结果

```javascript
new Promise(function(resolve, reject) {

  setTimeout(() => resolve(1), 1000);

}).then(function(result) {

  alert(result); // 1

  return new Promise((resolve, reject) => { // (*)
    setTimeout(() => resolve(result * 2), 1000);
  });

}).then(function(result) { // (**)

  alert(result); // 2

  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(result * 2), 1000);
  });

}).then(function(result) {

  alert(result); // 4

});
```

第一个 `.then` 显示 `1` 并在 返回 `new Promise()`，一秒后进行resolve，然后result被传递给第二个 `.then` 的处理程序，这个处理程序位于 `(**)` 行，它显示 `2`，并执行相同的动作，以此类推。



输出与前面的示例相同：1 → 2 → 4，但是现在在每次 `alert` 调用之间会有 1 秒钟的延迟。



返回 promise 使我们能够构建异步行为链。