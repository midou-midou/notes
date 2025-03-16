# 样式绑定

UI：

```HTML
<div id="root">
  <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
  <div class="basic" :class="mood" @click="changeMood">{{name}}</div> <br/><br/>

  <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
  <div class="basic" :class="classArr">{{name}}</div> <br/><br/>

  <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
  <div class="basic" :class="classObj">{{name}}</div> <br/><br/>

  <!-- 绑定style样式--对象写法 -->
  <div class="basic" :style="styleObj">{{name}}</div> <br/><br/>
  <!-- 绑定style样式--数组写法 -->
  <div class="basic" :style="styleArr">{{name}}</div>
</div>
```

样式对象：

```HTML
<script type="text/javascript">
        const vm = new Vue({
            el:'#root',
            data:{
                name:'尚硅谷',
                mood:'normal',
                classArr:['atguigu1','atguigu2','atguigu3'],
                classObj:{
                    atguigu1:false,
                    atguigu2:false,
                },
                styleObj:{
                    fontSize: '40px',
                    color:'red',
                },
                styleObj2:{
                    backgroundColor:'orange'
                },
                styleArr:[
                    {
                        fontSize: '40px',
                        color:'blue',
                    },
                    {
                        backgroundColor:'gray'
                    }
                ]
            },
            methods: {
                changeMood(){
                    const arr = ['happy','sad','normal']
                    const index = Math.floor(Math.random()*3)
                    this.mood = arr[index]
                }
            },
        })
    </script>
```

### Class绑定：

写法：`:class="xxx"`xxx可以是对象，数组，字符串

- 字符串写法适用于：类名不确定，要动态获取

- 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定

- 数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用

### style绑定：

写法：

- `:style="{fontSize: xxx}"`其中xxx是动态值

- `:style="[a,b]"`其中a、b是样式对象

