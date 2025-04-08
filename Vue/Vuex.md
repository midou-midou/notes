和rudux一样要解决多组件间通信的问题。比如非兄弟组件间的通行，以及很多组件的情况下

### 什么时候使用Vuex

- 多个组件依赖于同一个状态

- 来自不同组件的行为要变更同一个状态

- 共享状态

### 原理图：

![https://static.xiaoblogs.cn/img/1655980209729-62c6ba20-f1ca-41b8-92d6-f1cba68c23e4.png](https://static.xiaoblogs.cn/img/1655980209729-62c6ba20-f1ca-41b8-92d6-f1cba68c23e4.png)

### 术语解释：

**state**：`vuex`下面的一个公共的`state`，可以管理整个项目的所有状态

**action**：负责接收组件派发的事件，如果组件派发的事件仅有事件的类型，没有具体的值。具体的值需要从其他的后端API（也就是图中的Backend API）、获取或者做数据的加工，那么就需要action拿到这个值，之后`commit`给`mutations`，也可以直接返回值给调用的组件

**mutations**：拿到对应的事件类型和值，来对state进行处理。mutation对象里的方法不能有返回值，返回值拿不到数据，仅仅可以操作模块里面的数据

**store**：上面几个部分的管理者

### 白话vuex

vc（顾客）要点餐，点的菜名（dispatch派发的事件类型和数据），服务员（action）拿到了顾客点的菜之后就提交（commit）给后厨（mutations）后厨负责拿到顾客要的菜后做菜（拿到事件类型和值进行处理），之后做好的菜（state）给顾客（state数据给到对应的组件，会重新渲染组件）

### 代码实现（基础）

```js
//该文件用于创建Vuex中最为核心的store
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)
//准备actions——用于响应组件中的动作
const actions = {
    jiaOdd(context,value){
        console.log('actions中的jiaOdd被调用了')
        if(context.state.sum % 2){
            context.commit('JIA',value)
        }
    }
}
//准备mutations——用于操作数据（state）
const mutations = {
    JIA(state,value){
        console.log('mutations中的JIA被调用了')
        state.sum += value
    }
}
//准备state——用于存储数据
const state = {
    sum:0 //当前的和
}
//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state,
})
```

**注意：** 上面的`action`中的 `jiaOdd`中接收了一个`context`，这个对象身上带有`commit`、`dispatch`等方法，也有`state`可以在dispatch中做一些判断。但注意`context`对象并不是`store`实例

```js
export default {
        name:'Count',
        data() {
            return {
                n:1, //用户选择的数字
            }
        },
        methods: {
            increment(){
                this.$store.commit('JIA',this.n)
            },
            incrementOdd(){
                this.$store.dispatch('jiaOdd',this.n)
            }
        },
        mounted() {
            console.log('Count',this)
        },
    }
```

### store中的getter：

用于派生state数据，派生出来的数据可以做其他的操作

```js
getters: {
  // ...
  doneTodosCount (state, getters) {
    return this.$store.state.todos.filter(todo => todo.done).length
  }
}
```

接收state，有时候可以接收第二个参数`getter`（和上面的dispatch中的`context`对象中的dispatch方法差不多，可以继续使用对应的dispatch或者getter。

### mapState和mapGetter

mapState：将`store`中的`state`映射到组件的计算属性上。

```js
computed:{
  //靠程序员自己亲自去写计算属性
  sum(){
      return this.$store.state.sum
  }
  //借助mapState生成计算属性，从state中读取数据。（对象写法）
  // ...mapState({he:'sum'}),
  //借助mapState生成计算属性，从state中读取数据。（数组写法）
  ...mapState(['sum','school','subject']),
}
```

mapGetters和上面的用法一致

### mapMutations和mapActions

```js
methods: {
  //程序员亲自写方法
  increment(){
    this.$store.commit('JIA',this.n)
  },
  //借助mapMutations生成对应的方法，方法中会调用commit去联系mutation
  ...mapMutations({increment:'JIA',decrement:'JIAN'}),
  // ...mapActions(['jiaOdd','jiaWait'])
}
```

### vuex模块化、命名空间

单一的状态树会在项目中越来越大，越来越臃肿。所以可以将单一的store分割成多个store 每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割

```js
const moduleA = {
  namespaced:true,   // 将此模块注册成带有命名空间的模块，这样在下面的vue组件中使用才不会报错
  state:{
        sum:0, //当前的和
        school:'尚硅谷',
        subject:'前端',
    },
  mutations: { 
    JIA(state,value){
            console.log('mutations中的JIA被调用了')
            state.sum += value
        }
  },
  actions: { 
    jiaWait(context,value){
            console.log('actions中的jiaWait被调用了')
            setTimeout(()=>{
                context.commit('JIA',value)
            },500)
        }
  },
  getters:{
        bigSum(state){
            return state.sum*10
        }
    }
}
const moduleB = {
  namespaced:true,
  state: {
     personList:[{id:'001',name:'张三'}]
  },
  mutations: { ... },
  actions: { ... }
}
const store = createStore({
  modules: {
    countAbout: moduleA,
    personAbout: moduleB
  }
})
store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态

// vue组件中
<h1>当前求和为：{{sum}}</h1>
<h3>当前求和放大10倍为：{{bigSum}}</h3>
<h3>我在{{school}}，学习{{subject}}</h3>

computed: {
  // 直接将子store中的state映射出来就可以直接使用了，不需要用‘.’调用
   ...mapState('countAbout',['sum','school','subject']),
   ...mapState('personAbout',['personList']),
   ...mapMutations('countAbout',{increment:'JIA'}),
   ...mapActions('countAbout', {incrementWait:'jiaWait'})
},
methods: {
  add(){
    const personObj = {id:nanoid(),name:this.name}
    // 下面的写法是简写的形式
    this.$store.commit('personAbout/ADD_PERSON',personObj)
  }
}
```


