# ECMAScript ES6

The warehouse was created at 2019-5-30 PM

[哔哩哔哩es6](https://www.bilibili.com/video/av47304735?from=search&seid=725075707626700437)

## let、const

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
console.log(var) =  "输出undefined" //变量提升
console.log(let、const) = "报错ReferenceError"//不存在变量提升
//const一旦声明变量，就必须立即初始化
```

## 箭头函数

> 箭头函数都是匿名函数  ||  函数参数默认值

```shell
(a=2) => {}  //this的值会相应的变化
```

## 模板字符串

> 可以写HTML结构  ||  可以嵌套多个模板字符串  ||  可以写函数  || 可以写 三元表达式

```javascript
const jelly = {...}
function temp(){
    return{
        `<ul>
			${jelly.todos.map(todo => `<li>${todo.name}</li>`).join('')}
	     </ul>`
    }
}
//一般把一整个模块封装起来,然后直接再另一个模板字符串里直接调用
const dom = `<div>${temp(jelly)}</div>`
```

## 新增字符串函数

* startsWith()     || 判断是以什么开头  -  大小写敏感
* endsWith()       || 判断是以什么结尾
* includes()        || 判断字符串里有没有
* repeat()         ||重复字符串N次

```javascript
const id = '654x';
id.startsWith('6'); //true
id.startsWith('4',2); //true
id.endsWith('x'); //true
id.includes('4',1); //true  判断地1位后面有没有
id.repeat(3) //654x654x654x
```

## 解构赋值

> 重新命名 | 设置默认值 | 剩余参数

```javascript
const Tom = {age:6,name:'tom'};
const arr = ['one','tow','san'];
const {age:newAge,name,num = '100'} = Tom;
const [one,,san] = arr;
const [one,...age] = arr;
const a = 10;
const b = 20;
[a,b] = [b,a];
console.log(a,b);//20,10
```

## for of循环

> 不支持对象循环  ||  支持数组 、字符串、NodeList

```javascript
const data = ['one','tow']
for(let dom of data){
    console.log(dom);//'one'
}
//扩展
for(let [idx,fru] of data.entries()){
    console.log(idx,fru);//[0,'one']
}
```

## 数组新增的方法

### 不在Array的原型上

* Array.from()   ||  转化成真正的数组  || NodeList、字符串
* Array.of()   || 确保数组返回的结果一致性 

```javascript
const toArr = Array.from(NodeList,todo => todo.textContent);//第二个参数相当于调用了map方法
//const names = toArr.map(todo => todo.textContent);
const sum = 'abc'
console.log(Array.from(sum))
console.log(new Array(1)) // [undefined x 1]
console.log(new Array(1,2)) // [1,2]
console.log(Array.of(1)) // [1]
```

### 是在Array的原型上

* find()                   ||   返回数组中某一项的元素 , 不会继续执行后面的
* findIndex()         ||   返回数组中某一项的索引 , 不会继续执行后面的
* some()                ||   至少有一项满足就返回 , 不会继续执行后面的
* every()                ||   有一项不满足就返回 , 不会继续执行后面的

```javascript
const ins = [{name:"one",age:0},{name:"tow",age:1}]
const ban = ins.find(fruit => fruit.name === 'one') // {name:"one"}
const banIndex = ins.find(fruit => fruit.name === 'one') // 1
const isEno = ins.some(fruit => fruit.age > 0 ) // true
const isAll = ins.some(fruit => fruit.age > 0 ) // false
```

## 剩余参数

> 把所有剩余的参数,都以一个数组的方式储存

```javascript
function sum(rete,...objName){
    console.log(objName)//[1,2,3]
}
sum('obj',1,2,3)
const pla = ['tom',60,5,6,7]
const [name,id,...sum] = pla //'tom',60,[5,6,7]
```

## 扩展运算符

```javascript
const you = ['a','b','c']
const me = ['f','g','h']
const met = [...you,'e',...me]
const cunt = [...met] //改变cunt中的某一项不会对met有影响
const sri = 'abc'
console.log([...sri])//['a','b','c']
```

## 字面量

```javascript
const name = 'tom';
const keys = ['one','tow'];
const vals = [1,2];
const id = 1;
const tom = {
    name,
    age:18,
    fun(){}, //方法的简写
    [keys.shift()]:vals.shift(),
    [`'user-'${++id}`]:id,
    [`'user-'${++id}`]:id
}
```

# [进度](<https://www.bilibili.com/video/av47304735/?p=26>)

