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
1. mkdir ReactDemo  //创建ReactDemo文件夹
2. create-react-app demo   //用脚手架创建React项目
3. cd demo   //等创建完成后，进入项目目录
4. npm start   //预览项目，如果能正常打开，说明项目创建成功
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

## HelloWorld和组件

在`src目录`下，新建一个文件`index.js`

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
ReactDOM.render(<App />,document.getElementById('root'))
```

在`src目录`下，新建一个文件`App.js`

```javascript
import React, {Component} from 'react'
class App extends Component{
    render(){
        return (
            <div>
                Hello JSPang
            </div>
        )
    }
}
export default App;
```

## React中JSX语法

> JSX就是Javascript和XML结合的一种格式。React发明了JSX，可以方便的利用HTML语法来创建虚拟DOM，当遇到`<`，JSX就当作HTML解析，遇到`{`就当JavaScript解析.