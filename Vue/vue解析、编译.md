# 思路
vue的模板解析和编译的过程可以简化为下面的图片


这里都以vue3为例

# 解析
vue模板文件会被vue解析器解析，生成vue ast语法树。语法树节点会描述标签名、属性、子节点、绑定事件、绑定样式、静态节点标记等信息

## 步骤
针对SFC文件的解析，会通过模板状态机来完成，类似HTML状态机解析成tokens

开始标签 -> 标签名 -> 标签属性（vue中独有的动态绑定，vfor，vif特殊属性的解析，事件绑定解析，指令解析等
）-> 开始标签结束 -> 其中子节点的解析或内容解析 -> 结束标签解析 -> 标签名 -> 结束标签结束

## 结果
生成出来的AST树分析优化后交由生成器处理

# 优化
vue会分析节点是否为静态节点，静态节点不会改变，意味着生成一次渲染函数就可以缓存起来，方便以后使用


# 编译

AST数据要通过vue代码生成器生成最终的**渲染函数**，渲染函数都是js函数，可以直接执行。生成的渲染函数相当于上图中的**目标代码**

vue中的元素存在vue定义的很多属性，比如绑定，v-if，插槽等。针对每一个特殊属性都有对应的生成器生成对应代码

下面仅展示部分元素，属性的代码生成
- 静态元素：会生成`_m()`包裹的块  
  ```js
  function genStatic (el: ASTElement, state: CodegenState): string {
    el.staticProcessed = true
    state.staticRenderFns.push(`with(this){return ${genElement(el, state)}}`)
    return `_m(${
      state.staticRenderFns.length - 1
    }${
      el.staticInFor ? ',true' : ''
    })`
  }

  // renderStatic
  _m: (index: number, isInFor?: boolean) => VNode | VNodeChildren;
  ```
- v-if：简化了一下代码，可以看到最后生成一个三元表达式
  ```js
  function genIfConditions (
      conditions: ASTIfConditions,
      state: CodegenState,
      altGen?: Function,
      altEmpty?: string
    ): string {
      const condition = conditions.shift()
      if (condition.exp) {
        return `(${condition.exp})?${
          condition.block
        }:${
          genIfConditions(conditions, state, altGen, altEmpty)
        }`
      }
    }
  ```

最终生成的

# 解析 -> 编译

vue源码，比较容易看懂，没有精简
```js
// main src/compiler/index.js
export const createCompiler = createCompilerCreator(function baseCompile (
  template: string,
  options: CompilerOptions
): CompiledResult {
  // 这一步对应的 解析
  const ast = parse(template.trim(), options)
  // 这一步是优化 vue会标记永不改变的节点为静态节点，静态节点不需要反复对应的渲染函数，第一次生成好了就缓存起来
  if (options.optimize !== false) {
    optimize(ast, options)
  }
  // 编译，实则为渲染函数生成
  const code = generate(ast, options)
  return {
    ast,
    render: code.render,
    staticRenderFns: code.staticRenderFns
  }
})

```