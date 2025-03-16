# 数据绑定

### 单向数据绑定`v-bind`：

数据只能从vue的data流向页面 并且加了冒号会将“双引号”中的数据按照js表达式来解析（比如`option`标签里面的`value`属性要为数字，如果不写`:`那就会按照字符串去解析，写了就会按照数字(js表达式)去解析

```HTML
单向数据绑定：<input type="text" v-bind:value="name"><br/>
简写形式：       <input type="text" :value="name"><br/>
```

#### 组件的数据绑定

给一个自定义组件传值，比如父子组件的通信

```Vue
<BlogPost
  :author="{
    name: 'Veronica',
    company: 'Veridian Dynamics'
  }"
/>

<!-- Dynamically assign to the value of a variable. -->
<BlogPost :author="post.author" />
```

要给一个组件传入多个值，这些值正好都是一个对象的属性，可以用`v-bind`不带参数的写法来向子组件传递这个对象（相当于展开这个对象，jsx中可以直接用`...`来展开，vue不行）

```Vue
const post = {
  id: 1,
  title: 'My Journey with Vue'
}

<BlogPost v-bind="post" />
// 等同于下面的写法
<BlogPost :id="post.id" :title="post.title" />
```

#### 修饰符

`.sync`：这个修饰符就是下方写法的简写形式

```Vue
<comp :foo.sync="bar"></comp>
// 上面的写法相当于下面的写法
<comp :foo="bar" @update:foo="val => bar = val"></comp>
```

```Vue
this.$emit('update:foo', newValue) // 在子组件必须显示触发 'update:foo' 事件
```

就是一种很常见的需求，子组件需要通过父组件注册在子组件上的事件去更新`$emit`父组件的值

为了方便不写`@update:foo="val => bar = val"`这么一坨，用这个`.sync`修饰符

#### 那和`v-model`有什么区别呢

`.sync`一般用到自定义组件身上，而`v-model`一般会做`value`属性（vue2）或者`modelValue`属性（vue3）的数据绑定，并添加`update`事件





### 双向数据绑定`v-model`：

数据能从页面流向data，反之亦然。 说明：

- 一般是input，其他表单等元素会使用。不能用于非输入类型的标签上 eg. h1标签等

```HTML
双向数据绑定：<input type="text" v-model:value="name"><br/>
简写形式：       <input type="text" v-model="name"><br/>
```

双向数据绑定注意点：

- 单选框需要制定value属性，才能让vue获取到正确的数据

- 多选框在data中收集的值要使用数组类型



#### 在自定义组件身上的双向数据绑定

这里会有误区，觉得这是破坏单向数据流的做法，话没错，但是直接去修改子组件的`props`在vue中是禁止的

但是按照常规的做法将`v-model`拆开成普通的`v-bind`绑定和触发特定的事件`update:modelValue`就不再是破坏单向数据流了

例子

```Vue
<CustomInput
  :modelValue="searchText"
  @update:modelValue="newValue => searchText = newValue"
/>
// 上面的写法就等效于下面的写法
<CustomInput v-model="searchText" />
```

但是在子组件中就不要直接去修改`searchText`，而是用下面的写法（Vue3）

```Vue
<script setup>
defineProps(['modelValue'])
defineEmits(['update:modelValue'])
</script>

<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```





#### 修饰符

`.trim`：去除输入的空格的，'    `a   bc`    ' => '`abc`'

`.number`：自动转换输入的数据为`float`，如果不能转换的数据会原模原样的输出

`.lazy`：`input`标签去掉`change`触发更新双向绑定的值

```Vue
<input type="text" v-model="testfoo" />
```

![image.png](数据绑定+64a651fd-d6fa-42aa-a7d4-aa9e8f4a2e2e/image.png)

如果加上`.lazy`

```Vue
<input type="text" v-model.lazy="testfoo" />
```

![image.png](数据绑定+64a651fd-d6fa-42aa-a7d4-aa9e8f4a2e2e/image 1.png)

只会在最后失焦或者回车，反正不是触发`change`事件的时候去更新值



