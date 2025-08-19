# webpack

## 概念

按照官方的定义：

webpack是一种静态资源的打包器。比如说有两个模块都通过CJS或者是ESM引入了一些js库，通过webpack就可以将引入的这些js库打包到bundle中去。再比如，要通过require的方式引入一个非js的文件，也可以通过webpack来转换为可以让其直接引用的模块

其实说白了，就是处理前端资源、管理、打包的工具，但是`webpack`只能处理`js`和`json`格式的文件，处理不了其他格式的文件，如果要处理其他格式的文件，就需要`loader`

![https://static.xuyanshe.club/img/20210708121523.png](webpack/20210708121523.png)

## 术语

- `Entry`（入口）：webpack编译模块依赖图的起点，webpack会从这里开始打包模块需要的静态资源文件

- `Output`（出口）：webpack打包后的bundle的输出位置和名字

- `Loaders`：能让webpack处理各种各样的文件的工具或者说是模块，比如React的JSX，就需要一个react-loader来进行处理

- `Plugins`：可以完成各种打包工作中的事情，比如注入环境变量，压缩bundle等
- `module`（模块）：是webpack内部的类型，包括esm、cjs等定义的模块，同时，引入的css、图片等资源文件都会成为webpack模块
- `chunk`：webpack编译过程中的中间产物，最后要交由转换生成最终的bundle
- `bundle`：webpack打包后的最终生成产物

## 流程

1. 读取合并配置
2. 使用配置创建编译器
3. 插件注册
4. 编译器从入口文件开始编译（可能会有一些插件执行Hook）
5. 使用loader处理入口文件
6. 生成ast语法树
7. 通过AST收集依赖
8. 解析依赖执行
9. 再次使用loader处理文件，生成语法树，收集依赖这个过程
10. 编译完毕（可能还有插件Hook执行）
11. 编译产物转换成最终产物，可以在环境运行的

其中loader和plugin的执行时机是不一样的

## 模块解析
* 绝对路径解析
* 相对路径解析
* 模块文件夹解析

模块文件夹找包还是要看package.json文件是否存在，并且检查webpack配置中的`resolve.exportsFields`（和package.json的exports字段对应）和`resolve.mainFields`（和package.json的main、browser字段对应）

## 模块联邦 Module Federation

微前端解决方案
