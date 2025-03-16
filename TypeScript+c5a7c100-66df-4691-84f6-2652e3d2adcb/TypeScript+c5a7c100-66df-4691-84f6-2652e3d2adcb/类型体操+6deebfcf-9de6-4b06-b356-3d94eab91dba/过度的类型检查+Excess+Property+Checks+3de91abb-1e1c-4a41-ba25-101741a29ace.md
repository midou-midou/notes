过度的类型检查，就是`ts`类型检查会扩大被检查的对象的类型，导致程序过不去类型检查的阶段

注意，`ts`过不去类型检查不意味着代码不能运行，毕竟最后都是编译为`js`怎么可能不能运行

例子，上面解释应该看完例子看

```TypeScript
interface SquareConfig {
  color?: string;
  width?: number;
}
 
function createSquare(config: SquareConfig): { color: string; area: number } {
  return {
    color: config.color || "red",
    area: config.width ? config.width * config.width : 20,
  };
}
 
let mySquare = createSquare({ colour: "red", width: 100 });
// 报错在此代码框的下面
```

接上面的：line 14
Argument of type '{ colour: string; width: number; }' is not assignable to parameter of type 'SquareConfig'. Object literal may only specify known properties, but 'colour' does not exist in type 'SquareConfig'. Did you mean to write 'color'?

上面的`13行`接收了一个对象字面量，这个字面量里面有个`colour`的属性，和函数签名的`SquareConfig`这个类型的属性`color`对不上，所以会触发`过度类型检查`

比如我给你传的就是叫`colour`的属性， 你`ts`凭啥认为是我把`color`写错了呢，这就是`过度类型检查`

官方的handbook提供了几种小技巧可以避免这种检查

#### 1. 传一个变量进去

```TypeScript
let squareOptions = { colour: "red", width: 100 };
let mySquare = createSquare(squareOptions);
```

就是把上面的`对象字面量`赋给一个变量，再把这个变量给待检查的函数

这样做为啥行呢？

因为`ts`这时候就会找传进来的这个变量`squareOptions`和函数签名的共同属性，他俩都有`width`这个属性，所以放行，能通过函数签名的检查

如果没有公共属性呢，那不好意思，通过不了

```TypeScript
let squareOptions = { colour: "red" };
let mySquare = createSquare(squareOptions);
```

`Type '{ colour: string; }' has no properties in common with type 'SquareConfig'.Type '{ colour: string; }' has no properties in common with type 'SquareConfig'.`



#### 2. 类型断言+索引签名

指定`对象字面量`的类型的做法

```TypeScript
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
```

对应的`SquareConfig`类型

```TypeScript
interface SquareConfig {
  color?: string;
  width?: number;
  [propName: string]: any;
}
```

只要属性名是`string`类型的就可以，不限制这个属性的名称是`color`或者`width`



