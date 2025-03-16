可以通过条件表达式，根据条件表达式判断的结果返回一个类型

例子

```TypeScript
interface Animal {
  live(): void;
}
interface Dog extends Animal {
  woof(): void;
}
 
type Example1 = Dog extends Animal ? number : string;
// type Example1 = number
// 条件表达式是：Dog extends Animal 接口Dog是否继承了接口Animal，是的话返回一个number类型，不是则返回string类型
```

条件类型可以简化一些写法，比如说

```TypeScript
interface IdLabel {
  id: number /* some fields */;
}
interface NameLabel {
  name: string /* other fields */;
}
 
function createLabel(id: number): IdLabel;
function createLabel(name: string): NameLabel;
function createLabel(nameOrId: string | number): IdLabel | NameLabel;
function createLabel(nameOrId: string | number): IdLabel | NameLabel {
  throw "unimplemented";
}
```

有一个`createLabel`方法，接收`id: number`或者`name: string`类型，返回两个接口类型的数据`IdLabel`和`NameLabel`

上面用了穷举，把所有可能的的接收和返回都列出来了

但是可以用`条件类型`来简化一下

```TypeScript
type NameOrId<T extends number | string> = T extends number
  ? IdLabel
  : NameLabel;
```



### infer关键字 推断关键字（只能在条件类型里面用）

这里有点绕

如何拿一个任意类型数组中一个元素的类型呢

```TypeScript
type Flatten<T> = T extends any[] ? T[number] : T;
```

上面的做法当然可以，`T`为一个任意数组类型，任意的数组类型肯定会继承`any[]`类型，之后使用了`索引获取类型（indexed access types）`（也就是`number`，毕竟一个数组的索引都是数字嘛）获得这个`T`类型数组中元素的类型

这样做扩大了类型，没有做到类型约束

下面是做了类型约束的写法

```TypeScript
type Flatten<Type> = Type extends Array<infer Item> ? Item : Type;
```

`infer Item`就是推断出数组中每一个元素的类型，`Item`是每一个元素的类型

所以这个`infer`是作为一个类型变量的前关键字的，而不是值变量的关键字。比如说这里不能用`typeof`代替`infer`

回归正题，上面写的好处就是明确指定了`Type`类型的数组是不是继承了`Array<infer Item>`类型数组，比如说

```TypeScript
type Str = Flatten<string[]>

// type Str = string
```

这样做到了绝对的一对一，也就是类型约束



### 分发条件类型（在条件类型中使用联合类型）

例子

```TypeScript
type ToArray<Type> = Type extends any ? Type[] : never;
 
type StrArrOrNumArr = ToArray<string | number>;
```

给了一个`string | number`类型，根据条件类型表达式走`true`条件，会返回`string[] | number[]`联合类型

传进去的联合类型`string | number`，和`ToArray`组装成`ToArray<string> | ToArray<number>`，再和数组类型组装成数组联合类型`string[] | number[]`

如果想要一个联合数组类型呢`(string | number)[]`

需要用方括号，把类型括起来

```TypeScript
type ToArrayNonDist<Type> = [Type] extends [any] ? Type[] : never;
 
type StrArrOrNumArr = ToArrayNonDist<string | number>;
```



