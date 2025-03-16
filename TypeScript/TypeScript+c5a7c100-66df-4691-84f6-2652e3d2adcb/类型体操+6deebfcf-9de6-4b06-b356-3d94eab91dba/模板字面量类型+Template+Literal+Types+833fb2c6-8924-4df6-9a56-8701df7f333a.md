就是“字面量类型”，但是这个“字面”也就是字符串的部分，是一个“模板字符串”

注意⚠️，这里都是指的类型变量（`type`关键字声明），而不是一般变量、常量等

例子

```TypeScript
type World = "world";
 
type Greeting = `hello ${World}`;
// type Greeting = "hello world"
```

这样的好处是更灵活了，模板字符串可以使用一个类型变量替换，替换后能组成很多不同的类型

如果是联合模板字面量类型，最终组合成的类型的数量是每一种模板字面量类型可能组合成的数量的乘积（看着绕，这东西就是简单的排列组合）

例子

```TypeScript
type EmailLocaleIDs = "welcome_email" | "email_heading";
type FooterLocaleIDs = "footer_title" | "footer_sendoff";
 
type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;
// type AllLocaleIDs = "welcome_email_id" | "email_heading_id" | "footer_title_id" | "footer_sendoff_id"

```

最后的`AllLocaleIDs`是四种类型，不就是上面的`2*2`种类型嘛



