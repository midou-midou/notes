# Export和Exposts

## export

**export**为ES6中的模块导出声明，浏览器、node ESM均支持。但是引入必须是`import`

```JavaScript
export var a = "abc";
var a = "abc";
export {
	a
}
export default myModule;
```

在js中，如果要导入需要使用import关键字，且只有浏览器、node的ESM模式支持 `import`

```JavaScript
import myModule from './myModule.js';
```

如果模块为一个文件夹，文件夹中js文件为index.js。导入模块可以省略“.js”后缀 如果导入为一个对象，可以使用解构赋值 `import { a } from './myModule';`

### 注意：

一个文件中的多个import会汇总到js文件的最上方最先执行，无论`import`语句中间是否还有其他的js语句

```JavaScript
import './test.js'
console.log("1000")
import './test1.js'
```

上面的代码的执行顺序为： 先执行下`import`引入的两个js，`test.js`和`test1.js`。最后在执行`console.log`

### 默认导出

```JavaScript
export default function1
```

默认导出的东西可以在导入的时候重命名

```JavaScript
import func1 from 'func.js' // 默认导出会有一些问题，下面会提到
```



### 多导出

导出的文件中可以有多个`export`引导的导出语句，但是只能有一个`export default`引导的语句

```JavaScript
export default function1
export const function2
```

```JavaScript
import {default as func1, function2} from 'func.js'
```

导入要用解构，且默认导入要重命名（重命名导入）

#### 默认导出的一些问题

所以说，如果有了`默认导出`，那么那个导出的文件中就不应该再有其他的导出了（也就是一个文件只有一个默认导出），要不然上面的导入写法很恶心，并且你的默认导出的命名也无效，也不清楚模块中默认导出是啥



### 导出聚合

```JavaScript
export { default as function1, function2 } from "bar.js";
export { function3 } from "bar1.js"
```

```JavaScript
import { default as function1, function2, function3 } from "foo.js";
```

上面在`foo.js`中使用了`export ... from ...`的格式，在`main.js`中使用了*`import`*导入了`foo.js`，在`foo.js`中的导出就是聚合导出，聚合导出了`bar.js`还有`bar1.js`



## exports

`exports`为`nodejs cjs` (common js)导出的语句，`nodejs`可以用Babel编译工具从而识别`import`语法

**从node v13开始node自己就支持了ESM，但需要额外配置或放到特定的`.mjs` 文件中**

`module.exports`和 `exports.xxx`  是一样的，nodejs中将需要导出的内容放到了`exports`这个对象的身上，让其成为`exports`的属性

`exports` 身上的属性会自动添加到模块的根部

```JavaScript
exports.a = {
 	myFunction: function () => {} 
}
module.exports = {
		// something
}

// 默认导出
module.exports = a
```

引入模块在nodejs中使用require (nodejs中的commonjs写法)

```JavaScript
const a = require('myModule.js').a;
a.myFunction();
```

`module.exports`改变会导致 `exports.xxx`的改变，exports仅作为 `module.exports`的一个引用。当 `module.exports`改变后 `exports`就失效了

```JavaScript
exports.a = function(){
  console.log('a')
}
module.exports = {a: 2}
exports.a = 1
```

```JavaScript
var x = require('./foo');
console.log(x.a)
result: 2
```



