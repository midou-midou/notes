# ref、props、mixin、插件、scoped、template

### ref属性

引用，和react的ref差不多，都是为了避免原生的DOM操作，给子组件注册引用信息。 普通的html标签上的`ref`指向的是这个DOM元素，普通的组件的`ref`属性指向的这个组件的实例对象

```HTML
<h1 v-text="msg" ref="title"></h1>
<button ref="btn" @click="showDOM">点我输出上方的DOM元素</button>
<School ref="sch"/>
```

```JavaScript
export default {
  name:'App',
  methods: {
    showDOM(){
      console.log(this.$refs.title) //真实DOM元素
      console.log(this.$refs.btn) //真实DOM元素
      console.log(this.$refs.sch) //School组件的实例对象（vc）
    }
	}
}
```



### props属性

类似react的props属性，都是接受父组件传递的值。接收的方式有下面的三种

```JavaScript
//简单声明接收
props:['name','age','sex'] 
//接收的同时对数据进行类型限制
props:{
  name:String,
  age:Number,
  sex:String
}
//接收的同时对数据：进行类型限制+默认值的指定+必要性的限制
props:{
  name:{
    type:String, //name的类型是字符串
    required:true, //name是必要的
  },
  age:{
    type:Number,
    default:99 //默认值
  },
  sex:{
    type:String,
    required:true
  }
}
```

但是，传入子组件的prop属性，都是直接放到了子组件的身上，但是没有放到`_data`下。可以直接使用`this.xx`来使用。react中是放到了组件的props属性下，使用的时候要`this.props.xxx`



### mixin属性

混合属性，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项

```JavaScript
// 定义一个混入对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}
// 定义一个使用混入对象的组件
var Component = Vue.extend({
  mixins: [myMixin]
})
var component = new Component() // => "hello from mixin!"
```

如果混入对象中的配置对象和组件本身的属性重名了，那么先使用组件身上的配置对象 

如果混入对象上配置的是生命周期，那么组件的生命周期和混入对象配置的生命周期都会执行

#### 全局混入

通过`Vue.mixin()`API创建，每一个组件都会自动使用到这个混入器

```JavaScript
// 为自定义的选项 'myOption' 注入一个处理器。
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// => "hello!"
```



使用了混入，要注意。如果组件中定义了相同名称的方法，属性名等，会以组件为主，混入为辅，混入的数据不会替换原始组件的数据

组件中定义了相同的钩子，将**先执行**混入对象的，**再使用**组件自己的

```JavaScript
// 定义一个混入对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}

// 定义一个使用混入对象的组件
var Component = Vue.extend({
  mixins: [myMixin]
})

var component = new Component() // => "hello from mixin!"
```



### 插件

通常用来为 Vue 添加全局功能（增强功能） 比如可以在插件中添加一个`mixin`混入，里面配置一个`data`下面有`abc`。这样，vue中的每一个组件都有了这个`abc`属性 再比如，可以添加一个过滤属性，里面做一些数据的操作，比如`a+b`。这样所有的vue组件都可以使用这个过滤器了

### 使用方法

```JavaScript
// 调用 `MyPlugin.install(Vue)` 要在new Vue()之前使用
Vue.use(MyPlugin)
new Vue({
  // ...组件选项
})
```

插件的定义： 插件必须要实现`install`方法，默认接收两个参数，第一个方法就是`Vue`构造器，第二个参数是一个可选择的对象

```JavaScript
export default {
  install(Vue,options){
    //定义混入
    Vue.mixin({
      data() {
        return {
          option:{a:100, b:200}
        }
      },
    })
    //给Vue原型上添加一个方法,vm(vue实例对象)和vc(vuecomponent实例对象)就都能用了
    Vue.prototype.hello = ()=>{alert('你好啊')}
  }
}
```

### scoped

可以限定住组件的样式范围，让其仅在当前组件中生效 (写样式很常用)

```HTML
<style scoped>

</style>
```

### 占位标签template

和react的`Fragment`标签一致，vue中如下功能

- 作为单文件组件最外层标签

- 插槽模版标签

- 列表渲染、条件渲染也可以在`template`标签上使用`v-for、v-if`。标签不会渲染成真实dom，可以增加代码可读性，使代码结构清晰

