# vue脚手架

### 脚手架的结构

![https://static.xuyanshe.club/img/20230304154840.png](https://static.xuyanshe.club/img/20230304154840.png)

### 脚手架中引入的vue版本和完整版的vue.js有什么区别

1. `vue.js`是完整版的Vue，包含：核心功能 + 模板解析器。

2. `vue.runtime.xxx.js`是运行版的Vue，只包含：核心功能；没有模板解析器。

```JavaScript
// 模版解析器
const app = Vue.extend({
            template:`
                <div>    
                    <hello></hello>
                </div>
            `,
            components:{
        hello
            }
        })
// vue.runtime.js
new Vue({
  el:'#root',
  render: createElement => createElement(<App />)
})
```

### 配置文件vue.config.js

`vue.config.js` 是一个可选的配置文件，如果项目的 (和`package.json` 同级的) 根目录中存在这个文件，那么它会被 `@vue/cli-service`自动加载

[see more](https://cli.vuejs.org/zh/config)
