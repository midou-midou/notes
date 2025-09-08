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

