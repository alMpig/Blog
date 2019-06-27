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

```javascript
进度：https://www.bilibili.com/video/av49099807/?p=54
```

