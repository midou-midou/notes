# vue插槽（注意不是插件）

`<slot></slot>` 插槽，可以在组件标签中插入其他的标签。默认情况下的组件标签是自闭和的，但是vue中可以写带有闭合标签的组件

**注意**：下面的两个属性`slot`和`slot-scope`已经被废弃**(vue3)**但是没有被移除，所以还是可以使用这两个属性

### 默认插槽：

```html
父组件中：
<Category>
  <div>html结构1</div>
</Category>

子组件中：
<template>
  <div>
    <!-- 定义插槽 -->
    <slot>插槽默认内容...</slot>
  </div>
</template>
```

父组件中的`Category`标签里面的`div`就会被加入到子组件中的`slot`标签中

### 具名插槽

给`slot`一个属性“name” `<slot name="xxx"></slot>`

```html
父组件中：
<Category>
  <!-- 老写法 -->
  <template slot="center">
    <div>html结构1</div>
  </template>

  <!-- 新写法 -->
  <template v-slot:footer>
    <div>html结构2</div>
  </template>

  <!-- 新写法 缩写形式 -->
  <template #footer>
    <div>html结构2</div>
  </template>
</Category>

子组件中：
<template>
  <div>
    <!-- 定义插槽 -->
    <slot name="center">插槽默认内容...</slot>
    <slot name="footer">插槽默认内容...</slot>
  </div>
</template>
```

PS. 上面的写法中出现了占位标签`<template>`，如果要当成传入具名插槽的结构，要添加属性`v-slot:footer`（这是新的写法）老的写法看上面的`div` `v-slot`属性可以缩写，使用`#`来书写

### 作用域插槽：

可以往`slot`标签中传递数据，按照属性的方式传递。传递的数据会给**插槽的使用者**用。官方称：**插槽 prop**

```html
父组件中：
<Category>
  <template scope="scopeData">
    <!-- 生成的是ul列表 -->
    <ul>
      <li v-for="g in scopeData.games" :key="g">{{g}}</li>
    </ul>
  </template>
</Category>
<Category>
  <template slot-scope="scopeData">
    <!-- 生成的是h4标题 -->
    <h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
  </template>
</Category>
子组件中：
<template>
  <div>
    <slot :games="games"></slot>
  </div>
</template>
<script>
  export default {
    name:'Category',
    props:['title'],
    //数据在子组件自身
    data() {
      return {
        games:['红色警戒','穿越火线','劲舞团','超级玛丽']
      }
    },
  }
</script>
```

PS. 作用域插槽必须要使用`template`标签来接收数据，接收数据用属性`scope`或者`slot-scope`。组件之间的数据流可以通过这个特性可以逆方向传递。
