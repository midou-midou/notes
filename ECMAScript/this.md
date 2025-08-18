# this指向

## 简介
在函数的执行上下文中确定的，也就是说，是在调用函数的时候确定的

## 普通函数
### 普通调用
调用的时候决定函数内部的this指向，跟周围上下文环境相关
```js
const foo = function () {
  console.log(this);
}

foo() // esm 为 undefined, cjs 为 globalThis, 浏览器 为 window
```

### "."调用
字面量对象，类实例的"."调用普通函数，普通函数this指向前面的字面量对象，或类实例
```js
const fooFunc = {
  bar : function() {
    console.log(this);
  }
}

class fooCls{
  bar = function() {
    console.log(this);
  }
}

fooFunc.bar() // { bar: [Function: bar] }
let f = new fooCls
f.bar() // fooCls { bar: [Function: bar] }
```

箭头函数就不一样了，在对象字面量上调用会undefined（当然这里默认是严格模式下，esm模块默认行为），但类定义下确正常

```js
// 箭头函数会在创建函数时确定封闭词法环境，发现外部词法环境是类，做了一个bind绑定
class fooCls{
  barStr = 'bar str'
  constructor() {
    this.bar = this.bar.bind(this)
  }
  
  bar = function () {
    console.log(this.barStr);
  }
}
```

### Number、String、BigInt、Boolen、Symbol值调用
怎么调用，在这些原始值都有对应的构造函数，在构造函数的原型上添加方法就可以
```js
function getThis() {console.log(this)};

Symbol.prototype.getThis = getThis;
Symbol('foo').getThis();

Number.prototype.getThis = getThis;
(1).getThis();

Boolean.prototype.getThis = getThis;
(true).getThis();

BigInt.prototype.getThis = getThis;
(123456789012345678901234567890n).getThis()

String.prototype.getThis = getThis;
('foo').getThis()

// 结果
// 非严格
// [Symbol: Symbol(foo)]
// [Number: 1]
// [Boolean: true]
// [BigInt: 123456789012345678901234567890n]
// [String: 'foo']

// 严格
// Symbol(foo)
// 1
// true
// 123456789012345678901234567890n
// foo
```
原始值会在非严格模式下，this指向一个包装对象。严格模式下指向自身的值

## 回调
回调的this指向决定"在于什么时候调用回调"函数

但在`setTimeout`和`setInterval`中，普通函数做回调和箭头函数做回调有不同的表现
```js
setTimeout(function () {
  console.log(this)
}) 
// this指向 timeout对象
// node "c:\node\index.cjs"
// Timeout {
//   _idleTimeout: 1,
//   _idlePrev: null,
//   ....

// 箭头函数
setTimeout(() => {
  const foo = function () {
    console.log(this);
  }
  
  foo()
}, 1);
// 箭头函数当回调，箭头函数中调用了foo()，调用这个箭头函数的执行上下文为globalThis
// 输出：globalThis
```

再比如一个`Array.prototype`下的方法，接收回调，但回调调用时的this指向又这些API自己决定

## 箭头函数
箭头函数的this表现行为和普通函数不一样，箭头函数会在创建箭头函数时，在周围作用域创建一个封闭词法环境，封闭词法环境可以保证this指向是创建时的值，不会像其他函数在执行时可以更改this  

这种表现行为，很容易产生误解，会联想到箭头函数的this指向违反了函数执行上下文词法环境创建时确定this指向。其实是ECMAScript规范指定的行为，根据MDN，this函数会创建一个封闭词法环境，导致执行上下文创建时无法改变this（箭头函数调用时）

```js
const globalObject = this;
const foo = () => this;
console.log(foo() === globalObject); // this 指向 globalThis 或者 window

const obj2 = { 
  name: "obj2",
  func: () => {
    console.log(this);
  }
};
obj2.func()；//this 也指向globalThis 或者 window


const obj = {
  count: 10,
  doSomethingLater() {
    setTimeout(() => {
      // 定义这个箭头函数时，setTimeout也没有上下文，就会找到外面的上下文doSomethingLater
      // 使用了外部方法的“obj”上下文
      console.log(this.count);
    }, 300);

    setTimeout(function () {
      // 输出undefined，这个函数的this指向globalThis 或者 window
      // 不是箭头函数，回调执行的上下文是全局
      console.log(this.count);
    }, 300);
  },
};

// "."调用，作用域为前面对象执行上下文
obj.doSomethingLater(); // 输出 11
```

### 箭头函数使用call、apply
无效果，this指向上文提到不会发生动态改变，调用`call() apply()`会忽略传入的第一个值（第一个值不正是确认this指向的嘛）

## ESM和CJS中

### ESM
ESM模块里自动采用了严格模式，顶层的this一辈子都是undefined

### CJS
CJS本质每一个模块如下代码
```js
(function (exports, require, module, __filename, __dirname) {
  function someFunc() {}
  exports = someFunc;
  // At this point, exports is no longer a shortcut to module.exports, and
  // this module will still export an empty default object.
  module.exports = someFunc;
})
```
cjs中的代码其实是在函数中，导出`module.exports`默认为`{}`一个空对象，所以顶层的this指向这个空对象

#### CJS中的箭头函数，普通函数
```js
// cjs中顶层调用
const foo = function () {
  console.log(this);
}

const arrow = () => console.log(this);

foo() // globalThis
arrow() // {}
```
绝了，因为箭头函数是定义时形成封闭词法环境，那会儿的this为`{}`空对象，见上文  
`foo`调用时执行上下文为全局执行上下文，全局执行上下文为`globalThis`

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

```JavaScript
// worker对象，其中包含一个slow方法，也是slow方法的作用域
let worker = {
  value: 10,
  slow(x) {
    return x * this.value; // (*)
  }
};

function decorator(func) {
  return function(x) {
    // 这里是一个闭包，this指向调用对象worker
    let result = func.apply(this, [x]); // 现在 "this" 被正确地传递了
    return result;
  };
}

worker.slow = decorator(worker.slow);
alert( worker.slow(2) ); // 输出 20
```

### 针对不同函数

* 箭头函数：传入的`thisArg`无效，箭头函数中的this还是指向创建环境

# bind
返回一个新的函数，可以指定这个函数执行上下文中的this指向，俗称“绑定”

```js
// 函数签名
// bind(thisArg, arg1, arg2, /* …, */ argN)

```






