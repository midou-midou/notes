# 过滤器

```HTML
<!-- 过滤器实现 -->
<h3>现在是：{{time | timeFormater}}</h3>
<!-- 过滤器实现（传参） -->
<h3>现在是：{{time | timeFormater('YYYY_MM_DD') | mySlice}}</h3>
<h3 :x="msg | mySlice">尚硅谷</h3>

<script>
    new Vue({
        el:'#root',
        data:{
            time:1621561377603, //时间戳
            msg:'你好，尚硅谷'
        },
        //局部过滤器
        filters:{
            timeFormater(value,str='YYYY年MM月DD日 HH:mm:ss'){
                return dayjs(value).format(str)
            }
        }
        })
</script>
```

### 上面代码line4的一个执行过程

`time`从data中拿到响应的值，之后将值传递给第一个过滤器`timeFormater`，timeFormater的返回值传递给第二个过滤器`mySlice`，这个过程称之为过滤器的串联

### 定义

对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）

### 语法

- 注册过滤器：`Vue.filter(name,callback)`或 `new Vue{filters:{}}`

- 使用过滤器：`{{ xxx | 过滤器名}}` 或 `v-bind:属性 = "xxx | 过滤器名"`

### 注意

- 过滤器也可以接收额外参数、多个过滤器也可以串联

- 并没有改变原本的数据, 是产生新的对应的数据

- vue3中已经放弃此写法

