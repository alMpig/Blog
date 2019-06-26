# VUE核心技术-尚硅谷

[bilibili](https://www.bilibili.com/video/av49099807/?p=45)

## 前置储备知识

1. Array.prototype.slice.call(lis)   `将伪数组转换为真数组 `
2. node.nodeType   `得到节点类型 `

```javascript
//slice循环返回一个新的数组
//call把this指向其他
//instanceof运算符用于测试构造函数的prototype属性是否出现在对象的原型链中的任何位置[MDN]
const list = document.body.querySelectorAll('li')
const list2 = Array.prototype.slice.call(list)
console.log(list2 instanceof Array)
```

3. Object.defineProperty(obj, propName, {})   `给对象添加/修改属性(指定描述符) `

   * 该属性是ES5的语法，是Vue的核心，故Vue不支持 IE8 及以下浏览器 

   > 属性描述符

   1. 数据描述符
      * configurable：是否可以重新定义  (false后，重新定义了也无效)
      * enumerable ：是否可以枚举  (类似于循环) 默认为true
      * value：初始值 
      * writable：是否可以修改属性值  (修改会报错)
   2. 访问描述符
      * get：回调函数，根据其它相关的属性动态计算得到当前属性值
      * set：回调函数，监视当前属性值的变化，更新其他相关属性

```javascript
var obj = {
  firstName:'A',
  lastName:'B'
}
Object.defineProperty(obj,'fullName',{
  get () {
    return this.firstName + '-' + this.lastName
  },
  set (value) {
    const names = value.split('-')
    this.firstName = names[0]
    this.lastName  = names[1]
  }
})
console.log(obj.fullName) //A-B
obj.firstName = 'C'
obj.lastName = 'D'
console.log(obj.fullName) //C-D
obj.fullName = 'E-F'
console.log(obj.firstName,obj.lastName) //E F
```

4. Object.keys(obj)  `得到对象自身可枚举的属性名的数组` 

```javascript
const names = Object.keys(obj)
console.log(names) //["firstName", "lastName"]
------------------------------------------------
//当 obj 设置 enumerable : true 
const names = Object.keys(obj)
console.log(names) //["firstName", "lastName", "fullName"]
```

5. obj.hasOwnProperty('prop')  `判断 prop 是否是 obj 自身的属性 `

```javascript
console.log(obj.hasOwnProperty('fullName'),obj.hasOwnProperty('toString')) //true false
```

6. DocumentFragment  `文档碎片(高效批量更新多个节点) `

> fragment只在内存中，更新操作，操作完了再一键更新dom

* 本来需要多次更新dom，现在只需要一次，提高了效率

```javascript
/*
	<ul>
		<li>abc<li>
		<li>def<li>
		<li>hij<li>
	</ul>
*/
const ul = document.getElementById('ul')
//1. 创建fragment
const fragment = document.createDocumentFragment()
//2. 取出ul中所有的子节点保存到fragment
let child
while(child=ul.firstChild){ //一个节点只有一个父亲
      fragment.appendChild(child) //先将child从ul中移除，添加为fragment子节点
}
//3. 更新fragment中所有的li的文本
Array.prototype.slice.call(fragment.childNodes).forEach(node => {
  if(node.nodeType===1){ // 元素节点 <li>
     node.textContent = 'atguigu'
  }
})
//4. 将fragment插入到ul中
ul.appendChild(fragment)
```

## 数据代理

> 通过一个对象代理对另一个对象(在前一个对象内部)中属性的操作(读/写) 

