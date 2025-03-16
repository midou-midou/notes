> 命名空间在`ts`里还有些历史遗留问题，`ts1.5`之前，那会儿`ES6`还没出来，没有原生的`模块化`支持，那会儿的`namespace`被称之为`内部模块（Internal modules）`

`ts`中为了复用代码，更好的组织代码，会使用`namespace`声明，这个东西和`ts`中的`module`模块还不太一样，要注意⚠️

语法：

- 声明要在最外层，⚠️不要再在`namespace`前加一个`export`导出语法，如果使用了就要用`模块`的导入语法

- 内部的属性要暴漏给外部，可以使用`export`导出

来来来，看例子

```TypeScript
namespace Validation {
  export interface StringValidator {
    isAcceptable(s: string): boolean;
  }
  const lettersRegexp = /^[A-Za-z]+$/;
  const numberRegexp = /^[0-9]+$/;
  export class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string) {
      return lettersRegexp.test(s);
    }
  }
  export class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
      return s.length === 5 && numberRegexp.test(s);
    }
  }
}
```

`Validation`命名空间业务上做一些校验，`zip`包的校验，输入框仅字母的校验等。内部的两个正则是隐藏的，剩下的类默认是暴漏的

如何使用？

```TypeScript
// Some samples to try
let strings = ["Hello", "98052", "101"];
// Validators to use
let validators: { [s: string]: Validation.StringValidator } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();
// Show whether each string passed each validator
for (let s of strings) {
  for (let name in validators) {
    console.log(
      `"${s}" - ${
        validators[name].isAcceptable(s) ? "matches" : "does not match"
      } ${name}`
    );
  }
}
```
PS. 这个代码块和上面的代码块是一个，所以不用导入命名空间，如果声明的命名空间是一个单独的文件，则需要看下面的



### 多文件的namespace

多文件的`namespace`就是每一个文件一个`namespace`，这些文件的关系有两种方式来描述



#### 第一种：特殊标签`<reference path="" />`

这些`namespace`文件使用一种标签`<reference path="" />`（reference标签）来说明之间的关联

首先要有一个主`namespace`

例如

```TypeScript
namespace Validation {
  export interface StringValidator {
    isAcceptable(s: string): boolean;
  }
}
```

其他的`namespace`文件要使用`reference标签`标明自己和`Validation.ts`文件的关系

```TypeScript
/// <reference path="Validation.ts" />
namespace Validation {
  const lettersRegexp = /^[A-Za-z]+$/;
  export class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string) {
      return lettersRegexp.test(s);
    }
  }
}
```
看line1 的代码，指明了要和`Validation.ts`文件的关系，最后会自动合并

在测试的文件里

```TypeScript
/// <reference path="Validation.ts" />
/// <reference path="LettersOnlyValidator.ts" />

// Some samples to try
let strings = ["Hello", "98052", "101"];
// Validators to use
let validators: { [s: string]: Validation.StringValidator } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();
// Show whether each string passed each validator
for (let s of strings) {
  for (let name in validators) {
    console.log(
      `"${s}" - ${
        validators[name].isAcceptable(s) ? "matches" : "does not match"
      } ${name}`
    );
  }
}
```
主文件也要指明引入的这些命名空间的关系

之后要用`tsc`的`--outFile`选项指明 输出文件 和 主文件

```Shell
tsc --outFile sample.js Test.ts
```

如果不想用`reference标签`，就要手动指明所有命名空间文件在编译时

```Shell
tsc --outFile sample.js LettersOnlyValidator.ts Test.ts
```
就可以把`Test.ts`开头的标签声明去掉了



#### 第二种：别名Aliases

还需要再查查资料才能再写这一段，建设中

使用`import`关键字，可以声明一个特殊的symbol变量，可以作为引入命名空间或者其下变量的别名

这个和模块的`import`还有点区别，但是命名空间也是可以使用模块导入的，前提得导出

例子

```TypeScript
namespace Shapes {
  export namespace Polygons {
    export class Triangle {}
    export class Square {}
  }
}
import polygons = Shapes.Polygons;
let sq = new polygons.Square(); // Same as 'new Shapes.Polygons.Square()'
```



### 声明namespace （Ambient namespace declaration）

野生声明namespace，也就是既没有用模块导入，也没有用`refrence`标签、`import`别名指明定义namesapce的`.d.ts`文件中，使用`declare`声明关键字或者修饰符在`namespace`关键字前

这样声明出来的命名空间，就可以被其他文件看见，当然，`d.ts`文件也得配置在`tsconfig.json`文件中

```TypeScript
declare namespace D3 {
  export interface Selectors {
    select: {
      (selector: string): Selection;
      (element: EventTarget): Selection;
    };
  }
}
```

## namespace和modules

目的都是让项目结构更清晰，让代码可读性更好，复用性更好

[ES Module、Common JS](https://flowus.cn/f666b914-cdc3-4304-83f6-ea84e8271aea)

现在的前端脚手架或者构建工具都使用ESM，ES6模块支持。所以ts的`namespace`用的比较的少了，官方handbook也推荐使用`ESM`

而且，`namespace`仅是`ts`的特性，在js中也用不了，而且原生支持的东西，为什么还要用个别的东西替代掉呢？



