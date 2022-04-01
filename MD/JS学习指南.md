# JS学习指南

## 数据类型

### 1.变量

>```javascript
>const x = 1
>let y = 5
>```
>
>const 是常量
>
>let 是变量
>
>var在JS里不推荐使用了

### 2.数组

```javascript
const t = [1,-1,3]
t.push(4)
t.forEach(value => {
  console.log(value)  
})
```



## 得到时间

```javascript
    // 得到开始时间
    let startDay = new Date(element.startDate).getDate();
    // 得到结束时间
    let endDay = new Date(element.endDate).getDate();
```

