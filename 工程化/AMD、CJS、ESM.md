# AMD、CJS、ESM

### 概念

- AMD (Asynchronous Module Definition): 在浏览器中使用，并用 define 函数定义模块

- CJS (CommonJS): 在 NodeJS 中使用，用 require 和 module.exports 引入和导出模块

- ESM (ES Modules): JavaScript 从 ES6(ES2015) 开始支持的原生模块机制，使用 import 和 export 引入和导出模块

### AMD

是通过[RequireJS](https://requirejs.org/) （一个js文件，需要使用`script` 标签引入）是一个 module loader——模块加载器。给浏览器使用的，让浏览器支持模块化

### CJS

是Nodejs也就是后端js原始的模块化js的方式

`.cjs` 文件、 `package.json` 中配置`type`为`commonjs` 、或者 `require()` 函数默认调用`cjs`模块加载器

### ESM

浏览器ES6开始支持的导包方式，需要在 `sctipt type=module` 标签中来使用

nodejs需要使用 `.mjs` 文件，或者在 `package.json` 中配置 `type` 为 `module` 、或者启动node项目增加命令行参数 [--input-type](http://url.nodejs.cn/wSPyh9)



