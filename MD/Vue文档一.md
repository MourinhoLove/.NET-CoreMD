# Vue文档一

[TOC]

## 一些基础的概念

### prop

>prop的作用是父组件单向传递数据给子组件。
>
>其实可以理解为暴露内部属性给别的组件，从而别的组件对他进行一些操作

<img src="https://s2.loli.net/2022/03/01/PcfyHrEz8WND5V4.png" style="zoom:200%;" />

这里展示prop的另一种写法

```javascript
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

定义了2个属性：

 + required

   如果为True 意味着父组件调用时必须传值、

 + type

   该属性的数据类型

### data

其结构如下

```vue
data() {
  return {
    key: value
  }
}
```

data里面包含我们所有组件的状态管理，也就是可以理解为我可以创建需要完成逻辑的所有变量。这里的data变量不暴露给其他组件。这是自己内部定义的。

### $emit

> 子组件传递给父组件的方法

`this.$emit("event-name", value)`

这里的name必须小写开头

value是需要传递的数据可以在父组件接收
