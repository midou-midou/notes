js中没有枚举，很不方便，比如一个功能有多个状态，直接使用数字表示很不直观，过段时间没人知道这些数字代表的什么东西

枚举的作用就是解决这个问题，可以使用枚举类型的`key`拿到他的`val`也可以反过来

#### 语法

- 枚举`key`必须是字符串，`val`必须是`string`或者`number`，为啥呢？因为编译成js要让枚举的`val`作为对象的`key`，只能是这两种类型

- 只有数字枚举可以自增，也称之为计算枚举



js里面如果要实现类似枚举的功能，就得在一个对象里面把`key`和`val`翻来覆去的便利一遍

```TypeScript
enum FooEnum{
  Bar = 1,
  Bar1,
  Bar2,
}
```

```JavaScript
(function (FooEnum) {
    FooEnum[FooEnum["Bar"] = 1] = "Bar";
    FooEnum[FooEnum["Bar1"] = 2] = "Bar1";
    FooEnum[FooEnum["Bar2"] = 3] = "Bar2";
})(FooEnum || (FooEnum = {}));

// { '1': 'Bar', '2': 'Bar1', '3': 'Bar2', Bar: 1, Bar1: 2, Bar2: 3 }
```

下面的js代码就是上面的枚举编译后的结果，但一般都不会这样写吧，可读性太低了





