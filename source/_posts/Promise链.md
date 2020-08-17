---

categories:
    - 前端
tags:
    - promise
    - ES6
date: 2020-08-14
---


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