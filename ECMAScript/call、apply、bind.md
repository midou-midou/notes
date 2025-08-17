# this指向

## 简介
在函数的执行上下文中确定的，也就是说，是在调用函数的时候确定的

## 普通函数
### 普通调用


### "."调用


### 回调

## 箭头函数
箭头函数的this表现行为和普通函数不一样，箭头函数会在创建箭头函数时，在周围作用域创建一个闭包，闭包可以保证this指向是创建时的值，不会像其他函数在执行时可以更改this

```js
const globalObject = this;
const foo = () => this;
console.log(foo() === globalObject); // this 指向 globalThis

const obj2 = { 
  name: "obj2",
  func: () => {
    console.log(this);
  }
};
obj2.func()；//this 也指向globalThis
```

### 箭头函数使用call、apply
无效果，this指向上文提到不会发生动态改变，调用`call() apply()`都无法改变箭头函数内部this指向


# 改变普通函数词法环境的this指向

js中的函数：普通函数，箭头函数，构造函数，类静态函数

## call、apply
### 简介
可以在函数调用的时候传入一个`context`上下文参数以及`普通参数args`   
是`Function`下面的方法，调用要通过`.`调用

```JavaScript
func.call(context, arg1, arg2, ...)
func.apply(context, args)
```

### 区别
`call`接收多个参数“arg”等作为函数的参数，而`apply`接收一个类数组对象`args`作为参数

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

### 针对不同函数

* 箭头函数：传入的`context`无效，箭头函数中的this还是指向创建环境
* 构造函数：

# bind

返回的是一个函数，需要在调用一次

函数签名：

```js
bind(context, ...args)

// 调用
let fn = fn.bind(context, ...arg)
```







