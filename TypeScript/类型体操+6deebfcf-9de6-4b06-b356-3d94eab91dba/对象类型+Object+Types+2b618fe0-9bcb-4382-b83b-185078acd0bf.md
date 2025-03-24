对象类型是啥？

是一个类型，不是一个对象，注意主次关系，对象在前，类型在后，对象是修饰类型的。重点是一个类型，而不是一个对象

常见的：

```TypeScript
type Person = {
  name: string;
  age: number;
};
```

```TypeScript
interface Person {
  name: string;
  age: number;
}
```

上面的都是类型

## 可选、只读

可以给对象类型的`key`加一些操作符，比如让其可选或只读

```TypeScript
interface PaintOptions {
  shape: Shape;
  xPos?: number;
  readOnly yPos: number;
}
```

