## Vue指令语法

### 语法

- **v-bind**

  表达式 `v-bind:attribute="property"`

  attribute: 节点的属性比如title

  property: 实列的创建的变量

  > 将这个元素节点的 `attribute` attribute 和 Vue 实例的 `property` 

- **v-for**

  表达式 ` v-for= "value in array"`

  value: 数组里的对象

  array: 循环的数组

  > 这里记得还需要实现一下 `v-bind:key="value.id"`，方便vue记录索引更新Dom
  >
  > 循环一个元素，用于创建列表的多项数据

- **v-model**

  表达式

  ```
  v-model= "model"
  ```

  model: 用户定义的数据`

  >用于双向绑定。比如表单的输入`  <input v-model="value">`这里绑定了value，当用户输入内容后，value的内容会随之改变

- **v-on**

  表达式 

  ```vue
  全写:v-on:event="method"
  简写:@event="method"
  ```

  event：事件名称 比如click

  method：定义好的方法

  >绑定各种事件