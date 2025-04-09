# Promise

### 概念

Promise可以用于顺序的执行一些异步的代码。比如说，需要**先获取用户的信息，得到了用户的基本信息才可以去用基本信息中的userId拿到用户的权限信息**。 Promise可以处理一个不知道返回结果的方法。比如，获取学校的信息需要调用接口，但是我们没法保证这个接口每次调用100%正确（有可能会因为网络问题等出现异常）

### 状态state、结果result

这俩均为Promise对象的内部属性，可以访问 一个Promise执行过程中，这两个内部属性的变换

![https://static.xuyanshe.club/img/1675753033811-6522948b-9f63-4bc6-bc6c-248ec5f1c1c9.png](Promise\1675753033811-6522948b-9f63-4bc6-bc6c-248ec5f1c1c9.png)

`resolve()`: `pending -> fulfilled`

`reject()`：`pending -> rejected` 

如果一个Promise执行完了resolve或者reject，那么这个Promise的状态是settled

![https://static.xuyanshe.club/img/1675752301178-c3549495-57c0-406a-bc43-e8f131d26d33.jpeg](Promise\1675752301178-c3549495-57c0-406a-bc43-e8f131d26d33.jpeg)

### 使用

```JavaScript
new Promise(function(resolve, reject) {  
	// executor
	// 这里如果用return返回，返回的东西没有任何作用
});
```

接收一个`resolve`函数和`reject`函数。Promise接收的函数的函数体会自动的执行，执行的结果如果成功就会执行`resolve`失败就会执行`reject`，也可以自己调用`resolve`和`reject`

### resolve、reject执行结果的接收

使用`then()`来接收

```JavaScript
promise.then(
	function(result) { /* handle a successful result */ },  
	function(error)  { /* handle an error */ }
);
```

`then`也可以接收两个参数，第一个是处理`resolve`执行的结果(该函数将在 promise resolved 且接收到结果后执行)，第二个是`reject`执行的结果(该函数将在 promise rejected 且接收到 error 信息后执行) 当然也可以只写一个函数作为参数

```JavaScript
.then((result) => {})
.then(null, errorHandlingFunction)
```

上面代码块仅处理`error`也可以使用`catch`

```JavaScript
let promise = new Promise((resolve, reject) => {  
	setTimeout(() => reject(new Error("Whoops!"))
, 1000);
});
// .catch(f) 与 promise.then(null, f) 一样promise.catch(alert);
```

`catch`可以捕获Promise以及`resolve`产生的错误。且在链式调用下可以捕获前面的的所有错误

最后可以用`finally`来执行收尾工作

```JavaScript
new Promise((resolve, reject) => {  
	/* 做一些需要时间的事，之后调用可能会 resolve 也可能会 reject */
})  
// 在 promise 为 settled 时运行，无论成功与否  
.finally(() => stop loading indicator)
```

`finally`会接收上面执行成功的`value`或者执行失败的`error`

### Promise链

一个Promise的`resolve`和`reject`可以返回，返回的值为一个新的Promise

```JavaScript
loadScript("/article/promise-chaining/one.js")  
	.then(script => loadScript("/article/promise-chaining/two.js"))  
	.then(script => loadScript("/article/promise-chaining/three.js"))  
	.then(script => {    // 脚本加载完成，我们可以在这儿使用脚本中声明的函数    
					one();    
					two();    
					three();  
				});
```

### 回调地狱问题

比如上面代码块中需要**按顺序**以此请求对应的js文件，如果不使用Promise

```JavaScript
loadScript('1.js', function(error, script) {  
	if (error) {    
		handleError(error);  
	} else {    
	// ...    
	loadScript('2.js', function(error, script) {      
	if (error) {        
		handleError(error);      
	} else {        
				// ...        
				loadScript('3.js', function(error, script) {          
					if (error) {            
						handleError(error);          
					} else {            
							// ...加载完所有脚本后继续 (*)          
					}        
				});      
			}    
	});  
}});
```

没有发生错误的情况下，又会执行`loadScript`，之后在其中又判断是否有错误，没错误继续load…，上面的代码就是“回调地狱”

### Promise.all

概念：并行执行多个Promise，并且等待所有的Promise执行完毕

```JavaScript
Promise.all([  
	new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1  
	new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2  
	new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
])
.then(alert); 
// 1,2,3 当上面这些 promise 准备好时：每个 promise 都贡献了数组中的一个元素
```

`all()`接收一个数组，里面是`Promise`对象，返回一个`res`数组，并且会按照Promise数组的顺序组装`res`结果数组 如果执行的任意一个`Promise`被`reject`，则整个`Promise.all()`会立即的`reject`并且返回`error`

### Promise.race

和上面的`.all`API都可以同时执行多个`Promise`，但是`.race`仅等待第一个状态为`settled`的`Promise`（`settled`就是`Promise`要么`resolve`要么`reject`）

### Promise.any

和`.any`也是可以同时执行多个，但`.any`等待第一个`fulfilled`状态的`Promise`，并返回这个`Promise`（`fulfilled`就是`Promise`对象`resolve`了，而不是`reject`）

### Promise.allSettled

和`.all()`都是接收一个`Promise`对象数组，但是不会像`all()`碰到`reject`状态的`Promise`就返回，而是等待数组的所有`Promise`执行完毕（如果是`resolve`，返回的`{status: 'fulfilled', value: ''}`对象，如果是`reject`，返回的`{status: 'rejected', reason: ''}`对象）



