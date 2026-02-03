可以使用一个对象类型的`key`作为索引去取得对应`value`的类型，和`js`中使用字面量或者变量去取一个对象的属性有点相似

例子

```TypeScript
type Person = { age: number; name: string; alive: boolean };
type Age = Person["age"];
// type Age = number
```

如果要获取一个数组中元素的类型，可以使用`number`，毕竟数组的索引是从`0`开始的`number`类型的数字

```TypeScript
const MyArray = [
  { name: "Alice", age: 15 },
  { name: "Bob", age: 23 },
  { name: "Eve", age: 38 },
];
 
type Person = typeof MyArray[number];
//type Person = {
//    name: string;
//    age: number;
//}
```



