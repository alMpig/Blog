
# vue 

## MVC/MVVM概念

> MVC

MVC是三个单词的首字母缩写，它们是Model（模型）、View（视图）和Controller（控制）。

* 最上面的一层，是直接面向最终用户的"视图层"（View）。它是提供给用户的操作界面，是程序的外壳。

* 最底下的一层，是核心的"数据层"（Model），也就是程序需要操作的数据或信息。

* 中间的一层，就是"控制层"（Controller），它负责根据用户从"视图层"输入的指令，选取"数据层"中的数据，然后对其进行相应的操作，产生最终结果。

> MVVM

MVVM是前段视图层的分层开发思想，主要把每个页面分成了M、V和VM其中，VM是MVMM思想的核心：因为VM是M和V之间的调度者

* M：数据

* VM：调度者

* V：HTML结构

## 基本语法

> 简单入门

* `{{ x }}` : 插值表达式，写在内容里只会替换当前{{ x }},不会清空
* `v-cloak` : 可解决网络慢没数据 - 输出 {{ xxx }};
* `v-text`   : 会清空当前dom输出数据
* `v-html`   : 可输出html标签
* `v-bind`   : 提供绑定属性的指令,可以写合法的js表达式 v-bind可缩写成 :title
* `v-on`       ：提供绑定事件的指令，可以缩写成 @click="x"
* `ref='dom'`     :提供了获取子元素的方法, `this.$refs.dom` 来获取

```html
	<div id="app">
		<p v-cloak>-----{{ message }}------</p><!-- 插值表达式 只会替换差值表达式 -->
		<p v-text='text' @click="con"></p>
		<div v-html='html'></div>
		<input type="button" value="按钮" :title="mytitle + 123" v-on:click="con">
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		var app = new Vue({
		  el: '#app', // 表示当前  new 出来的实例，要控制页面上的哪个区域
		  // 这里data 就是 MVVM中的 M 专门用来保存页面数据的
		  data: { // data 属性中，存放的是 el 中要使用的数据
		    message: 'Hello Vue!' ,
		    text:'text',
		    html:'<span>html</span>',
		    mytitle:'提示'
		  },
		  methods:{//这个属性定义了当前vue实例所有可用的方法

		  	con:function(){
		  		console.log('console')
		  	}

		  }
		})
	</script>
```

## 基本使用

> 跑马灯案例

```
	<div id="app">
		<p @click="lang">开始</p>
		<p @click="ting">停止</p>
		<p>{{message}}</p>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		var app = new Vue({
		  el: '#app', 
		  data: { 
		    message: '猥琐发育！别浪',
		    intid:null //把定时器的id写在data里面方面读取
		  },
		  methods:{
		  	lang() { //es6 写法 直接写函数名
		  		if(this.intid != null)return;
		  		this.intid  = setInterval( () => { //es6 写法 改变this指向
		  			var sta = this.message.substring(0,1),
		  				end = this.message.substring(1);
		  			this.message = end + sta;
		  		},400)
		  	},
		  	ting(){
		  		clearInterval(this.intid);
		  		this.intid = null;
		  	}
		  }
		})
	</script>
```
## 事件方法

```
@click="fn";//fn($event)
fn(e){
	// e.target 是你当前点击的元素
    // e.currentTarget 是你绑定事件的元素
}
```

## 事件修饰符

* `.stop` ：阻止冒泡
* `.prevent` ：阻止默认行为
* `.capture` ：实现捕获触发事件的机制
* `.self` ：只有点击当前元素的时候，才会触发函数
* `.once` ： 只触发一次 

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
    	#app div{height: 100px;width: 100px;background-color: #333;margin: 10px};
    </style>
</head>
<body>
	<div id="app">
		<div @click="divClick">
			<!-- 使用 .stop 阻止事件冒泡 -->
			<input type="button" name="" value="点击" @click.stop="btnClick" >
		</div>
			<!-- 使用 .prevent 阻止默认行为 -->
		<a href="http:/www.baidu.com" @click.prevent="aClick">默认行为</a>

		<div @click.capture="divClick">
			<!-- 使用 .capture 实现捕获触发事件的机制 -->
			<input type="button" name="" value="点击" @click="btnClick" >
		</div>

		<div @click.self="divClick">
			<!-- 使用 .self 实现只有点击当前元素的时候，才会触发函数 -->
			<input type="button" name="" value="点击" @click="btnClick" >
		</div>
			<!-- 使用 .once 只触发一次 阻止默认行为也执行一次 顺序先后没关系 -->
		<a href="http:/www.baidu.com" @click.prevent.once="aClick">默认行为</a>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		var app = new Vue({
		  el: '#app', 
		  data: {},
		  methods:{
		  	divClick(){
		  		console.log("div");
		  	},
		  	btnClick(){
		  		console.log("btn");
		  	},
		  	aClick(){
		  		console.log('a');
		  	}
		  }
		})
	</script>
</body>
</html>
```
## 双向数据绑定

> v-model

```
<div id="app">
	<h4>{{msg}}</h4>
	<!-- 使用 v-model 指令，可以实现表单元素和model中的数据双向绑定
		注意：v-model 只能运用在表单元素中 -->
	<!-- input(radio,text,address,email...) select checkbox textarea -->
	<input type="text" v-model="msg">
</div>
<script type="text/javascript" src="vue.js"></script>
<script type="text/javascript">
	var app = new Vue({
	  el: '#app', 
	  data: {
	  	msg:'abc'
	  },
	  methods:{}
	})
</script>
```

> 简易计算器 (v-model案例)

```
	<div id="app">
		<input type="text" v-model="n1">
		<select v-model="opt">
			<option value="+">+</option>
			<option value="-">-</option>
			<option value="*">*</option>
			<option value="/">/</option>
		</select>
		<input type="text" v-model="n2">
		<input type="button" value="=" @click="calc">
		<input type="text" v-model="result">
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		var app = new Vue({
		  el: '#app', 
		  data: {
		  	n1:0,
		  	n2:0,
		  	result:0,
		  	opt:'+' 
		  },
		  methods:{
		  	calc(){//计算器算数方法
		  		// switch(this.opt){
		  		// 	case '+' :
		  		// 	this.result = parseInt(this.n1) + parseInt(this.n2);
		  		// 		break;
		  		// 	case '-' :
		  		// 	this.result = parseInt(this.n1) - parseInt(this.n2);
		  		// 		break;
		  		// 	case '*' :
		  		// 	this.result = parseInt(this.n1) * parseInt(this.n2);
		  		// 		break;
		  		// 	case '/' :
		  		// 	this.result = parseInt(this.n1) / parseInt(this.n2);
		  		// 		break;
		  		// }
		  		//这是投机取巧的方式，正式开发中尽量少用
		  		var code = 'parseInt(this.n1) '+ this.opt +' parseInt(this.n2)';
		  		//eval()把字符串解析执行
		  		this.result = eval(code);
		  	}
		  }
		})
	</script>
```
## 在vue中使用样式

### class


* `数      	 组`  :class="['red','act']"
* `三 元  表 示`  :class="['red','act', flag?'thin':'']"
* `数组嵌套对象`  :class="['red','act', {'thin':flag}]"
* `直接使用对象`  :class="{thin:flag , act:false , ita : true}"
* `使用绑定机制`  :class="obj"


```
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
    	.red{color: red}
    	.thin{font-weight: 20}
    	.ita{font-style: italic;}
    	.act{letter-spacing: 0.5em}
    </style>
</head>
<body>
	<div id="app">
		<!-- 数组的方式 class用v-bind 做数据绑定 -->
		<h1 :class="['red','act']">测试测试测试测试</h1>
		<!-- 在数组中使用三元表达式  -->
		<h1 :class="['red','act', flag?'thin':'']">测试测试测试测试</h1>
		<!-- 在数组中使用对象来代替三元表示提高代码的可读性 -->
		<h1 :class="['red','act', {'thin':flag}]">测试测试测试测试</h1>
		<!-- 直接使用对象 -->
		<h1 :class="{thin:flag , act:false , ita : true}">测试测试测试测试</h1>
		<!-- 也可以使用这种方式 -->
		<h1 :class="obj">测试测试测试测试</h1>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		var app = new Vue({
		  el: '#app', 
		  data: {
		  	flag:true,
		  	obj:{thin:this.flag , act:false , ita : true}
		  },
		  methods:{}
		})
	</script>
</body>
</html>
```

### 使用内联样式

* 直接在元素身上使用 :style的形式，书写样式对象
* 在:style中通过数组，引用多个data上的样式对象

```
<div id="app">
	<h1 :style="styleobj1">测试测试测试测试</h1>
	<h1 :style="[styleobj1,styleobj2]">测试测试测试测试</h1>
</div>
<script type="text/javascript" src="vue.js"></script>
<script type="text/javascript">
	var app = new Vue({
	  el: '#app', 
	  data: {
	  	styleobj1:{color:'red','font-weight':200},
	  	styleobj2:{'font-style':'italic'}
	  },
	  methods:{}
	})
</script>
```

## v-for 和 key 属性

### 迭代数组

```
<div id="app">
	<p v-for="item,i in list" :key="item.id">{{i}}----{{item}}</p>
</div>
<script type="text/javascript" src="vue.js"></script>
<script type="text/javascript">
	var app = new Vue({
	  el: '#app', 
	  data: {
	  	list:[1,2,3,4,5,6]
	  },
	  methods:{}
	})
</script>
```

### 输出对象数组

```
	<div id="app">
		<p v-for="item in list">{{item.id}}----{{item.name}}</p>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		var app = new Vue({
		  el: '#app', 
		  data: {
		  	list:[
		  		{id:1,name:'zs1'},
		  		{id:1,name:'zs2'},
		  		{id:1,name:'zs3'}
		  	]
		  },
		  methods:{}
		})
	</script>
```

### 输出对象
```
	<div id="app">
		<!-- 在遍历对象身上的键值对的时候，除了有val key 之外，在第三个位置还有一个索引 -->
		<p v-for="(val,key,i) in user">{{key}}----{{val}}---{{i}}</p>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		var app = new Vue({
		  el: '#app', 
		  data: {
		  	user:{
		  		id:1,
		  		name:'托尼.帕克',
		  		getder:'男'
		  	}
		  },
		  methods:{}
		})
	</script>
```

### 迭代数字

```
<div id="app">
	<p v-for="i in 10">{{i}}</p>
</div>
```

### v-for使用注意事项

* 注意：v-for循环的时候，key属性只能使用 number 或者 string
* 注意：key 在使用的时候，必须使用v-bind属性绑定的形式，指定key的值
* 在组件中，使用v-for循环的时候，或者在一些特殊情况中，如果v-for有问题，必须在使用v-for的同时，指定唯一的字符串/数字 类型 :key值

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
	<div id="app">
		<div>
			<label>
				ID:
				<input type="text" v-model='id'>
			</label>
			<label>
				Name:
				<input type="text" v-model='name'>
			</label>
			<input type="button" value="添加" @click="btn">
		</div>
		<p v-for="item in list" :key="item.id">
			<input type="checkbox" name="">
			{{item.id}}----{{item.name}}
		</p>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		var app = new Vue({
		  el: '#app', 
		  data: {
		  	id:'',
		  	name:'',
		  	list:[
		  		{id:1,name:'zs1'},
		  		{id:2,name:'zs2'},
		  		{id:3,name:'zs3'}
		  	]
		  },
		  methods:{
		  	btn(){
		  		//this.list.push({id:this.id,name:this.name}) 往数组后面添加
		  		this.list.unshift({id:this.id,name:this.name})
		  	}
		  }
		})
	</script>
</body>
</html>
```

### v-if 和 v-show

> v-if有较高的切换性能消耗

如果元素涉及到频繁的切换最好不要使用v-if

> v-show有较高的初始渲染消耗

如果元素永远也不会被显示出来被用户看到，则推荐使用v-if

```
	<div id="app">
		<input type="button" value="切换" @click="flag=!flag">
		<!-- v-if 的特点 每次都会重新删除或者创建元素 -->
		<p v-if="flag">v-if</p>
		<!-- v-show 的特点 不会进行dom的删除创建操作，只会切换元素display:none操作 -->
		<p v-show="flag">v-show</p>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		var app = new Vue({
		  el: '#app', 
		  data: {
		  	flag:true
		  },
		  methods:{
		  }
		})
	</script>
```


## 综合商城添加删除案例

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
    	#app p{display: flex;width: 100%;border:1px solid #000;border-right: 0;line-height: 30px;text-align: center;margin: 0;border-bottom: 0;}
    	#app p span{width: 25%;border-right: 1px solid #000;border-bottom: 1px solid #000;}
    	#app p  a{width: 25%;border-right: 1px solid #000;border-bottom: 1px solid #000;text-align: center;color: #005aff;text-decoration: none;}
    </style>
</head>
<body>
	<div id="app">
		<p>
			ID:<input type="text" name="" v-model="id">
			Name:<input type="text" name="" v-model="name">
			<!-- 在vue中，使用事件绑定事件机制，如果加了小括号就可以传参了 -->
			<input type="button" value="添加" @click="btn">
			搜索:<input type="text" name="" v-model="sousuo">
		</p>
		<p>
			<span>ID</span>
			<span>Name</span>
			<span>Time</span>
			<span>Opera</span>
		</p>
		<!-- 之前v-for 中的数据都是直接从data上的list中直接熏染过来的 -->
		<!-- 现在，定义了一个search方法，同时，所有的关键字，通过传参的形式传递给了search方法 -->
		<!-- 在search方法内部，通过执行for，把所有符合关键字的数据，保存到新的数组中 -->
		<p v-for="item in search(sousuo)" :key="item.id">
			<span>{{item.id}}</span>
			<span>{{item.name}}</span>
			<span>{{item.time}}</span>
			<a href="" @click.prevent="del(item.id)">删除</a>
		</p>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		var app = new Vue({
		  el: '#app', 
		  data: {
		  	id:"",
		  	name:"",
		  	sousuo:"",
		  	list:[
		  		{id:1,name:"奔驰",time:new Date()},
		  		{id:2,name:"宝马",time:new Date()}
		  	]
		  },
		  methods:{
		  	btn(){
		  		var car = {id:this.id,name:this.name,time:new Date()};
		  		this.list.push(car);
		  		this.id = this.name = '';		  	
		  	},
		  	del(id){
		  		//方法1
		  		this.list.some((item,i) => {
		  			if(item.id == id){
		  				this.list.splice(i,1);
		  				//在数组中的some方法中，如果return true，就会立即终止这个数组的后续循环
		  				return true;
		  			}
		  		})
		  		//方法2（查找索引）
		  		var index = this.list.findIndex(item => {
		  			if(item.id == id){
		  				return true;
		  			}
		  		})
		  		console.log(index);
		  		//this.list.splice(index,1);删除
		  	},
		  	search(sousuo){//根据关键字，进行数据的搜索
		  		//方法1
		  		// var newList = [];
		  		// this.list.forEach(item => {
		  		// 	if(item.name.indexOf(sousuo) != -1){
		  		// 		newList.push(item)
		  		// 	}
		  		// });
		  		// return newList;

		  		//注意：forEach some fiter findIndex 这些都属于数组的新方法
		  		//都会对数组的每一项，进行遍历，执行相关的操作；

		  		//注意：ES6中，为字符串提供了一个新方法，叫做 String.prototype.includes('包含的字符串')

		  		//如何包含，则返回 true ，否则返回 false

		  		//方法2
		  		return this.list.filter(item => {
		  			if(item.name.includes(sousuo))return item;
		  		})
		  	}
		  }
		})
	</script>
</body>
</html>
```

## 过滤器

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
	<div id="app">
		<!-- 可以调用多个多滤器 -->
		<p>{{mag | msgFormat('疯狂') | text}}</p>
		<p>{{mag | msgFormat('疯狂')}}</p>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">

		//定义一个Vue全局的过滤器，名字叫做 msgFormat
		//第一个值永远都是传进来的,后面参数可以传多个
		Vue.filter('msgFormat',function(msg,arg){
			//字符串的 replace方法，第一个参数，除了可以写字符串，还可以定义一个正则
			return msg.replace(/单纯/g,arg);
		})
		Vue.filter('text',function(msg){
			return msg + '====='
		})

		var app = new Vue({
		  el: '#app', 
		  data: {
		  	mag:"我是单纯的人，我是单纯的人，我是单纯的人"
		  },
		  methods:{

		  }
		})
	</script>
</body>
</html>
```

### 全局过滤器案例

```
<span>{{item.time | dateFom()}}</span>
//全局过滤器,进行时间的格式化
Vue.filter('dateFom',function(datastr,pattern=''){
	//根据给定的字符串，得到特定的时间
	var dt = new Date(datestr);

	var y = dt.getFullYear();
	//es6中新增字符串方法：String.protorype.padStart(maxLength,fillString='');
	//用来填充字符串,padStart(最后的长度,补什么)
	var m = (dt.getMonth() + 1).toString().padStart(2,'0')
	var d = dt.getDate().toString().padStart(2,'0');

	if(pattern.toLowerCase() === 'yyyy-mm-dd'){
		//return y + '-' + m + '-' + d;
		return `${y}-${m}-${d}`;
	}else{
		var hh = dt.getHours().toString().padStart(2,'0');
		var mm = dt.getMinutes().toString().padStart(2,'0');
		var ss = dt.getSeconds().toString().padStart(2,'0');
		return `${y}-${m}-${d} ${hh} : ${mm} : ${ss}`;
	}
})
```

### 时间过滤器案例

```javascript
//先npm安装 moment
npm moment -S
//导入时间插件
import moment from 'moment'

//定义全局的过滤器（时间过滤器）
//第一个参数是过滤的数据，第二个是过滤的格式(传递了一个默认参数)
Vue.filter('dataFormat',function(dataStr,pattern = 'YYYY-MM-DD HH:mm:ss'){
    return moment(dataStr).format(pattern)
})
//通过管道符调用一下即可
<span>时间：{{time | dataFormat}}</span>
//也可指定格式
<span>时间：{{time | dataFormat('YYYY-MM-DD')}}</span>
```



### 自定义私有的过滤器

```
	<div id="app">
		<p>{{dt | datefom()}}</p>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">


		var app = new Vue({
		  el: '#app', 
		  data: {
		  	dt:new Date()
		  },
		  methods:{

		  },
		  filters:{//定义私有过滤器，两个条件 【名称 和 处理函数】
		  	//过滤器调用的时候采用的是就近原则，
		  	//如果私有过滤器和私有过滤器名称一致
		  	//优先调用私有过滤器
		  	datefom:function(datestr,pattern=""){
		  		//根据给定的字符串，得到特定的时间
				var dt = new Date(datestr);

				var y = dt.getFullYear();
				//es6中新增字符串方法：String.protorype.padStart(maxLength,fillString='');
				//用来填充字符串,padStart(最后的长度,补什么)
				var m = (dt.getMonth() + 1).toString().padStart(2,'0')
				var d = dt.getDate().toString().padStart(2,'0');

				if(pattern.toLowerCase() === 'yyyy-mm-dd'){
					//return y + '-' + m + '-' + d;
					return `${y}-${m}-${d}`;
				}else{
					var hh = dt.getHours().toString().padStart(2,'0');
					var mm = dt.getMinutes().toString().padStart(2,'0');
					var ss = dt.getSeconds().toString().padStart(2,'0');
					return `${y}-${m}-${d} ${hh} : ${mm} : ${ss}`;
				}
		  	}
		  }
		})
	</script>

```


## 键盘修饰符

```
@keyup.enter = "fn"
```

### 自定义全局案件修饰符
```
//@keyup.键盘码
<input type="text" @keyup.f2="fn">

Vue.config.keyCodes.f2 = 113;
```


## 自定义指令

> 让文本框获取焦点

```
<input type="text" v-name>
//Vue.directive('名称',{对象})
Vue.directive('name',{
	bind:function(el){//每当指令绑定到元素上的时候，插入到dom中，会执行一次bind函数
	//和样式相关的操作，一般都写在在bind里面
	el.style.color = 'red'
	},
	inserted:function(el){//表示元素插入到dom的时候，会执行，inserted函数触发一次

	//注意：在每个函数中，第一个参数永远书el，表示被绑定了指令的那个元素，这个el参数，是个原生的js对象
	//注意：第二个参数是(形参，名字自定义)binding是一个对象

	//和js行为有关的操作，最好写在inserted里面，防止js行为不生效
		el.focus();
	},
	updated:function(el){//当vnode更新的时候，会执行updatad，可能会触发多次

	}
})
```

### 自定义私有指令

```
<input type="text" name="" v-font='18'>

var app = new Vue({
  el: '#app', 
  data: {
  	wenzi:'测试文字'
  },
  methods:{

  },
  filters:{
  },
  directives:{
  	'font':{
  		bind:function(el,binding){
  			el.style.fontWidth = binding.value;
  		}
  	}
  }
})
```

### 简化自定义私有指令

```
fonts:function(el,bin){//注意：这个function等同于把代码写到了bind和update中去
	console.log(el);
	console.log(bin);
}
```

## vue生命周期
```
它可以总共分为8个阶段：

beforeCreate：实例初始化

created：实例建立完成（可以取得$data）

beforeMount：模板挂载之前（还没有生成html）

mounted：模板挂载完成

beforeUpdate：如果data发生变化，触发组件更新，重新渲染

updated：更新完成

beforeDestroy：实例销毁之前（实例还可以使用）

destroyed：实例已销毁（所有绑定被解除、所有事件监听器被移除、所有子实例被移除）

生命周期钩子函数要跟data平级
```
## http请求

```
	var app = new Vue({
	  el: '#app', 
	  data: {cent:''},
	  methods:{
	  	getIns(){ 
	  		//当发起get请求后，通过.then来设置成功的回调函数
	  		this.$http.get('http://vue.studyit.io/api/getlunbo').then(function(result) {
	  			//通过result.body拿到服务器返回的成功的数据
	  			console.log(result.body)
	  		})
	  	},
	  	posIns(){
	  		//手动发起的post请求，默认没有表单格式，所以，有的服务器处理不了
	  		//通过设置post方法的第三个参数，设置提交内容为普通的表单格式 
	  		this.$http.post('http://vue.studyit.io/api/post',{value:this.cent},{emulateJSON:true}).then( result => {
		  			if(result.body.status === 0 ){//成功
		  				console.log(result);
		  			}
	  			}
	  		})
	  	},
	  	josnpIns(){
	  		this.$http.jsonp('http://vue.studyit.io/api/jsonp').then(result=>{
	  			console.log(result)
	  		})
	  	}
	  }
	})
```
### http案例

> html

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
	<script type="text/javascript">
		function showInfo(data) {
			console.log('ok')
			console.log(data)
		}
	</script>
	<script type="text/javascript" src="http://127.0.0.1:3000/get?callback=showInfo"></script>
</body>
</html>
```

> node服务器 

```
//导入http内置模块
const http = require('http');
//解析url地址
const urlModule = require('url');
//创建一个http服务器
const server = http.createServer();
//监听 http服务器request请求
server.on('request',function(req,res){
	const { pathname:url , query } = urlModule.parse(req.url , true);
	if(url === '/get'){
		var data = {
			name:'xjj',
			name:18,
			gend:'女'
		}
		var srip = `${query.callback}(${JSON.stringify(data)})`;
		res.end(srip);
	}else{
		res.end('404');
	}
});
server.listen(3000,function(){
	console.log('server listen at http://127.0.0.1:3000')
})
```

## 全局配置数据接口的设域名

```
//如果通过全局配置了，请求接口根域名则，在每次单独发起http请求的时候
//请求的url，应该以相对路径开头，前面不能带 / 否则不会启用
Vue.http.options.root = 'http://vue.studyit.io/';
this.$http.get('api/getlunbo').then(function(result) {
	console.log(result.body)
})
```
## 全局启用 emulateJSON 选项

```
Vue.http.options.emulateJSON = true;
```

# Vue动画

## 使用过渡类名实现动画

* v-enter-active {定义进入过渡生效时的状态}
* v-leave-active {定义离开过渡生效时的状态}


```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<link rel="stylesheet" type="text/css" href="animate.css">
	<script type="text/javascript" src="vue.js"></script>
</head>
<body>
	<div id="app">
		<input type="button" value="toggle" @click="flag=!flag">
		<transition enter-active-class='bounce' leave-active-class="swing"
					:duration="{ enter:800,leave:1600 }">
			<h3 v-if="flag" class="animated">测试动画</h3>
		</transition>
	</div>
	
	<script type="text/javascript">
		var vm = new Vue({
			el:'#app',
			 data: {
			 	flag:false
			 },
			 methods:{
		  	}
		})
	</script>
</body> 	
</html>
```

## 钩子函数实现半场动画

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<script type="text/javascript" src="vue.js"></script>
	<style type="text/css">
		.ball{height: 10px;width: 10px;border-radius: 50%;background-color: #000}
	</style>
</head>
<body>
	<div id="app">
		<input type="button" value="toggle" @click="flag=!flag">
		<transition
			@before-enter="beforeEnter"
			@enter="enter"
			@after-enter="afterEnter"
		>
			<div class="ball" v-if="flag"></div>
		</transition>
	</div>
	
	<script type="text/javascript">
		var vm = new Vue({
			el:'#app',
			 data: {
			 	flag:false
			 },
			 methods:{
			 	beforeEnter(el){
			 		el.style.transform = "translate(0,0)"
			 	},
			 	enter(el, done){
			 		el.offsetWidth
			 		el.style.transform = "translate(150px,450px)"
			 		el.style.transition = "all 1s ease"
			 		el.style.backgroundColor = '#00ff00'
			 		done()
			 	},
			 	afterEnter(el){
			 		this.flag = !this.flag
			 	}
		  	}
		})
	</script>
</body>
</html>
```

## transition-group元素实现列表动画

```

<style type="text/css">
	.v-enter,.v-leave-to{opacity:0;transform: translateY(150px);}
	.v-enter-active,.v-leave-active{transition:all 0.6s ease}

	.v-move{
		transition:all 0.6s ease
		<!-- 设置元素在位移的的时候进行相关的设置 -->
	}
	.v-leave-active{position:absolute}
</style>
//appear实现入场动画
//tag让transition-group自动渲染为一个ul元素（默认是span）
<transition-group appear tag="ul">
	<p v-for="item in list" :key="item.id">
		{{item.id}} --- {{item.name}}
	</p>
</transition-group>
```


# Vue组件

拆分了Vue实例的代码量，能够让我们以不同的组件，来划分不同的功能模块,将来我们需要什么样的功能，就可以去调用对应的组件即可

## 组件化和模块化的区别

* 组件化：是从UI界面的角度进行划分的；
	1. 方便代码分层开发，保证每个功能模块的职能单一；
* 模块化：是从代码逻辑的角度进行划分；
	2. 前端的组件化，方便UI组件的重用

## 创建组件方法1

```
	<div id="app">
		<my-com1></my-com1>
	</div>
	var com1 = Vue.extend({
		template:'<h3>Vue.extend</h3>'
	})
	//组件命名（驼峰形式的话，引用的时候就要加-并且小写）
	Vue.component('myCom1' , com1);
```

## 创建组件的方法2

```
<my-com2></my-com2>
Vue.component('myCom2' , ({
	template:'<h3>Vue.extends</h3>'
	//报错，只能有一个根元素（解决方案用div包住就行）
	//template:'<h3>Vue.extends</h3><span></span>'
}));
```

## 创建组件的方法3

```
	<div id="app">
		<my-com3></my-com3>
	</div>
	<template id="temp1">
		<div>
			<h1>h1h1h1h1</h1>
			<p>ppppppppp</p>
		</div>
	</template>
	Vue.component('myCom3' , ({
		template:'#temp1'
	}));

```

## 使用components定义实例内部私有组件

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<script type="text/javascript" src="vue.js"></script>
</head>
<body>
	<div id="app">
		<login></login>
	</div>
	<template id="temp1">
		<div>
			<h1>h1h1h1h1</h1>
			<p>{{mas}}</p>
		</div>
	</template>
	
	<script type="text/javascript">

		var vm = new Vue({
			el:'#app',
			components:{
				login:{
					template:'#temp1',
					//组件可以有自己的Data数据
					//组件的data和实例的data有点不一样,实例中的Data可以为一个对象，
					//但是组件中的DAta必须的一个方法，这个方法内部必须返回一个对象
					data:function(){
						return{
							mas:'这是组件中data定义的数据'
						}
					}
				}
			}
		})
	</script>
</body>
</html>
```

## 使用Vue提供的component元素实现组件切换

```
<div id="app">
	<a href="" @click.prevent="com='login1'">111</a>
	<a href="" @click.prevent="com='login2'">222</a>
	<component :is="com"></component>
</div>
Vue.component('login1',{
	template:'<h3>1111</h3>'
})
Vue.component('login2',{
	template:'<h3>2222</h3>'
})

var vm = new Vue({
	el:'#app',
	data:{
		com:'login1'
	}
})
```
## 使用Vue提供的component元素实现组件切换  添加 动画过滤

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<script type="text/javascript" src="vue.js"></script>
	<style type="text/css">
		.v-enter-active, .v-leave-active {
		  transition: all .5s ease;
		  
		}
		.v-enter, .v-leave-to /* .fade-leave-active below version 2.1.8 */ {
		  opacity: 0;
		  transform: translateX(150px);
		}
	</style>
</head>
<body>
	<div id="app">
		<a href="" @click.prevent="com='login1'">111</a>
		<a href="" @click.prevent="com='login2'">222</a>
		<!-- 通过mode属性，设置切换时候的模式 -->
		<transition mode='out-in'>
			<component :is="com"></component>
		</transition>
	</div>
	
	
	<script type="text/javascript">

		Vue.component('login1',{
			template:'<h3>1111</h3>'
		})
		Vue.component('login2',{
			template:'<h3>2222</h3>'
		})

		var vm = new Vue({
			el:'#app',
			data:{
				com:'login1'
			}
		})
	</script>
</body>
</html>
```

## 父组件向子组件传值的问题

> 添加

```javascript
<子组件 :data="item">
接收:this.$attrs.data
```

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <div id="app">
        <com1 v-bind:parent="msg"></com1>
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript">
        var app = new Vue({
          el: '#app', 
          data: {
             msg:'父组件中的数据'
          },
          methods:{
            
          },
          components:{
            com1:{
              data(){//注意：子组件中的data数据，并不是通过父组件传递过来的，而是子组件自身私有的，比如：子组件通过ajax请求回来的数据都可以放到data身上
                //data上的数据，都是可读可写的
                return{
                  title:'123',
                  content:'qqq'
                }
              },
              template:'<h1>子组件 --- {{ parent }}</h1>',
              //注意：组件中的所有 props中的数据，都是通过负组件传递给子组件的
              //props中的数据，都是只读的,无法重新赋值
              props:['parent']
              //把父组件传递过来的parentems属性，先在props数组中，定义一下，这样才能使用这个数据
              }
          }
        })
    </script>
</body>
</html>
```

## 组件传值 子组件通过事件调用向父组件传值

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <div id="app">
     <!--  父组件向子组件传递方法，使用的是事件绑定机制；v-on，当我们定义了一个事件属性之后，那么，子组件就能够，通过某些方式，来调用传递进去的这个方法 -->
        <com1 @func="show"></com1>
    </div>
    <template id="tmpl">
      <div>
        <h1>这是子组件</h1>
        <input type="button" value="(子组件)这是触发父组件的按钮" @click='myclick'>
      </div>
    </template>

    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript">

        var com1 = {
          template:'#tmpl',
          data(){
            return {
              masdata:{name:'Datourez',age:18}
            }
          },
          methods:{
            myclick(){
              //console.log('ok')
              //当点击子组件的时候通过this.$emit('func')，来调用func这个方法
              //emit：触发调用的意思
              this.$emit('func',this.masdata)
            }
          }
        }

        var app = new Vue({
          el: '#app', 
          data: {
             msg:null
          },
          methods:{
            show(data){
              //console.log('调用了父组件的方法-----'+data+data2)
              this.msg = data;
              console.log(this.msg)
            }
          },
          components:{
            com1
          }
        })
    </script>
</body>
</html>
```

## 组件案例（发表评论功能）

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
      ul,li{list-style: none;}
      span {margin: 5px 10px}
    </style>
</head>
<body>
    <div id="app">
        <com-box @func='loadConnts'></com-box>
        <ul>
          <li v-for="item in list">
            <span>{{item.id}}</span>
            <span>{{item.user}}</span>
            <span>{{item.content}}</span>
          </li>
        </ul>
    </div>
  
    <template id="tmpl">
        <div>
          <label>
            name:<input v-model="user">
          </label>
          <label>
            content:<input v-model="content">
          </label>
          <input type="button" value="提交" @click="postcontent">
        </div>
    </template>

    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript">
        //字面量的方式定义组件
        var comBox = {
          data(){
            return {
              user:'',
              content:''
            }
          },
          template:'#tmpl',
          methods:{
            postcontent(){
              //获取输入框的数据
              var comment = {id:Date.now(),user:this.user,content:this.content}
              //获取数据库的数据并转换返回数组格式
              var list = JSON.parse(localStorage.getItem('cmts') || '[]');
              //向数组前面添加获取的数据
              list.unshift(comment)
              //在吧最新的存到数据库
              localStorage.setItem('cmts',JSON.stringify(list))
              this.user = this.content = ''
              //调用父组件的函数
              this.$emit('func')
            }
          }
        }

        var app = new Vue({
          el: '#app', 
          data: {
             list:[
             //假数据
                {id:Date.now(),user:'李白',content:'天生无才必有用'}
             ]
          },
          created(){
            //在生命周期函数中（初始化vue实例之后调用函数）
            this.loadConnts();
          },
          methods:{
            loadConnts(){
              //获取数据库的数据并转换格式返回数组
              var list = JSON.parse(localStorage.getItem('cmts') || '[]');
              //重新给list赋值
              this.list = list;
            }
          },
          components:{
            //定义组件
            'com-box':comBox
          }
        })
    </script>
</body>
</html>
```

# ref获取DOM元素和组件引用

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <div id="app">
        <input type="button" value="获取元素" @click="yuansu" ref='mybtn'>
        <h3 ref='myh3'>hhhhhhhhhh</h3>
        <hr>

      <login ref='mylogin'></login>
    </div>

    
    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript">

        var login = {
          template:'<h1>登陆组件</h1>',
          data(){
            return{
              mas:'son mas'
            }
          },
          methods:{
            show(){
              console.log('子组件')
            }
          }
        }

        var app = new Vue({
          el: '#app', 
          data: {
             
          },
          methods:{
            yuansu(){
              console.log(this.$refs)
              console.log(this.$refs.mylogin.mas)
              this.$refs.mylogin.show()
            }
          },
          components:{
            login
          }
        })
    </script>
</body>
</html>
```


# 路由


## 安装

### 直接下载 / CDN
```
https://unpkg.com/vue-router/dist/vue-router.js
```

## 路由的基本使用

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
    
</head>
<body>
    <div id="app">
      <!-- 是由Vue-router提供的 不管生成什么元素，都会绑定对应的事件 -->
      <router-link to="/login" tag="span">登陆</router-link>
      <router-link to="/register">注册</router-link>

      <!-- 这是由Vue-router提供的元素，专门用来当作占位符的,将来由路由规则匹配到的组件，就会展示到这个router-view中去 -->
       <router-view></router-view>
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <!-- 1. 安装路由模块 -->
    <script type="text/javascript" src="vue-router.js"></script>
    <script type="text/javascript">
        //组件模板对象
        var login = {
          template:'<h1>登陆组件</h1>'
        }
        var register = {
          template:'<h1>注册组件</h1>'
        }

       // 2. 创建一个路由对象，当到日vue-router包之后，在window全局对象中，就有了一个路由的构造函数，叫做VurRouter
       //在new 路由对象额度时候，可以为构造函数，传递一个配置对象

       var routerObj = new VueRouter({
         // route 这个配置对象中的route/表示路由配置规则的意思
         routes:[//路由匹配规则
         //每个路由规则都是一个对象，这个规则对象身上，有两个必须的属性
         //属性1: path表示监听哪个路由链接地址 
         //属性2: component，如果路由是前面匹配到的path则展示component属性对应的那个组件
         //注意component属性值必须是组件的模板对象，不能是组件的引用名称
         // {path:'/',component:login},
         //默认指向登陆
         {path:'/',redirect:'/login'},
            {path:'/login',component:login},
            {path:'/register',component:register}
         ]
       })

        var app = new Vue({
          el: '#app', 
          data: {},
          methods:{},
          router:routerObj //将路由规则对象，注册到vm实例上，用来监听URL地址的变化，然后展示对应的组件
        })
    </script>
</body>
</html>
```

## 设置选中路由高亮显示的方法

* 直接设置类样式(router默认被激活会有当前选中样式)

```
.router-link-active{color: red}
```

* 在构造函数中修改与routes同级

```

linkActiveClass:'myactive'
```

## 为路由启用切换动画

```
<style type="text/css">
   .v-enter,.v-leave-to{opacity:0;transform: translateX(150px);}
    .v-enter-active,.v-leave-active{transition:all 0.6s ease}
</style>
<transition mode="out-in">
 <router-view></router-view>
</transition>
```

## 路由传参-使用queery方式传递参数

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <div id="app">
      <router-link to="/login?id=10&name=zs">登陆</router-link>
      <router-link to="/register">注册</router-link>
      <router-view></router-view>
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript" src="vue-router.js"></script>
    <script type="text/javascript">
        var login = {
          template:'<h1>登陆组件---{{$route.query.name}}</h1>',
          data(){
            return{
              mas:123
            }
          },
          created(){
            console.log(this.$route.query)
          }
        }
        var register = {
          template:'<h1>注册组件</h1>'
        }
       var routerObj = new VueRouter({
         routes:[
            {path:'/',redirect:'/login'},
            {path:'/login',component:login},
            {path:'/register',component:register}
         ],
         linkActiveClass:'myactive'
       })

        var app = new Vue({
          el: '#app', 
          data: {},
          methods:{},
          router:routerObj
        })
    </script>
</body>
</html>
```

## 路由传参-使用params方式传递参数

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <div id="app">
      <router-link to="/login/12/ls">登陆</router-link>
      <router-link to="/register">注册</router-link>
      <router-view></router-view>
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript" src="vue-router.js"></script>
    <script type="text/javascript">
        var login = {
          template:'<h1>登陆组件---{{$route.params.name}}------{{$route.params.id}}</h1>',
          data(){
            return{
              mas:123
            }
          },
          created(){
            console.log(this.$route.params)
          }
        }
        var register = {
          template:'<h1>注册组件</h1>'
        }
       var routerObj = new VueRouter({
         routes:[
            {path:'/',redirect:'/login'},
            {path:'/login/:id/:name',component:login},
            {path:'/register',component:register}
         ],
         linkActiveClass:'myactive'
       })

        var app = new Vue({
          el: '#app', 
          data: {},
          methods:{},
          router:routerObj
        })
    </script>
</body>
</html>
```

## 路由的嵌套

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <div id="app">
         <router-link to="/account">Account</router-link>

         <router-view></router-view>
    </div>

    <template id="teml">
        <div>
          <h1>这是Account</h1>

          <router-link to="/account/login">登陆</router-link>
          <router-link to="/account/regin">注册</router-link>

          <router-view></router-view>
        </div>
    </template>

    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript" src="vue-router.js"></script>
    <script type="text/javascript">


        var account = {
          template:'#teml'
        }
        var login = {
          template:'<h3>登陆</h3>'
        }
        var regin = {
          template:'<h3>注册</h3>'
        }
        var router = new VueRouter({
          routes:[
            {
              path:'/account',
              component:account,
              //使用children属性，实现子路由，同时，子路由的path前面，不要带斜线（/），否则永远以根路径开始请求，这样不方便我们用户去理解url地址
              children:[
                  {path:'login',component:login},
                  {path:'regin',component:regin}
              ]
            }
          ]
        })

        var vm = new Vue({
          el:'#app',
          data:{},
          methods:{},
          router
        })
    </script>
</body>
</html>
```

# 路由跳转提示

```javascript
beforeRouteUpdate(to,from,next){
    if(confirm('确定跳转？')){
       next();
     }
}
```

## 实现经典布局

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
    html,body,h1{margin: 0;padding: 0}
    .conBox{display: flex;}
    .hader{background-color: #cabe36}
    .conBox h1{height: 500px;}
    .left{width: 30%;background-color: #ababab}
    .main{width: 70%;background-color: #4f2bbf69}
    </style>
</head>
<body>
    <div id="app">
         <router-view name="hader"></router-view>
         <div class="conBox">
            <router-view name="left"></router-view>
            <router-view name="main"></router-view>
         </div>
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript" src="vue-router.js"></script>
    <script type="text/javascript">


        var hader = {
          template:'<h1 class="hader">头部</h1>'
        }
        var left = {
          template:'<h1 class="left">登陆</h1>'
        }
        var main = {
          template:'<h1 class="main">注册</h1>'
        }
        var router = new VueRouter({
          routes:[
           {path:'/',components:{
              'hader':hader,
              'left':left,
              'main':main
           }
          }
          ]
        })

        var vm = new Vue({
          el:'#app',
          data:{},
          methods:{},
          router
        })
    </script>
</body>
</html>
```

# watch 监听文本框数据的变化

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <div id="app">
        <input v-model="aaa">+
        <input v-model="bbb">=
        <input v-model="ccc">
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript" src="vue-router.js"></script>
    <script type="text/javascript">
        var vm = new Vue({
          el:'#app',
          data:{
            aaa:'',
            bbb:'',
            ccc:''
          },
          methods:{},
          watch:{
            aaa:function(){
              //监听data中aaa的变化
              console.log(111)
            },
            //提供了两个参数，1：newVal(新的值)。2：oldVal(旧的值)
            'bbb':function(newVal,oldVal){
              console.log(newVal+'---'+oldVal)
            }
          }
        })
    </script>
</body>
</html>
```

##  keyup 监听文本框数据的变化

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <div id="app">
        <input v-model="aaa" @keyup="getFunc">+
        <input v-model="bbb" @keyup="getFunc">=
        <input v-model="ccc" @keyup="getFunc">
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript" src="vue-router.js"></script>
    <script type="text/javascript">
        var vm = new Vue({
          el:'#app',
          data:{
            aaa:'',
            bbb:'',
            ccc:''
          },
          methods:{
            getFunc(){
              console.log(this.aaa+'---'+this.bbb+'---'+this.ccc)
            }
          }
        })
    </script>
</body>
</html>

```

# watch 监听路由地址的变化

```
watch:{
	'$route.path':function(newVal,oldVal){
	  console.log(newVal+'---'+oldVal)
	}
}
```

## computed 计算属性的使用

> 应用	

* 频繁使用的复杂公式

* 需要监控的数据 — 全局状态管理

  不是必须的

> 可获取可写

* get:function(){}
* set:function(value){}

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <div id="app">
        <input v-model="aaa">+
        <input v-model="bbb">=
        <input v-model="ccc">
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript" src="vue-router.js"></script>
    <script type="text/javascript">
        var vm = new Vue({
          el:'#app',
          data:{
            aaa:'',
            bbb:''
          },
          methods:{},
          computed:{
            //注意：计算属性，在引用的时候，一定不要加()，去调用，直接把它当作普通的属性去使用就好了
            //注意：计算属性，这个function内部所用到的data中数据发生了变化，就会重新计算这个计算属性的值
            //注意：计算属性的求值结果，会被缓存起来，方便下次直接使用，如果计算属性方法中，任何数据，都灭有发生变化，则不会重新对计算属性求值
            'ccc':function () {
              return this.aaa+"-"+this.bbb
            }
          }
        })
    </script>
</body>
</html>

```

# vuex

## 入门

### 安装

```javascript
npm vuex -S
```

### 配置

```javascript
//min.js文件
//引入外部自定义编写的Store
import store from './stort/'
//并把Store挂载到vm实例上
let vm=new Vue({
    store
})
```

### 创建

>  stort/index.js

```javascript
import Vue from 'vue'
//导包
import Vuex from 'vuex'
//注册Vuex到Vue中
Vue.use(Vuex)
//创建一个数据仓储对象，并返回
export default new Vuex.Store({
    strict:true, //严格模式---只能由mutation修改状态
    state:{
        //data
        //组件中访问：{{$store.state.count}}
        count:0
    },
    mutations:{
        //操作store中state的数据，只能调用mutations提供的方法才能操作对应的数据
        //组件中调用：this.$store.commit('add',str)
        add(store,str){
           store.count+=str
        }
    },
    actions:{
        //Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters
        increment (context) {
            context.commit('add')
        }
    },
    getters:{
        //只负责对外提供数据，不负责修改数据
        //组件中访问：{{$store.getters.optCount}}
        //此功能跟过滤器很相似，都没有修改原数据，只是对数据进行了包装
        optCount:function(store){
            retrun '当前最新的count值是' + store.count
        }
    }
})
```

# axios

## 装包

```shell
npm axios -S
```

## 配置

```javascript
import axios form 'axios'
//将axios绑定在vue原型上，这样可以在其他组件中通过this.$axios使用
Vue.prototype.$axios = axios
//---------------------------------------
//config/index.js(解决跨域问题、设置代理)
//---------------------------------------
proxyTable:{
    //设置调用的借口域名和端口号，注意加http
    target:'http:node.io/api',
    pathRewrite:{
        //用/api代替target里面的url，
		'^/api':'/'
    }
}
```

## 使用

```javascript
https://www.kancloud.cn/yunye/axios/234845
```



# nrm

> 注意：npm只是单纯的提供了几个常用的下载包的URL地址，并能够让我们在几个地址之间，很方便的进行切换，但是我们每次装包的时候，使用的装包工具永远都是，npm i ***

* 运行`npm i nrm -g`全局安装nrm包；
* 使用`nrm ls`查看当前所有可用的镜像源地址以及当前所使用的镜像源地址
* 使用`nrm use npm`或者`nrm use taobao`切换不同的镜像源地址

##	cnpm  下载包的速度更快一些。

```
	地址：http://npm.taobao.org/
	
	安装cnpm:

	npm install -g cnpm --registry=https://registry.npm.taobao.org
```
