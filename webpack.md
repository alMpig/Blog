

# webpack

[![img](https://img.shields.io/npm/v/webpack.svg?label=webpack&style=flat-square&maxAge=3600)](https://github.com/webpack/webpack/releases)

> npm install moduleName

```powershell
npm install moduleName # 安装模块到项目目录下
npm install -g moduleName # -g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。
npm install --save moduleName # --save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。
npm install --save-dev moduleName # --save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。
//-S就是--save      //开发
//-D就是--save-dev  //测试
//-O就是--save-optional
//-E就是--save-exact
//-B就是--save-bundle
```

## 安装

```shell
//全局安装
npm install --global webpack
//如果你使用 webpack 4+ 版本，你还需要安装 CLI。
npm install --save-dev webpack-cli
```

## [起步](https://www.webpackjs.com/guides/getting-started/)

```shell
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev
```

### **project**

```diff
  webpack-demo
  |- package.json
+ |- index.html
+ |- /src
+   |- index.js
```

### **src/index.js**

```javascript
function component() {
  var element = document.createElement('div');

  // Lodash（目前通过一个 script 脚本引入）对于执行这一行是必需的
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

### **index.html**

```html
<!doctype html>
<html>
  <head>
    <title>起步</title>
    <script src="https://unpkg.com/lodash@4.16.6"></script>
  </head>
  <body>
    <script src="./src/index.js"></script>
  </body>
</html>
```

### **package.json**

```diff
+   "private": true,
-   "main": "index.js",
```

### 创建一个 bundle 文件

#### **project**

```diff
+ |- /dist
+   |- index.html
- |- index.html
```

要在 `index.js` 中打包 `lodash` 依赖，我们需要在本地安装 library：

```bash
npm install --save lodash
```

#### **src/index.js**

```diff
+ import _ from 'lodash';
-   // Lodash, currently included via a script, is required for this line to work
+   // Lodash, now imported by this script
```

#### **dist/index.html**

```diff
-    <script src="https://unpkg.com/lodash@4.16.6"></script>
-    <script src="./src/index.js"></script>
+    <script src="main.js"></script>
```

#### 执行

```shell
npx webpack
```

在浏览器中打开 `index.html`，如果一切访问都正常，你应该能看到以下文本：'Hello webpack'。

### 模块

 [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) 和 [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

#### 使用一个配置文件

#### **project**

```diff
+ |- webpack.config.js
```

#### **webpack.config.js**

```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```

#### **dist/index.html**

```diff
-    <script src="main.js"></script>
+    <script src="bundle.js"></script>
```

```shell
//执行
npx webpack --config webpack.config.js
```

### NPM 脚本(NPM Scripts)

考虑到用 CLI 这种方式来运行本地的 webpack 不是特别方便，我们可以设置一个快捷方式。在 *package.json* 添加一个 [npm 脚本(npm script)](https://docs.npmjs.com/misc/scripts)：

#### **package.json**

```diff
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
+     "build": "webpack"
    },
```

现在，可以使用 `npm run build` 命令，来替代我们之前使用的 `npx` 命令。注意，使用 npm 的 `scripts`，我们可以像使用 `npx` 那样通过模块名引用本地安装的 npm 包。这是大多数基于 npm 的项目遵循的标准，因为它允许所有贡献者使用同一组通用脚本（如果必要，每个 flag 都带有 `--config` 标志）。

## 管理资源

### 加载 CSS

```bash
npm install --save-dev style-loader css-loader
```

为了从 JavaScript 模块中 `import` 一个 CSS 文件，你需要在 [`module` 配置中](https://www.webpackjs.com/configuration/module) 安装并添加 [style-loader](https://www.webpackjs.com/loaders/style-loader) 和 [css-loader](https://www.webpackjs.com/loaders/css-loader)：

#### **webpack.config.js**

```diff
 module.exports = {
  	output: {
    },
+   module: {
+     rules: [
+       {
+         test: /\.css$/,
+         use: [
+           'style-loader',
+           'css-loader'
+         ]
+       }
+     ]
+   }
};
```

#### **project**

```diff
  |- /src
+   |- style.css
```

#### **src/style.css**

```css
.hello {
  color: red;
}
```

#### **src/index.js**

```diff
+ import './style.css';
+   element.classList.add('hello');
```

```bash
npm run build
```

### 加载图片

```bash
npm install --save-dev file-loader
```

#### **webpack.config.js**

```diff
    module: {
      rules: [
+       {
+         test: /\.(png|svg|jpg|gif)$/,
+         use: [
+           'file-loader'
+         ]
+       }
 	]
}    
```

#### **project**

```diff
  |- /src
+   |- icon.png
```

#### **src/index.js**

```diff
+ import Icon from './icon.png';
+   // 将图像添加到我们现有的 div。
+   var myIcon = new Image();
+   myIcon.src = Icon;
+
+   element.appendChild(myIcon);
```

#### **src/style.css**

```diff
  .hello {
    color: red;
+   background: url('./icon.png');
  }
```

```bash
npm run build
```

### 加载字体

#### **webpack.config.js**

```diff
    module: {
      rules: [
+       {
+         test: /\.(woff|woff2|eot|ttf|otf)$/,
+         use: [
+           'file-loader'
+         ]
+       }
 	]
} 
```

#### **project**

```diff
  |- /src
+   |- my-font.woff
+   |- my-font.woff2
```

#### **src/style.css**

[字体格式](<http://www.mamicode.com/info-detail-627082.html>)|[字体下载](<http://font.chinaz.com/tag_font/WangYe.html>)

```diff
+ @font-face {
+   font-family: 'MyFont';
+   src:  url('Lazara.ttf') format('truetype');
+   font-weight: 600;
+   font-style: normal;
+ }

  .hello {
    color: red;
+   font-family: 'MyFont';
    background: url('./icon.png');
  }
```

```bash
npm run build
```

### [加载数据]([https://www.webpackjs.com/guides/asset-management/#%E5%8A%A0%E8%BD%BD%E6%95%B0%E6%8D%AE](https://www.webpackjs.com/guides/asset-management/#加载数据))

## 管理输出

#### **project**

```diff
  |- /src
    |- index.js
+   |- print.js
```

#### **src/print.js**

```js
export default function printMe() {
  console.log('I get called from print.js!');
}
```

#### **src/index.js**

```diff
import _ from 'lodash';
import printMe from './print.js';
function component() {
    var element = document.createElement('div');
    var btn = document.createElement('button');
    //// Lodash, now imported by this script
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
     
    btn.innerHTML = 'Click me and check the console!';
    btn.onclick = printMe;
    
    element.appendChild(btn);
    return element;
  }
  
  document.body.appendChild(component());
```

#### **dist/index.html**

```diff
  <!doctype html>
  <html>
    <head>
     <title>Output Management</title>
     <script src="./print.bundle.js"></script>
    </head>
    <body>
     <script src="./app.bundle.js"></script>
    </body>
  </html>
```

#### **webpack.config.js**

```diff
  const path = require('path');

  module.exports = {
   entry: {
     app: './src/index.js',
     print: './src/print.js'
   },
    output: {
     filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```

### 设定 HtmlWebpackPlugin

```bash
npm install --save-dev html-webpack-plugin
```

#### **webpack.config.js**

```diff
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    entry: {
      app: './src/index.js',
      print: './src/print.js'
    },
   plugins: [
     new HtmlWebpackPlugin({
       title: 'Output Management'
     })
   ],
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```

### 清理 `/dist` 文件夹

```bash
npm install clean-webpack-plugin --save-dev
```

#### **webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
module.exports = {
  entry: {
    app: './src/index.js',
    print: './src/print.js'
  },
 plugins: [
   new CleanWebpackPlugin(),
   new HtmlWebpackPlugin({
     title: 'Output Management'
   })
 ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```

## 开发

### 使用 source map

为了更容易地追踪错误和警告，JavaScript 提供了 [source map](http://blog.teamtreehouse.com/introduction-source-maps) 功能，将编译后的代码映射回原始源代码。如果一个错误来自于 `b.js`，source map 就会明确的告诉你。

> 仅解释说明，不要用于生产环境

#### **webpack.config.js**

```diff
  module.exports = {
  	entry: {},
+   devtool: 'inline-source-map',
}
```

#### **src/print.js**

```diff
  export default function printMe() {
-   console.log('I get called from print.js!');
+   cosnole.error('I get called from print.js!');
  }
```

### [选择一个开发工具]([https://www.webpackjs.com/guides/development/#%E9%80%89%E6%8B%A9%E4%B8%80%E4%B8%AA%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7](https://www.webpackjs.com/guides/development/#选择一个开发工具))

### 使用观察模式

你可以指示 webpack "watch" 依赖图中的所有文件以进行更改。如果其中一个文件被更新，代码将被重新编译，所以你不必手动运行整个构建。

#### **package.json**

```diff
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
+     "watch": "webpack --watch",
      "build": "webpack"
    },
```

```shell
npm run watch
```

现在,保存文件并检查终端窗口。应该可以看到 webpack 自动重新编译修改后的模块！

唯一的缺点是，为了看到修改后的实际效果，你需要刷新浏览器。如果能够自动刷新浏览器就更好了，可以尝试使用 `webpack-dev-server`，恰好可以实现我们想要的功能。

### 使用 webpack-dev-server

```bash
npm install --save-dev webpack-dev-server
```

#### **webpack.config.js**

```diff
  module.exports = {
  	entry: {},
  	devtool:inline-source-map,
+   devServer: {
+     contentBase: './dist'
+   },
```

#### **package.json**

```diff
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "watch": "webpack --watch",
+     "start": "webpack-dev-server --open",
      "build": "webpack"
    },
```

```sh
npm start
//如果现在修改和保存任意源文件，web 服务器就会自动重新加载编译后的代码
```

### [使用 webpack-dev-middleware]([https://www.webpackjs.com/guides/development/#%E4%BD%BF%E7%94%A8-webpack-dev-middleware](https://www.webpackjs.com/guides/development/#使用-webpack-dev-middleware))

```bash
npm install --save-dev express webpack-dev-middleware
```

#### **webpack.config.js**

```diff
   module.exports = {
 	output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist'),
+     publicPath: '/'
```

#### **project**

```diff
  webpack-demo
+ |- server.js
```

#### server.js

```js
const express = require('express');
const webpack = require('webpack');
const webpackDevMiddleware = require('webpack-dev-middleware');

const app = express();
const config = require('./webpack.config.js');
const compiler = webpack(config);

// Tell express to use the webpack-dev-middleware and use the webpack.config.js
// configuration file as a base.
app.use(webpackDevMiddleware(compiler, {
  publicPath: config.output.publicPath
}));

// Serve the files on port 3000.
app.listen(3000, function () {
  console.log('Example app listening on port 3000!\n');
});
```

#### **package.json**

```diff
    "scripts": {
+     "server": "node server.js",
```

```shell
npm run server
```

## 模块热替换

### 启用 HMR

#### **webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const webpack = require('webpack');
module.exports = {
  entry: {
    app: './src/index.js'
  },
  devtool: 'inline-source-map',
  devServer: {
         contentBase: './dist',
         hot: true
    },
 plugins: [
   new CleanWebpackPlugin(),
   new HtmlWebpackPlugin({
     title: 'Output Management'
   }),
   new webpack.NamedModulesPlugin(),
   new webpack.HotModuleReplacementPlugin()
 ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
    publicPath: '/'
  }
};
```

```shell
npm start
```

#### **index.js**

```diff
document.body.appendChild(component());
+ if (module.hot) {
+   module.hot.accept('./print.js', function() {
+     console.log('Accepting the updated printMe module!');
+     printMe();
+   })
+ }
```

#### **print.js**

```diff
  export default function printMe() {
-   console.log('I get called from print.js!');
+   console.log('Updating print.js...')
  }
```