## import导入

#### node

可以在`.mjs`文件中使用（后端），不是`.mjs`会报错误`Cannot use import statement outside a module`

或者在`package.json`中使用配置项`"type": "module"`，`esm`的支持虽然是实验性的，但是已经在`node v14`后默认开启，不需要手动开启了

或者添加启动选项`--input-type`



#### 前端浏览器

浏览器环境需要`<script type="module" src="path_to_js"></script>`，`src`中引入的文件格式可以是`.js`或者是`.mjs`（准确说，设置了MIME类型`text/javascript`）



## 加载

#### 前端浏览器

默认的模块都会异步加载，相当于`<script defer>`给标签添加了`defer`属性，并且会按照顺序执行



#### node

正常导入，正常加载



## 找包策略(node)、找模块策略(前端浏览器)



    #### node下查找包的策略

node下找包，不是找文件，文件的导入必须精确到拓展名，而包就是有`package.json`文件的目录

node查找包的策略简单描述会按照下面的步骤来

这里是按照`commonjs`规则，也就是`require`导入为前提条件的

1. 直接去`require`引入的文件的路径去找这个文件 eg. `var x = require("./moduleB");`会去`/path/to/moduleB.js`找这个文件在不在，通过引入的路径（看看是不是下面的几种`'/''../''./'`）
或者也会去`node_modules`找，在当前文件夹下的`node_modules`，在上一级目录的`node_modules`，再上一级。直到根目录`'/'`（`package.json`所在目录）

2. 如果没有`moduleB.js`就要在文件夹`moduleB`下找`package.json`文件里面的`"main"`配置项，这个配置项指定了这个`moduleB`包下的主文件

3. 如果没有指定这个配置型呢，就会去找名字为`index.js`的文件，作为这个`moduleB`文件夹的主文件

4. 如果再找不到就会报错了`can't find package`



#### node下`ES module`找包策略

和上面的`cjs`有些许的不同，`package.json`中使用`module`配置项代替`main`配置项，还有就是`type`配置项要设置为`module`



#### 浏览器找文件策略

只要是文件夹，里面无论是否有`package.json`都可以按照`node`的找包策略来（应该是脚手架支持）



## export导出

[Export和Exposts](https://flowus.cn/469ff45c-397f-4ba2-af33-750b77087f23)

在模块中的顶层变量会处在每一个模块的词法作用域里，不会污染到全局的环境



