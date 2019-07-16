# React.js

> ReactJS是由Facebook在2013年5月推出的一款JS前端**开源**框架,推出式主打特点式**函数式编程**风格。

`React` 三大体系

* `React.js` 用于web开发和组件的编写

* `ReactNative` 用于移动端开发
* `ReactVR` 用于虚拟实现技术的开发

## 脚手架的安装

> `create-react-app `是 React 官方出的脚手架工具

* `npm install -g create-react-app`

```javascript
1. create-react-app react-demo   //用脚手架创建React项目
2. cd react-dom   //等创建完成后，进入项目目录
3. npm start   //预览项目，如果能正常打开，说明项目创建成功
```

## 项目根目录

* **README.md** 

  > 这个文件主要作用就是对项目的说明

* **package.json**

  > webpack配置和项目包管理文件，项目中依赖的第三方包（包的版本）和一些常用命令配置都在这个里边进行配置

* **package-lock.json**

  > 锁定安装时的版本号，并且需要上传到git，以保证其他人再`npm install` 时大家的依赖能保证一致

* **gitignore**

  > 这个是git的选择性上传的配置文件

* **node_modules**

  > 项目的依赖包

* **public**

  > 公共文件，里边有公用模板和图标等一些东西

* **src**

  > 主要代码编写文件

### public文件夹介绍

> 这个文件都是一些项目使用的公共文件，也就是说都是共用的，我们就具体看一下有那些文件吧。

- **favicon.ico** : 这个是网站或者说项目的图标，一般在浏览器标签页的左上角显示。
- **index.html** : 首页的模板文件，我们可以试着改动一下，就能看到结果。
- **mainifest.json**：移动端配置文件，这个会在以后的课程中详细讲解。

### src文件夹介绍

这个目录里边放的是我们开放的源代码，我们平时操作做最多的目录。

- **index.js** : 这个就是项目的入口文件，视频中我们会简单的看一下这个文件。
- **index.css** ：这个是`index.js`里的CSS文件。
- **app.js** : 这个文件相当于一个方法模块，也是一个简单的模块化编程。
- **serviceWorker.js**: 这个是用于写移动端开发的，PWA必须用到这个文件，有了这个文件，就相当于有了离线浏览的功能。

## React生命周期图

![React声明周期图](https://jspang.com/images/React1901.png)

通过这张图你可以看到React生命周期的四个大阶段：

1. `Initialization`:初始化阶段。
2. `Mounting`: 挂在阶段。
3. `Updation`: 更新阶段。
4. `Unmounting`: 销毁阶段

## 什么是生命周期函数

> 生命周期函数指在某一个时刻组件会自动调用执行的函数

举例：写的小姐姐的例子。里边的`render()`函数，就是一个生命周期函数，它在state发生改变时自动执行。这就是一个标准的自动执行函数。

- `constructor`不算生命周期函数。

`constructor`我们叫构造函数，它是ES6的基本语法。虽然它和生命周期函数的性质一样，但不能认为是生命周期函数。

但是你要心里把它当成一个生命周期函数，我个人把它看成React的`Initialization`阶段，定义属性（props）和状态(state)。

## Mounting阶段

Mounting阶段叫挂载阶段，伴随着整个虚拟DOM的生成，它里边有三个小的生命周期函数，分别是：

1. `componentWillMount` : 在组件即将被挂载到页面的时刻执行。
2. `render` : 页面state或props发生变化时执行。
3. `componentDidMount` : 组件挂载完成时被执行。

## **注意的问题**

`componentWillMount`和`componentDidMount`这两个生命周期函数，只在页面刷新时执行一次，而`render`函数是只要有state和props变化就会执行，这个初学者一定要注意。

## `Updation`阶段

`shouldComponentUpdate`函数会在组件更新之前，自动被执行。

`componentWillUpdate`在组件更新之前，但`shouldComponenUpdate`之后被执行。但是如果`shouldComponentUpdate`返回false，这个函数就不会被执行了。

`componentDidUpdate`在组件更新之后执行，它是组件更新的最后一个环节。

`componentWillReceiveProps`子组件接收到父组件传递过来的参数，父组件render函数重新被执行，这个生命周期就会被执行。

- 也就是说这个组件第一次存在于Dom中，函数是不会被执行的;
- 如果已经存在于Dom中，函数才会被执行。

`componentWillUnmount`这个函数时组件从页面中删除的时候执行

## axios请求数据

强烈建议在`componentDidMount`函数里作`ajax`请求。

## react-transition-group动画组件

# Redux

> `antd` 是基于 Ant Design 设计体系的 React UI 组件库，主要用于研发企业级中后台产品。

```sh
npm install --save redux
```

安装好`redux`之后，在`src`目录下创建一个`store`文件夹,然后在文件夹下创建一个`index.js`文件。

`index.js`就是整个项目的`store`文件，打开文件，编写下面的代码。

```js
import { createStore } from 'redux'  // 引入createStore方法
const store = createStore()          // 创建数据存储仓库
export default store                 //暴露出去
```

这样虽然已经建立好了仓库，但是这个仓库很混乱，这时候就需要一个有管理能力的模块出现，这就是`Reducers`。这两个一定要一起创建出来，这样仓库才不会出现互怼现象。在`store`文件夹下，新建一个文件`reducer.js`,然后写入下面的代码。

```js
const defaultState = {}  //默认数据
export default (state = defaultState,action)=>{  //就是一个方法函数
    return state
}
```