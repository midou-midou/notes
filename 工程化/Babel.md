# 概念

Babel 把 ES新规范的 js代码转换成老版本规范的 js 代码，所以，Babel 本质是 js 到 js 的编译器

# 配置

怎么配置，得看如何使用 Babel，比如直接安装 Babel 进行配置，或者在其他脚手架工具（如. vite）中使用Babel

## 配置文件

- babel.config.js (.json、.js、.cjs 等)
- .babelrc或者(.babelrc.json、.babelrc.js)等文件
- package.json 中的 `babel`配置项

第一个配置文件 `babel.config.*`是项目范围的，优先级最高，可以配置 `configFile`为`false`来禁用这个配置文件
`.babelrc`配置文件可以配置到每一个 `package`中，这些单独配置的 babel 规则会被 Babel 自动合并成为一个项目级别的配置文件，可以配置 `babelrc`为 `false`来禁止这个文件

一般如果是 `monorepo`还是建议统一配置到 `babel.config.*`配置文件中，这个文件一定要在 `babelrcRoots`指定的目录中，或者 `configFile`配置下指定的文件。否则，各种 babel 文件合并，每一个babel 命名方式不同优先级不同导致的到底使用的哪一个配置文件都会有很多的问题

# 插件 plugins

Babel 在代码 `transform`转换阶段，会有很多的 `Hook`，Babel 提供了插件 `plugins`，可以自定义插件让 Babel 在转换代码的时候执行插件对应的 `Hook`

## 配置插件

在 `babel.config.json`中
```json
{
  "plugins": ["babel-plugin-myPlugin", "@babel/plugin-transform-runtime"]
}
```

在 webpack 中
```js

// babel-loader
module: {
  rules: [
    {
      test: /\.(?:js|mjs|cjs)$/,
      exclude: /node_modules/,
      use: {
        loader: 'babel-loader',
        options: {
          targets: "defaults",
          presets: [
            ['@babel/preset-env']
          ],
          plugins: ['@babel/plugin-proposal-decorators', { version: "2023-11" }]
        }
      }
    }
  ]
}
```

### 插件执行顺序

插件执行在 `preset`之前，`preset`中配置的顺序是从后往前执行，`plugins`中配置的插件是从前往后执行，并且，`plugins`配置的插件要在 `preset`配置的之前执行

### babel-plugin-import

> antd等组件库的按需导入插件

```js
import { Button } from 'antd';
ReactDOM.render(<Button>xxxx</Button>);

      ↓ ↓ ↓ ↓ ↓ ↓ 下面是转换的目标

var _button = require('antd/lib/button');
require('antd/lib/button/style/css');
ReactDOM.render(<_button>xxxx</_button>);

```

首先说一下这个插件要干什么？

目的是 `import` 具名导入 `Button`，转换成 `require`后要准确的拼出导入 `Button`的路径 `antd/lib/button`，同时，如果有样式，也要拿到完整相对路径去导入
（这只是这个插件支持的其中一种形式而已）
如果浏览器支持 `import`也可以直接输出

![[babelImportplugin.svg]]

上面的图片展示了 Babel 的基本工作原理，也展示了 `Babel-plugin-import`的工作原理

# presets

是官方提到的 `@babel/preset-env`包，配置在 `presets`选项下。目的是提供了一些转换代码的预设，当然，如果仅仅想转换其中的一些特性，可以配置，或者安装对应的转换 plugin

## 配置

```js
// babel.config.js

{
  "presets": [
    [
      "@babel/preset-env",
      {
        // options
      }
    ]
  ]
}
```

### 预设执行顺序
是从后往前执行的，且在 `plugins`插件选项执行后才执行预设


