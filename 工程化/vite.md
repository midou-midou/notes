# vite

> vite的出现，使用浏览器ES6模块原生支持，无需打包js文件，js文件以模块加载

## 原理

### 开发

对应 `vite 或 vite dev` 命令

#### 流程

1. 使用配置创建httpserver `_createServer`（服务端）
   - 解析配置
   - 创建和vite client（浏览器端）的websocket链接
   - 根据配置创建文件监听器（`server.watch`）
   - 创建插件容器、注册插件
   - 文件监听器注册文件修改事件，`change`、`add`、`unlink`
   - 注册中间件（比如transform中间件）
2. 开启服务器监听 `server.listen()`  
   - 当浏览器发起资源请求，请求index.html
   - 请求依次通过注册的中间件，中间件的执行顺序参考🧅洋葱模型
   - `indexHtmlMiddleware`中间件会处理这个请求，因为这个中间件是处理主页的，要读取磁盘上的这个文件，把内容发送给浏览器
   ![indexHtmlMiddleware中间件断点截图]()
   - 之后浏览器解析index.html，解析到`<head>`中的`<script>`，这个是用于HMR的js文件，浏览器发送异步加载请求，vite服务器拿到并处理返回对应文件
   - 之后的其他标签就不说了，本来SPA应用没多少内容
   - 之后就到了`<body>`中的`<script>`，这个就是我们的main.js文件了
   - 浏览器发送请求main.js的请求，这里就说下请求在transformMiddleware中间件中的处理
     - 首先，会判断请求资源的类型是不是js、css、模块化的html（html文件转换成了vite使用的模块）以及import请求导入的资源请求（比如导入的图片就是import请求），如果不是，就跳过transformMiddleware中间件
     - 请求了入口文件main.js，vite服务器看是否有缓存，如果没有要使用协商缓存，让浏览器缓存返回给自己的main.js文件
     - 浏览器请求的是/src/main.js这个url，vite中定义了模块图这个对象，里面有url和磁盘文件的映射表，这样就可以通过请求的url拿到真实文件
     - 之后main.js头部要请求style.css，同样的原理，也要把`import './style.css'`转换成`import '绝对路径的style.css'`
     - 所以说，这就是transformMiddleware中间件完成的工作，其实是中间件代码运行的过程中调用了注册的插件的transform Hook，（`vite:import-analysis`这个插件提供的功能）
     - 同样，如果引入了别的.vue文件或者.js等文件，也是同样的处理方式
3. 这样一来，所有文件都由vite服务器处理并返回给浏览器了，浏览器渲染显示了

#### HMR


### 打包

#### 流程

1. 解析配置
2. 找到注册在打包阶段的插件plugins，执行插件Hooks
3. 使用rollup打包


