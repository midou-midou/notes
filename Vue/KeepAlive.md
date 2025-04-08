缓存组件，可以缓存被此标签包裹的组件

```Vue
<KeepAlive>
  <MyComponent />
</KeepAlive>
```

缓存这个组件，这个组件的状态，可以在这个组件被移除出`DOM`树后保证内部的状态都还在，而不是这个组件第二次插入到`DOM`树中要重置这个组件身上的所有状态

### Props属性

`<keep-alive>`组件有下面的一些属性

```TypeScript
interface KeepAliveProps {
  /**
   * 让其缓存的，要包括那些缓存的组件，且组件有name属性指定了组件的名字
   */
  include?: MatchPattern
  /**
   * 哪些组件不缓存
   */
  exclude?: MatchPattern
  /**
   * 最大缓存几个组件，如果超出了最大缓存的组件，会按照LRU(Least recently use)最新最少使用的原则，将最近没有使用过的，没有再此插入到DOM树的组件移除缓存，给新的缓存腾位置
   */
  max?: number | string
}

// 可以填写一个正则，或者comma-delimit，或者一个数组
type MatchPattern = string | RegExp | (string | RegExp)[]
```

### 生命周期的变化

在`KeepAlive`包裹住的组件中，或者是在`include`属性中的，`onMounted`和`onUnmounted`这俩函数会被`onActivated`和`onDeactivated`替代掉

毕竟，缓存了组件，就不会被真正的卸载，但是注意，挂载的生命周期函数还是会触发的，毕竟这个组件还是要第一次插入到`DOM`树中去，会在第一次触发`onMounted`
