# Blog
The warehouse was created at 2019-5-30 PM

[哔哩哔哩es6](https://www.bilibili.com/video/av47304735?from=search&seid=725075707626700437)

# let、const

> 不允许在相同作用域内，重复声明同一个变量

```javascript
let count = 10 //可改变值
const key = 'abc132' //不可改变值
const person = { //可改变对象上的属性值,不可以直接给对象重新赋值
    name:'almpig'
}
```

> 扩展

```javascript
//不许改变对象的属性值[es5]
const jelly = Object.freeze(person)
//变量一定要在声明之后使用，否则就报错
var =  "输出undefined" //变量提升
let、const = "报错ReferenceError"//不存在变量提升
//const一旦声明变量，就必须立即初始化
```



