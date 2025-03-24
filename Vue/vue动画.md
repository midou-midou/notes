# vue动画

`transition标签`：要实现动画的标签外层要包裹的标签

`transition-group标签`：多个标签都要有动画，且是相同的动画。或者是标签的动画互斥播放 `xxx-enter/leave-active`css样式：下面的动画和transition执行激活的属性

### css3动画

```HTML
<template>
<div>
  <button @click="isShow = !isShow">显示/隐藏</button>
  <transition name="hello" appear>
    <h1 v-show="isShow">你好啊！</h1>
  </transition>
  </div>
</template>

<script>
  export default {
    name:'Test',
    data() {
      return {
        isShow:true
      }
    },
  }
</script>

<style scoped>
  h1{
    background-color: orange;
  }

  .hello-enter-active{
    animation: atguigu 0.5s linear;
  }

  .hello-leave-active{
    animation: atguigu 0.5s linear reverse;
  }

  @keyframes atguigu {
    from{
      transform: translateX(-100%);
    }
    to{
      transform: translateX(0px);
    }
  }
</style>
```

appear属性，代表标签上来要消失

### 过渡css transition

```HTML
<template>
    <div>
        <button @click="isShow = !isShow">显示/隐藏</button>
        <transition-group name="hello" appear>
            <h1 v-show="!isShow" key="1">你好啊！</h1>
            <h1 v-show="isShow" key="2">尚硅谷！</h1>
        </transition-group>
    </div>
</template>

<script>
    export default {
        name:'Test',
        data() {
            return {
                isShow:true
            }
        },
    }
</script>

<style scoped>
    h1{
        background-color: orange;
    }
    /* 进入的起点、离开的终点 */
    .hello-enter,.hello-leave-to{
        transform: translateX(-100%);
    }
    .hello-enter-active,.hello-leave-active{
        transition: 0.5s linear;
    }
    /* 进入的终点、离开的起点 */
    .hello-enter-to,.hello-leave{
        transform: translateX(0);
    }

</style>
```

### 总结

元素进入的样式：

1. v-enter：进入的起点

2. v-enter-active：进入过程中

3. v-enter-to：进入的终点

元素离开的样式：

1. v-leave：离开的起点

2. v-leave-active：离开过程中

3. v-leave-to：离开的终点

