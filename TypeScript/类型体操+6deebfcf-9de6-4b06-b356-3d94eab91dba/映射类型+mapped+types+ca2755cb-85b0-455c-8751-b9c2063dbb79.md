得和“索引签名”配合，在不知道一个对象类型有哪些`key`的前提下，注意⚠️，这里指的是类型，而不是值

要从另外一个类型拿到`key`

```TypeScript
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};
```

这里有点复杂，`Type`是一个对象类型，`OptionsFlags`类型的`key`是由`Type`这个类型决定的，`Type`有什么`key`，`OptionsFlags`就有什么`key`

这样做的好处就是，我们拆分出了`key`，但给`key`赋予的`value`的类型可以不同，比如说原来的`Type`里面`value`的类型为`Function`，新映射出来的类型可以为`boolean`

关键在于`in keyof`

还有一个用途的例子

```TypeScript
type MappedTypeWithNewProperties<Type> = {
    [Properties in keyof Type as NewKeyType]: Type[Properties]
}
```

这里多了`as`关键字，意思是可以把`key`换一个名字

```TypeScript
type Getters<Type> = {
    [Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property]
};
 
interface Person {
    name: string;
    age: number;
    location: string;
}
 
type LazyPerson = Getters<Person>;

// type LazyPerson = {
//  getName: () => string;
//  getAge: () => number;
//  getLocation: () => string;
//}
```

上面的例子`Person`类型映射的新类型`LazyPerson`，把`Person`的`key`重命名为了`getter`方法的名字



