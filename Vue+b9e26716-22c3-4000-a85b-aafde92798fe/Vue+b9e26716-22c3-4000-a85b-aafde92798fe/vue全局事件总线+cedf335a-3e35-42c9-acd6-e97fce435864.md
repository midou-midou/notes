# vue全局事件总线

1. 一种组件间通信的方式，适用于任意组件间通信。

2. 安装全局事件总线：

```JavaScript
new Vue({
	beforeCreate() {
		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
	},
})
```

1. 使用事件总线：

接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的回调留在A组件自身。

```JavaScript
methods(){
  demo(data){}
}
mounted() {
  this.$bus.$on('xxxx',this.demo)
}
```

```Plain Text
    提供数据：`this.$bus.$emit('xxxx',数据)`
```

1. 最好在beforeDestroy钩子中，用$off去解绑当前组件所用到的事件。

