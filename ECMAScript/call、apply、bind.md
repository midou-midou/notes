## call、apply

这俩都是跟函数相关的

可以在函数调用的时候传入一个`context`上下文参数以及`普通参数args` 是`Function`下面的方法，调用要通过`.`调用

```JavaScript
func.call(context, arg1, arg2, ...)
func.apply(context, args)
```



#### 区别在于：`call`接收多个参数“arg1”等作为函数的参数，而`apply`接收一个类数组对象`args`作为参数

**两者在函数式编程中的使用**

```JavaScript
// worker对象，其中包含一个slow方法，也是slow方法的作用域
let worker = {
  someMethod() {
    return 1;
  },

  slow(x) {
    alert("Called with " + x);
    return x * this.someMethod(); // (*)
  }
};
function cachingDecorator(func) {
  let cache = new Map();
  return function(x) {
    if (cache.has(x)) {
      return cache.get(x);
    }
    let result = func.apply(this, x); // 现在 "this" 被正确地传递了
    // 如果是多个参数就用.call()
    cache.set(x, result);
    return result;
  };
}

worker.slow = cachingDecorator(worker.slow); // 现在对其进行缓存

alert( worker.slow(2) ); // 工作正常
alert( worker.slow(2) ); // 工作正常，没有调用原始函数（使用的缓存）
```

## bind

返回的是一个函数，需要在调用一次

函数签名：

`bind(context, ...args)`



