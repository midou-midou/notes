`never`(the bottom type) —— 最底层的类型

[Documentation - TypeScript for Functional Programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#other-important-typescript-types)

ts的类型理解为一棵自上而下的树，`never`就是最底层，不能接收任何东西，也不返回任何东西

两种情况

- 一个函数永远不会返回（这里的意思是函数里有个死循环，而不是不返回东西`void`）

- 一个函数抛出错误，则这个函数的返回类型可以是`never`。毕竟也是永远不可能返回东西

这里很绕，会混淆

为啥是最底层的类型呢？

应该就是只能用`never`去给其他的变量，函数声明类型，但是无法给`never`声明的类型赋值吧

