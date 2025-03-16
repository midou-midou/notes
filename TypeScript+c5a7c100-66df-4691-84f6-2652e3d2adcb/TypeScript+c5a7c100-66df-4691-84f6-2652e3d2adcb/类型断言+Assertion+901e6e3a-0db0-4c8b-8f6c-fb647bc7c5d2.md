断言：就是让编译器以你自己指定的一个类型去解析

比如说，`document.getElementById`会返回一个`HTMLElement`的类型，但是这个类型太过于宽泛了，就可以使用断言来指定这个类型为`HTMLCanvasElement`（举例子，也有可能是其他的HTML元素）

```TypeScript
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```

断言的使用是在一个大类型（宽范围的类型）转换成一个特定的，明确的类型。比如上面是从`HTMLElement`类型转换成为一个`HTMLCanvasElement`类型，他们有交集的部分，这点很关键，也是理解断言怎么使用，是什么的关键

下面的类型断言就是错误的

```TypeScript
const x = "hello" as number;
```

Conversion of type 'string' to type 'number' may be a mistake because neither type sufficiently overlaps with the other. If this was intentional, convert the expression to 'unknown' first.

如果非得转换，就要将待断言的类型向上转型`unknown`，或者向下转型`any`

还有一种写法

```TypeScript
let value: any = "this is a string";
let length: number = (<string>value).length;
```

下面还有两种其他的断言

#### 非空断言

告诉编译器，这个变量绝对不是`null`或者`undefine`的，可以这么写

```TypeScript
function fun(value: string | undefined | null) {
  const str: string = value; // error value 可能为 undefined 和 null
  const str: string = value!; //ok
  const length: number = value.length; // error value 可能为 undefined 和 null
  const length: number = value!.length; //ok
}

```

#### 确定赋值断言

指的是确定这个变量一定会被赋值，写法如下

```TypeScript
let name!: string;
```

可以解决有些没有被赋值的变量，在没有被赋值前就使用了

比如说

```TypeScript
let count: number;
// let count!: number;

initialize();

// Variable 'count' is used before being assigned.(2454)
console.log(2 * count); // Error

// 编译器会认为count没赋值就使用了，因为赋值的函数在下面，编译器处理都是从上往下
// 这时就可以使用“确定断言赋值”告诉编译器这个值已经赋过值了，跳过检查放心使用吧！
function initialize() {
  count = 10;
}

```



