## 1. webpack是什么

按照官方的定义：

webpack是一种静态资源的打包器。比如说有两个模块都通过CJS或者是ESM引入了一些js库，通过webpack就可以将引入的这些js库打包到bundle中去。再比如，要通过require的方式引入一个非js的文件，也可以通过webpack来转换为可以让其直接引用的模块

其实说白了，就是处理前端资源管理、打包的工具，但是`webpack`只能处理`js`格式的文件，处理不了其他格式的文件，如果要处理其他格式的文件，就需要`loader`

![https://static.xuyanshe.club/img/20210708121523.png](https://static.xuyanshe.club/img/20210708121523.png)

## 2. webpack中的入口input、出口output、loader、plugin各个都是些什么

- 入口`input`：webpack构建模块依赖图的起点，webpack会从这里开始打包模块需要的静态资源文件

- 出口`output`：webpack打包后的bundle的输出位置和名字

- `loader`：能让webpack处理各种各样的文件的工具或者说是模块，比如React的JSX，就需要一个react-loader来进行处理

- `plugin`：拓展webpack的一些插件，常用的会紧接着补充

## 3. webpack中的loader

loader可以让webpack打包各种各样的文件，img，ttf，css，jsx等等。在使用这些资源的时候可以直接的import到相应的模块中去。在官方文档中将一个文件引入另一个文件这种关系称之为依赖关系。webpack可以自动的生成一张各种文件之间依赖关系的图（依赖图）

---

首先，我创建了一个项目

![https://static.xuyanshe.club/img/20210709040442.png](https://static.xuyanshe.club/img/20210709040442.png)

只要关心`app`文件夹和`public`文件夹以及`webpack.config.js`文件

app文件夹中的`entry.js`代码

```JavaScript
import greet from './greet'
import Image from '../res/tetris.png'

const ImageDom = document.createElement('img');
ImageDom.src = Image;

document.getElementById("container").appendChild(greet());
document.getElementById("container").appendChild(ImageDom);
```

我的目标是直接通过`import`引入一张图片，图片在res文件夹下。现在就需要`fs-loader`来完成这个任务了

在`webpack.config.js`中要这样配置

```JavaScript
module.exports={
    entry:__dirname+"/app/entry.js",   //入口文件
    output:{                           
        path:__dirname+"/public",      //输出文件的存放位置
        filename:"bundle.js"           //输出文件的名称
    },
    module:{
        rules:[
        {
            test: /\.(pngsvgjpggif)$/,
            use: [
                'file-loader'
                ]
        }
}
```

`module`是要使用的`webpack`模块，`rules`是对应的`*-loader`匹配的规则，里面`test`（条件）可以填写正则表达式，字符串等

`use`为loader的入口，表示你要用什么loader来处理

这样就很容易的完成了`webpack`的配置及简单的使用



