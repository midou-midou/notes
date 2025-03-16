# React拓展

### 组件优化

组件的两个问题

- 只要执行 `setState()`,即使不改变状态数据, 组件也会重新 `render()`

- 只当前组件重新 `render()`, 就会自动重新render子组件 ==> 效率低

效率高的做法： 只有当组件的state或props数据发生改变时才重新render

**出现component重复渲染的问题：** Component中的`shouldComponentUpdate()`总是返回true

**解决方法：**

- 办法1:

重写 `shouldComponentUpdate()`方法 比较新旧state或props数据, 如果有变化才返回true, 如果没有返回false

- 办法2:

使用 `PureComponent`

`PureComponent`重写了 `shouldComponentUpdate()`, 只有state或props数据有变化才返回true

**注意:**

- 只是进行state和props数据的浅比较, 如果只是数据对象内部数据变了, 返回false

- 不要直接修改state数据, 而是要产生新数据

- 项目中一般使用 `PureComponent`来优化

### Render props

问题：**如何向组件内部动态传入带内容的结构(标签)?**

- 使用children props: 通过组件标签体传入结构

- 使用render props: 通过组件标签属性传入结构, 一般用render函数属性

**children props**

```JavaScript
<A>
    <B >xxxx</B>
</A>
{this.props.children}
```

**render props**

```JavaScript
<A render={(data) => <C data={data}></C>}></A>
```

A组件:  `{this.props.render(内部state数据)}` C组件: 读取A组件传入的数据显示  `{this.props.data}`

