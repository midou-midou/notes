`interface`和`namespace`声明的类型可以合并，合并的方法就是定义相同名称的类型

简单例子解释概念

```TypeScript
interface Box {
  height: number;
  width: number;
}
interface Box {
  scale: number;
}
let box: Box = { height: 5, width: 6, scale: 10 };
```

合并的时候会根据属性的类型等有一个排序

```TypeScript
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


// 合并后
interface Document {
  createElement(tagName: "canvas"): HTMLCanvasElement;
  createElement(tagName: "div"): HTMLDivElement;
  createElement(tagName: "span"): HTMLSpanElement;
  createElement(tagName: string): HTMLElement;
  createElement(tagName: any): Element;
}
```

命名空间的合并要注意，在命名空间下的成员是导出的（也就是`export`的）要不然会找不到这个没有导出的成员



