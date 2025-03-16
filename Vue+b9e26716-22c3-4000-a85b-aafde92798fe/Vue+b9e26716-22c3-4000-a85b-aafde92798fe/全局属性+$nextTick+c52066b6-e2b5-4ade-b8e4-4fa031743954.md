数据改变后，在DOM重新渲染后执行传入其中的回调函数

本质上，是在程序的执行顺序上做了文章。

下面例子的`handleEdit()`方法里面的所有语句都会在**本次tick中执行**，本次tick改变了`data`里面的数据，那么vue会在**下一个tick执行用新数据去渲染DOM等操作**。

所以，这个`$nextTick`里面的回调函数就会在下一个tick的DOM渲染到页面后执行 这个Tick很像游戏引擎中的Tick的概念，都是代表一个时间片

```JavaScript
//编辑
handleEdit(todo){
  if(todo.hasOwnProperty('isEdit')){
    todo.isEdit = true
  }else{
    // console.log('@')
    this.$set(todo,'isEdit',true)
  }
  this.$nextTick(function(){
    this.$refs.inputTitle.focus()
  })
}
```



