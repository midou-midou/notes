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

## 继承
`interface`可以继承其他对象类型（包括 `type` 类型和 `interface` 类型还有 `class` 类型），使用 `extends`关键字
`type`不可继承

## 重载
方法属性的参数为字面量类型的在 interface 中有最高优先级，其他同名类型会遵循 **后来居上**的原则
```js
interface Document {
  createElement(tagName: any): Element;
}
interface Document {
  createElement(tagName: "div"): HTMLDivElement;
  createElement(tagName: "span"): HTMLSpanElement;
}
interface Document {
  createElement(tagName: string): HTMLElement;
  createElement(tagName: "canvas"): HTMLCanvasElement;
}

// 等同于
interface Document {
  createElement(tagName: "canvas"): HTMLCanvasElement;
  createElement(tagName: "div"): HTMLDivElement;
  createElement(tagName: "span"): HTMLSpanElement;
  createElement(tagName: string): HTMLElement;
  createElement(tagName: any): Element;
}
```

