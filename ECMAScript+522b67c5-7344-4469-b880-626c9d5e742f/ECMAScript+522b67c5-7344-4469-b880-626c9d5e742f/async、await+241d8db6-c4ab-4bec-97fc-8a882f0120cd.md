# async、await

### async

### 概念：

在函数前的一个关键字，一个标识。有此关键字的函数返回的值会自动的用一个`resolved`的`Promise`包装`Promise.resolve('xxx')`

### await

### 概念：

等待Promise的状态变为settle（也就是执行了这个Promise的`resolve`或者执行了这个Promise的`reject`）之后接收执行的结果

### 注意：

`async`和`await`必须成对的出现，且`async`包含`await`。在`Module`模块化的js文件中，`await`可以顶层写

```JavaScript
async function f() {
  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  });
  let result = await promise; // 等待，直到 promise resolve (*)
  alert(result); // "done!"
}
```

比如下面例子

```JavaScript
// 我们假设此代码在 module 中的顶层运行
let response = await fetch('/article/promise-chaining/user.json');
let user = await response.json();
```

其他模块通过`import`导入这个模块

### 错误处理

- `try...catch`

- `async`函数后直接`.catch()`调用捕获错误

- 全局错误处理函数

不处理会打印在控制台，且终止函数的运行，毕竟抛出了错误没有管

PS. 如果在Promise中发生了异常，会执行`Promise.reject(err)`，表现在`async/await`中就是`await`接收一个前面的`rejected`，如果没处理错误就会停止程序的执行

```JavaScript
async function f() {
  let response = await fetch('http://no-such-url');
}
// f() 变成了一个 rejected 的 promise
f().catch(alert); // TypeError: failed to fetch // (*)
```



