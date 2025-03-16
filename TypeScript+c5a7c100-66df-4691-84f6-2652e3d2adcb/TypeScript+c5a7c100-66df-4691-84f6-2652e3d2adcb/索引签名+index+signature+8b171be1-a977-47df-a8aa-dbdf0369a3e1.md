索引签名，可以用在“对象”“函数签名”等需要索引的地方

解决的问题就是：只知道具体的对象和具体对象的值，但是不知道键

下面简单的例子

```TypeScript
interface Dict{
  [key: number | string]: string
}

function printDictVal(d: Dict) {
 for (const key in d) {
  console.log(d[key]) 
 }
}

let d: Dict = {
  1: 'foo1',
  '2': 'foo2'
}

printDictVal(d)
```

上面定义了一个`Dict`字典，键不知道，但是知道它 (键) 的类型`string`和`number`

中间定义了一个函数，接收一个`Dict`类型的字典。注意，这个字典的**键**是不清楚的，这正是索引签名的作用

下面的`d`是一个对象，里面有两对键值对

PS. 索引签名的key类型如果是`number`那么就只能用`number`类型的数据作为key，但如果是`string`就可以用`number`或者`string`的类型作为key

```TypeScript
type Mapish = { [k: string]: boolean };
type M = keyof Mapish;
// type M = string | number
```



索引签名支持的类型有：`stringnumbersymbol`

### 工具类型 Record<Keys, Type>

这是ts中内定的一个工具类型（utility type）也可以实现类似索引签名的作用，但不受到索引签名key类型的限制，比如下面的例子中使用了字面量类型作为key

```TypeScript
interface CatInfo {
  age: number;
  breed: string;
}
 
type CatName = "miffy" | "boris" | "mordred";
 
const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
```



