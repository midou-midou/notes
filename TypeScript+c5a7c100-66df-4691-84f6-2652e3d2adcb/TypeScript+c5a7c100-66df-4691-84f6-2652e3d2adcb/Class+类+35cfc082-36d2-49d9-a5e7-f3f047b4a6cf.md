类在`ES6`里面是一个语法糖(并不完全是，里面有一个内部标记`[[IsClassConstructor]]: true`，并且，必须用`new`关键字去取一个实例)

ts中的类多了一些ts中的特性

## 对于`field`类字段

字段的声明、确定赋值断言、`readonly`只读字段前缀

```TypeScript
class GoodGreeter {
  name: string;
  school! : string;    // 可以通过--strictPropertyInitialization选项关闭“没有赋值field字段”这个检查
  readonly address: string = "street one"
 
  constructor() {
    this.name = "hello";
  }
}
```



## `constructor`构造器

多了“继承类必须调用`super()`方法”的检查，类的`constructor()`构造函数，相当于添加一个原型属性，具体看

[原型](https://flowus.cn/ec12764a-2aa4-423b-b950-c2ffb58d391f)

`constructor()`构造函数的参数，如果在前面添加了可见性修饰符，会变为 “参数属性” （Parameter Properties），就是不需要像其他属性那样额外声明，可以当成一般的属性直接可以使用

```TypeScript
class Params {
  constructor(
    public readonly x: number,
    protected y: number,
    private z: number
  ) {
    // No body necessary
  }
}
const a = new Params(1, 2, 3);
console.log(a.x); // x属性是可以打印出值的
```



## 类方法

类中的方法，可以添加一些可见性修饰符



#### Getter和Setter

接收器和设置器，可以设置一个类的属性，或者获得到一个属性的值，`get`拦截器和`set`拦截器

```TypeScript
class Thing {
  _size = 0;
 
  get size(): number {
    return this._size;
  }
 
  set size(value: string | number | boolean) {
    let num = Number(value);
    if (!Number.isFinite(num)) {
      this._size = 0;
      return;
    }
    this._size = num;
  }
}

const foo = new Thing()
console.log(foo.size)
```



## 继承

继承的一些特性类似其他面向对象的语言的语法，可以实现接口`implement interface`或者继承一个类`extends class`

实现接口是约束一个类，让其必须实现什么什么方法或者有什么什么属性

继承是拓展一个类，可以让类有一些没有的方法，属性等

#### 重写

这个针对的是继承，让继承类可以修改继承方法的能力

重写的规则：必须是类的子类，继承关系，子类与父类的方法名、参数列表要相同，方法体不同，可见性修饰符要大于等于父类的





## 可见性

类有三种修饰符可以描述内部成员的可见性

`public`、`protected`、`private`

ts的`private`是软私有性，也就是说不可以通过`.`访问，但是可以通过`['prototype name']`括号属性名的方法访问

js的`#`描述符描述的属性是硬私有，也就是说既不能通过`.`也不能通过上面的括号语法访问。对类的外部是完全的不可见



### static

静态描述符，可以让一个属性通过类名直接访问，不用通过类的实例化对象访问

有些名称不能作为静态属性`name`、`length`、`call`等，`name`是因为`Function.name`冲突了

#### static块 静态块

```TypeScript
class Foo {
    static #count = 0;
 
    get count() {
        return Foo.#count;
    }
 
    static {
        try {
            const lastInstances = loadLastInstances();
            Foo.#count += lastInstances.length;
        }
        catch {}
    }
}
```
可以添加一些初始化的操作在静态块中



## 抽象类

这点和其他面向对象的语言差不多，抽象类必须实现所有方法、属性。不能实例化抽象类，只能继承









