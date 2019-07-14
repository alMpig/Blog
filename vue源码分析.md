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

1. 数据代理: 通过一个对象代理对另一个对象(在前一个对象内部)中属性的操作(读/写) 
2. vue 数据代理: 通过 vm 对象来代理 data 对象中所有属性的操作 
3. 好处: 更方便的操作 data 中的数据 
4. 基本实现流程 
   1. 通过 Object.defineProperty()给 vm 添加与 data 对象的属性对应的属性描述符 
   2. 所有添加的属性都包含 getter/setter 
   3. getter/setter 内部去操作 data 中对应的属性数据 

```javascript
//相当于Vue的构造函数
function MVVM(options) {
    //将配置对象保存到vm
    this.$options = options || {};
  	//将data对象保存到变量vm和变量data中
    var data = this._data = this.$options.data;
    //保存vm到变量me
    var me = this;
    //遍历data中所有的属性
    Object.keys(data).forEach(function(key) { //key是data中的某一个属性名
        //对指定的的属性实现代理
        me._proxyData(key);
    });
}
MVVM.prototype = {
  //实现指定属性代理的方法  vm.xxx -> vm._data.xxx
  _proxyData: function(key) {
    		//保存vm到变量me
        var me = this;
    	  Object.defineProperty(me, key, {
            configurable: false,//不能重新定义
            enumerable: true,//可以枚举遍历
          	//当通过vm.xxx读取属性值的时候调用，从data中获取相应的属性值返回。代理读操作
            get: function proxyGetter() {
                return me._data[key];
            },
          	//当通过vm.xxx = value时，value被保存到data中对应的属性上。代理写操作
            set: function proxySetter(newVal) {
                me._data[key] = newVal;
            }
        });
  }
}
var vm = new MVVM({
    data: {
      someStr: 'hello '
    }
})
```

## 模版解析

### 模板解析的基本流程

1. 将 el 的所有子节点取出, 添加到一个新建的文档 fragment 对象中 
2. 对 fragment 中的所有层次子节点递归进行编译解析处理 
   * 对大括号表达式文本节点进行解析 
   *  对元素节点的指令属性进行解析 
     * 事件指令解析 
     * 一般指令解析 
3. 将解析后的 fragment 添加到 el 中显示

### 模板解析**(1):** 大括号表达式解析 

1. 根据正则对象得到匹配出的表达式字符串: 子匹配/RegExp.$1 name 
2. 从 data 中取出表达式对应的属性值 
3. 将属性值设置为文本节点的 textContent 

### **模板解析**(2): 事件指令解析 

1. 从指令名中取出事件名 
2. 根据指令的值(表达式)从 methods 中得到对应的事件处理函数对象 
3. 给当前元素节点绑定指定事件名和回调函数的 dom 事件监听 
4. 指令解析完后, 移除此指令属性 

### **模板解析**(3): 一般指令解析 

1. 得到指令名和指令值(表达式) text/html/class msg/myClass 
2. 从 data 中根据表达式得到对应的值 
3. 根据指令名确定需要操作元素节点的什么属性 
   * v-text---textContent 属性
   * v-html---innerHTML 属性
   * v-class--className 属性 
4. 将得到的表达式的值设置到对应的属性上 
5. 移除元素的指令属性 

## 数据绑定

> 一旦更新了 data 中的某个属性数据, 所有界面上直接使用或间接使用了此属性的节点都会
> 更新

### 数据劫持 

1. 数据劫持是 vue 中用来实现数据绑定的一种技术 

2. 基本思想: 通过 defineProperty()来监视 data 中所有属性(任意层次)数据的变化, 一旦变 

   化就去更新界面 

### 四个重要对象

#### Observer 

1. 用来对 data 所有属性数据进行劫持的构造函数 
2. 给 data 中所有属性重新定义属性描述(get/set) 
3. 为 data 中的每个属性创建对应的 dep 对象 

#### Dep(Depend) 

1. data 中的每个属性(所有层次)都对应一个 dep 对象 

2. 创建的时机:

   * 在初始化 define data 中各个属性时创建对应的 dep 对象 
   * 在 data 中的某个属性值被设置为新的对象时 

3. 对象的结构 

   {
    id, // 每个 dep 都有一个唯一的 id
    subs //包含 n 个对应 watcher 的数组(subscribes 的简写) 

   } 

4. subs 属性说明 

   * 当 watcher 被创建时, 内部将当前 watcher 对象添加到对应的 dep 对象的 subs 中 
   * 当此 data 属性的值发生改变时, subs 中所有的 watcher 都会收到更新的通知, 从而最终更新对应的界面

#### Compiler 

1. 用来解析模板页面的对象的构造函数(一个实例) 

2. 利用 compile 对象解析模板页面 

3. 每解析一个表达式(非事件指令)都会创建一个对应的 watcher 对象, 

   与 dep 的关系 

4. complie 与 watcher 关系: 一对多的关系 

#### Watcher 

1. 模板中每个非事件指令或表达式都对应一个 watcher 对象 

2. 监视当前表达式数据的变化 

3. 创建的时机: 在初始化编译模板时 

4. 对象的组成 { 

   vm, //vm 对象
    exp, //对应指令的表达式
    cb, //当表达式所对应的数据发生改变的回调函数
    value, //表达式当前的值
    depIds //表达式中各级属性所对应的 dep 对象的集合对象 

   //属性名为 dep 的 id, 属性值为 dep 

   } 

5. 总结: dep 与 watcher 的关系: 多对多 

   1. data 中的一个属性对应一个 dep, 一个 dep 中可能包含多个 watcher(模板中有几个 

      表达式使用到了同一个属性) 

   2. 模板中一个非事件表达式对应一个 watcher, 一个 watcher 中可能包含多个 dep(表 

      达式是多层: a.b) 

   3. 数据绑定使用到 2 个核心技术 

      \* defineProperty() * 消息订阅与发布 

## **MVVM** 原理图分析

![image-20190702154114357](/Users/page/Library/Application Support/typora-user-images/image-20190702154114357.png)

### 双向数据绑定

1. 双向数据绑定是建立在单向数据绑定(model==>View)的基础之上的 

2. 双向数据绑定的实现流程:
   a. 在解析v-model指令时,给当前元素添加input监听 

   b. 当 input 的 value 发生改变时, 将最新的值赋值给当前表达式所对应的 data 属性 

# Vue.js 深入浅出

> vue.js 是渐进式框架

## 变化侦测

### object的变化侦测

Object.define