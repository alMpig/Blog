# 组件化

- 开发效率高
- 可维护性好
- 笔记源码 --- [ component-study ]

# Element 

- http://element-cn.eleme.io/#/zh-CN
- 饿了吗团队开发

## 快速体验

1. 初始化项目

```shell
vue init webpack component-study
```

2. 启动项目

```shell
npm run dev
```

3. 为项目安装Element

```shell
npm i element-ui
```

4. 在 main.js 中配置

```javascript
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
Vue.use(ElementUI)
```

5. 组件体验（在app.vue组件使用）

```html
<el-row>
  <el-button>默认按钮</el-button>
  <el-button type="primary">主要按钮</el-button>
  <el-button type="success">成功按钮</el-button>
  <el-button type="info">信息按钮</el-button>
  <el-button type="warning">警告按钮</el-button>
  <el-button type="danger">危险按钮</el-button>
</el-row>
```

# 组件的注册

## 第一种注册组件方法

```html
<button-counter></button-counter>
```

```vue
Vue.component('button-counter',{//组件模板只能有一个根节点
	template:'
		<div>
			<button @click="fn">点击了{{count}}次</button>
             <button>第二个按钮</button>
         </div>
	',
    data(){//组件的data必须是函数
        return{
			count:0
        }
    },
    methods:{
        fn(){
	this.count++
        }
    }
})
```

## 第二种单文件组件

1. InputNumber.vue

```vue
<template>
	<div>
		<button @click="handleIncrement(-1)">-</button>
		<input type="text" v-model="num">
		<button @click="handleIncrement(1)">+</button>
	</div>
</template>
<script>
	export default{
		data(){
			return {
				num:0
			}
		},
		methods:{
			handleIncrement(count){
				this.num += count
			}
		}
	}
</script>
```

2. 在App.vue中引用组件

```vue
import InputNumber from './components/InputNumber'
```

3. 在App.vue的components中注册

```vue
components: {
    InputNumber
  }
```

4. 在App.vue中使用

```vue
<InputNumber></InputNumber>
```

# 组件通信

### 通过Props给子组件传值

- Props是单向的，不要在子组件中修改props数据
- 子组件中num值得改变应该由父组件来影响

1. 在App.vue中传值

```vue
<InputNumber :num="num" @num-change="handleNumChange"></InputNumber>
//通过自定义函数触发
<script>
export default {
  name: 'App',
  data(){
    return{
      num:5
    }
  },
  methods:{
    handleNumChange(num){
      this.num = num
    }
  },
  components: {
    InputNumber
  }
}
</script>
```

2. 在组件中拿数据并使用

```vue
<template>
	<div>
		<button @click="handleIncrement(-1)">-</button>
		<input type="text" :value="num">
		<button @click="handleIncrement(1)">+</button>
	</div>
</template>
<script>
	export default{
		name: 'InputNumber',
		props:['num'],
		data(){
			return {

			}
		},
		methods:{
			handleIncrement(count){
                 //自定义函数触发，并向父组件传值
				this.$emit('num-change',this.num + count)
			}
		}
	}
</script>
```

### 简化版

- v-model 一般用于表单元素
- 当 v-model 用于组件的时候，实际上相当于
  - v-bind:value = "num"  默认传递一个名字叫value的props数据
  - v-on:input = "e => num = e.target.value " 默认监听组件内部的input事件，修改绑定的数据

1. App.vue

```vue
<template>
  <div id="app">
    <InputNumber v-model="num"></InputNumber>
  </div>
</template>

<script>
import InputNumber from './components/InputNumber'

export default {
  name: 'App',
  data(){
    return{
      num:5
    }
  },
  components: {
    InputNumber
  }
}
</script>

```

2. InputNumber.vue

```vue
<template>
	<div>
		<button @click="handleIncrement(-1)">-</button>
		<input type="text" :value="value" @input="handleInput">
		<button @click="handleIncrement(1)">+</button>
	</div>
</template>
<script>
	export default{
		name: 'InputNumber',
		props:['value'],
		data(){
			return {

			}
		},
		methods:{
			handleIncrement(count){
				this.$emit('input',this.value + count)
			},
			handleInput(e){
				this.$emit('input',e.target.value - 0)
			}
		}
	}
</script>
```

# 组件的插槽

- slot就是插槽，组件默认会把使用组件中间内容渲染到内部
- 起名字会对应渲染到名字插槽
- 没有就渲染到没有名字的插槽

```vue
<Card>
    <h2 slot="header">头部</h2>
    <p>卡片内容</p>
    <p>卡片内容</p>
    <p>卡片内容</p>
</Card>
<template>
	<div class="Card">
        <div class="Card-header">
            <slot name="header"></slot>
    	</div>
        <div class="Card-box">
            <slot></slot>
    	</div>
    </div>
</template>
```

# 动态组件与keep-alive

```vue
<botton @click="currentTabComponent = foo"></botton>
<botton @click="currentTabComponent = bar"></botton>
<!-- 失活的组件将会被缓存！数据不会销毁比如组件内部有输入框切换后还存在-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
<script>
	import foo from './components/foo'
	import bar from './components/bar'
    data(){
        currentTabComponent = foo
    }
</script>
```

# 综合案例（TabBar）

- 组件注册
- 插槽的应用
- v-model的应用
- 父子组件传值

1. main.js（注册组件）

```javascript
import heads from './components/head'
Vue.component('heads',heads)
import tabBar from './components/tabBar'
Vue.component('tabBar',tabBar)
```

2. app.vue（使用组件）

```vue
<tabBar v-model="activeName">
    <heads label="首页" name="index">itema----首页</heads>
    <heads label="搜索" name="soure">itema----搜索</heads>
    <heads label="购物车" name="gcay">itema----购物车</heads>
    <heads label="我的" name="myd">itema----我的</heads>
</tabBar>
data(){
    return{
    	activeName:'index'
    }
}
```

3. tabBox.vue（内容盒子）

```vue
<template>
	<div v-show="name === $parent.activeName">
		<slot></slot>
	</div>
</template>
<script>
	export default{
		props:['label','name'],
		data(){
			return{}
		},
		mounted(){
			//console.log(this.$parent)
		}
	}
</script>
```

4. tabBar.vue（选项卡头部）

```vue
<template>
	<div class="tab-wrapper">
		<div class="tabs-heading">
			<span v-for="item in tabPanes" 
                   :class="{active:item.name === activeName}" 
                   @click="fnClick(item)">{{item.label}}
             </span>
		</div>
		<div class="tabs-body">
			<slot></slot>
		</div>
	</div>
</template>
<script>
	export default{
		props:['value'],
		data(){
			return{
				tabPanes:[]
			}
		},
		mounted(){
			//获取子组件
			this.tabPanes = this.$children
		},
		computed:{
			activeName(){
				return this.value
			}
		},
		methods:{
			fnClick(tab){
                 //向父组件传递 v-model默认绑定input事件
				this.$emit('input',tab.name)
			}
		}
	}
</script>
<style>
	.tab-wrapper{width: 400px;border: 1px solid #000;border-right: 0;}
	.tab-wrapper .tabs-heading{height: 50px;border-bottom: 1px solid #000;display:flex;}
	.tab-wrapper .tabs-heading span{width:100px;line-height: 50px;text-align: center;border- 	  right: 1px solid #000;}
	.tab-wrapper .tabs-heading span.active{color:red}
	.tab-wrapper .tabs-body{border-right: 1px solid #000;padding:10px;}
</style>
```

# VUEX

1. 安装

```shell
npm i vuex -S
```

2. 配置

```javascript
import Vuex from "vuex"
Vue.use(Vuex)

var store = new Vuex.Store({
    state:{
        //数据存储
        totalcount:1
    },
    mutations:{
        //函数使用来修改仓库中的数据
        updatecount(state,arg){
            state.totalcount = arg
        }
    }
})
//在Vue实例中加载
new Vue({
    //可简写store:store
    store
})
```

3. 使用

```html
{{this.$store.state.totalcount}}
```

4. 调用方法

```javascript
this.$store.commit("updatecount",参数)
```



