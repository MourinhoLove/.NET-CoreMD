# CSS学习经验

## 设置文字在div居中

### 1.单行居中

```css
text-align: center;
line-height:50px;
```

这里的line-height是元素的高度

### 2. FLEX BOX居中

```css
display: flex;
justify-content: center;
align-content: center;
```

## 设置Flex固定某个Item

```css
flex: 0 0 200px;
```

0 - 不拉长（flex-grow） 

0 - 不缩短（flex-shrink） 

200px - 开始于 200px （flex-basis)

## Javascript创建元素和Css的经验

```javascript
function tableCreate() {
  // table外面的Div
  let div = document.getElementById("maincontent");
  // 创建一个Table
  var tbl = document.createElement("table");
  tbl.style.tableLayout = 'fixed';
//   tbl.style.flexWrap = 'nowrap';
//   tbl.style.width = "100%";
  tbl.style.border = "1px solid #ccc";
  tbl.style.borderCollapse = "collapse";
  tbl.style.textAlign = "center";
  div.appendChild(tbl);
  for (var i = 0; i < 10; i++) {
    let tr = document.createElement("tr");
    tr.style.flexFlow = '0';
    tr.style.flexShrink = '0';
    tr.style.border = "1px solid #ccc";
    for (var j = 0; j < 15; j++) {
      let td = document.createElement("td");
      td.innerHTML = "无";
      td.style.width = "auto";
      td.style.fontSize = "8px";
      td.style.width = '100px';
      td.style.border = "1px solid #ccc";
      tr.appendChild(td);
    }
    // tr.style.backgroundColor ='red';
    tbl.appendChild(tr);
  }
  // 这是行数
  var rows = tbl.rows;
  // 这是最后一行
  var last = rows[0];
  // 最后一行第一个单元格
  var cell = last.cells[0];
  cell.style.backgroundColor = 'cornflowerblue'
  cell.style.color = 'white';
  cell.innerText = '测试';
  //   td.setAttribute('colspan', '0');
}
```

上面是一个创建表格的方法

## 设置边框包含在宽度高度内

```css
 box-sizing:border-box;
```

## 根据变量设置宽度

````javascript
div.style.width = 2*width + 'px'
````

## 在CSS里面创建通用变量

```css
:root {
  --cell-heght: 39px;
}
height: var(--cell-heght);
```

类似于全局变量 可以在多个样式中重复使用

## 设置边框

```css
  border-style: solid; /* 边框样式 */
  border-width: 1px; /* 边框宽度 */
  border-color: #0a6eb4; /* 边框颜色 */
  border-top: 0px; /* 取消上边框 */
  /* 同等于 */
  border:1px solid #0a6eb4;
```

## 设置元素中的最后一个样式

```css
.nav-box:last-child {
  border-style: solid;
  border-width: 1px;
  border-color: #0a6eb4;
  border-top: 0px;
}

```

## CSS选择器

```css
.nav div {

}
```

父与子的匹配方式

## 定位

有时候自带的布局对一些简单排列的元素可以很好的排版，可是面对各种不同的需求。你需要自己对元素进行定位

这个时候，就需要`position`属性了，可以让你根据坐标 比如xy把元素放到你想放到位置

### 实践

#### 1.根据父元素定位

要在父元素指定`position`属性，不然拟定位的时候可能会超过父元素的边界

```css
.parent {
    position: relative;
}
.child {
    position: absolute;
    top: 0;
}
```

#### 2.固定顶部导航栏

```css
 position: fixed; /* Set the navbar to fixed position */
 top: 0; /* Position the navbar at the top of the page */
 z-index: 1; /* 设置图层 */
```

这里还要记得 设置内容的margin-top。因为固定后内容会跟导航栏重叠

还有特别注意，如果滑动的时候内容还是跟导航栏重叠。需要你设置图层



## FlexBox布局

**容器属性**

### 1.flex-direction

定义了容器的排序方向

比如是横着排，还是竖着派

```css
.container {
    flex-direction: row | row-reverse | column | column-reverse;
}
```

***row***: 主轴为水平方向，起点在左端

***row-reverse***: 主轴为水平方向，起点在右端

***column***：主轴为垂直方向，起点在上沿

***column-reverse***: 主轴为垂直方向，起点在下沿

### 2.flex-wrap:

默认情况下，项目都排在主轴线上，使用 flex-wrap 可实现项目的换行。

```css
.container {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```

### 3.justify-content

定义了项目在主轴的对齐方式

```css
.container {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

### 4.align-items

定义了项目在交叉轴上的对齐方式

```css
.container {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

默认值为 stretch 即如果项目未设置高度或者设为 auto，将占满整个容器的高度。



### 5.align-content

定义了多根轴线的对齐方式，如果项目只有一根轴线，那么该属性将不起作用

```css
.container {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

>当你 flex-wrap 设置为 nowrap 的时候，容器仅存在一根轴线，因为项目不会换行，就不会产生多条轴线。
>
>当你 flex-wrap 设置为 wrap 的时候，容器可能会出现多条轴线，这时候你就需要去设置多条轴线之间的对齐方式了。



Item属性

>1. order
>2. flex-basis
>3. flex-grow
>4. flex-shrink
>5. flex
>6. align-self

### 1.order

定义项目在容器中的排列顺序，数值越小，排列越靠前，默认值为 0

```css
.item {
    order: <integer>;
}
```

### 2.flex-basis

定义了在分配多余空间之前，项目占据的主轴空间，浏览器根据这个属性，计算主轴是否有多余空间

```css
.item {
    flex-basis: <length> | auto;
}
```

默认值：auto，即项目本来的大小, 这时候 item 的宽高取决于 width 或 height 的值。

### 3.flex-grow

```css
.item {
    flex-grow: <number>;
}
```

默认值为 0，即如果存在剩余空间，也不放大

>当所有的项目都以 flex-basis 的值进行排列后，仍有剩余空间，那么这时候 flex-grow 就会发挥作用了。
>
>如果所有项目的 flex-grow 属性都为 1，则它们将等分剩余空间。(如果有的话)
>
>如果一个项目的 flex-grow 属性为 2，其他项目都为 1，则前者占据的剩余空间将比其他项多一倍。
>
>当然如果当所有项目以 flex-basis 的值排列完后发现空间不够了，且 flex-wrap：nowrap 时，此时 flex-grow 则不起作用了，这时候就需要接下来的这个属性

### 4.flex-shrink

```css
.item {
    flex-shrink: <number>;
}
```

默认值: 1，即如果空间不足，该项目将缩小，负值对该属性无效。

>如果所有项目的 flex-shrink 属性都为 1，当空间不足时，都将等比例缩小。
>
>如果一个项目的 flex-shrink 属性为 0，其他项目都为 1，则空间不足时，前者不缩小。

### 高度

默认里面的item没有设置的话 高度是容器的高度



## 拖拽

```javascript
/**
 * 当元素被拖动的时候执行
 */
function ondragstart(e) {
  e.dataTransfer.setData("Text", e.target.id);
  console.log("开始拖动啦");
  this.style.opacity = "0.4";
}
/**
 * 当元素被拖动的时候一直执行
 */
function ondrag(e) {
  // console.log("拖动中");
}
/**
 * 当元素拖动结束的时候执行
 */
function ondragend(e) {
  console.log("拖动结束");
  this.style.opacity = "1";
}

function ondragenter(e) {
  e.preventDefault();
  console.log("handle_enter-进入目的地");
}
function ondragover(e) {
  e.preventDefault();
  let returnObj = e.dataTransfer.getData("Text");
  console.log(returnObj + "-handle_over-在目的地范围内");
}
function ondragleave(e) {
  e.preventDefault();
  console.log("handle_leave-当元素离开目的地时触发");
  // 阻止浏览器默认行为
}
function ondrop(e) {
  e.stopPropagation();
  let returnObj = e.dataTransfer.getData('Text')
  if (returnObj) {
    e.target.append(document.getElementById(returnObj))
  }
  console.log(returnObj + '-handle_drop-在目的地区释放')
}
```



