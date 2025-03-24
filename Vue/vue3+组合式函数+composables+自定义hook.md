# vue3 组合式函数 composables 自定义hook

## 组合式函数：

vue3中有状态逻辑的函数（主要用于逻辑复用、代码组织、鲁棒性）类似于一个纯逻辑的组件，没有模版`template`标签

## 实例：

鼠标实时坐标示例

```JavaScript
import { ref, onMounted, onUnmounted } from 'vue'

// 按照惯例，组合式函数名以“use”开头
export function useMouse() {
  // 被组合式函数封装和管理的状态
  const x = ref(0)
  const y = ref(0)

  // 组合式函数可以随时更改其状态。
  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }

  // 一个组合式函数也可以挂靠在所属组件的生命周期上
  // 来启动和卸载副作用
  onMounted(() => window.addEventListener('mousemove', update))
  onUnmounted(() => window.removeEventListener('mousemove', update))

  // 通过返回值暴露所管理的状态
  return { x, y }
}
```

在其他的组件中使用：

```HTML
<script setup>
import { useMouse } from './mouse.js'

const { x, y } = useMouse()
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```

在组合式函数中，可以使用vue的生命周期钩子，可以让组合式函数挂靠到对应组件的生命周期钩子上面去执行。

### 带有参数的自定义hook实例：

如果需要在自定义hook中接收一个参数，直接在`useXXX(args)`写入指定的参数。之后就可以在自定义hook中处理了

```JavaScript
import { ref, isRef, unref, watchEffect } from 'vue'

export function useFetch(url) {
  const data = ref(null)
  const error = ref(null)

  function doFetch() {
    // 在请求之前重设状态...
    data.value = null
    error.value = null
    // unref() 解包可能为 ref 的值
    fetch(unref(url))
      .then((res) => res.json())
      .then((json) => (data.value = json))
      .catch((err) => (error.value = err))
  }

  if (isRef(url)) {
    // 若输入的 URL 是一个 ref，那么启动一个响应式的请求
    watchEffect(doFetch)
  } else {
    // 否则只请求一次
    // 避免监听器的额外开销
    doFetch()
  }

  return { data, error }
}
```

**接收参数**：接收的参数可以是一个`ref`响应式的值，也可以是一个普通的值。里面对响应式的值使用`unref`做了处理（响应式值只返回value，普通值原样返回）

**返回值：**返回值是一个普通的对象，不是响应式对象。可以在使用自定义hook的时候解构，如果使用了响应式的值使用了解构就会破坏原有值的响应式

