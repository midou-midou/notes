unknown (the top types)最顶层的类型

既然是最顶层的类型，可以将任何值赋值给一个`unknown`变量

可以做到类型收窄的作用，为什么这么说？就是因为一个`unknown`类型的变量只能赋值给一个`any`类型的变量或者是`unknown`类型的变量，而相反的，一个`any`类型的变量可以赋值给任意类型的变量

```TypeScript
const anyVar: any = 1
const unknownVar: unknown = 2

const num1: number = anyVar
const num2: number = unknownVar
```

在哪里用这个`unknown`

比如

- 接口返回值，不确定类型，可以用

- 接收、处理前端之外的，不确定类型的一些数据



